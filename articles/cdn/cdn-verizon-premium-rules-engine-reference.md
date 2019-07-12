---
title: Naslaginformatie over Azure CDN de regelengine | Microsoft Docs
description: Naslagdocumentatie voor Azure CDN regels criteria voor overeenkomst regelengine en functies.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 5fc611af75a7f733576f9343a4375fb56cacc030
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593148"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-reference"></a>Azure CDN van Verizon Premium regels-engine-verwijzing

Dit artikel vindt u gedetailleerde beschrijvingen van de criteria voor overeenkomst beschikbaar en functies voor het Azure Content Delivery Network (CDN) [regels-engine](cdn-verizon-premium-rules-engine.md).

De regelengine is ontworpen om de laatste instantie op hoe specifieke typen aanvragen worden verwerkt door het CDN.

**Veelvoorkomende toepassingen**:

- Overschrijven of een aangepaste-cache-beleid moet worden gedefinieerd.
- Beveilig of weigeren van aanvragen voor gevoelige inhoud.
- Aanvragen omleiden.
- Het aangepaste logboek data Store.

## <a name="terminology"></a>Terminologie

Een regel wordt gedefinieerd via het gebruik van [ **voorwaardelijke expressies**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [ **voldoen aan bepaalde voorwaarden**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), en [ **functies**](cdn-verizon-premium-rules-engine-reference-features.md). Deze elementen zijn gemarkeerd in de volgende afbeelding:

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
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | An asterisk allows the preceding character to be matched zero or more times.
Space | A space character is typically treated as a literal character.
'value' | Single quotes are treated as literal characters. A set of single quotes does not have special meaning.

## Next steps

- [Rules engine match conditions](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Override HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)
- [Azure CDN overview](cdn-overview.mdDe betekenis van een percentagesymbool, is afhankelijk van het gebruik ervan. Docs
descr`%{HTTPVariable}`: Deze syntaxis identificeert een HTTP-variabele.match`%{HTTPVariable%Pattern}`: Deze syntaxis gebruikt een percentagesymbool om te identificeren van een HTTP-variabele en als een scheidingsteken.1/2019`\``: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\`20`).f the available match conditions and features for the Azure Content Delivery Network (CDN) [rules engine](cdn-verizon-premium-rules-engine.md).

The rules engine is designed to be the final authority on how specific types of requests are processed by the CDN.

**Common uses**:

- Override or define a custom cache policy.
- Secure or deny requests for sensitive content.
- Redirect requests.
- Store custom log data.

## Terminology

A rule is defined through the use of [**conditional expressions**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**match conditions**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), and [**features**](cdn-verizon-premium-rules-engine-reference-features.md). These elements are highlighted in the following illustration:

 ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## Syntax

The manner in which special characters are treated varies according to how a match condition or feature handles text values. A match condition or feature may interpret text in one of the following ways:

1. [**Literal values**](#literal-values)
2. [**Wildcard values**](#wildcard-values)
3. [**Regular expressions**](#regular-expressions)

### Literal values

Text that is interpreted as a literal value treats all special characters, with the exception of the % symbol, as a part of the value that must be matched. In other words, a literal match condition set to `\'*'\` is only satisfied when that exact value (that is, `\'*'\`) is found.

A percentage symbol is used to indicate URL encoding (for example, `%20`).

### Wildcard values

Text that is interpreted as a wildcard value assigns additional meaning to special characters. The following table describes how the following set of characters is interpreted:

Character | Description
----------|------------
\ | A backslash is used to escape any of the characters specified in this table. A backslash must be specified directly before the special character that should be escaped.<br/>For example, the following syntax escapes an asterisk: `\*`
% | A percentage symbol is used to indicate URL encoding (for example, `%20`).
\* | An asterisk is a wildcard that represents one or more characters.
Space | A space character indicates that a match condition may be satisfied by either of the specified values or patterns.
'value' | A single quote does not have special meaning. However, a set of single quotes is used to indicate that a value should be treated as a literal value. It can be used in the following ways:<br><br/>- It allows a match condition to be satisfied whenever the specified value matches any portion of the comparison value.  For example, `'ma'` would match any of the following strings: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />- It allows a special character to be specified as a literal character. For example, you may specify a literal space character by enclosing a space character within a set of single quotes (that is, `' '` or `'sample value'`).<br/>- It allows a blank value to be specified. Specify a blank value by specifying a set of single quotes (that is, '').<br /><br/>**Important:**<br/>- If the specified value does not contain a wildcard, then it is automatically considered a literal value, which means that it is not necessary to specify a set of single quotes.<br/>- If a backslash does not escape another character in this table, it is ignored when it is specified within a set of single quotes.<br/>- Another way to specify a special character as a literal character is to escape it using a backslash (that is, `\`).

### Regular expressions

Regular expressions define a pattern that is searched for within a text value. Regular expression notation defines specific meanings to a variety of symbols. The following table indicates how special characters are treated by match conditions and features that support regular expressions.

Special Character | Description
------------------|------------
\ | A backslash escapes the character the follows it, which causes that character to be treated as a literal value instead of taking on its regular expression meaning. For example, the following syntax escapes an asterisk: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | Een sterretje kan het vorige teken nul of meer keer worden vergeleken.
Ruimte | Een spatie wordt doorgaans behandeld als een letterlijk teken.
'value' | Enkele aanhalingstekens worden behandeld als letterlijke tekens. Een set met enkele aanhalingstekens heeft geen speciale betekenis.

## <a name="next-steps"></a>Volgende stappen

- [Criteria voor overeenkomst regelengine](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Voorwaardelijke expressies regelengine](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Functies regelengine](cdn-verizon-premium-rules-engine-reference-features.md)
- [Met behulp van de regelengine HTTP-gedrag negeren](cdn-verizon-premium-rules-engine.md)
- [Overzicht van Azure CDN](cdn-overview.md)