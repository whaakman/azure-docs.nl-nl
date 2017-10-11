---
title: Azure Application Insights telemetrie gegevensmodel - telemetrie Context | Microsoft Docs
description: Application Insights telemetrie context-gegevensmodel
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: d6a0cad8bda6ca68aa691867e84f540c5ac9f6f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="telemetry-context-application-insights-data-model"></a>Telemetrie-context: Application Insights-gegevensmodel

Elk telemetrie-item heeft mogelijk een sterk getypeerde context-velden. Elk veld kunt een specifiek scenario voor bewaking. Gebruik de verzameling aangepaste eigenschappen aangepaste of toepassingsspecifieke contextuele informatie op te slaan.


##<a name="application-version"></a>Toepassingsversie

Informatie in de velden van de context is altijd over de toepassing die de telemetrie verzendt. Toepassingsversie wordt gebruikt voor het analyseren van wijzigingen van de trend in het gedrag van toepassingen en correlatie voor de implementaties.

Maximale lengte: 1024


##<a name="client-ip-address"></a>IP-clientadres

Het IP-adres van het clientapparaat. IPv4 en IPv6 worden ondersteund. Wanneer telemetrie van een service wordt verzonden, wordt de locatie-context is over de gebruiker die de bewerking in de service wordt gestart. Application Insights geografische locatie gegevens ophalen uit de client-IP en vervolgens worden afgekapt. Client-IP op zichzelf kan dus niet worden gebruikt als identificeerbare informatie voor de eindgebruiker. 

Maximale lengte: 46


##<a name="device-type"></a>Apparaattype

Dit veld is oorspronkelijk gebruikt om aan te geven van het type van het apparaat dat de eindgebruiker van de toepassing wordt gebruikt. Vandaag de dag worden gebruikt voor het onderscheiden van JavaScript telemetrie met het apparaattype typt 'PC' u 'Browser"serverzijde telemetrie met het apparaat.

Maximale lengte: 64


##<a name="operation-id"></a>Bewerkings-id

Een unieke id van de basis-bewerking. Deze id kunt groep telemetrie over meerdere onderdelen. Zie [telemetrie correlatie](application-insights-correlation.md) voor meer informatie. De bewerkings-id is gemaakt door een aanvraag of een paginaweergave. Dit veld alle andere telemetrie ingesteld op de waarde voor de weergave van het insluitende aanvraag of pagina. 

Maximale lengte: 128


##<a name="parent-operation-id"></a>Bovenliggende bewerkings-ID

De unieke id van bovenliggende venster van de telemetrie-item. Zie [telemetrie correlatie](application-insights-correlation.md) voor meer informatie.

Maximale lengte: 128


##<a name="operation-name"></a>De naam van bewerking

De naam (groeps) van de bewerking. De naam van de bewerking wordt gemaakt door een aanvraag of een paginaweergave. Dit veld instelt alle telemetrie-items op de waarde voor de weergave van het insluitende aanvraag of pagina. Naam van de bewerking wordt gebruikt voor het zoeken naar alle telemetrie-items voor een groep bewerkingen (bijvoorbeeld ' GET-startpagina/Index'). Deze contexteigenschap wordt gebruikt om te beantwoorden vragen zoals "Wat zijn de gebruikelijke uitzonderingen op deze pagina."

Maximale lengte: 1024


##<a name="synthetic-source-of-the-operation"></a>Synthetische bron van de bewerking

Naam van de synthetische bron. Telemetrie van de toepassing mogelijk synthetisch verkeer vertegenwoordigen. Webcrawler indexeren van de website, site-beschikbaarheidstests of traces van diagnostische bibliotheken zoals Application Insights-SDK zelf kan zijn.

Maximale lengte: 1024


##<a name="session-id"></a>Sessie-id

Sessie-ID - het exemplaar van de gebruikersinteractie met de app. Gegevens in de context sessie velden is altijd over de eindgebruiker. Wanneer telemetrie van een service wordt verzonden, wordt de sessiecontext is over de gebruiker die de bewerking in de service wordt gestart.

Maximale lengte: 64


##<a name="anonymous-user-id"></a>Anonieme gebruikers-id

Anonieme gebruikers-id. Hiermee geeft u de gebruiker van de toepassing. Wanneer telemetrie van een service wordt verzonden, is de context van de gebruiker over de gebruiker die de bewerking in de service wordt gestart.

[Steekproef nemen](app-insights-sampling.md) is een van de technieken om de hoeveelheid verzamelde telemetriegegevens te minimaliseren. Steekproeven algoritme probeert met een steekproef in of uit de gecorreleerde telemetrie. Anonieme gebruikers-id wordt gebruikt voor de steekproef nemen score generatie. Anonieme gebruikers-id moet dus een willekeurige genoeg waarde. 

Met anonieme gebruikers-id voor het opslaan van de gebruikersnaam is een misbruik van het veld. Geverifieerde gebruikers-id gebruiken.

Maximale lengte: 128


##<a name="authenticated-user-id"></a>Geverifieerde gebruikers-id

Geverifieerde gebruikers-id. Het tegenovergestelde van anonieme gebruikers-id, dit veld wordt de gebruiker met een beschrijvende naam. Sinds de persoonlijke gegevens is deze standaard niet verzameld door de meeste SDK.

Maximale lengte: 1024


##<a name="account-id"></a>Account-id

Dit is de account-ID of naam, die de gebruiker met optreedt in een multitenant-toepassingen. Voorbeelden mogelijk abonnements-ID voor de Azure portal of blog naam weblog-platform.

Maximale lengte: 1024


##<a name="cloud-role"></a>Cloud-rol

Naam van de rol van de toepassing is een onderdeel van. Maps rechtstreeks naar de naam van de rol in azure. Kan ook worden gebruikt om te onderscheiden micro services die deel van één toepassing uitmaken.

Maximale lengte: 256


##<a name="cloud-role-instance"></a>Cloud-rolinstantie

Naam van het exemplaar waarop de toepassing wordt uitgevoerd. Computernaam voor on-premises exemplaarnaam voor Azure.

Maximale lengte: 256


##<a name="internal-sdk-version"></a>Intern: SDK-versie

SDK-versie. Zie https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification voor meer informatie.

Maximale lengte: 64


##<a name="internal-node-name"></a>Intern: Naam van het knooppunt

Dit veld wordt de naam van het knooppunt voor facturering doeleinden gebruikt. Gebruik dit voor het overschrijven van de standaard detectie van knooppunten.

Maximale lengte: 256


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [uitbreiden en het filteren van telemetrie](app-insights-api-filtering-sampling.md).
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.
- Bekijk standaard context eigenschappen verzameling [configuratie](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).
