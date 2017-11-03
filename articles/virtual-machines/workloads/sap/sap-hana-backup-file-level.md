---
title: SAP HANA Azure back-up op bestandsniveau | Microsoft Docs
description: Er zijn twee primaire back-mogelijkheden voor SAP HANA op virtuele machines in Azure, in dit artikel bevat informatie over SAP HANA Azure Backup op bestandsniveau
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: 5db0ceb1648b5afa278e1cbe1c42fce8033bfdc1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sap-hana-azure-backup-on-file-level"></a>SAP HANA Azure back-up op bestandsniveau

## <a name="introduction"></a>Inleiding

Dit maakt deel uit van een reeks drie delen met verwante artikelen op SAP HANA back-up. [Back-handleiding voor SAP HANA op Azure Virtual Machines](./sap-hana-backup-guide.md) biedt een overzicht en informatie over het aan de slag, en [SAP HANA back-up op basis van opslag-momentopnamen](./sap-hana-backup-storage-snapshots.md) wordt ingegaan op de opslag op basis van een momentopname optie back-up.

Kijken naar de Azure VM-grootten, kunnen zien dat een GS5 64 bijgesloten gegevensschijven toestaat. Voor grote SAP HANA-systemen, mogelijk al een groot aantal schijven worden gebruikt voor gegevens en logboekbestanden, mogelijk in combinatie met software-RAID voor optimale schijf-i/o-doorvoer. Vervolgens de vraag waar u voor het opslaan van de back-upbestanden SAP HANA, die de schijven bijgesloten gegevens gedurende een bepaalde periode kunnen vol is? Zie [grootten voor virtuele Linux-machines in Azure](../../linux/sizes.md) voor de tabellen van de grootte van virtuele machine van Azure.

Er is geen back-up SAP HANA-integratie met Azure Backup-service beschikbaar op dit moment. De standaardmethode voor het beheren van back-up/herstel op bestandsniveau, is met een back-ups via SAP HANA Studio of een SAP HANA-SQL-instructies. Zie [SAP HANA SQL en System weergaven verwijzing](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) voor meer informatie.

![Deze afbeelding ziet u het dialoogvenster van de back-menu-item in SAP HANA-Studio](media/sap-hana-backup-file-level/image022.png)

Deze afbeelding ziet het dialoogvenster van de back-menu-item in SAP HANA Studio. Bij het kiezen van type &quot;bestand&quot; een heeft een pad opgeven in het bestandssysteem waar SAP HANA schrijft de back-upbestanden. Herstel werkt op dezelfde manier.

Deze keuze klinkt eenvoudig en eenvoudig, maar er zijn enkele overwegingen. Zoals al eerder vermeld, heeft een virtuele machine in Azure een beperking van het aantal gegevensschijven dat kan worden gekoppeld. Er mogelijk geen capaciteit voor het opslaan van back-upbestanden op het bestandssysteem van de virtuele machine, afhankelijk van de grootte van de database en schijf doorvoer, waarbij software SAP HANA RAID striping over meerdere schijven gebruiken. Verschillende opties voor het verplaatsen van deze back-upbestanden en het beheren van het bestand groottebeperkingen en prestaties bij het verwerken van terabytes aan gegevens, vindt u verderop in dit artikel.

Een andere optie, die meer vrijheid met betrekking tot de totale capaciteit biedt, is Azure blob-opslag. Terwijl één blob ook beperkt tot 1 TB wordt, is de totale capaciteit van een enkele blob-container momenteel 500 TB. Bovendien geeft klanten de keuze om te selecteren, zogenaamde &quot;cool&quot; blob-opslag, waarmee een voordeel kosten heeft. Zie [Azure Blob Storage: Hot en cool opslaglagen](../../../storage/blobs/storage-blob-storage-tiers.md) voor meer informatie over cool blob-opslag.

Gebruik een geogerepliceerde storage-account voor het opslaan van de SAP HANA back-ups voor extra veiligheid. Zie [Azure Storage-replicatie](../../../storage/common/storage-redundancy.md) voor meer informatie over de storage-account-replicatie.

Een kan toegewezen virtuele harde schijven voor SAP HANA back-ups in een toegewezen back-upopslag-account dat geogerepliceerde plaatsen. Anders een kan de virtuele harde schijven die de SAP HANA back-ups behouden geogerepliceerde storage-account of een opslagaccount die zich in een andere regio worden gekopieerd.

## <a name="azure-backup-agent"></a>Azure backup-agent

Azure backup biedt de optie voor het niet alleen back-up van VM's zijn voltooid, maar ook bestanden en mappen via de back-agent, die moet worden geïnstalleerd op het gastbesturingssysteem. Maar deze agent wordt vanaf December 2016 wordt alleen ondersteund op Windows (Zie [Back-up van een Windows Server of client in Azure maken met het implementatiemodel van Resource Manager](../../../backup/backup-configure-vault.md)).

Er is een tijdelijke oplossing eerst SAP HANA back-upbestanden kopiëren naar een virtuele Windows-machine in Azure (bijvoorbeeld via SAMBA-share) en vervolgens met de Azure backup agent daar. Hoewel het technisch mogelijk is, zou deze complexiteit en vertragen de back-up of herstel van proces nogal vanwege de kopie tussen de Linux- en de virtuele machine van Windows. Het is niet raadzaam deze benadering volgen.

## <a name="azure-blobxfer-utility-details"></a>Azure blobxfer hulpprogramma details

Voor het opslaan van de mappen en bestanden op Azure-opslag kan een CLI of PowerShell gebruiken of ontwikkelt u een hulpprogramma met een van de [Azure SDK's](https://azure.microsoft.com/downloads/). Er is ook een kant-en-klare hulpprogramma, AzCopy, voor het kopiëren van gegevens naar Azure-opslag, maar dit is alleen Windows (Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma hulpprogramma](../../../storage/common/storage-use-azcopy.md)).

Daarom is blobxfer gebruikt voor het kopiëren van de back-upbestanden SAP HANA. Het is open-source, die wordt gebruikt door veel klanten in een productieomgeving en beschikbaar zijn op [GitHub](https://github.com/Azure/blobxfer). Dit hulpprogramma kunt een om gegevens te kopiëren rechtstreeks naar Azure blob-opslag of Azure-bestandsshare. Biedt ook een aantal handige functies, zoals md5-hash of automatische parallelle uitvoering bij het kopiëren van een map met meerdere bestanden.

## <a name="sap-hana-backup-performance"></a>SAP HANA back-upprestaties

![Deze schermafbeelding is van de back-up SAP HANA-console in SAP HANA-Studio](media/sap-hana-backup-file-level/image023.png)

Deze schermafbeelding is van de back-up SAP HANA-console in SAP HANA Studio. Ongeveer 42 minuten nodig was voor de back-up maken van de 230 GB op een enkele Azure standard-opslag-schijf is gekoppeld aan de HANA-VM met XFS file system.

![Deze schermafbeelding is van het YaST op de testmachine SAP HANA VM](media/sap-hana-backup-file-level/image024.png)

Deze schermafbeelding is van YaST op de SAP HANA virtuele testmachine. De één schijf van 1 TB voor SAP HANA back-up kunnen zien zoals al eerder vermeld. Ongeveer 42 minuten nodig was voor het back-230 GB. Bovendien vijf 200 GB schijven zijn gekoppeld en software-RAID-md0 met striping boven op deze vijf Azure gegevensschijven gemaakt.

![De dezelfde back-up op de software te herhalen RAID met striping over vijf gegevensschijven Azure standard-opslag gekoppeld](media/sap-hana-backup-file-level/image025.png)

De dezelfde back-up op de software te herhalen RAID met striping over vijf gekoppeld Azure standard-opslag gegevensschijven gebracht van de back-uptijd van 42 minuten naar beneden op 10 minuten. De schijven zijn zonder caching naar de virtuele machine gekoppeld. Het is daarom duidelijk hoe belangrijk schijf schrijfbewerkingen is voor de back-uptijd. Een kan vervolgens overschakelen naar Azure premium-opslag naar het proces voor optimale prestaties nog verder te versnellen. In het algemeen moet Azure premium-opslag worden gebruikt voor productiesystemen.

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>Kopieer van SAP HANA back-upbestanden naar Azure blob storage

Vanaf December 2016 is de beste optie voor het opslaan van back-upbestanden voor SAP HANA snel Azure blob-opslag. Één enkele blob-container kent een limiet van 500 TB, voldoende is voor de meeste SAP HANA-systemen, in een VM GS5 uitgevoerd op Azure, zodat back-ups met voldoende SAP HANA. Klanten hebben de keuze tussen &quot;hot&quot; en &quot;koude&quot; blob-opslag (Zie [Azure Blob Storage: Hot en cool opslaglagen](../../../storage/blobs/storage-blob-storage-tiers.md)).

Met het hulpprogramma blobxfer al het snel kopiëren van de back-upbestanden voor SAP HANA rechtstreeks naar Azure blob storage.

![Hier ziet een de bestanden van een volledige SAP HANA bestandsback-up](media/sap-hana-backup-file-level/image026.png)

Hier ziet een de bestanden van een volledige SAP HANA bestandsback-up. Er zijn vier bestanden en de grootste heeft ongeveer 230 GB.

![Het heeft geduurd ongeveer 3000 seconden de 230 GB kopiëren naar een blob-container van Azure standard-opslag-account](media/sap-hana-backup-file-level/image027.png)

Md5-hash niet gebruikt in de eerste test, nodig die ongeveer 3000 seconden de 230 GB kopiëren naar een blob-container van Azure standard-opslag-account.

![In deze schermafbeelding kunnen zien hoe het eruit ziet in de Azure portal](media/sap-hana-backup-file-level/image028.png)

In deze schermafbeelding kunnen zien hoe het eruit ziet op de Azure-portal. Een blob-container met de naam &quot;sap-hana-back-ups&quot; is gemaakt en bevat de vier blobs die de back-upbestanden voor SAP HANA vertegenwoordigen. Een van beide heeft een grootte van ongeveer 230 GB.

De back-console HANA Studio kunt een voor de maximale bestandsgrootte van de back-upbestanden HANA beperken. In de voorbeeldomgeving, moet deze prestaties verbeterd doordat het mogelijk om meerdere kleinere back-upbestanden, in plaats van een grote 230 GB-bestand.

![Het instellen van de limiet voor de back-upbestand op het HANA kant bestaat &#39; t verbetering van de back-uptijd](media/sap-hana-backup-file-level/image029.png)

Het instellen van de limiet voor de back-upbestand op het HANA kant bestaat &#39; t de back-uptijd verbeteren omdat de bestanden opeenvolgend zoals weergegeven in deze afbeelding zijn geschreven. De maximale bestandsgrootte is ingesteld op 60 GB, zodat de back-up vier grote gegevensbestanden in plaats van het bestand 230 GB gemaakt.

![Als u wilt testen parallelle uitvoering van het hulpprogramma blobxfer, is klikt u vervolgens de maximale bestandsgrootte voor HANA back-ups ingesteld op 15 GB](media/sap-hana-backup-file-level/image030.png)

Als u wilt testen parallelle uitvoering van het hulpprogramma blobxfer, is de maximale bestandsgrootte voor back-ups HANA wordt ingesteld op 15 GB, wat tot 19 back-upbestanden leidde. Deze configuratie brengen van de tijd voor blobxfer de 230 GB kopiëren naar Azure blob-opslag van 3000 seconden naar beneden 875 seconden.

Dit resultaat is vanwege de limiet van 60 MB per seconde voor het schrijven van een Azure-blob. Parallelle uitvoering via meerdere blobs het knelpunt is opgelost, maar er is een neerwaartse: verhogen van de prestaties van het hulpprogramma blobxfer al deze HANA back-bestanden kopiëren naar Azure blob-opslag laden plaatst op zowel de HANA VM als het netwerk. Werking van HANA systeem wordt beïnvloed.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>BLOB-kopie van het toegewezen Azure gegevensschijven in de back-upsoftware RAID

In tegenstelling tot de handmatige VM gegevens schijfback-up, in hiervan een registreert back-up van de gegevensschijven op een virtuele machine op te slaan van de volledige installatie van het SAP HANA-gegevens, inclusief HANA niet bestanden en configuratiebestanden. In plaats daarvan is het idee zijn speciaal bestemd voor software-RAID met striping over meerdere Azure-gegevens VHD's voor het opslaan van een volledige SAP HANA bestandsback-up. Een alleen deze schijven, die de SAP HANA back-up hebt gekopieerd. Ze kunnen eenvoudig worden bewaard in een speciale HANA back-storage-account of gekoppeld aan een speciaal &quot;back-up management VM&quot; voor verdere verwerking.

![Alle VHD's die zijn betrokken zijn gekopieerd met behulp van de ** start-azurestorageblobcopy ** PowerShell-opdracht](media/sap-hana-backup-file-level/image031.png)

Nadat de back-up op de lokale RAID-software is voltooid, alle VHD's die zijn betrokken zijn gekopieerd met behulp van de **start azurestorageblobcopy** PowerShell-opdracht (Zie [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy)). Dit geldt alleen voor het specifieke bestandssysteem voor het bewaren van de back-upbestanden, zijn er geen opmerkingen over SAP HANA gegevensbestand of logboekbestand bestand consistentiecontrole op de schijf. Een voordeel van deze opdracht is dat deze werkt terwijl de virtuele machine online blijft. Om er zeker van zijn dat er geen proces naar de back-stripeset schrijft, moet u deze voordat u de kopie van de blob ontkoppelen en daarna opnieuw koppelen. Of een zou een juiste manier om te kunnen gebruiken &quot;blokkeren&quot; het bestandssysteem. Bijvoorbeeld, via xfs\_blokkeren voor het bestandssysteem XFS.

![Deze schermafbeelding ziet u de lijst met blobs in de container VHD's op de Azure-portal](media/sap-hana-backup-file-level/image032.png)

Deze schermafbeelding ziet u de lijst met blobs in de &quot;VHD's&quot; container in de Azure portal. De schermafbeelding ziet u de vijf VHD's zijn gekoppeld aan de server voor SAP HANA VM als de software-RAID SAP HANA back-bestanden moeten worden behouden. U ziet ook de vijf exemplaren zijn uitgevoerd via de opdracht blob kopiëren.

![Voor testdoeleinden, zijn de kopieën van de SAP HANA back-upsoftware RAID-schijven gekoppeld aan de appserver VM](media/sap-hana-backup-file-level/image033.png)

Voor testdoeleinden zijn de kopieën van de SAP HANA back-upsoftware RAID-schijven gekoppeld aan de appserver VM.

![Kopieert de appserver die VM koppelen van de schijf is afgesloten](media/sap-hana-backup-file-level/image034.png)

De appserver-machine is afgesloten koppelen van de schijf gekopieerd. Na het starten van de virtuele machine de schijven en de RAID zijn ontdekt correct (gekoppeld via UUID). Alleen het koppelpunt ontbreekt, is die via de partitioner YaST gemaakt. Daarna de exemplaren van de back-upbestand SAP HANA is geworden zichtbaar op OS-niveau.

## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>Kopieer de back-upbestanden SAP HANA naar NFS-share

Als u wilt verkleinen, de mogelijke gevolgen voor de SAP HANA-systeem van een prestatie- of schijfruimte, overwegen een de SAP HANA back-bestanden opslaan op een NFS-share. Technisch gezien het werkt, maar het betekent als de host van de NFS-share met behulp van een tweede virtuele Azure-machine. Mag niet een kleine VM-grootte, vanwege de bandbreedte van de VM-netwerk. Het zinvol is vervolgens dit afsluiten &quot;back-up van VM&quot; en alleen voor het uitvoeren van de SAP HANA back-up te brengen. Schrijven van een NFS share plaatst belasting op het netwerk en van invloed is op het SAP HANA-systeem, maar alleen het beheren van de back-upbestanden daarna op de &quot;back-up van VM&quot; het SAP HANA-systeem niet helemaal zou beïnvloeden.

![Een NFS-share van een andere Azure-virtuele machine is gekoppeld aan de server voor SAP HANA VM](media/sap-hana-backup-file-level/image035.png)

Om te controleren of dat de NFS-gebruiksvoorbeeld, is een NFS-share van een andere virtuele machine van Azure naar de server voor SAP HANA VM gekoppeld. Er is geen speciale NFS afstemmen toegepast.

![Het heeft geduurd 1 uur en 46 minuten rechtstreeks uitvoeren van de back-up](media/sap-hana-backup-file-level/image036.png)

De NFS-share is een snelle stripeset, zoals een op de server voor SAP HANA. Niettemin geduurd 1 uur en 46 minuten de back-up maken op die de NFS-sharemachtigingen in plaats van 10 minuten bij het schrijven naar een lokale streep instellen.

![Het alternatief is niet veel sneller op 1 uur en 43 minuten](media/sap-hana-backup-file-level/image037.png)

Het alternatief van een back-up naar een lokale stripeset doen en kopiëren naar de NFS-share op de OS-niveau (een eenvoudige **cp - avr** opdracht) is niet veel sneller. 1 uur en 43 minuten nodig was.

Zodat het werkt, maar prestaties is niet geschikt is voor de back-test 230 GB. Het eruitziet zelfs slechter voor meerdere terabytes.

## <a name="copy-sap-hana-backup-files-to-azure-file-service"></a>Back-upbestanden voor SAP HANA kopiëren naar Azure file-service

Het is mogelijk om een Azure-bestandsshare binnen een Azure Linux VM te koppelen. Het artikel [Azure File storage gebruiken met Linux](../../../storage/files/storage-how-to-use-files-linux.md) biedt details over hoe u dit doet. Houd er rekening mee dat er momenteel een quotumlimiet van 5 TB van een Azure-bestandsshare en een maximale grootte van 1 TB per bestand is. Zie [Azure Storage Scalability and Performance Targets](../../../storage/common/storage-scalability-targets.md) voor meer informatie over opslaglimieten.

Tests is gebleken, maar die niet van SAP HANA-back-up &#39; t momenteel rechtstreeks met dit soort CIFS koppelpunten werkt. Het is ook vermeld in de [SAP-notitie 1820529](https://launchpad.support.sap.com/#/notes/1820529) die CIFS wordt niet aanbevolen.

![Deze afbeelding ziet u een fout opgetreden in het dialoogvenster back-up in SAP HANA-Studio](media/sap-hana-backup-file-level/image038.png)

Deze afbeelding ziet u een fout opgetreden in het dialoogvenster back-up in SAP HANA Studio als u wilt back-ups rechtstreeks naar een CIFS gekoppelde Azure-bestandsshare. Dus een heeft een standaard SAP HANA back-up eerst doen in het bestandssysteem van een virtuele machine en kopieer de back-upbestanden van daaruit naar Azure file-service.

![Deze afbeelding ziet u dat het heeft geduurd ongeveer 929 seconden 19 SAP HANA back-upbestanden kopiëren](media/sap-hana-backup-file-level/image039.png)

Deze afbeelding ziet u dat het heeft geduurd ongeveer 929 seconden 19 SAP HANA back-upbestanden met een totale grootte van ongeveer 230 GB kopiëren naar de Azure-bestandsshare.

![De bron-mapstructuur op de SAP HANA-virtuele machine is gekopieerd naar de Azure-bestandsshare](media/sap-hana-backup-file-level/image040.png)

In deze schermafbeelding kunnen zien dat de bron-mapstructuur op de SAP HANA-virtuele machine is gekopieerd naar de Azure-bestandsshare: één map (hana\_back-\_fsl\_15 gb) en 19 afzonderlijke back-upbestanden.

Opslaan van back-upbestanden op Azure files SAP HANA mogelijk een interessante optie in de toekomst wanneer SAP HANA back-ups rechtstreeks ondersteunen. Of wanneer u wordt mogelijk te koppelen Azure bestanden via NFS en de limiet is aanzienlijk hoger is dan 5 TB.

## <a name="next-steps"></a>Volgende stappen
* [Back-handleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md) biedt een overzicht en informatie over aan de slag.
* [SAP HANA back-up op basis van opslag-momentopnamen](sap-hana-backup-storage-snapshots.md) opslag op basis van een momentopname back-up wordt de optie beschreven.
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md).
