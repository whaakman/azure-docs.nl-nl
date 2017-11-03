---
title: Voorbeeld van de Webhook meldingsactie in OMS Log Analytics | Microsoft Docs
description: "Een van de acties die u in reactie op een waarschuwing voor logboekanalyse uitvoeren kunt is een * webhook *, zodat u kunt het aanroepen van een extern proces via één HTTP-aanvraag. In dit artikel wordt een voorbeeld van het maken van een webhook actie in een waarschuwing voor logboekanalyse met Slack uitgelegd."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: bwren
ms.openlocfilehash: 55b66132f7ec5c26c0a7cac1ec0a5c403dbd1082
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-to-send-message-to-slack"></a>Maken van een actie waarschuwing webhook in OMS Log Analytics bericht te verzenden naar Slack
Een van de acties die u kunt uitvoeren in reactie op een [logboekanalyse waarschuwing](log-analytics-alerts.md) is een *webhook*, waarmee u een extern proces via één HTTP-aanvraag aanroepen.  U kunt meer informatie over de details van waarschuwingen en webhooks in [waarschuwingen in Log Analytics](log-analytics-alerts.md)

In dit artikel behandelen we een voorbeeld van het maken van een webhook actie in een waarschuwing voor logboekanalyse met Slack die een messaging-service.

> [!NOTE]
> U moet een toegestane account te voltooien in dit voorbeeld hebben.  U kunt zich aanmelden voor een gratis account bij [slack.com](http://slack.com).
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a>Stap 1 - webhooks in Slack inschakelen
1. Meld u aan met Slack op [slack.com](http://slack.com).
2. Een kanaal selecteren in de **kanalen** sectie in het linkerdeelvenster.  Dit is het kanaal dat het bericht wordt verzonden naar.  U kunt een van de standaard-kanalen zoals selecteren **algemene** of **willekeurige**.  In een productiescenario u wilt waarschijnlijk een speciale kanaal maken zoals **criticalservicealerts**. <br>
   
   ![Slack-kanalen](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. Klik op **toevoegen van een app of aangepaste integratie** om de App-map te openen.
4. Type *webhooks* in het zoekvak en selecteer vervolgens **binnenkomende WebHooks**. <br>
   
   ![Slack-kanalen](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. Klik op **installeren** naast de teamnaam van uw.
6. Klik op **configuratie toevoegen**.
7. Selecteer de het kanaal dat u wilt gebruiken voor dit voorbeeld en klik vervolgens op **binnenkomende WebHooks toevoegen integratie**.  
8. Kopieer de **Webhook-URL**.  U plakt dit in de configuratie van de waarschuwing. <br>
   
    ![Slack-kanalen](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Stap 2: waarschuwingsregel maken in Log Analytics
1. [Een waarschuwingsregel maakt](log-analytics-alerts.md) met de volgende instellingen.
   * Query:```    Type=Event EventLevelName=error ```
   * Controleer voor deze waarschuwing elke: 5 minuten
   * Het aantal resultaten: groter is dan 10
   * Via dit tijdvenster: 60 minuten
   * Selecteer **Ja** voor **Webhook** en **Nee** voor de andere acties.
2. Plak de URL van de vertraging in de **Webhook-URL** veld.
3. Selecteer de optie voor **aangepaste JSON-nettolading opnemen**.
4. Toegestane verwacht een nettolading in JSON is geformatteerd met een parameter genaamd *tekst*.  Dit is de tekst die wordt weergegeven in het bericht dat wordt gemaakt.  U kunt een of meer van de waarschuwing parameters met de  *#*  symbool zoals zoals in het volgende voorbeeld.
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```
   
    ![Voorbeeld van de JSON-nettolading](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. Klik op **opslaan** om op te slaan van de waarschuwingsregel.
6. Wacht voldoende tijd om een waarschuwing te worden gemaakt en vervolgens Slack controleren op een bericht dat op het volgende lijkt.
   
   ![Voorbeeld van de webhook in Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a>Geavanceerde webhook nettolading voor vertraging
U kunt binnenkomende berichten met Slack grote schaal aanpassen. Zie voor meer informatie [binnenkomende Webhooks](https://api.slack.com/incoming-webhooks) op de website van de toegestane vertraging. Hieronder vindt u een complexere nettolading met een uitgebreid bericht met opmaak maken:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Hierdoor zou een bericht gegenereerd in Slack vergelijkbaar met de volgende.

![Voorbeeld van de berichten in Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Samenvatting
Met deze waarschuwingsregel in plaats hebt u een bericht verzonden met Slack telkens wanneer de criteria wordt voldaan.  

Dit is slechts één voorbeeld van een actie die u in reactie op een waarschuwing kunt maken.  U kunt een webhook-actie die een andere externe service aanroept, een runbook-actie voor het starten van een runbook in Azure Automation of een e-actie een e-mail te verzenden naar uzelf of naar andere ontvangers maken.   

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over andere [waarschuwing acties in logboekanalyse](log-analytics-alerts-actions.md) met inbegrip van andere acties.


