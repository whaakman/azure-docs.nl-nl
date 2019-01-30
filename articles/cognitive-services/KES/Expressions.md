---
title: Gestructureerde query-expressies - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van gestructureerde query-expressies in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 335bcc025d2f3e972a02234da89e35c90c91afeb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222692"
---
# <a name="structured-query-expression"></a>Gestructureerde Query-expressie

Een gestructureerde query-expressie bevat een reeks bewerkingen om te beoordelen op basis van de gegevensindex.  Deze bestaat uit kenmerk query-expressies en functies op een hoger niveau.  Gebruik de [ *evalueren* ](evaluateMethod.md) methode voor het berekenen van de objecten die overeenkomen met de expressie.  Hier volgt een voorbeeld van het domein academische publicaties die als resultaat publicaties die zijn geschreven door Jaime Teevan sinds het jaar 2013 geeft.

`And(Composite(Author.Name=='jaime teevan'),Y>=2013)`

Gestructureerde query-expressies kunnen worden verkregen uit [ *interpreteren* ](interpretMethod.md) aanvragen, waarbij de semantische uitvoer van elke interpretatie is een gestructureerde query-expressie die wordt geretourneerd van de index-objecten die overeenkomen met de query van de invoer van natuurlijke taal.  U kunt ook kunnen deze worden handmatig geschreven met behulp van de syntaxis van de in deze sectie beschreven.

## <a name="attribute-query-expression"></a>Kenmerk Query-expressie

Een kenmerk query-expressie wordt een set van objecten op basis van overeenkomst op basis van een specifiek kenmerk geïdentificeerd.  Verschillende overeenkomende bewerkingen worden ondersteund, afhankelijk van het kenmerktype en geïndexeerde bewerking opgegeven in de [schema](SchemaFormat.md):

| Type | Bewerking | Voorbeelden |
|------|-------------|------------|
| Reeks | is gelijk aan | Titel = 'latente semantische analyse' (canonieke + synoniemen) |
| Reeks | is gelijk aan | Author.Name=='susan t dumais (canonieke alleen)|
| Reeks | starts_with | Titel = 'latente s'... |
| Int32/Int64/Double-waarde | is gelijk aan | Year=2000 |
| Int32/Int64/Double-waarde | starts_with | Jaar = "20"... (een decimale waarde beginnen met "20") |
| Int32/Int64/Double-waarde | is_between | Jaar&lt;2000 <br/> Jaar&lt;= 2000 <br/> Jaar&gt;2000 <br/> Jaar&gt;= 2000 <br/> Year=[2010,2012) *(alleen links grenswaarde bevat: 2010, 2011)* <br/> Jaar = [2000,2012] *(beide waarden bevat: 2010, 2011, 2012)* |
| Date | is gelijk aan | BirthDate='1984-05-14' |
| Date | is_between | Geboortedatum&lt;=' 2008/03/14' <br/> Publicatiedatum = ['2000-01-01', ' 31-12-2009'] |
| GUID | is gelijk aan | Id='602DD052-CC47-4B23-A16A-26B52D30C05B' |


Bijvoorbeeld, de expressie "Title = 'latente s'... ' komt overeen met alle academische publicaties waarvan u de titel met de tekenreeks"latente s begint".  Als u wilt evalueren deze expressie, moet de titel van het kenmerk de bewerking 'starts_with' opgeven in het schema dat wordt gebruikt voor het bouwen van de index.

Voor kenmerken met een bijbehorende synoniemen, kan een query-expressie waarvan de canonieke waarde komt overeen met een opgegeven tekenreeks met de operator "==" of objecten waarbij een van de canonieke/synoniem waarden overeenkomen met de operator "=" objecten opgeven.  Zowel de operator "is gelijk aan" moet worden opgegeven in de kenmerkdefinitie van het is vereist.


## <a name="functions"></a>Functions

Er is een ingebouwde verzameling functies waarmee de opbouw van complexere query-expressies basic kenmerk query's.

### <a name="and-function"></a>And -functie

`And(expr1, expr2)`

Retourneert het snijpunt van de twee ingevoerde query-expressies.

Het volgende voorbeeld retourneert de academische publicaties die zijn gepubliceerd in het jaar 2000 over het ophalen van informatie:

`And(Year=2000, Keyword=='information retrieval')`

### <a name="or-function"></a>Anders werkt

`Or(expr1, expr2)`

Retourneert de samenvoeging van de twee ingevoerde query-expressies.

Het volgende voorbeeld retourneert de academische publicaties die zijn gepubliceerd in het jaar 2000 over het ophalen van gegevens of gebruiker modellen:

`And(Year=2000, Or(Keyword='information retrieval', Keyword='user modeling'))`

### <a name="composite-function"></a>Samengestelde functie

`Composite(expr)`

Retourneert een expressie die een binnenste expressie ingekapseld bestaat uit de query's op onderliggende kenmerken van een gemeenschappelijk samengesteld kenmerk.  De inkapseling vereist het samengestelde kenmerk van overeenkomende gegevensobject dat ten minste één waarde die afzonderlijk voldoet aan de interne expressie.  Houd er rekening mee dat een query-expressie op onderliggende kenmerken van een samengestelde kenmerk heeft te integreren met behulp van de functie Composite() voordat deze kan worden gecombineerd met andere query-expressies.

De volgende expressie retourneert bijvoorbeeld academische publicaties door "harry shum" terwijl hij op 'microsoft is':

```
Composite(And(Author.Name="harry shum", 
              Author.Affiliation="microsoft"))
```

De volgende expressie retourneert aan de andere kant academische publicaties daar een van de auteurs "harry shum' en een van de lidmaatschappen is 'microsoft':

```
And(Composite(Author.Name="harry shum"), 
    Composite(Author.Affiliation="microsoft"))
```
