---
title: Query-expressies in de API-Service exploratie kennis gestructureerd | Microsoft Docs
description: Informatie over het gebruik van structured query-expressies in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 070ee311a1153bc9fb59870dce68f385a43b15f1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344517"
---
# <a name="structured-query-expression"></a>Structured Query-expressie
Een structured query-expressie bevat een reeks bewerkingen worden geëvalueerd op basis van de index.  Het bestaat kenmerk query-expressies en een hoger niveau functies.  Gebruik de [ *evalueren* ](evaluateMethod.md) methode voor het berekenen van de objecten die overeenkomt met de expressie.  Hier volgt een voorbeeld van het domein academic publicaties die als resultaat publicaties geschreven door Jaime Teevan sinds het jaar 2013 geeft.

`And(Composite(Author.Name=='jaime teevan'),Y>=2013)`

Structured query-expressies kunnen worden verkregen bij [ *interpreteren* ](interpretMethod.md) aanvragen, waarbij de semantische uitvoer van elke interpretatie is een structured query-expressie die als resultaat geeft de index-objecten die overeenkomt met de invoer natuurlijke taal query.  U kunt ook kunnen ze worden handmatig geschreven met behulp van de syntaxis van de in deze sectie beschreven.

## <a name="attribute-query-expression"></a>Kenmerk Query-expressie
Een query-expressie van het kenmerk identificeert een set objecten op basis van overeenkomst met een bepaald kenmerk.  Verschillende overeenkomende bewerkingen worden ondersteund, afhankelijk van het kenmerktype en geïndexeerde bewerking opgegeven in de [schema](SchemaFormat.md):

| Type | Bewerking | Voorbeelden |
|------|-------------|------------|
| Reeks | is gelijk aan | Titel = 'latente semantische analyse' (canonieke + synoniemen) |
| Reeks | is gelijk aan | Author.Name=='susan t dumais (canonieke alleen)|
| Reeks | starts_with | Titel = 'latente s'... |
| Int64-Int32/Double | is gelijk aan | Jaar 2000 = |
| Int64-Int32/Double | starts_with | Jaar = '20'... (een decimale waarde beginnen met '20') |
| Int64-Int32/Double | is_between | Jaar&lt;2000 <br/> Jaar&lt;= 2000 <br/> Jaar&gt;2000 <br/> Jaar&gt;= 2000 <br/> Year=[2010,2012) *(bevat alleen links grenswaarde: 2010, 2011)* <br/> Jaar = [2000,2012] *(beide waarden bevat: 2010, 2011, 2012)* |
| Date | is gelijk aan | Geboortedatum ='1984-05-14' |
| Date | is_between | Geboortedatum&lt;='03-2008/14' <br/> PublishDate = ['2000-01-01', ' 2009-12-31'] |
| GUID | is gelijk aan | Id = '602DD052-CC47-4B23-A16A-26B52D30C05B' |


Bijvoorbeeld de expressie "Title = 'latente s'... ' komt overeen met alle academic publicaties met de titel met de tekenreeks"latente s begint".  Om deze expressie niet evalueren, moet het kenmerk titel de bewerking 'starts_with' opgeven in het schema gebruikt om de index te maken.

Voor kenmerken met een bijbehorende synoniemen, kan een query-expressie objecten waarvan de canonieke waarde overeenkomt met de opgegeven tekenreeks met de operator "==" of objecten waarbij een van de waarden van de canonieke/synoniem overeenkomen met de operator '=' opgeven.  Zowel de operator 'is gelijk aan' worden opgegeven in de kenmerkdefinitie is vereist.


## <a name="functions"></a>Functions
Er is een ingebouwde verzameling functies waarmee de constructie van meer geavanceerde query-expressies basic kenmerk query's.

### <a name="and-function"></a>En de functie
`And(expr1, expr2)`

Retourneert het snijpunt van de twee ingevoerde queryexpressies.

Het volgende voorbeeld retourneert academic publicaties die zijn gepubliceerd in het jaar 2000 over het ophalen van informatie:

`And(Year=2000, Keyword=='information retrieval')`

### <a name="or-function"></a>Of functie
`Or(expr1, expr2)`

Retourneert de vereniging van de twee ingevoerde queryexpressies.

Het volgende voorbeeld retourneert academic publicaties die zijn gepubliceerd in het jaar 2000 over het ophalen van informatie of modellering van gebruiker:

`And(Year=2000, Or(Keyword='information retrieval', Keyword='user modeling'))`

### <a name="composite-function"></a>Samengestelde functie
`Composite(expr)`

Retourneert een expressie die een interne expressie ingekapseld bestaat uit een query uitgevoerd naar de submap kenmerken van een gemeenschappelijk samengesteld kenmerk.  De inkapseling moet het samengestelde kenmerk van een overeenkomende gegevensobject hebben van ten minste één waarde die afzonderlijk voldoet aan de interne expressie.  Houd er rekening mee dat er een query-expressie op onderliggende kenmerken van een samengesteld kenmerk heeft te integreren met behulp van de functie Composite() voordat deze kan worden gecombineerd met andere query-expressies.

Bijvoorbeeld retourneert de volgende expressie academic publicaties door 'harry shum' terwijl hij op 'microsoft':

```
Composite(And(Author.Name="harry shum", 
              Author.Affiliation="microsoft"))
```

De volgende expressie retourneert aan de andere kant academic publicaties waarin een van de auteurs is 'harry shum' en een van de verwantschappen 'microsoft':

```
And(Composite(Author.Name="harry shum"), 
    Composite(Author.Affiliation="microsoft"))
```
