---
title: Verwerken van de typen inhoud - Azure Logic Apps | Microsoft Docs
description: Hoe Azure Logic Apps omgaat met inhoudstypen in de ontwerp- en runtime
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: ac67838344bbd10384299c086ff096fbe5dec6a9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="handle-content-types-in-logic-apps"></a>De inhoudstypen die ingang in logic apps

Veel verschillende typen inhoud kunnen door een logische app, met inbegrip van de JSON, XML, platte bestanden en binaire gegevens stromen. Terwijl de Logic Apps-Engine alle typen inhoud ondersteunt, worden sommige systeemeigen begrepen door de Engine voor Logic Apps. Anderen mogelijk casten of conversies indien nodig. In dit artikel wordt beschreven hoe de engine voor verschillende typen inhoud verwerkt en hoe deze typen indien nodig de juiste manier verwerkt.

## <a name="content-type-header"></a>Content-Type-Header

Om te starten in feite, bekijken we de twee `Content-Types` waarvoor geen conversie of casten die u in een logische app gebruiken kunt: `application/json` en `text/plain`.

## <a name="applicationjson"></a>Application/JSON

De werkstroom-engine is afhankelijk van de `Content-Type` kop van HTTP-aanroepen om te bepalen van de juiste verwerking. Een aanvraag met het type inhoud `application/json` wordt opgeslagen en verwerkt als een JSON-Object. JSON-inhoud kan ook standaard zonder eventuele casten worden geparseerd. 

U kan bijvoorbeeld een aanvraag waarvoor de header inhoudstype parseren `application/json ` in een werkstroom met behulp van een expressie zoals `@body('myAction')['foo'][0]` om de waarde `bar` in dit geval:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

Er is geen aanvullende casten nodig. Als u met gegevens die JSON is, maar niet een header die is opgegeven werkt, u kunt handmatig dit casten naar JSON met behulp van de `@json()` functioneren, bijvoorbeeld: `@json(triggerBody())['foo']`.

### <a name="schema-and-schema-generator"></a>Schema- en schema-generator

De aanvraag-trigger kunt u het invoeren van een JSON-schema voor de nettolading die u verwacht te ontvangen. Dit schema kunt de tokens designer genereren zodat u de inhoud van de aanvraag kunt gebruiken. Als u een schema gereed geen hebt, selecteert u **gebruik voorbeeld nettolading voor het genereren van schema**, zodat u een JSON-schema van een voorbeeld-nettolading genereren kunt.

![Schema](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a>Parseren van JSON-actie

De `Parse JSON` actie kunt u JSON-inhoud in beschrijvende tokens voor logic app consumptie parseren. Deze actie is vergelijkbaar met de aanvraag-trigger, kunt u opgeven of een JSON-schema voor de inhoud die u wilt parseren configureren. Dit hulpprogramma maakt in beslag neemt gegevens van Service Bus, Azure Cosmos DB enzovoort eenvoudiger.

![Parseren van JSON](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a>text/plain

Net als bij `application/json`, HTTP-berichten ontvangen met de `Content-Type` koptekst van `text/plain` in onbewerkte vorm worden opgeslagen. Ook als deze berichten zijn opgenomen in de volgende acties zonder casten, deze aanvragen gaat met `Content-Type`: `text/plain` header. Bijvoorbeeld, als u werkt met een plat bestand, mogelijk, krijgt u dit HTTP inhoud als `text/plain`:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

Als u in de volgende actie als de hoofdtekst van een andere aanvraag de aanvraag verzendt (`@body('flatfile')`), de aanvraag heeft een `text/plain` header Content-Type. Als u met gegevens die als tekst zonder opmaak, maar niet hebben een koptekst opgegeven werkt, kunt u handmatig de gegevens voor het gebruik van tekst geconverteerd de `@string()` functioneren, bijvoorbeeld: `@string(triggerBody())`.

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml en toepassing/octet-stream en converter functies

De Engine voor Logic Apps behoudt altijd de `Content-Type` die op de HTTP-aanvraag of antwoord is ontvangen. Als de engine ontvangt inhoud met de `Content-Type` van `application/octet-stream`, en dat de inhoud van een verdere actie zonder casten, de uitgaande aanvraag heeft opneemt `Content-Type`: `application/octet-stream`. Op deze manier de engine kan worden gegarandeerd dat geen gegevens verloren gaan tijdens het verplaatsen van via de werkstroom. De actiestatus van de (invoer en uitvoer) wordt echter opgeslagen in een JSON-object als de status van de door de workflow wordt verplaatst. Dus om bepaalde gegevenstypen behouden, de engine de inhoud heeft geconverteerd naar een binaire base64-gecodeerde tekenreeks met de juiste metagegevens bestandsservergegevens beide `$content` en `$content-type`, die automatisch worden geconverteerd. 

* `@json()`-gegevens naar cast`application/json`
* `@xml()`-gegevens naar cast`application/xml`
* `@binary()`-gegevens naar cast`application/octet-stream`
* `@string()`-gegevens naar cast`text/plain`
* `@base64()`-inhoud converteert naar een base64-tekenreeks
* `@base64toString()`-een base64-gecodeerde tekenreeks converteren naar`text/plain`
* `@base64toBinary()`-een base64-gecodeerde tekenreeks converteren naar`application/octet-stream`
* `@encodeDataUri()`-een tekenreeks als een bytematrix dataUri codeert
* `@decodeDataUri()`-een dataUri decodeert naar een bytematrix

Bijvoorbeeld, als u een HTTP-aanvraag met ontvangen `Content-Type`: `application/xml`:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

U kunt cast-conversie en later gebruiken met ongeveer `@xml(triggerBody())`, of in een functie als `@xpath(xml(triggerBody()), '/CustomerName')`.

## <a name="other-content-types"></a>Andere typen inhoud

Andere typen inhoud worden ondersteund en werken met logic apps, maar mogelijk handmatig bij het ophalen van de berichttekst door het decoderen van de `$content`. Stel bijvoorbeeld dat u activeert een `application/x-www-url-formencoded` aanvragen waar `$content` is de nettolading gecodeerd als een base64-tekenreeks om alle gegevens te behouden:

```
CustomerName=Frank&Address=123+Avenue
```

Omdat de aanvraag niet tekst zonder opmaak of JSON, wordt de aanvraag opgeslagen in de actie als volgt:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Op dit moment is er een systeemeigen functie voor formuliergegevens niet, zodat u deze gegevens nog steeds kan gebruiken in een werkstroom door handmatig de toegang tot de gegevens met een functie, zoals `@string(body('formdataAction'))`. Als u de uitgaande aanvraag ook de `application/x-www-url-formencoded` inhoudstype-koptekst, zou u de aanvraag aan de hoofdtekst actie zonder eventuele casten zoals `@body('formdataAction')`. Maar deze methode werkt alleen als de instantie is de enige parameter in de `body` invoer. Als u probeert te gebruiken `@body('formdataAction')` in een `application/json` vragen, u een runtime-fout niet ophalen omdat de gecodeerde hoofdcode wordt verzonden.

