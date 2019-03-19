---
title: Azure Cosmos DB Table-API .NET Standard SDK en bronnen
description: Meer informatie over de Azure Cosmos DB Table-API en de standaard .NET-SDK met inbegrip van release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 10/18/2018
ms.openlocfilehash: 38b283ed666b39b4e090bd95051a4454a9b47e62
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57975658"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Azure Cosmos DB tabel standaard .NET-API: Download en opmerkingen bij de release
> [!div class="op_single_selector"]
> 
> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**Voorbeeld**|[Cosmos DB tabel-API .NET-voorbeeld](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started)|
|**Snelstartgids**|[Snelstartgids](create-table-dotnet.md)|
|**Zelfstudie**|[Zelfstudie](tutorial-develop-table-dotnet.md)|
|**Huidige ondersteunde framework**|[Microsoft .NET Standard 2.0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Versie met algemene beschikbaarheid

### <a name="a-name0110-preview0110-preview"></a><a name="0.11.0-preview"/>0.11.0-preview
* Wijzigingen zijn aangebracht in hoe CloudTableClient kan worden geconfigureerd. Het duurt nu nog maar een een object TableClientConfiguration tijdens het ontwerpen. TableClientConfiguration biedt verschillende eigenschappen voor het configureren van het gedrag van de client afhankelijk van of het doel-eindpunt Cosmos DB Table-API of Azure Storage-Table-API.
* Er is ondersteuning toegevoegd voor TableQuery retourneren van resultaten sorteren op een aangepaste kolom. Deze functie wordt alleen ondersteund voor Cosmos DB Table-eindpunten.
* Er is ondersteuning toegevoegd om RequestCharges op verschillende resultaattypen zichtbaar te maken. Deze functie wordt alleen ondersteund voor Cosmos DB Table-eindpunten.

### <a name="a-name0101-preview0101-preview"></a><a name="0.10.1-preview"/>0.10.1-preview
* Ondersteuning voor SAS-token, bewerkingen van TablePermissions ServiceProperties en ServiceStats op de eindpunten van Azure Storage-tabel toevoegen. 
   > [!NOTE]
   > Sommige functies in de vorige Azure Storage Table SDK's zijn nog niet ondersteund, zoals het client-side encryption.

### <a name="a-name0100-preview0100-preview"></a><a name="0.10.0-preview"/>0.10.0-preview
* Ondersteuning toevoegen voor core CRUD-, batch- en querybewerkingen op de eindpunten van Azure Storage-tabel. 
   > [!NOTE]
   > Sommige functies in de vorige Azure Storage Table SDK's zijn nog niet ondersteund, zoals het client-side encryption.

### <a name="a-name091-preview091-preview"></a><a name="0.9.1-preview"/>0.9.1-Preview
* Azure Cosmos DB tabel .NET Standard-SDK is een platformoverschrijdende .NET-bibliotheek die efficiënt toegang tot het gegevensmodel van de tabel op Cosmos DB biedt. Deze eerste release biedt ondersteuning voor de volledige set van tabel of entiteit CRUD + Query functies met dezelfde API's als de [Cosmos DB Table SDK voor .NET Framework](table-sdk-dotnet.md). 
   > [!NOTE]
   >  Azure Storage-tabel-eindpunten zijn nog niet ondersteund in de 0.9.1-preview-versie.

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.0.0](#1.0.0) |13 maart 2019 |--- |
| [0.11.0-preview](#0.11.0-preview) |5 maart 2019 |--- |
| [0.10.1-preview](#0.10.1-preview) |22 januari 2019 |--- |
| [0.10.0-preview](#0.10.0-preview) |18 december 2018 |--- |
| [0.9.1-Preview](#0.9.1-preview) |18 oktober 2018 |--- |


## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over de Azure Cosmos DB Table-API, [Inleiding tot Azure Cosmos DB Table-API](table-introduction.md). 
