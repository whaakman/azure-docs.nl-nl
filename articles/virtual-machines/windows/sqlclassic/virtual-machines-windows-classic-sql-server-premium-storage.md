---
title: Azure Premium Storage gebruiken met SQL Server | Microsoft Docs
description: In dit artikel maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel en biedt richtlijnen over het gebruik van Azure Premium Storage met SQL Server wordt uitgevoerd op Azure Virtual Machines.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: ac5b3bec9915574dd33d40ae2dcbc5aa3c91280a
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332163"
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Azure Premium Storage gebruiken met SQL Server op Virtual Machines
## <a name="overview"></a>Overzicht
[Azure Premium Storage](../premium-storage.md) is de volgende generatie van de opslag die een lage latentie hoge doorvoer i/o en. Deze geschikt is voor belangrijke i/o-intensieve werkbelastingen, zoals SQL Server op IaaS [virtuele Machines](https://azure.microsoft.com/services/virtual-machines/).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Dit artikel bevat plannings- en richtlijnen voor het migreren van een virtuele Machine met SQL Server het gebruik van Premium Storage. Dit omvat Azure-infrastructuur (netwerken, opslag) en Gast Windows VM stappen. Het voorbeeld in de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) ziet u een volledige uitgebreide end-to-migratie van het verplaatsen van grotere virtuele machines om te profiteren van verbeterde lokale SSD-opslag met PowerShell.

Het is belangrijk om te begrijpen van het proces voor end-to-end met Azure Premium Storage met SQL Server op IAAS-VM's. Dit omvat:

* Identificatie van de vereisten voor het gebruik van Premium Storage.
* Voorbeelden van de implementatie van SQL Server op IaaS naar Premium Storage voor nieuwe implementaties.
* Voorbeelden van migreren van bestaande implementaties, het zowel zelfstandige servers en implementaties met behulp van SQL Always On Availability Groups.
* Mogelijke migratiestrategie hanteren.
* Voorbeeld van de volledige end-to-end van Azure, Windows en SQL Server-stappen voor de migratie van een bestaande implementatie altijd op.

Zie voor meer achtergrondinformatie over SQL Server in Azure Virtual Machines, [SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

**Auteur:** Daniel Sol **technische controleurs:** Luis Carlos Vargas haring, Sanjay Mishra, Pravin Mital, Schwertl Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Vereisten voor Premium Storage
Er zijn verschillende vereisten voor het gebruik van Premium Storage.

### <a name="machine-size"></a>Grootte van de machine
Voor het gebruik van Premium-opslag die u wilt gebruiken van DS-serie virtuele Machines (VM). Als u niet uit de DS-machines in uw cloudservice voordat u hebt gebruikt, moet u de bestaande virtuele machine verwijdert, houden van de gekoppelde schijven en vervolgens een nieuwe cloudservice maken voordat u opnieuw maken van de virtuele machine als de grootte van de rol DS *. Zie voor meer informatie over de grootten van virtuele machines, [virtuele Machine en Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="cloud-services"></a>Cloud services
U kunt alleen virtuele machines DS * met Premium Storage gebruiken wanneer ze worden gemaakt in een nieuwe cloudservice. Als u SQL Server Always On in Azure, wordt de Always On-Listener verwijst naar het Azure interne of externe Load Balancer IP-adres dat is gekoppeld aan een cloudservice. In dit artikel richt zich op het migreren van behoud van beschikbaarheid in dit scenario.

> [!NOTE]
> Een reeks DS * moet de eerste virtuele machine die is geïmplementeerd naar de nieuwe Cloud-Service.
>
>

### <a name="regional-vnets"></a>Regionale vnet 's
U moet het virtuele netwerk (VNET) die als host fungeert voor uw virtuele machines om te worden regionale configureren voor virtuele machines DS *. Dit 'genereren' in het VNET om toe te staan de grotere virtuele machines om te worden ingericht in andere clusters en communicatie ertussen. In de volgende schermafbeelding ziet u de geselecteerde locatie regionale vnet's, terwijl de eerste resultaat een 'kleine' VNET toont.

![RegionalVNET][1]

U kunt een Microsoft-ondersteuningsticket als u wilt migreren naar een regionaal VNET verhogen. Microsoft maakt vervolgens een wijziging. Voor het voltooien van de migratie naar een regionaal vnet's, de eigenschap AffinityGroup in de configuratie van het netwerk te wijzigen. Eerst exporteert de netwerkconfiguratie in PowerShell en vervang de **AffinityGroup** eigenschap in de **VirtualNetworkSite** element met een **locatie** eigenschap. Geef `Location = XXXX` waar `XXXX` is een Azure-regio. Vervolgens importeert u de nieuwe configuratie.

Bijvoorbeeld, overweegt de volgende VNET-configuratie:

```xml
<VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
<AddressSpace>
  <AddressPrefix>10.0.0.0/8</AddressPrefix>
  <AddressPrefix>172.16.0.0/12</AddressPrefix>
</AddressSpace>
<Subnets>
...
</VirtualNetworkSite>
```

Als u wilt verplaatsen naar een regionaal VNET in West-Europa, de configuratie te wijzigen op het volgende:
```xml
<VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
<AddressSpace>
  <AddressPrefix>10.0.0.0/8</AddressPrefix>
  <AddressPrefix>172.16.0.0/12</AddressPrefix>
</AddressSpace>
<Subnets>
...
</VirtualNetworkSite>
```

### <a name="storage-accounts"></a>Opslagaccounts
U moet een nieuw opslagaccount dat is geconfigureerd voor Premium-opslag maken. Houd er rekening mee dat het gebruik van Premium-opslag is ingesteld op het storage-account, niet op afzonderlijke VHD's, maar wanneer u een DS *-serie VM u de VHD van Premium en Standard Storage-accounts koppelen kunt. U kunt dit desgewenst als u niet wilt plaatsen van de VHD met besturingssysteem u aan bij de Premium Storage-account.

De volgende **New-AzureStorageAccountPowerShell** opdracht met de 'Premium_LRS" **Type** maakt u een Premium Storage-Account:

```powershell
$newstorageaccountname = "danpremstor"
New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   
```

### <a name="vhds-cache-settings"></a>Cache-instellingen voor VHD 's
Het belangrijkste verschil tussen het maken van schijven die deel van een Premium Storage-account uitmaken is de cache-instelling van de schijf. Voor SQL Server-gegevens-volume-het schijven wordt aanbevolen dat u '**Caching van lezen**'. Voor transactie logboekvolumes, cache-instelling van de schijf moet worden ingesteld op '**geen**'. Dit wijkt af van de aanbevelingen voor Standard Storage-accounts.

Nadat de VHD's zijn gekoppeld, kan de cache-instelling kan niet worden gewijzigd. U moet loskoppelen en opnieuw koppelen van de VHD met een bijgewerkte cache-instelling.

### <a name="windows-storage-spaces"></a>Windows-opslagruimten
U kunt [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) net als bij vorige Standard-opslag, Hiermee kunt u voor het migreren van een virtuele machine die wordt al gebruikt door opslagruimten. Het voorbeeld in [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (stap 9 en verder) ziet u de Powershell-code voor het ophalen en importeren van een virtuele machine met meerdere gekoppelde VHD's.

Opslaggroepen zijn met de standaard-Azure storage-account gebruikt voor het verbeteren van doorvoer en latentie te verminderen. Wellicht waarde in de testfase van opslaggroepen met Premium Storage voor nieuwe implementaties, maar Voeg deze extra complexiteit met setup van opslag.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Welke virtuele Azure-schijven toewijzen aan opslaggroepen zoeken
Er zijn verschillende cache-instelling aanbevelingen voor het gekoppelde VHD's, kunt u besluiten om te kopiëren van de VHD's naar een Premium Storage-account. Echter wanneer u ze opnieuw aan de nieuwe virtuele machine van de DS-serie koppelen, mogelijk moet u de cache-instellingen wijzigen. Het is eenvoudiger om toe te passen van de Premium Storage aanbevolen van de cache-instellingen wanneer u afzonderlijke VHD's voor de SQL-gegevensbestanden en logboekbestanden (plaats één VHD met zowel) hebt.

> [!NOTE]
> De cache door u gekozen optie als u SQL Server-gegevens en logboekbestanden bestanden op hetzelfde volume hebt, is afhankelijk van de i/o-toegangspatronen voor workloads van uw databases. Alleen testen kunt laten zien welke optie voor opslaan in cache is het meest geschikt voor dit scenario.
>
>

Als u gebruikmaakt van Windows Storage Spaces die bestaan uit meerdere VHD's die u nodig hebt om te kijken naar de oorspronkelijke scripts om te identificeren welke VHD's gekoppeld zijn echter in welke specifieke groep, dus kan vervolgens stelt u de cache-instellingen dienovereenkomstig voor elke schijf.

Als u geen oorspronkelijke script beschikbaar om weer te geven welke VHD's worden toegewezen aan de opslaggroep hebt, kunt u de volgende stappen uit om te bepalen van de toewijzing van schijfopslag/pool.

Voor elke schijf, gebruikt u de volgende stappen uit:

1. Get-lijst met schijven die zijn gekoppeld aan virtuele machine met de **Get-AzureVM** opdracht:

```powershell
Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk
```

2. Noteer de Diskname en LUN.

    ![DisknameAndLUN][2]
3. Extern bureaublad op de virtuele machine. Ga vervolgens naar **Computerbeheer** | **Apparaatbeheer** | **schijfstations**. Bekijk de eigenschappen van elk van de 'Microsoft virtuele schijven'

    ![VirtualDiskProperties][3]
4. Hier het nummer van de logische eenheid is een verwijzing naar het LUN die u opgeeft bij het koppelen van de VHD aan de virtuele machine.
5. Voor de 'Microsoft Virtual Disk' gaat u naar de **Details** tabblad, klik dan in de **eigenschap** lijst, gaat u naar **stuurprogrammasleutel**. In de **waarde**, houd er rekening mee de **Offset**, namelijk 0002 in de volgende schermafbeelding. De 0002 geeft aan dat de PhysicalDisk2 die de opslaggroep naar verwijst.

    ![VirtualDiskPropertyDetails][4]
6. Dump van de gekoppelde schijven voor elke opslaggroep:

```powershell
Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk
```

![GetStoragePool][5]

U kunt nu gebruiken die deze informatie om te koppelen VHD's zijn gekoppeld aan fysieke schijven in opslaggroepen bevinden.

Wanneer u VHD's hebt gekoppeld met fysieke schijven in opslaggroepen vervolgens loskoppelen en kopieert u deze naar een Premium Storage-account, koppelt u ze met de juiste cache-instelling. Zie het voorbeeld in de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), stap 8 t/m 12. Deze stappen laten zien hoe een virtuele machine gekoppelde VHD-schijfconfiguratie op een CSV-bestand uitpakken, Kopieer de VHD's, de schijfconfiguratie cache-instellingen wijzigen en ten slotte de virtuele machine opnieuw te implementeren als een DS-serie VM met alle gekoppelde schijven.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM-opslag bandbreedte en doorvoer van VHD-opslag
Het bedrag van de prestaties van de opslag, is afhankelijk van de opgegeven DS * VM-grootte en de VHD-grootte. De virtuele machines hebben andere limieten voor het aantal virtuele harde schijven die kunnen worden gekoppeld en de maximale bandbreedte (MB/s) wordt ondersteund. Zie voor de nummers specifieke bandbreedte [virtuele Machine en Cloud Service Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Verbeterde IOP's worden bereikt met grotere schijfgrootten. U kunt deze moet overwegen wanneer u over uw migratiepad nadenkt. Voor meer informatie, [Zie de tabel voor IOPS en schijftypen](../premium-storage.md#scalability-and-performance-targets).

Ten slotte kunt u overwegen dat virtuele machines hebben andere maximale schijf-bandbreedten die worden ondersteund voor alle schijven die zijn gekoppeld. U kunt de maximale schijf-bandbreedte die beschikbaar zijn voor de grootte van die VM-rol kan verzadigen onder hoge belasting. Bijvoorbeeld ondersteunt een Standard_DS14 maximaal 512 MB/s; Daarom kunt u de bandbreedte van de schijf van de virtuele machine verzadigen met drie P30-schijven. Maar in dit voorbeeld kan de doorvoerlimiet wordt overschreden, afhankelijk van de combinatie van lees- en IOs.

## <a name="new-deployments"></a>Nieuwe implementaties
De volgende twee secties laten zien hoe u SQL Server-VM's naar Premium Storage kunt implementeren. Zoals al eerder vermeld, hoeft niet per se te plaatsen van de besturingssysteemschijf naar Premium storage. U kunt dit doen als u willen plaatsen van alle i/o-intensieve workloads op de VHD met het besturingssysteem.

Het eerste voorbeeld ziet u met behulp van bestaande Azure-galerie met installatiekopieën. Het tweede voorbeeld ziet hoe u een aangepaste VM-installatiekopie die u in een bestaande Standard storage-account hebt gebruikt.

> [!NOTE]
> Deze voorbeelden wordt ervan uitgegaan dat u al een regionaal VNET hebt gemaakt.
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Een nieuwe virtuele machine maken met Premium Storage met galerijafbeelding
In het volgende voorbeeld laat zien hoe u plaatst de VHD met het besturingssysteem naar premium-opslag en koppelen van Premium Storage-VHD's. U kunt echter ook de besturingssysteemschijf plaatsen in een Standard Storage-account en voeg vervolgens VHD's die zich in een Premium Storage-account bevinden. Beide scenario's toegelicht.

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Set up subscription
Set-AzureSubscription -SubscriptionName $mysubscription
Select-AzureSubscription -SubscriptionName $mysubscription -Current  
```

#### <a name="step-1-create-a-premium-storage-account"></a>Stap 1: Premium Storage-Account maken

```powershell
#Create Premium Storage account, note Type
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  
```

#### <a name="step-2-create-a-new-cloud-service"></a>Stap 2: Een nieuwe Cloudservice maken

```powershell
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Stap 3: Reserveer een Cloud Service VIP-(optioneel)

```powershell
#check exisitng reserved VIP
Get-AzureReservedIP

$reservedVIPName = “sqlcloudVIP”
New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location
```

#### <a name="step-4-create-a-vm-container"></a>Stap 4: Een VM-Container maken

```powershell
#Generate storage keys for later
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

##Generate storage acc contexts
$xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

#Create container
$containerName = 'vhds'
New-AzureStorageContainer -Name $containerName -Context $xioContext
```

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Stap 5: Het plaatsen van de VHD met besturingssysteem op Standard of Premium-opslag

```powershell
#NOTE: Set up subscription and default storage account which is used to place the OS VHD in

#If you want to place the OS VHD Premium Storage Account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

#If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
$standardstorageaccountname = "danstdams"

Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname
```

#### <a name="step-6-create-vm"></a>Stap 6: VM maken

```powershell
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
#Note the size specified ‘-DiskSizeInGB 1023’, this attaches 2 x P30 Premium Storage Disk Type
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
```

### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Een nieuwe virtuele machine voor het gebruik van Premium-opslag met een aangepaste installatiekopie maken
Dit scenario wordt uitgelegd waar u de bestaande aangepaste installatiekopieën die zich in een Standard Storage-account bevinden hebt. Zoals vermeld als u wilt plaatsen van VHD met het besturingssysteem op de Premium-opslag die u wilt kopiëren van de installatiekopie die voorkomt in de Standard-opslagaccount en deze overbrengen naar een Premium Storage voordat deze kan worden gebruikt. Als u een installatiekopie van een on-premises, kunt u deze methode ook gebruiken om te kopiëren die rechtstreeks naar de Premium Storage-account.

#### <a name="step-1-create-storage-account"></a>Stap 1: Opslagaccount maken

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Create Premium Storage account
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

#Standard Storage account
$origstorageaccountname = "danstdams"
```

#### <a name="step-2-create-cloud-service"></a>Stap 2-Cloudservice maken

```powershell
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-3-use-existing-image"></a>Stap 3: Bestaande installatiekopie gebruiken
U kunt een bestaande installatiekopie. U kunt [nemen van een installatiekopie van een bestaande virtuele machine](../classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Houd er rekening mee de machine die u installatiekopie niet hoeft te worden DS * machine. Zodra u de installatiekopie hebt, de volgende stappen laten zien hoe u het kopiëren naar de Premium Storage-account met de **Start AzureStorageBlobCopy** PowerShell-commandlet.

```powershell
#Get storage account keys:
#Standard Storage account
$originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
#Premium Storage account
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

#Set up contexts for the storage accounts:
$origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
$destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  
```

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Stap 4: Blob kopiëren tussen Opslagaccounts

```powershell
#Get Image VHD
$myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
$containerName = 'vhds'

#Copy Blob between accounts
$blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
-DestContainer vhds -Destblob "prem-$myImageVHD" `
-Context $origContext -DestContext $destContext  
```

#### <a name="step-5-regularly-check-copy-status"></a>Stap 5: Controleer regelmatig Kopieerstatus:

```powershell
$blob | Get-AzureStorageBlobCopyState
```

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Stap 6: Afbeelding schijf toevoegen aan Azure disk-opslagplaats in abonnement

```powershell
$imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
$newimageName = "prem"+"dansoldonorsql2k14"

Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation
```

> [!NOTE]
> U merkt u misschien dat zelfs als de status als geslaagd rapporteert, nog steeds een fout van de lease krijgt kan. In dit geval wacht tien minuten.
>
>

#### <a name="step-7--build-the-vm"></a>Stap 7:  De virtuele machine maken
Hier maakt u de virtuele machine van de afbeelding en twee Premium Storage-VHD's koppelen:

```powershell
$newimageName = "prem"+"dansoldonorsql2k14"
#Set up Machine Specific Information
$vmName = "dansolchild"
$vnet = "westeur"
$subnet = "Clients"
$ipaddr = "192.168.0.41"

#This needs to be a new cloud service
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
```

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Bestaande implementaties die geen Always On Availability Groups gebruiken
> [!NOTE]
> Voor bestaande implementaties raadpleegt u eerst de [vereisten](#prerequisites-for-premium-storage) sectie van dit artikel.
>
>

Er zijn verschillende overwegingen voor SQL Server-implementaties die geen Always On Availability Groups en die kan gebruiken. Als u altijd op niet gebruikt en een bestaande zelfstandige SQL Server hebt, kunt u upgraden naar Premium Storage met behulp van een nieuwe cloud service en storage-account. Houd rekening met de volgende opties:

* **Maak een nieuwe SQL Server-VM**. U kunt een nieuwe SQL Server-VM, die gebruikmaakt van een Premium Storage-account kunt maken, zoals beschreven in nieuwe implementaties. Een back-up en herstellen van uw SQL Server-databases voor configuratie en de gebruiker. De toepassing moet worden bijgewerkt om te verwijzen naar de nieuwe SQL Server als de intern of extern wordt geopend. U moet alle objecten voor buiten-db' kopiëren alsof je mee bezig was een naast elkaar (SxS) SQL Server-migratie. Het gaat hierbij om objecten zoals aanmeldingen, certificaten en gekoppelde servers.
* **Migreren van een bestaande SQL Server-VM**. Hiervoor moet het offline halen van de SQL Server-VM en u deze overbrengt naar een nieuwe service in de cloud, waaronder alle van de gekoppelde VHD's kopiëren naar de Premium Storage-account. Wanneer de virtuele machine online gaat, wordt de naam van de server-host als voordat verwezen naar de toepassing. Let erop dat de grootte van de bestaande schijf is van invloed op de prestatiekenmerken. Bijvoorbeeld, een 400 GB-schijf opgehaald naar boven afgerond op een P20. Als u weet dat u die prestaties van de schijf niet nodig hebt, kunt u opnieuw maken van de virtuele machine als een DS-serie VM, en koppelen van Premium Storage-VHD's van de grootte van/prestaties-specificatie die u nodig hebt. U kunt vervolgens loskoppelen en opnieuw koppelen van de SQL-database-bestanden.

> [!NOTE]
> Bij het kopiëren van de VHD-schijven die u moet rekening houden met de grootte, afhankelijk van de grootte welke schijftype voor Premium Storage betekent dat ze kunnen worden onderverdeeld in, deze schijf prestaties specificatie bepaalt. Azure rondt af naar de dichtstbijzijnde grootte van de schijf, dus hebt u een schijf van 400 GB, dit wordt afgerond naar een P20. Afhankelijk van uw bestaande i/o-vereisten van de OS VHD moet u mogelijk niet deze migreren naar een Premium Storage-account.
>
>

Als uw SQL-Server extern is geopend, klikt u vervolgens het VIP van cloud service gewijzigd. Ook hebt update-eindpunten, ACL's en DNS-instellingen.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Bestaande implementaties die gebruikmaken van Always On Availability Groups
> [!NOTE]
> Voor bestaande implementaties raadpleegt u eerst de [vereisten](#prerequisites-for-premium-storage) sectie van dit artikel.
>
>

In deze sectie bekijken we in eerste instantie altijd op de interactie met Azure Networking. We vervolgens migraties in twee scenario's opsplitsen: waar enige uitvaltijd kan worden getolereerd-migraties en migraties waar u de minimale downtime moet bereiken.

On-premises SQL Server Always On Availability Groups een Listener on-premises gebruiken waarmee een virtuele DNS-naam, samen met een IP-adres dat wordt gedeeld tussen een of meer SQL-Servers geregistreerd. Wanneer clients verbinding maken, zijn ze doorgestuurd via het listener-IP-adres voor de primaire SQL-Server. Dit is de server die eigenaar is van de altijd op IP-resource op dat moment.

![DeploymentsUseAlways op][6]

In Microsoft Azure die kunt u slechts één IP-adres toegewezen aan een NIC op de virtuele machine hebt, dus als u wilt bereiken dezelfde laag van abstractie zoals on-premises, Azure maakt gebruik van het IP-adres dat is toegewezen aan de interne/externe Load Balancers (ILB/ELB). De IP-resource die wordt gedeeld tussen de servers is ingesteld op het hetzelfde IP-adres als de ILB-/ ELB. Dit is gepubliceerd in de DNS-server en -clientverkeer via het ILB-/ ELB wordt doorgegeven aan de primaire SQL Server-replica. Het ILB-/ ELB weet dit SQL Server is primaire omdat deze tests gebruikt voor de altijd op IP-resource-test. In het vorige voorbeeld wordt het elk knooppunt dat een waarnaar wordt verwezen door de ELB/ILB eindpunt tests, afhankelijk van wat reageert, wordt de primaire SQL-Server.

> [!NOTE]
> De ILB en ELB zijn beide toegewezen aan een bepaald Azure cloud service, daarom cloudmigratie in Azure waarschijnlijk betekent dat het Load Balancer IP-adres wordt gewijzigd.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migratie altijd op implementaties die enige uitvaltijd kunnen toestaan
Er zijn twee strategieën voor het migreren van AlwaysOn-implementaties die voor enige downtime zorgen:

1. **Meer secundaire replica's toevoegen aan een bestaand altijd op Cluster**
2. **Migreren naar een nieuw altijd op Cluster**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Meer secundaire replica's toevoegen aan een bestaand altijd op Cluster
Een strategie bestaat uit het toevoegen van meer secundaire databases aan de AlwaysOn-beschikbaarheidsgroep. U moet deze in een nieuwe cloudservice toevoegen en bijwerken van de listener met de nieuwe load balancer IP-adres.

##### <a name="points-of-downtime"></a>Punten van downtime:
* Clustervalidatie van het.
* Always On failover testen voor de nieuwe secundaire replica's.

Als u Windows Storage Pools binnen de virtuele machine voor een hogere i/o-doorvoer, worden deze offline zijn ondernomen tijdens een volledige validatie van het Cluster. De validatietest is vereist als u knooppunten aan het cluster toevoegt. De tijd die nodig zijn om uit te voeren van de test kan variëren, dus moet u dit in uw testomgeving om op te halen van een bij benadering de tijd van hoe lang dit duurt testen.

Tijd waar u handmatige failover en chaos testen op de zojuist toegevoegde knooppunten uitvoeren kunt om te controleren of de Always On hoge beschikbaarheid functies zoals verwacht, moet u inrichten.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Stopt u alle exemplaren van SQL Server waar de opslaggroepen worden gebruikt voordat de validatie wordt uitgevoerd.
>
> ##### <a name="high-level-steps"></a>Stappen op hoog niveau
>

1. Maak twee nieuwe SQL-Servers in de nieuwe service in de cloud met Premium Storage.
2. Kopiëren via volledige back-ups en herstel met **NORECOVERY**.
3. Kopiëren via 'buiten het user DB' afhankelijke objecten, zoals aanmeldingen enzovoort.
4. Nieuwe een nieuwe interne Load Balancer (ILB) maken of gebruik een externe Load Balancer (ELB) en vervolgens Load Balanced eindpunten instellen op beide nieuwe knooppunten.

   > [!NOTE]
   > Controleer dat alle knooppunten de juiste configuratie van eindpunten hebt voordat u doorgaat
   >
   >
5. Gebruiker/toepassing toegang heeft tot de SQL-Server stoppen (als u opslaggroepen).
6. SQL Server-Engine-Services stoppen op alle knooppunten (als u opslaggroepen).
7. Nieuwe knooppunten voor het cluster en het uitvoeren van volledige validatie toevoegen.
8. Wanneer de validatie voltooid is, start u alle SQL Server-Services.
9. Back-up van transactielogboeken en terugzetten van de gebruiker.
10. Nieuwe knooppunten toevoegen aan de AlwaysOn-beschikbaarheidsgroep en replicatie in plaats **synchroon**.
11. De resource van het IP-adres van de nieuwe Cloud Service ILB-/ ELB via PowerShell toevoegen voor Always On op basis van meerdere locaties in het voorbeeld in de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage). In het Windows-clustering, stelt u de **mogelijke eigenaars** van de **IP-adres** resource met de oude nieuwe knooppunten. Zie de sectie 'Toe te voegen Resource IP-adres op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
12. Failover naar een van de nieuwe knooppunten.
13. Controleer de nieuwe knooppunten automatisch failover-Partners en test-failover.
14. Oorspronkelijke knooppunten verwijderen uit de beschikbaarheidsgroep.

##### <a name="advantages"></a>Voordelen
* Nieuwe SQL-Servers kunnen worden getest (SQL Server en toepassing) voordat ze worden toegevoegd voor Always On.
* U kunt wijzigen van de VM-grootte en de opslag aan uw exacte behoeften aanpassen. Het zou echter nuttig is om alle SQL-bestandspaden blijven hetzelfde zijn.
* U kunt bepalen wanneer de overdracht van de DB back-ups naar de secundaire replica's wordt gestart. Dit wijkt af van het gebruik van Azure **Start AzureStorageBlobCopy** commandlet kopiëren van VHD's, omdat die een asynchrone versie is.

##### <a name="disadvantages"></a>Nadelen
* Wanneer u opslaggroepen van Windows gebruikt, moet u er Cluster downtime tijdens de validatie van het volledige Cluster voor de nieuwe extra knooppunten is.
* Afhankelijk van de versie van SQL Server en het bestaande aantal secundaire replica's, kunnen mogelijk niet meer secundaire replica's toevoegen zonder te verwijderen van bestaande secundaire replica's.
* Er zijn mogelijk SQL data transfer lang tijdens het instellen van de secundaire replica's.
* Is er extra kosten tijdens de migratie als u nieuwe machines die parallel worden uitgevoerd.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Migreren naar een nieuw altijd op Cluster
Een andere strategie is een nieuwe altijd op Cluster maken met nieuwe knooppunten in een nieuwe cloudservice en leid vervolgens om de clients om deze te gebruiken.

##### <a name="points-of-downtime"></a>Punten van downtime
Er is uitvaltijd wanneer u toepassingen en gebruikers naar de nieuwe Always On-listener overdraagt. Afhankelijk van de downtime:

* De gebruikte tijd voor het herstellen van de laatste transactielogboekback-ups naar databases op nieuwe servers.
* De gebruikte tijd voor het bijwerken van de clienttoepassingen kunnen gebruikmaken van de nieuwe Always On-listener.

##### <a name="advantages"></a>Voordelen
* U kunt de daadwerkelijke productie-omgeving, SQL Server, testen en wijzigingen build van het besturingssysteem.
* U hebt de optie voor het aanpassen van de opslag en grootte van virtuele machine mogelijk te verminderen. Dit kan leiden tot kosten verminderen.
* Tijdens dit proces kunt u uw SQL Server-build of versie bijwerken. U kunt ook het besturingssysteem bijwerken.
* Het vorige altijd op Cluster kan fungeren als het doel van een solide terugdraaien.

##### <a name="disadvantages"></a>Nadelen
* U moet de DNS-naam van de listener wijzigen als u wilt dat beide AlwaysOn-clusters tegelijk worden uitgevoerd. Beheer overhead tijdens de migratie wordt toegevoegd als tekenreeksen van de client-toepassingen moeten de naam van de nieuwe Listener weerspiegelen.
* U kunt een mechanisme voor synchronisatie tussen de twee omgevingen dat ze zo dicht mogelijk bij het minimaliseren van de laatste synchronisatievereisten voor de migratie moet implementeren.
* Er wordt toegevoegd kosten tijdens de migratie als u de nieuwe omgeving uitgevoerd.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migratie altijd op implementaties voor minimale downtime
Er zijn twee strategieën voor het migreren Always On-implementaties voor minimale downtime:

1. **Gebruikmaken van een bestaande secundaire: Één locatie**
2. **Gebruikmaken van bestaande secundaire beschikbaarheidsreplica('s): Meerdere sites**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Gebruikmaken van een bestaande secundaire: Één locatie
Een strategie voor minimale downtime is een bestaande cloud secundaire en verwijderen uit de huidige cloudservice. Vervolgens kopieert u de VHD's naar de nieuwe Premium Storage-account en de virtuele machine maken in de nieuwe service in de cloud. Werk vervolgens de listener in clusters en failover.

##### <a name="points-of-downtime"></a>Punten van downtime
* Er is uitvaltijd wanneer u het laatste knooppunt met het eindpunt met gelijke taakverdeling bijwerken.
* Opnieuw verbinden met uw client kan zijn vertraagd, afhankelijk van uw client-en DNS-configuratie.
* Er is meer downtime als u ervoor kiest om de groep altijd op Cluster offline te wisselen van de IP-adressen. U kunt dit vermijden met behulp van een OR-afhankelijkheid en mogelijke eigenaars voor de toegevoegde IP-adresresource. Zie de sectie 'Toe te voegen Resource IP-adres op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).

> [!NOTE]
> Wanneer u wilt dat het knooppunt toegevoegd aan partake in als altijd op Failover-Partner, moet u een Azure-eindpunt met een verwijzing naar de Load Balanced Set toevoegen. Bij het uitvoeren van de **Add-AzureEndpoint** opdracht om dit te doen, opent u actieve verbindingen om te blijven, maar nieuwe verbindingen met de listener kan niet worden tot stand worden gebracht totdat de load balancer is bijgewerkt. In deze test is gedetecteerd op de laatste 90-120seconds, dit moet worden getest.
>
>

##### <a name="advantages"></a>Voordelen
* Zonder extra kosten in rekening gebracht tijdens de migratie.
* Een-op-een migratie.
* Minder complexiteit.
* Staat voor verbeterde IOPS van Premium-opslag-SKU's. Als de schijven ontkoppeld van de virtuele machine zijn en gekopieerd naar de nieuwe cloudservice, een 3e partij kan hulpprogramma worden gebruikt om te verhogen van de VHD-grootte, waarmee u hogere doorvoer. Zie voor het verhogen van VHD-grootte, dit [forumdiscussie](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nadelen
* Er is een tijdelijk onderbroken HA en DR tijdens de migratie.
* Omdat deze een 1:1-migratie, die u moet een minimale VM-grootte die ondersteuning biedt voor het aantal VHD's, zodat u mogelijk niet krimpen van uw virtuele machines gebruiken.
* In dit scenario gebruikt Azure **Start AzureStorageBlobCopy** -commandlet die asynchroon is. Er is geen SLA voor kopiëren is voltooid. De tijd van de exemplaren varieert, terwijl dit is afhankelijk van de wachttijd in wachtrij die ook afhankelijk van de hoeveelheid gegevens over te dragen. De tijd van het exemplaar wordt aangepast als de overdracht gaat naar een andere Azure-datacenter die ondersteuning biedt voor Premium-opslag in een andere regio. Als u alleen 2 knooppunten hebt, kunt u overwegen een mogelijke risicobeperking in het geval de kopie langer dan in de testfase duurt. Dit omvat bijvoorbeeld de volgende ideeën.
  * Een tijdelijke 3e SQL Server-knooppunt toevoegen voor HA vóór de migratie met overeengekomen downtime.
  * Voer de migratie buiten Azure gepland onderhoud.
  * Zorg ervoor dat u uw clusterquorum correct hebt geconfigureerd.  

##### <a name="high-level-steps"></a>Stappen op hoog niveau
Dit document biedt een compleet voorbeeld van de end-to-end niet demonstreren echter de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) bevat gedetailleerde informatie die kunnen worden gebruikt om dit uitvoeren.

![MinimalDowntime][8]

* Schijfconfiguratie verzamelen en verwijderen van het knooppunt (gekoppelde VHD's niet verwijderen).
* Een Premium Storage-account maken en kopiëren van VHD's van de Standard-opslagaccount
* Nieuwe cloudservice maken en implementeren van de VM SQL2 in die service in de cloud. Maak de virtuele machine met behulp van de gekopieerde VHD met het oorspronkelijke besturingssysteem en het koppelen van de gekopieerde VHD's.
* ILB configureren / ELB en het toevoegen van eindpunten.
* Update-Listener door:
  * Het offline halen van de altijd op groep en het bijwerken van de Always On-Listener met de nieuwe ILB / ELB-IP-adres.
  * Of het toevoegen van de IP-adresresource van nieuwe Cloud Service ILB-/ ELB via PowerShell in Windows-clustering. Vervolgens de mogelijke eigenaars van de bron-IP-adres ingesteld op het gemigreerde knooppunt SQL2, en stel deze optie in als de OR-afhankelijkheid in naam van het netwerk. Zie de sectie 'Toe te voegen Resource IP-adres op hetzelfde Subnet' van de [bijlage](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
* Controleer de DNS-configuratie/doorgiftetaak aan de clients.
* SQL1 virtuele machine migreren en stappen 2-4 doorlopen.
* Als stappen 5ii, voeg deze SQL1 als een mogelijke eigenaar voor de Resource van het toegevoegde IP-adres
* Testfailovers.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Gebruikmaken van bestaande secundaire beschikbaarheidsreplica('s): Multi-site
Als u knooppunten in meer dan één Azure-datacenter (DC hebt) of als u een hybride omgeving hebt, kunt klikt u vervolgens u de configuratie van een Always On in deze omgeving te minimaliseren van downtime.

De aanpak is het wijzigen van de synchronisatie Always On in synchrone voor de on-premises of secundaire Azure-domeincontroller, en vervolgens failover via naar SQL Server. Kopieer vervolgens de VHD's naar een Premium Storage-account, en de machine opnieuw implementeren naar een nieuwe cloudservice. De listener bijwerken en vervolgens een failback uitvoeren.

##### <a name="points-of-downtime"></a>Punten van downtime
Er bestaat de downtime van de tijd voor de failover naar de alternatieve DC en terug. Ook afhankelijk van uw client-en DNS-configuratie en opnieuw verbinden met uw client kan worden vertraagd.
Houd rekening met het volgende voorbeeld van een hybride Always On configuratie:

![MultiSite1][9]

##### <a name="advantages"></a>Voordelen
* U kunt gebruikmaken van bestaande infrastructuur.
* U hebt de optie voor de Azure-opslag op de DC DR Azure eerst vóór de upgrade.
* De DC DR-Azure-opslag kan worden geconfigureerd.
* Er is een minimum van twee failovers tijdens de migratie, met uitzondering van testfailovers.
* U hoeft niet te verplaatsen van SQL Server-gegevens met back-up en herstellen.

##### <a name="disadvantages"></a>Nadelen
* Afhankelijk van de clienttoegang tot SQL Server, kunnen er langere latentie als SQL Server wordt uitgevoerd in een andere domeincontroller aan de toepassing.
* De tijd van de kopie van de VHD's naar Premium storage kan lang zijn. Dit mogelijk invloed op uw beslissing of het knooppunt dat in de beschikbaarheidsgroep. Houd rekening met deze wanneer log intensief werk belasting worden uitgevoerd tijdens de migratie is vereist, omdat het primaire knooppunt heeft de niet-gerepliceerde transacties in het transactielogboek houden. Dus kan dit aanzienlijk groeien.
* In dit scenario gebruikt Azure **Start AzureStorageBlobCopy** -commandlet die asynchroon is. Er is geen SLA voor de voltooiing. De tijd van de exemplaren varieert, terwijl dit is afhankelijk van de wachttijd in wachtrij, deze ook afhankelijk is van de hoeveelheid gegevens over te dragen. Daarom u slechts één knooppunt hebt in uw datacenter 2e, u moet stappen nemen in het geval de kopie duurt langer dan in de testfase. Deze stappen zijn onder andere de volgende ideeën:
  * Een tijdelijke 2e SQL-knooppunt toevoegen voor HA vóór de migratie met overeengekomen downtime.
  * Voer de migratie buiten Azure gepland onderhoud.
  * Zorg ervoor dat u uw clusterquorum correct hebt geconfigureerd.

In dit scenario wordt ervan uitgegaan dat u de installatie hebt gedocumenteerd en hoe de opslag weet wilt aanbrengen voor cache-instellingen voor optimale schijf is toegewezen.

##### <a name="high-level-steps"></a>Stappen op hoog niveau
![Multisite2][10]

* Controleer de on-premises / Azure DC alternatieve als de primaire SQL-Server en geef deze de andere automatische failover-Partner (AFP).
* Verzamelen van gegevens over de schijfconfiguratie van SQL2 en verwijderen van het knooppunt (gekoppelde VHD's niet verwijderen).
* Een Premium Storage-account maken en kopieer VHD's van de Standard-opslagaccount.
* Een nieuwe cloudservice maken en de SQL2 virtuele machine maken met de premies Storage-schijven die zijn gekoppeld.
* ILB configureren / ELB en het toevoegen van eindpunten.
* De Always On-Listener voor bijwerken met nieuwe ILB / ELB IP-adres en failover.
* Controleer de DNS-configuratie.
* Wijzig de AFP in SQL2, SQL1 migreren en stappen 2-5 doorlopen.
* Testfailovers.
* De AFP overschakelen naar SQL1 en SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Bijlage: Een implementatie voor meerdere locaties altijd op een Cluster migreren naar Premium Storage
De rest van dit artikel bevat een gedetailleerd voorbeeld van het converteren van een Always On clusters op meerdere locaties naar Premium storage. Ook converteert naar een interne load balancer (ILB) de Listener van een externe load balancer (ELB).

### <a name="environment"></a>Omgeving
* Windows 2k12 / SQL 2k12
* Bestanden op SP 1 DB
* 2 x opslaggroepen per knooppunt

![Appendix1][11]

### <a name="vm"></a>VIRTUELE MACHINE:
In dit voorbeeld gaan we ter illustratie van verplaatsen van een ELB naar ILB. ELB is beschikbaar voor de ILB, zodat dit laat hoe u zien overschakelen naar de ILB tijdens de migratie.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Pre-stappen: Verbinding maken met abonnement

```powershell
Add-AzureAccount

#Set up subscription
Get-AzureSubscription
```

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Stap 1: Nieuw Opslagaccount maken en de Cloudservice

```powershell
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
```

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Stap 2: Verhoog de toegestane fouten op resources <Optional>
Voor bepaalde resources die deel uitmaken van de AlwaysOn-beschikbaarheidsgroep zijn er beperkingen voor het aantal storingen die in een periode waarin de cluster-service probeert optreden kunnen te starten van de resourcegroep. Het is raadzaam dat u deze verhogen terwijl u via deze procedure zijn doorlopen sinds als u dit niet handmatig failover en trigger failovers door machines u dicht bij deze limiet krijgt wordt afgesloten.

Moet worden beperkt op het dubbele van de aftrek van de fout, om dit te doen in Failoverclusterbeheer, gaat u naar de eigenschappen van de resourcegroep Always On:

![Appendix3][13]

Wijzig het maximum aantal fouten in 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Stap 3: Bronnen voor het IP-adres voor de clustergroep toevoegen <Optional>
Als u slechts één IP-adres hebt voor de clustergroep, en deze wordt uitgelijnd op het subnet van de cloud, houd er rekening mee, als u per ongeluk offline alle clusterknooppunten in de cloud op dat netwerk zetten en vervolgens de Cluster-IP-resource en de naam van clusternetwerk zijn pas weer online komt. In dit geval voorkomt updates dat naar andere clusterresources.

#### <a name="step-4-dns-configuration"></a>Stap 4: DNS-configuratie
Implementatie van een soepele overgang is afhankelijk van hoe DNS wordt gebruikt en wordt bijgewerkt.
Wanneer AlwaysOn is geïnstalleerd, wordt een Windows Cluster-resourcegroep gemaakt als u Failoverclusterbeheer opent, ziet u dat ten minste drie bronnen heeft, zijn de twee die het document naar verwijst:

* Virtuele-netwerknaam (VNN) – de DNS-naam waarmee clients verbinding maken wanneer die verbinding maken met SQL-Servers via altijd op.
* IP-Adresresource – het IP-adres die gekoppeld aan de VNN, u kunt meer dan één hebben en in een configuratie met meerdere locaties die u hebt een IP-adres per site/subnet.

Verbinding maken met SQL Server, SQL Server Client stuurprogramma haalt u de DNS-records die zijn gekoppeld aan de listener en probeert verbinding te maken voor elke Always On gekoppeld wanneer IP-adres. Vervolgens bespreken we enkele factoren die dit kunnen beïnvloeden.

Het aantal gelijktijdige DNS-records die gekoppeld aan de listener met de naam zijn is niet alleen op het aantal IP-adressen die zijn gekoppeld, afhankelijk van de ' RegisterAllIpProviders'setting in Failover Clustering voor de resource altijd op VNN.

Wanneer u altijd op in Azure implementeert, er zijn verschillende stappen voor het maken van de Listener en IP-adressen, u moet het handmatig configureren van de 'RegisterAllIpProviders' 1, dit is anders dan bij een on-premises AlwaysOn implementatie waarvan deze is al ingesteld op 1.

Als 'RegisterAllIpProviders' 0 is, klikt u vervolgens ziet alleen u een DNS-record in DNS die zijn gekoppeld aan de Listener van:

![Appendix4][14]

Als 'RegisterAllIpProviders' 1 is:

![Appendix5][15]

De onderstaande code dumpen van de instellingen voor VNN en stelt deze voor u. Voor de wijziging door te voeren, moet u de VNN offline te halen en weer online inschakelen. Hiermee wordt de Listener offline waardoor connectiviteit onderbreking van de client.

```powershell
##Always On Listener Name
$ListenerName = "Mylistener"
##Get AlwaysOn Network Name Settings
Get-ClusterResource $ListenerName| Get-ClusterParameter
##Set RegisterAllProvidersIP
Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1
```

In een latere migratiestap, moet u de Always On-listener bijwerken met een bijgewerkte IP-adres dat verwijst naar een load balancer, hiervoor moet een IP-adres resource verwijderen en toevoegen. Nadat de update IP moet u om te controleren of het nieuwe IP-adres in DNS-Zone is bijgewerkt en dat de clients hun lokale DNS-cache wordt bijgewerkt.

Als de clients bevinden zich in een ander netwerk-segment en verwijzen naar een andere DNS-server, moet u afvragen wat er gebeurt over DNS-Zone Transfer tijdens de migratie, omdat de toepassing opnieuw verbinding maken met tijd is beperkt door ten minste de Zone Transfer-tijd van een nieuw IP-adres adressen voor de listener. Als u hier een tijd-beperking, u moet bespreken en testen forceren van een incrementele zoneoverdracht met uw Windows-teams, en plaatst de DNS-hostrecord ook naar een lagere Time To Live (TTL), zodat de clients bijwerken. Zie voor meer informatie, [incrementele zoneoverdracht](https://technet.microsoft.com/library/cc958973.aspx) en [Start DnsServerZoneTransfer](https://docs.microsoft.com/powershell/module/dnsserver/start-dnsserverzonetransfer).

Is standaard de TTL voor DNS-Record die is gekoppeld aan de Listener altijd op in Azure 1200 seconden. U wilt dit beperken als u onder tijd beperking tijdens de migratie om te controleren of de clients hun DNS bijwerken met het bijgewerkte IP-adres voor de listener. U kunt zien en de configuratie wijzigen door het dumpen van een van de configuratie van de VNN:

```powershell
$AGName = "myProductionAG"
$ListenerName = "Mylistener"
#Look at HostRecordTTL
Get-ClusterResource $ListenerName| Get-ClusterParameter

#Set HostRecordTTL Examples
Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120
```

> [!NOTE]
> Hoe lager de 'HostRecordTTL', een hogere mate van DNS-verkeer plaatsvindt.

##### <a name="client-application-settings"></a>Clientinstellingen voor de toepassing
Als de toepassing van uw SQL-client .net 4.5 ondersteunt SQLClient, dan hebt u kunt gebruiken ' MULTISUBNETFAILOVER = TRUE' trefwoord. Dit trefwoord moet worden toegepast, omdat u Hiermee sneller verbinding met SQL AlwaysOn-beschikbaarheidsgroep tijdens de failover. Het inventariseren via alle IP-adressen die zijn gekoppeld aan de Always On-listener parallel en een agressievere TCP-verbinding opnieuw proberen snelheid tijdens een failover uitvoert.

Zie voor meer informatie over de vorige instellingen [MultiSubnetFailover trefwoord en functies die zijn gekoppeld](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Zie ook [SqlClient-ondersteuning voor hoge beschikbaarheid, herstel na noodgevallen](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Stap 5: Instellingen van het clusterquorum
Als u wilt worden uitleven van ten minste één SQL Server niet actief op een tijdstip, moet u het clusterquorum-instelling wijzigen als File Share Witness (FSW) met twee knooppunten, moet u de Quorumconfiguratie Knooppuntmeerderheid toestaan en gebruikmaken van dynamische stem instellen , waardoor het een enkel knooppunt permanente blijven.

```powershell
Set-ClusterQuorum -NodeMajority  
```

Zie voor meer informatie over het beheren en configureren van het clusterquorum [configureren en beheren van het Quorum in een Windows Server 2012-failovercluster](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Stap 6: Bestaande eindpunten en ACL's ophalen

```powershell
#GET Endpoint info
Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
#GET ACL Rules for Each EP, this example is for the Always On Endpoint
Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  
```

Deze tekst in een bestand opslaan.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Stap 7: Failover-Partners en Replicatiemodi wijzigen
Als u meer dan twee SQL-Servers hebt, moet u de failover van een andere secundaire in een andere domeincontroller of on-premises wijzigen in 'Synchrone' en er een automatische failover-Partner (AFP), is dit zodat u HA onderhouden terwijl u wijzigingen wilt aanbrengen. U kunt dit doen via TSQL van wijzigen via SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Stap 8: Secundaire virtuele machine van de cloudservice verwijderen
U moet eerst het secundaire knooppunt van een cloud migreren houden. Als dit knooppunt momenteel primair is, moet u een handmatige failover starten.

```powershell
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
```

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 9: Schijf opslaan in cache-instellingen in CSV-bestand wijzigen en opslaan
Voor gegevensvolumes, moeten deze worden ingesteld op alleen-lezen.

Voor TLOG volumes, moeten deze worden ingesteld op NONE.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Stap 10: Kopieer de VHD 's

```powershell
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
```


U kunt de kopiestatus van de VHD's naar de Premium Storage-account controleren:

```powershell
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
```

![Appendix8][18]

Wacht totdat deze worden geregistreerd als geslaagd.

Voor informatie over afzonderlijke blobs:

```powershell
Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext
```

#### <a name="step-11-register-os-disk"></a>Stap 11: Registreren van de schijf met besturingssysteem

```powershell
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
```

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Stap 12: Secundaire importeren in de nieuwe service in de cloud
De onderstaande code gebruikt ook de optie toegevoegd hier kunt u de machine importeren en het retainable VIP gebruiken.

```powershell
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
```

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Stap 13: ILB maken op het nieuwe Cloud Svc Load toevoegen met gelijke taakverdeling eindpunten en ACL's

```powershell
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
```

#### <a name="step-14-update-always-on"></a>Stap 14: De update is altijd ingeschakeld

```powershell
#Code to be executed on a Cluster Node
$ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
$newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

$AGName = "myProductionAG"
$ListenerName = "Mylistener"


Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

#set dependency and NETBIOS, then remove old IP address

#set NETBIOS, then remove old IP address
Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

#set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

#Make sure no static records in DNS
```

![Appendix9][19]

Nu de oude IP-adres van de cloudservice verwijderen.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Stap 15: DNS-updates controleren
Nu moet u DNS-Servers in uw SQL Server client netwerken controleren en zorg ervoor dat de extra hostrecord voor de toegevoegde IP-adres is toegevoegd als u een cluster. Als deze DNS-servers niet hebt bijgewerkt, kunt u een DNS-Zone-overdracht afdwingen en ervoor te zorgen dat de clients in er subnet zijn kunnen omzetten in beide altijd op IP-adressen, dit is dus u hoeft niet te wachten op automatische DNS-replicatie.

#### <a name="step-16-reconfigure-always-on"></a>Stap 16: Configureer altijd ingeschakeld
Op dit moment wachten u tot het secundaire knooppunt dat is gemigreerd volledig opnieuw worden gesynchroniseerd met het on-premises-knooppunt en schakel over naar het knooppunt synchrone replicatie en geef deze de AFP.  

#### <a name="step-17-migrate-second-node"></a>Stap 17: Tweede knooppunt migreren

```powershell
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
```

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 18: Schijf opslaan in cache-instellingen in CSV-bestand wijzigen en opslaan
Voor gegevensvolumes, moeten de cache-instellingen worden ingesteld op alleen-lezen.

Voor TLOG volumes, moeten de cache-instellingen worden ingesteld op NONE.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Stap 19: Maak nieuwe onafhankelijke Storage-Account voor secundair knooppunt

```powershell
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
```

#### <a name="step-20-copy-vhds"></a>Stap 20: Kopieer de VHD 's

```powershell
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
```

U kunt de status van de VHD-kopie controleren voor alle VHD's:

```powershell
ForEach ($disk in $diskobjects)
{
    $lun = $disk.Lun
    $vhdname = $disk.vhdname
    $cacheoption = $disk.HostCaching
    $disklabel = $disk.DiskLabel
    $diskName = $disk.DiskName

    $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
}
```

![Appendix12][22]

Wacht totdat deze worden geregistreerd als geslaagd.

Voor informatie over afzonderlijke blobs:

```powershell
#Check induvidual blob status
Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2
```

#### <a name="step-21-register-os-disk"></a>Stap 21: Registreren van de schijf met besturingssysteem

```powershell
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
```

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Stap 22: Toevoegen van Load Balanced eindpunten en ACL's

```powershell
#Endpoints
$epname="sqlIntEP"
$prot="tcp"
$locport=1433
$pubport=1433
Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


#STOP!!! CHECK in the Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

#SET ACLs or Azure Network Security Groups & Windows FWs

#http://msdn.microsoft.com/library/azure/dn495192.aspx
```

#### <a name="step-23-test-failover"></a>Stap 23: Testfailover
Wachten op het gemigreerde knooppunt om te synchroniseren met de on-premises AlwaysOn-knooppunt. Plaats deze in de modus voor synchrone replicatie en wacht totdat deze wordt gesynchroniseerd. Vervolgens failover van on-premises naar het eerste knooppunt gemigreerd, wordt de AFP. Zodra dat heeft gewerkt, wijzigt u het laatste knooppunt van de gemigreerde in de AFP.

U moet testen failovers tussen alle knooppunten en hoewel chaos-tests om te zorgen dat werk failovers als verwacht en in een tijdige manor worden uitgevoerd.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Stap 24 uur per dag: Instellingen van het clusterquorum teruggeplaatst / DNS TTL / Failover Pntrs / synchronisatie-instellingen
##### <a name="adding-ip-address-resource-on-same-subnet"></a>IP-Adresresource op hetzelfde Subnet toevoegen
Als u slechts twee SQL-Servers hebt en wilt migreren naar een nieuwe cloudservice, maar houd ze op hetzelfde subnet wilt, kunt u voorkomen de listener offline nemen om te verwijderen van het oorspronkelijke altijd op IP-adres en het nieuwe IP-adres toevoegen. Als u de virtuele machines naar een ander subnet migreert, hoeft u niet om dit te doen omdat er een extra clusternetwerk die verwijst naar dat subnet.

Wanneer u naar de gemigreerde secundaire gebracht en in de nieuwe bron-IP-adres voor de nieuwe cloudservice voordat de failover de bestaande primaire wordt toegevoegd, kunt u deze stappen in het failover-Clusterbeheer moet nemen:

Als u wilt toevoegen in IP-adres, Zie de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), stap 14.

1. Voor de resource van het huidige IP-adres, de mogelijke eigenaar aan 'Bestaande primaire SQL Server', in het voorbeeld 'dansqlams4' te wijzigen:

    ![Appendix13][23]
2. Voor de nieuwe bron-IP-adres, de mogelijke eigenaar 'Gemigreerd secundaire SQL Server', in het voorbeeld 'dansqlams5' te wijzigen:

    ![Appendix14][24]
3. Als deze optie is ingesteld, kunt u failover en wanneer het laatste knooppunt is gemigreerd de mogelijke eigenaars moeten worden bewerkt, zodat dit knooppunt wordt toegevoegd als een mogelijke eigenaar:

    ![Appendix15][25]

## <a name="additional-resources"></a>Aanvullende resources
* [Azure Premium Storage](../premium-storage.md)
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
