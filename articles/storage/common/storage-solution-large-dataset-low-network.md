---
title: Opties voor grote gegevenssets met weinig of geen bandbreedte van het netwerk van Azure de gegevensoverdracht | Microsoft Docs
description: Informatie over het kiezen van een Azure-oplossing voor de overdracht van wanneer u hebt beperkt tot er geen netwerkbandbreedte in uw omgeving en u van plan bent om over te dragen van grote gegevenssets.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 6922d49affdde86728b41ef1d3d0c6a2da5c323e
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805088"
---
# <a name="data-transfer-for-large-datasets-with-low-or-no-network-bandwidth"></a>Voor gegevensoverdracht voor grote gegevenssets met weinig of geen netwerkbandbreedte
 
In dit artikel bevat een overzicht van de gegevensoverdracht oplossingen bij u hebben beperkt tot er geen netwerkbandbreedte in uw omgeving en u van plan bent om over te dragen van grote gegevenssets. Het artikel beschrijft ook de aanbevolen data transfer-opties en de bijbehorende sleutel mogelijkhedenmatrix voor dit scenario.

Voor een overzicht van alle beschikbare overdracht Gegevensopties, gaat u naar [een Azure data transfer-oplossing kiezen](storage-choose-data-transfer-solution.md).

## <a name="offline-transfer-or-network-transfer"></a>Offline overdracht of netwerk overdracht

Grote gegevenssets impliceren dat er enkele TB aan enkele PBs van gegevens. U hebt beperkt tot er geen netwerkbandbreedte, het netwerk is traag of onbetrouwbaar is. Daarnaast:

- Er gelden limieten voor kosten voor netwerkoverdracht van uw internetproviders (ISP's).
- Beveiligings- of organisatiebeleid Sta geen uitgaande verbindingen wanneer omgaan met gevoelige gegevens.

Gebruik in de bovenstaande gevallen, een fysiek apparaat voor een eenmalige bulksgewijs gegevensoverdracht. Kies in de Data Box-schijf, Data Box, gegevens in het zware apparaten die worden geleverd door Microsoft of uw eigen schijven voor de Import/Export.

Gebruik de volgende tabel om te bevestigen of een fysiek apparaat de juiste optie. Het bevat de geschatte tijd voor de overdracht van netwerk, voor de verschillende beschikbare bandbreedten (ervan uitgaande dat 90% gebruik). Als netwerkoverdracht prognoses worden te langzaam verlopen, moet u een fysiek apparaat gebruiken.  

![Netwerkoverdracht of offline](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

## <a name="recommended-options"></a>Aanbevolen opties

De beschikbare opties in dit scenario zijn apparaten voor Azure Data Box offline overdracht of Azure Import/Export.

- **Azure Data Box-serie voor offline-overdrachten** – apparaten door Microsoft geleverde Data Box apparaten gebruiken voor het verplaatsen van grote hoeveelheden gegevens naar Azure wanneer u door de tijd, de beschikbaarheid van het netwerk of de kosten beperkt bent. On-premises gegevens kopiëren met behulp van hulpprogramma's zoals Robocopy. Afhankelijk van de gegevensgrootte die bestemd zijn voor de overdracht, kunt u kiezen uit Data Box-schijf, Data Box of gegevens in het zware.
- **Azure Import/Export** – gebruik Azure Import/Export-service door het verzenden van uw eigen schijven voor het veilig grote hoeveelheden gegevens importeren naar Azure Blob storage en Azure Files. Deze service kan ook worden gebruikt voor gegevensoverdracht van Azure Blob-opslag op schijven en verzend deze naar uw on-premises sites.

## <a name="comparison-of-key-capabilities"></a>Vergelijking van de belangrijkste mogelijkheden

De volgende tabel geeft een overzicht van de verschillen in de belangrijkste mogelijkheden.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy              |    Import/Export                       |
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
|    Prijzen                          |    [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/disk/)                    |   [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Prijzen](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


## <a name="next-steps"></a>Volgende stappen

- Begrijpen hoe u

    - [Gegevens overdragen met Data Box-schijf](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal).
    - [Gegevens overdragen met Data Box](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal).
    - [Gegevens overdragen met Import/Export](/azure/storage/common/storage-import-export-data-to-blobs).
