---
title: Naslaginformatie over Azure CDN de regelengine | Microsoft Docs
description: Naslagdocumentatie voor Azure CDN regels criteria voor overeenkomst regelengine en functies.
services: cdn
documentationcenter: ''
author: Lichard
manager: akucer
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 3163b33f69f4cc2d6cd4127253c7b6fadfddd6b0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57994237"
---
# <a name="azure-cdn-rules-engine-reference"></a>Naslaginformatie over Azure CDN de regelengine
Dit artikel vindt u gedetailleerde beschrijvingen van de criteria voor overeenkomst beschikbaar en functies voor het Azure Content Delivery Network (CDN) [regels-engine](cdn-rules-engine.md).

De regelengine is ontworpen om de laatste instantie op hoe specifieke typen aanvragen worden verwerkt door het CDN.

**Veelvoorkomende toepassingen**:

- Overschrijven of een aangepaste-cache-beleid moet worden gedefinieerd.
- Beveilig of weigeren van aanvragen voor gevoelige inhoud.
- Aanvragen omleiden.
- Het aangepaste logboek data Store.

## <a name="terminology"></a>Terminologie
Een regel wordt gedefinieerd via het gebruik van [ **voorwaardelijke expressies**](cdn-rules-engine-reference-conditional-expressions.md), [ **voldoen aan bepaalde voorwaarden**](cdn-rules-engine-reference-match-conditions.md), en [ **functies**](cdn-rules-engine-reference-features.md). Deze elementen zijn gemarkeerd in de volgende afbeelding:

 ![CDN-voorwaarde voor overeenkomst](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Syntaxis

De manier waarop de speciale tekens worden behandeld varieert op basis van hoe een voorwaarde voor overeenkomst of het onderdeel tekstwaarden verwerkt. Een voorwaarde voor overeenkomst of het onderdeel kan interpreteren van tekst in een van de volgende manieren:

1. [**Letterlijke waarden**](#literal-values) 
2. [**Jokertekens waarden**](#wildcard-values)
3. [**Reguliere expressies**](#regular-expressions)

### <a name="literal-values"></a>Letterlijke waarden
Tekst die wordt geïnterpreteerd als een letterlijke waarde worden alle speciale tekens bevatten, met uitzondering van het symbool % behandeld als onderdeel van de waarde die moet worden vergeleken. Met andere woorden, een letterlijke waarde overeenkomen met de voorwaarde is ingesteld op `\'*'\` alleen voldaan wanneer die waarde exacte (dat wil zeggen, `\'*'\`) is gevonden.
 
Een percentagesymbool wordt gebruikt om aan te geven van de URL-codering (bijvoorbeeld `%20`).

### <a name="wildcard-values"></a>Jokertekens waarden
Tekst die wordt geïnterpreteerd als een jokerteken-waarde wordt extra betekenis aan speciale tekens. De volgende tabel wordt beschreven hoe u de volgende set tekens wordt geïnterpreteerd:

Teken | Description
----------|------------
\ | Een backslash wordt gebruikt als escape voor de tekens die is opgegeven in deze tabel. Een backslash moet worden opgegeven direct vóór de speciale tekens die moet worden weergegeven.<br/>Bijvoorbeeld, verlaat u de volgende syntaxis sterretjes: `\*`
% | Een percentagesymbool wordt gebruikt om aan te geven van de URL-codering (bijvoorbeeld `%20`).
\* | Een sterretje is een jokerteken die staat voor een of meer tekens.
Ruimte | Een spatie geeft aan dat een voorwaarde voor overeenkomst kan worden voldaan door een van de opgegeven waarden of patronen.
'value' | Een enkel aanhalingsteken heeft geen speciale betekenis. Echter, een set met enkele aanhalingstekens wordt gebruikt om aan te geven dat een waarde moet worden behandeld als een letterlijke waarde. Het kan worden gebruikt in de volgende manieren:<br><br/>-Kan een voorwaarde voor overeenkomst worden voldaan voordat wanneer de opgegeven waarde komt overeen met een gedeelte van de vergelijkingswaarde.  Bijvoorbeeld, `'ma'` wordt er gezocht naar een van de volgende tekenreeksen: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/ business/sjabloon. **ma**p<br /><br />-Kan een speciaal teken moet worden opgegeven als een letterlijk teken. U kunt bijvoorbeeld een letterlijk teken opgeven door een spatie in een set enkele aanhalingstekens (dat wil zeggen, `' '` of `'sample value'`).<br/>-Hiermee kunt een lege waarde worden opgegeven. Geef een lege waarde door het opgeven van een set van enkele aanhalingstekens (dat wil zeggen, '').<br /><br/>**Belangrijk:**<br/>-Als de opgegeven waarde geen jokertekens bevat, wordt deze automatisch een letterlijke waarde, wat betekent dat het is niet nodig om op te geven van een set met enkele aanhalingstekens beschouwd.<br/>-Als een backslash niet een ander in deze tabel escapeteken is, wordt dit genegeerd wanneer deze is opgegeven in een set enkele aanhalingstekens.<br/>-Een andere manier om op te geven van een speciaal teken als een letterlijke waarde is als u met behulp van een backslash (dat wil zeggen, `\`).

### <a name="regular-expressions"></a>Reguliere expressies

Reguliere expressies definieert een patroon dat moet worden gezocht in een tekstwaarde. Notatie van de reguliere expressie definieert specifieke betekenis voor een verscheidenheid aan symbolen. De volgende tabel geeft aan hoe speciale tekens worden behandeld door de criteria voor overeenkomst en functies die ondersteuning bieden voor reguliere expressies.

Speciaal teken | Description
------------------|------------
\ | Een backslash verlaat het teken dat u de volgende it, waardoor het teken moet worden behandeld als een letterlijke waarde in plaats van nemen over de betekenis van de reguliere expressie. Bijvoorbeeld, verlaat u de volgende syntaxis sterretjes: `\*`
% | De betekenis van een percentagesymbool, is afhankelijk van het gebruik ervan.<br/><br/> `%{HTTPVariable}`: Deze syntaxis identificeert een HTTP-variabele.<br/>`%{HTTPVariable%Pattern}`: Deze syntaxis gebruikt een percentagesymbool om te identificeren van een HTTP-variabele en als een scheidingsteken.<br />`\%`: Een percentagesymbool escapereeksen kan de oplossing moet worden gebruikt als een letterlijke waarde of om aan te geven van de URL-codering (bijvoorbeeld `\%20`).
\* | Een sterretje kan het vorige teken nul of meer keer worden vergeleken. 
Ruimte | Een spatie wordt doorgaans behandeld als een letterlijk teken. 
'value' | Enkele aanhalingstekens worden behandeld als letterlijke tekens. Een set met enkele aanhalingstekens heeft geen speciale betekenis.


## <a name="next-steps"></a>Volgende stappen
* [Criteria voor overeenkomst regelengine](cdn-rules-engine-reference-match-conditions.md)
* [Voorwaardelijke expressies regelengine](cdn-rules-engine-reference-conditional-expressions.md)
* [Functies regelengine](cdn-rules-engine-reference-features.md)
* [Met behulp van de regelengine HTTP-gedrag negeren](cdn-rules-engine.md)
* [Overzicht van Azure CDN](cdn-overview.md)
