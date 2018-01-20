Wanneer u een gegevensschijf die is gekoppeld aan een virtuele machine (VM) niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Wanneer u een schijf van de virtuele machine loskoppelt, wordt de schijf niet verwijderd uit de opslag. Als u de bestaande gegevens op de schijf opnieuw wilt gebruiken, kunt u de schijf opnieuw koppelen aan dezelfde of een andere virtuele machine.  

> [!NOTE]
> Een VM in Azure gebruikt verschillende soorten schijven: een besturingssysteemschijf, een lokale tijdelijke schijf en optionele gegevensschijven. Zie [Informatie over schijven en VHD's voor virtuele machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie. U kunt de schijf van een besturingssysteem alleen loskoppelen als u ook de virtuele machine verwijdert.

## <a name="find-the-disk"></a>De schijf vinden
Voordat u een schijf van een virtuele machine kunt loskoppelen, moet u achter het LUN komen. Dit is een id voor de schijf die u wilt loskoppelen. Voer hiervoor de volgende stappen uit:

1. Open Azure CLI en [maak verbinding met uw Azure-abonnement](/cli/azure/authenticate-azure-cli). Zorg ervoor dat u zich in de modus voor Azure-servicebeheer (`azure config mode asm`) bevindt.
2. Achterhaal welke schijven zijn gekoppeld aan uw virtuele machine. In het volgende voorbeeld worden schijven weergegeven voor de virtuele machine met de naam `myVM`:

    ```azurecli
    azure vm disk list myVM
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. Noteer het LUN, oftewel het **nummer van de logische eenheid**, voor de schijf die u wilt loskoppelen.

## <a name="remove-operating-system-references-to-the-disk"></a>Verwijzingen van het besturingssysteem naar de schijf verwijderen
Voordat u de schijf loskoppelt van de Linux-gast, moet u ervoor zorgen dat geen van de partities op de schijf in gebruik is. Zorg ervoor dat het besturingssysteem ze na de reboot niet opnieuw probeert te koppelen. Met deze stappen maakt u de configuratie ongedaan die u waarschijnlijk hebt gemaakt tijdens het [koppelen](../articles/virtual-machines/linux/classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) van de schijf.

1. Gebruik de opdracht `lsscsi` om de schijf-id te achterhalen. U kunt `lsscsi` installeren met `yum install lsscsi` (Red Hat-distributies) of `apt-get install lsscsi` (Debian-distributies). De schijf-id kunt u vinden met behulp van het LUN. Het laatste getal in elke rij van de tuple is het LUN. In het volgende voorbeeld van `lsscsi`, is LUN 0 toegewezen aan */dev/sdc*.

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. Gebruik `fdisk -l <disk>` om de partities te detecteren die zijn gekoppeld aan de schijf die u wilt loskoppelen. In het volgende voorbeeld wordt de uitvoer weergegeven voor `/dev/sdc`:

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. Koppel elke partitie van de schijf los. In het volgende voorbeeld wordt `/dev/sdc1` losgekoppeld:

    ```bash
    sudo umount /dev/sdc1
    ```

4. Gebruik de opdracht `blkid` om de UUID's voor alle partities te achterhalen. De uitvoer lijkt op die in het volgende voorbeeld:

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. Verwijder de vermeldingen in het bestand **/etc/fstab** dat is gekoppeld aan de apparaatpaden of UUID's voor alle partities van de schijf die u wilt loskoppelen.  Vermeldingen voor dit voorbeeld kunnen zijn:

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    of
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-the-disk"></a>De schijf loskoppelen
Nadat u het LUN van de schijf hebt gevonden en de verwijzingen naar het besturingssysteem hebt verwijderd, kunt u de schijf loskoppelen:

1. Koppel de geselecteerde schijf los van de virtuele machine met de opdracht `azure vm disk detach
   <virtual-machine-name> <LUN>`. In het volgende voorbeeld wordt LUN `0` losgekoppeld van de virtuele machine met de naam `myVM`:
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. U kunt controleren of de schijf is losgekoppeld door `azure vm disk list` opnieuw uit te voeren. In het volgende voorbeeld wordt de VM met de naam `myVM` gecontroleerd:
   
    ```azurecli
    azure vm disk list myVM
    ```

    De uitvoer is vergelijkbaar met het volgende voorbeeld, waarin wordt aangegeven dat de gegevensschijf niet meer is gekoppeld:

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

De losgekoppelde schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.

