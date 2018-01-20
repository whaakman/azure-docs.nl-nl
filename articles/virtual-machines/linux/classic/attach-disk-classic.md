---
title: Een schijf koppelen aan een Linux-VM in Azure | Microsoft Docs
description: Informatie over het een gegevensschijf koppelen aan een Linux-VM met het klassieke implementatiemodel en de schijf initialiseren zodat deze gereed voor gebruik is
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: b47451bb8e53154760b893e452649f15ffdff7f9
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Hoe een gegevensschijf koppelen aan een virtuele Linux-Machine
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie hoe [een gegevensschijf met het implementatiemodel van Resource Manager koppelen](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

U kunt zowel leeg schijven en schijven met gegevens naar uw Azure VM's kunt koppelen. Beide typen schijven zijn VHD-bestanden die zich in een Azure storage-account bevinden. Als met een schijf toevoegen aan een Linux-machine, hoeft nadat u de schijf koppelen te initialiseren en formatteren zodat deze gereed voor gebruik is. Dit artikel gegevens zowel leeg schijven en schijven met al gegevens naar uw virtuele machines, evenals hoe u kunt vervolgens initialiseren en formatteren van een nieuwe schijf koppelen.

> [!NOTE]
> Het is een best practice om een of meer afzonderlijke schijven te bewaren van gegevens van een virtuele machine. Wanneer u een virtuele machine in Azure maakt, heeft deze een besturingssysteem en een tijdelijke schijf. **Gebruik de tijdelijke schijf geen permanente gegevens opslaan.** Zoals de naam al aangeeft, biedt deze alleen tijdelijke opslag. Biedt geen redundantie of back-up omdat deze zich niet in Azure-opslag.
> De tijdelijke schijf is gewoonlijk beheerd door de Azure Linux Agent en automatisch gekoppeld aan **mnt/resourcegroep** (of **mnt** Ubuntu afbeeldingen). Aan de andere kant een gegevensschijf kan de naam van de kernel Linux ongeveer `/dev/sdc`, en u moet partitie formatteren, en koppel deze resource. Zie de [gebruikershandleiding voor Azure Linux Agent] [ Agent] voor meer informatie.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Initialiseer de gegevensschijf van een nieuwe in Linux
1. SSH met uw virtuele machine. Zie voor meer informatie [aanmelden met een virtuele machine met Linux][Logon].
2. Vervolgens moet u de apparaat-id voor de gegevensschijf initialiseren vinden. Er zijn twee manieren doen:
   
    een) Grep voor SCSI-apparaten in de logboeken, zoals in de volgende opdracht:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Recente Ubuntu distributies, moet u wellicht gebruiken `sudo grep SCSI /var/log/syslog` omdat registratie in `/var/log/messages` standaard mogelijk uitgeschakeld.
   
    U vindt de id van de laatste gegevensschijf die is toegevoegd in de berichten die worden weergegeven.
   
    ![De berichten van de schijf ophalen](./media/attach-disk/scsidisklog.png)
   
    OF
   
    b) Gebruik de `lsscsi` opdracht om erachter te komen de apparaat-id. `lsscsi` kan worden geïnstalleerd door een van beide `yum install lsscsi` (gebaseerd op Red Hat distributies) of `apt-get install lsscsi` (gebaseerd op Debian distributies). U vindt de schijf die u zoekt door de *lun* of **nummer van de logische eenheid**. Bijvoorbeeld, de *lun* voor de schijven die door u bijgevoegde gemakkelijk kunnen worden gezien vanaf `azure vm disk list <virtual-machine>` als:

    ```azurecli
    azure vm disk list myVM
    ```

    De uitvoer lijkt op het volgende:

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    Deze gegevens met de uitvoer van vergelijken `lsscsi` voor dezelfde virtuele machine steekproef:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    Het laatste aantal in de tuple in elke rij is de *lun*. Zie `man lsscsi` voor meer informatie.
3. Typ de volgende opdracht om uw apparaat te maken bij de opdrachtprompt:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. Wanneer u wordt gevraagd, typt u  **n**  een partitie maken.

    ![Apparaat maken](./media/attach-disk/fdisknewpartition.png)

5. Wanneer u wordt gevraagd, typt u **p** om te maken van de partitie de primaire partitie. Type **1** zodat het de eerste partitie en typ vervolgens invoeren voor het accepteren van de standaardwaarde voor de cilinder. Op sommige systemen kan deze de standaardwaarden van de eerste en de laatste sectoren, in plaats van de cilinder weergeven. U kunt deze standaardinstellingen accepteren.

    ![Partitie maken](./media/attach-disk/fdisknewpartdetails.png)


6. Type **p** om de details over de schijf die wordt opgedeeld te bekijken.

    ![Schijfgegevens lijst](./media/attach-disk/fdiskpartitiondetails.png)


7. Type **w** de instellingen voor de schijf te schrijven.

    ![De schijfwijzigingen geschreven](./media/attach-disk/fdiskwritedisk.png)

8. Nu kunt u het bestandssysteem op de nieuwe partitie maken. Het partitienummer toevoegen aan de apparaat-ID (in het volgende voorbeeld `/dev/sdc1`). Het volgende voorbeeld wordt een partitie ext4 op /dev/sdc1:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Bestandssysteem maken](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > SuSE Linux Enterprise 11 alleen ondersteuning voor alleen-lezentoegang voor ext4-bestandssystemen. Voor deze systemen, is het aanbevolen om het nieuwe bestandssysteem als ext3 in plaats van ext4.

9. Moet een directory koppelen van het nieuwe bestandssysteem als volgt:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Ten slotte kunt u het station als volgt koppelen:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    De gegevensschijf is nu gereed voor gebruik als **/datadrive**.
   
    ![Maken van de map en de schijf koppelen](./media/attach-disk/mkdirandmount.png)

11. Het nieuwe station aan /etc/fstab toevoegen:
   
    Om te controleren of dat de schijf automatisch opnieuw wordt gekoppeld na opnieuw opstarten moet deze worden toegevoegd aan het bestand/etc/fstab-fouten. Het is ook raadzaam een dat de UUID (Universally Unique IDentifier) wordt gebruikt in /etc/fstab om te verwijzen naar het station in plaats van alleen de naam van het apparaat (dat wil zeggen /dev/sdc1). Met behulp van de UUID voorkomt de onjuiste schijf wordt gekoppeld aan een bepaalde locatie als het besturingssysteem detecteert een schijffout tijdens het opstarten en eventuele resterende gegevensschijven vervolgens deze apparaat-id's worden toegewezen. Als u de UUID van het nieuwe station zoekt, kunt u de **blkid** hulpprogramma:
   
    ```bash
    sudo -i blkid
    ```
   
    De uitvoer lijkt op het volgende voorbeeld:
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > Onjuist bewerken van de **/etc/fstab** bestand kan leiden tot een systeem opgestart. Als u niet zeker, Raadpleeg de distributie-documentatie voor informatie over het correct dit bestand te bewerken. Het is ook raadzaam dat een back-up van het bestand /etc/fstab is gemaakt voordat u bewerkt.

    Open vervolgens de **/etc/fstab** bestand in een teksteditor:

    ```bash
    sudo vi /etc/fstab
    ```

    In dit voorbeeld gebruiken we de UUID-waarde voor de nieuwe **/dev/sdc1** apparaat dat is gemaakt in de vorige stappen en het koppelpunt **/datadrive**. Voeg de volgende regel toe aan het einde van de **/etc/fstab** bestand:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Of op systemen die op basis van SuSE Linux moet u wellicht een iets andere notatie:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > De `nofail` optie zorgt ervoor dat de VM start zelfs als het bestandssysteem beschadigd is of de schijf tijdens het opstarten niet bestaat. Zonder deze optie kunnen optreden gedrag zoals beschreven in [niet kan SSH voor Linux VM vanwege FSTAB-fouten](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Nu kunt u testen of het bestandssysteem is gekoppeld goed door ontkoppelen en opnieuw opstellen van het bestandssysteem, dat wil zeggen met behulp van het voorbeeld koppelpunt `/datadrive` gemaakt in de eerdere stappen:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Als de `mount` opdracht, treedt een fout, controleert u het bestand/etc/fstab-fouten voor de juiste syntaxis. Als extra gegevensstations of partities worden gemaakt, / etc/fstab ook afzonderlijk aangaan.

    Het station beschrijfbare maken door met de volgende opdracht:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Een gegevensschijf vervolgens verwijderen zonder te bewerken fstab kan ervoor zorgen dat de virtuele machine niet kunnen worden opgestart. Als dit vaak het geval is, bieden de meeste distributies ofwel de `nofail` en/of `nobootwait` opstarttijd fstab-opties waarmee u een systeem op te starten, zelfs als de schijf niet koppelen aan. Raadpleeg de distributie-documentatie voor meer informatie over deze parameters.

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/UNMAP ondersteuning voor Linux in Azure
Sommige kernels Linux ondersteuning TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze bewerkingen zijn voornamelijk nuttig in standard-opslag om te informeren over Azure die verwijderde pagina's zijn niet langer geldig en kan worden verwijderd. 'S te verwijderen kunt kosten besparen, als u grote bestanden maken en deze vervolgens te verwijderen.

Er zijn twee manieren om in te schakelen TRIM ondersteunen in uw Linux-VM. Raadpleeg uw distributiepunt gebruikelijke voor de aanbevolen aanpak:

* Gebruik de `discard` koppelen optie in `/etc/fstab`, bijvoorbeeld:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* In sommige gevallen de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` opdracht handmatig vanaf de opdrachtregel of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Problemen oplossen
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het gebruik van uw Linux-VM in de volgende artikelen:

* [Aanmelden met een virtuele machine waarop Linux wordt uitgevoerd][Logon]
* [Het ontkoppelen van een schijf van een virtuele Linux-machine](detach-disk-classic.md)
* [De Azure CLI gebruiken met het klassieke implementatiemodel](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Configureren van RAID op een virtuele Linux-machine in Azure](../configure-raid.md)
* [LVM configureren op een virtuele Linux-machine in Azure](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
