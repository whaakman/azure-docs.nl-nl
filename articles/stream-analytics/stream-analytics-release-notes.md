---
title: Releaseopmerkingen voor stream Analytics | Microsoft Docs
description: Opmerkingen bij de Release van de stream Analytics
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5e59f893-cd2c-43fb-9eca-c146ce637203
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/03/2017
ms.author: samacha
ms.openlocfilehash: 3251cd47bb917912d63330345dbf392e724448ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="stream-analytics-release-notes"></a>Stream Analytics release-opmerkingen

## <a name="notes-for-06142017-update-of-stream-analytics-tools-for-visual-studio"></a>Opmerkingen voor de update 14-06/2017 van Stream Analytics-hulpprogramma's voor Visual Studio
Deze update is voor onze Visual Studio-hulpprogramma's. Deze versie bevat de volgende nieuwe functies:

| Titel | Beschrijving |
| --- | --- |
| Java-script editor-ondersteuning |U kunt de ervaring voor de systeemeigen java script editor na het maken van uw java-scriptfuncties genieten.|
| Taak weergeven foutbericht tijd worden uitgevoerd | Als er runtimefouten tijdens de taakuitvoering, kunt u ze op het tabblad fouten weergeven door het tijdvenster weer aan te passen. Standaard bevat deze de foutberichten voor de laatste 30 minuten. |
| Ondersteuning voor CSV en Avro voor lokale testen invoer | Naast de JSON, kunt nu u CSV en de Avro-bestandsindeling voor lokale testen invoer.|

## <a name="notes-for-05032017-update-of-stream-analytics"></a>Opmerkingen voor 03-05/2017 bijwerken van de Stream Analytics
Deze update is voor onze voor probleemoplossing documentatie-release.

De [probleemoplossingsgids](stream-analytics-troubleshooting-guide.md) en andere documenten zijn uitgebracht. Controleer, feedback is Welkom.

## <a name="notes-for-04242017-update-of-stream-analytics-tools-for-visual-studio"></a>Opmerkingen voor 24/04/2017 bijwerken van de Stream Analytics-hulpprogramma's voor Visual Studio
Deze update is voor onze Visual Studio-hulpprogramma's. Deze versie bevat de volgende nieuwe functies:

| Titel | Beschrijving |
| --- | --- |
| Testresultaat van lokale bekijken in Visual Studio | Als u wilt weergeven van het uitvoerresultaat van de lokale test, druk op ENTER in het consolevenster uitvoer of sluit het bestand. Het resultaat wordt weergegeven in een venster in Visual Studio in tabelindeling. |
| Lokale uitvoerresultaat in JSON-indeling | Wanneer u een lokale test uitvoert, wordt het uitvoerresultaat wordt gegenereerd als JSON- en CSV-bestandsindeling. |
| Een voorbeeld van Blob-/ tabelnaam opslag invoer/uitvoer-gegevens | Door te dubbelklikken op een blob of tabel opslag invoer/uitvoer in de Project-weergave, kunt u de gegevens in Visual Studio eenvoudig bekijken. |
| Foutbericht voor invoer/uitvoer weergeven | Als er een runtime-fouten gerelateerd aan uw project in- of uitgangen zijn, wordt deze weergegeven op de taak diagram waarin u de muisaanwijzer kunt erop om het gedetailleerde foutbericht te zien.|


## <a name="notes-for-02012017-release-of-stream-analytics"></a>Opmerkingen bij de release-01-02/2017 van Stream Analytics
Deze versie bevat de volgende update:

| Titel | Beschrijving |
| --- | --- |
| Inleiding tot JavaScript-gebruiker gedefinieerde functies (UDF) |[Java door de gebruiker gedefinieerde functies](stream-analytics-javascript-user-defined-functions.md) zijn nu beschikbaar voor extra flexibiliteit bij het maken van query's. |
| Introductie van hulpprogramma's voor Visual Studio en de Stream Analytics |[Hulpprogramma's voor Visual Studio](stream-analytics-tools-for-visual-studio.md) zijn nu beschikbaar voor foutopsporing en groter hulpprogramma. |
| Inleiding tot registratie van diagnostische gegevens |[Diagnostische logboekregistratie](stream-analytics-job-diagnostic-logs.md) is nu beschikbaar voor extra opties voor probleemoplossing. |
| Inleiding tot georuimtelijke functies |[Georuimtelijke functies](http://msdn.microsoft.com/library/mt778980(Azure.100).aspx) zijn nu algemeen beschikbaar. |

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Opmerkingen voor 15-04-2016-release van Stream Analytics
Deze versie bevat de volgende update:

| Titel | Beschrijving |
| --- | --- |
| Algemene beschikbaarheid voor Power BI-uitvoer |[Power BI-uitvoer](stream-analytics-power-bi-dashboard.md) zijn nu algemeen beschikbaar. De autorisatie van 90 dagen is verlopen voor Power BI is verwijderd. Zie voor meer informatie over scenario's waarin autorisatie moet worden vernieuwd, de [vernieuwen autorisatie](stream-analytics-power-bi-dashboard.md#renew-authorization) sectie van het maken van een Power BI-dashboard. |

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Opmerkingen bij de release van Stream Analytics 03-03-2016
Deze versie bevat de volgende update:

| Titel | Beschrijving |
| --- | --- |
| Nieuwe Stream Analytics Query Language-items |SAQL heeft nu [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN-pagina"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN-pagina") en [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN-pagina"). |

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Opmerkingen bij de release van Stream Analytics 12-10-2015
Deze versie bevat de volgende update:

| Titel | Beschrijving |
| --- | --- |
| Update van de REST-API-versie |De REST-API-versie is bijgewerkt naar 2015-10-01. Details zijn te vinden op MSDN op [Stream Analytics Management REST API-verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx) en [Machine Learning-integratie in Stream Analytics](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md). |
| Azure Machine Learning-integratie |Met deze versie bevat ondersteuning voor Azure Machine Learning van de gebruiker gedefinieerde functies. Zie de [zelfstudie](stream-analytics-machine-learning-integration-tutorial.md) voor meer informatie, evenals de [algemene blog aankondiging](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx). |

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Opmerkingen voor 11-12-2015-release van Stream Analytics
Deze versie bevat de volgende update:

| Titel | Beschrijving |
| --- | --- |
| Nieuwe gedrag van selecteren |Selecteer in de Stream Analytics uitgebreid zodat * als eigenschapsaccessor van een geneste record. Zie voor meer informatie [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "complexe gegevenstypen"). |

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Opmerkingen voor 22/10/2015-release van Stream Analytics
Deze versie bevat de volgende updates:

| Titel | Beschrijving |
| --- | --- |
| Extra query language-functies |Stream Analytics heeft de query language uitgebreid door de volgende functies: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [maximum](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [aanmelding](https://msdn.microsoft.com/library/azure/mt605290.aspx), [VIERKANT](https://msdn.microsoft.com/library/azure/mt605288.aspx), en [SQRT](https://msdn.microsoft.com/library/azure/mt605238.aspx). |
| Cumulatieve beperkingen verwijderd |Deze release verwijdert de beperking van 15 statistische functies in een query. Er is nu geen limiet aan het aantal aggregaties per query. |
| Toegevoegde groep door System.Timestamp-functie |De [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) functie nu kunt u beide window_type of [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx). |
| Toegevoegde OFFSET voor Tumbling en Hopping van windows |Standaard [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) en [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) windows zijn uitgelijnd tegen de nul tijd (1/1/0001 12:00:00 AM UTC). De nieuwe (optioneel) parameter 'offsetsize' kunt u opgeven die een aangepaste offset (of uitlijning). |

## <a name="notes-for-09292015-release-of-stream-analytics"></a>Opmerkingen voor 29-09/2015-release van Stream Analytics
Deze versie bevat de volgende updates:

| Titel | Beschrijving |
| --- | --- |
| Openbare Preview van Azure IoT Suite |Stream Analytics is opgenomen in de openbare Preview van Azure IoT Suite. |
| Integratie van Azure portal |Naast de blijvende aanwezigheid in de Azure portal nu Stream Analytics is geïntegreerd in de [Azure-portal](https://azure.microsoft.com/overview/preview-portal/). Houd er rekening mee dat Stream Analytics-functionaliteit in de Preview-portal momenteel een subset van de functionaliteit die worden aangeboden in de Azure-portal zonder ondersteuning is voor het testen van query's in de browser de dat Power BI-uitvoer configuratie, en bladeren naar of maken van nieuwe invoer en uitvoer bronnen in abonnementen die u toegang tot hebt. |
| Ondersteuning voor Cosmos-DB-uitvoer |Stream Analytics-taken nu kunnen uitvoeren naar [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). |
| Ondersteuning voor invoer van de IoT-Hub |Gegevens uit IoT Hubs kunnen nu opnemen voor stream Analytics-taken. |
| TIMESTAMP BY voor heterogene-gebeurtenissen |Wanneer een enkele gegevensstroom meerdere gebeurtenistypen met tijdstempels in verschillende velden bevat, kunt u nu gebruiken [TIMESTAMP BY](http://msdn.microsoft.com/library/mt573293.aspx) met expressies verschillende tijdstempelvelden voor elk geval opgeven. |

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Opmerkingen voor 09-10-2015-release van Stream Analytics
Deze versie bevat de volgende updates:

| Titel | Beschrijving |
| --- | --- |
| Ondersteuning voor Power BI-groepen |Als u wilt delen van gegevens met andere Power BI-gebruikers, Stream Analytics-taken nu kunnen schrijven naar [PowerBI groepen](stream-analytics-define-outputs.md#power-bi) binnen uw Power BI-account. |

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Opmerkingen voor 08/20/2015-release van Stream Analytics
Deze versie bevat de volgende updates:

| Titel | Beschrijving |
| --- | --- |
| LAATSTE functie opgeroepen toegevoegd |De [laatste](http://msdn.microsoft.com/library/mt421186.aspx) functie is nu beschikbaar in de Stream Analytics-taken, zodat u voor het ophalen van de meest recente gebeurtenis in de gebeurtenisstroom loskoppelt binnen een bepaalde periode. |
| Nieuwe matrix-functies |Matrix van functies [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) en [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) zijn nu beschikbaar. |
| Nieuwe functies van de Record |Functies vastleggen [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) en [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) zijn nu beschikbaar. |

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Opmerkingen voor 07/30/2015-release van Stream Analytics
Deze versie bevat de volgende updates:

| Titel | Beschrijving |
| --- | --- |
| Organisatie-Id voor Power BI is losgekoppeld van de Azure-Id van |Met deze functie kunt [Power BI-uitvoer](stream-analytics-power-bi-dashboard.md) voor ASA-jobs onder elk type Azure-account (Live Id of organisatie-Id). Bovendien kunt u een organisatie-Id hebben voor uw Azure-account en een andere naam voor het machtigen van Power BI-uitvoer. |
| Ondersteuning voor de uitvoer van de Service Bus-wachtrijen |[Service Bus-wachtrijen](stream-analytics-define-outputs.md#service-bus-queues) uitgangen zijn nu beschikbaar in de Stream Analytics-taken. |
| Ondersteuning voor de uitvoer van de Service Bus-onderwerpen |[Service Bus-onderwerpen](stream-analytics-define-outputs.md#service-bus-topics) uitgangen zijn nu beschikbaar in de Stream Analytics-taken. |

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Opmerkingen voor 07/09/2015-release van Stream Analytics
Deze versie bevat de volgende updates:

| Titel | Beschrijving |
| --- | --- |
| Aangepaste Blob partitioneren van uitvoer |BLOB storage uitvoer kunnen nu een optie voor de frequentie die uitvoer blobs worden geschreven en de structuur en indeling van de structuur van de pad naar map voor de uitvoer-gegevens. |

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Opmerkingen voor 05-03-2015-release van Stream Analytics
Deze versie bevat de volgende updates:

| Titel | Beschrijving |
| --- | --- |
| Maximale waarde verhoogd voor Out of volgorde tolerantieperiode |De maximale grootte voor de tolerantieperiode voor Out-of-volgorde is nu 59:59 (mm: SS) |
| JSON-indeling van uitvoer: De regel gescheiden of matrix |Er is nu een optie bij het uitvoeren naar Blob Storage of Event Hub om uit te voeren als een matrix van JSON-objecten of door het JSON-objecten scheiden met een nieuwe regel. |

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Opmerkingen bij de release van Stream Analytics-04-16-2015
| Titel | Beschrijving |
| --- | --- |
| Vertraging in de configuratie van Azure Storage-account |Wanneer u een Stream Analytics-taak in een regio voor de eerste keer maakt, wordt u gevraagd een nieuw opslagaccount maken of geef een bestaande account voor het bewaken van de Stream Analytics-taken in deze regio. Vanwege een wachttijd bij het configureren van bewaking, een ander Stream Analytics-taak maken in dezelfde regio binnen 30 minuten prompt voor het opgeven van een tweede opslagaccount in plaats van de recent geconfigureerde structuur in de vervolgkeuzelijst bewaking Storage-Account wordt weergegeven. Om te voorkomen dat een onnodige Storage-account maakt, wacht u 30 minuten na het maken van een taak in een regio voor de eerste keer voordat u extra taken in deze regio inricht. |
| Upgrade van de taak |Stream Analytics ondersteunt op dit moment geen live bewerkingen van de definitie of de configuratie van een actieve taak. Wijzigen van de invoer, uitvoer, query, scale of configuratie van een actieve taak, moet u de taak te stoppen. |
| Gegevenstypen die zijn afgeleid van de invoerbron |Als een CREATE TABLE-instructie wordt niet gebruikt, wordt het invoertype is afgeleid van invoerindeling, bijvoorbeeld alle velden uit CSV tekenreeks zijn. Velden moeten expliciet worden geconverteerd naar het juiste type met de CAST-functie om te voorkomen dat type verschil fouten. |
| Ontbrekende velden worden als null-waarden gegenereerd |Verwijst naar een veld dat is niet aanwezig in de invoerbron resulteert in een null-waarden in de uitvoergebeurtenis. |
| MET instructies moeten worden voorafgegaan door een SELECT-instructies |SELECT-instructies moeten subquery's gedefinieerd met de instructies volgen in uw query. |
| Probleem-geheugen |Streaming Analytics-taken met een grote tolerantie voor out-van-order gebeurtenissen en/of complexe query's voor het onderhouden van een grote hoeveelheid status kan ertoe leiden dat de taak is een tekort aan geheugen, wat resulteert in een taak opnieuw starten. De bewerkingen starten en stoppen, zijn zichtbaar in de bewerkingslogboeken van de taak. U lost dit schaal uit de query voor meerdere partities. Deze beperking wordt in een toekomstige release verholpen door op de prestaties van de betrokken taken in plaats van ze te beïnvloeden. |
| Grote blob invoer zonder nettolading timestamp kunnen-geheugen probleem veroorzaken |Grote bestanden van Blob-opslag mogelijk Stream Analytics-taken vastlopen als een tijdstempelveld niet is opgegeven via de TIMESTAMP BY. Om dit te voorkomen, houd dan 10 MB van elke blob in grootte. |
| SQL-Database gebeurtenis volume beperking |Wanneer u SQL-Database als een output-doel, kan zeer grote hoeveelheden uitvoergegevens kunnen ertoe leiden dat de Stream Analytics-taak time-out. U lost dit probleem, vermindering van het volume uitvoer met behulp van statistische functies of filteroperators of Azure Blob-opslag of Event Hubs in plaats daarvan als een output-doel kiezen. |
| Power BI-gegevenssets kan slechts één tabel bevatten. |Power BI biedt geen ondersteuning voor meerdere tabellen in een bepaalde gegevensset. |

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
