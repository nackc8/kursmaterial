# Övningsuppgifter – Volymhantering (GPT, LVM, Btrfs)

## Förberedelser

### Virtuell maskin med libvirt

- Ladda ned **Fedora Server ISO** (inte qcow2). Målet är att få erfarenhet av en distribution som ligger nära **Red Hat Enterprise Linux (RHEL)** i upplägg och verktyg. ([docs.fedoraproject.org](https://docs.fedoraproject.org/en-US/fedora-server/latest/installation/))
- Skapa VM:n med **virt-install** (på hosten) för att träna libvirt-flödet.

  - När VM:n är skapad: starta den och anslut med **virt-manager** för installation och konsol.
- Resurser för VM:n:

  - **Disk:** 40 GB
  - **Minne:** 5 GB
  - **Firmware:** UEFI (OVMF)
- Partitionering vid installation:

  - Välj **automatisk/standard** partitionering och **bekräfta** att den landar i en layout med **LVM**.
  - Obs: Om din ISO/installer-UI inte föreslår LVM automatiskt (kan variera mellan versioner/UI), välj LVM-layout i storage/partitioning-steget eller gör motsvarande manuellt.

#### SSH (innan labbarna)

- Efter installation: gör det möjligt att ansluta med SSH och **testa** anslutningen.

  - Se till att `sshd` är installerad och aktiverad, och att brandvägg tillåter SSH.
- Gör det möjligt att ansluta med **nyckelfil**:

  - Skapa nyckel på klienten (om du inte redan har): `ssh-keygen`
  - Lägg till publik nyckel i VM:n (t.ex. med `ssh-copy-id user@vm` eller via `~/.ssh/authorized_keys`).
  - Testa: `ssh user@vm`

#### Första backup av boot-disken

- När SSH fungerar: **stäng ned VM:n** och ta en första offline-backup av boot-diskens image på hosten med `cp`.

  - Backupen ska få filändelsen **.bak**.
  - Exempel: `cp /var/lib/libvirt/images/<vm>.qcow2 /var/lib/libvirt/images/<vm>.qcow2.bak`

### Säkerhet och återställning

- **Varning:** Flera moment är medvetet destruktiva.
- Var noga med att köra destruktiva kommandon (t.ex. `dd`, `parted`, `sgdisk`) **inne i VM:n** och inte på host-datorn.
- För **GPT-uppgifterna (1-2)** jobbar du på **VM:ens boot-disk** (den som innehåller OS/ESP), så att firmware faktiskt behöver läsa GPT under boot.
- Innan du förstör något: se till att du har gjort en **offline-backup** av diskimagen (VM:n avstängd) och att du vet hur du återställer den snabbt.

## Förkunskaper, konventioner och referenser

- Byt alltid ut platshållare som `<DISK>` och `<PART>` mot dina riktiga enheter.
- **GPT-uppgifter:** använd `<BOOT_DISK>` = VM:ens **OS/boot-disk** (där ESP och root ligger).
- **LVM/Btrfs-uppgifter:** använd separata labbdiskar (t.ex. `<LVM_DISK>`, `<BTRFS_DISK>`).
- I qemu/libvirt kan diskar i gästen dyka upp som **/dev/vdX** (virtio-blk) eller **/dev/sdX** (SCSI/SATA/virtio-scsi). Utgå alltid från `lsblk` i stället för att gissa namnet.
- Kontrollera alltid att du jobbar på rätt disk med:

  - `lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS`
  - `blkid`
- I uppgifter där du skriver direkt till disk (t.ex. med `dd`), gör en extra kontroll: **storlek + modell + att det verkligen är rätt disk**.

### Externa länkar att läsa

Begreppen kan läsas **översiktligt**. Du behöver t.ex. inte kunna räkna ut **CRC32** för hand. Det viktiga är att förstå varför checksummor finns och hur verktyg använder dem.

- **Arch Wiki**: LVM, Btrfs, Partitioning, fdisk (util-linux), gdisk (GPT fdisk), sgdisk
- **Wikipedia** (begrepp): GPT, UEFI, LBA, CRC32, Copy-on-write, Snapshot
- **Btrfs-projektets dokumentation** (status och what to use): Btrfs Status och `btrfs(5)`

## Uppgifter

### Uppgift 1: GPT primary korruption och fallback

Förstå validering (CRC), fallback till backup GPT och att auto repair kan utebli.

1. Identifiera VM:ens **boot-disk** som `<BOOT_DISK>` (den disk som innehåller OS och normalt även EFI System Partition).

   - Tips: `lsblk -f` och titta på var `/` och ev. `/boot` eller `/efi` ligger.
   - `<BOOT_DISK>` kan i gästen heta t.ex. `/dev/vda` eller `/dev/sda` beroende på buss.
2. Verifiera att du har en **offline-backup** av diskimagen på hosten enligt Förberedelser (VM avstängd när du tar backup).
3. Inspektera befintlig GPT på `<BOOT_DISK>` med ett verktyg (rekommenderat: `gdisk` eller `sgdisk`). Notera att det finns en **primär** tabell (vid LBA 1) och en **backup** (i slutet av disken).
4. Skada medvetet **Primary GPT Header** genom att skriva ASCII-texten `OHNO` på LBA 1:

   - Ta reda på sektorstorlek: `SS=$(blockdev --getss <BOOT_DISK>)`
   - OBS: Använd helst `blockdev --getsize64` för att räkna sektorer i samma enheter som `bs`, se exempel nedan.
   - Exempel (säkrare beräkning för LBA-index):

     ```bash
     SS=$(blockdev --getss <BOOT_DISK>)               # bytes per sector (t.ex. 512 eller 4096)
     BYTES=$(blockdev --getsize64 <BOOT_DISK>)       # totalstorlek i bytes
     SECTORS=$((BYTES / SS))                         # antal sektorer i SS-storlek
     # skriv till LBA1
     echo -n OHNO | dd of=<BOOT_DISK> bs=$SS seek=1 conv=notrunc
     # för att skriva till sista LBA: seek=$((SECTORS-1))
     ```

   - Skriv till LBA 1: `echo -n OHNO | dd of=<BOOT_DISK> bs=$SS seek=1 conv=notrunc`
5. Verifiera att `OHNO` finns på rätt ställe:

   - `dd if=<BOOT_DISK> bs=$SS skip=1 count=1 2>/dev/null | hexdump -C | head`
6. Försök nu att lista partitioner igen och kontrollera om systemet fortfarande hittar dem.
7. Dokumentera vad som händer:

   - Kan OS fortfarande hitta partitionerna?
   - Har Primary-headern blivit fixad automatiskt, eller är den fortfarande trasig?
8. Reparera Primary från backup med ett OS-verktyg (t.ex. `gdisk` recovery-funktion eller `sgdisk` enligt Arch Wiki).
9. Verifiera efteråt att GPT är frisk (t.ex. med `sgdisk -v <BOOT_DISK>`).

Förväntad lärdom: Du kan ofta fortsätta använda disken via backup GPT, men det är inte garanterat att firmware eller boot automatiskt skriver tillbaka en reparerad GPT.

### Uppgift 2: GPT återställning från backupfil

Kunna säkerhetskopiera och återställa GPT när ingen giltig GPT finns.

Viktigt: När du förstör både primary och backup GPT på `<BOOT_DISK>` kan systemet bli obootbart. Planera därför att boota från en ISO (live eller rescue) för att köra `sgdisk` och återställa.

1. Utgå från en **frisk** GPT-setup på `<BOOT_DISK>` (ingen `OHNO` i LBA 1 eller sista LBA).
2. Skapa en backupfil av GPT och placera den **utanför `<BOOT_DISK>`**:

   - `sgdisk --backup=gpt_backup.img <BOOT_DISK>`
   - Spara filen på en annan virtuell disk (t.ex. en separat backup-disk i VM), eller kopiera ut den till hosten (t.ex. via `scp`).
   - Att spara den på en filsystemvolym som ligger på samma disk hjälper inte om du förstör GPT på just den disken.
3. Skada Primary GPT Header (som i uppgift 1).
4. Skada även Backup GPT Header (sista LBA):

   - Använd säker beräkning som använder samma sektorstorlek som `bs`:

     ```bash
     SS=$(blockdev --getss <BOOT_DISK>)               # bytes per sektor
     BYTES=$(blockdev --getsize64 <BOOT_DISK>)       # total bytes
     SECTORS=$((BYTES / SS))                         # antal sektorer i SS-storlek
     echo -n OHNO | dd of=<BOOT_DISK> bs=$SS seek=$((SECTORS-1)) conv=notrunc
     ```

   - Obs: `blockdev --getsz` kan vara förvillande eftersom den ofta returnerar antal 512‑bytes-sektorer; undvik den när du använder `bs=$SS`.
5. Visa att partitionerna nu inte går att hitta på normalt sätt.
6. Boota från ISO och återställ från backupfilen:

   - `sgdisk --load-backup=gpt_backup.img <BOOT_DISK>`
7. Verifiera:

   - att partitionerna syns igen
   - att systemet kan boota normalt efter återställning

### Uppgift 3: LVM skapa PV VG LV och montera

Bygg en vardags LVM med två LV och pålitliga mounts.

1. Lägg till en tom virtuell disk `<LVM_DISK>` (t.ex. `/dev/vdc` eller `/dev/sdc`, 6 till 10 GiB).
2. Skapa en partition som täcker nästan hela disken och markera den som LVM.

   - Med parted: sätt lvm-flag (moderna LVM-verktyg fungerar även utan, men det är god praxis)
   - Med gdisk: partition type 8e00
3. Skapa LVM:

   - PV på `<LVM_DISK>1`
   - VG: `vg_data`
   - LV: `lv_data1` (2 GiB), `lv_data2` (2 GiB)
4. Formatera båda som ext4.
5. Skapa mountpoints `/mnt/data1` och `/mnt/data2`.
6. Lägg in i `/etc/fstab` så att de monteras vid boot.

   - Tips: använd `UUID=` via `blkid` eller `/dev/mapper/vg_data-lv_data1`.
7. Verifiera med `mount -a`, `findmnt`, `df -h` och lägg en testfil på varje volym.

### Uppgift 4: LVM väx partition PV och filsystem

Gör den vanliga kedjan när disk blir större.

Förutsättning: du har `vg_data` och `lv_data1` från uppgift 3.

1. Stäng av VM:n och öka storleken på disken som bär `<LVM_DISK>` (t.ex. plus 2 GiB) i libvirt eller qemu.
2. Starta VM:n och verifiera att disken är större (`lsblk`).
3. Väx partitionen `<LVM_DISK>1` så att den använder det nya utrymmet.

   - Gör detta med ett partitioneringsverktyg som kan utöka en befintlig partition. Ett enkelt verktyg för detta är `growpart` (från paketet `cloud-guest-utils` i Fedora): `growpart <LVM_DISK> 1`. Alternativt kan du använda `parted resizepart`.
   - Efter resize: kör `partprobe <LVM_DISK>` eller `partx -u <LVM_DISK>` om kernel inte upptäcker ändringen. Om detta inte hjälper, starta om VM:n innan `pvresize`.
4. Kör `pvresize <LVM_DISK>1`.
5. Utöka `lv_data1` med plus 1 GiB: `lvextend -L +1G /dev/vg_data/lv_data1`.
6. Väx ext4-filsystemet på `lv_data1`: `resize2fs /dev/vg_data/lv_data1`.
7. Verifiera nya storleken med `lvs` och `df -h /mnt/data1`.

### Uppgift 5: LVM snapshot och rollback

Ta snapshot, gör en dålig ändring och rulla tillbaka.

1. Skriv en testfil i `/mnt/data1/fil.txt`.
2. Skapa en snapshot av `lv_data1` på 500 MiB:

   - `lvcreate -L 500M -s -n lv_data1_snap /dev/vg_data/lv_data1`
3. Gör en tydlig ändring i `/mnt/data1/fil.txt`.
4. Förbered rollback:

   - Avmontera `/mnt/data1`.
   - Kör `lvconvert --merge /dev/vg_data/lv_data1_snap`.
   - Notera: `lvconvert --merge` kräver ofta att origin‑volymen är inaktiv för att merge ska köras; om den inte startar direkt, inaktivera volymen (t.ex. `lvchange -an /dev/vg_data/lv_data1`) eller starta om systemet för att påskynda merge.
5. Aktivera merge (ofta enklast genom att boota om).
6. Montera `/mnt/data1` och verifiera att filen är återställd.

Notera: Snapshot-merge kan kräva att volymen inte är aktiv eller monterad. Följ Arch Wiki och verktygets output.

### Uppgift 6: Btrfs status subvolymer och fstab

Skapa en modern Btrfs layout och läs rekommendationer.

1. Läs Btrfs-projektets Status samt `btrfs(5)`.

   - Skriv ner tre korta punkter: vad ni bör undvika (t.ex. vissa RAID-profiler), vad som är ok, och en varning som är viktig i drift.
2. Lägg till en tom virtuell disk `<BTRFS_DISK>` (t.ex. `/dev/vdd` eller `/dev/sdd`, 6 till 10 GiB) och skapa ett Btrfs-filsystem på den.
3. Montera Btrfs toppnivån på t.ex. `/mnt/btrfsroot`.
4. Skapa subvolymer:

   - `@` (root-liknande data)
   - `@data` (för labbdata)
   - `@snapshots` (för snapshots)
5. Skapa mountpoints `/mnt/btrfs-data` och `/mnt/btrfs-snapshots`.
6. Lägg in i `/etc/fstab` så att du monterar:

   - `subvol=@data` till `/mnt/btrfs-data`
   - `subvol=@snapshots` till `/mnt/btrfs-snapshots`
7. Verifiera med `btrfs subvolume list`, `findmnt` och `btrfs filesystem df`.

### Uppgift 7: Btrfs snapshots och återställning

Gör rollback på subvolymnivå.

Förutsättning: du har `@data` monterad på `/mnt/btrfs-data`.

1. Skapa en testfil: `echo före > /mnt/btrfs-data/fil.txt`.
2. Ta en snapshot av `@data` till `@snapshots/data_1`:

   - Skapa snapshoten som read-only om du vill träna säkrare snapshots.
3. Ändra filen: `echo efter > /mnt/btrfs-data/fil.txt`.
4. Återställ genom att:

   - avmontera `/mnt/btrfs-data`
   - ersätta eller återskapa `@data` från snapshoten (enligt Arch Wiki)
5. Montera igen och verifiera att filen innehåller `före`.

### Uppgift 8: Btrfs quota för subvolym

Sätt en praktisk gräns och förstå shared vs exclusive.

1. Aktivera quota på filsystemet (se `btrfs quota` och `btrfs qgroup` samt Arch Wiki):

   - `btrfs quota enable /mnt/btrfsroot` (eller valfri mountpoint för samma filsystem)
2. Kontrollera qgroup-status och lista qgroups.
3. Kontrollera qgroup‑id och attraktiv väg: `btrfs qgroup show -reF /mnt/btrfsroot`. Sätt en limit på `@data` (t.ex. 200 MiB) med exempel:

   - `btrfs qgroup limit 200M /mnt/btrfsroot/@data`  # eller använd qgroup‑id som visas av qgroup show

4. Försök skriva data som går över gränsen och notera beteendet.
5. Visa skillnaden mellan `shared` och `exclusive` i `btrfs qgroup show`-outputen. Gör så här:
   a. Notera qgroup-användningen för `@data`.
   b. Skapa en snapshot av `@data`.
   c. Skriv ny, unik data till `@data` (originalet).
   d. Kör `btrfs qgroup show -reF /mnt/btrfsroot` igen och analysera hur `shared` och `exclusive` har förändrats för `@data` och den nya snapshoten. `shared` visar data som delas mellan dem (data som fanns före snapshot), medan `exclusive` visar data som är unik för just den subvolymen.

Notera: Quotas kan ge prestandapåverkan. Använd dem när du faktiskt behöver dem.

## Lösningsförslag

Nedan följer ett sätt att lösa uppgifterna. Det finns oftast flera korrekta och likvärdiga metoder.

För mer information och alternativ, konsultera respektive kommandos manualsidor (man och info), eller sök efter artiklar, YouTube-klipp och sidor på Arch Wiki.

### Uppgift 1: GPT primary korruption och fallback

```bash
# Utgår från att <BOOT_DISK> redan innehåller OS och GPT.

# Inspektera GPT
sgdisk -p <BOOT_DISK>

# Skada LBA1 (säker metod för sektorer)
SS=$(blockdev --getss <BOOT_DISK>)               # bytes per sector
BYTES=$(blockdev --getsize64 <BOOT_DISK>)       # total bytes
SECTORS=$((BYTES / SS))                         # antal sektorer i SS-storlek
echo -n OHNO | dd of=<BOOT_DISK> bs=$SS seek=1 conv=notrunc

# Visa att OHNO ligger i LBA1
dd if=<BOOT_DISK> bs=$SS skip=1 count=1 2>/dev/null | hexdump -C | head

# Verifiera/inspektera
sgdisk -v <BOOT_DISK>

# Reparera med gdisk (exempel)
# Starta: gdisk <BOOT_DISK>
# I gdisk: tryck r (recovery & transformation)
# Sedan: b (use backup GPT header to rebuild primary)
# Avsluta: w (write changes)
# Läs instruktionerna i verktyget noga innan du skriver.
```

### Uppgift 2: GPT återställning från backupfil

```bash
# Skapa backup (kör innan du förstör något)
sgdisk --backup=gpt_backup.img <BOOT_DISK>
# Kopiera ut gpt_backup.img till annan disk eller host.

# Skada primary (LBA1)
SS=$(blockdev --getss <BOOT_DISK>)
BYTES=$(blockdev --getsize64 <BOOT_DISK>)
SECTORS=$((BYTES / SS))
echo -n OHNO | dd of=<BOOT_DISK> bs=$SS seek=1 conv=notrunc

# Skada backup header (sista LBA)
echo -n OHNO | dd of=<BOOT_DISK> bs=$SS seek=$((SECTORS-1)) conv=notrunc

# Boota från ISO/live och återställ
sgdisk --load-backup=gpt_backup.img <BOOT_DISK>
sgdisk -v <BOOT_DISK>
```

### Uppgift 3: LVM skapa PV VG LV och montera

```bash
# Partitionera disk för LVM
parted -s <LVM_DISK> mklabel gpt mkpart primary 1MiB 100%
parted -s <LVM_DISK> set 1 lvm on

# Skapa PV/VG/LV
pvcreate <LVM_DISK>1
vgcreate vg_data <LVM_DISK>1
lvcreate -L 2G -n lv_data1 vg_data
lvcreate -L 2G -n lv_data2 vg_data

mkfs.ext4 -F /dev/vg_data/lv_data1
mkfs.ext4 -F /dev/vg_data/lv_data2

mkdir -p /mnt/data1 /mnt/data2

# UUID i fstab
UUID1=$(blkid -s UUID -o value /dev/vg_data/lv_data1)
UUID2=$(blkid -s UUID -o value /dev/vg_data/lv_data2)

echo "UUID=$UUID1 /mnt/data1 ext4 defaults 0 2" | tee -a /etc/fstab
echo "UUID=$UUID2 /mnt/data2 ext4 defaults 0 2" | tee -a /etc/fstab

mount -a

echo "data på lv_data1" > /mnt/data1/fil.txt
echo "data på lv_data2" > /mnt/data2/fil.txt
```

### Uppgift 4: LVM väx partition PV och filsystem

```bash
lsblk

# Väx partitionen (exempel med parted resizepart)
parted <LVM_DISK> resizepart 1 100%
# bekräfta att kernel ser förändringen
partprobe <LVM_DISK> || partx -u <LVM_DISK> || true

pvresize <LVM_DISK>1

lvextend -L +1G /dev/vg_data/lv_data1
resize2fs /dev/vg_data/lv_data1

lvs
df -h /mnt/data1
```

### Uppgift 5: LVM snapshot och rollback

```bash
# Skapa initial fil med känt innehåll
echo FÖRE > /mnt/data1/fil.txt
cat /mnt/data1/fil.txt

lvcreate -L 500M -s -n lv_data1_snap /dev/vg_data/lv_data1
lvs

echo EFTER > /mnt/data1/fil.txt
cat /mnt/data1/fil.txt

umount /mnt/data1
# Starta merge. Kommandot kan avslutas direkt om volymen är i bruk;
# mergen schemaläggs då till nästa omstart eller inaktivering.
lvconvert --merge /dev/vg_data/lv_data1_snap || true
# Om merge inte startar: inaktivera origin (`lvchange -an /dev/vg_data/lv_data1`) eller reboot
reboot

mount /mnt/data1
cat /mnt/data1/fil.txt  # Bör visa: FÖRE
```

### Uppgift 6: Btrfs status subvolymer och fstab

```bash
mkfs.btrfs -f <BTRFS_DISK>

mkdir -p /mnt/btrfsroot
mount <BTRFS_DISK> /mnt/btrfsroot

btrfs subvolume create /mnt/btrfsroot/@
btrfs subvolume create /mnt/btrfsroot/@data
btrfs subvolume create /mnt/btrfsroot/@snapshots

mkdir -p /mnt/btrfs-data /mnt/btrfs-snapshots

UUID=$(blkid -s UUID -o value <BTRFS_DISK>)
cp /etc/fstab /etc/fstab.bak

echo "UUID=$UUID /mnt/btrfs-data      btrfs defaults,subvol=@data      0 0" | tee -a /etc/fstab
echo "UUID=$UUID /mnt/btrfs-snapshots btrfs defaults,subvol=@snapshots 0 0" | tee -a /etc/fstab

mount -a
btrfs subvolume list /mnt/btrfsroot
findmnt | grep btrfs
btrfs filesystem df /mnt/btrfsroot
```

### Uppgift 7: Btrfs snapshots och återställning

```bash
echo före > /mnt/btrfs-data/fil.txt

mkdir -p /mnt/btrfs-snapshots
btrfs subvolume snapshot -r /mnt/btrfs-data /mnt/btrfs-snapshots/data_1

echo efter > /mnt/btrfs-data/fil.txt

umount /mnt/btrfs-data
mount <BTRFS_DISK> /mnt/btrfsroot

# Metod 1: Ta bort och återskapa (destruktivt).
# btrfs subvolume delete /mnt/btrfsroot/@data
# btrfs subvolume snapshot /mnt/btrfsroot/@snapshots/data_1 /mnt/btrfsroot/@data

# Metod 2: Flytta den gamla och ersätt med snapshot (säkrare).
mv /mnt/btrfsroot/@data /mnt/btrfsroot/@data_old
btrfs subvolume snapshot /mnt/btrfsroot/@snapshots/data_1 /mnt/btrfsroot/@data

mount /mnt/btrfs-data
cat /mnt/btrfs-data/fil.txt
```

### Uppgift 8: Btrfs quota för subvolym

```bash
btrfs quota enable /mnt/btrfsroot

btrfs quota status /mnt/btrfsroot
btrfs qgroup show -reF /mnt/btrfsroot

# Sätt limit på subvolume via dess path eller qgroup-id
btrfs qgroup limit 200M /mnt/btrfsroot/@data  # eller använd den qgroup-id som qgroup show visar

# Test (exempel)
# dd if=/dev/zero of=/mnt/btrfs-data/bigfile bs=1M count=300

btrfs qgroup show -reF /mnt/btrfsroot
```
