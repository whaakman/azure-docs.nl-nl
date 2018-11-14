---
title: Werken met index paden in Azure Cosmos DB
description: Overzicht van de paden van de index in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: rimman
ms.openlocfilehash: 0515397fb9ab0f05b4c763a2b05e9d986960bd91
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629021"
---
# <a name="index-paths-in-azure-cosmos-db"></a>Paden van de index in Azure Cosmos DB

U kunt met behulp van paden van de index in Azure Cosmos DB, opnemen of uitsluiten van een specifiek pad van het indexeren. Kiezen van specifieke paden biedt verbeterde schrijven prestaties en lagere indexopslag voor scenario's waarin u weet de querypatronen dat. Index-paden beginnen met de hoofdmap (`/`) wildcard-operator en meestal eindigen met de `?` wildcard-operator. Dit patroon geeft aan dat er meerdere mogelijke waarden voor het voorvoegsel zijn. Bijvoorbeeld, om de query `SELECT * FROM Families F WHERE F.familyName = "Andersen"`, moet u een pad van de index voor opnemen `/familyName/?` in de index-beleid van de container.

Index paden kunt ook de `*` jokertekens operator op die het gedrag voor paden recursief onder het voorvoegsel opgeven. Bijvoorbeeld, `/payload/*` uitsluiten alles onder de eigenschap van de nettolading van het indexeren kan worden gebruikt.

## <a name="common-patterns-for-index-paths"></a>Algemene patronen voor index-paden

Hier volgen de algemene patronen voor het opgeven van index paden:

| **Pad** | **Beschrijving/use-case** |
| ---------- | ------- |
| /   | Het standaardpad voor de verzameling. Recursieve en is van toepassing op de structuur van de hele document.|
| / prop /?  | Index-pad voor het uitvoeren van query's zoals de volgende (met de typen hash- of -bereik, respectievelijk):<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop > 5<br><br>Selecteer uit verzameling c ORDER BY c.prop  |
| / prop / *  | Pad van de index voor alle paden onder het opgegeven label. Werkt met de volgende query 's<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop.subprop > 5<br><br>Selecteer uit verzameling c waar c.prop.subprop.nextprop = "waarde"<br><br>Selecteer uit verzameling c ORDER BY c.prop |
| / Eigenschappen / [] /?  | Index-pad is vereist voor het beheer iteratie en JOIN-query's op matrices met hoeken, zoals ["a", "b", "c"]:<br><br>Selecteer taggen van code IN collection.props waar tag = "waarde"<br><br>Selecteer tag van verzameling c JOIN-tag IN c.props waar code > 5  |
| /props/ [] /subprop/? | Pad van de index is vereist om herhaling van dienst en JOIN-query's op matrices met objecten, zoals [{subprop: "a"}, {subprop: "b"}]:<br><br>Selecteer taggen van code IN collection.props waar tag.subprop = "waarde"<br><br>Selecteer taggen van verzameling c JOIN-tag IN c.props waar tag.subprop = "waarde" |
| / prop/subprop /? | Index-pad voor het uitvoeren van query's (met de typen hash- of -bereik, respectievelijk):<br><br>Selecteer uit verzameling c waar c.prop.subprop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop.subprop > 5  |

Als u aangepaste index paden instelt, moet u bent de standaardregel voor indexering voor het volledige artikel, aangeduid met de speciale pad opgeven `/*`.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het indexeren in Azure Cosmos DB in de volgende artikelen:

- [Overzicht van het indexeren](index-overview.md)
- [Indexering in Azure Cosmos DB](indexing-policies.md)
- [Typen van de index in Azure Cosmos DB](index-types.md)
