---
title: Bijhouden van schema's voor B2B X12 monitoring - Azure Logic Apps | Microsoft Docs
description: Gebruik X12 bijhouden van schema's voor het bewaken van B2B-berichten van transacties in uw Azure-Account voor integratie.
author: padmavc
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 385ff73d780f62f19af2face591cd0b2291ef396
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300970"
---
# <a name="start-or-enable-tracking-of-x12-messages-to-monitor-success-errors-and-message-properties"></a>Begin- of inschakelen bijhouden van X12 berichten naar de monitor geslaagd, fouten en berichteigenschappen
U kunt deze X12 bijhouden van schema's in uw integratie van Azure-account gebruiken om u te helpen bij het bewaken van transacties voor business-to-business (B2B):

* X12 transactie bijhouden schema instellen
* X12 transactie bevestiging bijhouden schema instellen
* X12 interchange bijhouden schema
* X12 interchange bevestiging schema bijhouden
* Functionele X12 groep bijhouden schema
* Functionele X12 groep bevestiging schema bijhouden

## <a name="x12-transaction-set-tracking-schema"></a>X12 transactie bijhouden schema instellen
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "transactionSetControlNumber": "",
                "CorrelationMessageId": "",
                "messageType": "",
                "isMessageFailed": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "needAk2LoopForValidMessages":  "",
                "segmentsCount": ""
            }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van de afzender het bericht X12. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner van de ontvanger het bericht X12. (Optioneel) |
| senderQualifier | Reeks | Kwalificatie partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Verzenden partner-id. (Verplicht) |
| receiverQualifier | Reeks | Ontvangen partner-kwalificatie. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Controle-aantal Interchange. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele besturingselement getal. (Optioneel) |
| transactionSetControlNumber | Reeks | Transactie instellen besturingselement aantal. (Optioneel) |
| correlationMessageId | Reeks | Correlatie-ID. Een combinatie van {AgreementName} {*GroupControlNumber*} {TransactionSetControlNumber}. (Optioneel) |
| messageType | Reeks | Transactie is ingesteld of documenttype. (Optioneel) |
| isMessageFailed | Boole-waarde | Of de X12 bericht is mislukt. (Verplicht) |
| isTechnicalAcknowledgmentExpected | Boole-waarde | Hiermee wordt aangegeven of de technische bevestiging is geconfigureerd in de x12-overeenkomst(en) overeenkomst. (Verplicht) |
| isFunctionalAcknowledgmentExpected | Boole-waarde | Hiermee wordt aangegeven of het functionele bevestiging is geconfigureerd in de x12-overeenkomst(en) overeenkomst. (Verplicht) |
| needAk2LoopForValidMessages | Boole-waarde | Hiermee wordt aangegeven of de lus AK2 is vereist voor een geldig bericht. (Verplicht) |
| segmentsCount | Geheel getal | Aantal segmenten in de x12-overeenkomst(en) transactie set. (Optioneel) |

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 transactie bevestiging bijhouden schema instellen
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "respondingtransactionSetControlNumber": "",
                "respondingTransactionSetId": "",
                "statusCode": "",
                "processingStatus": "",
                "CorrelationMessageId": ""
                "isMessageFailed": "",
                "ak2Segment": "",
                "ak3Segment": "",
                "ak5Segment": ""
            }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van de afzender het bericht X12. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner van de ontvanger het bericht X12. (Optioneel) |
| senderQualifier | Reeks | Kwalificatie partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Verzenden partner-id. (Verplicht) |
| receiverQualifier | Reeks | Ontvangen partner-kwalificatie. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Controle-aantal van de functionele ontvangstbevestiging Interchange. Waarde gevuld alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele groep besturingselement nummer van de functionele bevestiging. Waarde gevuld alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| isaSegment | Reeks | ISA-segment van het bericht. Waarde gevuld alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| gsSegment | Reeks | GS-segment van het bericht. Waarde gevuld alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| respondingfunctionalGroupControlNumber | Reeks | Controle-aantal interchange reageert. (Optioneel) |
| respondingFunctionalGroupId | Reeks | Functionele groep-ID, die in de ontvangstbevestiging wordt toegewezen aan AK101 reageert. (Optioneel) |
| respondingtransactionSetControlNumber | Reeks | Reageert transactie instellen besturingselement aantal. (Optioneel) |
| respondingTransactionSetId | Reeks | Reageert transactie-ID, die in de ontvangstbevestiging wordt toegewezen aan AK201 gedefinieerd. (Optioneel) |
| statusCode | Boole-waarde | Transactie bevestiging statuscode ingesteld. (Verplicht) |
| segmentsCount | Enum | Statuscode bevestiging. Toegestane waarden zijn **geaccepteerde**, **geweigerd**, en **AcceptedWithErrors**. (Verplicht) |
| Verwerkingsstatusnaam | Enum | Verwerking van de status van de bevestiging. Toegestane waarden zijn **ontvangen**, **gegenereerde**, en **verzonden**. (Verplicht) |
| correlationMessageId | Reeks | Correlatie-ID. Een combinatie van {AgreementName} {*GroupControlNumber*} {TransactionSetControlNumber}. (Optioneel) |
| isMessageFailed | Boole-waarde | Of de X12 bericht is mislukt. (Verplicht) |
| ak2Segment | Reeks | Bevestiging voor een transactie ingesteld binnen de ontvangen functionele groep. (Optioneel) |
| ak3Segment | Reeks | Fouten in een gegevenssegment rapporteert. (Optioneel) |
| ak5Segment | Reeks | Rapporten of de transactie die is ingesteld in het segment AK2 geïdentificeerde wordt goedgekeurd of geweigerd en waarom. (Optioneel) |

## <a name="x12-interchange-tracking-schema"></a>X12 interchange bijhouden schema
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "isa09": "",
                "isa10": "",
                "isa11": "",
                "isa12": "",
                "isa14": "",
                "isa15": "",
                "isa16": ""
            }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van de afzender het bericht X12. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner van de ontvanger het bericht X12. (Optioneel) |
| senderQualifier | Reeks | Kwalificatie partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Verzenden partner-id. (Verplicht) |
| receiverQualifier | Reeks | Ontvangen partner-kwalificatie. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Controle-aantal Interchange. (Optioneel) |
| isaSegment | Reeks | Bericht ISA-segment. (Optioneel) |
| isTechnicalAcknowledgmentExpected | Boole-waarde | Hiermee wordt aangegeven of de technische bevestiging is geconfigureerd in de x12-overeenkomst(en) overeenkomst. (Verplicht) |
| isMessageFailed | Boole-waarde | Of de X12 bericht is mislukt. (Verplicht) |
| isa09 | Reeks | X12 document interchange datum. (Optioneel) |
| isa10 | Reeks | X12 Documenteer DIF-tijd. (Optioneel) |
| isa11 | Reeks | X12 interchange besturingselement standaarden id. (Optioneel) |
| isa12 | Reeks | X12 interchange besturingselement versienummer. (Optioneel) |
| isa14 | Reeks | X12 bevestiging wordt gevraagd. (Optioneel) |
| isa15 | Reeks | De indicator voor test- en productie. (Optioneel) |
| isa16 | Reeks | Element scheidingsteken. (Optioneel) |

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>X12 interchange bevestiging schema bijhouden
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "respondingInterchangeControlNumber": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ta102": "",
                "ta103": "",
                "ta105": ""
            }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van de afzender het bericht X12. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner van de ontvanger het bericht X12. (Optioneel) |
| senderQualifier | Reeks | Kwalificatie partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Verzenden partner-id. (Verplicht) |
| receiverQualifier | Reeks | Ontvangen partner-kwalificatie. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Interchange besturingselementnummer van de technische bevestiging dat ontvangen van partners. (Optioneel) |
| isaSegment | Reeks | ISA-segment voor de technische bevestiging dat ontvangen van partners. (Optioneel) |
| respondingInterchangeControlNumber |Reeks | Interchange besturingselementnummer voor de technische bevestiging dat ontvangen van partners. (Optioneel) |
| isMessageFailed | Boole-waarde | Of de X12 bericht is mislukt. (Verplicht) |
| statusCode | Enum | Interchange statuscode bevestiging. Toegestane waarden zijn **geaccepteerde**, **geweigerd**, en **AcceptedWithErrors**. (Verplicht) |
| Verwerkingsstatusnaam | Enum | Status van de bevestiging. Toegestane waarden zijn **ontvangen**, **gegenereerde**, en **verzonden**. (Verplicht) |
| 102 | Reeks | Interchange datum. (Optioneel) |
| ta103 | Reeks | Interchange tijd. (Optioneel) |
| ta105 | Reeks | Interchange Opmerking code. (Optioneel) |

## <a name="x12-functional-group-tracking-schema"></a>Functionele X12 groep bijhouden schema
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "gsSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "gs01": "",
                "gs02": "",
                "gs03": "",
                "gs04": "",
                "gs05": "",
                "gs07": "",
                "gs08": ""
            }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van de afzender het bericht X12. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner van de ontvanger het bericht X12. (Optioneel) |
| senderQualifier | Reeks | Kwalificatie partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Verzenden partner-id. (Verplicht) |
| receiverQualifier | Reeks | Ontvangen partner-kwalificatie. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Controle-aantal Interchange. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele besturingselement getal. (Optioneel) |
| gsSegment | Reeks | GS-berichtsegment. (Optioneel) |
| isTechnicalAcknowledgmentExpected | Boole-waarde | Hiermee wordt aangegeven of de technische bevestiging is geconfigureerd in de x12-overeenkomst(en) overeenkomst. (Verplicht) |
| isFunctionalAcknowledgmentExpected | Boole-waarde | Hiermee wordt aangegeven of het functionele bevestiging is geconfigureerd in de x12-overeenkomst(en) overeenkomst. (Verplicht) |
| isMessageFailed | Boole-waarde | Of de X12 bericht is mislukt. (Verplicht)|
| gs01 | Reeks | Functionele identificatiecode. (Optioneel) |
| gs02 | Reeks | De code van de afzender van de toepassing. (Optioneel) |
| gs03 | Reeks | De code van de ontvanger van de toepassing. (Optioneel) |
| gs04 | Reeks | Functionele groep datum. (Optioneel) |
| gs05 | Reeks | Functionele groep tijd. (Optioneel) |
| gs07 | Reeks | Verantwoordelijk agency code. (Optioneel) |
| gs08 | Reeks | Release-versie/bedrijfstak-id-code. (Optioneel) |

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>Functionele X12 groep bevestiging schema bijhouden
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ak903": "",
                "ak904": "",
                "ak9Segment": ""
            }
    }
````

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | Naam van de partner van de afzender het bericht X12. (Optioneel) |
| receiverPartnerName | Reeks | Naam van de partner van de ontvanger het bericht X12. (Optioneel) |
| senderQualifier | Reeks | Kwalificatie partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Verzenden partner-id. (Verplicht) |
| receiverQualifier | Reeks | Ontvangen partner-kwalificatie. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarnaar de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Controle-aantal voor de verzendkant gevuld wanneer u een technische bevestiging is ontvangen van partners Interchange. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele groep besturingselement nummer van de technische bevestiging, voor de verzendkant gevuld wanneer u een technische bevestiging is ontvangen van partners. (Optioneel) |
| isaSegment | Reeks | Zelfde als interchange getal, maar alleen in bijzondere gevallen ingevuld beheren. (Optioneel) |
| gsSegment | Reeks | Zelfde als functionele groep aantal, maar alleen in bijzondere gevallen ingevuld bepalen. (Optioneel) |
| respondingfunctionalGroupControlNumber | Reeks | Aantal van de oorspronkelijke functionele groep bepalen. (Optioneel) |
| respondingFunctionalGroupId | Reeks | Is toegewezen aan AK101 in de bevestiging functionele groep-id. (Optioneel) |
| isMessageFailed | Boole-waarde | Of de X12 bericht is mislukt. (Verplicht) |
| statusCode | Enum | Statuscode bevestiging. Toegestane waarden zijn **geaccepteerde**, **geweigerd**, en **AcceptedWithErrors**. (Verplicht) |
| Verwerkingsstatusnaam | Enum | Verwerking van de status van de bevestiging. Toegestane waarden zijn **ontvangen**, **gegenereerde**, en **verzonden**. (Verplicht) |
| ak903 | Reeks | Het aantal transactie sets ontvangen. (Optioneel) |
| ak904 | Reeks | Aantal transactie sets geaccepteerd in de geïdentificeerde functionele groep. (Optioneel) |
| ak9Segment | Reeks | Of de functionele-groep geïdentificeerd in het segment AK1 wordt goedgekeurd of geweigerd en waarom. (Optioneel) |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [B2B-berichten controleren](logic-apps-monitor-b2b-message.md).
* Meer informatie over [AS2 bijhouden schema's](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md).
* Meer informatie over [B2B aangepaste schema's bijhouden](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md).
* Meer informatie over [bijhouden B2B-berichten in logboekanalyse](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Meer informatie over de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).  
