---
title: AS2 bijhouden schema's voor het bewaken van B2B - Azure Logic Apps | Microsoft Docs
description: AS2 bijhouden schema's gebruiken voor het bewaken van B2B-berichten van transacties in uw Azure-Account voor integratie.
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 31bd296dc5ed5ac6998a6c05ee80fd38b12d662c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
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
| senderPartnerName | Tekenreeks | Naam van de partner van afzender AS2-bericht. (Optioneel) |
| receiverPartnerName | Tekenreeks | Naam van de partner AS2 bericht ontvanger. (Optioneel) |
| as2To | Tekenreeks | AS2-bericht ontvanger-naam van de headers van het bericht AS2. (Verplicht) |
| as2From | Tekenreeks | AS2-bericht van de afzender-naam van de headers van het bericht AS2. (Verplicht) |
| agreementName | Tekenreeks | Naam van de AS2-overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| Richting | Tekenreeks | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| MessageId | Tekenreeks | AS2-bericht-ID van de headers van het bericht AS2 (optioneel) |
| dispositionType |Tekenreeks | Bericht Disposition melding (MDN) disposition typewaarde. (Optioneel) |
| fileName | Tekenreeks | Bestandsnaam uit de kop van het bericht AS2. (Optioneel) |
| isMessageFailed |Booleaanse waarde | Hiermee wordt aangegeven of het AS2-bericht is mislukt. (Verplicht) |
| isMessageSigned | Booleaanse waarde | Hiermee wordt aangegeven of de AS2-bericht is ondertekend. (Verplicht) |
| isMessageEncrypted | Booleaanse waarde | Hiermee wordt aangegeven of het bericht AS2 is versleuteld. (Verplicht) |
| isMessageCompressed |Booleaanse waarde | Hiermee wordt aangegeven of het bericht AS2 is gecomprimeerd. (Verplicht) |
| correlationMessageId | Tekenreeks | AS2 bericht-ID, met elkaar correleren berichten met MDNs. (Optioneel) |
| incomingHeaders |Woordenlijst van JToken | Binnenkomende AS2-header details van bericht. (Optioneel) |
| outgoingHeaders |Woordenlijst van JToken | Uitgaande header details van bericht van AS2. (Optioneel) |
| isNrrEnabled | Booleaanse waarde | Gebruik de standaardwaarde als de waarde is niet bekend. (Verplicht) |
| isMdnExpected | Booleaanse waarde | Gebruik de standaardwaarde als de waarde is niet bekend. (Verplicht) |
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
| senderPartnerName | Tekenreeks | Naam van de partner van afzender AS2-bericht. (Optioneel) |
| receiverPartnerName | Tekenreeks | Naam van de partner AS2 bericht ontvanger. (Optioneel) |
| as2To | Tekenreeks | Partnernaam die het AS2-bericht ontvangt. (Verplicht) |
| as2From | Tekenreeks | Partnernaam die u het bericht AS2 verzendt. (Verplicht) |
| agreementName | Tekenreeks | Naam van de AS2-overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| Richting |Tekenreeks | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| MessageId | Tekenreeks | AS2-bericht-ID. (Optioneel) |
| OriginalMessageId |Tekenreeks | AS2 oorspronkelijke bericht-ID. (Optioneel) |
| dispositionType | Tekenreeks | MDN toestand typewaarde. (Optioneel) |
| isMessageFailed |Booleaanse waarde | Hiermee wordt aangegeven of het AS2-bericht is mislukt. (Verplicht) |
| isMessageSigned |Booleaanse waarde | Hiermee wordt aangegeven of de AS2-bericht is ondertekend. (Verplicht) |
| isNrrEnabled | Booleaanse waarde | Gebruik de standaardwaarde als de waarde is niet bekend. (Verplicht) |
| statusCode | Enum | Toegestane waarden zijn **geaccepteerde**, **geweigerd**, en **AcceptedWithErrors**. (Verplicht) |
| micVerificationStatus | Enum | Toegestane waarden zijn **niet van toepassing**, **geslaagd**, en **mislukt**. (Verplicht) |
| correlationMessageId | Tekenreeks | Correlatie-ID. De oorspronkelijke ID mailberichten (de bericht-ID van het bericht waarvoor MDN is geconfigureerd). (Optioneel) |
| incomingHeaders | Woordenlijst van JToken | Geeft de details van inkomende bericht-header. (Optioneel) |
| outgoingHeaders |Woordenlijst van JToken | Geeft de details van uitgaande bericht-header. (Optioneel) |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* Meer informatie over [B2B-berichten controleren](logic-apps-monitor-b2b-message.md).   
* Meer informatie over [B2B aangepaste schema's bijhouden](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Meer informatie over [X12 schema's bijhouden](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Meer informatie over [B2B-berichten in de Operations Management Suite-portal traceren](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
