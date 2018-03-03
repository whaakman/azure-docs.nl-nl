---
title: Azure CDN regels engine verwijzing | Microsoft Docs
description: Documentatie bij Azure CDN regels overeen motor en onderdelen.
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 602b4303dd1940791c11b8b71ac6a27f0474a6d5
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="azure-cdn-rules-engine-reference"></a>Azure CDN regels engine verwijzing
Dit artikel vindt u gedetailleerde beschrijvingen van de voorwaarden van de beschikbare overeenkomst en de functies voor het Azure Content Delivery Network (CDN) [regelengine](cdn-rules-engine.md).

De regelengine voor is ontworpen voor de laatste instantie op hoe specifieke typen aanvragen worden verwerkt door het CDN.

**Veelvoorkomende toepassingen**:

- Overschrijven of een aangepaste cachebeleid definiëren.
- Secure of weigeren van aanvragen voor gevoelige inhoud.
- Aanvragen omleiden.
- Aangepaste logboekgegevens opslaan.

## <a name="terminology"></a>Terminologie
Een regel is gedefinieerd met behulp van [ **voorwaardelijke expressies**](cdn-rules-engine-reference-conditional-expressions.md), [ **voldoen aan bepaalde voorwaarden**](cdn-rules-engine-reference-match-conditions.md), en [ **functies**](cdn-rules-engine-reference-features.md). Deze elementen zijn gemarkeerd in de volgende afbeelding:

 ![CDN overeen voorwaarde](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Syntaxis

De manier waarop u speciale tekens worden behandeld, is afhankelijk van hoe een overeenkomst voorwaarde of functie tekstwaarden verwerkt. Een overeenkomst voorwaarde of onderdeel kan interpreteren tekst in een van de volgende manieren:

1. [**Letterlijke waarden**](#literal-values) 
2. [**Jokertekens waarden**](#wildcard-values)
3. [**Reguliere expressies**](#regular-expressions)

### <a name="literal-values"></a>Letterlijke waarden
Tekst die wordt geïnterpreteerd als een letterlijke waarde beschouwt alle speciale tekens, met uitzondering van het symbool % als onderdeel van de waarde die moet overeenkomen. Met andere woorden, een letterlijke waarde overeenkomen met de voorwaarde is ingesteld op `\'*'\` alleen voldaan wanneer die waarde exacte (dat wil zeggen, `\'*'\`) is gevonden.
 
Een percentagesymbool wordt gebruikt om aan te geven met URL-codering (bijvoorbeeld `%20`).

### <a name="wildcard-values"></a>Jokertekens waarden
Tekst die wordt geïnterpreteerd als een jokerteken toegewezen extra betekenis aan speciale tekens. De volgende tabel wordt beschreven hoe u de volgende reeks tekens wordt geïnterpreteerd:

Teken | Beschrijving
----------|------------
\ | Een backslash wordt gebruikt als escape voor de tekens die is opgegeven in deze tabel. Een backslash moet direct vóór het speciale teken dat moet worden voorafgegaan worden opgegeven.<br/>De volgende syntaxis verlaat u bijvoorbeeld een sterretje: `\*`
% | Een percentagesymbool wordt gebruikt om aan te geven met URL-codering (bijvoorbeeld `%20`).
* | Een sterretje is een jokerteken dat een of meer tekens vertegenwoordigt.
Spatiebalk | Een spatie geeft aan dat een overeenkomst voorwaarde kan worden voldaan door een van de opgegeven waarden of patronen.
'value' | Een enkel aanhalingsteken heeft geen speciale betekenis. Een set met enkele aanhalingstekens wordt echter gebruikt om aan te geven dat een waarde moet worden behandeld als een letterlijke waarde. Deze kan worden gebruikt in de volgende manieren:<br><br/>-Kunt u een overeenkomst voorwaarde worden voldaan aan wanneer de opgegeven waarde komt overeen met een gedeelte van de vergelijkingswaarde.  Bijvoorbeeld: `'ma'` zou overeen met een van de volgende tekenreeksen: <br/><br/>/Business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>zakelijke/sjabloon. **ma**p<br /><br />-Kunt u een speciaal teken als een letterlijke waarde worden opgegeven. U kunt bijvoorbeeld een letterlijke spatie opgeven door een spatie binnen een set met enkele aanhalingstekens (dat wil zeggen, `' '` of `'sample value'`).<br/>-Kunt u een lege waarde worden opgegeven. Geef een lege waarde door te geven van een set met enkele aanhalingstekens (dat wil zeggen, '').<br /><br/>**Belangrijk:**<br/>-Als de opgegeven waarde geen jokertekens bevat, wordt deze automatisch een letterlijke waarde, wat betekent dat het is niet nodig om op te geven van een set met enkele aanhalingstekens beschouwd.<br/>-Als een backslash geen een andere in deze tabel escapeteken biedt, wordt dit genegeerd wanneer de waarde is opgegeven in een set van enkele aanhalingstekens.<br/>-Een andere manier om op te geven van een speciaal teken als een letterlijke waarde is als escapeteken voor het gebruik van een backslash (dat wil zeggen, `\`).

### <a name="regular-expressions"></a>Reguliere expressies

Reguliere expressies definieert een patroon dat moet worden gezocht binnen een tekstwaarde. Notatie van reguliere expressie definieert specifieke betekenis voor een aantal symbolen. De volgende tabel geeft aan hoe speciale tekens worden behandeld door overeenkomst voorwaarden en functies die ondersteuning bieden voor reguliere expressies.

Speciaal teken | Beschrijving
------------------|------------
\ | Een backslash verlaat het teken dat u de volgt it, waardoor dit teken moet worden behandeld als een letterlijke waarde in plaats van nemen over de betekenis van de reguliere expressie. De volgende syntaxis verlaat u bijvoorbeeld een sterretje: `\*`
% | De betekenis van een percentagesymbool is afhankelijk van het gebruik ervan.<br/><br/> `%{HTTPVariable}`: Deze syntaxis identificeert een HTTP-variabele.<br/>`%{HTTPVariable%Pattern}`: Een percentagesymbool deze syntaxis gebruikt voor het identificeren van een HTTP-variabele en als scheidingsteken.<br />`\%`: Een percentagesymbool aanhalingstekens toestaat dat deze moeten worden gebruikt als een letterlijke waarde of om aan te geven met URL-codering (bijvoorbeeld `\%20`).
* | Een sterretje kunt de voorgaande teken nul of meer keer worden vergeleken. 
Spatiebalk | Een spatie wordt meestal beschouwd als een letterlijke waarde. 
'value' | Enkele aanhalingstekens worden behandeld als letterlijke tekens. Een set met enkele aanhalingstekens heeft geen speciale betekenis.


## <a name="next-steps"></a>Volgende stappen
* [De overeenkomst motor regels](cdn-rules-engine-reference-match-conditions.md)
* [Regels engine voorwaardelijke expressies](cdn-rules-engine-reference-conditional-expressions.md)
* [Regels engine functies](cdn-rules-engine-reference-features.md)
* [HTTP-instelling met de regelengine van wijzigen](cdn-rules-engine.md)
* [Overzicht van Azure CDN](cdn-overview.md)
