---
title: Bronnen voor & Azure CosmosDB Graph API .NET SDK | Microsoft Docs
description: Meer informatie over de Azure-CosmosDB Graph API, met inbegrip van release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie.
services: cosmos-db
documentationcenter: .net
author: luisbosquez
manager: jhubbard
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/17/2017
ms.author: lbosq
ms.openlocfilehash: 28f926d8d1f4c6006a348ba9c8289b885aef7641
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-cosmos-db-graph-net-api-download-and-release-notes"></a>Azure Cosmos DB Graph .NET API: Downloaden en release-opmerkingen

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://aka.ms/acdbgraphnuget)|
|**API-documentatie**|[.NET API-naslagdocumentatie](https://aka.ms/acdbgraphapiref)|
|**Snelstartgids**|[Azure Cosmos DB: Een graph-app met behulp van .NET- en de Graph API maken](create-graph-dotnet.md)|
|**Zelfstudie**|[Azure CosmosDB: Een container maken met de Graph API](tutorial-develop-graph-dotnet.md)|
|**Huidige ondersteunde framework**| [Microsoft .NET Framework 4.6.1](https://www.microsoft.com/en-us/download/details.aspx?id=49981)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |


## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name031-preview031-preview"></a><a name="0.3.1-preview"/>0.3.1-Preview

#### <a name="bug-fixes"></a>Oplossingen voor problemen
* Los eventueel laden `appsettings.json` (`netstandard1.6`)

#### <a name="whats-new"></a>Nieuwe functies
* Microsoft.Azure.Graphs doelplatform AnyCPU activeren.
* Mono assembly van verwijderen `net461` pakket manifest.

### <a name="a-name030-preview030-preview"></a><a name="0.3.0-preview"/>0.3.0-Preview

#### <a name="whats-new"></a>Nieuwe functies
* Ondersteuning toegevoegd voor`.netstandard 1.6`
  * Vereist`Microsoft.Azure.DocumentDB.Core >= 1.5.1`
* Een nieuwe toegevoegd `gremlin-groovy` parser ter vervanging van bestaande parser. Deze parser ondersteunt een subset van de Tinkerpop `gremlin-groovy` syntaxis en omvat:
  * Verbeterde prestaties bij het parseren met 2 x.
  * Een aantal problemen die zijn gerelateerd aan teken aanhalingstekens in tekenreeksen, onjuist afgehandelde letterlijke waarden en andere onregelmatigheden in de oude parser is opgelost.
* Toegevoegde optimalisaties voor traversals met rand predicaten.
  *  Verandering hops met filters ziet deze verbetering, bijvoorbeeld: `g.V('1').outE().has('name', 'marko').inV()`.
* Optimalisaties voor traversals met toegevoegd `limit()` stap.

#### <a name="breaking-changes"></a>Wijzigingen op te splitsen
* Verwijderde ondersteuning voor .NET Framework 4.5.1

* De nieuwe parser wordt uitgelijnd met `gremlin-groovy` grammatica. Als gevolg hiervan zijn aantal expressies die u eerder hebt gewerkt voor de nieuwe parser dubbelzinnig. Een voorbeeld van Opmerking:
  * `in`en `as` zijn gereserveerde woorden in `gremlin-groovy`, zodat deze stappen moeten worden gekwalificeerd met `.in()` of `.as()` om te voorkomen dat syntaxisfouten. Bijvoorbeeld: `g.V().repeat(in()).times(2)`  ->  _genereert een syntaxisfout_  
 `g.V().repeat(__.in()).times(2)` -> _is geslaagd_

### <a name="a-name024-preview024-preview"></a><a name="0.2.4-preview"/>0.2.4-Preview

### <a name="a-name022-preview022-preview"></a><a name="0.2.2-preview"/>0.2.2-Preview

### <a name="a-name021-preview021-preview"></a><a name="0.2.1-preview"/>0.2.1-Preview

### <a name="a-name020-preview020-preview"></a><a name="0.2.0-preview"/>0.2.0-Preview

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview
* Initiële preview-versie.

## <a name="release--retirement-dates"></a>Release & buiten gebruik stellen datums
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk. 

Elk verzoek aan de Azure Cosmos-database met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [0.3.1-Preview](#0.3.1-preview) |17 oktober 2017 |--- |
| [0.3.0-Preview](#0.3.0-preview) |2 oktober 2017 |--- |
| [0.2.4-Preview](#0.2.4-preview) |4 augustus 2017 |--- |
| [0.2.2-Preview](#0.2.2-preview) |23 juni 2017 |--- |
| [0.2.1-Preview](#0.2.2-preview) |8 juni 2017 |--- |
| [0.2.0-Preview](#0.2.2-preview) |10 mei 2017 |--- |
| [0.1.0-Preview](#0.1.0-preview) |8 mei 2017 |--- |

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Azure Cosmos DB Graph API, [Inleiding tot Azure Cosmos DB: Graph API](graph-introduction.md). 
