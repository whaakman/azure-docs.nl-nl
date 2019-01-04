---
title: Azure Application Insights Transaction Diagnostics | Microsoft Docs
description: Diagnostische gegevens voor het end-to-end transacties van Application Insights
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/19/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: b7ad21bd3185e5e6ba4774cdc5780b0594033f14
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54028298"
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Diagnostische gegevens voor geïntegreerde tussen onderdelen transacties

Diagnostische gegevens over de geïntegreerde ervaring automatisch standaarddocumentnaam serverzijde telemetrie uit voor alle onderdelen van uw Application Insights bewaakt in één weergave. Het maakt niet uit als u meerdere bronnen met afzonderlijke instrumentatiesleutels hebt. Application Insights wordt de onderliggende relatie gedetecteerd en kunt u eenvoudig vaststellen van het toepassingsonderdeel, afhankelijkheid of uitzondering waardoor een transactie vertraging of fout.

## <a name="what-is-a-component"></a>Wat is een onderdeel?

Onderdelen zijn onafhankelijk implementeerbare onderdelen van uw toepassing gedistribueerd/microservices. Ontwikkelaars en uitvoerende teams hebben zichtbaarheid van de code op serverniveau of toegang tot telemetrie die is gegenereerd door de onderdelen van deze toepassing.

* Onderdelen zijn anders dan 'waargenomen' externe afhankelijkheden, zoals SQL, EventHub enz. die uw team/organisatie mogelijk geen toegang tot (code of telemetrie).
* Onderdelen worden uitgevoerd op een willekeurig aantal exemplaren van de rol-server-container.
* Onderdelen kunnen worden afzonderlijke Application Insights-instrumentatiesleutels (zelfs als abonnementen verschillen) of verschillende rollen die rapporteren aan een enkele Application Insights-instrumentatiesleutel. De nieuwe ervaring worden details weergegeven voor alle onderdelen, ongeacht hoe ze zijn ingesteld.

> [!NOTE]
> * **Ontbreekt de bijbehorende koppelingen?** Alle van de gerelateerde telemetrie zijn in de [boven](#cross-component-transaction-chart) en [onder](#all-telemetry-with-this-Operation-Id) secties van aan de linkerkant. 

## <a name="transaction-diagnostics-experience"></a>Ervaring van diagnostische gegevens voor transacties
In deze weergave heeft vier belangrijke onderdelen: resultaten van de lijst, een grafiek transacties tussen onderdelen, een tijdreeks lijst met alle telemetrie met betrekking tot deze bewerking en het deelvenster met details voor elk geselecteerde telemetrie-item aan de linkerkant.

![Belangrijke onderdelen](media/transaction-diagnostics/4partsCrossComponent.png)

## <a name="cross-component-transaction-chart"></a>Transacties tussen onderdelen grafiek

In deze grafiek biedt een tijdlijn met horizontale balken voor de duur van aanvragen en afhankelijkheden voor onderdelen. Eventuele uitzonderingen die worden verzameld, worden ook gemarkeerd in de tijdlijn geplaatst.

* De bovenste rij in deze grafiek vertegenwoordigt het toegangspunt de inkomende aanvraag naar het eerste onderdeel dat met de naam in deze transactie. De duur is de totale tijd voor de transactie te voltooien.
* Alle aanroepen naar externe afhankelijkheden zijn eenvoudige niet-samenvouwbare rijen, met de pictogrammen van het afhankelijkheidstype.
* Aanroepen naar de andere onderdelen zijn samenvouwbare rijen. Elke rij overeenkomt met een bepaalde bewerking aangeroepen op het onderdeel.
* Door standaard, de aanvraag, afhankelijkheid of uitzonderingen wordt die u hebt geselecteerd weergegeven aan de rechterkant.
* Selecteer een rij om te zien zijn [details aan de rechterkant](#details-of-the-selected-telemetry). 

> [!NOTE]
Aanroepen naar de andere onderdelen, met twee rijen bevatten: één rij vertegenwoordigt de uitgaande gesprekken (afhankelijkheden) van het onderdeel van de oproepende functie en de andere rij overeenkomt met de binnenkomende aanvraag op het onderdeel met de naam. De toonaangevende pictogram en de afzonderlijke stijl van de duur van de balken kunt onderscheid maken tussen deze.

## <a name="all-telemetry-with-this-operation-id"></a>Alle telemetrie met deze bewerking-Id

In deze sectie bevat platte lijstweergave in een tijdreeks van alle telemetrie die betrekking hebben op deze transactie. Het bevat ook de aangepaste gebeurtenissen en traceringen die niet worden weergegeven in de grafiek transactie. U kunt deze lijst om telemetrie die is gegenereerd door een specifieke component per aanroep te filteren. U kunt een telemetrie-item selecteren in deze lijst om te zien van de bijbehorende [details aan de rechterkant](#details-of-the-selected-telemetry).

![Volgorde van de tijd van alle telemetrie](media/transaction-diagnostics/allTelemetryDrawerOpened.png)

## <a name="details-of-the-selected-telemetry"></a>Details van de geselecteerde telemetrie

Dit samenvouwbare deelvenster toont de details van de transactie-grafiek of de lijst met geselecteerde items. 'Alles weergeven', worden alle van de standard-kenmerken die worden verzameld. Eventuele aangepaste kenmerken worden afzonderlijk weergegeven onder de norm die is ingesteld. Klik op de '...' onder de stack trace-venster om een optie voor het kopiëren van de tracering. 'Open profilertraces' of 'momentopname voor foutopsporing openen' ziet u code op diagnostische gegevens in deelvensters van de bijbehorende details.

![Details van uitzondering](media/transaction-diagnostics/exceptiondetail.png)

## <a name="search-results"></a>Zoekresultaten

Dit samenvouwbare deelvenster toont de resultaten die voldoen aan de filtercriteria. Klik op een van de resultaten naar de details van de respectieve de 3 secties bovenstaande bijwerken. We proberen te vinden van voorbeelden die de details die beschikbaar is via alle onderdelen hebben, zelfs als de sampling van kracht is in een van deze waarschijnlijk zijn. Deze worden weergegeven als 'aanbevolen'-voorbeelden.

![Zoekresultaten](media/transaction-diagnostics/searchResults.png)

## <a name="profiler-and-snapshot-debugger"></a>Profiler- en snapshot debugger

[Application Insights profiler](../../azure-monitor/app/profiler.md) of [snapshot debugger](snapshot-debugger.md) helpen met code-niveau van diagnostische gegevens van de prestaties en mislukt kwesties. Met deze ervaring, kunt u profiler-traceringen zien of momentopnamen van elk onderdeel met één klik.

Als u Profiler werken niet ophalen kan, contact op met **serviceprofilerhelp@microsoft.com**

Als u Snapshot Debugger werken niet ophalen kan, contact op met **snapshothelp@microsoft.com**

![Profiler-integratie](media/transaction-diagnostics/profilerTraces.png)

## <a name="faq"></a>Veelgestelde vragen

*Ik zie een één onderdeel in de grafiek en de andere worden alleen weergegeven als externe afhankelijkheden zonder alle details van wat is er gebeurd binnen deze onderdelen.*

Mogelijke oorzaken:

* Zijn de andere onderdelen geïnstrumenteerd met Application Insights?
* Gebruikt ze de nieuwste stabiele Application Insights SDK?
* Als deze onderdelen afzonderlijke Application Insights-resources zijn, hebt u vereist toegang tot hun telemetrie?

Als u toegang hebt en de onderdelen zijn uitgerust met de nieuwste Application Insights-SDK's, laat het ons weten via de bovenste juiste feedback-kanaal.

*Ik zie dubbele rijen voor de afhankelijkheden. Is dit verwacht?*

Op dit moment zijn we de uitgaande afhankelijkheidsaanroep gescheiden van de inkomende aanvraag weergegeven. Het aanroepen van de twee er normaal gesproken identiek zijn met alleen de duurwaarde round trip verschillen vanwege het netwerk wordt. De toonaangevende pictogram en de afzonderlijke stijl van de duur van de balken kunt onderscheid maken tussen deze. Deze presentatie van de gegevens verwarrend is? Geef ons uw feedback!

*Hoe zit klok Hiermee laat u overhellen over verschillende onderdeelexemplaren?*

Tijdlijnen worden tijdsverschillen in de grafiek transactie aangepast. Hier ziet u de exacte tijdstempels in het detailvenster worden weergegeven of met behulp van Analytics.

*Waarom ontbreekt de nieuwe ervaring voor de meeste van de verwante items query 's*

Dit is standaard. Alle verwante items, voor alle onderdelen zijn al beschikbaar aan de linkerkant (boven- en -secties). De nieuwe ervaring heeft twee verwante items die niet wordt behandeld in aan de linkerkant: alle telemetrie van vijf minuten voor en na deze gebeurtenis en de tijdlijn van de gebruiker.
