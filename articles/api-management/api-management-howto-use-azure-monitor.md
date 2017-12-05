---
title: Monitor gepubliceerd API's in Azure API Management | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor informatie over het bewaken van uw API in Azure API Management.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: bdca9d4968e9e68314f350787907f15e417821f7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="monitor-published-apis"></a>Gepubliceerde API's bewaken

Monitor voor Azure is een Azure-service met één bron voor de bewaking van alle Azure-resources. Met Azure-Monitor kunt u visualiseren, query, routeren, archiveren en acties uitvoeren op de metrische gegevens en de logboeken die afkomstig zijn van de Azure-bronnen zoals API Management. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Activiteitenlogboeken bekijken
> * Diagnostische logboeken bekijken
> * Metrische gegevens weergeven van uw API 
> * Een waarschuwingsregel instellen wanneer uw API aanroepen met niet-geautoriseerde opgehaald

De volgende video toont het bewaken van API Management met behulp van Azure-Monitor. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Vereisten

+ Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Ook de volgende zelfstudie te voltooien: [importeren en publiceren van uw eerste API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Activiteit-logboeken bekijken

Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw API Management-services. Met activiteitenlogboeken, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die op uw API Management-services worden uitgevoerd. 

> [!NOTE]
> Activiteitenlogboeken bevatten geen leesbewerkingen (GET) of bewerkingen uitgevoerd in de klassieke Portal van de uitgever of met de oorspronkelijke Management-API's.

U kunt toegang krijgen tot activiteitenlogboeken in uw API Management-service of toegang tot de logboeken van alle Azure-resources in Azure-Monitor. 

Activiteitenlogboeken weergeven:

1. Van uw **API Management** exemplaar, klikt u op **activiteitenlogboek**.

## <a name="view-diagnostic-logs"></a>Diagnostische logboeken bekijken

Logboeken met diagnostische gegevens bevatten uitgebreide informatie over bewerkingen en fouten die belangrijk zijn voor controle, evenals het oplossen van problemen. Diagnostische logboeken verschillen van activiteitenlogboeken. Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure-resources. Diagnostische logboeken bieden inzicht in bewerkingen dat de bron zelf uitgevoerd.

Toegang tot diagnoselogboeken:

1. Van uw **API Management** exemplaar, klikt u op **diagnostische logboeken**.

## <a name="view-metrics-of-your-apis"></a>Metrische gegevens weergeven van uw API 's

API Management verzendt metrische gegevens elke minuut, zodat u bijna real-time inzicht in de status en gezondheid van uw API's. Hier volgt een samenvatting van een deel van de beschikbare metrische gegevens:

* Capaciteit (preview): helpt u beslissingen te nemen over uw services APIM upgraden/downgraden. De metriek per minuut is verzonden en reflecteert de gateway-capaciteit op het moment van reporting. De metriek bereik van 0-100 en wordt berekend op basis van de gateway recourses zoals CPU-en geheugengebruik.
* Totaal aantal verzoeken van de Gateway: het aantal API-aanvragen in de periode. 
* Geslaagde aanvragen voor de Gateway: het aantal API-aanvragen dat geslaagde HTTP-antwoordcodes waaronder 304, 307 en iets kleiner is dan 301 (bijvoorbeeld 200) ontvangen. 
* Mislukte aanvragen van de Gateway: het aantal API-aanvragen dat foutieve HTTP-antwoordcodes waaronder 400 en iets groter zijn dan 500 ontvangen.
* Niet-geautoriseerde Gateway verzoeken: het aantal API-aanvragen die HTTP-antwoordcodes waaronder 401, 403 en 429 ontvangen. 
* Andere Gateway verzoeken: het aantal API-aanvragen die HTTP-antwoordcodes die niet bij een van de voorgaande categorieën (bijvoorbeeld 418 horen) ontvangen.

Toegang krijgen tot metrische gegevens:

1. Selecteer **metrische gegevens** in het menu aan de onderkant van de pagina.
2. Selecteer in de vervolgkeuzelijst metrische gegevens die u geïnteresseerd bent in (u kunt meerdere metrische gegevens toevoegen). 
    
    Selecteer bijvoorbeeld **totaal aantal aanvragen dat Gateway** en **mislukte aanvragen voor Gateway** uit de lijst met beschikbare metrische gegevens.
3. Het diagram toont het totale aantal API-aanroepen. Ook ziet u het aantal API-aanroepen die is mislukt. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Een waarschuwingsregel voor niet-geautoriseerde aanvraag instellen

U kunt configureren voor het ontvangen van meldingen op basis van de logboeken van metrische gegevens en de activiteit. Azure Monitor kunt u de volgende handelingen uit als er wordt een waarschuwing configureren:

* E-mailmelding verzenden
* een webhook aanroepen
* Een Azure Logic App aanroepen

Waarschuwingen configureren:

1. Selecteer **waarschuwing regels** in het menu aan de onderkant van de pagina.
2. Selecteer **metrische waarschuwing toevoegen**.
3. Voer een **naam** voor deze waarschuwing.
4. Selecteer **ongeautoriseerde Gateway aanvragen** als de metrische gegevens om te controleren.
5. Selecteer **e-eigenaren, bijdragers en lezers**.
6. Druk op **OK**.
7. Probeer onze API conferentie zonder een API-sleutel aan te roepen. Als de eigenaar van deze API Management-service ontvangt u een e-mailmelding. 

    > [!TIP]
    > De waarschuwingsregel kan een haakje Web of Azure Logic App ook aanroepen, wanneer deze wordt geactiveerd.

    ![installeren van waarschuwing](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Activiteitenlogboeken bekijken
> * Diagnostische logboeken bekijken
> * Metrische gegevens weergeven van uw API 
> * Een waarschuwingsregel instellen wanneer uw API aanroepen met niet-geautoriseerde opgehaald

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Traceringsaanroepen](api-management-howto-api-inspector.md)