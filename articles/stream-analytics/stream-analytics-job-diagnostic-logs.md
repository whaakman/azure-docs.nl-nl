---
title: Azure Stream Analytics oplossen met diagnostische logboeken | Microsoft Docs
description: Informatie over het analyseren van diagnostische logboeken van de Stream Analytics-taken in Microsoft Azure.
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: c9772df2c216d465ca6e90e69bce011969dd4f02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Azure Stream Analytics oplossen met behulp van logboeken met diagnostische gegevens

In sommige gevallen kan stopt een Azure Stream Analytics-taak onverwacht verwerking. Het is belangrijk om te kunnen oplossen van dit soort gebeurtenis. De gebeurtenis kan worden veroorzaakt door een onverwachte queryresultaat met connectiviteit voor apparaten, of door een onverwachte. De diagnostische logboeken in Stream Analytics kunt u de oorzaak van problemen identificeren wanneer ze optreden, en de hersteltijd reduceren.

## <a name="log-types"></a>Logboek-typen

Stream Analytics biedt twee typen logboeken: 
* [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (altijd ingeschakeld). Activiteitenlogboeken geven inzicht in de bewerkingen die worden uitgevoerd op taken.
* [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (configureerbaar). Diagnostische logboeken bieden meer inzicht in alles wat er met een taak gebeurt. Diagnostische logboeken begin wanneer de taak is gemaakt en einde wanneer de taak is verwijderd. Ze betrekking op gebeurtenissen wanneer de taak wordt bijgewerkt en terwijl deze wordt uitgevoerd.

> [!NOTE]
> U kunt services zoals Azure Storage, Azure Event Hubs en Azure Log Analytics gebruiken om niet-overeenkomend gegevens te analyseren. Er worden in rekening gebracht op basis van het prijsmodel voor deze services.
>

## <a name="turn-on-diagnostics-logs"></a>Logboeken met diagnostische gegevens inschakelen

Diagnostische logboeken zijn **uit** standaard. Als u logboeken met diagnostische gegevens, moet u deze stappen voltooien:

1.  Aanmelden bij de Azure-portal en gaat u naar de streaming job-blade. Onder **bewaking**, selecteer **diagnostische logboeken**.

    ![Blade navigatie naar Logboeken met diagnostische gegevens](./media/stream-analytics-job-diagnostic-logs/image1.png)  

2.  Selecteer **diagnostische gegevens inschakelen**.

    ![Logboeken met diagnostische gegevens inschakelen](./media/stream-analytics-job-diagnostic-logs/image2.png)

3.  Op de **diagnostische instellingen** pagina voor **Status**, selecteer **op**.

    ![Status van diagnostische logboeken wijzigen](./media/stream-analytics-job-diagnostic-logs/image3.png)

4.  De archivering doel (storage-account, event hub, logboekanalyse) dat u wilt instellen. Selecteer de categorieën van logboeken die u wenst te verzamelen (uitvoering, ontwerpen). 

5.  Sla de nieuwe configuratie van diagnostische gegevens.

De configuratie van diagnostische duurt ongeveer 10 minuten kracht te laten worden. De logboeken start na dat wordt weergegeven in de geconfigureerde archivering doel (ziet u deze naar de **diagnostische logboeken** pagina):

![Blade navigatie naar diagnostische logboeken - archivering doelen](./media/stream-analytics-job-diagnostic-logs/image4.png)

Zie voor meer informatie over het configureren van diagnostische gegevens [verzamelen en gebruiken van diagnostische gegevens van uw Azure-resources](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Meld u diagnostische gegevens categorieën

Op dit moment kunnen vastleggen we twee categorieën van diagnostische logboeken:

* **Ontwerpen**. Opnamen gebeurtenissen die gerelateerd zijn aan de taak authoring operations: maken, toevoegen en verwijderen van invoer en uitvoer, toevoegen en bijwerken van de query starten en stoppen van de taak van de taak.
* **Uitvoering**. Bevat gebeurtenissen die zich tijdens het uitvoeren van taak voordoen:
    * Connectiviteitsfouten
    * Gegevensverwerking fouten, met inbegrip van:
        * Gebeurtenissen die niet aan de definitie van de query (niet-overeenkomende veldtypen en waarden, ontbrekende velden, enzovoort voldoen)
        * Expressie evaluatie fouten
    * Andere gebeurtenissen en fouten

## <a name="diagnostics-logs-schema"></a>Diagnostische logboeken schema

Alle logboeken worden opgeslagen in de JSON-indeling. Elk item heeft de volgende algemene tekenreeksvelden:

Naam | Beschrijving
------- | -------
tijd | De tijdstempel (in UTC) van het logboek.
resourceId | ID van de resource dat de bewerking plaatsgevonden, in hoofdletters heeft. Omvat de abonnements-ID, de resourcegroep en de taaknaam van de. Bijvoorbeeld:   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMINGJOBS-STREAMANALYTICS/MYSTREAMINGJOB**.
category | Meld u categorie, ofwel **uitvoering** of **ontwerpen**.
operationName | De naam van de bewerking die wordt vastgelegd. Bijvoorbeeld: **gebeurtenissen verzenden: SQL Output Schrijffout naar mysqloutput**.
status | De status van de bewerking. Bijvoorbeeld: **mislukt** of **geslaagd**.
niveau | Logboek-niveau. Bijvoorbeeld: **fout**, **waarschuwing**, of **informatief**.
properties | Meld u post-specifieke details over deze geserialiseerd als een JSON-tekenreeks. Zie de volgende secties voor meer informatie.

### <a name="execution-log-properties-schema"></a>Schema voor uitvoering logboek eigenschappen

Uitvoeringslogboeken zijn gegevens over de gebeurtenissen die hebben plaatsgevonden tijdens het uitvoeren van de Stream Analytics-taak. Het schema van eigenschappen varieert, afhankelijk van het type gebeurtenis. Momenteel hebben we de volgende soorten uitvoeringslogboeken:

### <a name="data-errors"></a>Fouten met gegevens

Er is een fout die optreedt wanneer de taak van gegevens verwerken in deze categorie van Logboeken. Deze logboeken worden gemaakt tijdens het lezen, gegevens serialisatie, meestal en schrijfbewerkingen. Deze logboeken bevatten geen connectiviteitsfouten. Connectiviteitsfouten worden behandeld als algemene gebeurtenissen.

Naam | Beschrijving
------- | -------
Bron | Naam van de taak invoer of uitvoer waarin de fout is opgetreden.
Bericht | Het bericht dat is gekoppeld met de fout.
Type | Type fout. Bijvoorbeeld: **DataConversionError**, **CsvParserError**, of **ServiceBusPropertyColumnMissingError**.
Gegevens | Bevat gegevens die nuttig is nauwkeurig de bron van de fout te vinden. Onderworpen aan moet worden afgekapt, afhankelijk van de grootte.

Afhankelijk van de **operationName** waarde, fouten hebben het volgende schema:
* **Gebeurtenissen serialiseren**. Serialiseren gebeurtenissen plaatsvinden tijdens gebeurtenis leesbewerkingen. Ze optreden wanneer de gegevens op de invoer niet voldoen aan het Queryschema voor een van de volgende redenen:
    * *Niet-overeenkomend gegevenstype tijdens gebeurtenis (de) serialisatie toepassen op*: identificeert het veld met de fout wordt veroorzaakt.
    * *Een gebeurtenis, Ongeldige serialisatie kan niet worden gelezen*: bevat informatie over de locatie in de invoergegevens waar de fout is opgetreden. De blobnaam van de voor blob-invoer, offset en een voorbeeld van de gegevens bevat.
* **Verzenden van gebeurtenissen**. Verzenden van gebeurtenissen plaatsvinden tijdens schrijfbewerkingen. Het identificeren van de streaming-gebeurtenis die de fout heeft veroorzaakt.

### <a name="generic-events"></a>Algemene gebeurtenissen

Algemene gebeurtenissen hebben betrekking op alle andere.

Naam | Beschrijving
-------- | --------
Fout | (optioneel) Informatie over de fout. Meestal is dit uitzonderingsgegevens, als deze beschikbaar is.
Bericht| Logboekbericht.
Type | Type van het bericht. Is toegewezen aan interne categorisatie van fouten. Bijvoorbeeld: **JobValidationError** of **BlobOutputAdapterInitializationFailure**.
Correlatie-ID | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) die wordt aangeduid de taak wordt uitgevoerd. Alle logboekvermeldingen voor uitvoering vanaf het moment dat de taak wordt gestart totdat de taak stopt dezelfde hebben **correlatie-ID** waarde.

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor stream Analytics query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)
