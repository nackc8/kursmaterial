# Virtualisering - Introduktion

## 1. Vad är virtualisering?

- Abstraktion av hårdvara → köra flera system på samma maskin.
- Skapar isolerade miljöer (VMs).
- Användningsområden: test, utveckling, serverkonsolidering, äldre programvara, mjukvara gjord för ett annat OS.

## 2. Vad är containers?

- Containers delar kernel med värden.
- Snabb start, låg overhead.
- Används ofta för:
  - **Microservices**: många små tjänster som körs oberoende.
  - **Portabilitet**: enkelt att flytta samma container mellan utveckling, test och produktion.
- Mindre isolering än VMs.

## 3. Virtuella maskiner vs Containers

- VMs har ett fullständigt OS, containers delar kernel.
- VMs tyngre men mer isolerade, containers lättare och snabbare.
- VMs passar bra när man behöver köra olika operativsystem eller äldre mjukvara.
- Containers passar bra för att skala microservices och för snabb portabilitet mellan miljöer.

### 3.1 Övning: Virtuella maskiner vs Containers

Diskutera med andra vad som passar bäst för:

- Att köra en Windows-applikation?
- Att köra FreeBSD?
- Att köra macOS?
- Att köra mjukvara kompilerad för ARM-processorer?
- Att testa nya versioner av Linux-kärnan innan de installeras på företagets servrar?
- Att köra 100 microservices i produktion?

## 4. QEMU/KVM

### 4.1 Krav

- **QEMU kräver:** inget särskilt hårdvarustöd, men fungerar mycket snabbare med KVM.
- **KVM kräver:**
  - Hårdvarustöd för virtualisering (Intel VT-x/VMX eller AMD-V/SVM).
  - Att modulerna `kvm` och `kvm_intel`/`kvm_amd` kan laddas.
    - Kan misslyckas om BIOS/UEFI-inställningarna för virtualisering är avstängda.
    - Kan misslyckas om CPU saknar stöd.
    - Kan misslyckas om en annan hypervisor redan använder resurserna.

#### 4.1.1 Övning: Installera QEMU/KVM

- Kontrollera att CPU och moduler stöds:

    ```bash
    egrep -c '(vmx|svm)' /proc/cpuinfo
    lsmod | grep kvm
    ```

- Installera QEMU/KVM:

    ```bash
    sudo apt install qemu-kvm qemu-utils
    ```

- Lägg till din användare i gruppen `kvm`:

    ```bash
    sudo usermod -aG kvm $USER
    ```

- Starta om datorn:

    ```bash
    sudo reboot
    ```

- Kom ihåg att stänga ned VirtualBox innan du använder QEMU/KVM. Båda använder samma hårdvarustöd och kan därför inte köras samtidigt, men det går bra att använda en åt gången.

### 4.2 Nested virtualization

- Gör det möjligt att köra en hypervisor inne i en virtuell maskin (t.ex. för att testa Proxmox eller Hyper-V inuti en VM).
- Kräver att nested virtualization är påslaget i värdsystemet.
- Utan detta blir det bara icke-accelererad emulering, vilket är mycket långsammare.
- Inte nödvändigt för kursen, men intressant att prova i vissa scenarion.

#### 4.2.1 Övning: Kontrollera och aktivera nested virtualization

- Kontrollera vilken CPU du har (Intel eller AMD):

```bash
lscpu | grep 'Vendor ID'
```

- För Intel-CPU:

    ```bash
    cat /sys/module/kvm_intel/parameters/nested
    sudo modprobe -r kvm_intel
    sudo modprobe kvm_intel nested=1
    ```

  - Gör ändringen permanent genom att lägga till en konfigurationsfil, t.ex. `/etc/modprobe.d/kvm-intel.conf` med följande innehåll:

    ```plaintext
    options kvm-intel nested=1
    ```

- För AMD-CPU:

    ```bash
    cat /sys/module/kvm_amd/parameters/nested
    sudo modprobe -r kvm_amd
    sudo modprobe kvm_amd nested=1
    ```

  - Gör ändringen permanent genom att lägga till en konfigurationsfil, t.ex. `/etc/modprobe.d/kvm-amd.conf` med följande innehåll:

    ```plaintext
    options kvm-amd nested=1
    ```

### 4.3 QEMU-kommandoöversikt

- När QEMU installeras följer flera kommandon med på din PATH.
- De som börjar med `qemu-` används för olika syften:
  - `qemu-system-<arkitektur>`: startar en virtuell maskin för en viss CPU-arkitektur (t.ex. `qemu-system-x86_64`, `qemu-system-arm`, `qemu-system-riscv64`).
  - `qemu-img`: skapa, konvertera och hantera diskavbilder.
  - `qemu-io`: testa och felsöka I/O på diskavbilder.
  - `qemu-nbd`: exportera en diskavbild som en NBD-enhet (Network Block Device). Används när man vill montera eller komma åt innehållet i en image som om det vore en fysisk disk.
  - `qemu-pr-helper`: hanterar SCSI Persistent Reservations. Används i avancerade lagringsscenarion där flera servrar delar samma disk och behöver koordinera åtkomst (t.ex. SAN-miljöer).
- I kursen kommer vi bara att använda `qemu-img` och `qemu-system-x86_64` från  ovanstående.

### 4.4 qemu-img

- Används för att skapa, konvertera och inspektera diskavbilder.
- Vanliga diskformat i QEMU:
  - **qcow2**: standardformatet, stöder snapshots, komprimering, kryptering.
  - **raw**: innehåller exakt data som kan kopieras direkt till en fysisk enhet om man vill. Snabbt men utan extra funktioner.
  - **vmdk** (VMware), **vdi** (VirtualBox), **vhd** (Hyper-V): format för kompatibilitet med andra hypervisors.
- Exempel på att skapa en 10 GB qcow2-disk:

    ```bash
    qemu-img create -f qcow2 disk.qcow2 10G
    ```

- Exempel på att inspektera en disk:

    ```bash
    qemu-img info disk.qcow2
    ```

### 4.5 qemu-system-x86_64

- Används för att starta virtuella maskiner på x86_64-arkitekturen.
- Vanliga växlar:
  - `-enable-kvm`: aktiverar hårdvaruacceleration.
  - `-m <MB>`: tilldelar minne.
  - `-cpu host`: använd värd-CPU:n för bästa prestanda.
  - `-drive file=<disk>,format=qcow2`: ansluter en diskavbild.
  - `-cdrom <iso-fil>`: anger en ISO-fil att starta från (t.ex. installationsmedia).
  - `-boot d`: bootar från CD/ISO först.
  - `-vnc :1`: startar VNC-server på display :1 (port 5901). Kan användas för headless VM med fjärranslutning.
  - `-vga <typ>`: anger grafikkort som VM ska ha, t.ex. `-vga virtio` för modernt stöd.

> Notera: Ubuntu Server körs i textläge även under installationen. QEMU emulerar då en enkel standard VGA som fungerar via VNC, så `-vga` är inte nödvändigt i exemplet. För grafiska OS (t.ex. Ubuntu Desktop) rekommenderas dock `-vga virtio` eller annan adapter.

- Exempel: starta en VM med 2 GB RAM, en qcow2-disk och Ubuntu ISO, med VNC på port 5901:

    ```bash
    qemu-system-x86_64 -enable-kvm -m 2048 -cpu host \
        -drive file=disk.qcow2,format=qcow2 \
        -cdrom ubuntu-24.04.3-live-server-amd64.iso -boot d \
        -vnc :1
    ```

#### 4.5.1 Övning: Skapa och installera via VNC

- Installera en VNC-klient:
  - Linux: t.ex. `sudo apt install tigervnc-viewer`
  - Windows: tips – **RealVNC Viewer** eller **TightVNC** fungerar bra.
- Skapa en 70 GB disk:

    ```bash
    qemu-img create -f qcow2 ubuntu.qcow2 70G
    ```

- Starta QEMU för att påbörja installationen:

    ```bash
    qemu-system-x86_64 -enable-kvm -m 2048 -cpu host \
        -drive file=ubuntu.qcow2,format=qcow2 \
        -cdrom ubuntu-24.04.3-live-server-amd64.iso -boot d \
        -vnc :1
    ```

- Anslut till QEMU med din VNC-klient.
- Installera Ubuntu Server:
  - Välj _Guided – use entire disk_.
  - Välj enklaste alternativen.
  - Fortsätt tills installationen är klar.
- När installationen är färdig. Gör Ctrl + C för att avbryta ditt `qemu-system-x86_64`-kommando. Det är inställt på att boota från cdrom:en hela tiden.
- Starta om kommandot på samma vis fast utelämna både `-cdrom` och `-boot`.

## 5. QEMU/KVM och VirtualBox

QEMU/KVM och VirtualBox kan inte köras samtidigt eftersom båda vill använda CPU:ns hårdvarustöd för virtualisering (Intel VT-x/AMD-V). Endast ett program i taget kan använda detta.

### 5.1 Övning: Testa båda hypervisorer

- Starta VirtualBox och verifiera att din tidigare VM (t.ex. Arch Linux) kan startas.
- Stäng ned VirtualBox.
- Starta därefter Ubuntu Server som du installerade i övning 4.5.1 med QEMU/KVM.

### Vanliga problem och lösningar

- **Problem:** KVM-moduler kan inte laddas efter att VirtualBox har körts.
  - **Lösning:** Stäng ned VirtualBox helt och se till att dess kernelmoduler inte är laddade.
- **Problem:** VirtualBox klagar på att VT-x/AMD-V är upptaget.
  - **Lösning:** Avsluta alla QEMU/KVM-processer och försök igen.
- **Problem:** Inget av programmen fungerar efter att man växlat.
  - **Lösning:** Starta om datorn för att frigöra resurserna och ladda om rätt moduler.
