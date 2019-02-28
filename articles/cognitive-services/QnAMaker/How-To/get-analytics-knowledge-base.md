---
title: Analyse van Knowledge Base
titleSuffix: Azure Cognitive Services
description: QnA Maker worden alle chat-logboeken en andere telemetrie, opgeslagen als u de App Insights hebt ingeschakeld tijdens het maken van uw QnA Maker-service. Voer de voorbeeldquery's voor uw chatlogs van App Insights.
services: cognitive-services
author: tulasim88
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/04/2019
ms.author: tulasim
ms.openlocfilehash: 09369e760c654e7e56067e6da02bb772bffa7400
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960833"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Analytische gegevens verkrijgen voor uw knowledge base

QnA Maker slaat alle chat-logboeken en andere telemetrie, als u hebt ingeschakeld App Insights tijdens de [maken van uw QnA Maker-service](./set-up-qnamaker-service-azure.md). Voer de voorbeeldquery's voor uw chatlogs van App Insights.

1. Ga naar uw App Insights-resource.

    ![Selecteer uw application insights-resource](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Selecteer **Analytics**. Een nieuw venster te openen waar u de QnA Maker telemetrie kunt opvragen.

    ![Selecteer Analytics](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Plak in de volgende query en voer de.

    ```query
        requests
        | where url endswith "generateAnswer"
        | project timestamp, id, name, resultCode, duration
        | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
        | join kind= inner (
        traces | extend id = operation_ParentId
        ) on id
        | extend question = tostring(customDimensions['Question'])
        | extend answer = tostring(customDimensions['Answer'])
        | project KbId, timestamp, resultCode, duration, question, answer
    ```

    Selecteer **uitvoeren** de query uit te voeren.

    ![Query uitvoeren](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Query's voor andere analyses uitvoeren op uw QnA Maker knowledge base

### <a name="total-90-day-traffic"></a>Totale verkeer van 90 dagen

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Totaal aantal vraag verkeer in een bepaalde periode

```query
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Gebruikersverkeer

```query
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Distributie van clientlatentie vragen

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sleutels beheren](./key-management.md)
