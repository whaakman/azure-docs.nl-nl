---
title: X12 volgschema's voor B2B-berichten - Azure Logic Apps | Microsoft Docs
description: Volgschema's die B2B-berichten in integratieaccounts voor Azure Logic Apps met Enterprise Integration Pack bewaken X12 maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.date: 01/27/2017
ms.openlocfilehash: cfd195f2f812c8b2e09058d325d0dbb6f7a60d59
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125598"
---
# <a name="create-schemas-for-tracking-x12-messages-in-integration-accounts-for-azure-logic-apps"></a>Maken van schema's voor het bijhouden van X12 berichten in integratieaccounts voor Azure Logic Apps

Om u te helpen succes van de monitor, fouten en eigenschappen van berichten voor business-to-business (B2B)-transacties, kunt u deze X12 volgschema's in uw integratie-account:

* X12 volgschema's voor de transactieset
* X12 bevestiging bijhouden schema voor de transactieset
* X12 interchange volgschema 's
* X12 interchange bevestiging schema bijhouden
* X12 functionele groep volgschema 's
* X12 functionele groep bevestiging schema bijhouden

## <a name="x12-transaction-set-tracking-schema"></a>X12 volgschema's voor de transactieset

```json
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
```

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | X12 Partnernaam van de afzender het bericht. (Optioneel) |
| receiverPartnerName | Reeks | X12 Partnernaam van de ontvanger het bericht. (Optioneel) |
| senderQualifier | Reeks | De kwalificatie van de partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Partner-id verzenden. (Verplicht) |
| receiverQualifier | Reeks | De kwalificatie van de partner ontvangt. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarop de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Uitwisselingscontrolenummer. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele controlenummer. (Optioneel) |
| transactionSetControlNumber | Reeks | Transactiereekscontrolenummer. (Optioneel) |
| CorrelationMessageId | Reeks | Correlatie bericht-ID. Een combinatie van {agreementname die} {*GroupControlNumber*} {TransactionSetControlNumber}. (Optioneel) |
| MessageType | Reeks | Transactie standaarddocumenttype of ingesteld. (Optioneel) |
| isMessageFailed | Booleaans | Of de X12 bericht is mislukt. (Verplicht) |
| isTechnicalAcknowledgmentExpected | Booleaans | Of de technische bevestiging is geconfigureerd in de X12 overeenkomst. (Verplicht) |
| isFunctionalAcknowledgmentExpected | Booleaans | Of de functionele bevestiging is geconfigureerd in de X12 overeenkomst. (Verplicht) |
| needAk2LoopForValidMessages | Booleaans | Of de AK2-lus is vereist voor een geldig bericht. (Verplicht) |
| segmentsCount | Geheel getal | Aantal segmenten in de X12 transactieset. (Optioneel) |
||||

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 bevestiging bijhouden schema voor de transactieset

```json
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
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | X12 Partnernaam van de afzender het bericht. (Optioneel) |
| receiverPartnerName | Reeks | X12 Partnernaam van de ontvanger het bericht. (Optioneel) |
| senderQualifier | Reeks | De kwalificatie van de partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Partner-id verzenden. (Verplicht) |
| receiverQualifier | Reeks | De kwalificatie van de partner ontvangt. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarop de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Uitwisselingscontrolenummer van de functionele bevestiging. Waarde vult alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele groepscontrolenummer van de functionele bevestiging. Waarde vult alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| isaSegment | Reeks | Het ISA-segment van het bericht. Waarde vult alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| gsSegment | Reeks | GS-segment van het bericht. Waarde vult alleen voor de verzendkant waar functionele bevestiging is ontvangen voor de berichten die worden verzonden naar partner. (Optioneel) |
| respondingfunctionalGroupControlNumber | Reeks | Uitwisseling van het controlenummer reageert. (Optioneel) |
| respondingFunctionalGroupId | Reeks | Functionele groep-ID, die in de ontvangstbevestiging wordt toegewezen aan AK101 reageert. (Optioneel) |
| respondingtransactionSetControlNumber | Reeks | Reageert transactiereekscontrolenummer. (Optioneel) |
| respondingTransactionSetId | Reeks | Reageert transactieset-ID, die wordt toegewezen aan AK201 in de bevestiging. (Optioneel) |
| statusCode | Booleaans | Bevestiging statuscode voor de transactieset. (Verplicht) |
| segmentsCount | Enum | Bevestiging statuscode. Toegestane waarden zijn **geaccepteerde**, **afgewezen**, en **AcceptedWithErrors**. (Verplicht) |
| Verwerkingsstatusnaam | Enum | De verwerkingsstatus van de bevestiging. Toegestane waarden zijn **ontvangen**, **gegenereerde**, en **verzonden**. (Verplicht) |
| CorrelationMessageId | Reeks | Correlatie bericht-ID. Een combinatie van {agreementname die} {*GroupControlNumber*} {TransactionSetControlNumber}. (Optioneel) |
| isMessageFailed | Booleaans | Of de X12 bericht is mislukt. (Verplicht) |
| ak2Segment | Reeks | Bevestiging voor een transactie die is ingesteld in de functiegroep ontvangen. (Optioneel) |
| ak3Segment | Reeks | Rapporten fouten in een gegevenssegment. (Optioneel) |
| ak5Segment | Reeks | Rapporten of de transactie is ingesteld in het segment AK2 geïdentificeerde wordt geaccepteerd of geweigerd en waarom. (Optioneel) |
||||

## <a name="x12-interchange-tracking-schema"></a>X12 interchange volgschema 's

```json
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
```

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | X12 Partnernaam van de afzender het bericht. (Optioneel) |
| receiverPartnerName | Reeks | X12 Partnernaam van de ontvanger het bericht. (Optioneel) |
| senderQualifier | Reeks | De kwalificatie van de partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Partner-id verzenden. (Verplicht) |
| receiverQualifier | Reeks | De kwalificatie van de partner ontvangt. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarop de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Uitwisselingscontrolenummer. (Optioneel) |
| isaSegment | Reeks | Bericht ISA-segment. (Optioneel) |
| isTechnicalAcknowledgmentExpected | Booleaans | Of de technische bevestiging is geconfigureerd in de X12 overeenkomst. (Verplicht) |
| isMessageFailed | Booleaans | Of de X12 bericht is mislukt. (Verplicht) |
| isa09 | Reeks | X12 document uitwisseling datum. (Optioneel) |
| isa10 | Reeks | X12 document uitwisseling tijd. (Optioneel) |
| isa11 | Reeks | X12 interchange besturingselement standaarden-id. (Optioneel) |
| isa12 | Reeks | X12 interchange besturingselement versienummer. (Optioneel) |
| isa14 | Reeks | X12 bevestiging wordt gevraagd. (Optioneel) |
| isa15 | Reeks | Indicator voor test- of productieomgeving. (Optioneel) |
| isa16 | Reeks | Scheidingsteken voor gegevenselementen. (Optioneel) |
||||

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>X12 interchange bevestiging schema bijhouden

```json
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
```

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | X12 Partnernaam van de afzender het bericht. (Optioneel) |
| receiverPartnerName | Reeks | X12 Partnernaam van de ontvanger het bericht. (Optioneel) |
| senderQualifier | Reeks | De kwalificatie van de partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Partner-id verzenden. (Verplicht) |
| receiverQualifier | Reeks | De kwalificatie van de partner ontvangt. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarop de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Uitwisselingscontrolenummer van de technische ontvangstbevestiging die worden ontvangen van partners. (Optioneel) |
| isaSegment | Reeks | Het ISA-segment voor de technische bevestiging dat ontvangen van partners. (Optioneel) |
| respondingInterchangeControlNumber |Reeks | Uitwisselingscontrolenummer voor de technische bevestiging dat ontvangen van partners. (Optioneel) |
| isMessageFailed | Booleaans | Of de X12 bericht is mislukt. (Verplicht) |
| statusCode | Enum | Uitwisseling statuscode bevestiging. Toegestane waarden zijn **geaccepteerde**, **afgewezen**, en **AcceptedWithErrors**. (Verplicht) |
| Verwerkingsstatusnaam | Enum | Bevestigingsstatus. Toegestane waarden zijn **ontvangen**, **gegenereerde**, en **verzonden**. (Verplicht) |
| 102 | Reeks | Uitwisseling datum. (Optioneel) |
| ta103 | Reeks | Uitwisseling tijd. (Optioneel) |
| ta105 | Reeks | Houd er rekening mee code uitwisselbaar. (Optioneel) |
||||

## <a name="x12-functional-group-tracking-schema"></a>X12 functionele groep volgschema 's

```json
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
```

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | X12 Partnernaam van de afzender het bericht. (Optioneel) |
| receiverPartnerName | Reeks | X12 Partnernaam van de ontvanger het bericht. (Optioneel) |
| senderQualifier | Reeks | De kwalificatie van de partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Partner-id verzenden. (Verplicht) |
| receiverQualifier | Reeks | De kwalificatie van de partner ontvangt. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarop de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | Uitwisselingscontrolenummer. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele controlenummer. (Optioneel) |
| gsSegment | Reeks | GS-berichtsegment. (Optioneel) |
| isTechnicalAcknowledgmentExpected | Booleaans | Of de technische bevestiging is geconfigureerd in de X12 overeenkomst. (Verplicht) |
| isFunctionalAcknowledgmentExpected | Booleaans | Of de functionele bevestiging is geconfigureerd in de X12 overeenkomst. (Verplicht) |
| isMessageFailed | Booleaans | Of de X12 bericht is mislukt. (Verplicht)|
| gs01 | Reeks | Functionele id-code. (Optioneel) |
| gs02 | Reeks | De code van de afzender van de toepassing. (Optioneel) |
| gs03 | Reeks | De code van de ontvanger van de toepassing. (Optioneel) |
| gs04 | Reeks | Functionele groep datum. (Optioneel) |
| gs05 | Reeks | Functionele groep tijd. (Optioneel) |
| gs07 | Reeks | Code die verantwoordelijk zijn Bureau. (Optioneel) |
| gs08 | Reeks | Release-versie/branche-id-code. (Optioneel) |
||||

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>X12 functionele groep bevestiging schema bijhouden

```json
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
```

| Eigenschap | Type | Beschrijving |
| --- | --- | --- |
| senderPartnerName | Reeks | X12 Partnernaam van de afzender het bericht. (Optioneel) |
| receiverPartnerName | Reeks | X12 Partnernaam van de ontvanger het bericht. (Optioneel) |
| senderQualifier | Reeks | De kwalificatie van de partner verzenden. (Verplicht) |
| senderIdentifier | Reeks | Partner-id verzenden. (Verplicht) |
| receiverQualifier | Reeks | De kwalificatie van de partner ontvangt. (Verplicht) |
| receiverIdentifier | Reeks | Partner-id ontvangen. (Verplicht) |
| agreementName | Reeks | Naam van de X12 overeenkomst waarop de berichten opgelost zijn. (Optioneel) |
| richting | Enum | Richting van de berichtenstroom ontvangen of verzenden. (Verplicht) |
| interchangeControlNumber | Reeks | De uitwisselingscontrolenummer gevuld voor de verzendkant wanneer een technische bevestiging is ontvangen van partners. (Optioneel) |
| functionalGroupControlNumber | Reeks | Functionele groepscontrolenummer van de technische acceptatie gevuld voor de verzendkant wanneer een technische bevestiging is ontvangen van partners. (Optioneel) |
| isaSegment | Reeks | Hetzelfde als uitwisseling aantal, maar alleen in bepaalde gevallen ingevuld bepalen. (Optioneel) |
| gsSegment | Reeks | Hetzelfde als functiegroep aantal, maar alleen in bepaalde gevallen ingevuld bepalen. (Optioneel) |
| respondingfunctionalGroupControlNumber | Reeks | Aantal van de oorspronkelijke functiegroep bepalen. (Optioneel) |
| respondingFunctionalGroupId | Reeks | In de functiegroep bevestiging is toegewezen aan AK101-id. (Optioneel) |
| isMessageFailed | Booleaans | Of de X12 bericht is mislukt. (Verplicht) |
| statusCode | Enum | Bevestiging statuscode. Toegestane waarden zijn **geaccepteerde**, **afgewezen**, en **AcceptedWithErrors**. (Verplicht) |
| Verwerkingsstatusnaam | Enum | De verwerkingsstatus van de bevestiging. Toegestane waarden zijn **ontvangen**, **gegenereerde**, en **verzonden**. (Verplicht) |
| ak903 | Reeks | Het aantal transactiesets ontvangen. (Optioneel) |
| ak904 | Reeks | Aantal transactiesets geaccepteerd in de geïdentificeerde functiegroep. (Optioneel) |
| ak9Segment | Reeks | Of de functiegroep geïdentificeerd in het segment AK1 wordt geaccepteerd of geweigerd en waarom. (Optioneel) |
|||| 

## <a name="b2b-protocol-tracking-schemas"></a>Volgschema's voor B2B-protocol

Zie voor informatie over B2B-protocol volgschema's:

* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [Aangepaste volgschema's B2B](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [B2B-berichten controleren](logic-apps-monitor-b2b-message.md).
* Meer informatie over [bijhouden van B2B-berichten in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
