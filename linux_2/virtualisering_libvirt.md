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
- Det finns även grafiska verktyg som `virt-manager` och `virt-viewer`.
- Varje virtuell maskin beskrivs i en XML-fil, vilket gör det enkelt att automatisera, spara och återanvända konfigurationer.

⚠️ **Notera:** Libvirt är inte en hypervisor i sig utan ett abstraktionslager. Funktionalitet beror på vilken backend som används (t.ex. KVM eller Xen).

### System- och användarläge i Libvirt

Libvirt kan köras i två olika lägen: systemläge och användarläge.

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

- Konfigurationer lagras i `/etc/libvirt/`.
- VM\:ar kan startas automatiskt vid systemstart.

#### Användarläge (`qemu:///session`)

**Fördelar:**

- Kräver inte root- eller sudo-behörighet.
- Isolerad till användarens egen miljö.
- Fungerar utan systemtjänster (t.ex. i sandboxade miljöer).

**Nackdelar:**

- VM\:ar kan inte startas automatiskt vid systemstart.
- Begränsad åtkomst till resurser (t.ex. inga bridgade nät utan extra arbete).
- Varje användare har sin egen uppsättning VM\:ar.

> Användarläget fungerar bäst för utveckling, tester eller lokala labb. Inom kursen kommer vi endast att använda systemläget.

- Konfigurationer lagras i `~/.config/libvirt/`.

## Installation och uppsättning

### Kontrollera CPU-stöd för virtualisering

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

Kommandoraderna nedan gör följande:

- `sudo apt install ...` installerar libvirt och tillhörande verktyg.
- `usermod -aG libvirt $USER` lägger till användaren i gruppen `libvirt` så att du kan köra libvirt utan root.
- `systemctl enable --now libvirtd` startar och aktiverar libvirt-tjänsten.
- `echo 'export LIBVIRT_DEFAULT_URI=...' >> ~/.bashrc` gör att systemläge (`qemu:///system`) används som standard. Om du föredrar ett annat skal än Bash är det bra om du gör motsvarande inställning i dess uppstartsfiler.

```bash
sudo apt install virt-manager virtinst libvirt-daemon-system libvirt-clients bridge-utils virt-viewer
sudo usermod -aG libvirt $USER
sudo systemctl enable --now libvirtd

echo 'export LIBVIRT_DEFAULT_URI="qemu:///system"' >> ~/.bashrc
```

> Starta om systemet för att ändringen ska gälla.

Aktivera standardnätverket:

```bash
virsh net-define /usr/share/libvirt/networks/default.xml
virsh net-autostart default
virsh net-start default
```

Bekräfta att libvirt fungerar:

```bash
virsh list --all
```

## De viktigaste verktygen

- **virsh**: CLI för att styra domäner, nätverk, lagring, snapshots.
- **virt-install**: skapa nya VM\:ar från kommandoraden.
- **virt-manager**: GUI för enklare hantering.

⚠️ **Begränsning:** Vissa funktioner (t.ex. passt-nätverk) är ännu inte helt integrerade i virt-install och/eller virt-manager och måste anges manuellt i XML.

## Diskhantering

- Diskar skapas som `.qcow2` eller `.raw`.
- Kan skapas med `qemu-img` eller via `virt-install`.
- Diskar kopplas till VM via virt-manager, XML eller `virt-install`-flaggor.

**Exempel på att skapa en qcow2-disk på 30 GB:**

```bash
qemu-img create -f qcow2 /var/lib/libvirt/images/min-disk.qcow2 30G
```

## Skapa en domän

Tre alternativ:

- **virt-manager**: GUI, bra för nybörjare.
  **Instruktion:** Starta virt-manager → Skapa VM via guiden. Du kan välja ISO, diskstorlek och nätverk.
  **Fördelar:** Enkel att använda, bra överblick.
  **Nackdelar:** Mindre flexibel, begränsat för avancerade funktioner.

- **virt-install**: CLI, bra för automation.
  **Exempel:**

  ```bash
  virt-install \
    --name test-vm \
    --memory 2048 --vcpus 2 \
    --disk size=30,format=qcow2 \
    --cdrom /path/to/installer.iso \
    --network network=default,model=virtio \
    --graphics spice
  ```

  **Fördelar:** Scriptbart, snabbt, bra i servermiljö.
  **Nackdelar:** Kräver att du kan flaggorna, inte lika intuitivt som GUI.

- **XML + virsh**: full kontroll, men mer komplext.
  **Instruktion:** Skapa en XML-fil med domänbeskrivningen och definiera den:

  ```bash
  virsh define vm.xml
  virsh start vm
  ```

  **Minimal fungerande XML-exempel:**

  ```xml
  <domain type='kvm'>
    <name>minimal-vm</name>
    <memory unit='MiB'>1024</memory>
    <vcpu>1</vcpu>
    <os>
      <type arch='x86_64'>hvm</type>
      <boot dev='hd'/>
    </os>
    <devices>
      <disk type='file' device='disk'>
        <driver name='qemu' type='qcow2'/>
        <source file='/var/lib/libvirt/images/minimal.qcow2'/>
        <target dev='vda' bus='virtio'/>
      </disk>
      <interface type='network'>
        <source network='default'/>
      </interface>
      <graphics type='spice' autoport='yes'/>
    </devices>
  </domain>
  ```

  Dokumentation över XML-syntaxen finns på: [https://libvirt.org/formatdomain.html](https://libvirt.org/formatdomain.html)

  **Tips för VSCode (validering & autocompletion):**

1. Installera tillägget **XML (Red Hat)** i VSCode.
2. Använd libvirt\:s **RELAX NG (RNG)**-scheman för validering (libvirt använder RNG, inte XSD):

   - Exempel för en domän-XML:

```xml
<?xml version="1.0"?>
<?xml-model href="https://libvirt.org/schemas/domain.rng" schematypens="http://relaxng.org/ns/structure/1.0"?>
<domain type='kvm'>
  ...
</domain>
```

Detta aktiverar validering & förslag direkt i editorn.

3. Alternativ via VSCode-inställning (Settings → sök: *xml fileAssociations* → **Open Settings (JSON)**):

```json
"xml.fileAssociations": [
  {
    "pattern": "**/vm*.xml",
    "systemId": "https://libvirt.org/schemas/domain.rng"
  }
]
```

4. **Var hittar jag scheman?** Online finns de på libvirt\:s *schemas*-sida, till exempel:

- [https://libvirt.org/schemas/domain.rng](https://libvirt.org/schemas/domain.rng)
- [https://libvirt.org/schemas/network.rng](https://libvirt.org/schemas/network.rng)
- [https://libvirt.org/schemas/storagepool.rng](https://libvirt.org/schemas/storagepool.rng)
- Exempel för nätverk (`network.rng`):

  ```xml
  <?xml-model href="https://libvirt.org/schemas/network.rng" schematypens="http://relaxng.org/ns/structure/1.0"?>
  <network>
    ...
  </network>
  ```

- Exempel för storagepool (`storagepool.rng`):

  ```xml
  <?xml-model href="https://libvirt.org/schemas/storagepool.rng" schematypens="http://relaxng.org/ns/structure/1.0"?>
  <pool type='dir'>
    ...
  </pool>
  ```

**Fördelar:** Full flexibilitet, alla funktioner stöds.
**Nackdelar:** Mer komplext, manuellt arbete krävs.

⚠️ **Notera:** Virt-manager sparar VM-diskar i `/var/lib/libvirt/images/` i systemläge. I sessionläge används istället `~/.local/share/libvirt/images/`.

## Libvirts nätverksmodell

### Standardläge: NAT via virbr0

- Standardnätet `default` använder en intern bridge (`virbr0`).
- Fungerar som NAT-router med DHCP.
- Gäst-VM får IP via DHCP och når internet.
- Värden agerar som gateway.
- Motsvarar QEMUs `-net user`, men med bättre kontroll, till exempel kan du via `virsh net-edit` ändra DHCP-intervall eller lägga till statiska leases – något som inte går med QEMUs inbyggda user-mode NAT.

```plaintext
Internet
   |
 [ värd ]
   |
 [ virbr0 ] (NAT + DHCP)
   |
 [ VM ]
```

⚠️ **Viktigt:** virbr0 är en intern virtuell brygga – den delar inte värdens fysiska nät direkt.

**Exempel på virt-install:**

```bash
virt-install \
  --name vm-nat \
  --memory 2048 --vcpus 2 \
  --disk size=20,format=qcow2 \
  --cdrom /path/to/installer.iso \
  --network network=default,model=virtio \
  --graphics spice
```

**Exempel på virsh + XML (domänens interface):**

```xml
<interface type='network'>
  <source network='default'/>
  <model type='virtio'/>
</interface>
```

**Exempel för virt-manager:**

1. Öppna VM → **Details** → **NIC**. 2) **Network source:** `default` (NAT) → **Device model:** `virtio` → **Apply**.

### Isolerat nätverk

- VM får kommunicera med varandra via en intern bridge men inte ut mot internet.
- Används där extern åtkomst inte behövs.
- När du skapar ett nytt isolerat nätverk i libvirt skapas automatiskt en virtuell brygga. Den måste ha ett unikt namn, t.ex. `virbr1` (eftersom `virbr0` redan används av standardnätet `default`).

```plaintext
[ VM1 ]
   |
 [ virbr1 ] (ingen NAT)
   |
[ VM2 ]
```

**Varför behövs en ny brygga?**
Varje libvirt-nätverk får en egen brygga för att separera trafiken. `virbr0` används av default-nätet (NAT). För att undvika kollisioner skapas därför en ny brygga för det isolerade nätverket, t.ex. `virbr1`.

> **Obs:** Även om bryggan (`virbr1`) har en IP-adress (t.ex. `192.168.50.1`) för DHCP/DNS i det **interna** segmentet, är nätet isolerat eftersom `forward mode='none'`. Det innebär ingen NAT eller routing ut — adressen fungerar **inte** som gateway till internet.

**Exempel – skapa nätet (virsh + XML):**

`isolated-net.xml`:

```xml
<network>
  <name>isolerat-natverk</name>
  <forward mode='none'/>
  <bridge name='virbr1' stp='on' delay='0'/>
  <ip address='192.168.50.1' netmask='255.255.255.0'> <!-- används av libvirt för DHCP, men fungerar inte som gateway utåt eftersom nätet är isolerat -->
    <dhcp>
      <range start='192.168.50.100' end='192.168.50.254'/>
    </dhcp>
  </ip>
</network>
```

```bash
virsh net-define isolated-net.xml
virsh net-autostart isolerat-natverk
virsh net-start isolerat-natverk
```

**Exempel – virt-install:**

```bash
virt-install \
  --name vm-isolated \
  --memory 2048 --vcpus 2 \
  --disk size=20,format=qcow2 \
  --cdrom /path/to/installer.iso \
  --network network=isolerat-natverk,model=virtio \
  --graphics spice
```

**Exempel – virsh + XML (domänens interface):**

```xml
<interface type='network'>
  <source network='isolerat-natverk'/>
  <model type='virtio'/>
</interface>
```

**Exempel – virt-manager:**

1. **Edit → Connection Details → Virtual Networks** → **+** skapa nytt isolerat nät (Forwarding: None).
2. På VM: **NIC → Network source:** `isolerat-natverk`.

### Portforward till gäst i NAT-nät

- Libvirt kan NAT\:a via `virbr0`, men **per-VM-portforward** görs inte i domänens XML; reglerna gäller nätet.
- Rekommendation: ge gästen en **statisk DHCP-lease** och använd **nftables**/iptables på värden.

> **Notera:** Portforward-regler sätts på nätverket (`default`) och gäller därför alla VM\:ar som delar det nätet.
> Följ **Steg 1** och **Steg 2** nedan för **varje VM** på nätet som ska ha en port forward.

```plaintext
Internet
   |
 [ värd ] tcp/2222 ---> tcp/22 (DNAT)
   |
 [ virbr0 ] (NAT)
   |
 [ VM 192.168.122.50 ]
```

**Steg 1 – statisk DHCP-lease (virsh net-update):**

```bash
virsh net-update default add ip-dhcp-host \
  "<host mac='52:54:00:aa:bb:cc' name='vm-nat' ip='192.168.122.50'/>" \
  --live --config
```

**Steg 2 – nftables DNAT på värden:**

*nftables* är det moderna brandväggssystemet i Linux (efterföljare till iptables). Nedan kommandon gör följande steg för steg:

- `sudo nft add table ip nat` – skapar en ny tabell för NAT
- `sudo nft add chain ip nat PREROUTING { type nat hook prerouting priority 0; }` – skapar en kedja som behandlar inkommande trafik innan routingbeslut. Prioritet **0** innebär att regeln körs tidigt i kedjan.
- `sudo nft add chain ip nat POSTROUTING { type nat hook postrouting priority 100; }` – skapar en kedja som behandlar utgående trafik efter routingbeslut. Prioritet **100** styr bara ordningen inom POSTROUTING (högre = senare).
- `sudo nft add rule ip nat PREROUTING tcp dport 2222 dnat to 192.168.122.50:22` – skickar trafik till värdens port 2222 vidare till gästens port 22.
- `sudo nft add rule ip nat POSTROUTING oifname "virbr0" masquerade` – gör att svar från gästen ser ut att komma från värden (masquerade).

```bash
sudo nft add table ip nat
sudo nft add chain ip nat PREROUTING { type nat hook prerouting priority 0; }
sudo nft add chain ip nat POSTROUTING { type nat hook postrouting priority 100; }
sudo nft add rule ip nat PREROUTING tcp dport 2222 dnat to 192.168.122.50:22
sudo nft add rule ip nat POSTROUTING oifname "virbr0" masquerade
```

**Flöde – PREROUTING och POSTROUTING:**

```plaintext
[ Internet-klient ] <-------------------.
       |                                |
   (PREROUTING)                         |
       |                                |
       v                                |
   [ DNAT 2222 -> 22 ]                  |
       |                                |
    [ VM ]                              |
       |                                |
   (POSTROUTING)-> [ Masquerade via virbr0 ]
```

## Snapshots

> **OBS:** Libvirt stöder inte snapshots för UEFI-baserade VM\:ar. Om du använder UEFI (OVMF) och vill använda snapshots måste du istället hantera dessa direkt via QEMU-gränssnittet.

### Med `virsh`

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

1. Högerklicka på en VM i listan och välj **Snapshots**.
2. Klicka på **Ta snapshot**.
3. Namnge snapshoten.
4. För att återgå, markera snapshoten och välj **Återställ**.
5. För att ta bort, markera snapshoten och klicka **Ta bort**.

### Trädstruktur för snapshots

```plaintext
snap1
 ├── snap2
 │    └── snap3
 └── snap4
```

Varje snapshot kan ses som en gren. När du återgår till en äldre snapshot och skapar en ny, bildas en ny gren i trädet.
