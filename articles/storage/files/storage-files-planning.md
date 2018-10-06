---
title: Planning voor de implementatie van Azure Files | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van een implementatie van Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 0701049eb1aa86398e90484dbf21ef3781270fba
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831378"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planning voor de implementatie van Azure Files
[Azure Files](storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard SMB-protocol. Omdat Azure Files wordt volledig beheerd, is het veel eenvoudiger dan implementeren en beheren van een bestandsserver of een NAS-apparaat dat u deze in productie scenario's is geïmplementeerd. In dit artikel komen de onderwerpen om u te overwegen bij het implementeren van een Azure-bestandsshare voor gebruik in productieomgevingen binnen uw organisatie.

## <a name="management-concepts"></a>Concepten van Management
 Het volgende diagram illustreert de concepten van Azure Files management:

![Bestandsstructuur](./media/storage-files-introduction/files-concepts.png)

* **Opslagaccount**: alle toegang tot Azure Storage vindt plaats via een opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Share**: een File Storage-share is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bovenliggende share. Een account kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden, tot de 5 TiB totale capaciteit van de bestandsshare.

* **Map**: een optionele hiërarchie van mappen.

* **Bestand**: een bestand in de share. Een bestand mag maximaal 1 TiB in grootte.

* **URL-indeling**: voor aanvragen voor een Azure-bestandsshare gemaakt met de File REST-protocol, bestanden kunnen worden opgevraagd met behulp van de volgende URL-indeling:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/directories>/<file>
    ```

## <a name="data-access-method"></a>Data access-methode
Azure Files beschikt twee ingebouwde, handige data access-methoden die u afzonderlijk of in combinatie met elkaar worden verbonden, gebruiken kunt voor toegang tot uw gegevens:

1. **Direct cloudtoegang**: een Azure-bestandsshare kan worden gekoppeld door [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md), en/of [Linux](storage-how-to-use-files-linux.md) met de branche standaard Server Message Block (SMB) protocol of via de File REST-API. Lees- en schrijfbewerkingen op bestanden op de share met SMB, zijn er rechtstreeks op de bestandsshare in Azure. Voor het koppelen van een virtuele machine in Azure, de SMB-client in het besturingssysteem moet ondersteuning van ten minste SMB 2.1. Koppeling maken met on-premises, zoals op een Gebruikerswerkstation de SMB-client wordt ondersteund door het werkstation moet ondersteuning van ten minste SMB 3.0 (met versleuteling). Naast SMB, nieuwe toepassingen of services kunnen rechtstreeks toegang tot de bestandsshare via de REST-bestand, waarmee u een eenvoudige en schaalbare application programming interface voor het ontwikkelen van software.
2. **Azure File Sync**: met Azure File Sync, bestandsshares kunnen worden gerepliceerd naar Windows-Servers on-premises of in Azure. Uw gebruikers zou zoals toegang tot de bestandsshare via de Windows-Server, dat via een SMB- of NFS-share. Dit is handig voor scenario's waarin gegevens worden geopend en gewijzigd ver weg van een Azure-datacenter, zoals in een scenario voor het filiaal. Gegevens kunnen worden gerepliceerd tussen meerdere eindpunten voor Windows Server, zoals tussen meerdere filialen. Ten slotte kan worden gelaagde gegevens in Azure Files, zodat alle gegevens zijn nog steeds toegankelijk via de Server, maar de Server heeft geen een volledige kopie van de gegevens. In plaats daarvan gegevens naadloos ingetrokken wanneer door de gebruiker wordt geopend.

De volgende tabel ziet u hoe uw gebruikers en toepassingen toegang uw Azure-bestandsshare tot hebben:

| | Direct cloudtoegang | Azure File Sync |
|------------------------|------------|-----------------|
| Welke protocollen moet u gebruiken? | Azure Files biedt ondersteuning voor SMB 2.1, SMB 3.0 en File REST-API. | Toegang tot uw Azure-bestandsshare via een ondersteund protocol op Windows Server (SMB, NFS, FTPS, enz.) |  
| Waar worden u uw workload uitgevoerd? | **In Azure**: Azure Files biedt directe toegang tot uw gegevens. | **On-premises met een traag netwerk**: Windows, Linux en Mac OS-clients kunnen een lokale on-premises Windows-bestandsshare koppelen als een snelle cache van uw Azure-bestandsshare. |
| Welk niveau van ACL's hebt u nodig? | Het niveau van delen en de bestandsnaam. | Het niveau van delen, bestands- en gebruiker. |

## <a name="data-security"></a>Gegevensbeveiliging
Azure Files biedt verschillende ingebouwde opties voor het garanderen van de beveiliging van gegevens:

* Ondersteuning voor codering in beide protocollen over-the-wire: SMB 3.0-versleuteling en de REST-bestand via HTTPS. Standaard: 
    * Clients die ondersteuning bieden voor SMB 3.0-codering verzenden en ontvangen van gegevens via een versleuteld kanaal.
    * Clients die bieden geen ondersteuning voor SMB 3.0 met-codering kunnen intra-datacenter communiceren via het SMB 2.1 of SMB 3.0 zonder versleuteling. SMB-clients zijn niet toegestaan om te communiceren tussen datacenter via SMB 2.1 of SMB 3.0 zonder versleuteling.
    * Clients kunnen communiceren via de REST-bestand met HTTP of HTTPS.
* Versleuteling in rust ([Azure Storage-Serviceversleuteling](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): Storage Service Encryption (SSE) is ingeschakeld voor alle opslagaccounts. Gegevens in rust worden versleuteld met een volledig beheerde sleutels. Versleuteling in rust niet verhogen van de kosten voor opslag of de prestaties negatief beïnvloeden. 
* Optionele vereiste van versleutelde gegevens die worden verzonden: als u selecteert, Azure Files toegang tot de gegevens via niet-versleutelde kanalen geweigerd. Specifiek, zijn alleen HTTPS en SMB 3.0 met versleuteling verbindingen toegestaan. 

    > [!Important]  
    > Veilige overdracht van gegevens vereisen zorgt ervoor dat oudere SMB-clients niet geschikt voor communicatie met SMB 3.0 met-codering mislukken. Zie voor meer informatie, [koppelen in Windows](storage-how-to-use-files-windows.md), [koppelen in Linux](storage-how-to-use-files-linux.md), en [koppelen in macOS](storage-how-to-use-files-mac.md).

Voor een maximale beveiliging wordt aangeraden altijd inschakelen beide versleuteling in rust en inschakelen van versleuteling van gegevens die worden verzonden wanneer u van moderne clients gebruikmaakt toegang tot uw gegevens. Als u nodig hebt om te koppelen van een share op een Windows Server 2008 R2-VM, die alleen ondersteuning biedt voor SMB 2.1, moet u bijvoorbeeld niet-versleuteld verkeer naar uw storage-account toestaan omdat SMB 2.1 biedt geen ondersteuning voor versleuteling.

Als u Azure File Sync gebruikt voor toegang tot uw Azure-bestandsshare, zullen we de HTTPS en SMB 3.0 met versleuteling altijd gebruiken om te synchroniseren van uw gegevens met uw Windows-Servers, ongeacht of u versleuteling van inactieve gegevens nodig hebt.

## <a name="file-share-performance-tiers"></a>File share prestatielagen
Azure Files ondersteunt twee prestatielagen: standard en premium.

* **Standard-bestandsshares** worden ondersteund door roterende harde schijven (HDD's) waarmee u betrouwbare prestaties voor i/o-werkbelastingen die minder gevoelig zijn voor variaties in prestaties, zoals voor algemeen gebruik-bestandsshares en dev/test-omgevingen. Standard-bestandsshares zijn alleen beschikbaar in een betalen per gebruik factureringsmodel.
* **Premium-bestandsshares (preview)** worden ondersteund door SSD-schijven (SSD's) die consistent hoge prestaties en lage latentie en binnen enkele milliseconden voor de meeste i/o-bewerkingen voor de meeste i/o-intensieve workloads. Hierdoor kunt u ze geschikt zijn voor een groot aantal workloads, zoals databases, website hosting, ontwikkelomgevingen, enzovoort. Premium-bestandsshares zijn alleen beschikbaar in een ingerichte factureringsmodel.

### <a name="provisioned-shares"></a>Ingerichte shares
Premium-bestandsshares worden ingericht op basis van een vaste GiB/IOPS/doorvoer verhouding. Voor elke GiB ingericht, wordt de share één IOPS en doorvoer van 0,1 MiB/s tot de maximumlimiet per share worden uitgegeven. De minimaal toegestane inrichting is 100 GiB met minimale IOPS/doorvoer. Grootte van de bestandsshare op elk gewenst moment kan worden vergroot en verlaagd van elk gewenst moment maar kan elke 24 uur sinds de laatste toename worden verlaagd.

Alle shares kunnen beste vermogen burst maximaal drie IOP's per GiB van ingerichte opslag gedurende 60 minuten of langer, afhankelijk van de grootte van de share. Nieuwe shares beginnen met het volledige burst-tegoed op basis van de ingerichte capaciteit.

| Ingerichte capaciteit | 100 GiB | 500 GiB | 1 TiB | 5 TiB | 
|----------------------|---------|---------|-------|-------|
| Basislijn IOPS | 100 | 500 | 1,024 | 5.120 | 
| Burst-limiet | 300 | 1,500 | 3072 | 15,360 | 
| Doorvoer | 110 MiB/sec | 150 MiB/sec | 202 MiB/sec | 612 MiB/sec |

## <a name="file-share-redundancy"></a>File share redundantie
Azure Files ondersteunt drie opties voor gegevensredundantie: lokaal redundante opslag (LRS), zone-redundante opslag (ZRS) en geografisch redundante opslag (GRS). De volgende secties worden de verschillen tussen de verschillende redundantieopties voor:

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Zone-redundante opslag
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Geografisch redundante opslag
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Patroon van de groei van gegevens
De maximale grootte voor een Azure-bestandsshare is vandaag de dag van 5 TiB. Vanwege deze huidige beperking, u moet rekening houden met groei van de verwachte gegevens bij het implementeren van een Azure-bestandsshare. 

Het is mogelijk om te synchroniseren die meerdere Azure-bestandsshares op een enkele Windows-bestandsserver met Azure File Sync. Hiermee kunt u om ervoor te zorgen dat oudere, grote bestandsshares dat er on-premises naar Azure File Sync kunnen worden gebracht. Zie voor meer informatie, [plannen voor een implementatie van Azure File Sync](storage-files-planning.md).

## <a name="data-transfer-method"></a>Methode voor gegevensoverdracht
Er zijn veel eenvoudige opties voor het bulksgewijs overdracht van gegevens uit een bestaand bestand, zoals een bestandsshare on-premises naar Azure Files delen. Enkele populaire services zijn onder andere (niet-uitputtende lijst):

* **Azure File Sync**: als onderdeel van een eerste synchronisatie tussen een Azure-bestandsshare (een "Cloudeindpunt") en een Windows-directory-naamruimte (een "eindpunt voor de Server'), Azure File Sync van de bestaande bestandsshare alle gegevens worden gerepliceerd naar Azure Files.
* **[Azure Import/Export](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: de Azure Import/Export-service kunt u veilig grote hoeveelheden gegevens overdragen naar een Azure-bestandsshare met de verzending van harde schijven naar een Azure-datacenter. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: Robocopy is een bekende kopie-hulpprogramma dat wordt geleverd met Windows en Windows Server. Robocopy kan worden gebruikt om gegevens over naar Azure Files te koppelen van de bestandsshare lokaal en klik vervolgens met behulp van de gekoppelde locatie als het doel in de Robocopy-opdracht.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**: AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar en van Azure Files, evenals Azure Blob-opslag met behulp van eenvoudige opdrachten met optimale prestaties. AzCopy is beschikbaar voor Windows en Linux.

## <a name="next-steps"></a>Volgende stappen
* [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
* [Azure Files implementeren](storage-files-deployment-guide.md)
* [Azure File Sync implementeren](storage-sync-files-deployment-guide.md)
