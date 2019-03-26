---
title: Vereenvoudigde querysyntaxis - Azure Search
description: Referentie voor de eenvoudige query-syntaxis voor volledige-tekstquery's in Azure Search gebruikt.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/25/2019
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 99729141e5e1478f45ad385cf671c44a8e08f21a
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437489"
---
# <a name="simple-query-syntax-in-azure-search"></a>Vereenvoudigde querysyntaxis in Azure Search
Azure Search worden twee op basis van Lucene querytalen geïmplementeerd: [De eenvoudige Queryparser](https://lucene.apache.org/core/4_7_0/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) en de [Lucene-Queryparser](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). In Azure Search sluit de vereenvoudigde querysyntaxis de fuzzy/slop-opties.  

> [!NOTE]  
>  Azure Search biedt een alternatief [Lucene-querysyntaxis](query-lucene-syntax.md) voor complexere query's. Zie voor meer informatie over de architectuur en voordelen van elke syntaxis parseren van de query, [hoe volledige tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md).

## <a name="how-to-invoke-simple-parsing"></a>Het parseren van eenvoudige aanroepen

Eenvoudige syntaxis is de standaardinstelling. Aanroep is alleen nodig als u de syntaxis van volledig in een eenvoudig opnieuw instelt. U kunt de syntaxis van de expliciet instellen met de `queryType` zoekparameter. Geldige waarden zijn `simple|full`, met `simple` als de standaard, en `full` voor Lucene. 

## <a name="query-behavior-anomalies"></a>Query gedrag afwijkingen

Als er geen tekst met een of meer voorwaarden wordt beschouwd als een geldige beginpunt voor het uitvoeren van query's. Azure Search komt overeen met documenten met een of meer van de voorwaarden, inclusief eventuele wijzigingen gevonden tijdens de analyse van de tekst. 

Dezelfde wijze als dit klinkt, is er slechts één aspect van queryuitvoering in Azure Search die *mogelijk* produceren, onverwachte resultaten, verhogen in plaats van search verlagen resulteert naarmate meer voorwaarden en operators worden toegevoegd aan de invoer tekenreeks. Of deze uitbreiding daadwerkelijk gebeurt is afhankelijk van het opnemen van een niet-operator, gecombineerd met een `searchMode` instelling van de parameter waarmee wordt bepaald hoe niet wordt geïnterpreteerd in termen van en of of gedrag. De standaardwaarde opgegeven `searchMode=Any`, en een niet-operator, de bewerking wordt berekend als een actie of zodat `"New York" NOT Seattle` retourneert alle plaatsen die geen Seattle.  

Normaal gesproken waarschijnlijk u sneller om te zien van deze problemen in gebruiker interactie patronen voor toepassingen die zoeken in de inhoud, waar gebruikers zich waarschijnlijk om op te nemen van een operator in een query, in plaats van e-commerce sites waarvoor meer ingebouwde navigatiestructuur. Zie voor meer informatie, [operator NOT](#not-operator). 

## <a name="boolean-operators-and-or-not"></a>Booleaanse operators (AND, OR, NOT) 

U kunt operators insluiten in een queryreeks aan het bouwen van een grote verscheidenheid aan de criteria op basis waarvan de overeenkomende documenten zijn gevonden. 

### <a name="and-operator-"></a>EN de operator `+`

De operator is een plusteken (+). Bijvoorbeeld, `wifi+luxury` wordt zoeken naar documenten met beide `wifi` en `luxury`.

### <a name="or-operator-"></a>OF een operator `|`

De OR-operator is een verticale balk of een pipe-teken. Bijvoorbeeld, `wifi | luxury` wordt zoeken naar documenten met een `wifi` of `luxury` of beide.

<a name="not-operator"></a>

### <a name="not-operator--"></a>NIET-operator `-`

De niet-operator is een minteken. Bijvoorbeeld, `wifi –luxury` wordt zoeken naar documenten waarvoor de `wifi` termijn en/of geen `luxury` (en/of wordt bepaald door `searchMode`).

> [!NOTE]  
>  De `searchMode` besturingselementen de optie of een term met de operator NOT and of samengevoegd met de andere voorwaarden in de query in de afwezigheid van is een `+` of `|` operator. Intrekken die `searchMode` kan worden ingesteld op `any` (standaard) of `all`. Als u `any`, het intrekken van query's wordt verhoogd door te nemen meer resultaten en standaard `-` wordt geïnterpreteerd als 'Of niet'. Bijvoorbeeld, `wifi -luxury` komt overeen met documenten dat beide de term bevatten `wifi` of die niet de term hebben `luxury`. Als u `all`, de precisie van de query's worden verhoogd door te nemen minder resultaten en standaard - wordt geïnterpreteerd als 'En niet'. Bijvoorbeeld, `wifi -luxury` komt overeen met de documenten die de term bevatten `wifi` en geen bevatten die de term 'luxe'. Dit is weliswaar een meer intuïtieve benadering gedrag voor de `-` operator. Daarom moet u overwegen `searchMode=all` in plaats van `searchMode=any` als u wilt optimaliseren zoekt precisie in plaats van zoals eerder vermeld, *en* uw gebruikers gebruiken vaak de `-` operator in zoekopdrachten.

## <a name="suffix-operator"></a>Achtervoegsel operator

De operator achtervoegsel is een sterretje `*`. Bijvoorbeeld, `lux*` wordt zoeken naar documenten die gelden voor een termijn die met begint `lux`, niet hoofdlettergevoelig.  

## <a name="phrase-search-operator"></a>De zoekoperator woordgroep

De operator woordgroep een woordgroep tussen aanhalingstekens `" "`. Bijvoorbeeld, `Roach Motel` (zonder aanhalingstekens) wilt zoeken naar documenten met `Roach` en/of `Motel` overal in een willekeurige volgorde `"Roach Motel"` (met aanhalingstekens) wordt er alleen gezocht naar documenten die de hele woordgroep samen en in die bevatten volgorde (analyse van tekst nog steeds van toepassing).

## <a name="precedence-operator"></a>Prioriteit-operator

De operator prioriteit tekenreeks tussen haakjes `( )`. Bijvoorbeeld, `motel+(wifi | luxury)` wordt zoeken naar documenten met de term motel en een `wifi` of `luxury` (of beide).  

## <a name="escaping-search-operators"></a>Aanhalingstekens Zoekoperators  

 Als u wilt gebruiken de bovenstaande symbolen als werkelijke onderdeel van de zoektekst, moeten ze worden voorafgegaan door het voorvoegsel wordt deze gemaakt met een backslash. Bijvoorbeeld, `luxury\+hotel` zal leiden tot de term `luxury+hotel`. Om te kunnen indienen dingen eenvoudig de meer gebruikelijke gevallen, zijn er twee uitzonderingen op deze regel waar aanhalingstekens is niet nodig:  

- De operator NOT `-` moet alleen worden weergegeven als dit het eerste teken na een witruimte zijn, niet als deze zich in een term bevindt. Bijvoorbeeld, `wi-fi` is een termijn van één; terwijl GUID's (zoals `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`) worden behandeld als een enkele token.
- De operator achtervoegsel `*` moet worden weergegeven alleen als dit het laatste teken voordat een witruimte zijn, niet als deze zich in een term bevindt. Bijvoorbeeld, `wi*fi` wordt beschouwd als een enkele token.

> [!NOTE]  
>  Hoewel houdt tokens samen aanhalingstekens, analyse van tekst mogelijk gesplitst, afhankelijk van de analyse-modus. Zie [taalondersteuning &#40;Azure Search Service REST API&#41; ](index-add-language-analyzers.md) voor meer informatie.  

## <a name="see-also"></a>Zie ook  

+ [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 
+ [Lucene-querysyntaxis](query-lucene-syntax.md)
+ [Syntaxis voor OData-expressie](query-odata-filter-orderby-syntax.md) 
