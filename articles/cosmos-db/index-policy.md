---
title: Azure Cosmos DB indexeringsbeleid
description: Informatie over het configureren en wijzigen van de standaardbeleidsregels voor indexering van beleid voor automatisch indexeren en betere prestaties in Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: thweiss
ms.openlocfilehash: 4206fba8297672a1a24415169cfd19ff89344038
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431182"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Indexering in Azure Cosmos DB

Elke container heeft in Azure Cosmos DB, een indexeringsbeleid die bepaalt hoe de items van de container moeten worden geïndexeerd. De standaardbeleidsregels voor indexering van beleid voor nieuwe gemaakt containers indexen elke eigenschap van elk item, bereik indexen voor een tekenreeks of getal en ruimtelijke indexen voor een GeoJSON-object van type punt. Hiermee kunt u hoge queryprestaties zonder te denken over het indexeren en indexbeheer vooraf ophalen.

In sommige gevallen kunt u dit automatisch gedrag beter aan uw vereisten negeren. U kunt het indexeringsbeleid van een container aanpassen door in te stellen de *indexering modus*, en opnemen of uitsluiten *eigenschappaden*.

## <a name="indexing-mode"></a>Indexering modus

Azure Cosmos DB ondersteunt twee modi voor indexering:

- **Consistente**: Als het indexeringsbeleid van de container is ingesteld op een consistente, wordt de index synchroon bijgewerkt wanneer u maken, bijwerken of verwijderen van items. Dit betekent dat de consistentie van uw lezen query's worden de [consistentie geconfigureerd voor het account](consistency-levels.md).

- **Geen**: Als het indexeringsbeleid van de container is ingesteld op None, is indexeren effectief uitgeschakeld op die container. Dit wordt vaak gebruikt als een container wordt gebruikt als een zuivere sleutel-waardearchief zonder de noodzaak voor secundaire indexen. Het kan ook helpen versnellen bulksgewijs bewerkingen invoegen.

## <a name="including-and-excluding-property-paths"></a>Opnemen en uitsluiten van eigenschappaden

Een aangepast indexeringsbeleid kunt eigenschappaden die expliciet zijn opgenomen of uitgesloten van het indexeren opgeven. U kunt door het aantal paden die zijn geïndexeerd optimaliseert, verlaag de hoeveelheid opslag die wordt gebruikt door de container en verbeteren van de latentie van schrijfbewerkingen. Deze paden zijn gedefinieerd met het volgende [de methode die wordt beschreven in de sectie met indexering overzicht](index-overview.md#from-trees-to-property-paths) met de volgende toevoegingen:

- eindigt op een pad naar een scalaire waarde (tekenreeks of getal) `/?`
- elementen van een matrix samen worden opgelost door middel van de `/[]` notatie (in plaats van `/0`, `/1` enz.)
- de `/*` jokertekens kan worden gebruikt zodat deze overeenkomt met alle elementen onder het knooppunt

Het voorbeeld nemen opnieuw:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 }
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

- de `headquarters`van `employees` pad is `/headquarters/employees/?`
- de `locations`' `country` pad is `/locations/[]/country/?`
- het pad op naar iets onder `headquarters` is `/headquarters/*`

Wanneer een pad expliciet is opgenomen in het indexeringsbeleid, heeft deze ook om te definiëren welke typen index moeten worden toegepast op het opgegeven pad en voor elk indextype, het gegevenstype deze index is van toepassing op:

| Indextype | Doel-gegevenstypen toegestaan |
| --- | --- |
| Bereik | Tekenreeks of getal |
| Ruimtelijk | Point, LineString of veelhoek |

We kunnen bijvoorbeeld de `/headquarters/employees/?` pad en opgeven dat een `Range` index moet worden toegepast op het opgegeven pad voor zowel `String` en `Number` waarden.

### <a name="includeexclude-strategy"></a>Strategie voor opnemen/uitsluiten

Alle indexeringsbeleid heeft om op te nemen van pad naar de hoofdmap `/*` als een opgenomen of als een uitgesloten-pad.

- Het pad naar de hoofdmap als u wilt uitsluiten selectief paden die niet hoeven te worden geïndexeerd bevatten. Dit is de aanbevolen aanpak omdat het Azure Cosmos DB is een nieuwe eigenschap die kan worden toegevoegd aan uw model proactief index kunt.
- Sluit het hoofdpad selectief om paden te nemen die moeten worden geïndexeerd.

- Voor paden met gewone tekens bevatten: alfanumerieke tekens en _ (onderstrepingstekens), moet u geen escape-tekenreeks voor het pad om dubbele aanhalingstekens (bijvoorbeeld: "/ path /?"). Voor paden met andere speciale tekens bevat, moet u escape-tekenreeks voor het pad om dubbele aanhalingstekens (bijvoorbeeld: "/\"pad abc\"/?"). Als u speciale tekens in het pad verwacht, kunt u elk pad voor de veiligheid druk op ESC. Functioneel niet dat u enig verschil merkt als escape voor elk pad Vs alleen de resources die speciale tekens bevatten.

Zie [in deze sectie](how-to-manage-indexing-policy.md#indexing-policy-examples) voor indexering beleid voorbeelden.

## <a name="composite-indexes"></a>Samengestelde indexen

Query's die `ORDER BY` twee of meer eigenschappen vereist een samengestelde index. Op dit moment alleen samengestelde indexen worden gebruikt door meerdere `ORDER BY` query's. Standaard geen samengestelde indexen zijn gedefinieerd, moet u [samengestelde indexen toevoegen](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) indien nodig.

Bij het definiëren van een samengestelde index, moet u het volgende opgeven:

- Twee of meer eigenschappaden. De volgorde op waarin eigenschap paden zijn belangrijk is gedefinieerd.
- De volgorde (oplopend of aflopend).

De volgende overwegingen zijn gebruikt bij het gebruik van samengestelde indexen:

- Als de paden samengestelde index komen niet overeen met de volgorde van de eigenschappen in de component ORDER BY, kan niet klikt u vervolgens de samengestelde index ondersteund door de query

- De volgorde van samengestelde index paden (oplopend of aflopend) moet ook overeenkomen met de volgorde in de component ORDER BY.

- De samengestelde index biedt ook ondersteuning voor een component ORDER BY met de omgekeerde volgorde op alle paden.

Houd rekening met het volgende voorbeeld, waarbij een samengestelde index wordt gedefinieerd op eigenschappen a, b en c

| **Samengestelde Index**     | **Voorbeeld `ORDER BY` Query**      | **Ondersteund door de Index?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(a asc, b asc)```         | ```ORDER BY  a asc, b asc```        | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  b asc, a asc```        | ```No```             |
| ```(a asc, b asc)```          | ```ORDER BY  a desc, b desc```      | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  a asc, b desc```       | ```No```             |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc, c asc``` | ```Yes```            |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc```        | ```No```            |

U moet uw indexeringsbeleid aanpassen zodat u alle nodige kan dienen `ORDER BY` query's.

## <a name="modifying-the-indexing-policy"></a>Wijzigen van het indexeringsbeleid

Indexeringsbeleid van de container kan worden bijgewerkt op elk gewenst moment [met behulp van de Azure-portal of een van de ondersteunde SDK's](how-to-manage-indexing-policy.md). Een update aan voor het indexeringsbeleid activeert een transformatie van de oude index naar de nieuwe versie, die wordt uitgevoerd op het online en op locatie (zodat er geen extra opslagruimte wordt gebruikt tijdens de bewerking). Index van het oude beleid is efficiënt omgezet naar het nieuwe beleid zonder gevolgen voor de beschikbaarheid voor schrijven of de doorvoer die is ingericht voor de container. Index transformatie is een asynchrone bewerking en de tijd die nodig zijn om te voltooien is afhankelijk van de ingerichte doorvoer, het aantal items en de omvang ervan. 

> [!NOTE]
> Terwijl opnieuw indexeren uitgevoerd wordt, worden query's mogelijk niet de overeenkomende resultaten retourneren, en zal dit doen zonder eventuele fouten retourneren. Dit betekent dat de resultaten van query zijn mogelijk inconsistent totdat de index-transformatie is voltooid. Het is mogelijk om bij te houden van de voortgang van de index transformatie [met behulp van een van de SDK's](how-to-manage-indexing-policy.md).

Als het nieuwe indexeringsbeleid-modus is ingesteld op een consistente, kan er geen andere indexering beleidswijziging worden toegepast, terwijl de transformatie van de index uitgevoerd wordt. Een actieve transformatie van de index kan worden geannuleerd door het indexeringsbeleid-modus is ingesteld op None (die de index wordt onmiddellijk worden verwijderd).

## <a name="indexing-policies-and-ttl"></a>Beleid voor indexering en TTL

De [Time-to-Live (TTL) functie](time-to-live.md) vereist indexering actief zijn op de container is ingeschakeld. Dit betekent dat:

- het is niet mogelijk om te activeren TTL voor een container waarin de indexing-modus is ingesteld op None,
- het is niet mogelijk om in te stellen de indexing-modus op None voor een container waarin de TTL is geactiveerd.

Voor scenario's waarbij er geen eigenschapspad moet worden geïndexeerd, maar TTL-waarde is vereist, kunt u een indexeringsbeleid met:

- een indexering modus ingesteld op consistente, en
- Er zijn geen opgenomen pad, en
- `/*` Als het enige uitgesloten-pad.

## <a name="obsolete-attributes"></a>Verouderd kenmerken

Als u werkt met de indexing-beleid, kunnen de volgende kenmerken die nu verouderd zijn optreden:

- `automatic` een Booleaanse waarde gedefinieerd in de hoofdmap van een beleid voor indexering. Het is nu genegeerd en kan worden ingesteld op `true`, wanneer het hulpprogramma dat u gebruikt dit vereist.
- `precision` is een getal dat is gedefinieerd op het indexniveau van de voor opgenomen paden. Het is nu genegeerd en kan worden ingesteld op `-1`, wanneer het hulpprogramma dat u gebruikt dit vereist.
- `hash` een index-type dat nu door het type bereik vervangen is is.

## <a name="next-steps"></a>Volgende stappen

Lees meer over indexeren in de volgende artikelen:

- [Indexering-overzicht](index-overview.md)
- [Indexeringsbeleid beheren](how-to-manage-indexing-policy.md)
