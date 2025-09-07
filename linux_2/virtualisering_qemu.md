# Virtualisering - QEMU-KVM

## Vad är QEMU-KVM?

- **QEMU**: emulator som kan köra många arkitekturer.
- **KVM**: kärnmodul som använder CPU-stöd (VT-x/AMD-V) för snabbare virtualisering.

  - **Kan användas**: när gästsystemet är samma arkitektur som värden (t.ex. x86 på x86).
  - **Kan inte användas**: när gästsystemet är en annan arkitektur än värdens. Då används QEMUs TCG-emulering, som fungerar men är mycket långsammare än med KVM-acceleration.
- **Tillsammans**: effektiv och snabb virtualisering i Linux.

## Diskfiler

- `qcow2` är det rekommenderade formatet och har stöd för:

  - **Snapshots**: spara och återställ VM-tillstånd vid olika tidpunkter.
  - **Komprimering**: minska storleken på disken genom att lagra data effektivare.
  - **Thin provisioning**: allokerar endast faktiskt använd utrymme, inte hela den maximala storleken direkt.
- **Nackdelar**: qcow2 är långsammare än råa diskfiler (`raw`), särskilt om många snapshots eller backing files används. För högsta prestanda eller vissa avancerade funktioner (t.ex. direkt blockdevice) används ofta `raw`.

Skapa en ny disk:

```bash
qemu-img create -f qcow2 ubuntu.qcow2 70G
```

Inspektera en disk:

```bash
qemu-img info ubuntu.qcow2
```

- **När du vill kontrollera storlek**: se om den faktiska filstorleken skiljer sig från den virtuella maxstorleken.
- **När du felsöker**: kontrollera format, backing file och snapshots.

  - **Backing file** betyder att disken bygger på en annan fil (basimage). Endast ändringar sparas i den nya filen. Detta sparar utrymme men kan ge beroenden och sämre prestanda.

Konvertera en disk från vdi (VirtualBox) till qcow2 (snapshots följer inte med):

```bash
qemu-img convert -f vdi -O qcow2 arch.vdi arch.qcow2
```

## Starta en första VM och anslut med VNC

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 5000 \
  -smp 2 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -cdrom ubuntu-24.04.3-live-server-amd64.iso \
  -boot d \
  -vnc :0
```

- **-m**: minne i MB.
- **-smp**: antal CPU-kärnor.
- **-cdrom**: ISO-fil för installation.
- **-boot d**: starta från CD.
- **-vnc :0**: öppna VNC-server på display :0 (port 5900).

**Fördelar/nackdelar**:

- **VNC**: fungerar på alla plattformar, men har begränsad funktionalitet (inget ljud, inget urklipp).
- Statisk konfiguration – argument som `-cdrom`, `-boot` eller nätverk kan inte ändras under körning.

## Använda SPICE istället för VNC

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 5000 \
  -smp 2 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -cdrom ubuntu-24.04.3-live-server-amd64.iso \
  -boot d \
  -spice port=44400,disable-ticketing=on
```

- **SPICE**: ger bättre grafik, stöd för ljud, delat urklipp och filöverföring. Kräver ofta virtio-gpu i gästen för bästa prestanda. På Windows är drivrutinerna mindre mogna.
- **disable-ticketing=on**: ingen autentisering (osäkert utanför testmiljö).

## CPU-konfiguration

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -cpu host \
  -m 5000 \
  -smp 2 \
  -drive file=ubuntu.qcow2,format=qcow2
```

- **-cpu host**: gästen får samma CPU-funktioner som värden. Hög prestanda, men gör live-migrering svårare mellan värdar med olika CPU.
- **-cpu qemu64**: generisk CPU-modell med hög kompatibilitet men utan alla optimeringar.

## Maskintyp

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -machine q35 \
  -m 5000 \
  -drive file=ubuntu.qcow2,format=qcow2
```

- **-machine q35**: modern maskintyp med PCIe-stöd. Mindre kompatibel med äldre OS.
- **-machine pc**: äldre standard, används för enkelhet eller äldre OS.

## SMP (flera processorkärnor)

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -smp 4,sockets=1,cores=4,threads=1 \
  -m 4096 \
  -drive file=ubuntu.qcow2,format=qcow2
```

- **-smp 4**: totalt 4 vCPUs.
- **sockets/cores/threads**: påverkar hur gästen ser CPU-topologin.

## Virtio och ballooning

- **Virtio**: snabbare drivrutiner än emulerad hårdvara. Kräver drivrutiner i gästen (Linux har inbyggt, Windows måste installera manuellt – annars kan gästen bli obootbar).
- **Ballooning**: dynamisk minneshantering. Värden kan ta tillbaka RAM vid behov, men gästen kan börja swappa om minnet blir för litet.

## Port forward från värden till gästen

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 5000 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -net user,hostfwd=tcp::2222-:22 \
  -net nic
```

- **-net user**: enkel NAT via värden. Kräver inga root-rättigheter.
- **hostfwd**: vidarebefordrar portar.
- **Nackdelar**: gäster i `-net user` kan normalt inte prata direkt med varandra och prestandan är lägre än TAP/bridge eller passt.

```plaintext
Internet
   |
[ värd ]  <—— ssh tcp/2222 ——  (NAT & portfwd)
   |                                   \
 [ VM ] 22/tcp  <——————— forwarded ————/
```

```plaintext
[Host:2222] ---> [VM:22]
```

## Tap och bridge

Exempel: skapa TAP och bridge och starta en VM:

```bash
sudo ip tuntap add dev tap0 mode tap user "$USER"
sudo ip link set tap0 up
sudo ip link add br0 type bridge
sudo ip link set eth0 master br0
sudo ip link set tap0 master br0
sudo ip link set br0 up
sudo dhclient br0

qemu-system-x86_64 \
  -enable-kvm \
  -m 5000 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -netdev tap,id=mynet0,ifname=tap0,script=no,downscript=no \
  -device e1000,netdev=mynet0
```

```plaintext
Internet/LAN
     |
   [Switch/Router]
        |
      [br0] (mjukvarubrygga på värddatorn)
       /   \
    [tap0] [eth0]
       |
     [ VM ]  (får IP från LAN/DHCP)
```

- **Bridge**: gör att gästerna blir fullvärdiga maskiner på samma LAN.
- **Fördelar**: hög prestanda, VMs kan prata direkt med varandra och med andra datorer på nätet.
- **Nackdelar**: kräver root-konfiguration, och fungerar ofta dåligt med Wi-Fi eftersom trådlösa kort inte stöder vanlig Ethernet-bryggning.

## Isolerat nätverk (likt Docker)

Exempel med två gäster:

```bash
sudo ip link add ustwo_bridge type bridge
sudo ip link set ustwo_bridge up

sudo ip tuntap add dev tap0 mode tap user "$USER"
sudo ip link set tap0 up
sudo ip link set tap0 master ustwo_bridge

sudo ip tuntap add dev tap1 mode tap user "$USER"
sudo ip link set tap1 up
sudo ip link set tap1 master ustwo_bridge

qemu-system-x86_64 \
  -enable-kvm \
  -m 2000 \
  -smp 2 \
  -drive file=ubuntu1.qcow2,format=qcow2 \
  -netdev tap,id=mynet0,ifname=tap0,script=no,downscript=no \
  -device virtio-net-pci,netdev=mynet0,mac=52:54:00:12:34:56 \
  -daemonize

qemu-system-x86_64 \
  -enable-kvm \
  -m 2000 \
  -smp 2 \
  -drive file=ubuntu2.qcow2,format=qcow2 \
  -vnc :1 \
  -spice port=44401,disable-ticketing=on \
  -netdev tap,id=mynet1,ifname=tap1,script=no,downscript=no \
  -device virtio-net-pci,netdev=mynet1,mac=52:54:00:12:34:57 \
  -daemonize
```

```plaintext
           [värddator]
               |
        [ustwo_bridge]   (ingen route/NAT mot LAN)
           /       \
        [VM1]     [VM2]
```

- **Fördelar**: helt isolerat, bra för labbar.
- **Nackdelar**: ingen Internet/DNS utan extra routing eller NAT.

## Snapshots

Skapa snapshot medan VM är igång (inkluderar RAM och runtime-tillstånd = online snapshot):

```bash
(qemu) savevm snap1
```

Skapa snapshot när VM är avstängd (endast diskens tillstånd = offline snapshot):

```bash
qemu-img snapshot -c snap1 ubuntu.qcow2
```

- **Online snapshot**: inkluderar RAM och tillstånd.
- **Offline snapshot**: endast disk.
- **Nackdel**: långa snapshot-kedjor i qcow2 kan fragmentera och göra disken långsammare.

```plaintext
root.qcow2
   ├── snap1
   │    └── snap2
   └── snap3
```

## UEFI och NVRAM

Exempelstart med UEFI (OVMF):

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 3000 \
  -drive if=pflash,format=raw,readonly=on,file=/usr/share/OVMF/OVMF_CODE_4M.fd \
  -drive if=pflash,format=raw,file=Rocky10_UEFI_OVMF_VARS.fd \
  -drive file="Rocky10_UEFI.qcow2",format=qcow2 \
  -cdrom ~/Downloads/Rocky-10.0-x86_64-minimal.iso \
  -boot d
```

- **UEFI**: kräver OVMF.
- **NVRAM-fil**: lagrar inställningar (boot order etc).
- **Nackdel**: om du glömmer att kopiera NVRAM-filen vid snapshot/backup kan gästen “tappa” sina UEFI-inställningar.

```plaintext
[ VM ]
   |-- pflash (OVMF_CODE_4M.fd, readonly)
   |-- pflash (OVMF_VARS.fd, skrivbar NVRAM)
```

## TPM

Exempel med swtpm:

```bash
mkdir ubuntutpm
swtpm socket --tpm2 --tpmstate dir=./ubuntutpm --ctrl type=unixio,path=./ubuntutpm/swtpm-sock &

qemu-system-x86_64 \
  -enable-kvm \
  -m 2048 \
  -drive file=ubuntu1.qcow2,format=qcow2 \
  -chardev socket,id=chrtpm,path=./ubuntutpm/swtpm-sock \
  -tpmdev emulator,id=tpm0,chardev=chrtpm \
  -device tpm-tis,tpmdev=tpm0
```

- **Emuleras via swtpm**.
- **Notera**: TPM-state lagras i en separat katalog. Om den inte inkluderas i snapshot/backup så matchar inte tillståndet.

```plaintext
[ VM ]
   |-- virtuell TPM-enhet
           |
        [./ubuntutpm] (state-katalog)
```

## UEFI + TPM för Windows 11

Exempelstart:

```bash
mkdir win11tpm
swtpm socket --tpm2 --tpmstate dir=./win11tpm --ctrl type=unixio,path=./win11tpm/swtpm-sock &

qemu-system-x86_64 \
  -enable-kvm \
  -m 6000 \
  -cpu host \
  -drive if=pflash,format=raw,readonly=on,file=/usr/share/OVMF/OVMF_CODE_4M.ms.fd \
  -drive if=pflash,format=raw,file=Win11_OVMF_VARS_4M.ms.fd \
  -drive file=Win11.qcow2,format=qcow2 \
  -cdrom Windows11_Ent_Eval.iso \
  -chardev socket,id=chrtpm,path=./win11tpm/swtpm-sock \
  -tpmdev emulator,id=tpm0,chardev=chrtpm \
  -device tpm-tis,tpmdev=tpm0 \
  -spice port=44400,disable-ticketing=on
```

- Kräver UEFI + TPM + Secure Boot.
- **Nackdelar**: installationen är känslig för avsaknad av virtio-drivrutiner (disk/nät), och migrering mellan värdar kan vara svårare.

```plaintext
[ VM ]
   |-- pflash (OVMF_CODE_4M.ms.fd, readonly, med MS keys för Secure Boot)
   |-- pflash (Win11_OVMF_VARS_4M.ms.fd, NVRAM med bootinställningar)
   |-- virtuell TPM-enhet
            |
         [./win11tpm] (state-katalog)
```
