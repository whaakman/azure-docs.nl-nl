---
title: Azure data transfer opties voor kleine gegevenssets met weinig tot matige netwerkbandbreedte | Microsoft Docs
description: Informatie over het kiezen van een Azure-oplossing voor gegevensoverdracht wanneer lage tot gemiddelde netwerkbandbreedte in uw omgeving hebt en u van plan bent om over te dragen van kleine gegevenssets.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 12/05/2018
ms.author: alkohli
ms.openlocfilehash: 3e6f4f3eb312f0d4d96a008c0944a9608d0bf4a7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58124300"
---
# <a name="data-transfer-for-small-datasets-with-low-to-moderate-network-bandwidth"></a>De overdracht van gegevens voor kleine gegevenssets met weinig aan gemiddelde netwerkbandbreedte
 
In dit artikel bevat een overzicht van de gegevensoverdracht oplossingen bij lage tot gemiddelde netwerkbandbreedte in uw omgeving hebt en u van plan bent om over te dragen van kleine gegevenssets. Het artikel beschrijft ook de aanbevolen data transfer-opties en de bijbehorende sleutel mogelijkhedenmatrix voor dit scenario.

Voor een overzicht van alle beschikbare overdracht Gegevensopties, gaat u naar [een Azure data transfer-oplossing kiezen](storage-choose-data-transfer-solution.md).

## <a name="scenario-description"></a>Scenariobeschrijving

Kleine gegevenssets verwijzen naar de gegevenshoeveelheden in de volgorde van GB's aan een aantal TB's. Lage tot gemiddelde netwerkbandbreedte impliceert 45 Mbps (T3-verbinding in het datacenter) tot 1 Gbps.

- Als u slechts een handvol bestanden overbrengt en u hoeft voor het automatiseren van de overdracht van gegevens, kunt u de hulpprogramma's met een grafische interface.
- Als u vertrouwd met Systeembeheer bent, kunt u vanaf de opdrachtregel of programmatische/scripthulpprogramma's.

## <a name="recommended-options"></a>Aanbevolen opties

De opties aanbevolen in dit scenario zijn:

- **Hulpprogramma's voor grafische interface** zoals Azure Storage Explorer en Azure Storage in Azure portal. Deze bieden een eenvoudige manier om uw gegevens bekijken en snel een groot aantal bestanden overbrengen.

    - **Azure Storage Explorer** -deze platformoverschrijdende hulpprogramma kunt u de inhoud van uw Azure storage-accounts beheren. Hiermee kunt u om te uploaden, downloaden en beheren van blobs, bestanden, wachtrijen, tabellen en Azure Cosmos DB-entiteiten. Gebruik dit met Blob storage voor blobs en mappen, beheren, evenals blobs uploaden en downloaden tussen uw lokale bestandssysteem en Blob-opslag, of tussen opslagaccounts.
    - **Azure-portal** : Azure Storage in Azure portal biedt een webinterface voor het verkennen van bestanden en upload bestanden één voor één nieuwe. Dit is een goede optie als u niet wilt installeren van alle hulpprogramma's of opdrachten voor het snel uw bestanden te verkennen, of u uploadt gewoon een aantal nieuwe labels.

- **Hulpprogramma's voor het uitvoeren van scripts/programmatische** zoals AzCopy/PowerShell/Azure CLI en Azure Storage REST-API's.

    - **AzCopy** : Gebruik deze opdrachtregel-hulpprogramma eenvoudig om gegevens te kopiëren naar en van Azure-Blobs, bestanden, en Table storage met optimale prestaties. AzCopy biedt ondersteuning voor gelijktijdigheid van taken en parallelle uitvoering en de mogelijkheid om te hervatten kopieerbewerkingen wanneer onderbroken.
    - **Azure PowerShell** : als u vertrouwd bent met Systeembeheer, gebruikers de Azure Storage-module gebruiken in Azure PowerShell gegevens over te dragen.
    - **Azure CLI** -gebruik van dit hulpprogramma platformoverschrijdende Azure-services beheren en gegevens uploaden naar Azure Storage.
    - **Azure Storage REST API's / SDK's** – bij het bouwen van een toepassing, kunt u de toepassing op basis van Azure Storage REST API's /-SDK's ontwikkelen en gebruiken van de Azure-clientbibliotheken beschikbaar in meerdere talen.


## <a name="comparison-of-key-capabilities"></a>Vergelijking van de belangrijkste mogelijkheden

De volgende tabel geeft een overzicht van de verschillen in de belangrijkste mogelijkheden.

| Functie | Azure Opslagverkenner | Azure Portal | AzCopy<br>Azure PowerShell<br>Azure-CLI | Azure Storage REST-API's of -SDK 's |
|---------|------------------------|--------------|-----------------------------------------|---------------------------------|
| Beschikbaarheid | Downloaden en installeren <br>Zelfstandige hulpprogramma | Webgebaseerde hulpprogramma's in Azure portal | Vanaf de opdrachtregel-hulpprogramma |Programmeerbare interfaces in .NET, Java, Python, JavaScript, C++, Go, Ruby en PHP |
| Grafische interface | Ja | Ja | Nee | Nee |
| Ondersteunde platforms | Windows, Mac, Linux | Webconsole |Windows, Mac, Linux |Alle platforms |
| Blob-opslagbewerkingen toegestaan<br>voor blobs en mappen | Uploaden<br>Downloaden<br>Beheren | Uploaden<br>Downloaden<br>Beheren |Uploaden<br>Downloaden<br>Beheren | Ja, aanpasbare |
| Toegestane Gen1 van Data Lake-opslag<br>bewerkingen voor bestanden en mappen | Uploaden<br>Downloaden<br>Beheren | Nee |Uploaden<br>Downloaden<br>Beheren                   | Nee |
| Bestand-opslagbewerkingen toegestaan<br>voor bestanden en mappen | Uploaden<br>Downloaden<br>Beheren | Uploaden<br>Downloaden<br>Beheren   |Uploaden<br>Downloaden<br>Beheren | Ja, aanpasbare |
| Tabel-opslagbewerkingen toegestaan<br>voor tabellen |Beheren | Nee |Ondersteuning van de tabel in versie 7 van AzCopy |Ja, aanpasbare|
| Toegestane Queue storage | Beheren | Nee  |Nee | Ja, kan worden aangepast|


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gegevens overdragen met Azure Storage Explorer](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-azure-storage-explorer).
- [Gegevens overdragen met AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

