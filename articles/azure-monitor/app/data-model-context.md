---
title: Azure Application Insights telemetrie Data Model - telemetrie Context | Microsoft Docs
description: Application Insights telemetrie context-gegevensmodel
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: fc2b5a5badcf38f7bc94a876f9f0a21af4f558b7
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53812052"
---
# <a name="telemetry-context-application-insights-data-model"></a>Telemetrie-context: Application Insights-gegevensmodel

Elk telemetrie-item heeft mogelijk een sterk getypeerde context-velden. Elk veld kunt een specifiek scenario voor bewaking. Gebruik de verzameling aangepaste eigenschappen voor het opslaan van aangepaste of toepassingsspecifieke contextuele informatie.


## <a name="application-version"></a>Toepassingsversie

Gegevens in de velden van de context is altijd over de toepassing die de telemetrie verzendt. Toepassingsversie wordt gebruikt voor het analyseren van trend wijzigingen in de werking van de toepassing en de samenhang met de implementaties.

Maximumlengte: 1024


## <a name="client-ip-address"></a>IP-adres van client

Het IP-adres van het clientapparaat. IPv4 en IPv6 worden ondersteund. Wanneer telemetrie wordt verzonden via een service, is de locatie-context over de gebruiker die de bewerking in de service heeft gestart. Application Insights de geo-locatie-informatie onttrekken aan de client-IP en vervolgens afkappen. Client-IP op zichzelf kan dus niet worden gebruikt als identificeerbare informatie voor de eindgebruiker. 

Maximumlengte: 46


## <a name="device-type"></a>Apparaattype

Dit veld is oorspronkelijk hebt gebruikt om aan te geven van het type van het apparaat dat de eindgebruiker van de toepassing wordt gebruikt. Vandaag nog gebruikt voornamelijk te onderscheiden van JavaScript-telemetrie met het apparaattype typt 'PC' u "Browser" van de server-side-telemetriegegevens met het apparaat.

Maximumlengte: 64


## <a name="operation-id"></a>Bewerkings-id

Een unieke id van de bewerking hoofdmap. Deze id kunt groep telemetrie voor meerdere onderdelen. Zie [telemetriecorrelatie](../../azure-monitor/app/correlation.md) voor meer informatie. De bewerkings-id wordt gemaakt door een aanvraag of een paginaweergave. Alle andere telemetrie Hiermee stelt u dit veld aan de waarde voor de overkoepelende weergave van de aanvraag of paginaweergave. 

Maximumlengte: 128


## <a name="parent-operation-id"></a>Bovenliggende bewerkings-ID

De unieke id van de direct bovenliggende van de telemetrie-item. Zie [telemetriecorrelatie](../../azure-monitor/app/correlation.md) voor meer informatie.

Maximumlengte: 128


## <a name="operation-name"></a>Naam van bewerking

De naam (groep) van de bewerking. Naam van de bewerking is gemaakt door een aanvraag of een paginaweergave. Dit veld instellen alle overige telemetrie-items op de waarde voor de overkoepelende weergave van de aanvraag of paginaweergave. Naam van de bewerking wordt gebruikt voor het zoeken naar alle telemetrie-items voor een groep van bewerkingen (bijvoorbeeld ' GET Home/Index'). Deze contexteigenschap wordt gebruikt om te beantwoorden vragen zoals "Wat zijn de typische uitzonderingen op deze pagina."

Maximumlengte: 1024


## <a name="synthetic-source-of-the-operation"></a>Synthetische bron van de bewerking

De naam van de synthetische bron. Telemetrie van de toepassing mogelijk synthetisch verkeer vertegenwoordigen. Webcrawler indexeren van de website, site-beschikbaarheidstests of traceringen van diagnostische bibliotheken, zoals Application Insights-SDK zelf kan zijn.

Maximumlengte: 1024


## <a name="session-id"></a>Sessie-id

Sessie-ID - het exemplaar van de gebruikersinteractie met de app. Gegevens in de velden van de context sessie is altijd over de eindgebruiker. Wanneer telemetrie wordt verzonden via een service, wordt de sessiecontext is over de gebruiker die de bewerking in de service heeft gestart.

Maximumlengte: 64


## <a name="anonymous-user-id"></a>Anonieme gebruikers-id

Anonieme gebruikers-id. Hiermee geeft u de gebruiker van de toepassing. Wanneer telemetrie wordt verzonden via een service, wordt de context van de gebruiker over de gebruiker die de bewerking in de service heeft gestart.

[Sampling](../../application-insights/app-insights-sampling.md) is een van de technieken voor het minimaliseren van de hoeveelheid verzamelde telemetrie. Samplingalgoritme probeert een steekproef binnen of buiten de gecorreleerde telemetrie. Anonieme gebruikers-id wordt gebruikt voor het genereren van de score. Dus anonieme gebruikers-id moet een willekeurige genoeg waarde. 

Anonieme gebruikers-id gebruiken voor het opslaan van de gebruikersnaam van de is een misbruik van het veld. Geverifieerde gebruikers-id gebruiken.

Maximumlengte: 128


## <a name="authenticated-user-id"></a>Geverifieerde gebruikers-id

Geverifieerde gebruikers-id. Het tegenovergestelde van anonieme gebruikers-id, dit veld geeft de gebruiker met een beschrijvende naam. Sinds de persoonlijke gegevens is het niet verzameld door de meeste SDK standaard.

Maximumlengte: 1024


## <a name="account-id"></a>Account-id

Dit is de account-ID of naam, die de gebruiker met fungeert in multitenant-toepassingen. Voorbeelden mogelijk abonnements-ID voor Azure-portal of blog naam blogplatform.

Maximumlengte: 1024


## <a name="cloud-role"></a>Cloudrol

Naam van de rol van de toepassing is een onderdeel van. Maps rechtstreeks naar de naam van de rol in azure. Kan ook worden gebruikt om te onderscheiden van microservices, die deel van één toepassing uitmaken.

Maximumlengte: 256


## <a name="cloud-role-instance"></a>Cloudrolinstantie

De naam van het exemplaar waar de toepassing wordt uitgevoerd. Computernaam voor on-premises, exemplaarnaam voor Azure.

Maximumlengte: 256


## <a name="internal-sdk-version"></a>Interne: SDK-versie

SDK-versie. Zie https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification voor meer informatie.

Maximumlengte: 64


## <a name="internal-node-name"></a>Interne: Naam van het knooppunt

Dit veld geeft de naam van het knooppunt wordt gebruikt voor factureringsdoeleinden. Gebruik dit voor het overschrijven van de standaard detectie van knooppunten.

Maximumlengte: 256


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [uitbreiden en telemetrie filteren](../../azure-monitor/app/api-filtering-sampling.md).
- Zie [gegevensmodel](data-model.md) voor Application Insights-typen en -gegevensmodel.
- Bekijk standard context eigenschappen verzameling [configuratie](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).
