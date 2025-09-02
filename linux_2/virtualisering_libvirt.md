# Virtualisering - Libvirt

## Vad är Libvirt?

Libvirt är ett programvaruramverk som används för att hantera olika virtualiseringstekniker, även kallade hypervisors. Det fungerar som en gemensam mellanhand mellan själva virtualiseringsmotorn och de verktyg eller användare som styr den.

- Libvirt ger ett enhetligt sätt att kontrollera virtuella maskiner, oavsett vilken hypervisor som används (t.ex. KVM, Xen, VirtualBox).
- Det används till allt från produktionssystem och molnplattformar till testmiljöer.
- Istället för att kalla en virtuell maskin för "VM" använder Libvirt ofta begreppet "domän" för att betona att det är en självständig, konfigurerbar instans med resurser och metadata.

### Teknisk översikt över Libvirt

- Begreppet "domän" används som en generell abstraktion för virtuella instanser, oavsett vilken teknik som ligger bakom. Det gör det möjligt att använda samma modell oavsett hypervisor.
- Libvirt tillhandahåller en bakgrundstjänst, vanligtvis kallad `libvirtd` (eller `virtqemud` i nyare system), som sköter kommunikation med hypervisorn. Tjänsten ansvarar bland annat för att starta VM\:ar automatiskt om de är markerade för det.
- Kommandoverktyg som används med libvirt är bland annat `virsh`, `virt-install`, `virt-clone` och `virt-log`.
- Det finns även grafiska verktyg som `virt-manager` och `virt-viewer` som gör det lättare att hantera virtuella maskiner visuellt.
- Varje virtuell maskin beskrivs i en XML-fil, vilket gör det enkelt att automatisera, spara och återanvända konfigurationer.

### System- och användarläge i Libvirt

Libvirt kan köras i två olika lägen: systemläge och användarläge. Dessa avgör var dina virtuella maskiner lagras, vilka rättigheter som krävs och hur VM\:arna startas.

#### Systemläge (`qemu:///system`)

**Fördelar:**

- Full tillgång till systemresurser.
- Möjlighet att använda avancerade nätverkslägen (t.ex. bridges).
- VM\:ar kan autostartas och köras oberoende av inloggning.
- Centraliserad hantering för alla användare.

**Nackdelar:**

- Kräver root eller att användaren tillhör `libvirt`-gruppen.
- Felkonfigurationer påverkar hela systemet.

> Systemläget är standardval i produktion och för maskiner som behöver vara tillgängliga kontinuerligt.

- VM\:ar hanteras som systemresurser.
- Kräver root-behörighet (eller att du tillhör gruppen `libvirt`).
- Konfigurationer lagras i `/etc/libvirt/`.
- VM\:ar kan startas automatiskt vid systemstart.
- Används ofta i produktion, servermiljöer och molninfrastruktur.

#### Användarläge (`qemu:///session`)

**Fördelar:**

- Kräver inte root- eller sudo-behörighet.
- Isolerad till användarens egen miljö, vilket minskar risker vid test.
- Fungerar utan systemtjänster (t.ex. i sandboxade miljöer).

**Nackdelar:**

- VM\:ar kan inte startas automatiskt vid systemstart.
- Begränsad åtkomst till vissa systemresurser (exempelvis bridgat nätverk).
- Varje användare har sin egen uppsättning VM\:ar.

> Användarläget fungerar oftast bäst för utveckling, tester eller lokala labb där du vill undvika rootbehörighet.

- VM\:ar körs under den inloggade användarens session.
- Kräver inga root-rättigheter.
- Konfigurationer lagras i `~/.config/libvirt/`.
- VM\:ar startas och stoppas bara när användaren är inloggad.
- Används ofta för test och utveckling.

I `virt-manager` kan du välja mellan dessa när du ansluter till libvirt. Systemläge är standard i de flesta fall.

## Installation och uppsättning

### Kontrollera CPU-stöd för virtualisering

Om du inte redan har installerat QEMU/KVM:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
lsmod | grep kvm
```

### Installera QEMU/KVM (om inte redan installerat)

```bash
sudo apt install qemu-kvm qemu-utils
sudo usermod -aG kvm $USER
```

> Starta om systemet för att ändringen ska gälla.

### Installera Libvirt

Följande förutsätter att QEMU/KVM redan är installerat och att du tillhör gruppen `kvm`.

```bash
sudo apt install libvirt-daemon-system libvirt-clients bridge-utils
sudo usermod -aG libvirt $USER
```

> Starta om systemet för att ändringen ska gälla.

Starta och aktivera tjänsten:

```bash
sudo systemctl enable --now libvirtd
```

Bekräfta att libvirt fungerar:

```bash
virsh list --all
```

## De viktigaste verktygen

De tre viktigaste verktygen i Libvirt är `virsh`, `virt-install` och `virt-manager`.

### Vad är virsh?

Virsh är ett terminalprogram för att styra libvirt. Det ger tillgång till att skapa, starta, stänga av och konfigurera VM\:ar.

- `virsh list`, `virsh start`, `virsh shutdown`.
- `virsh edit` för att ändra XML direkt.
- Stöd för snapshots, nätverk, lagring m.m.

### Vad är virt-install?

Virt-install används för att skapa nya VM\:ar från kommandoraden.

- Kan skapa och ansluta diskar, ISO-filer.
- Kan specificera RAM, CPU, nätverk, grafik.
- Går att använda i script.

### Vad är virt-manager?

Virt-manager är ett GUI för att hantera libvirt. Det ger en grafisk överblick och kan användas för:

- Att skapa VM\:ar via guider.
- Visa resurser och status.
- Ansluta till konsol (VNC eller SPICE).

## Diskhantering

- Diskar skapas som `.qcow2` eller `.raw`.
- Kan skapas med `qemu-img` (se materialet om QEMU/KVM) eller via `virt-install` (beskrivs nedan).

  Diskar kopplas till VM via XML eller `virt-install`-flaggor.

## Skapa en domän

Här visas hur man skapar en ny virtuell maskin (domän) med Libvirt, med 5000 MB RAM, SPICE-grafik och standardnätverket.

### Med virt-manager

1. Starta `virt-manager`.
2. Klicka på "Skapa en ny virtuell maskin".
3. Välj installationskälla (t.ex. ISO-fil).
4. Välj RAM: 5000 MB och minst 2 CPU\:er.
5. Skapa en disk (t.ex. 70 GB qcow2).
6. Välj nätverk: standard (`default`).
7. Under "Slutför", klicka på "Anpassa innan installation" och välj SPICE som grafik.
8. Starta installationen.

### Med virt-install

```bash
virt-install \
  --name ubuntu-vm \
  --memory 5000 \
  --vcpus 2 \
  --disk size=70,path=/var/lib/libvirt/images/ubuntu.qcow2,format=qcow2 \
  --cdrom /path/to/ubuntu.iso \
  --network network=default,model=virtio \
  --graphics spice \
  --os-type linux \
  --os-variant ubuntu22.04
```

VM\:en startas direkt efter skapande.

### Med virsh och XML

> **Observera:** Diskfilen måste ha skapats innan du använder XML\:en. Använd till exempel `qemu-img create -f qcow2 /var/lib/libvirt/images/ubuntu.qcow2 70G`.

Skapa en XML-fil, t.ex. `ubuntu-vm.xml`:

```xml
<domain type='kvm'>
  <name>ubuntu-vm</name>
  <memory unit='MiB'>5000</memory>
  <vcpu placement='static'>2</vcpu>
  <os>
    <type arch='x86_64'>hvm</type>
    <boot dev='hd'/>
  </os>
  <devices>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source file='/var/lib/libvirt/images/ubuntu.qcow2'/>
      <target dev='vda' bus='virtio'/>
    </disk>
    <interface type='network'>
      <source network='default'/>
      <model type='virtio'/>
    </interface>
    <graphics type='spice' autoport='yes'/>
  </devices>
</domain>
```

Skapa och starta VM:

```bash
virsh define ubuntu-vm.xml
virsh start ubuntu-vm
```

## Libvirts nätverksmodell

### Standardläge: NAT via virbr0

- Libvirt skapar ett virtuellt nätverk `virbr0` som standard.
- Det fungerar som en NAT-router med DHCP.
- Gäst-VM får en IP-adress via DHCP och kan nå internet.
- Värden agerar som gateway.
- Motsvarar QEMUs `-net user`, men Libvirt ger mer kontroll över nätverksinställningar. Du kan till exempel enkelt styra DHCP-inställningar, öppna portar för åtkomst från värddatorn, och konfigurera NAT mer detaljerat. Libvirt skapar även en tydlig, beständig nätverksinfrastruktur som kan hanteras separat från varje individuell VM.

**Så här väljer du detta läge:**

- **virt-manager:** Välj nätverk "default" i nätverksinställningar.
- **virt-install:** Lägg till flaggan `--network network=default,model=virtio`.
- **XML / virsh:**

```xml
<interface type='network'>
  <source network='default'/>
  <model type='virtio'/>
</interface>
```

### Isolerat nätverk (som Docker `none`)

- Möjligt att skapa ett isolerat nätverk där VM inte har någon nätverksanslutning.

**Så här skapar du ett nytt isolerat nätverk:**

1. Skapa ett nätverks-XML, t.ex. `isolated-net.xml`:

```xml
<network>
  <name>isolerat-natverk</name>
  <forward mode='none'/>
  <bridge name='virbr1' stp='on' delay='0'/>
</network>
```

2. Skapa och starta nätverket:

```bash
virsh net-define isolated-net.xml
virsh net-start isolerat-natverk
virsh net-autostart isolerat-natverk
```

**Så här väljer du detta läge:**

- **virt-manager:** Välj ett definierat isolerat nätverk eller skapa ett utan NAT/DHCP.
- **virt-install:** Använd `--network network=isolerat-nätverk` (om skapat).
- **XML / virsh:**

```xml
<interface type='network'>
  <source network='isolerat-natverk'/>
  <model type='virtio'/>
</interface>
```

### Portforward (user-mode NAT med öppnad port)

- Libvirt stöder också att öppna portar genom `virsh` eller via XML.
- Exempel: vidarebefordra port 2222 på värden till 22 i gästen (SSH).
- Motsvarar `-net user,hostfwd=...` i QEMU.

**Så här väljer du detta läge:**

- **virt-manager:** Port forward kräver manuell redigering i nätverksdefinitionen via `virsh net-edit default`. Virt-manager har inte inbyggt GUI-stöd för att lägga till portvidarebefordring.

  > **Notera:** Om flera VM\:ar använder nätverket `default` kommer portvidarebefordringen att gälla för alla dessa. Skapa ett separat nätverk (se Instruktionerna i "Isolerat nätverk" ovan) om du vill att portarna bara ska vidarebefordras till en viss VM.
- **virt-install:**

```bash
--network network=default,model=virtio,port=hostfwd=tcp::2222-:22
```

- **XML / virsh:**

```xml
<interface type='network'>
  <source network='default'/>
  <model type='virtio'/>
  <protocol type='tcp'>
    <host port='2222'/>
    <guest port='22'/>
  </protocol>
</interface>
```

### Bridgat nätverk

- Använder en redan existerande Linux-brygga (t.ex. `br0`) på värdsystemet. Du behöver inte skapa ett separat nätverk i Libvirt.

- Se "Tap och bridge" i QEMU-materialet för instruktioner kring att skapa en brygga.

- Kräver systemläge och root/grupp-rättigheter.

- Gäst-VM kan få IP från samma DHCP som värddatorn.

**Så här väljer du detta läge:**

- **virt-manager:** Välj nätverkstyp "Bridged" och välj din fysiska nätverksadapter.
- **virt-install:**

```bash
--network bridge=br0,model=virtio
```

- **XML / virsh:**

```xml
<interface type='bridge'>
  <source bridge='br0'/>
  <model type='virtio'/>
</interface>
```

## Snapshots

> **OBS:** Libvirt stöder inte snapshots för UEFI-baserade VM\:ar. Om du använder UEFI (OVMF) och vill använda snapshots måste du istället hantera dessa direkt via QEMU-gränssnittet. Se dokumentet om QEMU/KVM för detaljer.

### Med `virsh`

> Snapshot-funktionerna i `virsh` stöder både online (när VM är igång) och offline (när VM är avstängd) snapshots. Standardbeteendet är att skapa en online-snapshot om VM är aktiv.

- **Skapa snapshot:**

  ```bash
  virsh snapshot-create-as ubuntu-vm snap1 "Min första snapshot"
  ```

- **Lista snapshots:**

  ```bash
  virsh snapshot-list ubuntu-vm
  ```

- **Återställ till snapshot:**

  ```bash
  virsh snapshot-revert ubuntu-vm snap1
  ```

- **Ta bort snapshot:**

  ```bash
  virsh snapshot-delete ubuntu-vm snap1
  ```

### Med `virt-manager`

> Virt-manager skapar som standard en online-snapshot om VM är igång. Du kan stänga av VM innan snapshot tas om du vill få en offline-snapshot.

1. Högerklicka på en VM i listan och välj **Snapshots**.
2. Klicka på **Ta snapshot**.
3. Namnge snapshoten.
4. För att återgå, markera snapshoten och välj **Återställ**.
5. För att ta bort, markera snapshoten och klicka **Ta bort**.
