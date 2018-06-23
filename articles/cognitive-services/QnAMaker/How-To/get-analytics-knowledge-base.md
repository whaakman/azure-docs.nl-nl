---
title: Het ophalen van analytics op uw knowledge base - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Het ophalen van analytics op uw kennisdatabase
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 1588d0c5a8eaf4e161b5319c9f33a772dc56b247
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345280"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Analytics op uw knowledge base ophalen

QnA Maker slaat alle chat-logboeken en andere telemetrie als u hebt ingeschakeld App Insights tijdens de [maken van uw service QnA Maker](./set-up-qnamaker-service-azure.md). Voer de voorbeeldquery's voor het ophalen van uw logboeken chat uit de App Insights.

1. Ga naar uw App Insights-resource.

    ![Selecteer uw application insights-resource](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Selecteer **Analytics**. Een nieuw venster te openen waar u QnA Maker telemetrie kunt opvragen.

    ![Selecteer Analytics](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Plak in de volgende query en voer deze uit.

    ```query
        requests
        | where url endswith "generateAnswer"
        | project timestamp, id, name, resultCode, duration
        | parse name with *"/knowledgebases/"KbId"/generateAnswer"
        | join kind= inner (
        traces | extend id = operation_ParentId
        ) on id
        | extend question = tostring(customDimensions['Question'])
        | extend answer = tostring(customDimensions['Answer'])
        | project KbId, timestamp, resultCode, duration, question, answer
    ```

    Selecteer **uitvoeren** de query uit te voeren.

    ![Query uitvoeren](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Query's voor andere analyses uitvoeren op uw QnA Maker-kB

### <a name="total-90-day-traffic"></a>Totale aantal 90 dagen verkeer

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
``` 

### <a name="total-question-traffic-in-a-given-time-period"></a>Verkeer van de totale vraag in een bepaalde periode

```query
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Gebruikersverkeer

```query
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse name with *"/knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Distributie van clientlatentie van vragen

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sleutels beheren](./key-management.md)
