---
title: In Azure API Management gepubliceerde API's bewaken | Microsoft Docs
description: Volg de stappen in deze zelfstudie voor informatie over het bewaken van uw API in Azure API Management.
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
ms.openlocfilehash: db1ed08c4d4c9e9abd525ec13f5511da82ee1fe4
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="monitor-published-apis"></a>Gepubliceerde API's bewaken

Azure Monitor is een Azure-service die één bron biedt voor het bewaken al uw Azure-resources. Met Azure Monitor kunt u visualiseren, query's uitvoeren, routeren, archiveren en actie ondernemen op basis van de metrische gegevens en logboeken in Azure-resources als API Management. 

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

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Activiteitenlogboeken bekijken

Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd voor uw API Management-services. Met activiteitenlogboeken kunt u het 'wat, wie en wanneer' bepalen voor schrijfbewerkingen (PUT, POST, DELETE) die voor uw API Management-services worden uitgevoerd. 

> [!NOTE]
> Activiteitenlogboeken bevatten geen lees(GET)-bewerkingen of bewerkingen die zijn uitgevoerd in de klassieke Publisher-portal of met behulp van de oorspronkelijke Management-API's.

U kunt activiteitenlogboeken in uw API Management-service openen. U kunt alle logboeken van al uw Azure-resources in Azure Monitor openen. 

Activiteitenlogboeken weergeven:

1. Selecteer uw exemplaar van de APIM-service.
2. Klik op **Activiteitenlogboek**.

## <a name="view-diagnostic-logs"></a>Diagnoselogboeken weergeven

Diagnoselogboeken bieden uitgebreide informatie over bewerkingen en fouten die belangrijk zijn voor zowel controles als het oplossen van problemen. Diagnoselogboeken verschillen van activiteitenlogboeken. Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure-resources. Diagnoselogboeken bieden inzicht in bewerkingen die door de resources zelf zijn uitgevoerd.

Diagnostische logboeken openen:

1. Selecteer uw exemplaar van de APIM-service.
2. Klik op **Diagnoselogboek**.

## <a name="view-metrics-of-your-apis"></a>Metrische gegevens van uw API's weergeven

API Management geeft elke minuut metrische gegevens vrij, waardoor u in vrijwel realtime inzicht hebt in de status van uw API's. Er volgt nu een overzicht van enkele beschikbare metrische gegevens:

* Capaciteit (preview): hiermee kunt u beslissingen nemen over het uitvoeren van een up- of downgrade van uw APIM-services. Dit gegeven komt elke minuut beschikbaar en is een weerspiegeling van de capaciteit van de gateway ten tijde van de export. Het bereik van het gegeven loopt van 0 tot 100 en wordt berekend op basis van gateway-resources als CPU- en geheugengebruik.
* Totaal aantal gateway aanvragen: het aantal API-aanvragen per periode. 
* Geslaagde gateway-aanvragen: het aantal API-aanvragen die HTTP-responscodes met goed gevolg hebben ontvangen, waaronder 304, 307 en codes kleiner dan 301 (bijvoorbeeld 200). 
* Mislukt gateway-aanvragen: het aantal API-aanvragen die HTTP-responscodes bij aanvraagfouten hebben ontvangen, waaronder 400 en codes groter dan 500.
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
7. Roep de vergadering-API aan zonder API-sleutel. Als eigenaar van deze API Management-service ontvangt u een e-mailmelding. 

    > [!TIP]
    > De waarschuwingsregel kan bij activering tevens een webhook of een logische Azure-app aanroepen.

    ![waarschuwing-instellingen](./media/api-management-azure-monitor/set-up-alert.png)

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
