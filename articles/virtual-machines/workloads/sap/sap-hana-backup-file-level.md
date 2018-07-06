---
title: SAP HANA Azure back-up op bestandsniveau | Microsoft Docs
description: Er zijn twee primaire back-mogelijkheden voor SAP HANA op Azure virtual machines, in dit artikel bevat informatie over SAP HANA Azure back-up op bestandsniveau
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: d3d1769766053b513a98df153cb635ae148f26b1
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867367"
---
# <a name="sap-hana-azure-backup-on-file-level"></a>SAP HANA Azure back-up op bestandsniveau

## <a name="introduction"></a>Inleiding

Dit maakt deel uit van een driedelige reeks gerelateerde artikelen op SAP HANA back-up. [Back-uphandleiding voor SAP HANA op Azure Virtual Machines](./sap-hana-backup-guide.md) bevat een overzicht en de informatie op aan de slag, en [back-up van SAP HANA op basis van opslagmomentopnamen](./sap-hana-backup-storage-snapshots.md) bevat informatie over de opslag op basis van een momentopname van back-upoptie.

Verschillende typen VM's in Azure kunnen een verschillend aantal VHD's die zijn gekoppeld. De exacte details worden gedocumenteerd in [grootten voor virtuele Linux-machines in Azure](../../linux/sizes.md). We gebruiken een GS5 Azure-VM's waardoor 64 gekoppelde gegevensschijven voor de tests die in deze documentatie genoemd. Voor grotere SAP HANA-systemen, mogelijk al een groot aantal schijven worden gebruikt voor gegevens en logboekbestanden, mogelijk in combinatie met striping van de software voor een optimale schijf-i/o-doorvoer. Lees het artikel voor meer informatie over voorgestelde schijfconfiguraties voor implementaties van SAP HANA op Azure Virtual machines [SAP HANA op Azure-bedieningshandleiding](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations). De aanbevelingen zijn inclusief schijfruimte aanbevelingen voor ook de lokale back-ups.

Er is op dit moment geen back-integratie met SAP HANA beschikbaar met Azure Backup-service. De standaardmethode voor het beheren van back-up/herstel op bestandsniveau, is met een back-ups via SAP HANA Studio of via SAP HANA SQL-instructies. Zie [SAP HANA SQL en weergaven verwijzing naar het bestandssysteem](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) voor meer informatie.

![In deze afbeelding ziet u het dialoogvenster van de back-menu-item in SAP HANA Studio](media/sap-hana-backup-file-level/image022.png)

In deze afbeelding ziet u het dialoogvenster van de back-menu-item in SAP HANA Studio. Bij het kiezen van type &quot;bestand&quot; een heeft een pad opgeven in het bestandssysteem waar de back-upbestanden in SAP HANA worden geschreven. Herstellen werkt op dezelfde manier.

Deze keuze klinkt eenvoudig en duidelijk, maar er zijn enkele overwegingen. Zoals al eerder vermeld, is een Azure-VM beperkt tot het aantal gegevensschijven dat kan worden gekoppeld. Er kan niet worden capaciteit voor het opslaan van back-upbestanden van de SAP HANA op de bestandssystemen van de virtuele machine, afhankelijk van de grootte van de database en de schijf doorvoer-vereisten, waarbij software striping over meerdere gegevensschijven mogelijk. Verschillende opties voor het verplaatsen van deze back-upbestanden en beheren van bestand groottebeperkingen en prestaties bij het verwerken van terabytes aan gegevens, vindt u verderop in dit artikel.

Een andere optie, die meer vrijheid met betrekking tot de totale capaciteit biedt, is Azure blob-opslag. Één blob is ook beperkt tot 1 TB, is de totale capaciteit van een enkele blob-container momenteel 500 TB. Bovendien het biedt klanten de keuze om te selecteren, zogenaamde &quot;cool&quot; blob-opslag, het voordeel van een kosten heeft. Zie [Azure Blob Storage: Hot en cool storage-lagen](../../../storage/blobs/storage-blob-storage-tiers.md) voor meer informatie over cool blob-opslag.

Voor extra veiligheid, door een opslag met geo-replicatie-account te gebruiken voor het opslaan van de back-ups van SAP HANA. Zie [Azure Storage-replicatie](../../../storage/common/storage-redundancy.md) voor meer informatie over de storage-account-replicatie.

Een kan gespecialiseerde VHD's voor back-ups van SAP HANA plaatsen in een toegewezen back-upopslag-account dat is geo-replicatie. Anders een kan de virtuele harde schijven die houden van de back-ups van SAP HANA naar een opslag met geo-replicatie-account of naar een opslagaccount dat is in een andere regio worden gekopieerd.

## <a name="azure-backup-agent"></a>Azure backup-agent

Azure backup biedt de optie voor het niet alleen back-up van volledige virtuele machines, maar ook bestanden en mappen via de back-agent, die moet worden geïnstalleerd op het gastbesturingssysteem te installeren. Maar deze agent wordt alleen ondersteund op Windows (Zie [Back-up van een Windows Server of client maken op Azure met het Resource Manager-implementatiemodel](../../../backup/backup-configure-vault.md)).

Er is een tijdelijke oplossing eerst SAP HANA-back-upbestanden naar een Windows-VM op Azure te kopiëren (bijvoorbeeld via SAMBA-share) en vervolgens met de Azure backup-agent van daaruit. Het is technisch mogelijk, zou deze complexiteit toevoegen en vertragen de back-up of herstel van proces nogal vanwege de kopie tussen de Linux- en de Windows-VM. Het verdient aanbeveling niet te volgen van deze benadering.

## <a name="azure-blobxfer-utility-details"></a>Azure blobxfer hulpprogramma details

Voor het opslaan van mappen en bestanden op Azure-opslag, kan een CLI of PowerShell gebruiken of ontwikkelt u een hulpprogramma met behulp van een van de [Azure-SDK's](https://azure.microsoft.com/downloads/). Er is ook een kant-en-klare hulpprogramma AzCopy, voor het kopiëren van gegevens naar Azure storage, maar dit is alleen Windows (Zie [gegevensoverdracht met het hulpprogramma AzCopy Command-Line](../../../storage/common/storage-use-azcopy.md)).

Daarom is blobxfer gebruikt voor het kopiëren van de back-upbestanden SAP HANA. Het is open-source, die wordt gebruikt door veel klanten in een productieomgeving en beschikbaar zijn op [GitHub](https://github.com/Azure/blobxfer). Dit hulpprogramma kunt een om te kopiëren van gegevens rechtstreeks naar Azure blob-opslag of Azure-bestandsshare. Het biedt ook een aantal handige functies, zoals md5-hash of automatische parallelle uitvoering bij het kopiëren van een map met meerdere bestanden.

## <a name="sap-hana-backup-performance"></a>Back-upprestaties van SAP HANA

![Deze schermopname is van de back-upconsole SAP HANA in SAP HANA Studio](media/sap-hana-backup-file-level/image023.png)

Deze schermopname is van de back-upconsole SAP HANA in SAP HANA Studio. Dit duurde ongeveer 42 minuten de back-up van de 230 GB op een enkele Azure standard-opslag-schijf aan de HANA-VM met behulp van het bestandssysteem XFS gekoppeld.

![Deze schermopname is van het YaST op de SAP HANA-test-VM](media/sap-hana-backup-file-level/image024.png)

Deze schermopname is van het YaST op de SAP HANA-test-VM. De betreffende schijf 1 TB voor back-up van SAP HANA kunnen zien zoals al eerder vermeld. Dit duurde ongeveer 42 minuten tot back-up 230 GB. Bovendien vijf 200 GB schijven zijn gekoppeld en software-RAID md0 gemaakt, met striping boven op deze vijf Azure-gegevensschijven.

![Herhalen van de dezelfde back-up op software RAID met striping in alle vijf aangesloten gegevensschijven Azure standard-opslag](media/sap-hana-backup-file-level/image025.png)

De dezelfde back-up op software herhalende RAID met striping in alle vijf Azure standard-opslag-gegevensschijven gebracht van de back-uptijd van 42 minuten tot 10 minuten gekoppeld. De schijven zijn zonder caching aan de virtuele machine gekoppeld. Het is dus duidelijk hoe belangrijk schijf schrijven-doorvoer is voor de back-uptijd. Een kan vervolgens overschakelen naar Azure Premium Storage om te het proces voor optimale prestaties verder te versnellen. Azure Premium Storage moet in het algemeen worden gebruikt voor productiesystemen.

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>Back-upbestanden voor SAP HANA naar Azure blob storage kopiëren

Een andere optie voor het snel opslaan van back-upbestanden voor SAP HANA wordt Azure blob-opslag. Een enkele blob-container heeft een limiet van 500 TB, voldoende zijn voor sommige kleinere SAP HANA-systemen, typen M32ts, M32ls M64ls en GS5-VM's van Azure, met voldoende SAP HANA-back-ups behouden. Klanten hebben de keuze tussen &quot;hot&quot; en &quot;koude&quot; blob-opslag (Zie [Azure Blob Storage: Hot en cool storage-lagen](../../../storage/blobs/storage-blob-storage-tiers.md)).

Met het hulpprogramma blobxfer is het eenvoudig om te kopiëren van de back-upbestanden van de SAP HANA rechtstreeks naar Azure blob-opslag.

![Hier ziet een de bestanden van een volledige SAP HANA-bestandsback-up](media/sap-hana-backup-file-level/image026.png)

Hier kunt zien de bestanden van een volledige SAP HANA-bestandsback-up. Er zijn vier bestanden en het grootste pakket heeft ongeveer 230 GB.

![Dit duurde ongeveer 3000 seconden de 230 GB kopiëren naar een Azure standard storage-account blob-container](media/sap-hana-backup-file-level/image027.png)

Md5-hash niet gebruikt in de eerste test, duurde dit ongeveer 3000 seconden de 230 GB kopiëren naar een Azure standard storage-account blob-container.

![In deze schermafbeelding kunnen zien hoe het eruit ziet in de Azure portal](media/sap-hana-backup-file-level/image028.png)

In deze schermafbeelding kunt zien hoe het eruit ziet in de Azure portal. Een blob-container met de naam &quot;sap-hana-back-ups&quot; is gemaakt en is voorzien van de vier blobs, die staan voor de back-upbestanden van de SAP HANA. Een van deze heeft een grootte van ongeveer 230 GB.

De back-upconsole HANA Studio kunt een om te beperken van de maximale bestandsgrootte van HANA back-upbestanden. In de voorbeeldomgeving, moet deze prestaties verbeterd doordat het mogelijk om meerdere kleinere back-upbestanden, in plaats van een grote 230 GB-bestand.

![Instellen van de grootte van de back-upbestand op de HANA kant heeft&#39;t verbetering van de back-uptijd](media/sap-hana-backup-file-level/image029.png)

Instellen van de grootte van de back-upbestand op de HANA kant heeft&#39;t verbetering van de back-uptijd, omdat de bestanden worden na elkaar zoals weergegeven in deze afbeelding geschreven. De maximale bestandsgrootte is ingesteld op 60 GB, zodat de back-up vier grote gegevensbestanden in plaats van het bestand 230 GB gemaakt. Met behulp van meerdere back-upbestanden is noodzakelijk voor het back-ups van HANA-databases die gebruikmaken van het geheugen van grotere Azure-VM's, zoals M64s, M64ms M128s en M128ms.

![Als u wilt testen parallelle uitvoering van het hulpprogramma blobxfer, is klikt u vervolgens de maximale bestandsgrootte voor back-ups van HANA ingesteld op 15 GB](media/sap-hana-backup-file-level/image030.png)

Als u wilt testen parallelle uitvoering van het hulpprogramma blobxfer, is de maximale bestandsgrootte voor HANA back-ups wordt ingesteld op 15 GB, wat leidde tot 19 back-upbestanden. Deze configuratie kan de tijd voor blobxfer de 230 GB naar Azure blob-opslag kopiëren van 3000 seconden tot 875 seconden.

Dit is vanwege de limiet van 60 MB per seconde voor het schrijven van een Azure-blob. Parallelle uitvoering via meerdere blobs het knelpunt is opgelost, maar er is een nadeel: load verhogen van de prestaties van het hulpprogramma blobxfer alle deze bestanden te kopiëren HANA back-up naar Azure blob-opslag worden geplaatst op zowel de HANA-VM en het netwerk. Bewerking van HANA-systeem wordt beïnvloed.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>BLOB kopiëren van schijven die zijn toegewezen Azure-gegevens in de back-upsoftware RAID

In tegenstelling tot de handmatige VM gegevens schijf back-up, in deze benadering een worden geen back-up van alle gegevensschijven op een virtuele machine op te slaan van de gehele SAP-installatie, met inbegrip van gegevens van HANA, HANA logboekgebeurtenissen bestanden en configuratiebestanden. Het idee is in plaats daarvan zijn speciaal bestemd voor software-RAID met striping over meerdere Azure-gegevens VHD's voor het opslaan van een volledige SAP HANA-bestandsback-up. Een kopieert alleen deze schijven, waarvoor de back-up van SAP HANA. Ze kunnen eenvoudig worden bewaard in een specifiek HANA back-upopslag-account, of die is gekoppeld aan een toegewezen &quot;back-up van virtuele machine management&quot; voor verdere verwerking.

![Alle VHD's die zijn gekopieerd met behulp van de ** start-azurestorageblobcopy ** PowerShell-opdracht](media/sap-hana-backup-file-level/image031.png)

Nadat de back-up naar de lokale software-RAID is voltooid, alle VHD's die zijn gekopieerd met behulp van de **start azurestorageblobcopy** PowerShell-opdracht (Zie [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy)). Als dit is alleen van invloed op het toegewezen bestandssysteem voor het bewaren van de back-upbestanden, zijn er geen zorgen te maken over SAP HANA bestandsconsistentie gegevensbestand of logboekbestand op de schijf. Een voordeel van deze opdracht is dat het werkt terwijl de virtuele machine online blijft. Als u wilt er zeker van zijn dat er geen proces naar de back-up stripeset schrijft, moet deze voordat u de kopie van de blob te ontkoppelen en koppel deze daarna opnieuw. Of een kan een juiste manier om te gebruiken &quot;blokkeren&quot; het bestandssysteem. Bijvoorbeeld via xfs\_blokkeren voor het bestandssysteem XFS.

![Deze schermafbeelding ziet u de lijst met blobs in de VHD-container in Azure portal](media/sap-hana-backup-file-level/image032.png)

Deze schermafbeelding ziet u de lijst met blobs in de &quot;VHD's&quot; container in Azure portal. De schermafbeelding ziet u de vijf VHD's, die zijn gekoppeld aan de VM die fungeert als de software-RAID te houden van SAP HANA-back-upbestanden van de SAP HANA-server. U ziet ook de vijf exemplaren die zijn gemaakt via de opdracht blob kopiëren.

![Voor testdoeleinden zijn de kopieën van de SAP HANA back-upsoftware RAID-schijven gekoppeld aan de appserver VM](media/sap-hana-backup-file-level/image033.png)

Voor testdoeleinden zijn de kopieën van de SAP HANA back-upsoftware RAID-schijven gekoppeld aan de virtuele machine van de app-server.

![De virtuele machine is afgesloten om de schijf te koppelen appserver opgehaald](media/sap-hana-backup-file-level/image034.png)

De app-server virtuele machine is afgesloten opgehaald om de schijf te koppelen. Na het starten van de virtuele machine, de schijven en de RAID zijn correct gedetecteerd (gekoppeld via UUID). Alleen het koppelpunt ontbreekt, wordt die is gemaakt via de partitioner YaST. Daarna de kopieën van de back-upbestand SAP HANA is geworden zichtbaar op besturingssysteemniveau.

## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>SAP HANA-back-upbestanden naar NFS-share te kopiëren

Als u wilt de potentiële impact op de SAP HANA-systeem van een prestatie- of schijfruimte standaardprocedures, overwegen een opslaan van de back-upbestanden van de SAP HANA op een NFS-share. Technisch gezien het werkt, maar het betekent als de host van de NFS-share met behulp van een tweede virtuele machine van Azure. Het mag geen een klein VM-grootte, vanwege de bandbreedte van het VM-netwerk. Het kan zinvol zijn vervolgens om deze af te sluiten &quot;back-up van virtuele machine&quot; en alleen voor het uitvoeren van de SAP HANA back-up te brengen. Schrijven van een NFS share load plaatst in het netwerk en heeft gevolgen voor de SAP HANA-systeem, maar alleen beheren van de back-upbestanden daarna op de &quot;back-up van virtuele machine&quot; de SAP HANA-systeem niet helemaal zou beïnvloeden.

![Een NFS-share van een andere Azure-VM is gekoppeld aan de virtuele machine van de SAP HANA-server](media/sap-hana-backup-file-level/image035.png)

Om te controleren of dat de NFS use-case, is een NFS-share van een andere Azure-VM gekoppeld aan de virtuele machine van de SAP HANA-server. Er is geen speciale NFS afstemmen toegepast.

![Het heeft geduurd 1 uur en 46 minuten aan de back-up rechtstreeks doen](media/sap-hana-backup-file-level/image036.png)

De NFS-share is een snelle stripeset, zoals het op de SAP HANA-server. Dit duurde 1 uur en 46 minuten evenwel de back-up op die de NFS-in plaats van 10 minuten sharemachtigingen, wanneer het schrijven naar een lokale stripe ingesteld.

![Het alternatief is veel sneller op 1 uur en 43 minuten niet](media/sap-hana-backup-file-level/image037.png)

De alternatieve manieren een back-up naar een lokale stripeset en worden gekopieerd naar de NFS-share op besturingssysteemniveau (een eenvoudige **cp - avr** opdracht) is niet veel sneller. Dit duurde 1 uur en 43 minuten.

Zodat deze werkt, maar prestaties is niet geschikt is voor de back-ups testen 230 GB. Het zou er zelfs slechter voor meerdere terabytes.

## <a name="copy-sap-hana-backup-files-to-azure-files"></a>Back-upbestanden voor SAP HANA op Azure-bestanden kopiëren

Het is mogelijk om te koppelen van een Azure-bestandsshare in een virtuele Azure Linux-machine. Het artikel [Azure File storage gebruiken met Linux](../../../storage/files/storage-how-to-use-files-linux.md) bevat gedetailleerde informatie over om dit te doen. Houd er rekening mee dat er momenteel een quotum van 5 TB-limiet van één Azure-bestandsshare en een maximale grootte van 1 TB per bestand. Zie [Azure Storage Scalability and Performance Targets](../../../storage/common/storage-scalability-targets.md) voor informatie over de maximale opslag.

Onderzoek heeft aangetoond, echter die SAP HANA-back-up niet&#39;t momenteel werk rechtstreeks met dit soort CIFS-koppelpunt. Het is ook vermeld in de [SAP-notitie 1820529](https://launchpad.support.sap.com/#/notes/1820529) die CIFS wordt niet aanbevolen.

![In deze afbeelding ziet u een fout in het dialoogvenster voor back-up in SAP HANA Studio](media/sap-hana-backup-file-level/image038.png)

Deze afbeelding ziet u een fout in het dialoogvenster voor back-up in SAP HANA Studio, bij het back-up rechtstreeks naar een CIFS gekoppeld Azure-bestandsshare. Dus een heeft een standaard back-up van SAP HANA in een bestandssysteem voor de virtuele machine eerst en kopieer vervolgens de back-upbestanden van daaruit naar Azure file-service.

![In deze afbeelding ziet u dat ongeveer 929 seconden nodig was voor het kopiëren van 19 back-upbestanden van SAP HANA](media/sap-hana-backup-file-level/image039.png)

In deze afbeelding ziet u dat dit ongeveer 929 seconden duurde 19 back-upbestanden van de SAP HANA met een totale grootte van ongeveer 230 GB kopiëren naar de Azure-bestandsshare.

![De bron-mapstructuur op de SAP HANA-VM is gekopieerd naar de Azure-bestandsshare](media/sap-hana-backup-file-level/image040.png)

In deze schermafbeelding kunnen zien dat de bron-mapstructuur op de SAP HANA-VM is gekopieerd naar de Azure-bestandsshare: één map (hana\_back-up\_fsl\_15 gb) en 19 afzonderlijke back-upbestanden.

Opslaan van back-upbestanden van de SAP HANA op Azure files kan een interessante optie worden in de toekomst wanneer back-ups van SAP HANA-ondersteuning bieden voor het rechtstreeks. Of wanneer u wordt mogelijk om te koppelen van Azure files via NFS en de limiet is aanzienlijk hoger is dan 5 TB.

## <a name="next-steps"></a>Volgende stappen
* [Back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md) biedt een overzicht en informatie over aan de slag.
* [Back-up van SAP HANA op basis van opslagmomentopnamen](sap-hana-backup-storage-snapshots.md) beschrijving van de opslag op basis van een momentopname van back-upoptie.
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).
