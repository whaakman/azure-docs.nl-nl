---
title: In Azure API Management gepubliceerde API's bewaken | Microsoft Docs
description: Volg de stappen in deze zelfstudie voor informatie over het bewaken van uw API in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 658588b29e65c9b1cd2f9d82c1c4528929875b2f
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="monitor-published-apis"></a>Gepubliceerde API's bewaken

Met Azure Monitor kunt u visualiseren, query's uitvoeren, routeren, archiveren en actie ondernemen op basis van de metrische gegevens en logboeken van resources in Azure.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Activiteitenlogboeken bekijken
> * Diagnostische logboeken weergeven
> * Metrische gegevens van uw API weergeven 
> * Een waarschuwingsregel instellen als uw API niet-gemachtigde aanroepen krijgt

In de volgende video kunt u zien hoe u API Management kunt bewaken met Azure Monitor. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Vereisten

+ Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Voltooi tevens de volgende zelfstudie: [Uw eerste API importeren en publiceren](import-and-publish.md).

## <a name="view-metrics-of-your-apis"></a>Metrische gegevens van uw API's weergeven

API Management geeft elke minuut metrische gegevens vrij, waardoor u in vrijwel realtime inzicht hebt in de status van uw API's. Er volgt nu een overzicht van enkele beschikbare metrische gegevens:

* Capaciteit (preview): hiermee kunt u beslissingen nemen over het uitvoeren van een up- of downgrade van uw APIM-services. Dit gegeven komt elke minuut beschikbaar en is een weerspiegeling van de capaciteit van de gateway ten tijde van de export. Het gegevensbereik loopt van 0 tot 100 en wordt berekend op basis van gateway-resources als CPU- en geheugengebruik.
* Totaal aantal gateway aanvragen: het aantal API-aanvragen per periode. 
* Geslaagde gateway-aanvragen: het aantal API-aanvragen die HTTP-responscodes met goed gevolg hebben ontvangen, waaronder 304, 307 en codes kleiner dan 301 (bijvoorbeeld 200).
* Mislukte gateway-aanvragen: het aantal API-aanvragen die HTTP-responscodes bij aanvraagfouten hebben ontvangen, waaronder 400 en codes groter dan 500.
* Niet-gemachtigde gateway-aanvragen: het aantal API-aanvragen die HTTP-responscodes hebben ontvangen, waaronder de nummers 401,403 en 429.
* Overige gateway-aanvragen: het aantal API-aanvragen die HTTP-responscodes hebben ontvangen die niet in een van de bovenstaande categorieën vallen (bijvoorbeeld 418).

Metrische gegevens openen:

1. Selecteer **Metrische gegevens** in het menu onder aan de pagina.
2. Selecteer in de vervolgkeuzelijst de gewenste metrische gegevens (u kunt meerdere gegevens toevoegen). 

    Selecteer bijvoorbeeld **Totaal aantal gateway-aanvragen** en **Mislukte gateway-aanvragen** in de lijst met beschikbare metrische gegevens.
3. De grafiek toont het totale aantal API-aanroepen. Ook het aantal mislukte API-aanroepen wordt getoond. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Een waarschuwingsregel instellen voor een niet-gemachtigde aanvraag

U kunt instellen dat u waarschuwingen ontvangt op basis van metrische gegevens en activiteitenlogboeken. Met Azure Monitor kunt u een waarschuwing zodanig configureren dat deze bij activering het volgende doet:

* Een e-mailmelding verzenden
* Een webhook aanroepen
* Een logische Azure-app aanroepen

Waarschuwingen configureren:

1. Selecteer **Waarschuwingsregels** in de menubalk onder aan de pagina.
2. Selecteer **Waarschuwing voor metrische gegevens toevoegen**.
3. Voer een **Naam** voor deze waarschuwing in.
4. Selecteer **Niet-gemachtigde gateway-aanvragen** als het te controleren metrische gegeven.
5. Selecteer **E-maileigenaren, bijdragers en lezers**.
6. Druk op **OK**.
7. Probeer de conferentie-API aan te roepen zonder API-sleutel. Als eigenaar van deze API Management-service ontvangt u een e-mailmelding. 

    > [!TIP]
    > De waarschuwingsregel kan bij activering tevens een webhook of een logische Azure-app aanroepen.

    ![waarschuwing-instellingen](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="activity-logs"></a>Activiteitenlogboeken

Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd voor uw API Management-services. Met activiteitenlogboeken kunt u het 'wat, wie en wanneer' bepalen voor schrijfbewerkingen (PUT, POST, DELETE) die voor uw API Management-services worden uitgevoerd.

> [!NOTE]
> Activiteitenlogboeken bevatten geen lees-bewerkingen (GET) of bewerkingen die zijn uitgevoerd in Azure Portal of met behulp van de oorspronkelijke beheer-API's.

U kunt activiteitenlogboeken in uw API Management-service openen. U kunt alle logboeken van al uw Azure-resources in Azure Monitor openen. 

Activiteitenlogboeken weergeven:

1. Selecteer uw exemplaar van de APIM-service.
2. Klik op **Activiteitenlogboek**.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

Diagnoselogboeken bieden uitgebreide informatie over bewerkingen en fouten die belangrijk zijn voor zowel controles als het oplossen van problemen. Diagnoselogboeken verschillen van activiteitenlogboeken. Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure-resources. Diagnoselogboeken bieden inzicht in bewerkingen die door de resources zelf zijn uitgevoerd.

Diagnostische logboeken configureren:

1. Selecteer uw exemplaar van de APIM-service.
2. Klik op **Diagnoselogboek**.
3. Klik op **Diagnostische gegevens inschakelen**. U kunt diagnostische logboeken samen met metrische gegevens naar een opslagaccount archiveren, ze naar een Event Hub streamen of ze naar Log Analytics verzenden. 

API Management biedt momenteel diagnostische logboeken (ingedeeld in batches per uur) over afzonderlijke API-aanvragen waarbij elk item het volgende schema heeft:

```json
{  
    "isRequestSuccess" : "",
    "time": "",
    "operationName": "",
    "category": "",
    "durationMs": ,
    "callerIpAddress": "",
    "correlationId": "",
    "location": "",
    "httpStatusCodeCategory": "",
    "resourceId": "",
    "properties": {   
        "method": "", 
        "url": "", 
        "clientProtocol": "", 
        "responseCode": , 
        "backendMethod": "", 
        "backendUrl": "", 
        "backendResponseCode": ,
        "backendProtocol": "",  
        "requestSize": , 
        "responseSize": , 
        "cache": "", 
        "cacheTime": "", 
        "backendTime": , 
        "clientTime": , 
        "apiId": "",
        "operationId": "", 
        "productId": "", 
        "userId": "", 
        "apimSubscriptionId": "", 
        "backendId": "",
        "lastError": { 
            "elapsed" : "", 
            "source" : "", 
            "scope" : "", 
            "section" : "" ,
            "reason" : "", 
            "message" : ""
        } 
    }      
}  
```

| Eigenschap  | Type | Beschrijving |
| ------------- | ------------- | ------------- |
| isRequestSuccess | booleaans | Waar als de HTTP-aanvraag is voltooid met een antwoordstatuscode binnen 2xx tot 3xx |
| tijd | date-time | Tijdstempel van ontvangst van de HTTP-aanvraag door de gateway |
| operationName | tekenreeks | Constante waarde 'Microsoft.ApiManagement/GatewayLogs' |
| category | tekenreeks | Constante waarde 'GatewayLogs' |
| durationMs | geheel getal | Aantal milliseconden vanaf het moment dat de gateway de aanvraag ontving tot het moment dat het antwoord volledig werd verzonden |
| callerIpAddress | tekenreeks | IP-adres van de onmiddellijke gatewayaanroeper (kan een tussenschakel zijn) |
| correlationId | tekenreeks | Unieke HTTP-aanvraag-ID die is toegewezen door API Management |
| location | tekenreeks | Naam van de Azure-regio waar de gateway die de aanvraag heeft verwerkt zicht bevond |
| httpStatusCodeCategory | tekenreeks | Categorie van de HTTP-antwoordstatuscode: geslaagd (301 of minder of 304 of 307), niet geautoriseerd (401, 403, 429), fout (400, tussen 500 en 600), anders |
| resourceId | tekenreeks | ID van de API Management-resource /SUBSCRIPTIONS/<subscription>/RESOURCEGROUPS/<resource-group>/PROVIDERS/MICROSOFT.APIMANAGEMENT/SERVICE/<name> |
| properties | object | Eigenschappen van de huidige aanvraag |
| method | tekenreeks | HTTP-methode van de inkomende aanvraag |
| url | tekenreeks | URL van de binnenkomende aanvraag |
| clientProtocol | tekenreeks | HTTP-protocolversie van de inkomende aanvraag |
| responseCode | geheel getal | Statuscode van het HTTP-antwoord dat is verzonden naar een client |
| backendMethod | tekenreeks | HTTP-methode van de aanvraag die is verzonden naar een back-end |
| backendUrl | tekenreeks | URL van de aanvraag die is verzonden naar een back-end |
| backendResponseCode | geheel getal | Code van het HTTP-antwoord dat is ontvangen van een back-end |
| backendProtocol | tekenreeks | HTTP-protocolversie van de aanvraag die is verzonden naar een back-end | 
| requestSize | geheel getal | Aantal bytes dat van een client is ontvangen tijdens de verwerking van aanvragen | 
| responseSize | geheel getal | Aantal bytes dat naar een client is verzonden tijdens de verwerking van aanvragen | 
| cache | tekenreeks | Status van de betrokkenheid van de API Management-cache bij het verwerken van aanvragen (dat wil zeggen, treffer, misser, geen) | 
| cacheTime | geheel getal | Aantal milliseconden dat in totaal is besteed aan IO van de API Management-cache (verbinding maken, bytes verzenden en ontvangen) | 
| backendTime | geheel getal | Aantal milliseconden dat in totaal is besteed aan IO van de back-end (verbinding maken, bytes verzenden en ontvangen) | 
| clientTime | geheel getal | Aantal milliseconden dat in totaal is besteed aan IO van de client (verbinding maken, bytes verzenden en ontvangen) | 
| apiId | tekenreeks | ID van de API-entiteit voor de huidige aanvraag | 
| operationId | tekenreeks | ID van de bewerkingsentiteit voor de huidige aanvraag | 
| productId | tekenreeks | ID van de productentiteit voor de huidige aanvraag | 
| userId | tekenreeks | ID van de gebruikersentiteit voor de huidige aanvraag | 
| apimSubscriptionId | tekenreeks | ID van de abonnemententiteit voor de huidige aanvraag | 
| backendId | tekenreeks | ID van de back-end-entiteit voor de huidige aanvraag | 
| LastError | object | Laatste fout bij de aanvraagverwerking | 
| elapsed | geheel getal | Het aantal milliseconden tussen het moment dat de gateway de aanvraag ontving tot het moment dat de fout optrad | 
| source | tekenreeks | Naam van de interne handler voor beleid of verwerking die de fout heeft veroorzaakt | 
| scope | tekenreeks | Bereik van het beleidsdocument met het beleid dat de fout heeft veroorzaakt | 
| section | tekenreeks | Gedeelte van het beleidsdocument met het beleid dat de fout heeft veroorzaakt | 
| reason | tekenreeks | Reden voor de fout | 
| message | tekenreeks | Foutbericht | 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Activiteitenlogboeken bekijken
> * Diagnostische logboeken weergeven
> * Metrische gegevens van uw API weergeven
> * Een waarschuwingsregel instellen als uw API niet-gemachtigde aanroepen krijgt

Ga door naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Oproepen traceren](api-management-howto-api-inspector.md)
