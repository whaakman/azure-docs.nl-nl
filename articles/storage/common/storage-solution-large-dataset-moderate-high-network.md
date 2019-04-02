---
title: Opties voor grote gegevenssets, matige tot hoge netwerkbandbreedte van Azure de gegevensoverdracht | Microsoft Docs
description: Leer hoe u een Azure-oplossing voor de overdracht van kiezen wanneer u gemiddeld in hoog netwerkbandbreedte in uw omgeving hebt en u van plan bent om over te dragen van grote gegevenssets.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 8dd55032c933cdc31b848addfdac991550376dcf
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805569"
---
# <a name="data-transfer-for-large-datasets-with-moderate-to-high-network-bandwidth"></a>Gegevensoverdracht voor grote gegevenssets met gemiddeld naar hoogste netwerkbandbreedte
 
In dit artikel biedt een overzicht van de gegevensoverdracht oplossingen wanneer u gemiddeld in hoog netwerkbandbreedte in uw omgeving hebt en u van plan bent om over te dragen van grote gegevenssets. Het artikel beschrijft ook de aanbevolen data transfer-opties en de bijbehorende sleutel mogelijkhedenmatrix voor dit scenario.

Voor een overzicht van alle beschikbare overdracht Gegevensopties, gaat u naar [een Azure data transfer-oplossing kiezen](storage-choose-data-transfer-solution.md).

## <a name="scenario-description"></a>Scenariobeschrijving

Grote gegevenssets verwijzen naar gegevensgroottes in volgorde van TB aan PBs. Gemiddeld naar hoogste netwerkbandbreedte verwijst naar 100 Mbps naar 10 Gbps.

## <a name="recommended-options"></a>Aanbevolen opties

De opties aanbevolen in dit scenario is afhankelijk van of u matige netwerkbandbreedte of hoge netwerkbandbreedte hebt.

### <a name="moderate-network-bandwidth-100-mbps---1-gbps"></a>Gemiddelde netwerkbandbreedte (100 Mbps - 1 Gbps)

Gemiddelde netwerkbandbreedte moet u de tijd voor de overdracht van project via het netwerk.

Gebruik de volgende tabel om te schatten van de tijd en op basis van die kiezen tussen een offline overdracht of via de netwerkoverdracht. De tabel bevat de geschatte tijd voor de overdracht van netwerk, voor de verschillende beschikbare netwerkbandbreedten (ervan uitgaande dat 90% gebruik).  

![Netwerkoverdracht of offline](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

- Als u de netwerkoverdracht en prognoses worden te langzaam verlopen, moet u een fysiek apparaat gebruiken. De aanbevolen opties zijn in dit geval de overdracht van offline apparaten uit Azure Data Box-familie of Azure Import/Export voor het gebruik van uw eigen schijven.

    - **Azure Data Box-serie voor offline-overdrachten** – apparaten door Microsoft geleverde Data Box apparaten gebruiken voor het verplaatsen van grote hoeveelheden gegevens naar Azure wanneer u door de tijd, de beschikbaarheid van het netwerk of de kosten beperkt bent. On-premises gegevens kopiëren met behulp van hulpprogramma's zoals Robocopy. Afhankelijk van de gegevensgrootte die bestemd zijn voor de overdracht, kunt u kiezen uit Data Box-schijf, Data Box of gegevens in het zware.
    - **Azure Import/Export** – gebruik Azure Import/Export-service door het verzenden van uw eigen schijven voor het veilig grote hoeveelheden gegevens importeren naar Azure Blob storage en Azure Files. Deze service kan ook worden gebruikt voor gegevensoverdracht van Azure Blob-opslag op schijven en verzend deze naar uw on-premises sites.

- Als u de netwerkoverdracht en prognoses redelijk, dan kunt u een van de volgende hulpprogramma's beschreven in [hoge netwerkbandbreedte](#high-network-bandwidth).


### <a name="high-network-bandwidth-1-gbps---100-gbps"></a>Hoge netwerkbandbreedte (1 Gbps - 100 Gbps)

Als de beschikbare netwerkbandbreedte hoog is, gebruik een van de volgende hulpprogramma's.

- **AzCopy** : Gebruik deze opdrachtregel-hulpprogramma eenvoudig om gegevens te kopiëren naar en van Azure-Blobs, bestanden, en Table storage met optimale prestaties. AzCopy biedt ondersteuning voor gelijktijdigheid van taken en parallelle uitvoering en de mogelijkheid om te hervatten kopieerbewerkingen wanneer onderbroken.
- **Azure Storage REST API's / SDK's** – bij het bouwen van een toepassing, kunt u de toepassing op basis van Azure Storage REST-API's ontwikkelen en gebruiken van de Azure-SDK's die worden aangeboden in meerdere talen.
- **Azure Data Box-familie voor online-overdrachten** -gegevens in het Edge- en Data Box-Gateway zijn online netwerkapparaten die gegevens van en naar Azure verplaatsen kunnen. Gebruik Edge van gegevens in het fysieke apparaat wanneer er een gelijktijdige nodig hebt voor continue gegevensopname en de voorverwerking van de voorafgaande gegevens wilt uploaden. Data Box-Gateway is een virtuele versie van het apparaat met de dezelfde mogelijkheden voor de overdracht van gegevens. In elk geval wordt wordt de overdracht van gegevens beheerd door het apparaat.
- **Azure Data Factory** : Data Factory moet worden gebruikt voor het opschalen van een overdrachtbewerking van, en indien nodig is voor de orchestration- en enterprise uitvoeren voor de mogelijkheden voor bewaking. Gebruik Data Factory regelmatig bestanden overbrengt tussen verschillende Azure-services, on-premises of een combinatie van beide. u kunt met Data Factory kunt maken en plannen van gegevensgestuurde werkstromen (pijplijnen genoemd) die gegevens uit verschillende gegevensarchieven opnemen en automatiseren gegevensverplaatsingen en gegevenstransformaties.

## <a name="comparison-of-key-capabilities"></a>Vergelijking van de belangrijkste mogelijkheden

De volgende tabellen geven een overzicht van de verschillen in de belangrijkste mogelijkheden voor de aanbevolen opties.

### <a name="moderate-network-bandwidth"></a>Gemiddelde netwerkbandbreedte

Als u offline gegevensoverdracht, gebruik de volgende tabel om te weten wat de verschillen in de belangrijkste mogelijkheden.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy            |    Import/Export                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    Gegevensgrootte                        |    Tot 35 TB                 |    Maximaal 80 TB per apparaat                       |    Maximaal 800 TB per apparaat               |    Variabele                            |
|    Gegevenstype                        |    Azure Blobs                  |    Azure Blobs<br>Azure Files                    |    Azure Blobs<br>Azure Files            |    Azure Blobs<br>Azure Files          |
|    Vormfactor                      |    5 SSD's per order             |    1 x 50-lbs. Desktop-formaat apparaat per order    |    1 X ~500-lbs. grote apparaat per order    |    Maximaal 10 HDD's / SSD's per order        |
|    Initiële insteltijd               |    Laag <br>(15 minuten)            |    Lage tot gemiddelde <br> (<30 mins)               |    Gemiddeld<br>(1-2 uur)               |    Gemiddelde tot moeilijk<br>(variabele) |
|    Gegevens verzenden naar Azure               |    Ja                          |    Ja                                           |    Ja                                   |    Ja                                 |
|    Gegevens exporteren van Azure           |    Nee                           |    Nee                                            |    Nee                                    |    Ja                                 |
|    Versleuteling                       |    AES 128-bit                  |    AES 256-bit                                   |    AES 256-bit                           |    AES 128-bit                         |
|    Hardware                         |     Microsoft geleverd          |    Microsoft geleverd                            |    Microsoft geleverd                    |    De klant worden verstrekt                   |
|    Netwerkinterface                |    USB-3.1/SATA                 |    RJ 45, SFP+                                   |    RJ45, QSFP+                           |    SATA II/SATA III                    |
|    Partnerintegratie              |    Sommige                         |    [Hoog](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                          |    [Hoog](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                  |    Sommige                                |
|    Verzenden                         |    Beheerd door Microsoft            |    Beheerd door Microsoft                             |    Beheerd door Microsoft                     |    Beheerd door de klant                    |
| Gebruik deze sjabloon wanneer gegevens worden verplaatst         |Binnen een grens commerce|Binnen een grens commerce|Binnen een grens commerce|Over de geografische grenzen, bijvoorbeeld VS EU|
|    Prijzen                          |    [Prijzen](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Prijzen](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


Als u online gegevensoverdracht, gebruikt u de tabel in de volgende sectie voor hoge netwerkbandbreedte.

### <a name="high-network-bandwidth"></a>Hoge netwerkbandbreedte

|                                     |    Hulpprogramma's-AzCopy <br>Azure PowerShell, <br>Azure-CLI             |    Azure Storage REST API's en SDK 's                   |    Data Box-Gateway of Data Box Edge          |    Azure Data Factory                                            |
|-------------------------------------|------------------------------------|----------------------------------------------|----------------------------------|-----------------------------------------------------------------------|
|    Gegevenstype                  |    Azure-Blobs, Azure Files, Azure-tabellen    |    Azure-Blobs, Azure Files, Azure-tabellen    |    Azure-Blobs, Azure Files                           |   Biedt ondersteuning voor 70 gegevensconnectors voor data-archieven en indelingen    |
|    Vormfactor                |    Opdrachtregelprogramma's                        |    Programma-interface                    |    Microsoft levert een virtuele <br>of het fysieke apparaat     |    -Service in Azure portal                                            |
|    Eerste eenmalige configuratie     |    Eenvoudig               |    Gemiddeld                       |    Eenvoudig (< 30 minuten) op Normaal (1-2 uur)            |    Uitgebreide                                                          |
|    Gegevens vooraf verwerken              |    Nee                                        |    Nee                                        |    Ja (rekenen met rand)                               |    Ja                                                                |
|    Overdracht uit andere clouds       |    Nee                                        |    Nee                                        |    Nee                                                    |    Ja                                                                |
|    Gebruikerstype                        |    IT Pro of ontwikkelen                                       |    Dev                                       |    IT Pro                                                |    IT Pro                                                             |
|    Prijzen                          |    Gratis, kosten voor uitgaand verkeer toepassen         |    Gratis, kosten voor uitgaand verkeer toepassen         |    [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                                               |    [Prijzen](https://azure.microsoft.com/pricing/details/data-factory/)                                                            |

## <a name="next-steps"></a>Volgende stappen

- [Leer hoe u gegevens overdragen met Import/Export](/azure/storage/common/storage-import-export-data-to-blobs).
- Begrijpen hoe u

    - [Gegevens overdragen met Data Box-schijf](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal).
    - [Gegevens overdragen met Data Box](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal).
- [Gegevens overdragen met AzCopy](/azure/storage/common/storage-use-azcopy-v10).
- Begrijpen hoe:
    - [Gegevens overdragen met Data Box Gateway](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
    - [Gegevens transformeren met gegevens in Edge voordat ze worden verzonden naar Azure](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute).
- [Meer informatie over het overbrengen van gegevens met Azure Data Factory](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal).
- De REST API's gebruikt voor het overbrengen van gegevens

    - [In .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [In Java](https://docs.microsoft.com/java/api/overview/azure/storage/client)
