---
title: Bronnen voor & Azure CosmosDB tabel API .NET SDK | Microsoft Docs
description: Meer informatie over de Azure Cosmos DB tabel API met inbegrip van release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie.
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: 9dc0f5140a538c3a359dd90b74de822dc163fd70
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB tabel .NET API: Downloaden en release-opmerkingen
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://aka.ms/acdbtablenuget)|
|**API-documentatie**|[.NET API-naslagdocumentatie](https://aka.ms/acdbtableapiref)|
|**Snelstartgids**|[Azure Cosmos DB: Een app met .NET- en de tabel-API maken](create-table-dotnet.md)|
|**Zelfstudie**|[Azure Cosmos DB: Ontwikkelen met de tabel API in .NET](tutorial-develop-table-dotnet.md)|
|**Huidige ondersteunde framework**|[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)|

> [!IMPORTANT]
> Als u een tabel-API-account hebt gemaakt tijdens de preview, maakt u een [nieuwe tabel API account](create-table-dotnet.md#create-a-database-account) werken met de SDK algemeen beschikbaar tabel-API's.
>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Algemene beschikbaarheid release

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-Preview
* Initiële preview-versie

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk. 

De aanvragen die naar Azure Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.
<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.0.0](#1.0.0) |15 november 2017|--- |
| [0.9.0-Preview](#0.1.0-preview) |11 november 2017 |--- |

## <a name="troubleshooting"></a>Problemen oplossen

Als het foutbericht 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

bij een poging tot het Microsoft.Azure.CosmosDB.Table NuGet-pakket gebruiken, hebt u twee opties om het probleem te verhelpen:

* Pakket beheren Console gebruiken voor het installeren van het pakket Microsoft.Azure.CosmosDB.Table en de bijbehorende afhankelijkheden. Hiertoe typt u het volgende in de Package Manager-Console voor uw oplossing. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```
    
* Met behulp van uw voorkeur Nuget package management-hulpprogramma het Microsoft.Azure.Storage.Common Nuget-pakket installeren voordat u Microsoft.Azure.CosmosDB.Table installeert.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over de Azure-API voor tabel Cosmos DB, [Inleiding tot Azure Cosmos DB tabel API](table-introduction.md). 
