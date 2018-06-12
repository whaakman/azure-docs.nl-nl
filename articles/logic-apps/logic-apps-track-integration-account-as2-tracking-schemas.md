---
title: AS2 bijhouden schema's voor het bewaken van B2B - Azure Logic Apps | Microsoft Docs
description: AS2 bijhouden schema's gebruiken voor het bewaken van B2B-berichten van transacties in uw Azure-Account voor integratie.
author: padmavc
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 48e39fd20716e962c4a3e367fdff18e0b4fba32d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300878"
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-to-monitor-success-errors-and-message-properties"></a>Starten of het bijhouden van AS2 berichten en MDNs monitor geslaagd, fouten en berichteigenschappen inschakelen
U kunt deze AS2 bijhouden schema's in uw integratie van Azure-account gebruiken om u te helpen bij het bewaken van transacties voor business-to-business (B2B):

* AS2-bericht bijhouden schema
* AS2-MDN bijhouden schema

## <a name="as2-message-tracking-schema"></a>AS2-bericht bijhouden schema
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van afzender AS2-bericht. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner AS2 bericht ontvanger. (Optioneel) |
| as2To | Reeks | AS2-bericht ontvanger-naam van de headers van het bericht AS2. (Verplicht) |
| as2From | Reeks | AS2-bericht van de afzender-naam van de headers van het bericht AS2. (Verplicht) |
| agreementName | Reeks | Naam van de AS2-overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting | Reeks | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| MessageId | Reeks | AS2-bericht-ID van de headers van het bericht AS2 (optioneel) |
| dispositionType |Reeks | Bericht Disposition melding (MDN) disposition typewaarde. (Optioneel) |
| fileName | Reeks | Bestandsnaam uit de kop van het bericht AS2. (Optioneel) |
| isMessageFailed |Boole-waarde | Hiermee wordt aangegeven of het AS2-bericht is mislukt. (Verplicht) |
| isMessageSigned | Boole-waarde | Hiermee wordt aangegeven of de AS2-bericht is ondertekend. (Verplicht) |
| isMessageEncrypted | Boole-waarde | Hiermee wordt aangegeven of het bericht AS2 is versleuteld. (Verplicht) |
| isMessageCompressed |Boole-waarde | Hiermee wordt aangegeven of het bericht AS2 is gecomprimeerd. (Verplicht) |
| correlationMessageId | Reeks | AS2 bericht-ID, met elkaar correleren berichten met MDNs. (Optioneel) |
| incomingHeaders |Woordenlijst van JToken | Binnenkomende AS2-header details van bericht. (Optioneel) |
| outgoingHeaders |Woordenlijst van JToken | Uitgaande header details van bericht van AS2. (Optioneel) |
| isNrrEnabled | Boole-waarde | Gebruik de standaardwaarde als de waarde is niet bekend. (Verplicht) |
| isMdnExpected | Boole-waarde | Gebruik de standaardwaarde als de waarde is niet bekend. (Verplicht) |
| mdnType | Enum | Toegestane waarden zijn **NotConfigured**, **Sync**, en **asynchrone**. (Verplicht) |

## <a name="as2-mdn-tracking-schema"></a>AS2-MDN bijhouden schema
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van afzender AS2-bericht. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner AS2 bericht ontvanger. (Optioneel) |
| as2To | Reeks | Partnernaam die het AS2-bericht ontvangt. (Verplicht) |
| as2From | Reeks | Partnernaam die u het bericht AS2 verzendt. (Verplicht) |
| agreementName | Reeks | Naam van de AS2-overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting |Reeks | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| MessageId | Reeks | AS2-bericht-ID. (Optioneel) |
| OriginalMessageId |Reeks | AS2 oorspronkelijke bericht-ID. (Optioneel) |
| dispositionType | Reeks | MDN toestand typewaarde. (Optioneel) |
| isMessageFailed |Boole-waarde | Hiermee wordt aangegeven of het AS2-bericht is mislukt. (Verplicht) |
| isMessageSigned |Boole-waarde | Hiermee wordt aangegeven of de AS2-bericht is ondertekend. (Verplicht) |
| isNrrEnabled | Boole-waarde | Gebruik de standaardwaarde als de waarde is niet bekend. (Verplicht) |
| statusCode | Enum | Toegestane waarden zijn **geaccepteerde**, **geweigerd**, en **AcceptedWithErrors**. (Verplicht) |
| micVerificationStatus | Enum | Toegestane waarden zijn **niet van toepassing**, **geslaagd**, en **mislukt**. (Verplicht) |
| correlationMessageId | Reeks | Correlatie-ID. De oorspronkelijke ID mailberichten (de bericht-ID van het bericht waarvoor MDN is geconfigureerd). (Optioneel) |
| incomingHeaders | Woordenlijst van JToken | Geeft de details van inkomende bericht-header. (Optioneel) |
| outgoingHeaders |Woordenlijst van JToken | Geeft de details van uitgaande bericht-header. (Optioneel) |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* Meer informatie over [B2B-berichten controleren](logic-apps-monitor-b2b-message.md).   
* Meer informatie over [B2B aangepaste schema's bijhouden](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Meer informatie over [X12 schema's bijhouden](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Meer informatie over [bijhouden B2B-berichten in logboekanalyse](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
