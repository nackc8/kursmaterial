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
- **-vnc :0**: öppna VNC-server på display :0 (port 5900). Värdet `:n` har förhållandet `n+5900` till porten.
- **Installera en VNC-klient**:
  - Linux: t.ex. `sudo apt install tigervnc-viewer`
  - Windows: tips – **RealVNC Viewer** eller **TightVNC** fungerar bra.
  - macOS: tips – **TigerVNC** eller **RealVNC Viewer** fungerar bra.

**Notera**: I denna form är konfigurationen statisk. Du kan inte ändra argument som `-cdrom`, `-boot`, eller nätverksinställningar (t.ex. port forwarding) medan VM körs. För att ändra dessa måste du stoppa VM och starta om den med nya parametrar.

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

- **Anslut med**: `remote-viewer spice://127.0.0.1:44400`
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
  -m 5000 \
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
  -m 5000 \
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

## Interagera med QEMU under körning

När en VM körs kan du öppna QEMUs monitor (kommandogränssnitt) för att påverka inställningar och köra kommandon utan att starta om.

- Starta VM med monitor på terminalen:

    ```bash
    qemu-system-x86_64 \
      -enable-kvm \
      -m 5000 \
      -smp 2 \
      -drive file=ubuntu.qcow2,format=qcow2 \
      -monitor stdio
    ```

- Vanliga kommandon i QEMU-monitor:
  - **info block**: visar information om diskar.
  - **info network**: visar nätverkskort.
  - **device_add/device_del**: lägg till eller ta bort virtuella enheter dynamiskt.
  - **change**: byt CD/DVD-avbild under körning.
  - **eject ide1-cd0**: ta ut CD/DVD-avbilden under körning (eller `info block` för att se om den heter något annat än `ide1-cd0`).
  - **boot_set c**: Boota från hårddisken (används ofta tillsammans med `eject` efter färdig installation).
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
  -m 5000 \
  -smp 2 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -qmp unix:/tmp/qmp-sock,server,nowait
```

- **-qmp unix:/tmp/qmp-sock,server,nowait**: skapar en UNIX-socket där QEMU tar emot JSON-kommandon.
- Anslut till socketen med `socat` eller `nc` och skicka JSON-meddelanden.
- Exempel: skicka följande JSON till QMP-socketen för att visa VM:ens status:

```bash
echo '{"execute": "qmp_capabilities"}{"execute": "query-block"}' | socat - UNIX-CONNECT:/tmp/qmp-sock
```

QMP används oftast av verktyg och bibliotek, inte direkt av människor.

## Ingen nätverksanslutning

Starta en VM utan nätverk:

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 5000 \
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

## Port forward från värden till gästen

Skapa en VM med användarnätverk och vidarebefordra portar:

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 5000 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -net user,hostfwd=tcp::2222-:22 \
  -net nic
```

- **-net user**: aktiverar användarläge för nätverk (NAT via värden).
- **hostfwd=tcp::2222-:22**: vidarebefordrar port 2222 på värden till port 22 (SSH) i gästen.

```plaintext
Internet
   |
[Värd]  <—— ssh tcp/2222 ——  (NAT & portfwd)
   |                                   \
 [ VM ] 22/tcp  <——————— forwarded ————/
```

```plaintext
[Host:2222] ---> [VM:22]
```

## Tap och bridge

**Bakgrund för nybörjare:**

- **TUN** och **TAP** är virtuella nätverkskort som skapas i mjukvara. TUN jobbar med IP-paket (ungefär som en router gör), medan TAP jobbar med hela Ethernet-ramar (som en nätverkskabel eller switch gör).
- **Bridge** fungerar som en mjukvaruswitch i Linux. Den kopplar ihop flera nätverkskort så att de hamnar i samma nät. Man kan se det som att flera kort sammanfogas till ett gemensamt logiskt kort. En skillnad mot en typisk switch är att den kan ges ett IP-nummer, vilket typiskt är IP-numret som värddatorn använder för att ansluta via bryggan.

Genom att kombinera en TAP-enhet och en bridge kan en virtuell maskin kopplas in i samma nät som värden, få en egen IP-adress (via DHCP eller statiskt) och prata med andra datorer precis som om den var inkopplad i ett riktigt nätverk.

### Exempel: skapa TAP och bridge

I det här exemplet gör vi ändringar i nätverket som bara gäller i RAM. Det innebär att en omstart återställer datorn till sitt vanliga läge. De fyra första raderna installerar en extern DHCP-klient (men väljer att inte ha dess service aktiv) och stänger av tjänsten NetworkManager, som annars hanterar nätverksinställningarna. Detta behövs för att br0 ska kunna få en IP-adress via DHCP och för att NetworkManager inte ska skriva över konfigurationen under testet.

Byt `eth0` nedan mot ditt faktiska nätverkskort. Ett enkelt sätt att hitta rätt är att köra `ip address` och se vilket kort som har din dators nuvarande IP-adress.

```bash

sudo apt update
sudo apt install dhcpcd5
sudo systemctl disable --now dhcpcd.service
sudo systemctl stop NetworkManager
sudo ip tuntap add dev tap0 mode tap user "$USER"
sudo ip link set tap0 up
sudo ip link add br0 type bridge
sudo ip link set eth0 master br0
sudo ip link set tap0 master br0
sudo ip link set br0 up
sudo ip addr flush dev eth0 # Kasta IP-numret eth0 har
sudo dhcpcd br0 # Hämta en IP till br0 via DHCP

qemu-system-x86_64 \
  -enable-kvm \
  -m 5000 \
  -drive file=ubuntu.qcow2,format=qcow2 \
  -netdev tap,id=mynet0,ifname=tap0,script=no,downscript=no \
  -device e1000,netdev=mynet0
```

- **TAP**: virtuellt nätverkskort för VM.
- **Bridge (br0)**: mjukvaruswitch som binder ihop TAP och värddatorns nätverksport.

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

```plaintext
[VM] <--> [tap0] <--> [br0] <--> [eth0] <--> [LAN / Internet]
```

### Vad kan vara med i en bridge?

- Fysiska nätverkskort (t.ex. `eth0`, `eth1`)
- Virtuella TAP-enheter
- Andra virtuella interface som `veth`-par (ett par virtuella nätverkskablar som kopplar ihop två nätverksnamnsrum (network namespaces, alltså isolerade nätverksmiljöer i Linux)) eller `macvtap` (en virtuell port som kopplas direkt mot ett fysiskt kort och kan användas av en VM). Dessa ligger utanför kursen.

Alla dessa kan kopplas in som portar i en bridge, förutsatt att de är i samma nät.

### MAC-adresser i en bridge

- En bridge (`br0`) har ett en egen MAC-adress. Vanligtvis väljer den en av portarnas MAC när den skapas, men den kan också få en egen.
- Denna MAC används för värddatorns IP-trafik.
- Varje port (t.ex. `tap0`, `eth0`) behåller sin egen MAC, men fysiska portar använder inte sin IP längre när de är del av bryggan.

### IP-adressering i en bridge

- Alla portar i en bridge måste tillhöra samma nät (samma Layer-2-segment).
- Fysiska kort som `eth0`/`eth1` blir bara portar och har ingen egen IP längre.
- Själva bryggan (`br0`) är det interface som värddatorn använder och som får en IP via DHCP eller statiskt.
- Varje TAP-enhet har en egen MAC-adress och kan därför få en egen IP-adress (via DHCP eller statiskt), precis som en fysisk dator i nätet.
- Resultatet blir att `br0` har värddatorns IP, medan varje TAP motsvarar en separat maskin i nätet med egen IP.

### Hur får `br0` en IP via DHCP?

För att värddatorn ska få nätverksåtkomst måste `br0` själv begära en IP-adress från DHCP. Det görs med vanliga DHCP-klienter:

```bash
sudo dhclient br0
```

(Debian använder `dhclient`; andra system kan använda `dhcpcd`.)

### Hur får en VM en IP via DHCP?

En virtuell maskin som är kopplad via TAP till en bridge beter sig som en egen dator i nätet. När den startar och kör en DHCP-klient (oftast automatiskt via operativsystemet) skickar den en förfrågan med sin egen MAC-adress, och kan då få en unik IP från samma DHCP-server som värddatorn använder.

### Tips om permanenta inställningar

Observera att `ip link`-kommandon bara gäller tills nästa omstart. Om du vill göra ändringarna permanenta använder du systemets nätverksverktyg, till exempel NetworkManager, `systemd-networkd` eller konfiguration i `/etc/network/interfaces`.

## Isolerat nätverk (likt Docker)

Skapa en virtuell brygga och anslut två virtuella nätverksinterface (`tap0` och `tap1`) till den. Både bryggan och nätverksinterfacen måste sättas till status `UP`. Det användarkonto som kör de virtuella maskinerna måste också ha rätt att använda nätverksinterfacen:

```bash
sudo ip link add ustwo_bridge type bridge
sudo ip link set ustwo_bridge up

sudo ip tuntap add dev tap0 mode tap user "$USER"
sudo ip link set tap0 up
sudo ip link set tap0 master ustwo_bridge

sudo ip tuntap add dev tap1 mode tap user "$USER"
sudo ip link set tap1 up
sudo ip link set tap1 master ustwo_bridge
```

Därefter startar vi två virtuella maskiner, var och en med en egen hårddisk. Observera att hårddiskarna behöver skapas i förväg och installeras med ett operativsystem, vilket inte ingår i exemplet. Maskinerna kopplas till var sitt nätverksinterface med unika MAC-adresser och kan nås via både VNC och SPICE:

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 2000 \
  -smp 2 \
  -drive file=ubuntu1.qcow2,format=qcow2 \
  -vnc :0 \
  -spice port=44400,disable-ticketing=on \
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
  -netdev tap,id=mynet0,ifname=tap1,script=no,downscript=no \
  -device virtio-net-pci,netdev=mynet0,mac=52:54:00:12:34:57 \
  -daemonize
```

### Nätverksöversikt

- **ustwo\_bridge**: En brygga som är helt isolerad från värddatorns LAN.
- Virtuella maskiner anslutna till samma brygga kan kommunicera med varandra, men har ingen åtkomst till LAN eller internet utan att routing eller NAT konfigureras.

```plaintext
           [värddator]
               |
        [ustwo_bridge]   (ingen route/NAT mot LAN)
           /       \
        [VM1]     [VM2]
```

```plaintext
[VM1] <--> [ustwo_bridge] <--> [VM2]
```

### Statisk IP-adress

Tilldela varje virtuell maskin en statisk IP-adress inifrån respektive system:

- ubuntu1: `sudo ip addr add 192.168.10.10/24 dev ens3`
- ubuntu2: `sudo ip addr add 192.168.10.20/24 dev ens3`

Nätverksgränssnittets namn kan variera och behöver inte vara `ens3`. Hoppa över att ange gateway, eftersom nätverket är helt internt. Testa sedan anslutningen med `ping` mellan maskinerna.

### DHCP med `dnsmasq`

Alternativt kan du konfigurera en enkel DHCP-server på värddatorn med `dnsmasq`.

1. Tilldela värden IP-adressen `10.10.0.1` i det isolerade nätverket:

```bash
sudo ip addr add 10.10.0.1/24 dev ustwo_bridge
```

2. Starta sedan DHCP-servern:

```bash
sudo dnsmasq --interface=ustwo_bridge --bind-interfaces \
  --dhcp-range=10.10.0.2,10.10.0.254,255.255.255.0,12h
```

Virtuella maskiner som ansluts till `ustwo_bridge` får då automatiskt en IP-adress tilldelad via DHCP.

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

## UEFI och NVRAM

- **UEFI**: använd `OVMF` (Open Virtual Machine Firmware) istället för BIOS. Installera paketet `ovmf` om du inte redan har det.

```bash
qemu-system-x86_64 \
  -enable-kvm \
  -m 3000 \
  -drive if=pflash,format=raw,readonly=on,file=/usr/share/OVMF/OVMF_CODE_4M.fd \
  -drive if=pflash,format=raw,file=Rocky10_UEFI_OVMF_VARS.fd \
  -drive file="Rocky10_UEFI.qcow2",format=qcow2 \
  -cdrom ~/Downloads/Rocky-10.0-x86_64-minimal.iso \
  -boot d \
```

- **NVRAM-fil**: lagrar UEFI-inställningar.

  - Kopiera mallfilen `/usr/share/OVMF/OVMF_VARS_4M.fd` till `Rocky10_UEFI_OVMF_VARS.fd`. Kopian kommer att agera lagringsutrymme för UEFI-inställningar som boot-ordning med mera. Det är viktigt att skriva till en kopia och inte till mallfilen.
  - När du tar snapshots måste du även spara/kopiera denna fil om du vill behålla UEFI-inställningarna.
  - Snapshots via `qemu-img` påverkar inte automatiskt NVRAM-filen.

## TPM (Trusted Platform Module)

- TPM kan emuleras i QEMU via `swtpm` (`sudo apt install swtpm`)
- Exempel:

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

Undersök sedan inne i VMen att TPM finns, vilket det gör om någon `/dev/tpm*` existerar:

```bash
ls /dev/tpm*
```

- **Notera**: TPM-state lagras i en separat katalog (här `/.ubuntutpm/mytpm`). Om du tar snapshots måste du även spara denna katalog för att få en komplett återställning.

## UEFI + TPM för Windows 11

- Windows 11 kräver UEFI + TPM + Secure Boot.
- Exempel (Windows 11 Enterprise Evaluation):

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
    -device tpm-tis,tpmdev=tpm0
    -spice port=44400,disable-ticketing=on
  ```

- **Secure boot**: kräver att du använder de OVMF-diskbilder som slutar med `.ms.fd`. Det krävs då dessa är förberedda med Microsofts signaturfiler, vilka i princip alla moderkort "litar på" (de ligger inlagda som signaturer att lita på) från början.
- **Snapshots**: kräver att du hanterar både `Win11.qcow2`, `Win11_OVMF_VARS_4M.ms.fd` och TPM-state-mappen `./win11tpm` om du vill återställa till exakt samma tillstånd.
