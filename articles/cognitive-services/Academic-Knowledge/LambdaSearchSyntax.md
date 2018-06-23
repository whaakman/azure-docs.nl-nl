---
title: Lambda-zoeksyntaxis in de Academic Knowledge API | Microsoft Docs
description: Meer informatie over de syntaxis van de Lambda-zoekopdracht die kunt u in de Academic Knowledge API in cognitieve Microsoft-Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f486368e1d0258087091acb846a84b125712db40
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344440"
---
# <a name="lambda-search-syntax"></a>Lambda-Zoeksyntaxis

Elke *lambda* zoekquery tekenreeks beschrijft een patroon van de grafiek. Een query moet ten minste één beginknooppunt, geven we start vanaf welk knooppunt van de grafiek het transport hebben. Geef een beginknooppunt door aanroepen de *magnetische. StartFrom()* methode en geeft u de id('s) van een of meer knooppunten of een query-object waarmee de zoekcriteria. De *StartFrom()* methode heeft drie overloads. Deze duren twee argumenten met de tweede wordt optioneel. Het eerste argument kan een lange integer, een lange integer inventariseerbare verzameling of een tekenreeks voor een JSON-object met dezelfde betekenis zoals in *json* zoeken:
```
StartFrom(long cellid, IEnumerable<string> select = null)
StartFrom(IEnumerable<long> cellid, IEnumerable<string> select = null)
StartFrom(string queryObject, IEnumerable<string> select = null)
```

Schrijven van een lambda-zoekquery wordt stapsgewijs van het ene knooppunt naar een andere. Gebruik het opgeven van het type van de rand te doorlopen *FollowEdge()* en in de gewenste rand typen. *FollowEdge()* duurt een willekeurig aantal tekenreeksargumenten:
```
FollowEdge(params string[] edgeTypes)
```
> [!NOTE]
> Als er niet van belang is over de typen van de edge(s) te volgen, weglaten *FollowEdge()* tussen twee knooppunten: de query wordt de mogelijke randen doorlopen tussen deze twee knooppunten.

We kunnen de traversal acties moeten worden uitgevoerd op een knooppunt via opgeven *VisitNode()*, dat wil zeggen, of om te stoppen op dit knooppunt en het huidige pad als het resultaat te retourneren of moet worden doorgegaan met het verkennen van de grafiek.  Het enum-type *actie* definieert twee soorten bewerkingen: *Action.Return* en *Action.Continue*. Geven we deze een enum-waarde rechtstreeks in *VisitNode()*, of ze combineren met bitwise- en de operator '&'. Wanneer twee actie worden gecombineerd, betekent dit dat beide acties worden uitgevoerd. Opmerking: gebruik geen Bitsgewijs- of operator ' |' van de acties. Hierdoor wordt de query is beëindigd zonder iets te retourneren. Overslaan *VisitNode()* tussen twee *FollowEdge()* aanroepen, wordt de query voor de grafiek onvoorwaardelijk verkennen nadat zij op een knooppunt.

```
VisitNode(Action action, IEnumerable<string> select = null)
```

Voor *VisitNode()*, kunnen we ook doorgeven in een lambda-expressie van het type *expressie\<Func\<inodes door, actie\>\>*, wat een duurt*Inodes door* en retourneert een transport-actie:

```
VisitNode(Expression<Func<INode, Action>> action, IEnumerable<string> select = null)
```

## <a name="inode"></a>*Inodes door* 

*Inodes door* biedt *alleen-lezen* data access-interfaces en enkele ingebouwde hulpfuncties die op een knooppunt. 

### <a name="basic-data-access-interfaces"></a>Basic data access-interfaces

##### <a name="long-cellid"></a>lange CellID

De 64-bits-ID van het knooppunt. 

##### <a name="t-getfieldtstring-fieldname"></a>T GetField\<T\>(string fieldName)

Hiermee wordt de waarde van de opgegeven eigenschap. *T* is het gewenste type dat moet worden geïnterpreteerd als het veld. Automatische type casten wordt geprobeerd als het gewenste type kan niet impliciet worden geconverteerd van het type van het veld. Opmerking: als de eigenschap is met meerdere waarden *GetField\<tekenreeks\>*  , wordt de lijst om te worden geserialiseerd naar een Json-tekenreeks ['waarde1', 'waarde2',...]. Als de eigenschap niet bestaat, wordt Veroorzaak een exception en de huidige grafiek exploratie afbreken.

##### <a name="bool-containsfieldstring-fieldname"></a>BOOL ContainsField (veldnaam tekenreeks)

Geeft aan of een veld met de gegeven naam in het huidige knooppunt bestaat.

##### <a name="string-getstring-fieldname"></a>tekenreeks (string veldnaam) ophalen

Werkt als *GetField\<tekenreeks\>(veldnaam)*. Echter deze komt niet uitzonderingen genereert wanneer het veld niet wordt gevonden, wordt een lege string("") in plaats daarvan.

##### <a name="bool-hasstring-fieldname"></a>BOOL heeft (veldnaam tekenreeks)

Geeft aan of de opgegeven eigenschap in het huidige knooppunt bestaat. Hetzelfde als *ContainsField(fieldName)*.

##### <a name="bool-hasstring-fieldname-string-value"></a>BOOL heeft (veldnaam string, string-waarde)

Geeft aan of de eigenschap de opgegeven waarde heeft. 

##### <a name="int-countstring-fieldname"></a>int count (veldnaam tekenreeks)

Het aantal waarden van een bepaalde eigenschap ophalen. Wanneer de eigenschap niet bestaat, retourneert 0.

### <a name="built-in-helper-functions"></a>Ingebouwd Help-functies

##### <a name="action-returnifbool-condition"></a>Actie return_if (bool voorwaarde)

Retourneert *Action.Return* als de voorwaarde *true*. Als de voorwaarde *false* en deze expressie wordt niet samengevoegd met andere acties van een bitwise- en -operator, de exploratie van de grafiek wordt afgebroken.

##### <a name="action-continueifbool-condition"></a>Actie continue_if (bool voorwaarde)

Retourneert *Action.Continue* als de voorwaarde *true*. Als de voorwaarde *false* en deze expressie wordt niet samengevoegd met andere acties van een bitwise- en -operator, de exploratie van de grafiek wordt afgebroken.

##### <a name="bool-dicedouble-p"></a>BOOL opdelen (dubbele p)

Genereert een willekeurig getal dat groter is dan of gelijk zijn aan 0,0 en kleiner dan 1.0. Deze functie retourneert *true* alleen als het aantal is kleiner dan of gelijk zijn aan *p*.

Vergeleken met de *json* zoeken, *lambda* zoekopdracht heeft meer expressieve: C# lambda-expressies kunnen rechtstreeks worden gebruikt om op te geven querypatronen. Hier vindt u twee voorbeelden.

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
