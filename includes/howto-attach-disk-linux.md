
Zie [About Disks and VHDs for Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Informatie over schijven en VHD's voor virtuele machines) voor meer informatie over schijven.

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a>Een lege schijf koppelen
1. Open Azure CLI 1.0 en [maak verbinding met uw Azure-abonnement](/cli/azure/authenticate-azure-cli). Zorg ervoor dat u zich in de modus voor Azure-servicebeheer (`azure config mode asm`) bevindt.
2. Voer `azure vm disk attach-new` in om een nieuwe schijf te maken en te koppelen, zoals wordt weergegeven in het volgende voorbeeld. Vervang *myVM* met de naam van uw virtuele Linux-machine en geef de grootte van de schijf op in GB. In dit voorbeeld is dat *100 GB*:

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. Nadat u de gegevensschijf hebt gemaakt en gekoppeld, wordt deze weergegeven in de uitvoer van `azure vm disk list <virtual-machine-name>`, zoals weergegeven in het volgende voorbeeld:
   
    ```azurecli
    azure vm disk list TestVM
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a>Een bestaande schijf koppelen
Als u een bestaande schijf wilt koppelen, dient u een .vhd beschikbaar te hebben in een opslagaccount.

1. Open Azure CLI 1.0 en [maak verbinding met uw Azure-abonnement](/cli/azure/authenticate-azure-cli). Zorg ervoor dat u zich in de modus voor Azure-servicebeheer (`azure config mode asm`) bevindt.
2. Controleer of de VHD die u wilt toevoegen al is geüpload naar uw Azure-abonnement:
   
    ```azurecli
    azure vm disk list
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. Als u de schijf die u wilt gebruiken niet kunt vinden, kunt u een lokale VHD uploaden naar uw abonnement door `azure vm disk create` of `azure vm disk upload` te gebruiken. `disk create` zou er als volgt uit kunnen zien:
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   U kunt ook `azure vm disk upload` gebruiken om een VHD naar een specifiek opslagaccount te uploaden. [Hier](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) vindt u meer informatie over de opdrachten voor het beheren van de gegevensschijven voor uw virtuele Azure-machine.

4. U kunt nu de gewenste VHD koppelen aan uw virtuele machine:
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   Vervang *myVM* door de naam van uw virtuele machine en *myVHD* door de gewenste VHD.

5. U kunt met `azure vm disk list <virtual-machine-name>` controleren of de schijf is gekoppeld aan de virtuele machine:
   
    ```azurecli
    azure vm disk list myVM
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> Nadat u een gegevensschijf hebt toegevoegd, dient u zich aan te melden bij de virtuele machine en de schijf te initialiseren zodat de virtuele machine de schijf voor opslag kan gebruiken. Zie de volgende stappen voor meer informatie over het initialiseren van de schijf.
> 
> 

