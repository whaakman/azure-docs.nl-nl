---
title: Planning voor de implementatie van een Azure-bestanden | Microsoft Docs
description: Meer informatie over wat u moet overwegen bij het plannen van de implementatie van een Azure-bestanden.
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: c28f341fb64271e2173cd377fa06c567e0e054a6
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="planning-for-an-azure-files-deployment"></a>Planning voor de implementatie van Azure Files
[Azure Files](storage-files-introduction.md) biedt volledig beheerd bestandsshares in de cloud die toegankelijk zijn via het SMB-standaardprotocol. Omdat Azure Files volledig wordt beheerd, is het veel eenvoudiger dan implementeren en beheren van een bestandsserver of een NAS-apparaat dat u in productiescenario's implementeren. In dit artikel wordt de onderwerpen in overweging moet nemen bij het implementeren van een Azure-bestandsshare voor gebruik in productieomgevingen binnen uw organisatie.

## <a name="management-concepts"></a>Concepten van Management
 Het volgende diagram illustreert de bestanden van de Azure management-constructs:

![Bestandsstructuur](./media/storage-files-introduction/files-concepts.png)

* **Opslagaccount**: alle toegang tot Azure Storage vindt plaats via een opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Share**: een File Storage-share is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bovenliggende share. Een account kan een onbeperkt aantal shares bevatten en een bestandsshare kan een onbeperkt aantal bestanden, tot aan de 5 TiB totale capaciteit van de bestandsshare opslaan.

* **Map**: een optionele hiërarchie van mappen.

* **Bestand**: een bestand in de share. Een bestand mag maximaal 1 TiB in grootte.

* **URL-indeling**: voor aanvragen voor een Azure-bestandsshare gemaakt met het bestand REST-protocol, bestanden kunnen worden opgevraagd met de volgende URL-indeling:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/directories>/<file>
    ```

## <a name="data-access-method"></a>Data access-methode
Azure bestanden biedt twee ingebouwde, handige data access-methoden waarmee u kunt afzonderlijk of in combinatie met elkaar, toegang tot uw gegevens:

1. **Directe toegang tot de cloud**: een Azure-bestandsshare kan worden gekoppeld door [Windows](storage-how-to-use-files-windows.md), [Mac OS](storage-how-to-use-files-mac.md), en/of [Linux](storage-how-to-use-files-linux.md) met de branche standaard Server Message Block ( SMB)-protocol of via de REST-API van het bestand. Lees- en schrijfbewerkingen op bestanden op de share met SMB, zijn er rechtstreeks op de bestandsshare in Azure. Koppelen door een virtuele machine in Azure, de SMB-client in het besturingssysteem moet ondersteuning van ten minste SMB 2.1. On-premises kunt koppelen, zoals op een gebruiker werkstation de SMB-client wordt ondersteund door het werkstation moet ondersteuning van ten minste SMB 3.0 (met versleuteling). Naast SMB, nieuwe toepassingen of services kunnen rechtstreeks toegang tot de bestandsshare via de REST-bestand, dat voorziet in een eenvoudige en schaalbare application programming interface voor de ontwikkeling van software.
2. **Azure File-synchronisatie** (preview): met Azure bestand Sync shares kunnen worden gerepliceerd naar Windows-Servers on-premises of in Azure. Uw gebruikers zou zoals toegang tot de bestandsshare via de Windows-Server dat via een SMB- of NFS-share. Dit is nuttig voor scenario's waarin gegevens worden geopend en gewijzigd ver van een Azure-datacenter, zoals in een scenario voor het filiaal. Gegevens kunnen worden gerepliceerd tussen meerdere eindpunten voor Windows Server, zoals tussen meerdere filialen. Ten slotte gegevens mogelijk tiers worden verdeeld naar Azure-bestanden, zodat alle gegevens zijn nog steeds toegankelijk via de Server, maar de Server geen een volledige kopie van de gegevens heeft. In plaats daarvan gegevens naadloos teruggehaald wanneer geopend door de gebruiker.

De volgende tabel ziet u hoe uw gebruikers en toepassingen toegang uw Azure-bestandsshare tot hebben:

| | Toegang tot de directe cloud | Azure File-synchronisatie |
|------------------------|------------|-----------------|
| Welke protocollen moet u gebruiken? | Azure Files SMB 2.1, SMB 3.0 en File REST-API ondersteunt. | Toegang tot uw Azure-bestandsshare via elke ondersteunde protocollen op Windows Server (SMB, NFS, FTPS, enz.) |  
| Waar wordt u uw workload uitgevoerd? | **In Azure**: Azure Files biedt directe toegang tot uw gegevens. | **On-premises met traag netwerk**: Windows, Linux en Mac OS-clients een lokale lokale Windows-bestandsshare als een snelle cache van uw Azure-bestandsshare kunnen koppelen. |
| Welk niveau van ACL's hebt u nodig? | Share- en niveau. | Share-, bestands- en gebruiker niveau. |

## <a name="data-security"></a>Gegevensbeveiliging
Azure Files bevat verschillende ingebouwde opties voor optimale gegevensbeveiliging:

* Ondersteuning voor codering in beide protocollen via de kabel: versleuteling door SMB 3.0 en File REST via HTTPS. Standaard: 
    * Clients die ondersteuning bieden voor versleuteling door SMB 3.0 verzenden en ontvangen van gegevens via een versleuteld kanaal.
    * Clients die geen ondersteuning bieden voor SMB 3.0 kunnen zonder versleuteling intra-datacenter communiceren via SMB 2.1 of SMB 3.0. Houd er rekening mee dat clients niet inter datacenter communiceren via SMB 2.1 of SMB 3.0 zonder versleuteling zijn toegestaan.
    * Clients kunnen communiceren via de REST-bestand met HTTP of HTTPS.
* Codering in rust ([Azure Storage-Service: versleuteling](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): We zijn bezig met inschakelen van versleuteling voor opslag-Service (SSE) op de onderliggende opslag van de Azure-platform. Dit betekent dat versleuteling wordt standaard voor alle opslagaccounts worden ingeschakeld. Als u een nieuw opslagaccount in een regio met versleuteling in rust op standaard maakt, hebt u niet te ondernemen om in te schakelen. Gegevens in rust versleuteld met een volledig beheerde sleutels. Codering in rust niet opslagkosten verhogen of de prestaties negatief beïnvloeden. 
* Optionele vereiste van versleutelde gegevens in transit: wanneer u selecteert, Azure Files staat niet toe dat toegang tot de gegevens via niet-versleutelde kanalen. In het bijzonder wordt alleen HTTPS en SMB 3.0 met versleuteling verbindingen worden toegestaan. 

    > [!Important]  
    > Veilige overdracht van gegevens vereisen zorgt ervoor dat oudere SMB-clients niet kan communiceren met SMB 3.0 met versleuteling mislukken. Zie [koppelen aan Windows](storage-how-to-use-files-windows.md), [koppelen op Linux](storage-how-to-use-files-linux.md), [koppelen op Mac OS](storage-how-to-use-files-mac.md) voor meer informatie.

Voor een optimale beveiliging wordt aangeraden altijd inschakelen beide versleuteling in rust en codering van gegevens in doorvoer inschakelen wanneer u moderne clients gebruikt voor toegang tot uw gegevens. Als u koppelen van een share op een Windows Server 2008 R2 VM, die alleen SMB 2.1 wilt ondersteunt, moet u bijvoorbeeld niet-versleuteld verkeer naar uw opslagaccount staan omdat SMB 2.1 biedt geen ondersteuning voor versleuteling.

Als u Azure File-synchronisatie gebruikt voor toegang tot uw Azure-bestandsshare, altijd gebruiken we HTTPS en SMB 3.0 met versleuteling uw gegevens naar uw Windows-Servers, ongeacht of het vereisen van versleuteling van gegevens in rust te synchroniseren.

## <a name="data-redundancy"></a>Gegevensredundantie
Azure Files ondersteunt twee opties voor redundantie van gegevens: lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS). De volgende secties worden de verschillen tussen lokaal redundante opslag en geografisch redundante opslag:

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="geo-redundant-storage"></a>Geografisch redundante opslag
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Patroon van de groei van gegevens
De maximale grootte voor een Azure-bestandsshare is vandaag de dag 5 TiB, met inbegrip van momentopnamen van de share. Vanwege deze huidige beperking, moet u de groei van de verwachte gegevens overwegen bij het implementeren van een Azure-bestandsshare. Houd er rekening mee dat een Azure Storage-account kan worden gebruikt voor het opslaan van meerdere shares met een totaal van 500 TiB opgeslagen in alle shares.

Het is mogelijk meerdere Azure-bestandsshares op een enkele Windows-bestandsserver synchroniseren met Azure File-synchronisatie. Hiermee kunt u om ervoor te zorgen dat oudere, zeer grote bestandsshares dat er lokaal naar Azure bestand Sync kunnen worden gebracht. Zie [plannen voor de implementatie van een Azure-bestand Sync](storage-files-planning.md) voor meer informatie.

## <a name="data-transfer-method"></a>Gegevensoverdracht-methode
Er zijn veel eenvoudige opties bulksgewijs overdracht gegevens uit een bestaand bestand, zoals een bestandsshare lokaal naar Azure-bestanden delen. Enkele populaire daarvan zijn (niet-uitputtende lijst):

* **Azure File-synchronisatie**: als onderdeel van een eerste synchronisatie tussen een Azure-bestandsshare (een ' Cloudeindpunt') en een Windows-directory-naamruimte (een ' eindpunt voor de Server'), Azure bestand synchronisatie van de bestaande bestandsshare alle gegevens worden gerepliceerd naar Azure-bestanden.
* **[Azure Import/Export](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: de Azure Import/Export-service kunt u veilig overbrengen grote hoeveelheden gegevens in een Azure-bestandsshare back-upfunctie harde schijven naar een Azure-datacenter. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: Robocopy is een bekende kopie-hulpprogramma dat wordt geleverd met Windows en Windows Server. Robocopy kan worden gebruikt om gegevens over naar Azure-bestanden te koppelen van de bestandsshare lokaal en vervolgens de gekoppelde locatie als de bestemming in de Robocopy-opdracht.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**: AzCopy is een opdrachtregelprogramma dat is ontworpen voor het kopiëren van gegevens naar en van de Azure-bestanden, evenals Azure Blob-opslag met behulp van eenvoudige opdrachten met optimale prestaties. AzCopy is beschikbaar voor Windows en Linux.

## <a name="next-steps"></a>Volgende stappen
* [Planning voor een implementatie van Azure File-synchronisatie](storage-sync-files-planning.md)
* [Azure Files implementeren](storage-files-deployment-guide.md)
* [Azure File synchronisatie implementeren](storage-sync-files-deployment-guide.md)