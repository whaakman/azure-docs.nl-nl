---
title: Verwerken van grote berichten in Azure Logic Apps | Microsoft Docs
description: Meer informatie over het verwerken van grote berichtgrootten met opdelen in logic apps
services: logic-apps
documentationcenter: ''
author: shae-hurst
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.devlang: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.date: 4/27/2018
ms.author: shhurst
ms.openlocfilehash: a99fbdd7c9beb32f640d5ca623f8bcda3cb9aba4
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="handle-large-messages-with-chunking-in-logic-apps"></a>Verwerken van grote berichten met opdelen in Logic Apps

Bij het verwerken van berichten, beperkt Logic Apps berichtinhoud tot een maximale grootte. Deze limiet helpt overhead verminderen door opslaan en verwerken van grote berichten worden gemaakt. Voor het afhandelen van berichten die groter zijn dan deze limiet Logic Apps kunt *chunk* een groot bericht in kleinere berichten. Op die manier kunt u nog steeds grote bestanden met behulp van Logic Apps onder bepaalde omstandigheden overbrengen. Om te communiceren met andere services die via connectors of HTTP-, Logic Apps grote berichten kunt gebruiken, maar *alleen* in stukken verdeeld. Deze voorwaarde betekent connectors moeten bieden ook ondersteuning voor verdeling in segmenten of de verdeling in segmenten van de onderliggende HTTP-bericht uitwisseling tussen Logic Apps en deze services moet gebruiken.

Dit artikel laat zien hoe u ondersteuning voor berichten die groter zijn dan de limiet voor verdeling in segmenten kunt instellen.

## <a name="what-makes-messages-large"></a>Wat berichten "large" maakt?

Berichten zijn 'groot' op basis van de service die deze berichten worden verwerkt. De exacte maximumgrootte op grote berichten verschilt voor elk voor Logic Apps en connectors. Grote berichten die moeten worden chunked kunnen niet rechtstreeks in beslag nemen zowel Logic Apps en connectors. Zie voor de maximale grootte van Logic Apps bericht [Logic Apps en configuratie](../logic-apps/logic-apps-limits-and-config.md).
Zie voor de maximale berichtgrootte van elke connector, de [specifieke technische details van connector](../connectors/apis-list.md).

### <a name="chunked-message-handling-for-logic-apps"></a>Gesegmenteerde bericht verwerking voor Logic Apps

Logic Apps niet rechtstreeks gebruiken voor uitvoer van gesegmenteerde berichten die groter dan de maximale grootte zijn. Alleen de acties die ondersteuning bieden voor verdeling in segmenten hebben toegang tot de inhoud van het bericht in deze uitvoer. Dus een actie die verantwoordelijk is voor grote berichten moet voldoen aan *beide* deze criteria:

* Systeemeigen ondersteuning voor verdeling in segmenten wanneer de actie die bij een connector hoort. 
* Hebben de verdeling in segmenten-ondersteuning is ingeschakeld in de runtime-configuratie van de actie. 

U krijgt anders een runtime-fout wanneer u probeert te krijgen tot grote inhoud uitvoer. Zie inschakelen verdeling in segmenten [instellen verdeling in segmenten ondersteuning](#set-up-chunking).

### <a name="chunked-message-handling-for-connectors"></a>Gesegmenteerde bericht verwerking voor connectors

Services die met Logic Apps communiceren kunnen hun eigen limieten voor de berichtgrootte hebben. Deze limieten zijn vaak kleiner is dan de limiet van Logic Apps. Bijvoorbeeld, ervan uitgaande dat een connector ondersteunt de verdeling in segmenten, overwegen een connector een bericht 30 MB groot is, maar niet door Logic Apps. Om te voldoen aan de limiet voor deze connector splitst Logic Apps elk bericht groter zijn dan 30 MB in kleinere reeksen.

De onderliggende chunking-protocol is voor connectors die ondersteuning bieden voor verdeling in segmenten, onzichtbaar voor eindgebruikers. Niet alle connectors ondersteunen echter verdeling in segmenten, zodat deze connectors runtime-fouten genereren wanneer de binnenkomende berichten overschrijden de maximale grootte is de connectors.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>Instellen van de verdeling in segmenten via HTTP

In de algemene HTTP-scenario's, grote downloaden van inhoud kunt opsplitsen en uploadt via HTTP, zodat uw logische app en een eindpunt kunnen grote berichten uitwisselen. U moet echter van berichten in de manier waarop Logic Apps verwacht Segmentselectie. 

Als een eindpunt heeft ingeschakeld verdeling in segmenten voor downloads of uploads, van de HTTP-acties in uw logische app automatisch Segmentselectie grote berichten. Anders moet u instellen verdeling in segmenten ondersteuning op het eindpunt. Als u niet de eigenaar of bepalen van het eindpunt of de connector, hebt u mogelijk niet de optie voor het instellen van de verdeling in segmenten.

Ook als een HTTP-bewerking niet al voor verdeling in segmenten inschakelt, moet u ook instellen opdelen in de actie `runTimeConfiguration` eigenschap. U kunt deze eigenschap in de actie kunt instellen, rechtstreeks in de weergave-editor zoals verderop of in Logic Apps Designer zoals hier wordt beschreven:

1. Kies in de rechterbovenhoek van de HTTP-actie, knop met het weglatingsteken (**...** ), en kies vervolgens **instellingen**.

   ![Open het menu instellingen op de actie](./media/logic-apps-handle-large-messages/http-settings.png)

2. Onder **inhoud overdragen**stelt **toestaan verdeling in segmenten** naar **op**.

   ![Verdeling in segmenten inschakelen](./media/logic-apps-handle-large-messages/set-up-chunking.png)

3. Doorgaan met de volgende secties om door te gaan verdeling in segmenten voor downloads of uploads instellen.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>Inhoud downloaden in segmenten

Veel eindpunten verzenden automatisch grote berichten in stukken verdeeld wanneer gedownload via een HTTP GET-aanvraag. Gesegmenteerde om berichten te downloaden van een eindpunt via HTTP, het eindpunt moet ondersteuning bieden voor gedeeltelijke inhoud aanvragen, of *chunked downloads*. Wanneer uw logische app een HTTP GET-aanvraag naar een eindpunt verzendt voor het downloaden van inhoud en het eindpunt met een '206' statuscode reageert, bevat het antwoord gesegmenteerde inhoud. Logic Apps kan niet bepalen of een eindpunt ondersteuning biedt voor gedeeltelijke aanvragen. Echter, wanneer uw logische app de eerste '206'-antwoord ontvangt, uw logische app verzendt automatisch meerdere aanvragen om de inhoud te downloaden.

Als u wilt controleren of een eindpunt kunt ondersteuning voor gedeeltelijke inhoud, een HEAD-aanvraag te verzenden. Deze aanvraag kunt u bepalen of het antwoord bevat de `Accept-Ranges` header. Op die manier als het eindpunt gesegmenteerde downloads ondersteunt, maar niet gesegmenteerde inhoud verzenden, kunt u *voorstellen* deze optie door in te stellen de `Range` -header in HTTP GET-aanvraag. 

Deze stappen beschrijven de gedetailleerde procedure die Logic Apps gebruikt voor het downloaden van gedeelde inhoud van een eindpunt naar uw logische app:

1. Uw logische app verzendt een HTTP GET-aanvraag naar het eindpunt.

   De aanvraagheader kan desgewenst een `Range` veld die een bytebereik beschrijft voor het aanvragen van inhoud segmenten.

2. Het eindpunt reageert met de '206' statuscode en de hoofdtekst van een HTTP-bericht.

    Meer informatie over de inhoud van dit segment weergegeven in het antwoord `Content-Range` header, met inbegrip van informatie die Logic Apps helpt bepalen de begin- en einddatum voor het segment, plus de totale grootte van de volledige inhoud voordat de verdeling in segmenten.

3. Uw logische app verzendt automatisch vervolgzelfstudie HTTP GET-aanvragen.

    Uw logische app verzendt vervolgzelfstudie GET-aanvragen totdat de hele inhoud is opgehaald.

Deze actie definitie ziet u bijvoorbeeld een HTTP GET-aanvraag die bepaalt de `Range` header. De header *wordt voorgesteld* dat het eindpunt met reageren moet chunked inhoud:

```json
"getAction": {
    "inputs": {
        "headers": {
            "Range": "bytes=0-1023"
        },
       "method": "GET",
       "uri": "http://myAPIendpoint/api/downloadContent"
    },
    "runAfter": {},
    "type": "Http"
}
```

De GET-aanvraag in de header 'Range' wordt ingesteld op ' bytes = 0-1023 ', die het bereik van bytes is. Als het eindpunt aanvragen voor gedeeltelijke inhoud ondersteunt, wordt de status van het eindpunt reageert met een deel van de inhoud van het aangevraagde bereik. Op basis van het eindpunt, kan de precieze indeling voor het veld 'Bereik'-header verschillen.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>Inhoud in segmenten uploaden

Als u wilt uploaden gesegmenteerde inhoud van een HTTP-bewerking, de actie moet ondersteuning hebt ingeschakeld chunking via de actie `runtimeConfiguration` eigenschap. Deze instelling kan de actie voor het starten van de chunking-protocol. Uw logische app kan vervolgens een initiële POST of PUT-bericht verzenden naar het doel-eindpunt. Nadat de eindpunt met een voorgestelde chunkgrootte reageert, volgt uw logische app om door te sturen PATCH voor HTTP-aanvragen die de inhoud segmenten bevatten.

Deze stappen beschrijven de gedetailleerde procedure die voor het uploaden van gedeelde inhoud van uw logische app naar een eindpunt maakt gebruik van Logic Apps:

1. Uw logische app verzendt een initiële HTTP POST of PUT-aanvraag met een lege berichttekst. De aanvraagheader omvat deze informatie over de inhoud die uw logische app wil in segmenten uploaden:

   | Logische Apps aanvragen header-veld | Waarde | Type | Beschrijving |
   |---------------------------------|-------|------|-------------|
   | **x-ms-transfer-modus** | Chunked | Reeks | Hiermee wordt aangegeven dat de inhoud wordt geüpload in segmenten |
   | **x-ms-content-length** | <*lengte van inhoud*> | Geheel getal | De volledige inhoud grootte in bytes vóór de verdeling in segmenten |
   ||||

2. Het eindpunt reageert met "200" geslaagd-statuscode en deze optionele informatie:

   | Eindpunt antwoord header-veld | Type | Vereist | Beschrijving |
   |--------------------------------|------|----------|-------------|
   | **x-ms--chunkgrootte** | Geheel getal | Nee | De voorgestelde chunkgrootte in bytes |
   | **Locatie** | Reeks | Nee | De URL-locatie waar de PATCH voor HTTP-berichten worden verzonden |
   ||||

3. Uw logische app maakt en vervolgzelfstudie PATCH voor HTTP-berichten - elk met deze informatie verzendt:

   * Een deel van de inhoud op basis van **x-ms--chunkgrootte** of sommige intern berekende grootte totdat alle de inhoud Samentelling **x-ms-content-length** sequentieel is geüpload

   * Deze header details over de inhoud chunk in elk PATCH-bericht verzonden:

     | Logische Apps aanvragen header-veld | Waarde | Type | Beschrijving |
     |---------------------------------|-------|------|-------------|
     | **Content-Range** | <*bereik*> | Reeks | Het bereik aan bytes voor de huidige inhoud chunk, met inbegrip van de beginwaarde, einddatum van de waarde en de totale grootte van inhoud, bijvoorbeeld: ' bytes = 0-1023/10100 ' |
     | **Content-Type** | <*type inhoud*> | Reeks | Het type gesegmenteerde inhoud |
     | **Content-Length** | <*lengte van inhoud*> | Reeks | De lengte van de grootte in bytes van het huidige segment |
     |||||

4. Na elke PATCH-aanvraag wordt het eindpunt de ontvangst van elk segment bevestigd door met de statuscode "200" te reageren.

Deze actie definitie ziet u bijvoorbeeld een HTTP POST-aanvraag voor het uploaden van gedeelde inhoud naar een eindpunt. In de actie `runTimeConfiguration` eigenschap, de `contentTransfer` eigenschappensets `transferMode` naar `chunked`:

```json
"postAction": {
    "runtimeConfiguration": {
        "contentTransfer": {
            "transferMode": "chunked"
        }
    },
    "inputs": {
        "method": "POST",
        "uri": "http://myAPIendpoint/api/action",
        "body": "@body('getAction')"
    },
    "runAfter": {
    "getAction": ["Succeeded"]
    },
    "type": "Http"
}
```