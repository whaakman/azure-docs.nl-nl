---
title: B2B-berichten in de Operations Management Suite - Azure Logic Apps bijhouden | Microsoft Docs
description: Bijhouden van de B2B-communicatie voor uw integratie-account en logic apps in Operations Management Suite (OMS) met Azure Log Analytics
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 128abd504785227c1f27debd329d46d358e6e516
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="track-b2b-communication-in-the-microsoft-operations-management-suite-oms"></a>Bijhouden van de B2B-communicatie in de Microsoft Operations Management Suite (OMS)

Na het instellen van B2B-communicatie tussen twee bedrijfsprocessen of toepassingen via uw account integratie met deze entiteiten berichten met elkaar kunnen uitwisselen. Om te controleren of deze berichten correct worden verwerkt, u AS2, X12, bijhouden kunt en EDIFACT met berichten [Azure Log Analytics](../log-analytics/log-analytics-overview.md) in de [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). U kunt bijvoorbeeld deze mechanismen voor het web gebaseerde bijhouden gebruiken voor het bijhouden van berichten:

* Aantal berichten en status
* Bevestigingen status
* Berichten correleren met bevestigingen
* Beschrijvingen van de gedetailleerde fouten voor mislukte
* Zoekmogelijkheden

## <a name="requirements"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Meer informatie over [het maken van een logische app](quickstart-create-first-logic-app-workflow.md) en [het instellen van logboekregistratie voor die app logica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integratie-account ingesteld met de controle en logboekregistratie. Meer informatie over [het maken van een account integratie](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [het instellen van controle en logboekregistratie voor dat account](../logic-apps/logic-apps-monitor-b2b-message.md).

* Als u dat nog niet gedaan hebt, [diagnostische gegevens publiceren met logboekanalyse](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) in OMS.

> [!NOTE]
> Nadat u de vorige vereisten hebt voldaan, moet er een werkruimte in de [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). U moet dezelfde OMS-werkruimte gebruiken voor het bijhouden van uw B2B-communicatie in OMS. 
>  
> Als u een OMS-werkruimte niet hebt, ontdek [het maken van een OMS-werkruimte](../log-analytics/log-analytics-get-started.md).

## <a name="add-the-logic-apps-b2b-solution-to-the-operations-management-suite-oms"></a>De oplossing Logic Apps B2B toevoegen aan de Operations Management Suite (OMS)

Als u wilt bijhouden B2B-berichten voor uw logische app OMS, moet u toevoegen de **Logic Apps B2B** oplossing naar de OMS-portal. Meer informatie over [oplossingen toe te voegen aan OMS](../log-analytics/log-analytics-get-started.md).

1. In de [Azure-portal](https://portal.azure.com), kies **meer Services**. Zoek naar 'log analytics' in en kies vervolgens **logboekanalyse** als volgt te werk:

   ![Log Analytics vinden](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. Onder **logboekanalyse**, zoeken en selecteert u de OMS-werkruimte. 

   ![Selecteer de OMS-werkruimte](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Onder **Management**, kies **OMS-Portal**.

   ![Kies de OMS-portal](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Nadat de OMS-startpagina wordt geopend, kiest u **galerie met oplossingen**.    

   ![Kies de galerie met oplossingen](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. Onder **alle oplossingen**, zoeken en kiest u **Logic Apps B2B**.     

   ![Logic Apps B2B kiezen](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. Onder **Logic Apps B2B**, kies **toevoegen**.

   ![Kies toevoegen](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

   Op de OMS-startpagina, de tegel voor **Logic Apps B2B-berichten** wordt nu weergegeven. 
   Deze tegel-het aantal berichten updates wanneer uw B2B-berichten worden verwerkt.

   ![OMS startpagina van de tegel Logic Apps B2B-berichten](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-the-operations-management-suite"></a>Berichtstatus en details in de Operations Management Suite bijhouden

1. Nadat uw B2B-berichten worden verwerkt, kunt u de status en details voor deze berichten weergeven. Kies op de startpagina OMS de **Logic Apps B2B-berichten** tegel.

   ![Aantal bijgewerkte berichten](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

   > [!NOTE]
   > Standaard de **Logic Apps B2B-berichten** tegel ziet u gegevens op basis van een enkele dag. Om het bereik van de gegevens wijzigen in een ander interval, kiest u het bereik-besturingselement aan de bovenkant van de OMS-pagina:
   > 
   > ![Bereik van de gegevens wijzigen](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)
   >

2. Na het bericht weergegeven dashboard status, kunt u meer details voor een specifieke bericht-type, waarin gegevens op basis van een enkele dag weergeven. Kies de tegel voor **AS2**, **X12**, of **EDIFACT**.

   ![Bericht-status weergeven](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Er wordt een lijst met berichten weergegeven voor uw gekozen tegel. 
   Zie voor meer informatie over de eigenschappen voor elk berichttype deze beschrijvingen van de eigenschap bericht:

   * [AS2-berichteigenschappen](#as2-message-properties)
   * [Berichteigenschappen die X12](#x12-message-properties)
   * [EDIFACT-berichteigenschappen](#EDIFACT-message-properties)

   Dit is wat een lijst met berichten AS2 als volgt uitzien:

   ![AS2-berichten weergeven](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Als u wilt weergeven of exporteren van de invoer en uitvoer voor specifieke berichten, die berichten te selecteren en kies **downloaden**. Wanneer u wordt gevraagd, sla het ZIP-bestand op uw lokale computer en pak vervolgens dat bestand. 

   De uitgepakte map bevat een map voor elk bericht dat is geselecteerd. 
   Als u bevestigingen hebt ingesteld, bevat de berichtenmap ook bestanden met details van de bevestiging. 
   Elke berichtenmap heeft ten minste deze bestanden: 
   
   * Leesbare bestanden met de nettolading van de invoer en uitvoer nettolading details
   * Gecodeerde bestanden met de invoer en uitvoer

   Voor elk berichttype kunt u de map en de naam bestandsindelingen hier vinden:

   * [AS2 indelingen voor mappen en bestanden](#as2-folder-file-names)
   * [X12 map- en naam indelingen](#x12-folder-file-names)
   * [EDIFACT indelingen voor mappen en bestanden](#edifact-folder-file-names)

   ![Berichtbestanden downloaden](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Om weer te geven uitvoeren alle acties die dezelfde ID, op de **logboek zoeken** pagina, kiest u een bericht uit de lijst met berichten.

   U kunt deze acties door de kolom, of zoeken naar specifieke resultaten sorteren.

   ![Bewerkingen met dezelfde ID worden uitgevoerd](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Zoekresultaten met vooraf gedefinieerde query's, kiest u **Favorieten**.

   * Meer informatie over [query's maken door filters toe te voegen](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Of u meer informatie over [het zoeken van gegevens met logboek zoekopdrachten in logboekanalyse](../log-analytics/log-analytics-log-searches.md).

   * Om de query wijzigt in het zoekvak, bijwerken van de query met de kolommen en waarden die u wilt gebruiken als filters.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Eigenschapbeschrijvingen en indelingen voor AS2, X 12 en EDIFACT-berichten

Voor elk berichttype hier worden de eigenschapbeschrijvingen en indelingen voor gedownloade bestanden.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>AS2-bericht eigenschapbeschrijvingen

Hier volgen beschrijvingen van de eigenschap voor elk bericht AS2.

| Eigenschap | Beschrijving |
| --- | --- |
| Afzender | De Gast partner opgegeven in **instellingen ontvangen**, of de partner van de host is opgegeven in **instellingen voor verzenden** voor een AS2-overeenkomst |
| Ontvanger | De partner host is opgegeven in **instellingen ontvangen**, of de partner Gast opgegeven **instellingen voor verzenden** voor een AS2-overeenkomst |
| Logische apps | De logische app waar de AS2-acties instellen |
| Status | De status van de AS2-bericht <br>Geslaagde = ontvangen of een geldige AS2-bericht verzonden. Er is geen MDN is ingesteld. <br>Geslaagde = ontvangen of een geldige AS2-bericht verzonden. MDN is ingesteld en ontvangen of MDN wordt verzonden. <br>Kan geen = een ongeldig AS2-bericht ontvangen. Er is geen MDN is ingesteld. <br>In behandeling = ontvangen of een geldige AS2-bericht verzonden. MDN is ingesteld en MDN wordt verwacht. |
| ACK | De status van het bericht MDN <br>Geaccepteerd = ontvangen of verzonden, een positieve MDN. <br>= Wachten op ontvangen of verzenden van een MDN in behandeling. <br>Afgewezen = ontvangen of een negatieve MDN verzonden. <br>Niet vereist = MDN is niet ingesteld in de overeenkomst. |
| Richting | De richting van de AS2-bericht |
| Correlatie-id | De ID die alle triggers en acties in een logische app correleert |
| Bericht-id | De AS2-bericht-ID van de berichtkoppen AS2 |
| Timestamp | De tijd waarop de actie AS2 het bericht verwerkt |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>AS2 indelingen voor gedownloade berichtbestanden

Hier vindt u de naam indelingen voor elke map voor gedownloade AS2 bericht en de bestanden.

| Bestand of map | Indeling van de |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Invoer, uitvoer en instellen van de bevestiging bestanden als | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_input_payload.txt </p>**De nettolading van de uitvoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 bericht eigenschapbeschrijvingen

Hier volgen beschrijvingen van de eigenschap voor elke X12 bericht.

| Eigenschap | Beschrijving |
| --- | --- |
| Afzender | De Gast partner opgegeven in **instellingen ontvangen**, of de partner van de host is opgegeven in **instellingen voor verzenden** voor een X12 overeenkomst |
| Ontvanger | De partner host is opgegeven in **instellingen ontvangen**, of de partner Gast opgegeven **instellingen voor verzenden** voor een X12 overeenkomst |
| Logische apps | De logische app waar de X12 acties zijn ingesteld |
| Status | De X12 bericht-status <br>Geslaagde = ontvangen of een geldige X12 verzonden bericht. Er zijn geen functionele ack is ingesteld. <br>Geslaagde = ontvangen of een geldige X12 verzonden bericht. Functionele ack is ingesteld en ontvangen, of een functionele ack wordt verzonden. <br>Kan niet ontvangen = of een ongeldige X12 verzonden bericht. <br>In behandeling = ontvangen of een geldige X12 verzonden bericht. Functionele ack is ingesteld en een functionele ack wordt verwacht. |
| ACK | Functionele Ack (997)-status <br>Geaccepteerd = ontvangen of een positief functionele bevestiging verzonden <br>Afgewezen = ontvangen of een negatieve functionele bevestiging verzonden <br>In behandeling zijnde = een functionele ack verwacht maar niet ontvangen. <br>In behandeling zijnde = gegenereerd een functionele ack maar kan niet verzenden naar partner. <br>Niet vereist = functionele ack is niet ingesteld. |
| Richting | De X12 bericht richting |
| Correlatie-id | De ID die alle triggers en acties in een logische app correleert |
| Bericht-type | Het berichttype voor EDI X 12 |
| ICN | Het besturingselement Interchange nummer voor de X12 bericht |
| TSCN | Het transactie ingesteld besturingselement nummer voor de X12 bericht |
| Timestamp | De tijd wanneer de X12 actie verwerkt het bericht |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 naam indelingen voor gedownloade berichtbestanden

Hier vindt u de naam indelingen voor elk X12 gedownload bericht map en bestanden.

| Bestand of map | Indeling van de |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Invoer, uitvoer en instellen van de bevestiging bestanden als | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_X12\_[controle-DIF-aantal]\_input_payload.txt </p>**De nettolading van de uitvoer**: [afzender]\_[ontvanger]\_X12\_[controle-DIF-aantal]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_X12\_[controle-DIF-aantal]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_X12\_[controle-DIF-aantal]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT bericht eigenschapbeschrijvingen

Hier volgen beschrijvingen van de eigenschap voor elk bericht EDIFACT.

| Eigenschap | Beschrijving |
| --- | --- |
| Afzender | De Gast partner opgegeven in **instellingen ontvangen**, of de partner van de host is opgegeven in **instellingen voor verzenden** voor een overeenkomst EDIFACT |
| Ontvanger | De partner host is opgegeven in **instellingen ontvangen**, of de partner Gast opgegeven **instellingen voor verzenden** voor een overeenkomst EDIFACT |
| Logische apps | De logische app waar de acties EDIFACT instellen |
| Status | De status van het bericht EDIFACT <br>Geslaagde = ontvangen of een geldige EDIFACT-bericht verzonden. Er zijn geen functionele ack is ingesteld. <br>Geslaagde = ontvangen of een geldige EDIFACT-bericht verzonden. Functionele ack is ingesteld en ontvangen, of een functionele ack wordt verzonden. <br>Kan niet ontvangen = of een ongeldig EDIFACT-bericht verzonden <br>In behandeling = ontvangen of een geldige EDIFACT-bericht verzonden. Functionele ack is ingesteld en een functionele ack wordt verwacht. |
| ACK | Functionele Ack (997)-status <br>Geaccepteerd = ontvangen of een positief functionele bevestiging verzonden <br>Afgewezen = ontvangen of een negatieve functionele bevestiging verzonden <br>In behandeling zijnde = een functionele ack verwacht maar niet ontvangen. <br>In behandeling zijnde = gegenereerd een functionele ack maar kan niet verzenden naar partner. <br>Niet vereist = functionele Ack is niet ingesteld. |
| Richting | De richting van het bericht EDIFACT |
| Correlatie-id | De ID die alle triggers en acties in een logische app correleert |
| Bericht-type | Het berichttype EDIFACT |
| ICN | Het besturingselement Interchange nummer voor het bericht EDIFACT |
| TSCN | Het transactie ingesteld besturingselement nummer voor het bericht EDIFACT |
| Timestamp | De tijd waarop de actie EDIFACT het bericht verwerkt |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>EDIFACT indelingen voor gedownloade berichtbestanden

Hier vindt u de naam indelingen voor elke map voor gedownloade EDIFACT bericht en de bestanden.

| Bestand of map | Indeling van de |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Invoer, uitvoer en instellen van de bevestiging bestanden als | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-DIF-aantal]\_input_payload.txt </p>**De nettolading van de uitvoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-DIF-aantal]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-DIF-aantal]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-DIF-aantal]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Volgende stappen

* [Query voor B2B-berichten in de Operations Management Suite](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema’s voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Bijhouden van aangepaste schema 's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)