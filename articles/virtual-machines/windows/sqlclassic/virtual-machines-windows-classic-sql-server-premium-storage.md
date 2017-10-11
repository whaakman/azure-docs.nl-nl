---
title: Gebruik Azure Premium-opslag met SQL Server | Microsoft Docs
description: Dit artikel maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel en biedt richtlijnen over het gebruik van Azure Premium-opslag met SQL Server wordt uitgevoerd op Azure Virtual Machines.
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: jroth
ms.openlocfilehash: 6790db207fc7ec8a4b1546ef07c97ef30abe9513
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Azure Premium Storage gebruiken met SQL Server op Virtual Machines
## <a name="overview"></a>Overzicht
[Azure Premium-opslag](../../../storage/common/storage-premium-storage.md) is de volgende generatie van opslag die een lage latentie hoge doorvoersnelheid IO en. Het meest geschikt is voor belangrijke IO intensieve werkbelastingen, zoals SQL Server op IaaS [virtuele Machines](https://azure.microsoft.com/services/virtual-machines/).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Dit artikel bevat plannen en richtlijnen voor het migreren van een virtuele Machine met SQL Server met Premium-opslag. Dit omvat de Azure-infrastructuur (netwerk-, opslag) en Gast Windows VM stappen. Het voorbeeld in de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) ziet u een volledige uitgebreide end-to-end-migratie van het verplaatsen van grotere virtuele machines om te profiteren van betere lokale SSD-opslag met PowerShell.

Het is belangrijk te weten met Azure Premium-opslag met SQL Server op IAAS VM's van het end-to-end-proces. Dit omvat:

* Identificatie van de vereisten voor het gebruik van Premium-opslag.
* Voorbeelden van de implementatie van SQL Server op IaaS naar Premium-opslag voor nieuwe implementaties.
* Voorbeelden van migreren bestaande implementaties, zowel zelfstandige servers en implementaties met behulp van SQL AlwaysOn-beschikbaarheidsgroepen.
* Mogelijke migratie nadert.
* Volledige end-to-end-voorbeeld is van de stapsgewijze instructies voor de migratie van een bestaande implementatie altijd op Azure, Windows en SQL Server.

Zie voor meer achtergrondinformatie op SQL Server in Azure Virtual Machines, [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

**Auteur:** Daniel Sol **technische controleurs:** Luis Jeroen Vargas haring, Sanjay Mishra, Pravin Mital, Schwertl Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Vereisten voor de Premium-opslag
Er zijn verschillende vereisten voor het gebruik van Premium-opslag.

### <a name="machine-size"></a>Grootte van de machine
U moet DS-serie virtuele Machines (VM) gebruiken voor het gebruik van Premium-opslag. Als u DS-serie machines niet in uw cloudservice voordat u hebt gebruikt, moet u de bestaande virtuele machine verwijderen, houden van de gekoppelde schijven en vervolgens een nieuwe cloudservice maken vóór het opnieuw maken van de virtuele machine als de grootte van de rol DS *. Zie voor meer informatie over grootten van virtuele machines [virtuele Machine en Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="cloud-services"></a>Cloud services
U kunt alleen virtuele machines DS * met Premium-opslag gebruiken wanneer ze worden gemaakt in een nieuwe cloudservice. Als u SQL Server Always On in Azure, wordt het altijd-Listener voor verwijzen naar het Azure interne of externe Load Balancer-IP-adres dat is gekoppeld aan een cloudservice. Dit artikel is gericht op het migreren van behoud van beschikbaarheid in dit scenario.

> [!NOTE]
> Een reeks DS * moet de eerste virtuele machine die wordt geïmplementeerd naar de nieuwe Service in de Cloud.
>
>

### <a name="regional-vnets"></a>Regionaal vnet 's
U moet het virtuele netwerk (VNET) die als host fungeert voor uw virtuele machines worden regionale configureren voor virtuele machines DS *. Dit 'genereren' in het VNET om toe te staan de grotere virtuele machines kunnen worden ingericht in andere clusters en toestaan dat communicatie ertussen. In de volgende schermafbeelding ziet u de gemarkeerde locatie regionaal vnet's, terwijl het eerste resultaat een 'smalle' VNET toont.

![RegionalVNET][1]

U kunt een Microsoft-ondersteuningsticket om te migreren naar een regionaal VNET verhogen, Microsoft wordt een wijziging aanbrengt en klik vervolgens op voor het voltooien van de migratie naar een regionaal vnet's, de eigenschap AffinityGroup in de netwerkconfiguratie. De netwerkconfiguratie in PowerShell eerst te exporteren en vervang de **AffinityGroup** eigenschap in de **VirtualNetworkSite** element met een **locatie** eigenschap. Geef `Location = XXXX` waar `XXXX` is een Azure-regio. De nieuwe configuratie importeren.

Bijvoorbeeld, overweegt de volgende VNET-configuratie:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Als u wilt dit verplaatsen naar een regionaal VNET in West-Europa, de configuratie te wijzigen met het volgende:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Opslagaccounts
U moet een nieuw opslagaccount die is geconfigureerd voor Premium-opslag maken. Houd er rekening mee dat het gebruik van Premium-opslag is ingesteld op het opslagaccount niet op afzonderlijke VHD's, maar bij gebruik van een DS * reeks VM u de VHD van Premium en Standard-opslag-accounts koppelen kunt. U kunt dit overwegen als u niet wilt dat de OS-VHD doorsturen naar de Premium-opslagaccount plaatsen.

De volgende **nieuw AzureStorageAccountPowerShell** opdracht met de 'Premium_LRS' **Type** maakt een Premium-Opslagaccount:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Cache-instellingen voor virtuele harde schijven
Het belangrijkste verschil tussen het maken van schijven die deel van een Premium Storage-account uitmaken is de schijfcache-instelling. Voor SQL Server-gegevens volume het schijven wordt aanbevolen dat u '**Lees-Caching**'. Voor transactie logboekvolumes, de schijfcache-instelling moet worden ingesteld op '**geen**'. Dit wijkt af van de aanbevelingen voor Standard-opslagaccounts.

Zodra de VHD's zijn gekoppeld, kan de cache-instelling kan niet worden gewijzigd. U moet ontkoppelen en opnieuw koppelen van de VHD met een bijgewerkte cache-instelling.

### <a name="windows-storage-spaces"></a>Windows-opslagruimten
U kunt [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) zoals u deed met vorige Standard-opslag, Hierdoor kunt u voor het migreren van een virtuele machine die wordt al gebruikt door opslagruimten. Het voorbeeld in [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (stap 9 en voorwaarts) ziet u de Powershell-code voor het uitpakken en importeren van een virtuele machine met meerdere gekoppelde VHD's.

Opslaggroepen zijn gebruikt met standaard Azure storage-account om de doorvoer te verbeteren en de latentie te verminderen. Wellicht waarde in nieuwe implementaties het testen van opslaggroepen met Premium-opslag, maar ze extra complexiteit met setup opslag toevoegen.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Welke virtuele Azure-schijven zijn toegewezen aan opslaggroepen zoeken
Als er verschillende cache-instelling aanbevelingen voor het gekoppelde VHD's zijn, moet u besluiten de VHD's kopiëren naar een Premium Storage-account. Echter, wanneer u opnieuw aan de nieuwe virtuele machine van de DS-serie koppelen, mogelijk moet u voor het wijzigen van de cache-instellingen. Het is eenvoudiger om toe te passen de Premium-opslag aanbevolen cache-instellingen wanneer u afzonderlijke VHD's voor de SQL-gegevensbestanden en logboekbestanden (eerder dan een enkele VHD met zowel) bevatten.

> [!NOTE]
> Als u SQL Server-gegevens en logboekbestanden bestanden op hetzelfde volume hebt, afhankelijk van de cache in optie die u kiest de i/o-toegangspatronen voor de werkbelasting van uw database. Alleen testen kunt laten zien welke cache-instelling wordt aanbevolen voor dit scenario.
>
>

Als u gebruikmaakt van Windows Storage Spaces die bestaan uit meerdere VHD's moet u kijken naar uw oorspronkelijke scripts te identificeren die gekoppeld VHD's zijn echter in welke specifieke groep, dus vervolgens stelt u de cache-instellingen dienovereenkomstig voor elke schijf.

Als u geen oorspronkelijke script beschikbaar om weer te geven die VHD's zijn toegewezen aan de opslaggroep, kunt u de volgende stappen uit om te bepalen van de toewijzing van schijfopslag/groep.

Gebruik de volgende stappen uit voor elke schijf:

1. Haal de lijst met schijven die zijn gekoppeld aan VM met de **Get-AzureVM** opdracht:

    Get-AzureVM - ServiceName <servicename> -naam <vmname> | Get-AzureDataDisk
2. Noteer de Diskname en LUN.

    ![DisknameAndLUN][2]
3. Extern bureaublad in de virtuele machine. Ga vervolgens naar **Computerbeheer** | **Apparaatbeheer** | **schijfstations**. Bekijk de eigenschappen van elk van de 'Microsoft virtuele schijven'

    ![VirtualDiskProperties][3]
4. Hier het nummer van de LUN is een verwijzing naar het LUN-nummer dat u opgeeft bij het toevoegen van de VHD naar de virtuele machine.
5. Voor de 'Microsoft virtuele schijf, gaat u naar de **Details** tabblad en klik dan in de **eigenschap** lijst, gaat u naar **stuurprogrammasleutel**. In de **waarde**, Opmerking de **Offset**, namelijk 0002 in de volgende schermafbeelding. De 0002 geeft aan dat de PhysicalDisk2 die de opslaggroep naar verwijst.

    ![VirtualDiskPropertyDetails][4]
6. Voor elke opslaggroep dump uit de gekoppelde schijven:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk

    ![GetStoragePool][5]

Nu kunt u deze informatie om te koppelen virtuele harde schijven gekoppeld aan fysieke schijven in opslaggroepen.

Wanneer u de VHD's aan de fysieke schijven in opslaggroepen vervolgens loskoppelen en kopieer deze naar een Premium Storage-account toegewezen hebt via, koppelt u ze met de juiste cache-instelling. Zie het voorbeeld in de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), stappen 8 tot en met 12. Deze stappen laten zien hoe een schijfconfiguratie VHD VM gekoppeld aan een CSV-bestand uitpakken, kopieert u de VHD's, wijzigen van de schijf configuratie cache-instellingen en ten slotte de virtuele machine opnieuw te implementeren als een reeks DS VM met alle gekoppelde schijven.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM-opslag bandbreedte en opslagdoorvoer VHD
Het bedrag van de prestaties van de opslag, is afhankelijk van de DS * VM-grootte die is opgegeven en de grootte van de VHD. De virtuele machines hebben verschillende rechten voor het aantal virtuele harde schijven die kunnen worden gekoppeld en de maximale bandbreedte (MB/s) wordt ondersteund. Zie voor de getallen specifieke bandbreedte [virtuele Machine en Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Verbeterde IOPS worden verkregen met grotere schijf. Dit moet u overwegen wanneer u over het migratiepad voor de nadenkt. Voor meer informatie [Zie de tabel voor IOPS en schijftypen](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).

Overweeg tot slot dat virtuele machines hebben verschillende maximale schijf-bandbreedten die wordt ondersteund voor alle schijven die zijn gekoppeld. U kunt de maximale schijf-bandbreedte die beschikbaar zijn voor de grootte van die VM-rol kan verzadigen onder hoge belasting. Bijvoorbeeld ondersteunt een Standard_DS14 maximaal 512 MB/s; Daarom kan u de bandbreedte van de schijf van de virtuele machine verzadigen met drie P30 schijven. Maar in dit voorbeeld wordt de doorvoer kan worden overschreden, afhankelijk van de combinatie van lees- en IOs.

## <a name="new-deployments"></a>Nieuwe implementaties
De volgende twee secties laten zien hoe u SQL Server-VM's kunt implementeren naar Premium-opslag. Zoals al eerder vermeld, wilt u niet per se plaatsen van de schijf met het besturingssysteem naar Premium-opslag. U kunt kiezen om dit te doen als u wilde een intensieve i/o-werkbelastingen op de VHD OS plaatsen.

Het eerste voorbeeld laat zien met behulp van de bestaande Azure-galerie met installatiekopieën. Het tweede voorbeeld toont hoe u een aangepaste VM-installatiekopie die u in een bestaand Standard-opslag-account hebt gebruikt.

> [!NOTE]
> Deze voorbeelden wordt ervan uitgegaan dat u al een regionaal VNET hebt gemaakt.
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Maak een nieuwe virtuele machine met Premium-opslag met afbeelding
Het volgende voorbeeld laat zien hoe u plaatst de OS-VHD naar premium-opslag en Premium-opslag-VHD's koppelen. U kunt echter ook de OS-schijf in een Standard-opslagaccount plaatsen en vervolgens VHD's die zich in een Premium Storage-account bevinden koppelen. Beide scenario's worden uitgelegd.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Stap 1: Een Premium Storage-Account maken
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Stap 2: Maak een nieuwe Cloudservice
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Stap 3: Gereserveerd VIP van de Cloud-Service (optioneel)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Stap 4: Een VM-Container maken
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Stap 5: Brengen OS VHD Standard of Premium-opslag
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Stap 6: Een virtuele machine maken
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Een nieuwe virtuele machine voor het gebruik van Premium-opslag met een aangepaste installatiekopie maken
Dit scenario laat zien waar u de bestaande aangepaste installatiekopieën die zich in een Standard-opslag-account bevinden hebt. Zoals vermeld als u wilt de OS-VHD op Premium-opslag plaatsen, moet u kopiëren van de installatiekopie die voorkomt in de Standard-opslagaccount en dragen deze naar een Premium-opslag voordat deze kan worden gebruikt. Als u een installatiekopie van een lokale, kunt u deze methode ook gebruiken om te kopiëren die rechtstreeks aan de Premium-opslag-account.

#### <a name="step-1-create-storage-account"></a>Stap 1: De Storage-Account maken
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Stap 2-Cloudservice maken
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Stap 3: Een bestaande installatiekopie gebruiken
U kunt een bestaande installatiekopie gebruiken. U kunt [nemen van een installatiekopie van een bestaande machine](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Let op de machine installatiekopie u niet hoeft te worden DS * machine. Zodra u de installatiekopie hebt, de volgende stappen laten zien hoe dit te kopiëren naar de Premium-opslag-account met de **Start AzureStorageBlobCopy** PowerShell-cmdlet.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Stap 4: Kopieer Blob tussen Opslagaccounts
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Stap 5: Controleer regelmatig de status van het exemplaar:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Stap 6: Installatiekopie schijf toevoegen aan de schijf van Azure-opslagplaats in abonnement
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> Kan het gebeuren dat hoewel de statusrapporten als geslaagd u kan nog steeds een schijffout lease. In dit geval wacht tien minuten.
>
>

#### <a name="step-7--build-the-vm"></a>Stap 7: De virtuele machine maken
U maakt hier de virtuele machine van uw installatiekopie en twee Premium-opslag-VHD's koppelen:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Bestaande implementaties die altijd op beschikbaarheidsgroepen niet wordt gebruikt
> [!NOTE]
> Voor bestaande implementaties raadpleegt u eerst de [vereisten](#prerequisites-for-premium-storage) sectie van dit onderwerp.
>
>

Er zijn verschillende overwegingen voor implementaties van SQL Server die niet altijd op beschikbaarheidsgroepen en programma's die u wilt gebruiken. Als u altijd op niet gebruikt en een bestaande zelfstandige SQL Server hebt, kunt u upgraden naar de Premium-opslag met behulp van een nieuw cloud service- en storage-account. Houd rekening met de volgende opties:

* **Maak een nieuwe SQL Server VM**. U kunt een nieuwe versie van SQL Server VM die gebruikmaakt van een Premium-opslagaccount maken, zoals beschreven in nieuwe implementaties. Vervolgens back-up en herstel van SQL Server-configuratie- en databases. De toepassing moet worden bijgewerkt om te verwijzen naar de nieuwe SQL-Server als intern of extern wordt geopend. U moet alle objecten voor buiten-db' kopiëren als je bezig een gelijktijdige (SxS) SQL Server-migratie was mee. Dit omvat objecten, zoals aanmeldingen, certificaten en gekoppelde servers.
* **Migreren van een bestaande SQL Server VM**. Hiervoor moet de virtuele machine van SQL Server offline moet worden gezet en u deze overbrengt naar een nieuwe cloudservice, waaronder alle van de gekoppelde VHD's te kopiëren naar de Premium-opslagaccount. Wanneer de virtuele machine online wordt gezet, wordt de naam van de server-host als voordat verwijzen naar de toepassing. Let erop dat de grootte van de bestaande schijf invloed is op de prestatiekenmerken. Bijvoorbeeld een schijf 400 GB opgehaald naar boven afgerond op een P20. Als u weet dat u niet nodig hebt die schijfprestaties kan u opnieuw de virtuele machine als een reeks DS VM maken en koppelen van Premium Storage VHD's van de grootte van/prestatiegebeurtenissen-specificatie die u nodig hebt. Vervolgens kan loskoppelen en koppelt u de bestanden van de SQL-database opnieuw.

> [!NOTE]
> Wanneer welk type opslagschijf Premium kopiëren van de VHD-schijven die u moet rekening houden met de grootte, afhankelijk van de grootte betekent dat ze vallen, dit schijf prestaties specificatie bepaalt. Azure worden afgerond op het dichtstbijzijnde schijf grootte, dus als er een schijf 400 GB, dit wordt afgerond op een P20. Afhankelijk van uw bestaande i/o-vereisten van de OS-VHD moet u mogelijk niet dit migreren naar een Premium Storage-account.
>
>

Als uw SQL-Server extern is geopend, wordt het VIP van cloud service wijzigen. Er wordt ook zijn update eindpunten, ACL's en DNS-instellingen.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Bestaande implementaties die gebruikmaken van altijd op beschikbaarheidsgroepen
> [!NOTE]
> Voor bestaande implementaties raadpleegt u eerst de [vereisten](#prerequisites-for-premium-storage) sectie van dit onderwerp.
>
>

In eerste instantie in dit gedeelte kijken we altijd op de interactie met Azure-netwerken. We vervolgens migraties in twee scenario's wordt opsplitsen: waar u enige uitvaltijd kan verdragen migraties en migraties waar u de minimale downtime moet bereiken.

Lokale SQL Server AlwaysOn-beschikbaarheidsgroepen gebruiken een Listener op lokale waarmee een virtuele DNS-naam samen met een IP-adres dat wordt gedeeld tussen een of meer SQL-Servers geregistreerd. Wanneer clients verbinding maken ze doorgestuurd via de listener-IP voor de primaire SQL-Server. Dit is de server die eigenaar is van de altijd op IP-resource op dat moment.

![DeploymentsUseAlways op][6]

In Microsoft Azure die kunt u slechts één IP-adres is toegewezen aan een NIC op de virtuele machine hebt, dus het oog op dezelfde laag van abstractie als on-premises Azure maakt gebruik van het IP-adres dat is toegewezen aan de interne of externe Load Balancers (ILB/ELB). De IP-resource die wordt gedeeld tussen de servers is ingesteld op het hetzelfde IP-adres als de ILB-/ ELB. Dit is gepubliceerd in de DNS-server en clientverkeer via de ILB-/ ELB wordt doorgegeven aan de primaire SQL Server-replica. De ILB-/ ELB kent die SQL Server is de primaire omdat deze tests gebruikt voor de resource altijd op IP-test. Deze tests op elk knooppunt dat een waarnaar wordt verwezen door de ELB/ILB-eindpunt is in het vorige voorbeeld, afhankelijk van wat reageert, wordt de primaire SQL-Server.

> [!NOTE]
> De ILB en ELB zijn beide toegewezen aan een bepaald Azure-cloudservice, dus cloudmigratie in Azure waarschijnlijk betekent dat dat het IP-adres van Load Balancer wordt gewijzigd.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migreren altijd op implementaties die enige downtime kunnen toestaan
Er zijn twee strategieën voor het migreren van altijd op implementaties die voor enige downtime toestaan:

1. **Meer secundaire replica's toevoegen aan een bestaand altijd op Cluster**
2. **Migreren naar een nieuwe altijd op Cluster**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Meer secundaire replica's toevoegen aan een bestaand altijd op Cluster
Er is een strategie voor een meer secundaire databases toevoegen aan de AlwaysOn-beschikbaarheidsgroep. U moet deze in een nieuwe cloudservice toevoegen en bijwerken van de listener met de nieuwe load balancer IP-adres.

##### <a name="points-of-downtime"></a>De punten van downtime:
* Clustervalidatie van het.
* Always On failover testen voor de nieuwe secundaire replica's.

Als u Windows-opslaggroepen vanuit de virtuele machine voor een hogere i/o-doorvoer en vervolgens deze zal offline worden gehaald tijdens een volledige validatie van de Cluster. De validatietest is vereist als u knooppunten aan het cluster toevoegen. De tijd die nodig is voor het uitvoeren van de test kan variëren, dus moet u dit in uw testomgeving om op te halen bij benadering de tijd van hoe lang dit gaat testen.

U moet inrichten tijd waar u handmatige failover en chaos testen op de zojuist toegevoegde knooppunten uitvoeren kunt om ervoor te zorgen altijd op hoge beschikbaarheid functies zoals verwacht.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Alle exemplaren van SQL Server waar de opslaggroepen worden gebruikt moet worden gestopt voordat de validatie wordt uitgevoerd.
>
> ##### <a name="high-level-steps"></a>Stappen op hoog niveau
>

1. Maak twee nieuwe SQL-Servers in de nieuwe cloudservice met gekoppelde Premium-opslag.
2. Kopiëren via volledige back-ups en herstellen met **NORECOVERY**.
3. Kopiëren via 'buiten user DB' afhankelijke objecten, zoals aanmeldingen enzovoort.
4. Maken van nieuwe een nieuwe interne Load Balancer (ILB) of gebruik een externe Load Balancer (ELB) en vervolgens Load Balanced eindpunten instellen op beide nieuwe knooppunten.

   > [!NOTE]
   > Controleer dat alle knooppunten de eindpuntconfiguratie juist hebt voordat u doorgaat
   >
   >
5. Stop de toepassing/gebruiker toegang tot de SQL-Server (als u de opslaggroepen).
6. SQL Server-Engine-Services stoppen op alle knooppunten (als u de opslaggroepen).
7. Toevoegen van nieuwe knooppunten om te clusteren en volledige validatie uitgevoerd.
8. Wanneer validatie voltooid is, start u alle SQL Server-Services.
9. Transactielogboeken back-up en herstel van gebruikersdatabases.
10. Nieuwe knooppunten toevoegen aan de AlwaysOn-beschikbaarheidsgroep en replicatie in plaats **synchroon**.
11. De bron van de IP-adres van de nieuwe Cloud Service ILB-/ ELB via PowerShell toevoegen voor altijd op op basis van het voorbeeld van meerdere sites in de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage). In het Windows-clustering, stelt u de **mogelijke eigenaars** van de **IP-adres** resource toe aan de nieuwe knooppunten oude. Zie de sectie 'Toe te voegen IP-adres Resource op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
12. Failover naar een van de nieuwe knooppunten.
13. Controleer de nieuwe knooppunten automatisch Failoverpartners en test failovers.
14. Oorspronkelijke knooppunten verwijderen uit de beschikbaarheidsgroep.

##### <a name="advantages"></a>Voordelen
* Nieuwe SQL-Servers kunnen worden getest (SQL Server en toepassing) voordat ze worden toegevoegd aan altijd op.
* U kunt de VM-grootte wijzigen en aanpassen van de opslag voor uw exacte vereisten. Het normaal zou zijn echter handig zijn om alle SQL-bestandspaden hetzelfde blijven.
* U kunt bepalen wanneer de overdracht van de database back-ups naar de secundaire replica's worden gestart. Dit wijkt af van het gebruik van Azure **Start AzureStorageBlobCopy** commandlet kopiëren VHD's, omdat die een asynchrone kopie.

##### <a name="disadvantages"></a>Nadelen
* Wanneer met behulp van Windows-opslaggroepen, is dit Cluster uitvaltijd tijdens de validatie van het volledige Cluster voor de nieuwe extra knooppunten.
* Afhankelijk van de versie van SQL Server en het bestaande aantal secundaire replica's, kunnen mogelijk niet meer secundaire replica's toevoegen zonder te verwijderen van bestaande secundaire replica's.
* Kan er lang SQL gegevens overdrachtstijd tijdens het instellen van de secundaire replica's zijn.
* Er is extra kosten tijdens de migratie terwijl u nieuwe machines parallelle uitvoering zijn.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Migreren naar een nieuwe altijd op Cluster
Een andere strategie is een geheel nieuwe altijd op Cluster maken met de nieuwe knooppunten in een nieuwe cloudservice en leid de clients om dit te gebruiken.

##### <a name="points-of-downtime"></a>Punten van uitvaltijd
Er is uitvaltijd wanneer u toepassingen en gebruikers overdraagt naar de nieuwe listener altijd op. Afhankelijk van de uitvaltijd:

* De tijd laatste transactielogboekback-ups terugzetten naar databases op nieuwe servers.
* De tijd die nodig is voor het bijwerken van de clienttoepassingen kunnen gebruikmaken van nieuwe listener altijd op.

##### <a name="advantages"></a>Voordelen
* U kunt de werkelijke productieomgeving, SQL Server, testen en OS-build-wijzigingen.
* U hebt de optie voor het aanpassen van de opslag en grootte van virtuele machine mogelijk te verminderen. Dit kan leiden tot kosten te beperken.
* Tijdens dit proces kunt u uw versie van SQL Server of een versie bijwerken. U kunt ook het besturingssysteem bijwerken.
* Het vorige altijd op Cluster kan fungeren als een doel effen terugdraaien.

##### <a name="disadvantages"></a>Nadelen
* U moet de DNS-naam van de listener wijzigen als u wilt dat beide AlwaysOn-clusters die tegelijkertijd wordt uitgevoerd. Beheer overhead tijdens de migratie wordt toegevoegd als client toepassing tekenreeksen moeten overeenstemming met de naam van de nieuwe Listener.
* U moet een mechanisme voor synchronisatie tussen de twee omgevingen te houden zo dicht mogelijk bij de laatste synchronisatievereisten voor de migratie minimaliseren implementeren.
* Er is toegevoegd kosten tijdens de migratie als u de nieuwe omgeving uitgevoerd.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migreren altijd op implementaties voor minimale downtime
Er zijn twee strategieën voor het migreren altijd op implementaties voor minimale downtime:

1. **Gebruikmaken van een bestaande secundaire: één Site**
2. **Gebruikmaken van bestaande secundaire beschikbaarheidsreplica('s): meerdere locaties**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Gebruikmaken van een bestaande secundaire: één Site
Een strategie voor minimale downtime is een bestaande cloud secundaire nemen en te verwijderen uit de huidige cloudservice. Vervolgens de VHD's kopiëren naar het nieuwe account voor de Premium-opslag en de virtuele machine in de nieuwe cloudservice maken. Werk vervolgens de listener in clustering en failover.

##### <a name="points-of-downtime"></a>Punten van uitvaltijd
* Er is uitvaltijd wanneer u het laatste knooppunt met het eindpunt met gelijke taakverdeling bijwerken.
* Opnieuw verbinden met uw client mogelijk afhankelijk van de client en DNS-configuratie worden uitgesteld.
* Er is extra uitvaltijd als u de altijd op clustergroep offline nemen wilt om het wisselen van de IP-adressen. U kunt dit vermijden met behulp van een afhankelijkheid of en mogelijke eigenaars voor de toegevoegde IP-adres-resource. Zie de sectie 'Toe te voegen IP-adres Resource op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).

> [!NOTE]
> Wanneer u wilt dat de toegevoegde knooppunt partake in als altijd op Failover-Partner, moet u een Azure-eindpunt met een verwijzing naar de Load Balanced Set toevoegen. Bij het uitvoeren van de **toevoegen AzureEndpoint** opdracht om dit te doen, huidige verbindingen in stand moeten blijven, maar nieuwe verbindingen met de listener kan niet worden vastgesteld totdat de load balancer is bijgewerkt. In dit testen is gezien naar de laatste 90-120seconds dit moet worden getest.
>
>

##### <a name="advantages"></a>Voordelen
* Er zijn geen extra kosten die zijn gemaakt tijdens de migratie.
* Een-op-een migratie.
* Minder complexiteit.
* Verbeterde IOPS staat van Premium-opslag-SKU's. Wanneer de schijven losgekoppeld van de virtuele machine zijn en een 3rd partij gekopieerd naar de nieuwe cloudservice kan hulpprogramma worden gebruikt om de VHD-grootte die hoger doorvoercapaciteit biedt. Zie voor het VHD-grootte verhogen, [forumdiscussie](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nadelen
* Er is een tijdelijk onderbroken HA en Noodherstel tijdens de migratie.
* Omdat dit een migratie 1:1, moet u een minimale VM-grootte die wordt ondersteuning voor het aantal virtuele harde schijven, zodat u mogelijk niet afslanken van uw virtuele machines gebruiken.
* Dit scenario gebruikt Azure **Start AzureStorageBlobCopy** commandlet die asynchroon is. Er is geen SLA op voltooiing van de kopie. De tijd van de exemplaren varieert, terwijl dit afhankelijk van de wachttijd in wachtrij die het is ook afhankelijk van de hoeveelheid gegevens om over te dragen. De tijd kopie verhoogd als de overdracht wordt een andere Azure-datacenter die ondersteuning biedt voor Premium-opslag in een andere regio. Als u zojuist 2 knooppunten hebt, kunt u overwegen een mogelijke Risicobeperking voor het geval de kopie langer dan in de testfase duurt. Dit omvat de volgende ideeën.
  * Toevoegen van een tijdelijk 3e SQL Server-knooppunt voor HA vóór de migratie overeengekomen uitvaltijd.
  * Voer de migratie buiten Azure gepland onderhoud.
  * Zorg ervoor dat u hebt uw clusterquorum correct geconfigureerd.  

##### <a name="high-level-steps"></a>Stappen op hoog niveau
Dit document niet illustratie van een volledige end-to-end-voorbeeld, maar het [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) voorziet in gedetailleerde informatie die kunnen worden gebruikt om deze.

![MinimalDowntime][8]

* De schijfconfiguratie verzamelen en verwijderen van het knooppunt (gekoppelde VHD's niet verwijderd).
* Premium-opslagaccount maken en VHD's van het standaard opslagaccount kopiëren
* Nieuwe cloudservice maken en implementeren van de VM SQL2 in deze cloudservice. Maak de virtuele machine met behulp van de gekopieerde oorspronkelijke besturingssysteem-VHD en koppelen van de gekopieerde VHD's.
* Configureer ILB / ELB en het toevoegen van eindpunten.
* Update-Listener door:
  * De altijd op groep offline te halen en het bijwerken van de altijd op Listener met nieuwe ILB / ELB-IP-adressen.
  * Of het IP-adres resource van nieuwe Cloud Service ILB-/ ELB via PowerShell in Windows-clustering toe te voegen. Vervolgens stelt de mogelijke eigenaars van de bron-IP-adres gemigreerde-knooppunt, SQL2, en dit als afhankelijkheid of in de netwerknaam. Zie de sectie 'Toe te voegen IP-adres Resource op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
* Controleer de DNS-configuratie/doorgiftetaak voor clients.
* SQL1 VM gemigreerd en doorloop de stappen 2-4.
* Als stappen 5ii wordt gebruikt, voegt u SQL1 als een mogelijke eigenaar voor de bron van de toegevoegde IP-adres
* Testfailovers.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Gebruikmaken van bestaande secundaire beschikbaarheidsreplica('s): meerdere locaties
Als u knooppunten in meer dan één Azure-datacenter (DC hebben) of als u een hybride omgeving hebt, kunt klikt u vervolgens u een AlwaysOn-configuratie in deze omgeving uitvaltijd te minimaliseren.

De aanpak is om te wijzigen van de synchronisatie altijd op in synchrone voor de on-premises of secundaire Azure domeincontroller, en vervolgens failover via naar SQL Server. Vervolgens kopieert u de VHD's naar een Premium Storage-account en de machine implementeren in een nieuwe cloudservice. Werk de listener en vervolgens een failback uit.

##### <a name="points-of-downtime"></a>Punten van uitvaltijd
De uitvaltijd bestaat uit de tijd om failover naar de alternatieve DC en terug. Ook afhankelijk van de client en DNS-configuratie en de client opnieuw verbinden mag worden uitgesteld.
Houd rekening met het volgende voorbeeld van een hybride altijd op de configuratie:

![MultiSite1][9]

##### <a name="advantages"></a>Voordelen
* U kunt gebruikmaken van bestaande infrastructuur.
* U hebt de optie voor het Azure-opslag op de DC DR Azure eerst vóór de upgrade.
* De DC DR Azure-opslag opnieuw kan worden geconfigureerd.
* Er is een minimum van twee failovers tijdens de migratie, met uitzondering van testfailovers.
* U hoeft niet te verplaatsen van SQL Server-gegevens met back-up en herstellen.

##### <a name="disadvantages"></a>Nadelen
* Afhankelijk van de clienttoegang tot SQL Server, kunnen er langere latentie als SQL Server wordt uitgevoerd in een andere domeincontroller aan de toepassing.
* De tijd van de kopie van VHD's voor Premium-opslag kan lang zijn. Dit kan invloed hebben op uw beslissing op of het knooppunt in de beschikbaarheidsgroep. U kunt dit wanneer logboek intensieve werk belastingen worden uitgevoerd tijdens de migratie vereist, is omdat het primaire knooppunt moet niet-gerepliceerde transacties in het transactielogboek behouden. Daarom kan dit aanzienlijk groeien.
* Dit scenario gebruikt Azure **Start AzureStorageBlobCopy** commandlet die asynchroon is. Er is geen SLA op voltooiing. De tijd van de exemplaren varieert, terwijl dit afhankelijk van de wachttijd in wachtrij, het is ook afhankelijk van de hoeveelheid gegevens om over te dragen. Daarom alleen hebt u één knooppunt in uw datacenter 2e, u moet treffen Risicobeperking voor het geval de kopie langer duurt dan in de testfase. Dit omvat de volgende ideeën.
  * Een tijdelijke SQL-knooppunt 2e toevoegen voor HA vóór de migratie overeengekomen uitvaltijd.
  * Voer de migratie buiten Azure gepland onderhoud.
  * Zorg ervoor dat u hebt uw clusterquorum correct geconfigureerd.

Dit scenario wordt ervan uitgegaan dat u de installatie hebt gedocumenteerd en weten hoe de opslag is toegewezen om te kunnen aanbrengen voor optimale schijf cache-instellingen.

##### <a name="high-level-steps"></a>Stappen op hoog niveau
![Multisite2][10]

* De on-premises / DC van Azure naar de SQL Server-primaire alternatieve, en kunnen de andere automatische Failover Partner (AFP).
* Verzamelen van gegevens over de schijfconfiguratie van SQL2 en verwijderen van het knooppunt (gekoppelde VHD's niet verwijderd).
* Premium-opslagaccount maken en kopieer VHD's van het standaard opslagaccount.
* Maak een nieuwe cloudservice en de SQL2 virtuele machine maken met de opslag van premies schijven die zijn gekoppeld.
* Configureer ILB / ELB en het toevoegen van eindpunten.
* De altijd op Listener bijwerken met nieuwe ILB / ELB IP-adres en test failover.
* Controleer de DNS-configuratie.
* Wijzig de AFP in SQL2, SQL1 migreren en stappen 2 tot en met 5 doorlopen.
* Testfailovers.
* De AFP overschakelen naar SQL1 en SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Bijlage: Een implementatie voor meerdere locaties altijd op een Cluster migreren naar de Premium-opslag
De rest van dit onderwerp biedt een gedetailleerd voorbeeld van het converteren van een altijd op clusters op meerdere locaties naar Premium-opslag. Ook converteert naar een interne load balancer (ILB) de Listener van het gebruik van een externe load balancer (ELB).

### <a name="environment"></a>Omgeving
* Windows 2k 12 / SQL 2k 12
* Bestanden op SP 1 DB
* 2 x opslaggroepen per knooppunt

![Appendix1][11]

### <a name="vm"></a>VIRTUELE MACHINE:
In dit voorbeeld gaan we demonstreren verplaatsen van een ELB naar ILB. ELB is beschikbaar voordat ILB, zodat dit overschakelen naar dit tijdens de migratie worden weergegeven.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Pre-stappen: Verbinding maken met abonnement
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Stap 1: Nieuw Opslagaccount maken en in de Cloud Service
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Stap 2: Verhoog de toegestane fouten op resources<Optional>
Bepaalde bronnen die deel uitmaken van de AlwaysOn-beschikbaarheidsgroep er gelden beperkingen op hoeveel storingen die kunnen optreden in een periode waarin de cluster-service probeert het opnieuw opstarten van de resourcegroep. Het is raadzaam dat u deze terwijl u via deze procedure zijn doorlopen sinds als u niet handmatig failover en trigger failovers door het afsluiten van computers die u dicht bij deze limiet krijgt verhogen.

Moet worden beperkt te verdubbelen het tegoed is mislukt om dit te doen in Failoverclusterbeheer, gaat u naar de eigenschappen van de resourcegroep altijd op:

![Appendix3][13]

Wijzig het maximum aantal fouten in 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Stap 3: Aanvullende IP-adres resource voor de clustergroep<Optional>
Als er slechts één IP-adres voor de clustergroep en dit wordt uitgelijnd op het subnet van de cloud, houd er rekening mee, als u per ongeluk offline alle clusterknooppunten in de cloud op dat netwerk vervolgens de Cluster-IP-resource halen en de clusternetwerknaam kan niet worden online worden gezet. In het geval van dit kan updates voor andere clusterbronnen.

#### <a name="step-4-dns-configuration"></a>Stap 4: DNS-configuratie
Voor het implementeren van een vloeiende overgang is afhankelijk van hoe DNS wordt gebruikt en bijgewerkt.
Wanneer altijd op is geïnstalleerd, wordt er een resourcegroep van Windows-Cluster gemaakt als u Failoverclusterbeheer opent, ziet u dat ten minste deze drie bronnen heeft, zijn de twee dat het document naar verwijst:

* Virtuele-netwerknaam (VNN) – Dit is de DNS-naam die client verbinding maken wanneer er verbinding maken met SQL-Servers via altijd op willen.
* IP-adres van bron: dit is het IP-adres dat aan de VNN gekoppeld, kunt u meer dan één hebben en in een configuratie voor meerdere sites hebt u een IP-adres per site/subnet.

Bij het verbinding maken met SQL Server, SQL Server Client stuurprogramma wordt de DNS-records die zijn gekoppeld aan de listener ophalen en probeer het verbinding maken met elke altijd op gekoppelde IP-adres, bespreken hieronder we een aantal factoren die van invloed kunnen zijn op dit.

Het aantal gelijktijdige DNS-records die gekoppeld aan de naam van de listener zijn afhankelijk is, niet alleen op het aantal IP-adressen die zijn gekoppeld, maar de ' RegisterAllIpProviders'setting in Failover Clustering voor de resource altijd ON VNN.

Wanneer u altijd op in Azure implementeert, er zijn verschillende stappen voor het maken van de Listener en IP-adressen, u moet handmatig configureren van de 'RegisterAllIpProviders' 1, dit is anders dan bij een on-premises altijd op implementatie waarop deze is al ingesteld op 1.

Als 'RegisterAllIpProviders' 0 is, klikt u vervolgens ziet alleen u een DNS-record in DNS die zijn gekoppeld aan de Listener:

![Appendix4][14]

Als 'RegisterAllIpProviders' 1:

![Appendix5][15]

Hieronder de code wordt uit de instellingen VNN dump en voor u ingesteld, houd er rekening mee voor deze duurt Listener offline waardoor de wijziging door te voeren u moet de VNN offline zetten en schakel deze weer online onderbreking van de client-connectiviteit.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

In een latere migratiestap van een moet u de listener Always On bijwerken met een bijgewerkte IP-adres dat verwijst naar een load balancer, dient hiervoor een IP-adres resource verwijderen en toevoegen. Nadat de IP-update moet u ervoor zorgen het nieuwe IP-adres is bijgewerkt in DNS-Zone en dat de clients hun lokale DNS-cache wordt bijgewerkt.

Als uw clients bevinden zich in een ander netwerksegment en verwijzen naar een andere DNS-server, moet u nagaan wat er gebeurt over het overdragen van DNS-Zone tijdens de migratie als de toepassing opnieuw tijd worden beperkt door ten minste de Zone Transfer-tijd van alle nieuwe IP-adressen voor de listener. Als u hier tijd-beperking, u moet worden behandeld en testen forceren van een incrementele zoneoverdracht met uw Windows-teams en plaatst de DNS-hostrecord ook naar een lagere tijd To Live (TTL), zodat de clients bijwerken. Zie voor meer informatie [incrementele zoneoverdrachten](https://technet.microsoft.com/library/cc958973.aspx) en [Start DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Standaard de TTL voor DNS-Record die is gekoppeld aan de Listener altijd op in Azure is 1200 seconden. Mogelijk wilt u dit reduceren als u onder tijd beperking tijdens de migratie om te controleren of de clients hun DNS bijwerken met de bijgewerkte IP-adres voor de listener. U kunt zien en de configuratie wijzigen door het dumpen van een van de configuratie van de VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Houd er rekening mee hoe lager het 'HostRecordTTL', een hogere mate van DNS-verkeer wordt uitgevoerd.

##### <a name="client-application-settings"></a>Instellingen voor client-toepassing
Als u de SQL-clienttoepassing de .net 4.5 ondersteunt SQLClient, dan hebt u kunt gebruiken ' MULTISUBNETFAILOVER = TRUE' sleutelwoord, dit wordt aanbevolen om te worden toegepast zoals maakt het mogelijk sneller verbinding met SQL AlwaysOn-beschikbaarheidsgroep tijdens failover. Het inventariseren via alle IP-adressen die zijn gekoppeld aan de listener altijd op parallel en een agressievere TCP opnieuw verbindingssnelheid tijdens een failover uitvoert.

Zie voor meer informatie over de bovenstaande instellingen [MultiSubnetFailover sleutelwoord en functies die zijn gekoppeld](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Zie ook [SqlClient-ondersteuning voor hoge beschikbaarheid, herstel na noodgevallen](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Stap 5: Instellingen van het clusterquorum
Als u te hebben van ten minste één SQL-Server niet actief op een tijdstip gaat, moet u de instelling van het clusterquorum wijzigen als bestand Share Witness (FSW) met 2 knooppunten, moet u de Quorumconfiguratie Knooppuntmeerderheid toestaan en gebruikmaken van dynamische stemmen instellen , en dit is om toe te staan voor één knooppunt in het permanente blijven.

    Set-ClusterQuorum -NodeMajority  

Zie voor meer informatie over het beheren en configureren van het clusterquorum [configureren en beheren van het Quorum in een Windows Server 2012-failovercluster](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Stap 6: Extraheren bestaande eindpunten en ACL 's
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Sla deze naar een tekstbestand.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Stap 7: Failoverpartners en replicatie-modi wijzigen
Als u meer dan 2 SQL-Servers hebt, moet u de failover van een andere secundaire in een andere domeincontroller of on-premises wijzigen in 'Synchrone' en kunt u een Partner voor automatische Failover (AFP), is dit zodat u HA onderhouden terwijl u wijzigingen doorvoert. U kunt dit doen via TSQL van echter SSMS wijzigen:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Stap 8: Verwijder secundaire virtuele machine van de cloudservice
U rekening mee moet houden voor het migreren van een secundaire knooppunt cloud eerst als dit momenteel primaire is, moet u een handmatige failover initiëren.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 9: Instellingen in CSV-bestand van de schijfcache wijzigen en opslaan
Voor gegevensvolumes moeten deze worden ingesteld op alleen-lezen.

Voor TLOG volumes moeten deze worden ingesteld op NONE.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Stap 10: Kopie VHD 's
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



U kunt de status van het exemplaar van de VHD's met de Premium-opslagaccount controleren:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Wacht totdat deze worden geregistreerd als geslaagd.

Voor informatie over afzonderlijke blobs:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Stap 11: Register OS-schijf
    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Stap 12: Secundaire importeren in de nieuwe cloudservice
De volgende code gebruikt ook de optie toegevoegde hier kunt u de machine importeren en gebruiken van het retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Stap 13: Maak ILB op nieuwe Cloud Svc toevoegen van Load Balanced eindpunten en ACL's
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a>Stap 14: Altijd bijwerken op
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nu de oude IP-adres van de cloudservice verwijderen.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Stap 15: DNS-updates controleren
Nu moet u DNS-Servers in uw SQL Server client netwerken controleren en zorg ervoor dat het clustering de extra hostrecord toegevoegde IP-adres heeft toegevoegd. Als deze DNS-servers niet hebt bijgewerkt, houd rekening met het forceren van een DNS-Zone-overdracht en zorg ervoor dat de clients in er subnet kunnen worden omgezet in beide altijd op IP-adressen, dit is dus u hoeft niet te wachten op automatische DNS-replicatie.

#### <a name="step-16-reconfigure-always-on"></a>Stap 16: Configureer altijd opnieuw op
Op dit moment wachten op de secundaire dat knooppunt die volledig zijn gesynchroniseerd met het lokale knooppunt en overschakelen naar het knooppunt synchrone replicatie en kunt u de AFP is gemigreerd.  

#### <a name="step-17-migrate-second-node"></a>Stap 17: Tweede knooppunt migreren
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 18: Instellingen in CSV-bestand van de schijfcache wijzigen en opslaan
Voor gegevensvolumes moeten deze worden ingesteld op alleen-lezen.

Voor TLOG volumes moeten deze worden ingesteld op NONE.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Stap 19: Maak een nieuwe onafhankelijke Storage-Account voor secundair knooppunt
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Stap 20: Kopie VHD 's
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


U kunt de status van het VHD-exemplaar controleren voor alle virtuele harde schijven: ForEach ($disk in $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Schijflabel $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Wacht totdat deze worden geregistreerd als geslaagd.

Voor informatie over afzonderlijke blobs:

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Stap 21: Register OS-schijf
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Stap 22: Toevoegen Load Balanced eindpunten en ACL's
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Stap 23: De testfailover
U moet nu gebruiken om het gemigreerde knooppunt synchroniseren met het lokale altijd aan knooppunt in naar de modus voor synchrone replicatie plaats en wacht totdat deze is gesynchroniseerd. Vervolgens wordt de failover van on-premises naar het eerste knooppunt gemigreerd, die de AFP is. Zodra die eerder heeft gewerkt, wijzigt u het laatste knooppunt van de gemigreerde in de AFP.

U moet testfailovers tussen alle knooppunten en hoewel chaos tests uit om ervoor te zorgen failovers work als verwacht en in een tijdige manor uitvoeren.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>24 stap: Opnieuw clusterquoruminstellingen zetten / DNS TTL / Failover Pntrs / synchronisatie-instellingen
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Toevoegen van de bron van het IP-adres op hetzelfde Subnet
Als u alleen 2 SQL-Servers hebt en wilt deze migreren naar een nieuwe cloudservice maar wilt houden in hetzelfde subnet, kunt u voorkomen dat de listener offline brengen naar de oorspronkelijke altijd op IP-adres verwijderen en het nieuwe IP-adres toevoegen. Als u de virtuele machines naar een ander subnet migreert moet u niet om dit te doen omdat er een extra clusternetwerk dat verwijst naar dat subnet.

Zodra u hebt de gemigreerde secundaire beschikbaar komen en in de nieuwe resource IP-adres voor de nieuwe cloudservice vóór de failover de bestaande primaire toegevoegd, moet u deze stappen binnen de failover-Clusterbeheer uitvoeren:

Als u wilt toevoegen aan IP-adres, Zie de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), stap 14.

1. Wijzig de mogelijke eigenaar 'Bestaande primaire SQL-Server', in het onderstaande voorbeeld 'dansqlams4' voor de huidige bron van de IP-adres:

    ![Appendix13][23]
2. Wijzig de mogelijke eigenaar naar 'Gemigreerd secundaire SQL-Server', in het onderstaande voorbeeld 'dansqlams5' voor de nieuwe bron voor het IP-adres:

    ![Appendix14][24]
3. Wanneer deze optie is ingesteld, kunt u failover en wanneer het laatste knooppunt is gemigreerd de mogelijke eigenaars worden bewerkt zodat dit knooppunt wordt toegevoegd als een mogelijke eigenaar:

    ![Appendix15][25]

## <a name="additional-resources"></a>Aanvullende bronnen
* [Azure Premium-opslag](../../../storage/common/storage-premium-storage.md)
* [Virtuele machines](https://azure.microsoft.com/services/virtual-machines/)
* [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
