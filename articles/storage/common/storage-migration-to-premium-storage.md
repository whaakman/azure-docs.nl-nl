---
title: Migreren virtuele machines naar Azure Premium-opslag | Microsoft Docs
description: Uw bestaande virtuele machines migreren naar Azure Premium-opslag. Premium-opslag biedt ondersteuning voor schijven voor hoge prestaties, lage latentie voor I/O-intensieve werkbelastingen die worden uitgevoerd op Azure Virtual Machines.
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: ca893f87b155a92c457e3bf6d9d39aaf86bf5fb3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a>Migreren naar Azure Premium-opslag (niet-beheerde schijven)

> [!NOTE]
> Dit artikel wordt beschreven hoe u migreert van een virtuele machine die gebruikmaakt van niet-beheerde standaardschijven voor een virtuele machine die gebruikmaakt van niet-beheerde premium-schijven. U wordt aangeraden dat u Azure beheerd schijven gebruikt voor nieuwe virtuele machines en u uw vorige niet-beheerde schijven converteren naar beheerde schijven. Beheerde schijven ingang onderliggende storage-accounts, zodat u niet te hoeft. Zie voor meer informatie onze [schijven overzicht beheerde](../../virtual-machines/windows/managed-disks-overview.md).
>

Azure Premium-opslag biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines met I/O-intensieve werkbelastingen. U kunt profiteren van de snelheid en prestaties van deze schijven nemen door te migreren van uw toepassing VM schijven naar Azure Premium-opslag.

Het doel van deze handleiding is om nieuwe gebruikers van betere Azure Premium-opslag voorbereiden voor een goede overgang van hun huidige systeem naar Premium-opslag. De handleiding biedt drie van de belangrijkste onderdelen van dit proces:

* [Plannen voor de migratie naar Premium-opslag](#plan-the-migration-to-premium-storage)
* [Voorbereiden en virtuele harde schijven (VHD's) te kopiëren naar Premium-opslag](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Premium-opslag met van Azure virtuele Machine maken](#create-azure-virtual-machine-using-premium-storage)

U kunt virtuele machines migreren van andere platforms naar Azure Premium-opslag of migreren van bestaande Azure-virtuele machines van de Standard-opslag naar Premium-opslag. Deze handleiding worden de stappen beschreven voor beide twee scenario's. Volg de stappen die zijn opgegeven in de relevante sectie afhankelijk van uw scenario.

> [!NOTE]
> U vindt een overzicht van functies en prijzen van Premium-opslag in Premium-opslag: [krachtige opslag voor Azure Virtual Machine-werkbelasting](storage-premium-storage.md). Het is raadzaam om een hoge IOPS naar Azure Premium-opslag voor de beste prestaties voor uw toepassing vereisen schijf voor de virtuele machine migreren. Als de schijf niet hoge IOPS vereist, kunt u kosten beperken door handhaven standaardopslag die schijfgegevens voor virtuele machine op de harde schijven (HDD's) in plaats van SSD's opslaat.
>

De migratie voltooien in zijn geheel moet mogelijk extra acties vóór en na de stappen in deze handleiding. Voorbeelden zijn virtuele netwerken of eindpunten configureren of u code wijzigingen aanbrengt in de toepassing zelf waarin u mogelijk de enige downtime in uw toepassing. Deze acties zijn uniek voor elke toepassing en u ze samen met de stappen in deze handleiding voor de volledige overgang naar Premium-opslag zo weinig mogelijk moet voltooien.

## <a name="plan-the-migration-to-premium-storage"></a>Plannen voor de migratie naar Premium-opslag
In deze sectie zorgt ervoor dat u bent klaar om de migratie stappen in dit artikel helpt u bij het maken van de beste beslissing op schijf en VM-typen.

### <a name="prerequisites"></a>Vereisten
* U moet een Azure-abonnement. Als u niet hebt, kunt u één maand [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) abonnement of gaat u naar [prijzen van Azure](https://azure.microsoft.com/pricing/) voor meer opties.
* Voor het uitvoeren van PowerShell-cmdlets, moet u de Microsoft Azure PowerShell-module. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor het installatiepunt en de installatie-instructies.
* Wanneer u van plan bent te gebruiken van Azure VM's uitgevoerd op de Premium-opslag, moet u de compatibele virtuele machines voor een Premium-opslag gebruiken. Standaard- en Premium-opslag-schijven kunt u met Premium-opslag kunnen virtuele machines. Premium-schijven voor opslag zijn in de toekomst meer VM-typen beschikbaar. Zie voor meer informatie over alle beschikbare virtuele machine van Azure schijftypen en groottes [grootten voor virtuele machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [grootten voor Cloudservices](../../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Overwegingen
Een virtuele machine van Azure ondersteunt meerdere schijven met Premium-opslag koppelen zodat uw toepassingen kunnen maximaal 256 TB aan opslag per VM hebben. Met de Premium-opslag, kunnen uw toepassingen 80.000 IOP's (i/o-bewerkingen per seconde) per VM en 2000 MB per tweede schijfdoorvoer per virtuele machine met zeer lage latentie voor leesbewerkingen bereiken. Hebt u opties in tal van virtuele machines en de schijven. Deze sectie is om te zoeken naar een optie die het beste past bij uw workload.

#### <a name="vm-sizes"></a>Formaten van virtuele machines
De Azure VM-grootte specificaties worden vermeld in [grootten voor virtuele machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bekijk de prestatiekenmerken van virtuele machines die geschikt is voor Premium-opslag en kiest u de meest geschikte VM-grootte die het beste past bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het verkeer van de schijf.

#### <a name="disk-sizes"></a>Schijfformaten
Er zijn vijf typen schijven die kunnen worden gebruikt met uw virtuele machine en elke principal heeft bepaalde IOPs en doorvoerlimieten limieten. In overweging nemen deze limieten bij het kiezen van het schijftype voor de virtuele machine op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek worden geladen.

| Premium-schijven Type  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Schijfgrootte           | 128 GB| 512 GB| 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 
| IOP's per schijf       | 500   | 2300  | 5000           | 7500           | 7500           | 
| Doorvoer per schijf | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde |

Bepalen of extra gegevensschijven nodig zijn voor uw virtuele machine zijn afhankelijk van uw workload. U kunt meerdere permanente gegevensschijven koppelen aan uw virtuele machine. Indien nodig, kunt u op de schijven te verhogen van de capaciteit en prestaties van het volume stripe. (Zie Wat is er schijf Striping [hier](storage-premium-storage-performance.md#disk-striping).) Als u Premium-opslag gegevensschijven met stripe [opslagruimten][4], moet u deze configureren met één kolom voor elke schijf die wordt gebruikt. Anders wordt de algehele prestaties van de striped volumes mogelijk lager dan verwacht vanwege ongelijke verdeling van het verkeer op de schijven. Voor Linux VM's kunt u de *mdadm* hulpprogramma dezelfde bereiken. Zie het artikel [Software-RAID configureren op Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

#### <a name="storage-account-scalability-targets"></a>Schaalbaarheidsdoelen van Storage-account
Premium-opslagaccounts hebben het volgende schaalbaarheidsdoelen naast de [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md). Als uw toepassingsvereisten de schaalbaarheidsdoelen van een enkele opslagaccount overschrijdt, bouwen van uw toepassing gebruik van meerdere opslagaccounts en partitioneren van uw gegevens over de storage-accounts.

| Capaciteit van de totale Account | Totale bandbreedte voor een Account met lokaal redundante opslag |
|:--- |:--- |
| Schijf capaciteit: 35TB<br />Momentopname maken van capaciteit: 10 TB |Maximaal 50 gigabits per seconde voor inkomend en uitgaand |

Bekijk voor meer informatie op Premium-opslag-specificaties [Scalability and Performance Targets wanneer u Premium-opslag](storage-premium-storage.md#scalability-and-performance-targets).

#### <a name="disk-caching-policy"></a>Het beleid voor schijf
Beleid voor de schijfcache is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen-schrijven* voor de Premium-schijf is gekoppeld aan de VM. Deze configuratieinstelling wordt aanbevolen de optimale prestaties voor uw toepassing IOs bereiken. Voor schijven schrijven zware of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden), uitschakelen schijfcache, zodat u kunt betere prestaties bereiken. De cache-instellingen voor bestaande gegevensschijven kunnen worden bijgewerkt met behulp van [Azure Portal](https://portal.azure.com) of de *- HostCaching* parameter van de *Set AzureDataDisk* cmdlet.

#### <a name="location"></a>Locatie
Kies een locatie waar Azure Premium-opslag beschikbaar is. Zie [Azure-Services per regio](https://azure.microsoft.com/regions/#services) voor actuele informatie over beschikbare locaties. Virtuele machines zich in dezelfde regio bevinden als het opslagaccount dat slaat de schijven voor de virtuele machine u veel betere prestaties krijgt dan als ze zich in afzonderlijke regio's.

#### <a name="other-azure-vm-configuration-settings"></a>Andere Azure VM-configuratie-instellingen
Wanneer u een virtuele machine in Azure maakt, wordt u gevraagd om bepaalde VM-instellingen te configureren. Denk eraan dat enkele instellingen vast voor de levensduur van de virtuele machine, terwijl u kunt wijzigen of anderen later toevoegen. Deze configuratie-instellingen voor virtuele machine van Azure controleren en zorg ervoor dat deze overeenkomen met de vereisten van uw werkbelasting op de juiste wijze zijn geconfigureerd.

### <a name="optimization"></a>Optimalisatie
[Azure Premium-opslag: Ontwerpen voor hoge prestaties](storage-premium-storage-performance.md) biedt richtlijnen voor het bouwen van krachtige toepassingen met behulp van Azure Premium-opslag. Volg de richtlijnen in combinatie met aanbevolen procedures voor prestaties van toepassing op de technologieën die worden gebruikt door uw toepassing.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Voorbereiden en virtuele harde schijven (VHD's) te kopiëren naar Premium-opslag
De volgende sectie bevat richtlijnen voor het voorbereiden van VHD's van uw virtuele machine en VHD's kopiëren naar Azure Storage.

* [Scenario 1: 'Ik ben migreren van bestaande Azure virtuele machines naar Azure Premium-opslag."](#scenario1)
* [Scenario 2: 'Ik ben migreren VM's van andere platforms naar Azure Premium-opslag."](#scenario2)

### <a name="prerequisites"></a>Vereisten
Als u de VHD's voorbereiden voor migratie, hebt u het volgende nodig:

* Een Azure-abonnement, een opslagaccount en een container in dat storage-account waarmee u uw VHD kunt kopiëren. Houd er rekening mee dat het doelopslagaccount kan een Standard of Premium-opslag-account, afhankelijk van uw behoeften.
* Een hulpprogramma voor de VHD generalize als u wilt maken van meerdere exemplaren van de virtuele machine uit. Bijvoorbeeld: sysprep voor Windows of virt-sysprep voor Ubuntu.
* Een hulpprogramma voor het uploaden van het VHD-bestand naar de Storage-account. Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md) of gebruik een [Azure Opslagverkenner](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Deze handleiding beschrijft de VHD met het AzCopy-hulpprogramma kopieert.

> [!NOTE]
> Als u de optie synchrone kopiëren met AzCopy, kiest voor optimale prestaties, kopieert u uw VHD door een van deze hulpprogramma's van een Azure-virtuele machine die zich in dezelfde regio bevinden als het doelopslagaccount. Als u een VHD van een Azure-virtuele machine in een andere regio kopieert, kan de prestaties van uw trager worden.
>
> Voor het kopiëren een grote hoeveelheid gegevens over beperkte bandbreedte, kunt u overwegen [via de Azure Import/Export-service gegevens overdraagt naar Blob Storage](../storage-import-export-service.md); Hiermee kunt u uw gegevens overbrengen back-upfunctie harde schijven naar een Azure-datacenter. U kunt de Azure Import/Export-service gebruiken om gegevens te kopiëren naar een standard-opslagaccount alleen. Nadat de gegevens uw account standard-opslag is, kunt u ofwel de [kopie Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) of AzCopy voor de gegevensoverdracht naar uw premium-opslagaccount.
>
> Houd er rekening mee dat Microsoft Azure biedt alleen ondersteuning voor VHD-bestanden van vaste grootte. VHDX-bestanden of dynamische virtuele harde schijven worden niet ondersteund. Als u een dynamische VHD hebt, kunt u deze converteren naar vaste grootte met behulp van de [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.
>
>

### <a name="scenario1"></a>Scenario 1: 'Ik ben migreren van bestaande Azure virtuele machines naar Azure Premium-opslag."
Als u bestaande Azure-virtuele machines migreert, stop de virtuele machine, virtuele harde schijven per het type VHD die u wilt voorbereiden en kopieer de VHD met AzCopy of PowerShell.

De virtuele machine moet volledig omlaag voor het migreren van een oude toestand. Er zijn een uitvaltijd totdat de migratie is voltooid.

#### <a name="step-1-prepare-vhds-for-migration"></a>Step 1. VHD's voorbereiden voor migratie
Als u bestaande Azure-virtuele machines naar Premium-opslag migreert, mogelijk uw VHD:

* De installatiekopie van een gegeneraliseerde besturingssysteem
* Een unieke besturingssysteemschijf
* Een gegevensschijf

Hieronder doorlopen we deze 3 scenario's voor het voorbereiden van uw VHD.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Een gegeneraliseerde besturingssysteem-VHD gebruiken voor het maken van meerdere VM-exemplaren
Als u een VHD die wordt gebruikt voor het maken van meerdere algemene Azure VM-exemplaren uploaden wilt, moet u eerst de VHD met een hulpprogramma sysprep generalize. Dit geldt voor een VHD die on-premises of in de cloud. Sysprep verwijdert alle systeemspecifieke gegevens uit de VHD.

> [!IMPORTANT]
> Een momentopname of back-up van uw VM voordat deze het generaliseren. Sysprep uitgevoerd worden gestopt en toewijzing van de VM-instantie. Volg onderstaande stappen om sysprep een Windows-besturingssysteem-VHD. Houd er rekening mee dat de opdracht Sysprep uit te voeren, moet u de virtuele machine afgesloten. Zie voor meer informatie over Sysprep [Sysprep overzicht](http://technet.microsoft.com/library/hh825209.aspx) of [technische documentatie van Sysprep](http://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Open een opdrachtpromptvenster als administrator.
2. Voer de volgende opdracht Sysprep openen:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. Selecteer in het hulpprogramma voor systeemvoorbereiding, voer System Out-of-Box Experience (OOBE), schakel het selectievakje Generalize, selecteert u **afsluiten**, en klik vervolgens op **OK**, zoals wordt weergegeven in de onderstaande afbeelding. Sysprep wordt generalize van het besturingssysteem en het systeem afsluiten.

    ![][1]

Voor een VM Ubuntu virt-sysprep te gebruiken als u dezelfde. Zie [virt sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) voor meer informatie. Zie ook enkele van de open source [inrichten van Linux-servers software](http://www.cyberciti.biz/tips/server-provisioning-software.html) voor andere Linux-besturingssystemen.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Een unieke besturingssysteem-VHD gebruiken voor het maken van een enkel VM-exemplaar
Als u een toepassing die wordt uitgevoerd op de virtuele machine waarvoor u de specifieke gegevens van de machine hebt, niet generalize voor de VHD. Een niet-gegeneraliseerd VHD kan worden gebruikt voor het maken van een uniek exemplaar van de virtuele machine van Azure. Bijvoorbeeld, als u een domeincontroller op uw VHD hebt, maakt sysprep wordt uitgevoerd het niet effectief als een domeincontroller. Bekijk de toepassingen die worden uitgevoerd op uw virtuele machine en de impact van sysprep erop worden uitgevoerd voordat het generaliseren van de VHD.

##### <a name="register-data-disk-vhd"></a>Gegevensschijf VHD registreren
Hebt u gegevensschijven in Azure worden gemigreerd, moet u ervoor zorgen dat de virtuele machines die gebruikmaken van de gegevensschijven van deze worden afgesloten.

Volg de stappen die hieronder worden beschreven op de VHD naar Azure Premium-opslag te kopiëren en registreren als een ingerichte gegevensschijf.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. Het doel voor de VHD maken
Een opslagaccount voor het onderhouden van uw VHD's maken. Houd rekening met de volgende punten bij het plannen waar uw VHD's wordt opgeslagen:

* Het doel van Premium storage-account.
* De opslaglocatie voor de account moet hetzelfde als de Premium-opslag kunnen virtuele Azure-machines in de laatste fase wordt gemaakt. U kan kopiëren naar een nieuw opslagaccount of wilt gebruiken hetzelfde opslagaccount op basis van uw behoeften.
* Kopieer en sla de opslagaccountsleutel van het opslagaccount voor de bestemming voor de volgende fase.

Voor gegevensschijven, kunt u een aantal gegevensschijven in een standard-opslagaccount (bijvoorbeeld schijven die koelervoorbeeld opslag) behouden, maar wij raden u alle gegevens voor de werkbelasting van de productie te gebruiken van premium-opslag te verplaatsen.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Stap 3. Kopieer de VHD met AzCopy of PowerShell
U moet uw container pad en de opslagruimte accountsleutel vinden voor het verwerken van een van deze twee opties. Container pad en de opslagruimte accountsleutel vindt u in **Azure Portal** > **opslag**. De container-URL worden zoals 'https://myaccount.blob.core.windows.net/mycontainer/'.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Optie 1: Kopieer geen VHD met AzCopy (asynchrone kopiëren)
AzCopy gebruikt, kunt u eenvoudig uploaden de VHD via Internet. Dit kan tijd duren, afhankelijk van de grootte van de VHD's. Vergeet niet om te controleren limieten inkomend en uitgaand van het opslagaccount wanneer u deze optie. Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie.

1. Download en installeer AzCopy vanaf hier: [meest recente versie van AzCopy](http://aka.ms/downloadazcopy)
2. Open Azure PowerShell en Ga naar de map waarin AzCopy is geïnstalleerd.
3. Gebruik de volgende opdracht om te kopiëren van de VHD-bestand van 'Bron' naar 'Bestemming'.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Voorbeeld:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    Hier volgen beschrijvingen van de parameters gebruikt in de AzCopy-opdracht:

   * **/ Bron:  *&lt;bron&gt;:***  locatie van de map of URL van de opslagcontainer die de VHD bevat.
   * **/ SourceKey:  *&lt;bron accountsleutel&gt;:***  opslagaccountsleutel van het opslagaccount van de bron.
   * **/ Dest:  *&lt;bestemming&gt;:***  URL van de opslagcontainer voor het kopiëren van de VHD.
   * **/ DestKey:  *&lt;dest accountsleutel&gt;:***  opslagaccountsleutel van het doelopslagaccount.
   * **/ Patroon:  *&lt;bestandsnaam&gt;:***  Geef de bestandsnaam van de VHD te kopiëren.

Hulpprogramma voor meer informatie over het gebruik van AzCopy, Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Optie 2: Kopieer geen VHD met PowerShell (Synchronized kopiëren)
U kunt ook het VHD-bestand met de PowerShell-cmdlet Start-AzureStorageBlobCopy kopiëren. Gebruik de volgende opdracht op Azure PowerShell te kopiëren van de VHD. Vervang de waarden in <> met overeenkomende waarden uit de bron- en storage-account. Voor het gebruik van deze opdracht moet u een zogenaamd VHD's in uw doelopslagaccount hebben. Als de container niet bestaat, maken voordat de opdracht uit te voeren.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Voorbeeld:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>Scenario 2: 'Ik ben migreren VM's van andere platforms naar Azure Premium-opslag."
Als u VHD van niet - Azure Cloud-opslag naar Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Het pad van de volledige broncode van de lokale map waar de VHD handige wordt opgeslagen en vervolgens met behulp van AzCopy te uploaden naar Azure Storage.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Step 1. Exporteren van VHD naar een lokale map
##### <a name="copy-a-vhd-from-aws"></a>Kopieer geen VHD van AWS
1. Als u van AWS gebruikmaakt, moet u het exemplaar EC2 exporteren naar een VHD in een Amazon S3-bucket. Volg de stappen beschreven in de Amazon-documentatie voor Amazon EC2 exemplaren voor het installeren van het hulpprogramma EC2 Amazon-opdrachtregelinterface (CLI) en voer de opdracht create-exemplaar-export-taak het exemplaar EC2 exporteren naar een VHD-bestand exporteren. Zorg ervoor dat u **VHD** voor de schijf &#95; INSTALLATIEKOPIE &#95; INDELING variabele bij het uitvoeren van de **-exemplaar-export-taak maken** opdracht. Het geëxporteerde VHD-bestand wordt opgeslagen in de Amazon S3-bucket die u tijdens dat proces opgeeft.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. Download het VHD-bestand van de S3-bucket. Selecteer vervolgens het VHD-bestand **acties** > **downloaden**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopieer geen VHD van andere niet-Azure-cloud
Als u VHD van niet - Azure Cloud-opslag naar Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Kopieer het pad van de volledige broncode van de lokale map waar de VHD wordt opgeslagen.

##### <a name="copy-a-vhd-from-on-premises"></a>Kopieer geen VHD van on-premises
Als u een VHD van een on-premises omgeving migreert, moet u de volledige broncode pad op waar de VHD is opgeslagen. Het bronpad kan een server of bestandsshare worden.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. Het doel voor de VHD maken
Een opslagaccount voor het onderhouden van uw VHD's maken. Houd rekening met de volgende punten bij het plannen waar uw VHD's wordt opgeslagen:

* Het doel storage-account kan worden standaard of premium-opslag, afhankelijk van uw vereisten voor toepassingsimplementatie.
* Regio van het opslagaccount moet hetzelfde zijn als Premium-opslag kunnen Azure virtuele machines in de laatste fase wordt gemaakt. U kan kopiëren naar een nieuw opslagaccount of wilt gebruiken hetzelfde opslagaccount op basis van uw behoeften.
* Kopieer en sla de opslagaccountsleutel van het opslagaccount voor de bestemming voor de volgende fase.

Wij raden u alle gegevens voor de werkbelasting van de productie te gebruiken van premium-opslag te verplaatsen.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Stap 3. De VHD te uploaden naar Azure Storage
Nu dat u uw VHD in de lokale directory hebt, kunt u AzCopy of AzurePowerShell het VHD-bestand uploaden naar Azure Storage. Beide opties vindt u hier:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Optie 1: Met behulp van Azure PowerShell Add-AzureVhd het .vhd-bestand te uploaden

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Een voorbeeld <Uri> mogelijk ***'https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd'***. Een voorbeeld <FileInfo> mogelijk ***'C:\path\to\upload.vhd'***.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Optie 2: Upload het .vhd-bestand met behulp van AzCopy
AzCopy gebruikt, kunt u eenvoudig uploaden de VHD via Internet. Dit kan tijd duren, afhankelijk van de grootte van de VHD's. Vergeet niet om te controleren limieten inkomend en uitgaand van het opslagaccount wanneer u deze optie. Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie.

1. Download en installeer AzCopy vanaf hier: [meest recente versie van AzCopy](http://aka.ms/downloadazcopy)
2. Open Azure PowerShell en Ga naar de map waarin AzCopy is geïnstalleerd.
3. Gebruik de volgende opdracht om te kopiëren van de VHD-bestand van 'Bron' naar 'Bestemming'.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Voorbeeld:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    Hier volgen beschrijvingen van de parameters gebruikt in de AzCopy-opdracht:

   * **/ Bron:  *&lt;bron&gt;:***  locatie van de map of URL van de opslagcontainer die de VHD bevat.
   * **/ SourceKey:  *&lt;bron accountsleutel&gt;:***  opslagaccountsleutel van het opslagaccount van de bron.
   * **/ Dest:  *&lt;bestemming&gt;:***  URL van de opslagcontainer voor het kopiëren van de VHD.
   * **/ DestKey:  *&lt;dest accountsleutel&gt;:***  opslagaccountsleutel van het doelopslagaccount.
   * **/ BlobType: pagina:** geeft aan dat de bestemming een pagina-blob.
   * **/ Patroon:  *&lt;bestandsnaam&gt;:***  Geef de bestandsnaam van de VHD te kopiëren.

Hulpprogramma voor meer informatie over het gebruik van AzCopy, Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD
U kunt ook een VHD uploaden naar uw storage-account met behulp van een van de volgende wijzen:

* [API voor Azure Storage kopiëren-Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Azure Storage Explorer uploaden van BLOB 's](https://azurestorageexplorer.codeplex.com/)
* [Opslag voor importeren/exporteren Service REST API-verwijzing](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> U kunt het beste Import/Export-Service gebruikt als de geschatte tijd uploaden is langer dan zeven dagen. U kunt [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) om in te schatten het tijdstip in gegevenseenheid grootte en de overdracht.
>
> Import/Export kan worden gebruikt om te kopiëren naar een standard-opslagaccount. U wilt kopiëren van de standard-opslag naar premium storage-account met behulp van een hulpprogramma zoals AzCopy.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Azure virtuele machines met Premium-opslag maken
Nadat de VHD is geüpload of gekopieerd naar het gewenste opslagaccount, volg de instructies in deze sectie om de VHD registreren als een installatiekopie van het besturingssysteem, of de besturingssysteemschijf afhankelijk van uw scenario en maak vervolgens een VM-instantie uit. De gegevensschijf VHD kan worden gekoppeld aan de virtuele machine nadat deze is gemaakt.
Een migratie voorbeeldscript wordt verstrekt aan het einde van deze sectie. Dit eenvoudige script komt niet overeen met alle scenario's. Mogelijk moet u het script moet overeenkomen met uw specifieke scenario bijwerken. Als dit script wordt toegepast op uw scenario Zie onderstaande [A migratie voorbeeldscript](#a-sample-migration-script).

### <a name="checklist"></a>Controlelijst
1. Wacht totdat alle VHD-schijven kopiëren is voltooid.
2. Zorg ervoor dat de dat Premium-opslag is beschikbaar in de regio die u naar migreert.
3. Bepaal de nieuwe VM-reeks die u wilt gebruiken. Dit moet een Premium-opslag die geschikt zijn en de grootte moet hangt af van de beschikbaarheid in regio en op basis van uw behoeften.
4. Bepaal de exacte VM-grootte die u wilt gebruiken. VM-grootte moet groot genoeg zijn om het aantal gegevensschijven dat u hebt ondersteunen. Bijvoorbeeld Als u 4 gegevensschijven hebt, moet de virtuele machine 2 of hoger kernen hebben. Houd ook rekening met verwerkingskracht, geheugen en netwerkbandbreedte moet.
5. Premium-opslagaccount maken in de doelregio. Dit is het account dat u voor de nieuwe virtuele machine gebruiken wilt.
6. De huidige VM details die handig zijn, met inbegrip van de lijst van schijven en de bijbehorende VHD-blobs hebben.

Bereid uw toepassing uitvaltijd. U hebt hiervoor een schone migratie stoppen van de verwerking in het huidige systeem. Alleen dan kunt u dit downloaden naar consistente status die u naar het nieuwe platform migreren kunt. Duur van uitvaltijd is afhankelijk van de hoeveelheid gegevens op de schijven om te migreren.

> [!NOTE]
> Als u een Azure Resource Manager VM vanaf een speciale VHD schijf maakt, raadpleegt u [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) voor de implementatie van Resource Manager-VM met bestaande schijf.
>
>

### <a name="register-your-vhd"></a>Registreren van uw VHD
Een virtuele machine van besturingssysteem-VHD maken of een gegevensschijf koppelen aan een nieuwe virtuele machine, moet u deze eerst registreren. Voer onderstaande stappen, afhankelijk van uw VHD-scenario.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Besturingssysteem-VHD voor het maken van meerdere exemplaren van de virtuele machine van Azure gegeneraliseerd
Nadat gegeneraliseerde installatiekopie van het besturingssysteem VHD is geüpload naar het opslagaccount, registreert u dit als een **Azure VM-installatiekopie** zodat u een of meer VM-exemplaren van deze maken kunt. Gebruik de volgende PowerShell-cmdlets voor het registreren van uw VHD als de installatiekopie van een besturingssysteem van de Azure-virtuele machine. Geef de URL van de volledige container waarnaar de VHD is gekopieerd.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Kopiëren en opslaan van de naam van deze nieuwe Azure VM-installatiekopie. In het bovenstaande voorbeeld is het *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unieke besturingssysteem-VHD voor het maken van één exemplaar van de virtuele machine in Azure
Nadat de unieke OS VHD is geüpload naar het opslagaccount, registreert u dit als een **Azure Besturingssysteemschijf** zodat u een VM-exemplaar van deze maken kunt. Deze PowerShell-cmdlets gebruiken om uw VHD registreren als een Besturingssysteemschijf Azure. Geef de URL van de volledige container waarnaar de VHD is gekopieerd.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Kopiëren en opslaan van de naam van deze nieuwe Azure OS-schijf. In het bovenstaande voorbeeld is het *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Gegevens schijf VHD moet worden gekoppeld aan de nieuwe virtuele machine van Azure-exemplaren
Nadat de gegevensschijf VHD is geüpload naar storage-account, registreren als een Azure-gegevensschijf zodat deze kan worden gekoppeld aan uw nieuwe DS-serie, DSv2 reeks of GS-serie Azure VM-instantie.

Deze PowerShell-cmdlets gebruiken om uw VHD registreren als een Azure-gegevensschijf. Geef de URL van de volledige container waarnaar de VHD is gekopieerd.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Kopiëren en opslaan van de naam van deze nieuwe Azure-gegevensschijf. In het bovenstaande voorbeeld is het *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Maak een geschikt VM voor Premium-opslag
Eenmaal de installatiekopie van het besturingssysteem of besturingssysteemschijf zijn geregistreerd, maakt u een nieuwe Active Directory-serie, DSv2-serie- of GS-serie VM. U gaat gebruiken de installatiekopie van besturingssysteem of de naam van een besturingssysteem schijf die u hebt geregistreerd. Selecteer het VM-type van de laag Premium-opslag. In onderstaand voorbeeld gebruiken we de *Standard_DS2* VM-grootte.

> [!NOTE]
> De schijfgrootte om ervoor te zorgen dat deze overeenkomt met de capaciteit en prestatie-eisen en de beschikbare Azure schijfgrootte bijwerken.
>
>

Volg de stapsgewijze PowerShell-cmdlets hieronder om aan te maken van de nieuwe virtuele machine. Stel eerst de algemene parameters:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

Maak eerst een cloudservice waarin u uw nieuwe virtuele machines worden gehost.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Maak vervolgens de virtuele machine van Azure-instantie van de installatiekopie van het besturingssysteem of de OS-schijf die u hebt geregistreerd, afhankelijk van uw scenario.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Besturingssysteem-VHD voor het maken van meerdere exemplaren van de virtuele machine van Azure gegeneraliseerd
Maken van een of meer nieuwe DS reeks Azure VM-exemplaren met de **installatiekopie van het besturingssysteem Azure** die u hebt geregistreerd. Geef de naam van deze installatiekopie van het besturingssysteem in de VM-configuratie bij het maken van nieuwe virtuele machine, zoals hieronder wordt weergegeven.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unieke besturingssysteem-VHD voor het maken van één exemplaar van de virtuele machine in Azure
Maak een nieuw DS reeks Azure VM exemplaar met de **Besturingssysteemschijf Azure** die u hebt geregistreerd. Bij het maken van de nieuwe virtuele machine zoals hieronder wordt weergegeven, moet u deze naam Besturingssysteemschijf opgeven in de VM-configuratie.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Geef andere Azure VM-informatie, zoals een cloudservice, regio, storage-account, beschikbaarheidsset en het beleid. Houd er rekening mee dat de VM-instantie geplaatst met bijbehorende besturingssysteem of gegevensschijven, worden moet zodat de geselecteerde cloud-service, regio en storage-account op dezelfde locatie als de onderliggende virtuele harde schijven van deze schijven zijn moet.

### <a name="attach-data-disk"></a>Een gegevensschijf koppelen
Ten slotte, als u de gegevensschijf VHD's hebt geregistreerd, koppel ze aan de nieuwe Premium-opslag compatibele Azure VM.

Gebruikt u de volgende PowerShell-cmdlet gegevensschijf koppelen aan de nieuwe virtuele machine en het cachebeleid opgeven. In onderstaand voorbeeld het cachebeleid is ingesteld op *ReadOnly*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Mogelijk zijn er extra stappen die nodig zijn voor ondersteuning van uw toepassing die niet worden gedekt door deze handleiding.
>
>

### <a name="checking-and-plan-backup"></a>Controleren en back-up plannen
Nadat de nieuwe virtuele machine actief en werkend is, toegang tot dit met behulp van dezelfde aanmeldings-id en wachtwoord is als de oorspronkelijke virtuele machine en controleer of dat alles werkt zoals verwacht. Alle instellingen, inclusief de striped volumes, zou aanwezig zijn in de nieuwe virtuele machine.

De laatste stap is het plannen van back-up en -onderhoudsschema voor de nieuwe virtuele machine op basis van de behoeften van de toepassing.

### <a name="a-sample-migration-script"></a>Een voorbeeldscript voor migratie
Als er meerdere virtuele machines te migreren, wordt de automation via PowerShell-scripts nuttig zijn. Hier volgt een voorbeeld van een script waarmee de migratie van een virtuele machine worden geautomatiseerd. Opmerking die onderstaand script is slechts een voorbeeld en er zijn enkele veronderstellingen over de huidige VM-schijven. Mogelijk moet u het script moet overeenkomen met uw specifieke scenario bijwerken.

Het uitgangspunten zijn:

* Klassieke Azure-VM's die u maakt.
* Uw bron OS schijven en de gegevensschijven van de bron zich in hetzelfde opslagaccount en in dezelfde container. Als de OS-schijven en de gegevensschijven niet op dezelfde plaats, kunt u AzCopy of Azure PowerShell te kopiëren van VHD's via de storage-accounts en containers. Raadpleeg de vorige stap: [kopie VHD met AzCopy of PowerShell](#copy-vhd-with-azcopy-or-powershell). Het bewerken van dit script om te voldoen aan uw scenario is een andere keuze, maar we raden via AzCopy of PowerShell nadat het is eenvoudiger en sneller.

Hieronder vindt u het automatiseringsscript. Tekst vervangen door uw gegevens en bijwerken van het script moet overeenkomen met uw specifieke scenario.

> [!NOTE]
> Met het bestaande script behoudt niet de netwerkconfiguratie van de bron-VM. U moet re-config de netwerkinstellingen op de gemigreerde virtuele machines.
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>Optimalisatie
Uw huidige configuratie van de virtuele machine kan worden aangepast specifiek werken goed met standaardschijven. Bijvoorbeeld, de prestaties verbeteren door het gebruik van veel schijven in striped volumes. Bijvoorbeeld, in plaats van 4 schijven afzonderlijk op Premium-opslag, kunt u mogelijk de kosten optimaliseren door één schijf. Optimalisaties zoals dit moet worden verwerkt op basis van geval tot geval en aangepaste stappen moeten worden uitgevoerd na de migratie. Merk ook op dit proces werkt niet goed voor databases en toepassingen die afhankelijk van de schijfindeling dat is gedefinieerd in de instellingen zijn.

##### <a name="preparation"></a>Voorbereiding
1. De eenvoudige migratie voltooid zoals beschreven in de vorige sectie. Optimalisaties worden uitgevoerd op de nieuwe virtuele machine na de migratie.
2. De nieuwe schijfgrootte die nodig zijn voor de configuratie van de geoptimaliseerde definiëren.
3. Toewijzing van de huidige schijven/volumes in de nieuwe schijf specificaties bepalen.

##### <a name="execution-steps"></a>De stappen worden uitgevoerd
1. De nieuwe schijven maken met de juiste grootte beschikbaar op de virtuele machine Premium-opslag.
2. Meld u aan de virtuele machine en kopieer de gegevens van het huidige volume aan de nieuwe schijf die is toegewezen aan dat volume. Doe dit voor de huidige volumes die moeten worden toegewezen aan een nieuwe schijf.
3. Vervolgens wijzigt u de toepassingsinstellingen overschakelen naar de nieuwe schijven en de oude volumes loskoppelen.

Voor het afstemmen van de toepassing voor betere prestaties van de schijf, raadpleegt u [toepassingsprestaties optimaliseren](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Migraties van toepassing
Databases en andere complexe toepassingen mogelijk speciale stappen zoals gedefinieerd door de aanbieder van de toepassing voor de migratie. Raadpleeg de documentatie van de desbetreffende toepassing. Bijvoorbeeld doorgaans databases kunnen worden gemigreerd door middel van de back-up en herstel.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor specifieke scenario's voor het migreren van virtuele machines:

* [Azure virtuele Machines tussen Opslagaccounts migreren](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Maken en een Windows Server-VHD uploaden naar Azure.](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Maakt en uploadt u een virtuele harde schijf met het Linux-besturingssysteem](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migreren van virtuele Machines van Amazon AWS naar Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Zie ook de volgende bronnen voor meer informatie over Azure Storage en Azure Virtual Machines:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Virtuele Machines in Azure](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: opslag met hoge prestaties voor de werkbelasting van virtuele Azure-machines](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
