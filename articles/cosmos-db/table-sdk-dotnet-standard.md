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
ms.openlocfilehash: ce7cc489b107ce4bd95270b9a7f8cb560a2d2398
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249643"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Azure Cosmos DB tabel standaard .NET-API: Download en opmerkingen bij de release
> [!div class="op_single_selector"]

> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**Huidige ondersteunde framework**|[Microsoft .NET Standard 2.0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Releaseopmerkingen

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
| [0.10.1-preview](#0.10.1-preview) |22 januari 2019 |--- |
| [0.10.0-preview](#0.10.0-preview) |18 december 2018 |--- |
| [0.9.1-Preview](#0.9.1-preview) |18 oktober 2018 |--- |


## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over de Azure Cosmos DB Table-API, [Inleiding tot Azure Cosmos DB Table-API](table-introduction.md). 
