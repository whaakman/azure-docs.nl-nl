---
title: Bijhouden van aangepaste schema's voor het bewaken van B2B - Azure Logic Apps | Microsoft Docs
description: Bijhouden van aangepaste schema's voor het bewaken van B2B-berichten van transacties in uw Azure-integratie-Account maken.
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b71a4938dde2a71f1ce29403af7aa9101358d64c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-tracking-to-monitor-your-complete-workflow-end-to-end"></a>Bijhouden voor het bewaken van de volledige werkstroom, end-to-end inschakelen
Er is ingebouwde bijhouden dat u voor de verschillende onderdelen van uw business-to-business-werkstroom, zoals bijhouden AS2 of X12 berichten inschakelen kunt. Wanneer u werkstromen maakt die bevat een logische app, BizTalk Server, SQL Server of een andere laag en vervolgens kunt u aangepaste bijhouden die legt gebeurtenissen vanaf het begin tot het einde van de werkstroom vast inschakelen. 

Dit onderwerp bevat aangepaste code die u in de lagen buiten uw logische app kunt gebruiken. 

## <a name="custom-tracking-schema"></a>Bijhouden van aangepaste schema
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| SourceType |   | Type van de bron-uitvoeren. Toegestane waarden zijn **Microsoft.Logic/workflows** en **aangepaste**. (Verplicht) |
| Bron |   | Als het type bron **Microsoft.Logic/workflows**, moet de broninformatie Volg dit schema. Als het type bron **aangepaste**, het schema is een JToken. (Verplicht) |
| systeem-id | Tekenreeks | Logic app systeem-ID. (Verplicht) |
| runId | Tekenreeks | Logische app uitgevoerd ID. (Verplicht) |
| operationName | Tekenreeks | De naam van de bewerking (bijvoorbeeld actie of trigger). (Verplicht) |
| repeatItemScopeName | Tekenreeks | Herhaal itemnaam als de actie is opgenomen in een `foreach` / `until` lus. (Verplicht) |
| repeatItemIndex | Geheel getal | Of de actie is binnen een `foreach` / `until` lus. Geeft de itemindex herhaalde. (Verplicht) |
| trackingId | Tekenreeks | Tracerings-ID die de berichten correleren. (Optioneel) |
| correlationId | Tekenreeks | Correlatie-ID die de berichten correleren. (Optioneel) |
| clientRequestId | Tekenreeks | Client kunt vullen om te correleren van berichten. (Optioneel) |
| eventLevel |   | Niveau van de gebeurtenis. (Verplicht) |
| eventTime |   | Tijd van de gebeurtenis, in UTC-notatie jjjj-MM-DDTHH:MM:SS.00000Z. (Verplicht) |
| recordType |   | Type van de record bijhouden. De waarde is toegestaan **aangepaste**. (Verplicht) |
| Record |   | Aangepaste recordtype. Toegestane indeling is JToken. (Verplicht) |

## <a name="b2b-protocol-tracking-schemas"></a>Schema's voor bijhouden van B2B-protocol
Zie voor informatie over B2B-protocol voor het bijhouden van schema's:
* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [Volgschema’s voor X12](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [B2B-berichten controleren](logic-apps-monitor-b2b-message.md).   
* Meer informatie over [B2B-berichten in de Operations Management Suite-portal traceren](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Meer informatie over de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).
