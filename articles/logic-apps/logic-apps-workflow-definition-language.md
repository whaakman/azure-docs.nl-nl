---
title: Werkstroom Definition Language schema - Azure Logic Apps | Microsoft Docs
description: "Werkstromen op basis van het schema van werkstroom definitie voor Azure Logic Apps definiëren"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 36eee42b7b10dfb62e569d665f62a94fc94365be
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="workflow-definition-language-schema-for-azure-logic-apps"></a>Werkstroom Definition Language-schema voor Azure Logic Apps

De werkstroomdefinitie van een bevat de werkelijke logica die wordt uitgevoerd als onderdeel van uw logische app. Deze definitie bevat een of meer triggers die de logische app start en een of meer acties voor de logische app te laten worden.  
  
## <a name="basic-workflow-definition-structure"></a>Basiswerkstroom definitie structuur

Hier wordt de basisstructuur van een werkstroomdefinitie:  
  
```json
{
    "$schema": "<schema-of the-definition>",
    "contentVersion": "<version-number-of-definition>",
    "parameters": { <parameter-definitions-of-definition> },
    "triggers": [ { <definition-of-flow-triggers> } ],
    "actions": [ { <definition-of-flow-actions> } ],
    "outputs": { <output-of-definition> }
}
```
  
> [!NOTE]
> De [werkstroom Management REST API](https://docs.microsoft.com/rest/api/logic/workflows) documentatie bevat informatie over het maken en beheren van werkstromen voor logic app.
  
|Elementnaam|Vereist|Beschrijving|  
|------------------|--------------|-----------------|  
|$schema|Nee|Hiermee geeft u de locatie voor de JSON-schema-bestand dat de versie van de definition language beschrijft. Deze locatie is vereist wanneer u verwijst naar een extern-definitie. Dit document is de locatie: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#`|  
|contentVersion|Nee|Hiermee geeft u de versie-definitie. Wanneer u een werkstroom met behulp van de definitie implementeert, kunt u deze waarde om ervoor te zorgen dat de juiste definitie wordt gebruikt.|  
|parameters|Nee|Hiermee geeft u parameters die worden gebruikt voor het invoeren van gegevens in de definitie. Een maximum van 50 parameters kan worden gedefinieerd.|  
|Triggers|Nee|Hiermee geeft u informatie voor de triggers die de werkstroom te starten. Er kan maximaal 10 triggers worden gedefinieerd.|  
|acties|Nee|Geeft de acties die worden uitgevoerd als de stroom wordt uitgevoerd. Maximaal 250 acties kan worden gedefinieerd.|  
|uitvoer|Nee|Hiermee geeft u informatie over de geïmplementeerde resource. Maximaal 10 uitvoer kan worden gedefinieerd.|  
  
## <a name="parameters"></a>Parameters

Deze sectie geeft de parameters die worden gebruikt in de werkstroomdefinitie tijdens de implementatie. Alle parameters moeten worden gedeclareerd in deze sectie voordat ze kunnen worden gebruikt in andere gedeelten van de definitie.  
  
Het volgende voorbeeld ziet u de structuur van de parameterdefinitie van een:  

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": <default-value-of-parameter>,
        "allowedValues": [ <array-of-allowed-values> ],
        "metadata" : { "key": { "name": "value"} }
    }
}
```

|Elementnaam|Vereist|Beschrijving|  
|------------------|--------------|-----------------|  
|type|Ja|**Type**: tekenreeks <p> **Declaratie**:`"parameters": {"parameter1": {"type": "string"}` <p> **Specificatie**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Type**: securestring <p> **Declaratie**:`"parameters": {"parameter1": {"type": "securestring"}}` <p> **Specificatie**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Type**: int <p> **Declaratie**:`"parameters": {"parameter1": {"type": "int"}}` <p> **Specificatie**:`"parameters": {"parameter1": {"value" : 5}}` <p> **Type**: bool <p> **Declaratie**:`"parameters": {"parameter1": {"type": "bool"}}` <p> **Specificatie**:`"parameters": {"parameter1": { "value": true }}` <p> **Type**: matrix <p> **Declaratie**:`"parameters": {"parameter1": {"type": "array"}}` <p> **Specificatie**:`"parameters": {"parameter1": { "value": [ array-of-values ]}}` <p> **Type**: object <p> **Declaratie**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specificatie**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Type**: secureobject <p> **Declaratie**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specificatie**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Opmerking:** de `securestring` en `secureobject` typen worden niet geretourneerd `GET` bewerkingen. Alle wachtwoorden, sleutels en geheimen moeten gebruiken voor dit type.|  
|Standaardwaarde|Nee|Hiermee geeft u de standaardwaarde voor de parameter wanneer er geen waarde is opgegeven op het moment dat de resource is gemaakt.|  
|allowedValues|Nee|Hiermee geeft u een matrix van toegestane waarden voor de parameter.|  
|Metagegevens|Nee|Hiermee geeft u aanvullende informatie over de parameter, zoals een leesbare beschrijving of het moment van ontwerp gegevens die worden gebruikt door Visual Studio of andere hulpprogramma's.|  
  
Dit voorbeeld ziet hoe u een parameter in de sectie hoofdtekst van een actie kunt gebruiken:  
  
```json
"body" :
{
  "property1": "@parameters('parameter1')"
}
```

 Parameters kunnen ook worden gebruikt in de uitvoer.  
  
## <a name="triggers-and-actions"></a>Triggers en acties  

Triggers en acties geeft de oproepen die kunnen deelnemen in workflow worden uitgevoerd. Zie voor meer informatie over deze sectie [werkstroomacties en Triggers](logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>uitvoer  

Uitvoer geven informatie op die kan worden geretourneerd van een werkstroom uitgevoerd. Als u een specifieke status of een waarde die u wilt traceren voor elke run hebt, kunt u die gegevens opnemen in de uitvoer uitvoeren. De gegevens worden weergegeven in de Management REST API voor die run en in de gebruikersinterface voor die uitvoeren in de Azure portal. U kunt ook deze uitvoer naar andere externe systemen zoals Power BI stromen om dashboards te maken. Uitvoer zijn *niet* gebruikt om te reageren op binnenkomende aanvragen op de REST-API-Service. Om te reageren op een binnenkomende aanvraag met behulp van de `response` actietype, Hier volgt een voorbeeld:
  
```json
"outputs": {  
  "key1": {  
    "value": "value1",  
    "type" : "<type-of-value>"  
  }  
} 
```

|Elementnaam|Vereist|Beschrijving|  
|------------------|--------------|-----------------|  
|Key1|Ja|Hiermee geeft u de sleutel-id voor de uitvoer. Vervang **key1** met een naam die u gebruiken wilt voor het identificeren van de uitvoer.|  
|waarde|Ja|Hiermee geeft u de waarde van de uitvoer.|  
|type|Ja|Geeft het type voor de waarde die is opgegeven. Mogelijke zijn waarden: <ul><li>`string`</li><li>`securestring`</li><li>`int`</li><li>`bool`</li><li>`array`</li><li>`object`</li></ul>|
  
## <a name="expressions"></a>Expressies  

JSON-waarden in de definitie letterlijk kunnen zijn kunnen, of ze expressies die worden geëvalueerd wanneer de definitie wordt gebruikt. Bijvoorbeeld:  
  
```json
"name": "value"
```

 of  
  
```json
"name": "@parameters('password') "
```

> [!NOTE]
> Aantal expressies worden de waarden van de runtime-acties die niet aan het begin van de uitvoering bestaat mogelijk. U kunt **functies** bij het ophalen van sommige van deze waarden.  
  
Expressies kunnen overal voorkomen in een string-waarde van JSON en altijd resulteren in een andere JSON-waarde. Wanneer een JSON-waarde is een expressie worden bepaald, de hoofdtekst van de expressie wordt opgehaald door het verwijderen van het apenstaartje (@). Als een letterlijke tekenreeks is vereist dat met begint @, dat tekenreeks moet worden voorafgegaan door@. De volgende voorbeelden ziet hoe expressies worden geëvalueerd.  
  
|JSON-waarde|Resultaat|  
|----------------|------------|  
|'parameters'|De tekens 'parameters' worden geretourneerd.|  
|'[1] parameters'|De tekens 'parameters [1]' worden geretourneerd.|  
|"@@"|Een tekenreeks 1 teken ' @' wordt geretourneerd.|  
|" @"|Een tekenreeks 2 teken ' @' wordt geretourneerd.|  
  
Met *interpolatie string*, expressies kunnen ook worden weergegeven in tekenreeksen waar expressies zijn samengevoegd in `@{ ... }`. Bijvoorbeeld: <p>`"name" : "First Name: @{parameters('firstName')} Last Name: @{parameters('lastName')}"`

Het resultaat is altijd een tekenreeks, waardoor deze functie vergelijkbaar met de `concat` functie. Stel dat u hebt gedefinieerd `myNumber` als `42` en `myString` als `sampleString`:  
  
|JSON-waarde|Resultaat|  
|----------------|------------|  
|"@parameters(myString)"|Retourneert `sampleString` als een tekenreeks.|  
|"@{parameters('myString')}"|Retourneert `sampleString` als een tekenreeks.|  
|"@parameters(myNumber)"|Retourneert `42` als een *nummer*.|  
|"@{parameters('myNumber')}"|Retourneert `42` als een *tekenreeks*.|  
|"Antwoord is: @{parameters('myNumber')}"|Retourneert de tekenreeks `Answer is: 42`.|  
|'@concat(' Antwoord is: ', string(parameters('myNumber'))) '|Retourneert de tekenreeks`Answer is: 42`|  
|"Antwoord: @@ {parameters('myNumber')}"|Retourneert de tekenreeks `Answer is: @{parameters('myNumber')}`.|  
  
## <a name="operators"></a>Operators  

Operatoren zijn de tekens die u in expressies of functies gebruiken kunt. 
  
|Operator|Beschrijving|  
|--------------|-----------------|  
|.|De puntoperator kunt u verwijzen naar eigenschappen van een object|  
|?|De operator vraagteken kunt u verwijzen naar null eigenschappen van een object zonder een runtime-fout. Bijvoorbeeld, kunt u deze expressie voor het afhandelen van null trigger uitvoer: <p>`@coalesce(trigger().outputs?.body?.property1, 'my default value')`|  
|'|Enkel aanhalingsteken is de enige manier om het inpakken van letterlijke tekenreeks. U kunt dubbele aanhalingstekens in expressies niet gebruiken omdat deze leestekens strijdig is met de JSON-aanhalingsteken die de hele expressie terugloopt.|  
|[]|De vierkante haken kan worden gebruikt om een waarde niet ophalen uit een array met een specifieke index. Bijvoorbeeld, als u hebt een actie die wordt doorgegeven `range(0,10)`in voor de `forEach` functioneren, kunt u deze functie gebruiken om items buiten matrices te halen:  <p>`myArray[item()]`|  
  
## <a name="functions"></a>Functies  

U kunt ook functies binnen expressies aanroepen. De volgende tabel ziet u de functies die kunnen worden gebruikt in een expressie.  
  
|expressie|Evaluatie|  
|----------------|----------------|  
|'@function('Hallo') '|Roept het lid van de functie van de definitie van de letterlijke tekenreeks Hallo als eerste parameter.|  
|'@function(' Dit '' s Cool!') '|Roept het lid van de functie van de definitie van de letterlijke tekenreeks ''s Cool!' Als de eerste parameter|  
|'@function.prop1 () '|Retourneert de waarde van de eigenschap eig1 van de `myfunction` lid van de definitie.|  
|'@function('Hallo') .prop1 '|Het lid van de functie van de definitie van de letterlijke tekenreeks 'Hallo' aanroepen als de eerste parameter en retourneert de eigenschap prop1 van het object.|  
|'@function(parameters('Hello')) '|De parameter Hallo geëvalueerd en wordt de waarde doorgegeven aan functie|  
  
### <a name="referencing-functions"></a>Functies die verwijzen naar  

U kunt deze functies gebruiken om te verwijzen naar uitvoer van andere acties in de logische app of de waarden die worden doorgegeven als de logische app is gemaakt. Bijvoorbeeld, u kunt verwijzen naar de gegevens van de ene stap om deze te gebruiken in een andere.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|parameters|Retourneert een parameterwaarde die is gedefinieerd in de definitie. <p>`parameters('password')` <p> **Parameternummer**: 1 <p> **Naam**: Parameter <p> **Beschrijving**: vereist. De naam van de parameter waarvan de gewenste waarden.|  
|Actie|Hiermee kunt een expressie die moet worden afgeleid van de waarde van andere JSON-naam en waarde-paren of de uitvoer van de huidige runtime-actie. De eigenschap die wordt vertegenwoordigd door propertyPath in het volgende voorbeeld is optioneel. Als propertyPath niet is opgegeven, wordt de verwijzing naar het hele action-object is. Deze functie kan alleen worden gebruikt binnen een-totdat de voorwaarden van een actie. <p>`action().outputs.body.propertyPath`|  
|acties|Hiermee kunt een expressie die moet worden afgeleid van de waarde van andere JSON-naam en waarde-paren of de uitvoer van de runtime-actie. Deze expressies declareren expliciet dat één actie afhankelijk van een andere actie is. De eigenschap die wordt vertegenwoordigd door propertyPath in het volgende voorbeeld is optioneel. Als propertyPath niet is opgegeven, wordt de verwijzing naar het hele action-object is. U kunt dit element of het element voorwaarden gebruiken om op te geven, afhankelijkheden, maar u hoeft niet te gebruiken voor dezelfde afhankelijke resource. <p>`actions('myAction').outputs.body.propertyPath` <p> **Parameternummer**: 1 <p> **Naam**: naam van de actie <p> **Beschrijving**: vereist. De naam van de actie waarvan de gewenste waarden. <p> Beschikbare eigenschappen van het actie-object zijn: <ul><li>`name`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Zie de [Rest-API](http://go.microsoft.com/fwlink/p/?LinkID=850646) voor meer informatie over deze eigenschappen.|
|Trigger|Hiermee kunt een expressie die moet worden afgeleid van de waarde van andere JSON-naam en waarde-paren of de uitvoer van de runtime-trigger. De eigenschap die wordt vertegenwoordigd door propertyPath in het volgende voorbeeld is optioneel. Als propertyPath niet is opgegeven, wordt de verwijzing naar het hele triggerobject is. <p>`trigger().outputs.body.propertyPath` <p>Binnen een trigger invoer retourneert, de functie de uitvoer van de vorige uitvoering. Echter, wanneer gebruikt in een triggervoorwaarde de `trigger` functie retourneert de uitvoer van de huidige uitvoering. <p> Beschikbare eigenschappen op het triggerobject zijn: <ul><li>`name`</li><li>`scheduledTime`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Zie de [Rest-API](http://go.microsoft.com/fwlink/p/?LinkID=850644) voor meer informatie over deze eigenschappen.|
|actionOutputs|Deze functie is de afkorting van`actions('actionName').outputs` <p> **Parameternummer**: 1 <p> **Naam**: naam van de actie <p> **Beschrijving**: vereist. De naam van de actie waarvan de gewenste waarden.|  
|actionBody en hoofdtekst|Deze functies zijn steno voor`actions('actionName').outputs.body` <p> **Parameternummer**: 1 <p> **Naam**: naam van de actie <p> **Beschrijving**: vereist. De naam van de actie waarvan de gewenste waarden.|  
|triggerOutputs|Deze functie is de afkorting van`trigger().outputs`|  
|triggerBody|Deze functie is de afkorting van`trigger().outputs.body`|  
|item|Wanneer gebruikt in een terugkerende actie, retourneert deze functie het item dat in de matrix voor deze herhaling van de actie. Bijvoorbeeld, als u een actie die wordt uitgevoerd voor elk item een matrix van berichten hebt, kunt u deze syntaxis: <p>`"input1" : "@item().subject"`| 
  
### <a name="collection-functions"></a>Functies van de verzameling  

Deze functies bepalen het verloop van verzamelingen en Algemeen van toepassing op matrices, tekenreeksen, en soms woordenlijsten.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|Bevat|Retourneert waar als het woordenboek bevat een lijst met sleutels, bevat een waarde of tekenreeks subtekenreeks bevat. Bijvoorbeeld: deze functie retourneert `true`: <p>`contains('abacaba','aca')` <p> **Parameternummer**: 1 <p> **Naam**: binnen de verzameling <p> **Beschrijving**: vereist. De verzameling om te doorzoeken in. <p> **Parameternummer**: 2 <p> **Naam**: Find-object <p> **Beschrijving**: vereist. Het object te vinden binnen de **binnen verzameling**.|  
|lengte|Retourneert het aantal elementen in een matrix of een tekenreeks. Bijvoorbeeld: deze functie retourneert `3`:  <p>`length('abc')` <p> **Parameternummer**: 1 <p> **Naam**: verzameling <p> **Beschrijving**: vereist. De verzameling waarvoor u de lengte ophalen.|  
|leeg|Retourneert waar als het object, matrix of tekenreeks leeg is. Bijvoorbeeld: deze functie retourneert `true`: <p>`empty('')` <p> **Parameternummer**: 1 <p> **Naam**: verzameling <p> **Beschrijving**: vereist. De verzameling om te controleren als deze leeg is.|  
|snijpunt|Retourneert een matrix of object waarvoor standaardelementen tussen matrices of objecten die zijn doorgegeven. Bijvoorbeeld: deze functie retourneert `[1, 2]`: <p>`intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])` <p>De parameters voor de functie kunnen een set van objecten of een set van matrices (niet een combinatie van beide) zijn. Als er twee objecten met dezelfde naam, is het laatste object met deze naam wordt weergegeven in het definitieve-object. <p> **Parameternummer**: 1...*n* <p> **Naam**: verzameling*n* <p> **Beschrijving**: vereist. De verzamelingen om te evalueren. Een object moet zich in alle verzamelingen die zijn doorgegeven aan in het resultaat verschijnen.|  
|Union|Retourneert een matrix of object met de elementen die in de matrix of object aan deze functie doorgegeven zijn. Bijvoorbeeld: deze functie retourneert `[1, 2, 3, 10, 101]`: <p>`union([1, 2, 3], [101, 2, 1, 10])` <p>De parameters voor de functie kunnen een set van objecten of een set van matrices (niet een combinatie daarvan) zijn. Als er twee objecten met dezelfde naam in de uiteindelijke uitvoer, is het laatste object met deze naam wordt weergegeven in het definitieve-object. <p> **Parameternummer**: 1...*n* <p> **Naam**: verzameling*n* <p> **Beschrijving**: vereist. De verzamelingen om te evalueren. Een object dat wordt weergegeven in een van de verzamelingen wordt ook weergegeven in het resultaat.|  
|eerste|Retourneert het eerste element in de matrix of tekenreeks doorgegeven. Bijvoorbeeld: deze functie retourneert `0`: <p>`first([0,2,3])` <p> **Parameternummer**: 1 <p> **Naam**: verzameling <p> **Beschrijving**: vereist. De verzameling te laten worden van het eerste-object.|  
|laatste|Retourneert het laatste element in de matrix of tekenreeks doorgegeven. Bijvoorbeeld: deze functie retourneert `3`: <p>`last('0123')` <p> **Parameternummer**: 1 <p> **Naam**: verzameling <p> **Beschrijving**: vereist. De verzameling te laten worden van het laatste-object.|  
|duren|Retourneert de eerste **aantal** elementen van de matrix of tekenreeks doorgegeven. Bijvoorbeeld: deze functie retourneert `[1, 2]`:  <p>`take([1, 2, 3, 4], 2)` <p> **Parameternummer**: 1 <p> **Naam**: verzameling <p> **Beschrijving**: vereist. De verzameling waar te nemen van de eerste **aantal** objecten. <p> **Parameternummer**: 2 <p> **Naam**: aantal <p> **Beschrijving**: vereist. Het aantal objecten uit de **verzameling**. Moet een positief geheel getal zijn.|  
|Overslaan|Retourneert de elementen in de matrix die begint bij index **aantal**. Bijvoorbeeld: deze functie retourneert `[3, 4]`: <p>`skip([1, 2 ,3 ,4], 2)` <p> **Parameternummer**: 1 <p> **Naam**: verzameling <p> **Beschrijving**: vereist. De verzameling overslaan van de eerste **aantal** objecten uit. <p> **Parameternummer**: 2 <p> **Naam**: aantal <p> **Beschrijving**: vereist. Het aantal objecten verwijderen uit de voorgrond van **verzameling**. Moet een positief geheel getal zijn.|  
|join|Retourneert een tekenreeks die bij elk item van een matrix die is gekoppeld met een scheidingsteken, bijvoorbeeld dit retourneert `"1,2,3,4"`:<br /><br /> `join([1, 2, 3, 4], ',')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling<br /><br /> **Beschrijving**: vereist. De verzameling items uit koppelen.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: scheidingsteken<br /><br /> **Beschrijving**: vereist. De tekenreeks van elkaar te scheiden van items.|  
  
### <a name="string-functions"></a>Tekenreeks-functies

De volgende functies zijn alleen van toepassing op tekenreeksen. U kunt ook sommige functies van de verzameling op tekenreeksen.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|concat|Een combinatie van een willekeurig aantal tekenreeksen samen. Bijvoorbeeld, als parameter 1 `p1`, retourneert deze functie `somevalue-p1-somevalue`: <p>`concat('somevalue-',parameters('parameter1'),'-somevalue')` <p> **Parameternummer**: 1...*n* <p> **Naam**: tekenreeks*n* <p> **Beschrijving**: vereist. De tekenreeksen combineren tot één tekenreeks.|  
|de subtekenreeks|Retourneert een subset van tekens van een tekenreeks. Bijvoorbeeld: deze functie retourneert `abc`: <p>`substring('somevalue-abc-somevalue',10,3)` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks waaruit de subtekenreeks wordt opgehaald. <p> **Parameternummer**: 2 <p> **Naam**: startIndex <p> **Beschrijving**: vereist. De index van waar de subtekenreeks in de parameter 1 begint. <p> **Parameternummer**: 3 <p> **Naam**: lengte <p> **Beschrijving**: vereist. De lengte van de subtekenreeks.|  
|vervangen|Vervangt een tekenreeks door een bepaalde tekenreeks. Bijvoorbeeld: deze functie retourneert `the new string`: <p>`replace('the old string', 'old', 'new')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die wordt doorzocht voor parameter 2 en 3,-parameter bijgewerkt als parameter 2 wordt gevonden in parameter 1. <p> **Parameternummer**: 2 <p> **Naam**: oude tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die moet worden vervangen door de parameter 3, wanneer een overeenkomst is gevonden in de parameter 1 <p> **Parameternummer**: 3 <p> **Naam**: nieuwe tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die wordt gebruikt ter vervanging van de tekenreeks in de parameter 2 wanneer een overeenkomst is gevonden in de parameter 1.|  
|GUID|Deze functie genereert een globally unique identifier (GUID)-tekenreeks. Deze functie kan bijvoorbeeld deze GUID genereren:`c2ecc88d-88c8-4096-912c-d6f2e2b138ce` <p>`guid()` <p> **Parameternummer**: 1 <p> **Naam**: indeling <p> **Beschrijving**: optioneel. Een enkele indelingsopgave die aangeeft [opmaken van de waarde van deze Guid](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). De indelingsparameter is "N", "D", "B", 'P' of 'X'. Als de indeling niet is opgegeven, wordt "D" gebruikt.|  
|toLower|Zet een tekenreeks naar kleine letters. Bijvoorbeeld: deze functie retourneert `two by two is four`: <p>`toLower('Two by Two is Four')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks converteren naar lagere hoofdlettergebruik. Als een teken in de tekenreeks is geen equivalent van een kleine letter, is het teken opgenomen in de geretourneerde tekenreeks ongewijzigd.|  
|toUpper|Zet een tekenreeks naar hoofdletters. Bijvoorbeeld: deze functie retourneert `TWO BY TWO IS FOUR`: <p>`toUpper('Two by Two is Four')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks converteren naar hoofdletters hoofdlettergebruik. Als een teken in de tekenreeks is geen equivalent hoofdletters, is het teken opgenomen in de geretourneerde tekenreeks ongewijzigd.|  
|indexOf|De index van een waarde binnen case tekenreeks insensitively vinden. Bijvoorbeeld: deze functie retourneert `7`: <p>`indexof('hello, world.', 'world')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten. <p> **Parameternummer**: 2 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De waarde zoeken in de index van.|  
|lastIndexOf|De laatste index van een waarde binnen case tekenreeks insensitively vinden. Bijvoorbeeld: deze functie retourneert `3`: <p>`lastindexof('foofoo', 'foo')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten. <p> **Parameternummer**: 2 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De waarde zoeken in de index van.|  
|StartsWith|Controleert of de tekenreeks op de insensitively met een waarde-aanvraag begint. Bijvoorbeeld: deze functie retourneert `true`: <p>`startswith('hello, world', 'hello')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten. <p> **Parameternummer**: 2 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. Met kan de waarde van de tekenreeks beginnen.|  
|EndsWith|Controleert of de tekenreeks op de insensitively op een waarde-aanvraag eindigt. Bijvoorbeeld: deze functie retourneert `true`: <p>`endswith('hello, world', 'world')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten. <p> **Parameternummer**: 2 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De waarde van de tekenreeks kan eindigen op.|  
|split|Splitst de tekenreeks met een scheidingsteken. Bijvoorbeeld: deze functie retourneert `["a", "b", "c"]`: <p>`split('a;b;c',';')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die wordt gesplitst. <p> **Parameternummer**: 2 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. Het scheidingsteken.|  
  
### <a name="logical-functions"></a>Logische functies  

Deze functies zijn nuttig in voorwaarden en kunnen worden gebruikt voor het evalueren van elk type logica.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|is gelijk aan|Retourneert waar als twee waarden gelijk zijn. Bijvoorbeeld, parameter1 someValue wordt deze functie retourneert `true`: <p>`equals(parameters('parameter1'), 'someValue')` <p> **Parameternummer**: 1 <p> **Naam**: 1-Object <p> **Beschrijving**: vereist. Het object te vergelijken met **Object 2**. <p> **Parameternummer**: 2 <p> **Naam**: Object-2 <p> **Beschrijving**: vereist. Het object te vergelijken met **1 Object**.|  
|minder|Retourneert waar als het eerste argument kleiner is dan de tweede. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld: deze functie retourneert `true`: <p>`less(10,100)` <p> **Parameternummer**: 1 <p> **Naam**: 1-Object <p> **Beschrijving**: vereist. Het object om te controleren of het minder dan **Object 2**. <p> **Parameternummer**: 2 <p> **Naam**: Object-2 <p> **Beschrijving**: vereist. Het object wilt controleren of groter zijn dan **1 Object**.|  
|lessOrEquals|Retourneert waar als het eerste argument kleiner dan of gelijk zijn aan de tweede is. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld: deze functie retourneert `true`: <p>`lessOrEquals(10,10)` <p> **Parameternummer**: 1 <p> **Naam**: 1-Object <p> **Beschrijving**: vereist. Het object om te controleren of het kleiner of gelijk aan **Object 2**. <p> **Parameternummer**: 2 <p> **Naam**: Object-2 <p> **Beschrijving**: vereist. Het object om te controleren of deze is groter dan of gelijk zijn aan **1 Object**.|  
|groter|Retourneert waar als het eerste argument groter dan de tweede is. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld: deze functie retourneert `false`:  <p>`greater(10,10)` <p> **Parameternummer**: 1 <p> **Naam**: 1-Object <p> **Beschrijving**: vereist. Het object wilt controleren of groter zijn dan **Object 2**. <p> **Parameternummer**: 2 <p> **Naam**: Object-2 <p> **Beschrijving**: vereist. Het object om te controleren of het minder dan **1 Object**.|  
|greaterOrEquals|Retourneert waar als het eerste argument groter dan of gelijk zijn aan de tweede is. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld: deze functie retourneert `false`: <p>`greaterOrEquals(10,100)` <p> **Parameternummer**: 1 <p> **Naam**: 1-Object <p> **Beschrijving**: vereist. Het object om te controleren of deze is groter dan of gelijk zijn aan **Object 2**. <p> **Parameternummer**: 2 <p> **Naam**: Object-2 <p> **Beschrijving**: vereist. Het object om te controleren of het kleiner dan of gelijk zijn aan **1 Object**.|  
|en|Retourneert waar als beide parameters van toepassing zijn. Beide argumenten moeten Booleaanse waarden. Bijvoorbeeld: deze functie retourneert `false`: <p>`and(greater(1,10),equals(0,0))` <p> **Parameternummer**: 1 <p> **Naam**: Booleaanse 1 <p> **Beschrijving**: vereist. Het eerste argument die moet worden `true`. <p> **Parameternummer**: 2 <p> **Naam**: Booleaanse 2 <p> **Beschrijving**: vereist. Het tweede argument moet `true`.|  
|of|Retourneert waar als de parameter ingesteld op true is. Beide argumenten moeten Booleaanse waarden. Bijvoorbeeld: deze functie retourneert `true`: <p>`or(greater(1,10),equals(0,0))` <p> **Parameternummer**: 1 <p> **Naam**: Booleaanse 1 <p> **Beschrijving**: vereist. Het eerste argument die mogelijk `true`. <p> **Parameternummer**: 2 <p> **Naam**: Booleaanse 2 <p> **Beschrijving**: vereist. Het tweede argument mogelijk `true`.|  
|niet|Retourneert waar als de parameters zijn `false`. Beide argumenten moeten Booleaanse waarden. Bijvoorbeeld: deze functie retourneert `true`: <p>`not(contains('200 Success','Fail'))` <p> **Parameternummer**: 1 <p> **Naam**: Booleaanse <p> **Beschrijving**: retourneert true als de parameters zijn `false`. Beide argumenten moeten Booleaanse waarden. Deze functie retourneert `true`:`not(contains('200 Success','Fail'))`|  
|Als|Retourneert een opgegeven waarde, op basis van of de expressie uitgewezen `true` of `false`.  Bijvoorbeeld: deze functie retourneert `"yes"`: <p>`if(equals(1, 1), 'yes', 'no')` <p> **Parameternummer**: 1 <p> **Naam**: expressie <p> **Beschrijving**: vereist. De expressie moet een Booleaanse waarde die welke waarde bepaalt retourneren. <p> **Parameternummer**: 2 <p> **Naam**: True <p> **Beschrijving**: vereist. De waarde te retourneren als de expressie is `true`. <p> **Parameternummer**: 3 <p> **Naam**: False <p> **Beschrijving**: vereist. De waarde te retourneren als de expressie is `false`.|  
  
### <a name="conversion-functions"></a>Van conversiefuncties  

Deze functies worden gebruikt om te converteren tussen elk van de systeemeigen typen in de taal:  
  
- Tekenreeks  
  
- geheel getal  
  
- Float  
  
- Booleaanse waarde  
  
- Matrices  
  
- woordenlijsten  

-   Formulieren  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|int|De parameter niet converteren naar een geheel getal. Deze functie retourneert bijvoorbeeld 100 als een getal in plaats van een tekenreeks: <p>`int('100')` <p> **Parameternummer**: 1 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een geheel getal.|  
|Tekenreeks|De parameter niet converteren naar een tekenreeks. Bijvoorbeeld: deze functie retourneert `'10'`: <p>`string(10)` <p>U kunt ook een object converteren naar een tekenreeks. Bijvoorbeeld, als de `myPar` parameter is een object met één eigenschap `abc : xyz`, retourneert deze functie `{"abc" : "xyz"}`: <p>`string(parameters('myPar'))` <p> **Parameternummer**: 1 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een tekenreeks.|  
|JSON|De parameter converteren naar een JSON-type-waarde en is het tegenovergestelde van `string()`. Bijvoorbeeld: deze functie retourneert `[1,2,3]` als een matrix in plaats van een tekenreeks: <p>`json('[1,2,3]')` <p>U kunt ook een tekenreeks converteren naar een object. Bijvoorbeeld: deze functie retourneert `{ "abc" : "xyz" }`: <p>`json('{"abc" : "xyz"}')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die wordt geconverteerd naar een systeemeigen type-waarde. <p>De `json()` functie XML te invoer ondersteunt. Bijvoorbeeld, de parameterwaarde van: <p>`<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>` <p>wordt geconverteerd naar deze JSON: <p>`{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|Float|Het parameterargument niet converteren naar een getal met drijvende komma. Bijvoorbeeld: deze functie retourneert `10.333`: <p>`float('10.333')` <p> **Parameternummer**: 1 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een getal met drijvende komma.|  
|BOOL|De parameter niet converteren naar een Booleaanse waarde. Bijvoorbeeld: deze functie retourneert `false`: <p>`bool(0)` <p> **Parameternummer**: 1 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een Booleaanse waarde.|  
|Base64|Retourneert de base64-weergave van de invoertekenreeks. Bijvoorbeeld: deze functie retourneert `c29tZSBzdHJpbmc=`: <p>`base64('some string')` <p> **Parameternummer**: 1 <p> **Naam**: String 1 <p> **Beschrijving**: vereist. De tekenreeks voor het coderen naar base64-weergave.|  
|base64ToBinary|Retourneert een binaire voorstelling van een base64-gecodeerde tekenreeks. Bijvoorbeeld: deze functie retourneert de binaire voorstelling van `some string`: <p>`base64ToBinary('c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De met base64 gecodeerde tekenreeks.|  
|base64ToString|Retourneert een string-weergave van een based64 gecodeerde tekenreeks. Bijvoorbeeld: deze functie retourneert `some string`: <p>`base64ToString('c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De met base64 gecodeerde tekenreeks.|  
|Binaire|Retourneert een binaire voorstelling van een waarde.  Bijvoorbeeld: deze functie retourneert een binaire voorstelling van `some string`: <p>`binary('some string')` <p> **Parameternummer**: 1 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar binair.|  
|dataUriToBinary|Retourneert een binaire voorstelling van een gegevens-URI. Bijvoorbeeld: deze functie retourneert de binaire voorstelling van `some string`: <p>`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De gegevens te converteren naar een binaire voorstelling URI.|  
|dataUriToString|Retourneert een tekenreeksrepresentatie van een gegevens-URI. Bijvoorbeeld: deze functie retourneert `some string`: <p>`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks<p> **Beschrijving**: vereist. De gegevens te converteren naar tekenreeksweergave URI.|  
|dataUri|Retourneert een gegevens-URI van een waarde. Bijvoorbeeld: deze functie retourneert deze gegevens URI `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=`: <p>`dataUri('some string')` <p> **Parameternummer**: 1<p> **Naam**: waarde<p> **Beschrijving**: vereist. De waarde converteren naar gegevens-URI.|  
|decodeBase64|Retourneert een tekenreeksrepresentatie van een invoer based64-tekenreeks. Bijvoorbeeld: deze functie retourneert `some string`: <p>`decodeBase64('c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: een tekenreeksrepresentatie van een invoer based64-tekenreeks geretourneerd.|  
|encodeUriComponent|URL-Hiermee heft u de tekenreeks die doorgegeven. Bijvoorbeeld: deze functie retourneert `You+Are%3ACool%2FAwesome`: <p>`encodeUriComponent('You Are:Cool/Awesome')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks escape-URL onveilige tekens bevatten uit.|  
|decodeUriComponent|Ongedaan maken-URL-Hiermee heft de tekenreeks die doorgegeven. Bijvoorbeeld: deze functie retourneert `You Are:Cool/Awesome`: <p>`encodeUriComponent('You+Are%3ACool%2FAwesome')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks URL onveilige tekens vanaf decoderen.|  
|decodeDataUri|Retourneert een binaire voorstelling van invoergegevens URI-tekenreeks. Bijvoorbeeld: deze functie retourneert de binaire voorstelling van `some string`: <p>`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De dataURI in een binaire voorstelling decoderen.|  
|uriComponent|Retourneert een URI gecodeerde representatie van een waarde. Bijvoorbeeld: deze functie retourneert `You+Are%3ACool%2FAwesome`: <p>`uriComponent('You Are:Cool/Awesome')` <p> **Parameternummer**: 1<p> **Naam**: tekenreeks <p> **Beschrijving**: vereist. De tekenreeks die moet worden gecodeerd URI.|  
|uriComponentToBinary|Retourneert dat een binaire voorstelling van een URI-gecodeerde tekenreeks. Bijvoorbeeld: deze functie retourneert een binaire voorstelling van `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parameternummer**: 1 <p> **Naam**: tekenreeks<p> **Beschrijving**: vereist. De URI-gecodeerde tekenreeks.|  
|uriComponentToString|Retourneert dat een tekenreeksrepresentatie van een URI-gecodeerde tekenreeks. Bijvoorbeeld: deze functie retourneert `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parameternummer**: 1<p> **Naam**: tekenreeks<p> **Beschrijving**: vereist. De URI-gecodeerde tekenreeks.|  
|xml|Een XML-representatie van de waarde retourneren. Bijvoorbeeld: deze functie retourneert XML-inhoud dat wordt vertegenwoordigd door `'\<name>Alan\</name>'`: <p>`xml('\<name>Alan\</name>')` <p>De `xml()` functie biedt ondersteuning voor JSON-object te invoer. Bijvoorbeeld: de parameter `{ "abc": "xyz" }` wordt geconverteerd naar een XML-inhoud:`\<abc>xyz\</abc>` <p> **Parameternummer**: 1<p> **Naam**: waarde<p> **Beschrijving**: vereist. De waarde te converteren naar XML.|  
|matrix|De parameter niet converteren naar een matrix. Bijvoorbeeld: deze functie retourneert `["abc"]`: <p>`array('abc')` <p> **Parameternummer**: 1 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een matrix.|
|createArray|Maakt een matrix van de parameters. Bijvoorbeeld: deze functie retourneert `["a", "c"]`: <p>`createArray('a', 'c')` <p> **Parameternummer**: 1...*n* <p> **Naam**:*n* <p> **Beschrijving**: vereist. De waarden te combineren in een matrix.|
|triggerFormDataValue|Retourneert één waarde die overeenkomt met de naam van de sleutel van gegevens van een formulier of trigger formulier gecodeerde uitvoer.  Als er meerdere overeenkomt met deze fout wordt.  Bijvoorbeeld de volgende retourneert `bar`:`triggerFormDataValue('foo')`<br /><br />**Parameternummer**: 1<br /><br />**Naam**: naam sleutel<br /><br />**Beschrijving**: vereist. De sleutelnaam van de gegevenswaarde formulier om terug te keren.|
|triggerFormDataMultiValues|Retourneert een matrix met waarden die overeenkomen met de naam van de sleutel van gegevens van een formulier of trigger formulier gecodeerde uitvoer.  Bijvoorbeeld de volgende retourneert `["bar"]`:`triggerFormDataValue('foo')`<br /><br />**Parameternummer**: 1<br /><br />**Naam**: naam sleutel<br /><br />**Beschrijving**: vereist. De naam van de sleutel van de waarden van de vorm te retourneren.|
|triggerMultipartBody|Wordt de instantie voor een deel van een meerdelige uitvoer van de trigger.<br /><br />**Parameternummer**: 1<br /><br />**Naam**: Index<br /><br />**Beschrijving**: vereist. De index van het onderdeel om op te halen.|
|formDataValue|Retourneert één waarde die overeenkomt met de naam van de sleutel van gegevens van een formulier of actie formulier gecodeerde uitvoer.  Als er meerdere overeenkomt met deze fout wordt.  Bijvoorbeeld de volgende retourneert `bar`:`formDataValue('someAction', 'foo')`<br /><br />**Parameternummer**: 1<br /><br />**Naam**: naam van de actie<br /><br />**Beschrijving**: vereist. De naam van de actie met het antwoord op een formulier gegevens of formulier-codering.<br /><br />**Parameternummer**: 2<br /><br />**Naam**: naam sleutel<br /><br />**Beschrijving**: vereist. De sleutelnaam van de gegevenswaarde formulier om terug te keren.|
|formDataMultiValues|Retourneert een matrix met waarden die overeenkomen met de naam van de sleutel van gegevens van een formulier of actie formulier gecodeerde uitvoer.  Bijvoorbeeld de volgende retourneert `["bar"]`:`formDataMultiValues('someAction', 'foo')`<br /><br />**Parameternummer**: 1<br /><br />**Naam**: naam van de actie<br /><br />**Beschrijving**: vereist. De naam van de actie met het antwoord op een formulier gegevens of formulier-codering.<br /><br />**Parameternummer**: 2<br /><br />**Naam**: naam sleutel<br /><br />**Beschrijving**: vereist. De naam van de sleutel van de waarden van de vorm te retourneren.|
|multipartBody|Wordt de instantie voor een deel van een meerdelige uitvoer van een actie.<br /><br />**Parameternummer**: 1<br /><br />**Naam**: naam van de actie<br /><br />**Beschrijving**: vereist. De naam van de actie met een meerdelige antwoord.<br /><br />**Parameternummer**: 2<br /><br />**Naam**: Index<br /><br />**Beschrijving**: vereist. De index van het onderdeel om op te halen.|

### <a name="manipulation-functions"></a>Functies voor het bewerken
 
Deze functies zijn van toepassing op XML en objecten.
 
|Functienaam|Beschrijving|  
|-------------------|-----------------| 
|Coalesce|Retourneert het eerste niet-null-object in de doorgegeven argumenten. **Opmerking**: een lege tekenreeks niet null is. Bijvoorbeeld, als parameters 1 en 2 niet zijn gedefinieerd, deze functie retourneert `fallback`:  <p>`coalesce(parameters('parameter1'), parameters('parameter2') ,'fallback')` <p> **Parameternummer**: 1...*n* <p> **Naam**: Object*n* <p> **Beschrijving**: vereist. De objecten om te controleren op null.|
|addProperty|Retourneert een object met een extra eigenschap. Als de eigenschap al tijdens runtime die een fout wordt gegenereerd bestaat. Deze functie retourneert bijvoorbeeld het object `{ "abc" : "xyz", "def": "uvw" }`: <p>`addProperty(json('{"abc" : "xyz"}'), 'def', 'uvw')` <p> **Parameternummer**: 1 <p> **Naam**: Object <p> **Beschrijving**: vereist. Het object dat een nieuwe eigenschap toevoegen aan. <p> **Parameternummer**: 2 <p> **Naam**: de naam van eigenschap <p> **Beschrijving**: vereist. De naam van de nieuwe eigenschap. <p> **Parameternummer**: 3 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde toewijzen aan de nieuwe eigenschap.|
|setProperty|Retourneert een object met een extra eigenschap of een bestaande eigenschap ingesteld op de opgegeven waarde. Deze functie retourneert bijvoorbeeld het object `{ "abc" : "uvw" }`: <p>`setProperty(json('{"abc" : "xyz"}'), 'abc', 'uvw')` <p> **Parameternummer**: 1 <p> **Naam**: Object <p> **Beschrijving**: vereist. Het object waarop de eigenschap in te stellen.<p> **Parameternummer**: 2 <p> **Naam**: de naam van eigenschap<p> **Beschrijving**: vereist. De naam van de nieuwe of bestaande eigenschap. <p> **Parameternummer**: 3 <p> **Naam**: waarde <p> **Beschrijving**: vereist. De waarde toewijzen aan de eigenschap.|
|removeProperty|Retourneert een object met een eigenschap die is verwijderd. Als de eigenschap te verwijderen, niet bestaat wordt het oorspronkelijke object geretourneerd. Deze functie retourneert bijvoorbeeld het object `{ "abc" : "xyz" }`: <p>`removeProperty(json('{"abc" : "xyz", "def": "uvw"}'), 'def')` <p> **Parameternummer**: 1 <p> **Naam**: Object <p> **Beschrijving**: vereist. Het object te verwijderen van de eigenschap uit.<p> **Parameternummer**: 2 <p> **Naam**: de naam van eigenschap <p> **Beschrijving**: vereist. De naam van de eigenschap te verwijderen. <p>|
|XPath|Retourneert een matrix van die overeenkomt met de xpath-expressie van een waarde die de xpath-expressie wordt geëvalueerd als XML-knooppunt. <p> **Voorbeeld 1** <p>Stel de waarde van parameter `p1` een tekenreeksrepresentatie van deze XML is: <p>`<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>` <p>Deze code:`xpath(xml(parameters('p1')), '/lab/robot/name')` <p>retourneert <p>`[ <name>R1</name>, <name>R2</name> ]` <p>tijdens deze code: <p>`xpath(xml(parameters('p1')), ' sum(/lab/robot/parts)')` <p>retourneert <p>`13` <p> <p> **Voorbeeld 2** <p>De volgende XML-inhoud opgegeven: <p>`<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>` <p>Deze code:`@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')` <p>of deze code: <p>`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')` <p>retourneert <p>`<Location xmlns="http://abc.com">xyz</Location>` <p>En deze code:`@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')` <p>retourneert <p>``xyz`` <p> **Parameternummer**: 1 <p> **Naam**: Xml <p> **Beschrijving**: vereist. De XML op waarvoor de XPath-expressie wordt geëvalueerd. <p> **Parameternummer**: 2 <p> **Naam**: XPath <p> **Beschrijving**: vereist. De XPath-expressie om te evalueren.|

### <a name="math-functions"></a>Rekenkundige functies  

Deze functies kunnen worden gebruikt voor beide typen van de getallen: **gehele getallen** en **zwevend**.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|toevoegen|Retourneert het resultaat van het toevoegen van de twee getallen. Bijvoorbeeld: deze functie retourneert `20.333`: <p>`add(10,10.333)` <p> **Parameternummer**: 1 <p> **Naam**: Summand 1 <p> **Beschrijving**: vereist. Het aantal toevoegen aan **Summand 2**. <p> **Parameternummer**: 2 <p> **Naam**: Summand 2 <p> **Beschrijving**: vereist. Het aantal toevoegen aan **Summand 1**.|  
|Sub|Retourneert het resultaat van de twee getallen af te trekken. Bijvoorbeeld: deze functie retourneert `-0.333`: <p>`sub(10,10.333)` <p> **Parameternummer**: 1 <p> **Naam**: Minuend <p> **Beschrijving**: vereist. Het getal dat **Subtrahend** wordt verwijderd uit. <p> **Parameternummer**: 2 <p> **Naam**: Subtrahend <p> **Beschrijving**: vereist. Het getal te verwijderen uit de **Minuend**.|  
|mul|Retourneert het resultaat van de twee getallen vermenigvuldigen. Bijvoorbeeld: deze functie retourneert `103.33`: <p>`mul(10,10.333)` <p> **Parameternummer**: 1 <p> **Naam**: Multiplicand 1 <p> **Beschrijving**: vereist. Het getal wilt vermenigvuldigen **Multiplicand 2** met. <p> **Parameternummer**: 2 <p> **Naam**: Multiplicand 2 <p> **Beschrijving**: vereist. Het getal wilt vermenigvuldigen **Multiplicand 1** met.|  
|div|Retourneert het resultaat van de deling van de twee getallen. Bijvoorbeeld: deze functie retourneert `1.0333`: <p>`div(10.333,10)` <p> **Parameternummer**: 1 <p> **Naam**: deeltal <p> **Beschrijving**: vereist. Het nummer te delen door het **deler**. <p> **Parameternummer**: 2 <p> **Naam**: deler <p> **Beschrijving**: vereist. Het aantal te verdelen de **deeltal** door.|  
|Mod|Retourneert het restgetal deling van de twee getallen (modulo). Bijvoorbeeld: deze functie retourneert `2`: <p>`mod(10,4)` <p> **Parameternummer**: 1 <p> **Naam**: deeltal <p> **Beschrijving**: vereist. Het nummer te delen door het **deler**. <p> **Parameternummer**: 2 <p> **Naam**: deler <p> **Beschrijving**: vereist. Het aantal te verdelen de **deeltal** door. Na de deling worden wordt de rest genomen.|  
|min.|Er zijn twee verschillende patronen voor het aanroepen van deze functie. <p>Hier `min` duurt een matrix en de functie retourneert `0`: <p>`min([0,1,2])` <p>U kunt ook deze functie kan duren voordat een door komma's gescheiden lijst met waarden en ook retourneert `0`: <p>`min(0,1,2)` <p> **Opmerking**: alle waarden moeten getallen, dus als de parameter een matrix is, de matrix zijn alleen getallen hebben. <p> **Parameternummer**: 1 <p> **Naam**: verzameling of -waarde <p> **Beschrijving**: vereist. Ofwel een matrix met waarden naar de minimale waarde of de eerste waarde van een set. <p> **Parameternummer**: 2...*n* <p> **Naam**: waarde*n* <p> **Beschrijving**: optioneel. Als de eerste parameter een waarde is, kunt u aanvullende waarden doorgeven en het minimum van alle doorgegeven waarden wordt geretourneerd.|  
|Maximum aantal|Er zijn twee verschillende patronen voor het aanroepen van deze functie. <p>Hier `max` duurt een matrix en de functie retourneert `2`: <p>`max([0,1,2])` <p>U kunt ook deze functie kan duren voordat een door komma's gescheiden lijst met waarden en ook retourneert `2`: <p>`max(0,1,2)` <p> **Opmerking**: alle waarden moeten getallen, dus als de parameter een matrix is, de matrix zijn alleen getallen hebben. <p> **Parameternummer**: 1 <p> **Naam**: verzameling of -waarde <p> **Beschrijving**: vereist. Ofwel een matrix met waarden naar de maximale waarde of de eerste waarde van een set. <p> **Parameternummer**: 2...*n* <p> **Naam**: waarde*n* <p> **Beschrijving**: optioneel. Als de eerste parameter een waarde is, kunt u aanvullende waarden doorgeven en het maximum van alle doorgegeven waarden wordt geretourneerd.|  
|bereik|Genereert een matrix van gehele getallen vanaf een bepaald aantal. U definieert de lengte van de geretourneerde matrix. <p>Bijvoorbeeld: deze functie retourneert `[3,4,5,6]`: <p>`range(3,4)` <p> **Parameternummer**: 1 <p> **Naam**: startIndex <p> **Beschrijving**: vereist. De eerste geheel getal in de matrix. <p> **Parameternummer**: 2 <p> **Naam**: aantal <p> **Beschrijving**: vereist. Deze waarde is het aantal getallen in de matrix.|  
|rand|Genereert een willekeurige geheel getal zijn binnen het opgegeven bereik (inclusief alleen op de eerste end). Deze functie kunt bijvoorbeeld een `0` of '1': <p>`rand(0,2)` <p> **Parameternummer**: 1 <p> **Naam**: minimale <p> **Beschrijving**: vereist. De laagste geheel getal dat kan worden geretourneerd. <p> **Parameternummer**: 2 <p> **Naam**: maximale <p> **Beschrijving**: vereist. Deze waarde is het volgende gehele getal na het hoogste gehele getal dat kan worden geretourneerd.|  
 
### <a name="date-functions"></a>Datumfuncties  

|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|utcnow|Retourneert de huidige tijdstempel een tekenreeks, bijvoorbeeld: `2017-03-15T13:27:36Z`: <p>`utcnow()` <p> **Parameternummer**: 1 <p> **Naam**: indeling <p> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addSeconds|Een geheel getal aantal seconden toegevoegd aan de tijdstempel van een tekenreeks doorgegeven. Het aantal seconden mag positief of negatief zijn. Standaard is het resultaat een tekenreeks in de ISO 8601-notatie ("o"), tenzij een indelingsopgave is opgegeven. Bijvoorbeeld: `2015-03-15T13:27:00Z`: <p>`addseconds('2015-03-15T13:27:36Z', -36)` <p> **Parameternummer**: 1 <p> **Naam**: Timestamp <p> **Beschrijving**: vereist. Een tekenreeks waarin de tijd. <p> **Parameternummer**: 2 <p> **Naam**: seconden <p> **Beschrijving**: vereist. Het aantal seconden om toe te voegen. Kan niet negatief moet worden afgetrokken seconden zijn. <p> **Parameternummer**: 3 <p> **Naam**: indeling <p> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addminutes|Een geheel getal aantal minuten toegevoegd aan de tijdstempel van een tekenreeks doorgegeven. Het aantal minuten kan positief of negatief zijn. Standaard is het resultaat een tekenreeks in de ISO 8601-notatie ("o"), tenzij een indelingsopgave is opgegeven. Bijvoorbeeld: `2015-03-15T14:00:36Z`: <p>`addminutes('2015-03-15T13:27:36Z', 33)` <p> **Parameternummer**: 1 <p> **Naam**: Timestamp <p> **Beschrijving**: vereist. Een tekenreeks waarin de tijd. <p> **Parameternummer**: 2 <p> **Naam**: minuten <p> **Beschrijving**: vereist. Het aantal minuten om toe te voegen. Kan niet negatief moet worden afgetrokken minuten zijn. <p> **Parameternummer**: 3 <p> **Naam**: indeling <p> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addhours|Voegt een geheel getal van uur aan de tijdstempel van een tekenreeks doorgegeven. Het aantal uren kan positief of negatief zijn. Standaard is het resultaat een tekenreeks in de ISO 8601-notatie ("o"), tenzij een indelingsopgave is opgegeven. Bijvoorbeeld: `2015-03-16T01:27:36Z`: <p>`addhours('2015-03-15T13:27:36Z', 12)` <p> **Parameternummer**: 1 <p> **Naam**: Timestamp <p> **Beschrijving**: vereist. Een tekenreeks waarin de tijd. <p> **Parameternummer**: 2 <p> **Naam**: uren <p> **Beschrijving**: vereist. Het aantal uren om toe te voegen. Kan moet worden afgetrokken uren negatief zijn. <p> **Parameternummer**: 3 <p> **Naam**: indeling <p> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addDays|Een geheel getal aantal dagen toegevoegd aan de tijdstempel van een tekenreeks doorgegeven. Het aantal dagen kan positief of negatief zijn. Standaard is het resultaat een tekenreeks in de ISO 8601-notatie ("o"), tenzij een indelingsopgave is opgegeven. Bijvoorbeeld: `2015-02-23T13:27:36Z`: <p>`adddays('2015-03-15T13:27:36Z', -2)` <p> **Parameternummer**: 1 <p> **Naam**: Timestamp <p> **Beschrijving**: vereist. Een tekenreeks waarin de tijd. <p> **Parameternummer**: 2 <p> **Naam**: dagen <p> **Beschrijving**: vereist. Het aantal dagen om toe te voegen. Kan niet negatief moet worden afgetrokken dagen zijn. <p> **Parameternummer**: 3 <p> **Naam**: indeling <p> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|formatDateTime|Retourneert een tekenreeks in datumnotatie. Standaard is het resultaat een tekenreeks in de ISO 8601-notatie ("o"), tenzij een indelingsopgave is opgegeven. Bijvoorbeeld: `2015-02-23T13:27:36Z`: <p>`formatDateTime('2015-03-15T13:27:36Z', 'o')` <p> **Parameternummer**: 1 <p> **Naam**: datum <p> **Beschrijving**: vereist. Een tekenreeks met de datum. <p> **Parameternummer**: 2 <p> **Naam**: indeling <p> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|startOfHour|Retourneert het begin van het uur aan de tijdstempel van een tekenreeks doorgegeven. Bijvoorbeeld `2017-03-15T13:00:00Z`:<br /><br /> `startOfHour('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Dit is een tekenreeks waarin de tijd.<br /><br />**Parameternummer**: 2<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|startOfDay|Retourneert het begin van de dag en een tijdstempel voor een tekenreeks doorgegeven. Bijvoorbeeld `2017-03-15T00:00:00Z`:<br /><br /> `startOfDay('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Dit is een tekenreeks waarin de tijd.<br /><br />**Parameternummer**: 2<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.| 
|startOfMonth|Retourneert het begin van de maand en een tijdstempel voor een tekenreeks doorgegeven. Bijvoorbeeld `2017-03-01T00:00:00Z`:<br /><br /> `startOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Dit is een tekenreeks waarin de tijd.<br /><br />**Parameternummer**: 2<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.| 
|DayOfWeek|Retourneert de dag van het onderdeel van de week van een tijdstempel voor een tekenreeks.  Zondag is ingesteld op 0, maandag 1, enzovoort. Bijvoorbeeld `3`:<br /><br /> `dayOfWeek('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Dit is een tekenreeks waarin de tijd.| 
|DayOfMonth|Retourneert de dag van het onderdeel van de maand van een tijdstempel voor een tekenreeks. Bijvoorbeeld `15`:<br /><br /> `dayOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Dit is een tekenreeks waarin de tijd.| 
|DayOfYear|Retourneert de dag van het jaar onderdeel van een tijdstempel voor een tekenreeks. Bijvoorbeeld `74`:<br /><br /> `dayOfYear('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Dit is een tekenreeks waarin de tijd.| 
|ticks|Retourneert de maatstreepjes eigenschap van een tijdstempel voor een tekenreeks. Bijvoorbeeld `1489603019`:<br /><br /> `ticks('2017-03-15T18:36:59Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Dit is een tekenreeks waarin de tijd.| 
  
### <a name="workflow-functions"></a>Werkstroom-functies  

Deze functies vindt u informatie over de werkstroom zichzelf tijdens runtime.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|listCallbackUrl|Retourneert een tekenreeks aan te roepen om aan te roepen de trigger of actie. <p> **Opmerking**: deze functie kan alleen worden gebruikt een **httpWebhook** en **apiConnectionWebhook**, niet in een **handmatige**, **terugkeerpatroon**, **http**, of **apiConnection**. <p>Bijvoorbeeld, de `listCallbackUrl()` retourneert werken: <p>`https://prod-01.westus.logic.azure.com:443/workflows/1235...ABCD/triggers/manual/run?api-version=2015-08-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=xxx...xxx` |  
|werkstroom|Deze functie biedt de details voor de werkstroom zichzelf tijdens de runtime. <p> Beschikbare eigenschappen van het werkstroomobject zijn: <ul><li>`name`</li><li>`type`</li><li>`id`</li><li>`location`</li><li>`run`</li></ul> <p> De waarde van de `run` eigenschap is een object met de volgende eigenschappen: <ul><li>`name`</li><li>`type`</li><li>`id`</li></ul> <p>Zie de [Rest-API](http://go.microsoft.com/fwlink/p/?LinkID=525617) voor meer informatie over deze eigenschappen.<p> Bijvoorbeeld, als u de naam van de huidige sessie, gebruikt de `"@workflow().run.name"` expressie. |

## <a name="next-steps"></a>Volgende stappen

[Werkstroomacties en -triggers](logic-apps-workflow-actions-triggers.md)
