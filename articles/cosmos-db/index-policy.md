---
title: Azure Cosmos DB indexerings beleid
description: Meer informatie over het configureren en wijzigen van het standaard indexerings beleid voor automatische indexering en betere prestaties in Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: thweiss
ms.openlocfilehash: 01e3e1f1c9bffee0604de1260e8e466f5b1d229d
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467880"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Indexerings beleid in Azure Cosmos DB

In Azure Cosmos DB heeft elke container een indexerings beleid dat bepaalt hoe de items van de container moeten worden geïndexeerd. Het standaard indexerings beleid voor nieuw gemaakte containers indexeert elke eigenschap van elk item, waarbij bereik indexen worden afgedwongen voor wille keurige teken reeksen of getallen en ruimtelijke indexen voor een geojson-object van het type punt. Zo kunt u hoge query prestaties krijgen zonder dat u op de hoogte hoeft te zijn van indexering en index beheer vooraf.

In sommige gevallen is het mogelijk dat u dit automatische gedrag wilt overschrijven zodat het beter aansluit bij uw vereisten. U kunt het indexerings beleid van een container aanpassen door de *indexerings modus*in te stellen en *eigenschaps paden*op te nemen of uit te sluiten.

> [!NOTE]
> De methode voor het bijwerken van het indexerings beleid dat in dit artikel wordt beschreven, is alleen van toepassing op de SQL-API (core) van Azure Cosmos DB.

## <a name="indexing-mode"></a>Indexerings modus

Azure Cosmos DB ondersteunt twee indexerings modi:

- **Consistent**: Als het indexerings beleid van een container is ingesteld op consistent, wordt de index synchroon bijgewerkt wanneer u items maakt, bijwerkt of verwijdert. Dit betekent dat de consistentie van uw Lees query's de consistentie is die [voor het account is geconfigureerd](consistency-levels.md).

- **Geen**: Als het indexerings beleid van een container is ingesteld op geen, wordt indexeren effectief uitgeschakeld voor die container. Dit wordt meestal gebruikt wanneer een container wordt gebruikt als een pure sleutel waarde Store zonder dat hiervoor secundaire indexen nodig zijn. Het kan ook helpen bij het versnellen van Bulk Insert-bewerkingen.

## <a name="including-and-excluding-property-paths"></a>Eigenschaps paden opnemen en uitsluiten

Een aangepast indexerings beleid kan eigenschaps paden opgeven die expliciet worden opgenomen of uitgesloten van indexeren. Door het aantal geïndexeerde paden te optimaliseren, kunt u de hoeveelheid opslag die wordt gebruikt door uw container verlagen en de latentie van schrijf bewerkingen verbeteren. Deze paden worden gedefinieerd volgens [de methode die wordt beschreven in de sectie Overzicht indexering](index-overview.md#from-trees-to-property-paths) met de volgende toevoegingen:

- een pad dat leidt naar een scalaire waarde (teken reeks of getal) eindigt op`/?`
- elementen uit een matrix worden samen met de `/[]` notatie (in plaats van `/0`, `/1` enzovoort) beschreven.
- het `/*` Joker teken kan worden gebruikt om alle elementen onder het knoop punt te vergelijken

Hetzelfde voor beeld opnieuw uitvoeren:

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

- het `headquarters`padis `employees``/headquarters/employees/?`
- het `locations`padis `country``/locations/[]/country/?`
- het pad naar iets onder `headquarters` is`/headquarters/*`

Wanneer een pad expliciet is opgenomen in het indexerings beleid, moet het ook bepalen welke index typen moeten worden toegepast op dat pad en voor elk index type, het gegevens type van deze index is van toepassing op:

| Indextype | Toegestane doel gegevens typen |
| --- | --- |
| Bereik | Teken reeks of getal |
| Ruimtelijk | Punt, Lines Tring of veelhoek |

`/headquarters/employees/?` We kunnen bijvoorbeeld het pad toevoegen en opgeven dat een `Range` index moet worden toegepast op dat pad voor zowel `String` en `Number` -waarden.

### <a name="includeexclude-strategy"></a>Strategie voor opnemen/uitsluiten

Elk indexerings beleid moet het `/*` basispad bevatten als een opgenomen of uitgesloten pad.

- Neem het hoofdpad op om selectieve paden uit te sluiten die niet hoeven te worden geïndexeerd. Dit is de aanbevolen benadering, omdat hiermee Azure Cosmos DB proactief een nieuwe eigenschap kan indexeren die aan uw model kan worden toegevoegd.
- Sluit het hoofdpad uit om selectieve paden op te nemen die moeten worden geïndexeerd.

- Voor paden met gewone tekens die bevatten: alfanumerieke tekens en _ (onderstrepings teken), hoeft u de padtekenreeks niet te escapepen rond dubbele aanhalings tekens (bijvoorbeeld '/Path/? '). Voor paden met andere speciale tekens moet u de teken reeks voor het pad Escape rond dubbele aanhalings tekens (bijvoorbeeld "\"/Path-\"ABC/?"). Als u speciale tekens in uw pad verwacht, kunt u elk pad voor de beveiliging op te zeggen. Dit is functioneel geen verschil als u elk pad en alleen de bestanden met speciale tekens weglaat.

- De systeem eigenschap "ETAG" wordt standaard uitgesloten van indexeren, tenzij de ETAG wordt toegevoegd aan het opgenomen pad voor indexering.

Zie [deze sectie](how-to-manage-indexing-policy.md#indexing-policy-examples) voor voor beelden van Indexing-beleid.

## <a name="composite-indexes"></a>Samengestelde indexen

Query's waarvoor `ORDER BY` twee of meer eigenschappen een samengestelde index vereisen. Op dit moment worden samengestelde indexen alleen gebruikt door meerdere `ORDER BY` query's. Standaard zijn er geen samengestelde indexen gedefinieerd, dus u moet zo nodig [samengestelde indexen toevoegen](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) .

Wanneer u een samengestelde index definieert, geeft u het volgende op:

- Twee of meer eigenschaps paden. De volg orde waarin eigenschaps paden worden gedefinieerd.
- De volg orde (oplopend of aflopend).

De volgende overwegingen worden gebruikt bij het gebruik van samengestelde indexen:

- Als de samengestelde index paden niet overeenkomen met de volg orde van de eigenschappen in de ORDER BY-component, kan de samengestelde index de query niet ondersteunen

- De volg orde van samengestelde index paden (oplopend of aflopend) moet ook overeenkomen met de volg orde in de ORDER BY-component.

- De samengestelde index ondersteunt ook een component ORDER BY met de tegenovergestelde volg orde op alle paden.

Bekijk het volgende voor beeld waarbij een samengestelde index is gedefinieerd voor de eigenschappen a, b en c:

| **Samengestelde index**     | **Voorbeeld `ORDER BY` query**      | **Ondersteund door index?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(a asc, b asc)```         | ```ORDER BY  a asc, b asc```        | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  b asc, a asc```        | ```No```             |
| ```(a asc, b asc)```          | ```ORDER BY  a desc, b desc```      | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  a asc, b desc```       | ```No```             |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc, c asc``` | ```Yes```            |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc```        | ```No```            |

U moet het indexerings beleid aanpassen zodat u alle benodigde `ORDER BY` query's kunt uitvoeren.

## <a name="modifying-the-indexing-policy"></a>Het indexerings beleid wijzigen

Het indexerings beleid van een container kan op elk gewenst moment worden bijgewerkt [met behulp van de Azure portal of een van de ondersteunde sdk's](how-to-manage-indexing-policy.md). Een update voor het indexerings beleid activeert een trans formatie van de oude index naar de nieuwe, die online en op locatie wordt uitgevoerd (zodat er geen extra opslag ruimte wordt verbruikt tijdens de bewerking). De oude beleids index is efficiënt getransformeerd naar het nieuwe beleid zonder dat dit van invloed is op de schrijf Beschik baarheid of de door Voer ingericht op de container. Index transformatie is een asynchrone bewerking en de tijd die nodig is om te volt ooien, is afhankelijk van de ingerichte door Voer, het aantal items en de grootte ervan. 

> [!NOTE]
> Tijdens het opnieuw indexeren van wordt uitgevoerd, retour neren query's mogelijk niet alle overeenkomende resultaten en worden deze zonder fouten geretourneerd. Dit betekent dat query resultaten mogelijk niet consistent zijn totdat de index transformatie is voltooid. Het is mogelijk om de voortgang van de index transformatie [te volgen met behulp van een van de sdk's](how-to-manage-indexing-policy.md).

Als de modus nieuw indexerings beleid is ingesteld op consistent, kan er geen andere wijziging van het indexerings beleid worden toegepast terwijl de index transformatie wordt uitgevoerd. Een actieve index transformatie kan worden geannuleerd door de modus van het indexerings beleid in te stellen op geen (waardoor de index onmiddellijk wordt verwijderd).

## <a name="indexing-policies-and-ttl"></a>Indexerings beleid en TTL

Voor de [functie time-to-Live (TTL)](time-to-live.md) moet indexering actief zijn op de container waarop deze is ingeschakeld. Dit betekent dat:

- het is niet mogelijk om TTL te activeren voor een container waarbij de indexerings modus is ingesteld op geen,
- het is niet mogelijk om de indexerings modus in te stellen op geen in een container waarin TTL wordt geactiveerd.

Voor scenario's waarin geen eigenschapspad moet worden geïndexeerd, maar TTL vereist is, kunt u een indexerings beleid gebruiken met:

- een indexerings modus is ingesteld op consistent en
- geen opgenomen pad en
- `/*`alleen als het uitgesloten pad.

## <a name="obsolete-attributes"></a>Verouderde kenmerken

Wanneer u werkt met indexerings beleid, kunt u de volgende kenmerken ondervinden die nu verouderd zijn:

- `automatic`is een Booleaanse waarde die is gedefinieerd in de hoofdmap van een indexerings beleid. Het wordt nu genegeerd en kan worden ingesteld op `true`, wanneer het hulp programma dat u gebruikt vereist.
- `precision`is een getal dat is gedefinieerd op index niveau voor opgenomen paden. Het wordt nu genegeerd en kan worden ingesteld op `-1`, wanneer het hulp programma dat u gebruikt vereist.
- `hash`is een index soort die nu wordt vervangen door de bereik soort.

## <a name="next-steps"></a>Volgende stappen

Lees meer over indexeren in de volgende artikelen:

- [Overzicht van indexeren](index-overview.md)
- [Indexerings beleid beheren](how-to-manage-indexing-policy.md)
