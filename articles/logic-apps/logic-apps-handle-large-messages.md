---
title: Verwerken van grote berichten - Azure Logic Apps | Microsoft Docs
description: Meer informatie over het verwerken van grote berichtgrootten met logische groepen te verdelen in Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: shae-hurst
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.devlang: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.date: 4/27/2018
ms.author: shhurst
ms.openlocfilehash: 5aa5ea2a39a0fb9f969e965fed14063522197cda
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085776"
---
# <a name="handle-large-messages-with-chunking-in-azure-logic-apps"></a>Grote berichten afhandelen met logische groepen te verdelen in Azure Logic Apps

Bij het verwerken van berichten, beperkt Logic Apps berichtinhoud tot een maximale grootte. Deze limiet helpt overhead te verminderen die zijn gemaakt door opslaan en verwerken van grote berichten. Voor het afhandelen van berichten die groter zijn dan deze limiet, Logic Apps kunt *chunk* grote berichten in kleinere berichten. Op die manier kunt u nog steeds grote bestanden met behulp van Logic Apps onder bepaalde omstandigheden overbrengen. Bij het communiceren met andere services via connectors of HTTP, grote berichten kunnen worden gebruikt door Logic Apps, maar *alleen* in segmenten. Deze voorwaarde betekent connectors moeten ook ondersteuning voor het logische groepen te verdelen, of de logische groepen te verdelen van de onderliggende HTTP-berichtuitwisseling tussen Logic Apps en deze services moet gebruiken.

In dit artikel wordt beschreven hoe u logische groepen te verdelen voor afhandeling van berichten die groter zijn dan de limiet voor acties kunt instellen. Triggers voor logische App bieden geen ondersteuning voor logische groepen te verdelen vanwege de verhoogde overhead van meerdere berichten uitwisselen. 

## <a name="what-makes-messages-large"></a>Wat kunt u berichten 'grote'?

Berichten zijn "grote" op basis van de service die berichten worden verwerkt. De exacte groottelimiet voor grote berichten wijkt af in Logic Apps en connectors. Grote berichten, die moeten worden chunked kunnen niet rechtstreeks gebruikmaken van zowel Logic Apps en connectors. Zie voor de Logic Apps-berichtlimiet [Logic Apps-limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md).
Zie voor de maximale berichtgrootte van elke connector, de [specifieke technische details van de connector](../connectors/apis-list.md).

### <a name="chunked-message-handling-for-logic-apps"></a>Gesegmenteerde bericht verwerken voor logische Apps

Logic Apps niet rechtstreeks uitvoer van gesegmenteerde berichten die groter dan de maximale berichtgrootte zijn gebruiken. Alleen de acties die ondersteuning bieden voor logische groepen te verdelen, hebben toegang tot de inhoud van het bericht in deze uitvoer. Dus een actie die verantwoordelijk is voor grote berichten moet voldoen aan *beide* deze criteria voldoen:

* Systeemeigen ondersteuning bieden voor logische groepen te verdelen bij die actie bij een connector hoort. 
* Zijn logische groepen te verdelen-ondersteuning is ingeschakeld in de runtime-configuratie van de actie. 

Anders krijgt u een runtime-fout wanneer u probeert te krijgen tot grote inhoud uitvoer. Inschakelen van logische groepen te verdelen [instellen van logische groepen te verdelen ondersteuning](#set-up-chunking).

### <a name="chunked-message-handling-for-connectors"></a>Gesegmenteerde bericht verwerking voor connectors

Services die met Logic Apps communiceren kunnen hun eigen limieten voor de berichtgrootte hebben. Deze limieten zijn vaak kleiner is dan de limiet voor Logic Apps. Bijvoorbeeld, ervan uitgaande dat een connector biedt ondersteuning voor logische groepen te verdelen, overwegen een connector om een bericht 30 MB groot is, maar niet voor Logic Apps. Om te voldoen aan de limiet van deze connector, Logic Apps Hiermee wordt een bericht groter is dan 30 MB in kleinere chunks.

De onderliggende chunking-protocol is voor connectors die ondersteuning bieden voor logische groepen te verdelen, onzichtbaar voor eindgebruikers. Echter niet alle connectors bieden ondersteuning voor logische groepen te verdelen, zodat deze connectors runtime-fouten genereren wanneer de binnenkomende berichten overschrijden de maximale grootte is de connectors.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>Instellen van logische groepen te verdelen via HTTP

In algemene HTTP-scenario's kunt splitsen up grote downloaden van inhoud en uploadt via HTTP, zodat uw logische app en een eindpunt grote berichten kunnen uitwisselen. U moet echter berichten in de manier waarop de Logic Apps wordt verwacht dat wordt verdeeld. 

Als een eindpunt is ingeschakeld voor downloads of uploads logische groepen te verdelen, wordt de HTTP-acties in uw logische app automatisch verdeeld grote berichten. Anders moet u instellen-ondersteuning voor het eindpunt van de logische groepen te verdelen. Als u geen eigenaar bent of het eindpunt of de connector bepalen, hebt u mogelijk niet de optie voor het instellen van logische groepen te verdelen.

Ook als een HTTP-actie niet al logische groepen te verdelen inschakelt, moet u ook instellen van logische groepen te verdelen in van de actie `runTimeConfiguration` eigenschap. U kunt deze eigenschap in de actie kunt instellen, hetzij rechtstreeks in de weergave-editor zoals later wordt beschreven, hetzij in de Logic Apps Designer zoals hier wordt beschreven:

1. Kies in de rechterbovenhoek van de HTTP-actie, de knop met het weglatingsteken (**...** ), en kies vervolgens **instellingen**.

   ![Open het instellingenmenu op de actie.](./media/logic-apps-handle-large-messages/http-settings.png)

2. Onder **inhoud overdragen**, stel **toestaan logische groepen te verdelen** naar **op**.

   ![Schakel op het logische groepen te verdelen](./media/logic-apps-handle-large-messages/set-up-chunking.png)

3. Als u wilt doorgaan met het instellen van logische groepen te verdelen voor downloaden of uploaden, Ga door met de volgende secties.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>Downloaden van inhoud in segmenten

Grote berichten verzenden veel eindpunten automatisch in segmenten wanneer gedownload via een HTTP GET-aanvraag. Gesegmenteerde om berichten te downloaden van een eindpunt via HTTP, het eindpunt van de gedeeltelijke inhoud aanvragen, moet ondersteunen of *gesegmenteerde overdrachtscodering downloads*. Als uw logische app een HTTP GET-aanvraag naar een eindpunt stuurt voor het downloaden van inhoud en het eindpunt met een statuscode '206 reageert', betekent dit dat het antwoord gesegmenteerde inhoud bevat. Logic Apps kan niet bepalen of een eindpunt gedeeltelijke aanvragen ondersteunt. Echter, wanneer uw logische app de eerste '206'-antwoord ontvangt, verzendt uw logische app automatisch meerdere aanvragen om alle inhoud te downloaden.

Als u wilt controleren of een eindpunt kunt ondersteuning voor gedeeltelijke inhoud, een HEAD-aanvraag te verzenden. Deze aanvraag kunt u bepalen of het antwoord bevat de `Accept-Ranges` header. Op die manier als het eindpunt biedt ondersteuning voor gesegmenteerde downloads, maar geen gesegmenteerde inhoud verzendt, kunt u *voorstellen* deze optie door in te stellen de `Range` -header in uw HTTP GET-aanvraag. 

Deze stappen beschrijven de gedetailleerde procedure die voor het downloaden van gedeelde inhoud van een eindpunt aan uw logische app maakt gebruik van Logic Apps:

1. Uw logische app verzendt een HTTP GET-aanvraag naar het eindpunt.

   De aanvraagheader kunt desgewenst een `Range` veld waarin een bereik in bytes voor het aanvragen van inhoud segmenten wordt beschreven.

2. Het eindpunt reageert met de statuscode '206' en de hoofdtekst van een HTTP-bericht.

    Meer informatie over de inhoud in dit segment worden weergegeven in het antwoord van `Content-Range` header, met inbegrip van informatie die Logic Apps helpt het begin en einde van het segment is aangekondigd, plus de totale grootte van de volledige inhoud bepalen voordat u logische groepen te verdelen.

3. Uw logische app verzendt automatisch follow-up HTTP GET-aanvragen.

    Totdat de volledige inhoud wordt opgehaald, verzendt uw logische app follow-up GET-aanvragen.

Deze definitie actie ziet u bijvoorbeeld een HTTP GET-aanvraag die Hiermee stelt u de `Range` header. De header *stelt* dat het eindpunt met reageren moet gesegmenteerde overdrachtscodering inhoud:

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

De GET-aanvraag wordt de 'Range'-header ingesteld op ' bytes = 0-1023 ', is het bereik van bytes. Als het eindpunt aanvragen voor gedeeltelijke inhoud ondersteunt, wordt de status van het eindpunt reageert met een segment van de inhoud van het gevraagde bereik. Op basis van het eindpunt, kan de precieze indeling voor het veld 'Bereik'-header afwijken.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>Inhoud in segmenten uploaden

Als u wilt uploaden gesegmenteerde inhoud van een HTTP-actie, de actie moet zijn ingeschakeld chunking ondersteuning door middel van de actie `runtimeConfiguration` eigenschap. Deze instelling kan de actie start het chunking-protocol. Uw logische app kan vervolgens een initiële post- of PUT-bericht verzenden naar het doel-eindpunt. Nadat het eindpunt met een voorgestelde chunkgrootte reageert, follow-up uw logische app door te sturen vullen van de HTTP-aanvragen die de inhoud segmenten bevatten.

Deze stappen beschrijven de gedetailleerde procedure die voor het uploaden van gesegmenteerde inhoud van uw logische app naar een eindpunt maakt gebruik van Logic Apps:

1. Uw logische app verzendt een eerste HTTP POST of PUT-aanvraag met een lege berichttekst. De aanvraagheader, bevat deze informatie over de inhoud die uw logische app wil uploaden in segmenten:

   | Logic Apps vragen header-veld | Waarde | Type | Beschrijving |
   |---------------------------------|-------|------|-------------|
   | **x-ms-overdracht-modus** | gesegmenteerde overdrachtscodering | Reeks | Geeft aan dat de inhoud in segmenten is geüpload |
   | **x-ms-content-length** | <*lengte van inhoud*> | Geheel getal | De volledige inhoud grootte in bytes vóór logische groepen te verdelen |
   ||||

2. Het eindpunt reageert met de statuscode voor '200' geslaagd en deze optionele informatie:

   | Veld voor eindpunt response-header | Type | Vereist | Beschrijving |
   |--------------------------------|------|----------|-------------|
   | **x-ms-segment-grootte** | Geheel getal | Nee | De voorgestelde chunkgrootte in bytes |
   | **Locatie** | Reeks | Nee | De URL-locatie waar u het vullen van de HTTP-berichten verzenden |
   ||||

3. Uw logische app maakt en follow-up vullen van de HTTP-berichten - allemaal zijn voorzien van deze gegevens verzendt:

   * Een segment van de inhoud op basis van **x-ms--chunkgrootte** of sommige intern berekende grootte totdat alle de inhoud Samentelling **x-ms-content-length** sequentieel is geüpload

   * Deze header-informatie over de inhoud chunk in elk PATCH-bericht verzonden:

     | Logic Apps vragen header-veld | Waarde | Type | Beschrijving |
     |---------------------------------|-------|------|-------------|
     | **Content-Range** | <*Bereik*> | Reeks | Het bereik in bytes voor de huidige inhoud chunk, met inbegrip van de beginwaarde, de einddatum van de waarde en de totale grootte van inhoud, bijvoorbeeld: ' bytes = 0-1023/10100 " |
     | **Content-Type** | <*inhoudstype*> | Reeks | Het type gesegmenteerde inhoud |
     | **Content-Length** | <*lengte van inhoud*> | Reeks | De lengte van de grootte in bytes van het huidige segment |
     |||||

4. Na elke PATCH-aanvraag bevestigt het eindpunt de ontvangst van elk segment door met de statuscode '200' te reageren.

Deze definitie actie ziet u bijvoorbeeld een HTTP POST-aanvraag voor het gesegmenteerde inhoud uploaden naar een eindpunt. In de actie `runTimeConfiguration` eigenschap, de `contentTransfer` eigenschappensets `transferMode` naar `chunked`:

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