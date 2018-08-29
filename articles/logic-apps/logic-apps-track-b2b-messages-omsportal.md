---
title: Met Azure Log Analytics - Azure Logic Apps B2B-berichten bijhouden | Microsoft Docs
description: Bijhouden van de B2B-communicatie voor integratieaccounts en Azure Logic Apps met Azure Log Analytics
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 06/19/2018
ms.openlocfilehash: 5bf5385824eb9b711a2fee547c29d24d7ef5a01d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125765"
---
# <a name="track-b2b-communication-with-azure-log-analytics"></a>Bijhouden van de B2B-communicatie met Azure Log Analytics

Na het instellen van B2B-communicatie tussen twee met zakelijke processen of toepassingen via uw integratie-account, deze entiteiten berichten met elkaar kunnen uitwisselen. Om te controleren of deze berichten correct worden verwerkt, u AS2, X12, bijhouden kunt en EDIFACT-met berichten [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Bijvoorbeeld, kunt u deze bijhouden op basis van een web-mogelijkheden voor het bijhouden van berichten:

* Aantal berichten en status
* Status van bevestigingen
* Berichten correleren met bevestigingen
* Beschrijvingen van de gedetailleerde fouten voor mislukte aanmeldingen
* Zoekopties

## <a name="requirements"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Informatie over [over het maken van een logische app](quickstart-create-first-logic-app-workflow.md) en [over het instellen van logboekregistratie voor deze logische app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integratie-account ingesteld met bewaking en logboekregistratie. Informatie over [over het maken van een integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [over het instellen van controle en logboekregistratie voor dat account](../logic-apps/logic-apps-monitor-b2b-message.md).

* Als u dat nog niet gedaan hebt, [diagnostische gegevens publiceren naar Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Nadat u de bovenstaande vereisten hebt voldaan, moet u een werkruimte van Log Analytics hebben. U moet dezelfde werkruimte gebruiken voor het bijhouden van uw B2B-communicatie in Log Analytics. 
>  
> Als u een Log Analytics-werkruimte hebt, krijgt u informatie [over het maken van een Log Analytics-werkruimte](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="add-the-logic-apps-b2b-solution-to-log-analytics"></a>De Logic Apps B2B-oplossing toevoegen aan Log Analytics

Als u Log Analytics voor uw logische app B2B-berichten bijhouden, moet u toevoegen de **Logic Apps B2B** oplossing naar de OMS-portal. Meer informatie over [oplossingen toe te voegen aan Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

1. In de [Azure-portal](https://portal.azure.com), kiest u **alle Services**. Zoek naar "log analytics" en kies vervolgens **Log Analytics** zoals hier wordt weergegeven:

   ![Log Analytics zoeken](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte. 

   ![Selecteer uw Log Analytics-werkruimte](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Onder **Management**, kiest u **overzicht**.

   ![Kies de Log Analytics-portal](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Nadat de startpagina wordt geopend, kiest u **toevoegen** voor het installeren van logische Apps B2B-oplossing.    
   ![Galerie van oplossingen kiezen](media/logic-apps-track-b2b-messages-omsportal/add-b2b-solution.png)

5. Onder **beheeroplossingen**, zoeken en maken **Logic Apps B2B** oplossing.     
   ![Kies van logische Apps B2B](media/logic-apps-track-b2b-messages-omsportal/create-b2b-solution.png)

   Op de startpagina van de tegel voor **Logic Apps B2B-berichten** wordt nu weergegeven. 
   Deze tegel werkt het aantal berichten wanneer uw B2B-berichten worden verwerkt.

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-log-analytics"></a>Status van het bericht bijhouden en details in Log Analytics

1. Nadat uw B2B-berichten worden verwerkt, kunt u de status en details voor deze berichten bekijken. Kies op de pagina overzicht de **Logic Apps B2B-berichten** tegel.

   ![Aantal bijgewerkte berichten](media/logic-apps-track-b2b-messages-omsportal/b2b-overview-tile.png)

   > [!NOTE]
   > Standaard de **Logic Apps B2B-berichten** tegel ziet u gegevens op basis van een enkele dag. Als u wilt het bereik van gegevens naar een ander interval wijzigen, kies de bereik-besturingselement aan de bovenkant van de pagina:
   > 
   > ![Bereik van de gegevens wijzigen](media/logic-apps-track-b2b-messages-omsportal/server-filter.png)
   >

2. Na het bericht wordt statusdashboard weergegeven, vindt u meer informatie over een specifieke berichttype met de gegevens op basis van een enkele dag. Kies de tegel voor **AS2**, **X12**, of **EDIFACT**.

   ![Bericht-status weergeven](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Er wordt een lijst met berichten weergegeven voor uw gekozen tegel. 
   Zie voor meer informatie over de eigenschappen voor elk berichttype, deze beschrijvingen van de eigenschap bericht:

   * [Eigenschappen van de AS2-berichten](#as2-message-properties)
   * [Berichteigenschappen die X12](#x12-message-properties)
   * [Eigenschappen van de EDIFACT-berichten](#EDIFACT-message-properties)

   Dit is wat een AS2-bericht-lijst eruit kan:

   ![AS2-berichten weergeven](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Als u wilt weergeven of exporteren van de invoer en uitvoer voor specifieke berichten, selecteert u deze berichten en kies **downloaden**. Wanneer u wordt gevraagd, sla het ZIP-bestand op uw lokale computer en pak vervolgens dat bestand. 

   De uitgepakte map bevat een map voor elk geselecteerde bericht. 
   Als u van bevestigingen ingesteld, wordt in de berichtenmap ook bestanden met de bevestiging details bevat. 
   De berichtenmap voor elk heeft ten minste deze bestanden: 
   
   * Leesbare bestanden met de nettolading van de invoer en uitvoer nettolading details
   * Gecodeerde bestanden met de invoer en uitvoer

   Voor elk berichttype, kunt u de map en de naam bestandsindelingen hier vinden:

   * [AS2-map en elk bestand naam indelingen](#as2-folder-file-names)
   * [X12 map en elk bestand naam indelingen](#x12-folder-file-names)
   * [EDIFACT-map en elk bestand naam indelingen](#edifact-folder-file-names)

   ![Bestanden downloaden](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Om weer te geven Voer alle acties die dezelfde ID, op de **zoeken in logboeken** pagina, kiest u een bericht uit de lijst met berichten.

   U kunt deze acties door de kolom, of zoeken naar specifieke resultaten sorteren.

   ![Bewerkingen met dezelfde ID worden uitgevoerd](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Zoekresultaten met vooraf gedefinieerde query's, kies **Favorieten**.

   * Informatie over [over het bouwen van query's door filters toe te voegen](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Of lees meer over [over het vinden van gegevens met zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-searches.md).

   * Als u wilt wijzigen van query's in het zoekvak, werk de query met de kolommen en waarden die u wilt gebruiken als filters.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>De eigenschapbeschrijvingen en Naamindelingen voor AS2, X 12 en EDIFACT-berichten

Voor elk berichttype hier worden de eigenschapbeschrijvingen en typen Naamindelingen voor gedownloade bestanden.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Eigenschapbeschrijvingen AS2-bericht

Hier volgen eigenschapbeschrijvingen voor elk AS2-bericht.

| Eigenschap | Beschrijving |
| --- | --- |
| Afzender | De gastpartner die is opgegeven in **instellingen ontvangen**, of de hostpartner die is opgegeven in **instellingen voor verzenden** voor een AS2-overeenkomst |
| Ontvanger | De hostpartner die is opgegeven in **instellingen ontvangen**, of de gastpartner is opgegeven **instellingen voor verzenden** voor een AS2-overeenkomst |
| Logische apps | De logische app waar de AS2-acties zijn ingesteld |
| Status | De status van de AS2-bericht <br>Geslaagd = ontvangen of een geldige AS2-bericht verzonden. Er is geen MDN is ingesteld. <br>Geslaagd = ontvangen of een geldige AS2-bericht verzonden. MDN is ingesteld en ontvangen, of MDN wordt verzonden. <br>Kan geen = een ongeldige AS2-bericht ontvangen. Er is geen MDN is ingesteld. <br>In behandeling = ontvangen of een geldige AS2-bericht verzonden. MDN is ingesteld en MDN wordt verwacht. |
| ACK | De status van het MDN-bericht <br>Geaccepteerd = ontvangen of een positief MDN verzonden. <br>In afwachting van = wachten ontvangen of een MDN te verzenden. <br>Afgewezen = ontvangen of een negatieve MDN verzonden. <br>Niet vereist = MDN is niet ingesteld in de overeenkomst. |
| Richting | De richting van de AS2-bericht |
| Correlatie-id | De ID die overeenkomt met alle triggers en acties in een logische app |
| Bericht-id | De AS2-bericht-ID van de AS2-bericht-headers |
| Tijdstempel | De tijd wanneer het bericht worden verwerkt door de AS2-actie |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>AS2 typen Naamindelingen voor gedownloade bestanden

Hier volgen de typen Naamindelingen voor elke map voor gedownloade AS2-bericht en de bestanden.

| Bestand of map | Naamindeling |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Invoer en uitvoer als instellen, bevestiging bestanden | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_input_payload.txt </p>**Uitvoerpayload**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 message eigenschapbeschrijvingen

Hier vindt u de eigenschapbeschrijvingen voor elke X12 bericht.

| Eigenschap | Beschrijving |
| --- | --- |
| Afzender | De gastpartner die is opgegeven in **instellingen ontvangen**, of de hostpartner die is opgegeven in **instellingen voor verzenden** voor een X12 overeenkomst |
| Ontvanger | De hostpartner die is opgegeven in **instellingen ontvangen**, of de gastpartner is opgegeven **instellingen voor verzenden** voor een X12 overeenkomst |
| Logische apps | De logische app waar de X12 acties zijn ingesteld |
| Status | De X12 status message <br>Geslaagd = ontvangen of een geldige X12 verzonden bericht. Er zijn geen functionele ack is ingesteld. <br>Geslaagd = ontvangen of een geldige X12 verzonden bericht. Functionele ack is ingesteld en ontvangen, of een functionele ack wordt verzonden. <br>Kan geen = ontvangen of een ongeldige X12 verzonden bericht. <br>In behandeling = ontvangen of een geldige X12 verzonden bericht. Functionele ack is ingesteld en een functionele ack wordt verwacht. |
| ACK | Status van functionele Ack (een 997-bevestiging) <br>Geaccepteerd = ontvangen of een positief functionele ack. verzonden <br>Afgewezen = ontvangen of een negatieve functionele ack. verzonden <br>In afwachting van = een functionele ack verwacht maar niet ontvangen. <br>In behandeling zijnde = gegenereerd een functionele ack maar niet verzenden naar partner. <br>Niet vereist = functionele ack is niet ingesteld. |
| Richting | De X12 bericht richting |
| Correlatie-id | De ID die overeenkomt met alle triggers en acties in een logische app |
| Berichttype | Het berichttype voor EDI, X 12 |
| ICN | De Uitwisselingscontrolenummer voor de X12 bericht |
| TSCN | De transactie ingesteld controlenummer voor de X12 bericht |
| Tijdstempel | De tijd wanneer de X12 actie verwerkt het bericht |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 naam indelingen voor gedownloade bestanden

Hier vindt u de van Naamindelingen voor elk X12 gedownload bericht map en bestanden.

| Bestand of map | Naamindeling |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Invoer en uitvoer als instellen, bevestiging bestanden | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_input_payload.txt </p>**Uitvoerpayload**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT-bericht-eigenschapbeschrijvingen

Hier volgen eigenschapbeschrijvingen voor elk EDIFACT-bericht.

| Eigenschap | Beschrijving |
| --- | --- |
| Afzender | De gastpartner die is opgegeven in **instellingen ontvangen**, of de hostpartner die is opgegeven in **instellingen voor verzenden** voor een EDIFACT-overeenkomst |
| Ontvanger | De hostpartner die is opgegeven in **instellingen ontvangen**, of de gastpartner is opgegeven **instellingen voor verzenden** voor een EDIFACT-overeenkomst |
| Logische apps | De logische app waar de EDIFACT-acties zijn ingesteld |
| Status | De status van de EDIFACT-bericht <br>Geslaagd = ontvangen of een geldige EDIFACT-bericht verzonden. Er zijn geen functionele ack is ingesteld. <br>Geslaagd = ontvangen of een geldige EDIFACT-bericht verzonden. Functionele ack is ingesteld en ontvangen, of een functionele ack wordt verzonden. <br>Kan geen = ontvangen of heeft een ongeldige EDIFACT-bericht verzonden <br>In behandeling = ontvangen of een geldige EDIFACT-bericht verzonden. Functionele ack is ingesteld en een functionele ack wordt verwacht. |
| ACK | Status van functionele Ack (een 997-bevestiging) <br>Geaccepteerd = ontvangen of een positief functionele ack. verzonden <br>Afgewezen = ontvangen of een negatieve functionele ack. verzonden <br>In afwachting van = een functionele ack verwacht maar niet ontvangen. <br>In behandeling zijnde = gegenereerd een functionele ack maar niet verzenden naar partner. <br>Niet vereist = functionele Ack is niet ingesteld. |
| Richting | De richting van de EDIFACT-bericht |
| Correlatie-id | De ID die overeenkomt met alle triggers en acties in een logische app |
| Berichttype | Het type EDIFACT-bericht |
| ICN | De Uitwisselingscontrolenummer voor de EDIFACT-bericht |
| TSCN | De transactie ingesteld controlenummer voor de EDIFACT-bericht |
| Tijdstempel | De tijd waarop de EDIFACT-actie het bericht verwerkt |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>EDIFACT typen Naamindelingen voor gedownloade bestanden

Hier volgen de typen Naamindelingen voor elke map voor gedownloade EDIFACT-bericht en de bestanden.

| Bestand of map | Naamindeling |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Invoer en uitvoer als instellen, bevestiging bestanden | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_input_payload.txt </p>**Uitvoerpayload**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Volgende stappen

* [Query voor B2B-berichten in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema’s voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Aangepaste volgschema 's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)