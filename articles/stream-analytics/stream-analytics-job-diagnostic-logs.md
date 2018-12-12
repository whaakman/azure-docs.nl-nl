---
title: Problemen oplossen met behulp van logboeken met diagnostische gegevens van Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u diagnostische logboeken in Azure Stream Analytics analyseren.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: db3c9874676e3240f6896c1e1ff8f873360c20d5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090819"
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Azure Stream Analytics oplossen met behulp van logboeken met diagnostische gegevens

Een Azure Stream Analytics-taak stopt soms onverwacht verwerking. Het is belangrijk om te kunnen oplossen van dit type gebeurtenis. De gebeurtenis kan worden veroorzaakt door een onverwachte queryresultaat, door de verbinding met apparaten of door een onverwachte. De diagnostische logboeken in Stream Analytics kunt u de oorzaak van problemen identificeren wanneer ze zich voordoen en hersteltijd te verminderen.

## <a name="log-types"></a>Typen logboeken

Stream Analytics biedt twee typen logboeken: 
* [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (altijd ingeschakeld). Activiteitenlogboeken bieden inzicht in bewerkingen die worden uitgevoerd op taken.
* [Logboeken met diagnostische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (configureerbaar). Diagnoselogboeken bieden uitgebreidere inzicht in alles wat er met een taak gebeurt. Diagnostische logboeken starten wanneer de taak is gemaakt en einde van wanneer de taak wordt verwijderd. Deze gebeurtenissen van toepassing wanneer de taak wordt bijgewerkt en terwijl deze wordt uitgevoerd.

> [!NOTE]
> U kunt services zoals Azure Storage, Azure Event Hubs en Azure Log Analytics gebruiken om niet-overeenkomende gegevens te analyseren. U betaalt op basis van het prijsmodel voor deze services.
>

## <a name="turn-on-diagnostics-logs"></a>Logboeken met diagnostische gegevens inschakelen

Logboeken met diagnostische gegevens zijn **uit** standaard. Als u wilt inschakelen op Logboeken met diagnostische gegevens, de volgende stappen uit:

1.  Meld u aan bij Azure portal en gaat u naar het streaming job-blade. Onder **bewaking**, selecteer **diagnoselogboeken**.

    ![Navigatie in de blade naar Logboeken met diagnostische gegevens](./media/stream-analytics-job-diagnostic-logs/diagnostic-logs-monitoring.png)  

2.  Selecteer **diagnostische gegevens inschakelen**.

    ![Stream Analytics diagnostische logboeken inschakelen](./media/stream-analytics-job-diagnostic-logs/turn-on-diagnostic-logs.png)

3.  Op de **diagnostische instellingen** pagina voor **Status**, selecteer **op**.

    ![Status van logboeken met diagnostische gegevens wijzigen](./media/stream-analytics-job-diagnostic-logs/save-diagnostic-log-settings.png)

4.  De archivering doel (storage-account, event hub, Log Analytics) die u wilt instellen. Selecteer vervolgens de categorieën van logboeken die u wenst te verzamelen (kan worden uitgevoerd, ontwerp). 

5.  De nieuwe configuratie van de diagnostische gegevens op te slaan.

De configuratie van diagnostische duurt ongeveer 10 minuten worden doorgevoerd. Hierna start de logboeken worden weergegeven in de geconfigureerde archivering doel (u kunt deze zien op de **diagnoselogboeken** pagina):

![Navigatie in de blade voor diagnostische logboeken - archivering doelen](./media/stream-analytics-job-diagnostic-logs/view-diagnostic-logs-page.png)

Zie voor meer informatie over het configureren van diagnostische gegevens [verzamelen en gebruiken van diagnostische gegevens van uw Azure-resources](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Categorieën-diagnoselogboek

Op dit moment vastleggen we twee categorieën van logboeken met diagnostische gegevens:

* **Ontwerpen**. Legt gebeurtenissen die gerelateerd zijn aan de taak schrijven bewerkingen: taak-maken, toevoegen en verwijderen van invoer en uitvoer, toevoegen en bijwerken van de query, starten en stoppen van de taak.
* **Uitvoering**. Bevat gebeurtenissen die tijdens het uitvoeren van taak optreden:
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
properties | Meld u post-specifieke details, geserialiseerd als een JSON-tekenreeks. Zie de volgende secties voor meer informatie.

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
    * *Niet-overeenkomend gegevenstype tijdens gebeurtenis (de) serialiseren*: geeft het veld dat de fout wordt veroorzaakt.
    * *Een gebeurtenis, een ongeldige serialisatie kan niet worden gelezen*: bevat informatie over de locatie in de ingevoerde gegevens waar de fout is opgetreden. Blobnaam van de voor blob-invoer, offset en een voorbeeld van de gegevens bevat.
* **Gebeurtenissen verzenden**. Verzenden van gebeurtenissen tijdens schrijfbewerkingen. De streaming-gebeurtenis die de fout heeft veroorzaakt, worden geïdentificeerd.

### <a name="generic-events"></a>Algemene gebeurtenissen

Algemene gebeurtenissen dekken alles anders.

Name | Description
-------- | --------
Fout | (optioneel) Foutgegevens. Dit is doorgaans informatie over de uitzondering, als deze beschikbaar is.
Bericht| Logboekbericht.
Type | Het type van het bericht. Toegewezen aan interne classificatie van fouten. Bijvoorbeeld, **JobValidationError** of **BlobOutputAdapterInitializationFailure**.
Correlatie-id | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) die uniek voor het uitvoeren van taak. Alle vermeldingen in het uitvoeringslogboek vanaf het moment dat de taak wordt gestart totdat de taak stopt dezelfde hebben **correlatie-ID** waarde.

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Stream Analytics query language-referentie](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn835031.aspx)
