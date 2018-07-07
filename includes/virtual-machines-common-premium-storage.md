---
title: bestand opnemen
description: bestand opnemen
services: storage
author: ramankumarlive
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: ramankum
ms.custom: include file
ms.openlocfilehash: 4c14bfbad58849acefdc8c3a5513f681aba84ab8
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37909915"
---
# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Krachtige Premium Storage en beheerde schijven voor virtuele machines
Azure Premium Storage voorziet in ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines (VM's) met invoer/uitvoer (I/O)-intensieve workloads. VM-schijven die gebruikmaken van Premium Storage worden gegevens opgeslagen op SSD-schijven (SSD's). Als u wilt profiteren van de snelheid en prestaties van premium storage-schijven, kunt u bestaande VM-schijven kunt migreren naar Premium Storage.

In Azure, kunt u meerdere premium-opslagschijven koppelen aan een virtuele machine. Uw toepassingen voor het gebruik van meerdere schijven biedt tot 256 TB opslagruimte per virtuele machine. Met Premium Storage kunnen uw toepassingen maar liefst 80.000 i/o-bewerkingen per seconde (IOPS) per VM en een schijfdoorvoer van 2000 MB per seconde (MB/s) per virtuele machine. Leesbewerkingen bieden u een zeer lage latentie.

Met Premium Storage biedt Azure de mogelijkheid tot echt lift-and-shift veeleisende bedrijfstoepassingen, zoals Dynamics AX-, Dynamics CRM-, Exchange Server-, SAP Business Suite- en SharePoint-farms naar de cloud. U kunt prestatie-intensieve databaseworkloads uitvoeren in toepassingen zoals SQL Server, Oracle, MongoDB, MySQL en Redis, die consistent hoge prestaties en lage latentie vereisen.

> [!NOTE]
> Voor de beste prestaties voor uw toepassing, wordt u aangeraden dat u een VM-schijf waarvoor hoge IOPS naar Premium Storage migreert. Als de schijf geen hoge IOPS is vereist, kunt u kosten beperken door deze te houden in standaard Azure Storage. Standard-opslag, worden schijf-VM-gegevens opgeslagen op hardeschijfstations (HDD's) in plaats van op SSD's.
> 

Azure biedt twee manieren om u te maken van premium storage-schijven voor virtuele machines:

* **Niet-beheerde schijven**

    De oorspronkelijke methode is het gebruik van niet-beheerde schijven. In een niet-beheerde schijf beheert u de storage-accounts die u gebruikt voor het opslaan van de virtuele harde schijf (VHD)-bestanden die met de VM-schijven overeenkomen. VHD-bestanden worden opgeslagen als pagina-blobs in Azure storage-accounts. 

* **Beheerde schijven**

    Als u ervoor kiest [Azure Managed Disks](../articles/virtual-machines/windows/managed-disks-overview.md), beheerd door de storage-accounts die u voor de VM-schijven gebruiken van Azure. Geeft u het schijftype (Premium of Standard) en de grootte van de schijf die u nodig hebt. Azure maakt en beheert de schijf voor u. U hebt geen zorgen te hoeven maken over het plaatsen van de schijven in meerdere opslagaccounts om ervoor te zorgen dat u binnen een de schaalbaarheidslimieten van voor uw storage-accounts blijven. Azure regelt die voor u.

Het is raadzaam dat u beheerde schijven, om te profiteren van de vele functies kiezen.

Aan de slag met Premium Storage [uw gratis Azure-account maken](https://azure.microsoft.com/pricing/free-trial/). 

Zie voor meer informatie over het migreren van uw bestaande virtuele machines naar Premium Storage [een Windows-VM van niet-beheerde schijven converteren naar beheerde schijven](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md) of [een Linux-VM van niet-beheerde schijven converteren naar managed disks](../articles/virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Premium Storage is beschikbaar in de meeste regio's. Voor de lijst met beschikbare regio's, Zie de rij voor **schijfopslag** in [Azure-producten beschikbaar per regio](https://azure.microsoft.com/regions/#services).
> 

## <a name="features"></a>Functies

Hier volgen enkele van de functies van Premium Storage:

* **Premium storage-schijven**

    Premium-opslag biedt ondersteuning voor VM-schijven die kunnen worden gekoppeld aan specifieke grootte-serie VM's. Premium Storage ondersteunt een groot aantal Azure-VM's. U hebt een ruime keuze aan zeven schijfgrootten: P4 (32 GB), P6 (64 GB), P10 (128 GB), P20 (512 GB), P30 (1024 GB), P40 (2048 GB), P50 (4095 GB). P4 en P6 schijfgrootten worden alleen nog ondersteund voor Managed Disks. De grootte van elke schijf heeft een eigen prestatiespecificaties. Afhankelijk van uw toepassingsvereisten kunt u een of meer schijven koppelen aan uw virtuele machine. We beschrijven de specificaties in meer detail in [schaalbaarheids- en prestatiedoelen van Premium Storage](#scalability-and-performance-targets).

* **Premium-pagina-blobs**

    Premium-opslag biedt ondersteuning voor pagina-blobs. Pagina-blobs gebruikt voor het opslaan van permanente, niet-beheerde schijven voor virtuele machines in Premium-opslag. In tegenstelling tot standaard Azure Storage, Premium Storage niet ondersteunen van blok-blobs, toevoeg-blobs, bestanden, tabellen of wachtrijen. Premium-pagina-blobs ondersteunen zes grootten van P10 P50 en P60 (8191GiB). P60 Premium-pagina-blob wordt niet ondersteund als VM-schijven worden gekoppeld. 

    Een object dat wordt geplaatst in een premium storage-account worden een pagina-blob. De pagina-blob uitgelijnd op een van de ondersteunde ingerichte grootte. Dit is de reden waarom een premium storage-account is niet bedoeld om te worden gebruikt voor het opslaan van kleine blobs.

* **Premium storage-account**

    Als u wilt gaan met Premium Storage, een premium storage-account voor niet-beheerde schijven te maken. In de [Azure-portal](https://portal.azure.com), voor het maken van een premium storage-account, kiest u de **Premium** prestatielaag. Selecteer de **lokaal redundante opslag (LRS)** replicatie-optie. U kunt een premium storage-account ook maken door de prestatielaag in te stellen **Premium_LRS**. Als u wilt de prestatielaag wijzigen, moet u een van de volgende methoden gebruiken:
     
    - [PowerShell voor Azure Storage](../articles/storage/common/storage-powershell-guide-full.md#manage-the-storage-account)
    - [Azure CLI voor Azure Storage](../articles/storage/common/storage-azure-cli.md#manage-storage-accounts)
    - [Azure Storage Resource Provider REST API](https://docs.microsoft.com/rest/api/storagerp) (voor Azure Resource Manager-implementaties) of een van de Azure Storage resource provider-clientbibliotheken

    Zie voor meer informatie over limieten voor premium storage-account, [schaalbaarheids- en prestatiedoelen van Premium Storage](#premium-storage-scalability-and-performance-targets).

* **Premium lokaal redundante opslag**

    Premium storage-account ondersteunt alleen lokaal redundante opslag als de replicatie-optie. Lokaal redundante opslag worden drie kopieën van de gegevens binnen één regio. Voor regionaal herstel na noodgevallen, u moet back-up van de VM-schijven in een andere regio met behulp van [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md). U moet ook een geografisch redundante opslag (GRS)-account gebruiken als de back-upkluis. 

    Azure maakt gebruik van uw storage-account als een container voor uw niet-beheerde schijven. Wanneer u een virtuele machine van Azure die ondersteuning biedt voor Premium-opslag met niet-beheerde schijven maakt en u een premium storage-account selecteert, worden uw besturingssysteem en gegevensschijven worden opgeslagen in dat opslagaccount.

## <a name="supported-vms"></a>Ondersteunde VM 's

Premium Storage wordt ondersteund op een groot aantal Azure-VM's. U kunt standard en premium storage-schijven gebruiken met deze VM-typen. U niet premium storage-schijven gebruiken met-serie VM's die geen Premium Storage-compatibel.


Zie [Grootten voor virtuele Windows-machines](../articles/virtual-machines/windows/sizes.md) voor informatie over de VM-typen en -grootten in Azure voor Windows. Zie [Grootten voor virtuele Linux-machines](../articles/virtual-machines/linux/sizes.md) voor informatie over de VM-typen en -grootten in Azure voor Linux.

Dit zijn enkele van de functies die worden ondersteund in premium-opslag ingeschakeld virtuele machines:

* **Beschikbaarheidsset**

    In het voorbeeld van een DS-serie VM's kunt u een DS-serie VM toevoegen aan een cloudservice die alleen uit de DS-serie VM's. Voeg geen DS-serie VM's toe aan een bestaande cloudservice met een ander type heeft dan uit de DS-serie VM's. U kunt uw bestaande VHD's migreren naar een nieuwe cloudservice die alleen uit de DS-serie VM's wordt uitgevoerd. Als u wilt gebruiken hetzelfde virtuele IP-adres voor de nieuwe cloudservice die als host fungeert voor uw VM's van DS-serie gebruik [gereserveerde IP-adressen](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

* **Besturingssysteemschijf**

    U kunt uw VM voor Premium-opslag instellen om een premium- of een standaardbesturingssysteem worden uitgevoerd-schijf te gebruiken. Voor de beste ervaring wordt aangeraden de schijf van een besturingssysteem op basis van een Premium-opslag.

* **Gegevensschijven**

    U kunt premium en standard disks gebruiken in de dezelfde VM voor Premium-opslag. Met Premium Storage kunt u een virtuele machine inrichten en meerdere permanente gegevensschijven koppelen aan de virtuele machine. Indien nodig, om te verhogen van de capaciteit en prestaties van het volume, kunt u voor uw schijven stripe.

    > [!NOTE]
    > Als u premium-opslagschijven gegevens met behulp van stripe [opslagruimten](http://technet.microsoft.com/library/hh831739.aspx), instellen van opslagruimten met 1 kolom voor elke schijf die u gebruikt. Anders kan algehele prestaties van het striped volume lager is dan verwacht vanwege ongelijke distributie van verkeer op de schijven zijn. Standaard, in Serverbeheer, kunt u kolommen voor maximaal 8 schijven instellen. Als u meer dan 8 schijven hebt, kunt u PowerShell gebruiken om het volume te maken. Het aantal kolommen handmatig opgeven. Anders blijft de Serverbeheer-UI 8 kolommen gebruiken, zelfs als u meer schijven hebt. Bijvoorbeeld, hebt u 32 schijven in een stripeset één, 32 kolommen opgeven. Het aantal kolommen de virtuele schijf gebruikt opgeven de [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell-cmdlet gebruikt de *NumberOfColumns* parameter. Zie voor meer informatie, [overzicht van opslagruimten](http://technet.microsoft.com/library/hh831739.aspx) en [Veelgestelde vragen over de Storage Spaces](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
    >
    > 

* **Cache**

    Virtuele Machines (VM's) die ondersteuning bieden voor Premium Storage hebben een unieke mogelijkheid van de cache voor een hogere mate van doorvoer en lagere latentie. De mogelijkheid opslaan in cache is groter dan onderliggende premium storage-schijfprestaties. Niet alle VM's-ondersteuning voor caching, dus Controleer de virtuele machine de specificaties voor de VM-grootten waarin u geïnteresseerd bent voor meer informatie.  VM's die ondersteuning voor opslaan in cache bieden wordt dit aangegeven in hun spec met een meting 'Max. caching en tijdelijke opslagdoorvoer'.  Ze zijn ook direct onder de titel van de virtuele machine opgegeven.
    
    Met caching, kunt u de schijf beleid op premium storage-schijven aan voor caching instellen **ReadOnly**, **ReadWrite**, of **geen**. De standaard-schijf beleid voor caching **ReadOnly** voor alle schijven voor premium-gegevens, en **ReadWrite** voor besturingssysteemschijven. Voor optimale prestaties voor uw toepassing Neem Vergeet niet de juiste cache-instelling gebruiken. 
    
    Als voorbeelden, voor leesintensief of alleen-lezen gegevensschijven, zoals SQL Server-gegevensbestanden, het beleid voor schijfcache **ReadOnly**. Voor schrijfintensief of alleen-schrijven gegevensschijven, zoals SQL Server-logboekbestanden, stelt u het beleid voor schijfcache **geen**. 
    
    Zie voor meer informatie over het optimaliseren van uw ontwerp met Premium Storage, [ontwerp voor prestaties met Premium Storage](../articles/virtual-machines/windows/premium-storage-performance.md).

* **Analytische gegevens**

    Inschakelen voor het analyseren van prestaties van de virtuele machine met behulp van schijven in Premium-opslag, diagnostische gegevens van virtuele machine in de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, [Azure VM-bewaking met Azure Diagnostics-extensie](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/). 

    Als u wilt zien van prestaties van de schijf, hulpprogramma's voor gebruik op basis van het besturingssysteem, zoals [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) voor Windows-VM's en de [iostat](http://linux.die.net/man/1/iostat) opdracht voor virtuele Linux-machines.

* **Schaallimieten voor virtuele machine en de prestaties**

    Elke Premium-opslag ondersteund VM-grootte heeft schaallimieten en van de prestatiespecificaties voor IOPS, bandbreedte en het aantal schijven dat per virtuele machine kan worden gekoppeld. Als u premium storage-schijven met virtuele machines gebruikt, zorg ervoor dat er voldoende IOPS en bandbreedte op de virtuele machine op het station schijfverkeer.

    Een VM STANDARD_DS1 heeft bijvoorbeeld een toegewezen bandbreedte van 32 MB/s voor premium disk opslagverkeer. Een schijf voor opslag van P10 premium biedt een bandbreedte van 100 MB/s. Als een P10 premium storage-schijf is gekoppeld aan deze virtuele machine, kan deze alleen gaat u maximaal 32 MB/s. Het kan het maximaal 100 MB/s die de P10-schijf biedt niet gebruiken.

    De grootste virtuele machine in de DS-serie is momenteel de Standard_DS15_v2. De Standard_DS15_v2 krijgt u maximaal 960 MB/s voor alle schijven. De grootste virtuele machine in de GS-serie is de Standard_GS5. De Standard_GS5 kan maximaal 2.000 MB/s voor alle schijven bieden.

    Deze limieten zijn alleen beschikbaar voor schijfverkeer. Deze limieten zijn niet opgenomen cachetreffers en netwerkverkeer. Een afzonderlijke bandbreedte is beschikbaar voor VM-netwerkverkeer. Bandbreedte voor netwerkverkeer wijkt af van de toegewezen bandbreedte die wordt gebruikt door premium storage-schijven.

    Zie voor de meest actuele informatie over de maximale IOPS en doorvoer (bandbreedte) voor Premium Storage-ondersteunde VM's, [Windows VM-grootten](../articles/virtual-machines/windows/sizes.md) of [Linux VM-grootten](../articles/virtual-machines/linux/sizes.md).

    Zie de tabel in de volgende sectie voor meer informatie over premium storage-schijven en hun limieten voor IOPS en doorvoer.

## <a name="scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen
In deze sectie beschrijven we de schaalbaarheids- en prestatiedoelen moet overwegen bij het gebruik van Premium Storage.

Premium storage-accounts hebben de volgende schaalbaarheidsdoelen:

| Totale capaciteit | Totale bandbreedte voor een account voor lokaal redundante opslag |
| --- | --- | 
| Schijf capaciteit: 35 TB <br>Snapshot-capaciteit: 10 TB | Omhoog naar 50 gigabits per seconde voor binnenkomende<sup>1</sup> + uitgaande<sup>2</sup> |

<sup>1</sup> alle gegevens (aanvragen) die worden verzonden naar een opslagaccount

<sup>2</sup> alle gegevens (reacties) die worden ontvangen van een storage-account

Zie voor meer informatie, [schaalbaarheids- en prestatiedoelen voor Azure Storage](../articles/storage/common/storage-scalability-targets.md).

Als u premium storage-accounts voor niet-beheerde schijven gebruikt en uw toepassing is groter dan de schaalbaarheidsdoelen van een enkel opslagaccount, is het raadzaam om te migreren naar managed disks. Als u niet migreren naar managed disks wilt, moet u uw toepassing gebruik meerdere opslagaccounts maken. Partities voor uw gegevens vervolgens over de storage-accounts. Bijvoorbeeld, als u koppelen 51-TB schijven op meerdere virtuele machines wilt, verdeeld over deze twee opslagaccounts. 35 TB is de limiet voor één premium storage-account. Zorg ervoor dat één premium storage-account nooit meer dan 35 TB ingerichte schijven heeft.

### <a name="premium-storage-disk-limits"></a>Limieten voor Premium Storage-schijf
Als u een premium-opslagschijf inricht, wordt de grootte van de schijf bepaalt de maximale IOPS en doorvoer (bandbreedte). Azure biedt zeven typen premium-opslagschijven: P4 (beheerde schijven alleen), P6 (beheerde schijven alleen), P10, P20 of P30, P40 of P50. Elke schijftype voor premium storage heeft bepaalde limieten voor IOPS en doorvoer. Limieten voor de schijftypen worden beschreven in de volgende tabel:

| Schijftype voor Premium-schijven  | P4    | P6    | P10   | P15   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 32 GB| 64 GB| 128 GB| 256 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 1100   | 2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde | 125 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde | 

> [!NOTE]
> Zorg ervoor dat voldoende bandbreedte is beschikbaar op de virtuele machine op station schijfverkeer, zoals beschreven in [Premium Storage-ondersteunde VM's](#premium-storage-supported-vms). Anders wordt is de doorvoer van schijfgegevens en IOP's beperkt tot het lagere waarden. Maximale doorvoer en IOPS zijn gebaseerd op de VM-limieten, niet op de schijflimieten die worden beschreven in de voorgaande tabel.  
> 
> 

Hier volgen enkele belangrijke dingen om te weten over de schaalbaarheids- en prestatiedoelen van Premium Storage:

* **Ingerichte capaciteit en prestaties**

    Wanneer u een premium-opslagschijf, in tegenstelling tot standaard opslag inrichten kunt u de capaciteit, IOPS en doorvoer van die schijf worden gegarandeerd. Als u een P50 schijf maakt, richt Azure bijvoorbeeld 4.095 GB opslagcapaciteit, 7.500 IOPS en doorvoer van 250 MB/s voor de schijf. Uw toepassing kunt gebruiken of een deel van de capaciteit en prestaties.

* **Schijfgrootte**

    Azure kaarten-de grootte van de schijf (afgerond) naar de dichtstbijzijnde premium opslagschijfoptie, zoals opgegeven in de tabel in de vorige sectie. Bijvoorbeeld, is de grootte van een schijf van 100 GB geclassificeerd als een P10-optie. Deze kunt maximaal 500 IOP's, uitvoeren met maximaal 100 MB/s-doorvoereenheden. Op deze manier een schijf van de grootte van die 400 GB is geclassificeerd als een P20. Er kunnen worden uitgevoerd tot 2300 IOP's, met doorvoer 150 MB/s.
    
    > [!NOTE]
    > De grootte van bestaande schijven kunt u eenvoudig verhogen. U wilt bijvoorbeeld Verhoog de grootte van een 30 GB-schijf van 128 GB, of zelfs tot 1 TB. Of u wilt de schijf P20 converteren naar een P30-schijf omdat u meer capaciteit of meer IOPS en doorvoer nodig hebt. 
    > 
 
* **I/o-grootte**

    De grootte van een i/o is 256 KB. Als de gegevens worden overgebracht minder dan 256 KB is, wordt deze beschouwd als 1 i/o-eenheid. I/o-grotere worden geteld als meerdere i/o's van de grootte van 256 KB. Bijvoorbeeld, wordt 1100 KB i/o geteld als 5 i/o-eenheden.

* **Doorvoer**

    De maximale doorvoer van schrijfbewerkingen naar de schijf bevat, en deze schijf leesbewerkingen die niet worden opgehaald uit de cache bevat. Een P10-schijf heeft bijvoorbeeld 100 MB/s doorvoer per schijf. Enkele voorbeelden van geldige doorvoer voor een P10-schijf worden in de volgende tabel weergegeven:

    | Max. doorvoer per P10-schijf | Niet-cache leesbewerkingen van schijf | Niet-cache-schrijfbewerkingen naar de schijf |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/s | 40 MB/s |

* **Treffers in cache**

    Treffers in cache zijn niet beperkt door de toegewezen IOP's of doorvoer van de schijf. Bijvoorbeeld, wanneer u een gegevensschijf met een **ReadOnly** cache-instelling op een virtuele machine die wordt ondersteund door Premium Storage, leesbewerkingen die worden aangeleverd vanuit de cache vallen niet onder de IOPS en doorvoer limieten van de schijf. Als de werkbelasting van een schijf voornamelijk is leest, krijgt u mogelijk zeer hoge doorvoer. De cache is onderhevig aan afzonderlijke IOPS en doorvoerlimieten op de virtuele machine op, op basis van de VM-grootte. DS-serie virtuele machines hebben ongeveer 4000 IOPS en doorvoer van 33 MB/s per kern voor cache en lokale SSD-i/o's. GS-serie VM's hebben een limiet van 5000 IOP's en doorvoer van 50 MB/s per kern voor cache en lokale SSD-i/o's. 

## <a name="throttling"></a>Beperking
Beperking kan optreden, als uw toepassing IOP's of doorvoer groter is dan de toegewezen limieten voor de schijf voor een premium-opslag. Beperking ook optreden als uw schijfverkeer totale over alle schijven op de virtuele machine de schijf bandbreedte beschikbaar voor de virtuele machine overschrijdt. Om te voorkomen beperking, is het raadzaam dat u het aantal openstaande i/o-aanvragen voor de schijf beperken. Gebruik een beperken op basis van de schaalbaarheids- en prestatiedoelen voor de schijf die u hebt ingericht, en de bandbreedte van de schijf beschikbaar zijn voor de virtuele machine.  

Uw toepassing kan de laagste latentie worden behaald als het is ontworpen om te voorkomen dat beperking. Echter, als het aantal openstaande i/o-aanvragen voor de schijf te klein is, uw toepassing kan niet profiteren van de maximale IOPS en de doorvoerniveaus die beschikbaar voor de schijf zijn.

De volgende voorbeelden laten zien hoe u berekenen bandbreedtebeperking niveaus. Alle berekeningen zijn gebaseerd op een i/o-grootte van 256 KB.

### <a name="example-1"></a>Voorbeeld 1
Uw toepassing heeft 495 i/o-eenheden van 16 KB's verwerkt in één seconde op een P10-schijf. De i/o-eenheden worden geteld als 495 IOPS. Als u een 2 MB i/o's in dezelfde seconde, is het totaal van i/o-eenheden gelijk zijn aan 495 + 8 IOPS. Dit komt doordat 2 MB i/o = 2048 KB / 256 KB = 8 i/o-eenheden, wanneer de grootte van de i/o-eenheid 256 KB is. Omdat de som van 495 + 8 groter is dan de 500 IOPS-limiet voor de schijf, treedt beperking op.

### <a name="example-2"></a>Voorbeeld 2
Uw toepassing is verwerkt 400 i/o-eenheden van de grootte van 256 KB op een P10-schijf. De totale bandbreedte verbruikt is (400 &#215; 256) / 1024 KB = 100 MB/s. Een P10-schijf heeft een doorvoerlimiet van 100 MB/s. Als uw toepassing probeert te meer i/o-bewerkingen uitvoeren in die tweede, is deze beperkt omdat deze de limiet voor toegewezen overschrijdt.

### <a name="example-3"></a>Voorbeeld 3
U hebt een DS4-VM met twee P30-schijven die zijn gekoppeld. Elke P30-schijf is 200 MB/s doorvoer geschikt is. Een DS4-VM heeft echter een totale bandbreedte van het schijfcapaciteit van 256 MB/s. U kan niet zowel gekoppelde schijven aan de maximale doorvoer station op deze DS4-VM op hetzelfde moment. U kunt dit oplossen kunt u mogelijkheden om verkeer van 200 MB/s op één schijf en 56 MB/s op de andere schijf. Als de som van de schijfverkeer via 256 MB/s gaat, is schijfverkeer beperkt.

> [!NOTE]
> Als uw schijfverkeer voornamelijk uit kleine i/o-grootten bestaat, wordt uw toepassing waarschijnlijk de IOPS-limiet voor de maximale doorvoer bereikt. Echter, als het schijfverkeer voornamelijk uit grote i/o-grootte bestaat, uw toepassing waarschijnlijk bereikt de maximale doorvoer eerst, in plaats van de IOPS-limiet. U kunt van uw toepassing IOPS en van doorvoercapaciteit maximaliseren met behulp van de optimale i/o-grootte. U kunt ook het aantal openstaande i/o-aanvragen voor een schijf beperken.
> 

Zie voor meer informatie over het ontwerpen voor hoge prestaties met behulp van Premium-opslag, [ontwerp voor prestaties met Premium Storage](../articles/virtual-machines/windows/premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Momentopnamen en kopiëren van de Blob

Met de Storage-service is het VHD-bestand een pagina-blob. U kunt momentopnamen van pagina-blobs en kopieer deze naar een andere locatie, zoals naar een ander opslagaccount.

### <a name="unmanaged-disks"></a>Niet-beheerde schijven

Maak [incrementele momentopnamen](../articles/virtual-machines/linux/incremental-snapshots.md) voor niet-beheerde premium-schijven dezelfde manier als u momentopnamen gebruiken met de standard-opslag. Premium Storage ondersteunt alleen lokaal redundante opslag als de replicatie-optie. U wordt aangeraden dat u momentopnamen maken en kopieer vervolgens de momentopnamen naar een geografisch redundante standard storage-account. Zie voor meer informatie, [redundantieopties voor Azure Storage](../articles/storage/common/storage-redundancy.md).

Als een schijf is gekoppeld aan een virtuele machine, worden sommige API-bewerkingen op de schijf zijn niet toegestaan. Bijvoorbeeld, u kunt niet uitvoeren een [kopiëren van de Blob](/rest/api/storageservices/Copy-Blob) bewerking op die blob als de schijf is gekoppeld aan een virtuele machine. In plaats daarvan eerst een momentopname maken van die blob met behulp van de [momentopname maken van Blob](/rest/api/storageservices/Snapshot-Blob) REST-API. Voer vervolgens de [kopiëren van de Blob](/rest/api/storageservices/Copy-Blob) van de momentopname naar de gekoppelde schijf kopiëren. U kunt ook de schijf loskoppelen en vervolgens voert geen bewerkingen uit die nodig zijn.

De volgende limieten gelden voor premium storage-blob-momentopnamen:

| Limiet voor Premium-opslag | Waarde |
| --- | --- |
| Maximum aantal momentopnamen per blob | 100 |
| Capaciteit van een opslagaccount voor momentopnamen<br>(Bevat gegevens van momentopnamen alleen. Bevat geen gegevens in basis-blob.) | 10 TB |
| Minimale tijd tussen opeenvolgende momentopnamen | 10 minuten |

Als u wilt behouden geografisch redundante kopieën van uw momentopnamen, kunt u momentopnamen van een premium storage-account naar een geografisch redundante standard storage-account kopiëren met behulp van AzCopy of kopiëren van de Blob. Zie voor meer informatie, [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../articles/storage/common/storage-use-azcopy.md) en [kopiëren van de Blob](/rest/api/storageservices/Copy-Blob).

Zie voor gedetailleerde informatie over het uitvoeren van REST-bewerkingen voor pagina-blobs in een premium storage-account [servicebewerkingen met Azure Premium Storage-Blob](http://go.microsoft.com/fwlink/?LinkId=521969).

### <a name="managed-disks"></a>Managed Disks

Een momentopname van een beheerde schijf is een alleen-lezen kopie van de beheerde schijf. De momentopname wordt opgeslagen als een standard-beheerde schijven. Op dit moment [incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md) worden niet ondersteund voor beheerde schijven. Zie voor meer informatie over een momentopname te maken voor een beheerde schijf, [een kopie maken van een VHD die is opgeslagen als een door Azure beheerde schijf met behulp van beheerde momentopnamen in Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md) of [een kopie maken van een VHD die is opgeslagen als een door Azure beheerde schijf met behulp van beheerd momentopnamen in Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md).

Als een beheerde schijf is gekoppeld aan een virtuele machine, worden sommige API-bewerkingen op de schijf zijn niet toegestaan. U kunt bijvoorbeeld een shared access signature (SAS) om uit te voeren van een kopieerbewerking terwijl de schijf is gekoppeld aan een virtuele machine niet genereren. In plaats daarvan maakt u eerst een momentopname van de schijf en voert u de kopie van de momentopname. U kunt ook de schijf loskoppelen en vervolgens een SAS om uit te voeren van de kopieerbewerking te genereren.


## <a name="premium-storage-for-linux-vms"></a>Premium-opslag voor virtuele Linux-machines
U kunt de volgende informatie om u te helpen bij het instellen van uw virtuele Linux-machines in Premium-opslag:

Om te realiseren schaalbaarheidsdoelen in Premium-opslag, voor alle premium-opslagschijven met cache ingesteld op **ReadOnly** of **geen**, moet u "barrières" uitschakelen wanneer u het bestandssysteem koppelen. U hoeft geen barrières in dit scenario omdat de schrijfbewerkingen naar premium storage-schijven duurzame voor deze cache-instellingen zijn. Wanneer de schrijfaanvraag met succes is voltooid, heeft de gegevens zijn geschreven voor het permanente archief. Als wilt uitschakelen 'barrières', een van de volgende methoden te gebruiken. Kies de koppeling voor het bestandssysteem:
  
* Voor **reiserFS**, zodat barrières uitschakelen, gebruikt de `barrier=none` optie koppelen. (Gebruiken om in te schakelen barrières, `barrier=flush`.)
* Voor **ext3/ext4**, zodat barrières uitschakelen, gebruikt de `barrier=0` optie koppelen. (Gebruiken om in te schakelen barrières, `barrier=1`.)
* Voor **XFS**, zodat barrières uitschakelen, gebruikt de `nobarrier` optie koppelen. (Gebruiken om in te schakelen barrières, `barrier`.)
* Voor premium storage-schijven met cache ingesteld op **ReadWrite**, barrières voor schrijven duurzaamheid inschakelen.
* Voor de volumelabels van het om vast te leggen nadat de virtuele machine opnieuw is opgestart, moet u/etc/fstab bijwerken met de universele, unieke id (UUID) verwijzingen naar de schijven. Zie voor meer informatie, [een beheerde schijf toevoegen aan een Linux-VM](../articles/virtual-machines/linux/add-disk.md).

De volgende Linux-distributies zijn gevalideerd voor Azure Premium Storage. Voor betere prestaties en stabiliteit met Premium Storage, is het raadzaam dat u uw VM's upgraden naar een van deze versies, ten minste (of naar een nieuwere versie). Voor sommige van de versies van moet de meest recente Linux Integration Services (LIS), v4.0, voor Azure. Als u wilt downloaden en installeren van een distributiepunt, volgt u de koppeling in de volgende tabel weergegeven. We toevoegen afbeeldingen aan de lijst als we klaar zijn met validatie. Houd er rekening mee dat onze validaties weergegeven dat de prestaties wisselend voor elke afbeelding. Prestaties zijn afhankelijk van uw installatiekopie-instellingen en kenmerken van de werkbelasting. Verschillende afbeeldingen zijn afgesteld voor verschillende soorten workloads.

| Distributie | Versie | Ondersteunde kernel | Details |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| SUSE-sles-12-prioriteit-v20150213 <br> SUSE-sles-12-v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [LIS4 vereist](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Zie de opmerking in de volgende sectie* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [Aanbevolen LIS4](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Zie de opmerking in de volgende sectie* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 of RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 of RHCK met[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 of RHCK met[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>LIS-stuurprogramma's voor OpenLogic CentOS

Als u de OpenLogic CentOS-VM's worden uitgevoerd, voert u de volgende opdracht om de meest recente stuurprogramma's installeren:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

Start de computer voor het activeren van de nieuwe stuurprogramma's.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Als u Premium-opslag gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

* **Premium storage disk en blob-grootte**

    Facturering voor een premium-opslagschijf of de blob is afhankelijk van de ingerichte grootte van de schijf of de blob. Azure wijst de ingerichte grootte (afgerond) toe aan de dichtstbijzijnde premium opslagschijfoptie. Voor meer informatie, Zie de tabel in [schaalbaarheids- en prestatiedoelen van Premium Storage](#premium-storage-scalability-and-performance-targets). Elke schijf toegewezen aan een grootte van de ondersteunde ingerichte schijf, en dienovereenkomstig wordt gefactureerd. Facturering voor ingerichte schijven is per uur Pro rata met behulp van de maandelijkse prijs voor de Premium-opslag-aanbieding. Bijvoorbeeld, als u een P10-schijf ingericht en deze na 20 uur, in rekening gebracht voor de aanbieding P10 20 uur Pro rata berekend. Dit is, ongeacht de hoeveelheid feitelijke gegevens geschreven naar de schijf of de IOPS en doorvoer die wordt gebruikt.

* **Premium niet-beheerde schijven, momentopnamen**

    Momentopnamen op niet-beheerde premium-schijven worden in rekening gebracht voor de extra capaciteit gebruikt door de momentopnamen. Zie voor meer informatie over momentopnamen [een momentopname maken van een blob](/rest/api/storageservices/Snapshot-Blob).

* **Premium beheerde schijven, momentopnamen**

    Een momentopname van een beheerde schijf is een alleen-lezen kopie van de schijf. De schijf wordt opgeslagen als een standard-beheerde schijven. Een momentopname van een kosten hetzelfde als een standaard schijf beheerde. De kosten van de momentopname is bijvoorbeeld gelijk aan een standaard beheerde schijf van 128 GB als u een momentopname van een beheerde schijf van 128 GB premium.  

* **Uitgaande gegevensoverdracht**

    [Uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) (gegevens die vanuit datacenters van Azure) worden gefactureerd voor bandbreedtegebruik.

Zie de volgende artikelen voor gedetailleerde informatie over prijzen voor Premium-opslag, Premium Storage-ondersteunde VM's en beheerde schijven:

* [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Prijzen voor Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Prijzen van Managed disks](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Ondersteuning van Azure back-up 

Voor regionaal herstel na noodgevallen, u moet back-up van de VM-schijven in een andere regio met behulp van [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) en een GRS-opslagaccount als een back-upkluis.

Eenvoudig herstel van de virtuele machine, en het beleid voor back-upretentie gebruik Azure Backup voor het maken van een back-uptaak met back-ups op basis van tijd. U kunt back-up beide gebruiken met niet-beheerde en beheerde schijven. Zie voor meer informatie, [Azure back-up voor virtuele machines met niet-beheerde schijven](../articles/backup/backup-azure-vms-first-look-arm.md) en [Azure back-up voor virtuele machines met beheerde schijven](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Premium-opslag.
