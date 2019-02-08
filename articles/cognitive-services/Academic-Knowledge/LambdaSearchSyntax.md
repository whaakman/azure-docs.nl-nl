---
title: Lambda-zoeksyntaxis - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de Lambda-zoeksyntaxis die kunt u in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 4d4c540e00794bfdf1df265457798cc13530c828
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873196"
---
# <a name="lambda-search-syntax"></a>Lambda-Zoeksyntaxis

Elke *lambda* zoekquery tekenreeks een graph-patroon wordt beschreven. Een query moet ten minste één vanaf knooppunt op te geven van welk knooppunt graph beginnen we eerst het transport. Als u een eerste knooppunt, aanroepen de *MAG. StartFrom()* methode en geeft u de id's van een of meer knooppunten of een query-object dat Hiermee geeft u de zoekcriteria. De *StartFrom()* methode heeft drie overloads. Al deze neemt twee argumenten met de tweede wordt optioneel. Het eerste argument kan een lange integer, een verzameling invoeroverzicht van lange geheel getal, of een tekenreeks voor een JSON-object, met dezelfde semantiek als in *json* zoeken:
```
StartFrom(long cellid, IEnumerable<string> select = null)
StartFrom(IEnumerable<long> cellid, IEnumerable<string> select = null)
StartFrom(string queryObject, IEnumerable<string> select = null)
```

Het proces van het schrijven van een lambda-zoekquery wordt worden uitgelegd van het ene knooppunt naar een andere. Gebruik het opgeven van het type van de rand om te zien hoe *FollowEdge()* en door te geven in de gewenste edge-typen. *FollowEdge()* duurt een willekeurig aantal tekenreeksargumenten:
```
FollowEdge(params string[] edgeTypes)
```
> [!NOTE]
> Als we niet geïnteresseerd in het type van de edge(s) bent te volgen, laat *FollowEdge()* tussen twee knooppunten: de query helpt bij de mogelijke randen tussen deze twee knooppunten.

We kunnen de traversal acties worden uitgevoerd op een knooppunt via opgeven *VisitNode()*, dat wil zeggen, of om te stoppen op dit knooppunt en het huidige pad als het resultaat te retourneren, of om door te gaan naar het verkennen van de grafiek.  Het type enum *actie* definieert u twee soorten acties: *Action.Return* en *Action.Continue*. Die kunnen worden doorgegeven die een enum-waarde rechtstreeks in *VisitNode()*, of ze worden gecombineerd met bitwise- en de operator '&'. Als twee actie worden gecombineerd, betekent dit dat beide acties worden uitgevoerd. Opmerking: gebruik geen Bitsgewijs- of -operator ' |' op acties. In dat geval zorgt ervoor dat de query te beëindigen zonder iets te retourneren. Overgeslagen *VisitNode()* tussen twee *FollowEdge()* aanroepen zorgt ervoor dat de query in de grafiek onvoorwaardelijk verkennen nadat zij op een knooppunt.

```
VisitNode(Action action, IEnumerable<string> select = null)
```

Voor *VisitNode()*, die ook kunnen worden doorgegeven in een lambda-expressie van het type *expressie\<Func\<Inodes, actie\>\>*, die een duurt*Inodes* en retourneert een transport-actie:

```
VisitNode(Expression<Func<INode, Action>> action, IEnumerable<string> select = null)
```

## <a name="inode"></a>*INode* 

*Inodes* biedt *alleen-lezen* data access-interfaces en een aantal ingebouwde hulpfuncties die op een knooppunt. 

### <a name="basic-data-access-interfaces"></a>Basic data access-interfaces

##### <a name="long-cellid"></a>long CellID

De 64-bits-ID van het knooppunt. 

##### <a name="t-getfieldtstring-fieldname"></a>T GetField\<T\>(fieldName string)

Hiermee haalt u de waarde van de opgegeven eigenschap. *T* is het gewenste type dat het veld worden geïnterpreteerd moet als. Automatische type casten zal worden uitgevoerd als het gewenste type kan niet impliciet worden geconverteerd van het type van het veld. Opmerking: als de eigenschap is meerdere waarden *GetField\<tekenreeks\>*  zorgt ervoor dat de lijst om te worden geserialiseerd naar een Json-tekenreeks ["val1", "val2',...]. Als de eigenschap niet bestaat, wordt een uitzondering en afbreken van de huidige graph verkennen.

##### <a name="bool-containsfieldstring-fieldname"></a>bool ContainsField(string fieldName)

Geeft aan of een veld met de opgegeven naam in het huidige knooppunt bestaat.

##### <a name="string-getstring-fieldname"></a>tekenreeks (tekenreeks veldnaam) ophalen

Werkt als *GetField\<tekenreeks\>(veldnaam)*. Echter deze komt niet uitzonderingen genereren wanneer het veld niet wordt gevonden, resulteert dit in een lege string("") in plaats daarvan.

##### <a name="bool-hasstring-fieldname"></a>BOOL heeft (veldnaam tekenreeks)

Geeft aan of de opgegeven eigenschap in het huidige knooppunt bestaat. Hetzelfde als *ContainsField(fieldName)*.

##### <a name="bool-hasstring-fieldname-string-value"></a>BOOL heeft (veldnaam string, string-waarde)

Geeft aan of de eigenschap de opgegeven waarde heeft. 

##### <a name="int-countstring-fieldname"></a>int count (veldnaam tekenreeks)

Het aantal waarden van een bepaalde eigenschap ophalen. Wanneer de eigenschap niet bestaat, retourneert 0.

### <a name="built-in-helper-functions"></a>Ingebouwde ondersteunende functies

##### <a name="action-returnifbool-condition"></a>Actie return_if (Boole-voorwaarde)

Retourneert *Action.Return* als de voorwaarde is *waar*. Als de voorwaarde is *false* en deze expressie is niet samengevoegd met andere acties van een bitwise- en -operator, de verkenning van de grafiek wordt afgesloten.

##### <a name="action-continueifbool-condition"></a>Actie continue_if (Boole-voorwaarde)

Retourneert *Action.Continue* als de voorwaarde is *waar*. Als de voorwaarde is *false* en deze expressie is niet samengevoegd met andere acties van een bitwise- en -operator, de verkenning van de grafiek wordt afgesloten.

##### <a name="bool-dicedouble-p"></a>BOOL dice (dubbele p)

Genereert een willekeurig getal dat groter is dan of gelijk zijn aan 0,0 en kleiner dan 1,0 liggen. Deze functie retourneert *waar* alleen als het getal is kleiner dan of gelijk zijn aan *p*.

Vergeleken met *json* zoeken, *lambda* zoekopdracht heeft meer expressieve: C#Lambda-expressies kunnen rechtstreeks worden gebruikt om op te geven querypatronen. Hier volgen twee voorbeelden.

```
MAG.StartFrom(@"{
    type  : ""ConferenceSeries"",
    match : {
        FullName : ""graph""
    }
}", new List<string>{ "FullName", "ShortName" })
.FollowEdge("ConferenceInstanceIDs")
.VisitNode(v => v.return_if(v.GetField<DateTime>("StartDate").ToString().Contains("2014")),
        new List<string>{ "FullName", "StartDate" })
```

```
MAG.StartFrom(@"{
    type  : ""Affiliation"",
    match : {
        Name : ""microsoft""
    }
}").FollowEdge("PaperIDs")
.VisitNode(v => v.return_if(v.get("NormalizedTitle").Contains("graph") || v.GetField<int>("CitationCount") > 100),
        new List<string>{ "OriginalTitle", "CitationCount" })
```
