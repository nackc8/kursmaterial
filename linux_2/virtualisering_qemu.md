# Virtualisering - QEMU-KVM

## Vad är QEMU-KVM?

- **QEMU**: emulator som kan köra många arkitekturer.
- **KVM**: kärnmodul som använder CPU-stöd (VT-x/AMD-V) för snabbare virtualisering.
  - **Kan användas**: när gästsystemet är samma arkitektur som värden (t.ex. x86 på x86).
  - **Kan inte användas**: när gästsystemet är en annan arkitektur än värden. Då används bara QEMU-emulering, utan KVM-acceleration.
- **Tillsammans**: effektiv och snabb virtualisering i Linux.

## Diskfiler

- `qcow2` är det rekommenderade formatet och har stöd för:
  - **Snapshots**: spara och återställ VM-tillstånd vid olika tidpunkter.
  - **Komprimering**: minska storleken på disken genom att lagra data effektivare.
  - **Thin provisioning**: allokerar endast faktiskt använd utrymme, inte hela den maximala storleken direkt.
- Skapa en ny disk:

    ```bash
    qemu-img create -f qcow2 ubuntu.qcow2 70G
    ```

- Inspektera en disk:
  - **När du vill kontrollera storlek**: se om den faktiska filstorleken skiljer sig från den virtuella maxstorleken.
  - **När du felsöker**: kontrollera följande:
    - **Format**: vilken typ av diskfil (t.ex. qcow2 eller raw).
    - **Backing file**: om disken bygger på en annan fil (basimage).
      - Det betyder att diskfilen inte innehåller all data själv, utan hänvisar till en annan fil som bas.
      - Endast ändringar sparas i den nya filen.
      - Detta sparar utrymme och gör det enkelt att skapa kloner av en grundinstallation.
    - **Snapshots**: om disken har sparade tillstånd som kan återställas.

    ```bash
    qemu-img info ubuntu.qcow2
    ```

- Konvertera en disk från vdi (VirtualBox) till qcow2 (originalfilen förblir intakt):

    ```bash
    qemu-img convert -f vdi -O qcow2 arch.vdi arch.qcow2
    ```

  - **Obs**: konverteringen tar med diskens data men **snapshots följer inte med** vid konvertering.

## Starta en första VM och anslut med VNC

Exempel:

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 2048 \
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
- **-vnc :0**: öppna VNC-server på display :0 (port 5900+0).
- **Installera en VNC-klient**:
  - Linux: t.ex. `sudo apt install tigervnc-viewer`
  - Windows: tips – **RealVNC Viewer** eller **TightVNC** fungerar bra.
  - macOS: tips – **TigerVNC** eller **RealVNC Viewer** fungerar bra.

**Notera**: I denna form är konfigurationen statisk. Du kan inte ändra argument som `-cdrom`, `-boot`, eller nätverksinställningar (t.ex. port forwarding) medan VM körs. För att ändra dessa måste du stoppa VM och starta om den med nya parametrar.

## Använda SPICE istället för VNC

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 2048 \
  -smp 2 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -cdrom ubuntu-24.04.3-live-server-amd64.iso \
  -boot d \
  -spice port=5901,disable-ticketing=on
```

- **Anslut med**: `remote-viewer spice://127.0.0.1:5901`
- **disable-ticketing=on**: betyder att ingen autentisering krävs för SPICE-anslutningen (endast för test, inte säkert i produktion).
- **Installera en SPICE-klient**:
  - Linux: `sudo apt install virt-viewer`
  - Windows: ladda ner **Virt Viewer (Remote Viewer)** från [virt-manager.org](https://virt-manager.org/)
  - macOS: installera **Remote Viewer** via Homebrew (`brew install virt-viewer`) eller ladda ner källkod/binärer från samma sida.
- **Varför SPICE?**: ger bättre grafik, stöd för ljud, filöverföring och delat urklipp mellan värd och gäst.
- **Fördel med VNC**: det finns många klienter för alla plattformar.

## CPU-konfiguration

Du kan ange vilken CPU-modell eller funktioner som exponeras för gästen.

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -cpu host \
  -m 2048 \
  -smp 2 \
  -drive file=ubuntu.qcow2,format=qcow2
```

- **-cpu host**: gästen får samma CPU-funktioner som värden. Bra när prestanda och tillgång till hårdvarufunktioner är viktigast.
- **-cpu qemu64**: generisk CPU-modell med hög kompatibilitet men utan alla optimeringar. Används om du behöver maximal portabilitet eller för att undvika problem vid migrering mellan olika värdar.

## Maskintyp

QEMU kan emulera olika maskintyper (chipset, BIOS/UEFI-stöd).

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -machine q35 \
  -m 2048 \
  -drive file=ubuntu.qcow2,format=qcow2
```

- **-machine q35**: modern maskintyp med PCIe-stöd.
- Standard är ofta `pc`.
- Maskintyp påverkar kompatibilitet med operativsystem och funktioner som UEFI.

## SMP (flera processorkärnor)

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -smp 4,sockets=1,cores=4,threads=1 \
  -m 4096 \
  -drive file=ubuntu.qcow2,format=qcow2
```

- **-smp 4**: totalt 4 vCPUs.
- **sockets/cores/threads**: specificerar hur de presenteras för gästen.
- Kan påverka prestanda och hur operativsystemet ser CPU-topologin.

## Översikt CPU, Maskintyp och SMP

| Inställning   | Exempel                              | När använda                                                            |
| ------------- | ------------------------------------ | ---------------------------------------------------------------------- |
| **CPU**       | `-cpu host` eller `-cpu qemu64`      | `host` för prestanda och hårdvarufunktioner, `qemu64` för portabilitet |
| **Maskintyp** | `-machine q35` eller `-machine pc`   | `q35` för moderna OS och PCIe-stöd, `pc` för äldre OS och enkelhet     |
| **SMP**       | `-smp 4,sockets=1,cores=4,threads=1` | Anpassa efter antal kärnor och hur du vill exponera CPU-topologi       |

## Virtio och ballooning

- **Virtio**: paravirtualiserade drivrutiner som ger högre prestanda jämfört med emulerad hårdvara (t.ex. nätverk och disk).
  - Exempel: `-device virtio-net-pci` för nätverk, `-device virtio-blk-pci` för disk.
  - Kräver drivrutiner i gästen (finns i Linux, måste installeras i Windows).
- **Ballooning**: dynamisk minneshantering där värden kan ta tillbaka RAM från gästen vid behov.
  - Exempel: `-device virtio-balloon-pci`.
  - Gästsystemet justerar sitt minne efter instruktioner från värden.

## QEMU Guest Additions

- Linux-gäster:
  - Använd virtio-drivrutiner (nät, disk, balloon, GPU) som redan finns i kärnan.
  - Extra funktioner som förbättrad grafik, musintegration och delade mappar kan läggas till via SPICE-agenter och 9p-filsystem.
- Windows-gäster:
  - QEMU har inga officiella "guest additions" som i VirtualBox.
  - Istället används virtio-drivrutiner för Windows (kan laddas ner från Fedora/virtio-win).
  - Ger bättre nätverk, disk, och ibland grafikprestanda.
- Sammanfattning:
  - Linux: fullt stöd direkt i kärnan, komplettera med SPICE-agent.
  - Windows: installera virtio-drivrutiner, finns inget eget "guest additions"-paket.

## Interagera med QEMU under körning

När en VM körs kan du öppna QEMUs monitor (kommandogränssnitt) för att påverka inställningar och köra kommandon utan att starta om.

- Starta VM med monitor på terminalen:

    ```bash
    qemu-system-x86_64 \
      -enable-kvm \
      -m 2048 \
      -smp 2 \
      -drive file=ubuntu.qcow2,format=qcow2 \
      -monitor stdio
    ```

- Vanliga kommandon i QEMU-monitor:
  - **info**: visar allt man kan få information om.
  - **info block**: visar information om diskar.
  - **info network**: visar nätverkskort.
  - **device_add/device_del**: lägg till eller ta bort virtuella enheter dynamiskt.
  - **change**: byt CD/DVD-avbild under körning.
  - **savevm / info snapshots / loadvm / delvm**: hantera snapshots.
  - **quit**: stäng av VM.

**Frågor och svar**:

- **Kommer man in i monitorn direkt med `-monitor stdio`?** Ja, kommandotolken för QEMU-monitor öppnas i samma terminal direkt.
- **Är monitorn kopplad till just den VM som körs?** Ja, varje QEMU-process har sin egen monitor. Om du startar flera VMs behöver varje VM sin egen monitor (t.ex. en monitor per terminalfönster eller via olika sockets).
**Notera**: QEMU-monitor är kraftfullt men låg nivå.
- QEMU-monitor har två lägen:
  - **Human Monitor Protocol (HMP)**: textkommandon för människor, som i exemplen ovan.
  - **QEMU Machine Protocol (QMP)**: JSON-baserat gränssnitt för program/script att kommunicera med QEMU.

I senare steg kommer vi att använda libvirt/virsh som bygger ovanpå QMP för ett mer strukturerat sätt att ändra inställningar under körning.

## Starta QEMU med QMP

För att använda QMP startar du QEMU med en socket för maskinprotokollet.

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 2048 \
  -smp 2 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -qmp unix:/tmp/qmp-sock,server,nowait
```

- **-qmp unix:/tmp/qmp-sock,server,nowait**: skapar en UNIX-socket där QEMU tar emot JSON-kommandon.
- Anslut till socketen med `socat` eller `nc` och skicka JSON-meddelanden.
- Exempel: efter att du anslutit måste du först skicka {"execute": "qmp_capabilities"} för att aktivera funktionerna. Därefter kan du skicka t.ex. {"execute": "query-status"} för att visa VM:ens status:

```bash
printf '%s\n' \
  '{"execute":"qmp_capabilities"}' \
  '{"execute":"query-status"}' \
| socat - UNIX-CONNECT:/tmp/qmp-sock
```

QMP används oftast av verktyg och bibliotek, inte direkt av människor.

Exempel på output:

```json
{"QMP": {"version": {"qemu": {"micro": 18, "minor": 2, "major": 7}, "package": "Debian 1:7.2+dfsg-7+deb12u15"}, "capabilities": ["oob"]}}
{"return": {}}
{"return": {"status": "running", "singlestep": false, "running": true}}
```

- Första raden: QEMU skickar en hälsning med version och vilka funktioner som stöds.
- Andra raden: svar på qmp_capabilities, visar att handskakningen lyckats.
- Tredje raden: svar på query-status, visar att VM körs (status: running).

## Utan nätverksanslutning

Starta en VM utan nätverk:

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 1024 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -net none
```

- **-net none**: stänger av all nätverksanslutning för VM.
- Användbart för helt isolerade tester.

```plaintext
[Värd]
  |
[ VM ]  (ingen NIC)
```

```plaintext
[Virtuell maskin]-   X   -[Nätverk]
```

## Port forward till värden

Skapa en VM med användarnätverk och vidarebefordra portar:

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 1024 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -net user,hostfwd=tcp::2222-:22 \
  -net nic
```

- **-net user**: aktiverar användarläge för nätverk (NAT via värden). Detta kallas även slirp av historiska skäl. Internet via TCP/UDP fungerar normalt, men ICMP (ping) stöds inte i detta läge.
- **hostfwd=tcp::2222-:22**: vidarebefordrar port 2222 på värden till port 22 (SSH) i gästen.

```plaintext
Internet
   |
[Värd]  <—— ssh tcp/2222 ——  (NAT & portfwd)
   |                          \
 [ VM ] 22/tcp  <——————— forwarded ————/
```

```plaintext
[Host:2222] ---> [VM:22]
```

**Tips:**

- Testa internet med TCP i gästen: `curl -I https://example.com` eller `sudo apt update`.
- Vill du kunna pinga: använd TAP/bridge i stället för user-mode NAT.

## Tap och bridge

Använd TAP-enhet och brygga mot värdens nätverk:

```bash
sudo ip tuntap add dev tap0 mode tap user $(whoami)
sudo ip link set tap0 up
sudo brctl addbr br0
sudo brctl addif br0 tap0 eth0
sudo ip link set br0 up

qemu-system-x86_64 \
  -enable-kvm \
  -m 1024 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -netdev tap,id=mynet0,ifname=tap0,script=no,downscript=no \
  -device e1000,netdev=mynet0
```

- **TAP**: ett virtuellt nätverksinterface som beter sig som ett nätverkskort kopplat till VM. Värden kan läsa och skriva paket direkt på detta interface.
- **Bridge (br0)**: en virtuell switch på värden som kopplar ihop flera nätverksinterface (t.ex. fysiskt kort + TAP). Detta gör att VM kan få IP från samma nät som värden.
- **Nätverksläge**: detta är "bridged networking" där gästen blir en fullvärdig del av LAN, synlig för routern och andra maskiner.
- **Begränsning**: fungerar i praktiken endast med Ethernet. De flesta WiFi-drivrutiner stöder inte att sätta gränssnittet i bridge-läge.
- **Alternativ**: man kan skapa och hantera bryggor med `NetworkManager` istället för `ip` och `brctl`.
  - Exempel: `nmcli connection add type bridge ifname br0` och sedan lägga till `eth0` och `tap0` som slavar.
  - **Fördelar med ip/brctl**: enkla och tydliga kommandon, fungerar utan extra verktyg.
  - **Fördelar med NetworkManager**: bryggan sparas som permanent konfiguration, enklare att hantera om man ofta växlar nätverk.

```plaintext
Internet/LAN
     |
   [Switch/Router]
        |
      [värd: eth0]
           \
          [br0]
           / \
       [tap0] [eth0]
          |
        [ VM ]  (får IP från LAN/DHCP)
```

```plaintext
[VM] <--> [tap0] <--> [br0] <--> [LAN / Internet]
```

## Isolerat nätverk (likt Docker)

Skapa en virtuell brygga för flera VMs:

```bash
sudo brctl addbr virbr1
sudo ip link set virbr1 up

qemu-system-x86_64 \
  -enable-kvm \
  -m 1024 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -netdev bridge,id=mynet0,br=virbr1 \
  -device virtio-net-pci,netdev=mynet0
```

- **virbr1**: en ny brygga isolerad från värdens LAN.
- **Nätverksläge**: detta är ett helt isolerat nät, ungefär som ett eget internt switchat segment. VMs på samma brygga kan prata med varandra men inte med värdens LAN eller internet.
- **Användning**: bra för labb och testmiljöer där man vill att flera VMs ska kommunicera privat.
- **Begränsning**: gästerna når inte internet utan att värden agerar router/NAT.
- **Alternativ**: kan även konfigureras via NetworkManager (`nmcli connection add type bridge ifname virbr1`).

  - **Fördelar med ip/brctl**: direkt kontroll, enkelt att testa.
  - **Fördelar med NetworkManager**: gör bryggan permanent och integrerad med systemets nätverkskonfiguration.

```plaintext
           [värd]
             |
          [virbr1]   (ingen route/NAT mot LAN)
           /    \
        [VM1]  [VM2]
       10.10.0.2 10.10.0.3  (kan prata inom segmentet)
```

```plaintext
[VM1] <--> [virbr1] <--> [VM2]
```

## Skapa snapshots

Skapa snapshot medan VM är igång (inkluderar RAM och runtime-tillstånd = online snapshot):

```bash
(qemu) savevm snap1
```

Skapa snapshot när VM är avstängd (endast diskens tillstånd = offline snapshot):

```bash
qemu-img snapshot -c snap1 ubuntu.qcow2
```

- **Online snapshot**: tar med RAM och pågående tillstånd.
- **Offline snapshot**: tar endast med diskfilens tillstånd.

## Lista och återställ snapshots

- Lista snapshots i QEMU-monitor:

```bash
(qemu) info snapshots
```

- Ladda snapshot (återställ VM till det tillståndet):

```bash
(qemu) loadvm snap1
```

- **Online**: VM återgår även till sparat RAM-tillstånd.
- **Offline**: VM återgår till diskens sparade läge, men startar om från det.

## Ta bort snapshots

- Ta bort snapshot i QEMU-monitor:

```bash
(qemu) delvm snap1
```

- Ta bort snapshot från en diskfil:

```bash
qemu-img snapshot -d snap1 ubuntu.qcow2
```

**Notera**: Att ta bort snapshot frigör inte alltid allt utrymme direkt, eftersom data kan vara kedjad i flera qcow2-lager.

## UEFI och NVRAM

- **UEFI**: använd `OVMF` (Open Virtual Machine Firmware) istället för BIOS.

  ```bash
  qemu-system-x86_64 \
    -enable-kvm \
    -bios /usr/share/OVMF/OVMF_CODE.fd \
    -drive if=pflash,format=raw,file=OVMF_VARS.fd \
    -drive file=ubuntu.qcow2,format=qcow2
  ```

- **NVRAM-fil (OVMF\_VARS.fd)**: lagrar UEFI-inställningar.

  - När du tar snapshots måste du även spara/kopiera denna fil om du vill behålla UEFI-inställningarna.
  - Snapshots via `qemu-img` påverkar inte automatiskt NVRAM-filen.

## TPM (Trusted Platform Module)

- TPM kan emuleras i QEMU via `swtpm`.
- Exempel:

  ```bash
  swtpm socket --tpm2 --tpmstate dir=/tmp/mytpm --ctrl type=unixio,path=/tmp/mytpm/swtpm-sock &

  qemu-system-x86_64 \
    -enable-kvm \
    -m 2048 \
    -drive file=ubuntu.qcow2,format=qcow2 \
    -chardev socket,id=chrtpm,path=/tmp/mytpm/swtpm-sock \
    -tpmdev emulator,id=tpm0,chardev=chrtpm \
    -device tpm-tis,tpmdev=tpm0
  ```

- **Notera**: TPM-state lagras i en separat katalog (här `/tmp/mytpm`). Om du tar snapshots måste du även spara denna katalog för att få en komplett återställning.

## UEFI + TPM för Windows 11

- Windows 11 kräver UEFI + TPM + Secure Boot.
- Exempel (Windows 11 Enterprise Evaluation):

  ```bash
  swtpm socket --tpm2 --tpmstate dir=/tmp/mytpm --ctrl type=unixio,path=/tmp/mytpm/swtpm-sock &

  qemu-system-x86_64 \
    -enable-kvm \
    -m 4096 \
    -cpu host \
    -smp 4 \
    -bios /usr/share/OVMF/OVMF_CODE.fd \
    -drive if=pflash,format=raw,file=OVMF_VARS.fd \
    -drive file=win11.qcow2,format=qcow2 \
    -cdrom Win11_Enterprise_Eval.iso \
    -chardev socket,id=chrtpm,path=/tmp/mytpm/swtpm-sock \
    -tpmdev emulator,id=tpm0,chardev=chrtpm \
    -device tpm-tis,tpmdev=tpm0 \
    -spice port=5902,disable-ticketing=on
  ```

- **Snapshots**: kräver att du hanterar både `win11.qcow2`, `OVMF_VARS.fd` och TPM-state-mappen om du vill återställa till exakt samma tillstånd.
