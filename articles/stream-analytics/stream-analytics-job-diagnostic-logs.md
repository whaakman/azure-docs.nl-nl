---
title: Problemen oplossen met behulp van logboeken met diagnostische gegevens van Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u diagnostische logboeken in Azure Stream Analytics analyseren.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/19/2019
ms.custom: seodec18
ms.openlocfilehash: 34f994bfca8bdeaffde6732572f47aeaa86b2ac5
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54818928"
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Azure Stream Analytics oplossen met behulp van logboeken met diagnostische gegevens

Een Azure Stream Analytics-taak stopt soms onverwacht verwerking. Het is belangrijk om dit type gebeurtenissen te kunnen oplossen. Fouten kunnen worden veroorzaakt door een onverwachte queryresultaat, door de verbinding met apparaten of door een onverwachte. De diagnostische logboeken in Stream Analytics kunt u de oorzaak van problemen identificeren wanneer ze zich voordoen en hersteltijd te verminderen.

## <a name="log-types"></a>Typen logboeken

Stream Analytics biedt twee typen logboeken:

* [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (altijd ingeschakeld), die inzicht in bewerkingen die worden uitgevoerd op taken geven.

* [Logboeken met diagnostische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (configureerbaar), die uitgebreider inzicht geven in alles dat gebeurt met een taak. Diagnostische logboeken start wanneer de taak is gemaakt en einde van wanneer de taak wordt verwijderd. Deze gebeurtenissen van toepassing wanneer de taak wordt bijgewerkt en terwijl deze wordt uitgevoerd.

> [!NOTE]
> U kunt services zoals Azure Storage, Azure Event Hubs en Azure Log Analytics gebruiken om niet-overeenkomende gegevens te analyseren. U betaalt op basis van het prijsmodel voor deze services.

## <a name="debugging-using-activity-logs"></a>Foutopsporing met behulp van activiteit-Logboeken

Activiteitenlogboeken zijn standaard ingeschakeld en geef op hoog niveau inzicht in bewerkingen die worden uitgevoerd door de Stream Analytics-taak. Gegevens die aanwezig zijn in de activiteitenlogboeken kan helpen de hoofdoorzaak van de problemen die invloed hebben op uw taak. Voer de volgende stappen uit voor het gebruik van activiteitenlogboeken in Stream Analytics:

1. Aanmelden bij de Azure portal en selecteer **activiteitenlogboek** onder **overzicht**.

   ![Stream Analytics-activiteitenlogboek](./media/stream-analytics-job-diagnostic-logs/stream-analytics-menu.png)

2. U ziet een lijst met bewerkingen die zijn uitgevoerd. Elke bewerking waardoor de mislukt taak heeft een bel rode info.

3. Klik op een bewerking om de samenvattingsweergave te openen. Hier is vaak beperkt. Meer informatie over de werking, klikt u op **JSON**.

   ![Stream Analytics activiteit bewerking samenvatting log](./media/stream-analytics-job-diagnostic-logs/operation-summary.png)

4. Schuif omlaag naar de **eigenschappen** sectie van de JSON, waarmee u details van de fout waardoor de mislukte bewerking. In dit voorbeeld wordt de uitvoering is mislukt met een runtime-fout zelfs waarden voor breedtegraad gebonden.

   ![Details van de JSON-fout](./media/stream-analytics-job-diagnostic-logs/error-details.png)

5. U kunt nemen corrigerende acties op basis van het foutbericht in JSON. In dit voorbeeld wordt gecontroleerd om ervoor te zorgen Breedtegraadwaarde ligt tussen-90 graden en 90 graden moeten worden toegevoegd aan de query.

6. Als het foutbericht in de activiteitenlogboeken niet handig zijn bij het identificeren van de hoofdoorzaak te achterhalen, diagnostische logboeken inschakelen en Log Analytics gebruiken.

## <a name="send-diagnostics-to-log-analytics"></a>Diagnostische gegevens verzenden naar Log Analytics

Inschakelen van logboeken met diagnostische gegevens en deze te verzenden naar Log Analytics wordt sterk aanbevolen. Logboeken met diagnostische gegevens zijn **uit** standaard. Als u wilt inschakelen op Logboeken met diagnostische gegevens, de volgende stappen uit:

1.  Meld u aan bij Azure portal en navigeer naar uw Stream Analytics-taak. Onder **bewaking**, selecteer **diagnoselogboeken**. Selecteer vervolgens **diagnostische gegevens inschakelen**.

    ![Navigatie in de blade naar Logboeken met diagnostische gegevens](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs-monitoring.png)  

2.  Maak een **naam** in **diagnostische instellingen** en schakel het selectievakje in naast **verzenden naar Log Analytics**. Een bestaande toevoegen of maak een nieuwe **Log analytics-werkruimte**. Schakel de selectievakjes voor **uitvoering** en **ontwerpen** onder **LOG**, en **AllMetrics** onder **METRIC** . Klik op **Opslaan**.

    ![Instellingen voor diagnostische logboeken](./media/stream-analytics-job-diagnostic-logs/diagnostic-settings.png)

3. Wanneer uw Stream Analytics-taak wordt gestart, worden de logboeken met diagnostische gegevens doorgestuurd naar uw Log Analytics-werkruimte. Navigeer naar de Log Analytics-werkruimte en kies **logboeken** onder de **algemene** sectie.

   ![Log Analytics-Logboeken onder algemene sectie](./media/stream-analytics-job-diagnostic-logs/log-analytics-logs.png)

4. U kunt [Schrijf uw eigen query](../azure-monitor/log-query/get-started-portal.md) om te zoeken naar termen, trends te identificeren, patronen analyseren en inzichten op basis van uw gegevens te kunnen bieden. Bijvoorbeeld, kunt u een query schrijven om te filteren alleen diagnostische logboeken waarvoor het bericht 'de streaming-taak is mislukt.' Diagnostische logboeken van Azure Stream Analytics worden opgeslagen in de **AzureDiagnostics** tabel.

   ![Diagnostische gegevens over query's en resultaten](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs-query.png)

5. Wanneer u een query waarin wordt gezocht naar de juiste logboeken hebt, sla deze door te selecteren **opslaan** en geef een naam en categorie. Vervolgens kunt u een waarschuwing maken door te selecteren **nieuwe waarschuwingsregel**. Daarna geeft u een voorwaarde voor de waarschuwing. Selecteer **voorwaarde** en voer de waarde voor drempel en de frequentie waarmee deze aangepaste zoekopdrachten in Logboeken wordt geëvalueerd.  

   ![Diagnostische logboeken zoekquery](./media/stream-analytics-job-diagnostic-logs/search-query.png)

6. Kies de actiegroep en geef Waarschuwingsdetails, zoals naam en beschrijving, voordat u de waarschuwingsregel kunt maken. U kunt de diagnostische logboeken van verschillende taken versturen naar de dezelfde werkruimte van Log Analytics. Hiermee kunt u waarschuwingen instellen wanneer dat in alle projecten werkt.  

## <a name="diagnostics-log-categories"></a>Categorieën-diagnoselogboek

Op dit moment vastleggen we twee categorieën van logboeken met diagnostische gegevens:

* **Ontwerpen**: Gebeurtenissen die betrekking hebben op de taak schrijven bewerkingen, zoals het maken van de taak, toevoegen en verwijderen van invoer en uitvoer, toe te voegen en het bijwerken van de query en het starten of stoppen van de taak bevat.

* **Uitvoering**: Bevat gebeurtenissen die tijdens het uitvoeren van taak optreden.
    * Fouten in de basisnetwerkverbinding
    * Fouten van gegevensverwerking, met inbegrip van:
        * Gebeurtenissen die niet aan de definitie van de query (niet-overeenkomende veldtypen en waarden, ontbrekende velden, enzovoort voldoen)
        * Expressie evaluatie fouten
    * Andere gebeurtenissen en fouten

## <a name="diagnostics-logs-schema"></a>Diagnostics-schema voor logboeken

Alle logboeken worden opgeslagen in de JSON-indeling. Elk item heeft de volgende algemene tekenreeksvelden:

Name | Description
------- | -------
time | De tijdstempel (in UTC) van het logboek.
resourceId | ID van de resource die de bewerking plaatsgevonden, in hoofdletters heeft. Hiertoe behoren de abonnements-ID, de resourcegroep en de taaknaam. Bijvoorbeeld,   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB**.
category | Meld u categorie, ofwel **uitvoering** of **ontwerpen**.
operationName | Naam van de bewerking die wordt vastgelegd. Bijvoorbeeld, **gebeurtenissen verzenden: SQL-uitvoer schrijven fout naar mysqloutput**.
status | Status van de bewerking. Bijvoorbeeld, **mislukt** of **geslaagd**.
niveau | Logboek-niveau. Bijvoorbeeld, **fout**, **waarschuwing**, of **ter informatie**.
properties | Meld u post-specifieke details, geserialiseerd als een JSON-tekenreeks. Zie de volgende secties in dit artikel voor meer informatie.

### <a name="execution-log-properties-schema"></a>Schema voor uitvoering van logboek-eigenschappen

Logboeken van taakuitvoeringen bevat gegevens over gebeurtenissen die hebben plaatsgevonden tijdens het uitvoeren van Stream Analytics-taak. Het schema van de eigenschappen van varieert, afhankelijk van het type gebeurtenis. Op dit moment hebben we de volgende typen uitvoeringslogboeken:

### <a name="data-errors"></a>Fouten met gegevens

Elke fout die optreedt terwijl de taak is verwerken van gegevens is in deze categorie van Logboeken. Deze logboeken worden gemaakt tijdens het lezen, gegevens serialisatie, meestal als schrijfbewerkingen. Deze logboeken bevatten geen fouten in de basisnetwerkverbinding. Fouten in de basisnetwerkverbinding worden behandeld als algemene gebeurtenissen.

Name | Description
------- | -------
Bron | Naam van de taak invoer of uitvoer waar de fout is opgetreden.
Bericht | Bericht met betrekking tot de fout.
Type | Het type fout. Bijvoorbeeld, **DataConversionError**, **CsvParserError**, of **ServiceBusPropertyColumnMissingError**.
Gegevens | Bevat gegevens die is handig om nauwkeurig de bron van de fout te vinden. Kan worden afgekapt, afhankelijk van de grootte.

Afhankelijk van de **operationName** waarde, fouten met gegevens hebben de volgende schema:
* **Gebeurtenissen serialiseren**. Serialiseren gebeurtenissen plaatsvinden tijdens leesbewerkingen-gebeurtenis. Ze zich voordoen wanneer de gegevens op de invoer niet voldoet aan de Queryschema voor een van de volgende redenen:
    * *Niet-overeenkomend gegevenstype tijdens gebeurtenis (de) serialiseren*: Hiermee geeft u het veld dat de fout wordt veroorzaakt.
    * *Een gebeurtenis, een ongeldige serialisatie kan niet worden gelezen*: Bevat informatie over de locatie in de ingevoerde gegevens waar de fout is opgetreden. Blobnaam van de voor blob-invoer, offset en een voorbeeld van de gegevens bevat.
* **Gebeurtenissen verzenden**. Verzenden van gebeurtenissen tijdens schrijfbewerkingen. De streaming-gebeurtenis die de fout heeft veroorzaakt, worden geïdentificeerd.

### <a name="generic-events"></a>Algemene gebeurtenissen

Algemene gebeurtenissen dekken alles anders.

Name | Description
-------- | --------
Fout | (optioneel) Foutgegevens. Dit is doorgaans informatie over de uitzondering als deze beschikbaar is.
Bericht| Logboekbericht.
Type | Het type van het bericht. Toegewezen aan interne classificatie van fouten. Bijvoorbeeld, **JobValidationError** of **BlobOutputAdapterInitializationFailure**.
Correlatie-id | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) die uniek voor het uitvoeren van taak. Alle vermeldingen in het uitvoeringslogboek vanaf het moment dat de taak wordt gestart totdat de taak stopt dezelfde hebben **correlatie-ID** waarde.

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Stream Analytics query language-referentie](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn835031.aspx)
