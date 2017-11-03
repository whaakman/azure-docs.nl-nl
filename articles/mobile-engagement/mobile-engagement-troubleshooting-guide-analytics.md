---
title: Azure Mobile Engagement Troubleshooting Guide - analyses
description: Het oplossen van problemen Analytics, bewaking, segmentering en Dashboard in Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e30c9ac0a8421ffcf4fc3e2548cfd7ac49701900
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Gids voor probleemoplossing voor problemen die Analytics, bewaking, segmentering en Dashboard
Hieronder vindt u mogelijke problemen optreden bij hoe Azure Mobile Engagement wordt informatie verzameld over uw toepassingen, apparaten en gebruikers.

## <a name="missingdelayed-information"></a>Ontbrekende/uitgestelde informatie
### <a name="issue"></a>Probleem
* Informatie is in die voorkomen in Analytics, segmentering of Dashboard vertraagd.
* Er ontbreekt informatie in de bewaking.
* Er ontbreekt informatie in Analytics, segmentering of Dashboard.
* Roept segmentering beperkt.

### <a name="causes"></a>Oorzaken
* U kunt de API Analytics, Monitor-API en segmenten API om te zien of er gegevens u ontbreekt in de gebruikersinterface is zichtbaar via de API's.
* Als de Azure Mobile Engagement SDK is niet correct worden geïntegreerd in uw app vervolgens u niet mogelijk om gegevens in de Analytics, segmentering, bewaking of Dashboards te bekijken.
* Segmenten kunnen niet worden gewijzigd als ze zijn gemaakt, segmenten kunnen alleen worden 'gekloonde' (overgenomen) of 'vernietigd' (verwijderd). Segmenten kunnen alleen 10 criteria bevatten.
* De beste manier om te testen ontbrekende informatie van bewaking is voor het instellen van een testapparaat, verwijderen en/of installeer de app opnieuw op het testapparaat.
* Informatie wordt elke 24 uur vernieuwd voor analyses, segmentering of Dashboards.
* Informatie in de nieuwe segmenten mogelijk niet weergegeven tot 24 uur nadat ze zijn gemaakt, zelfs als het segment is gebaseerd op eerdere informatie.
* Uw analytische gegevens in de gebruikersinterface voor het filteren ziet alle voorbeelden van dit type ongeacht de versie van uw app (bijvoorbeeld) "Loopt vast" gefilterd met de naam ziet van versie 1 en versie 2 van uw app).
* De periode voor Analytics is gebaseerd op de datum van de gebruikers-instellingen voor apparaten, zodat een gebruiker waarvan telefoon heeft de onjuist ingestelde datum in de verkeerde periode weergegeven kan.
* Er zijn geen gegevens worden geregistreerd wanneer u met de knop 'test' serverzijde pushes, gegevens alleen worden geregistreerd voor echte pushcampagnes.

## <a name="cant-locate-items-in-ui"></a>Kan items niet vinden in de gebruikersinterface
### <a name="issue"></a>Probleem
* Kan niet op basis van bepaalde ingebouwd in segmenten maken of aangepaste app-gegevens labelen criteria.
* Kan niet vinden op bepaalde ingebouwde of aangepaste app-gegevens labelen criteria in Analytics, bewaking of Dashboards.
* De gegevens in Analytics, bewaking, segmentering of Dashboards kunnen niet worden geïnterpreteerd.

### <a name="causes"></a>Oorzaken
* Sommige items ingebouwd en app-info labels zijn alleen beschikbaar als push criteria, maar mogen niet worden toegevoegd aan een segment of zichtbaar vanuit Analytics, bewaking of Dashboard. 
* Voor ingebouwde artikelen en app-info-labels die kunnen niet worden toegevoegd aan een segment, moet u setup-lijst met doelen criteria in elke campagne om uit te voeren dezelfde functie uit als die gericht is op basis van een segment.
* Zie de contextmenu's in de secties Analytics, bewaking, segmentering en Dashboards van de gebruikersinterface van Azure Mobile Engagement voor meer informatie en hoe u informatie.

## <a name="crash-troubleshooting"></a>Crash het oplossen van problemen
### <a name="issue"></a>Probleem
* Toepassing is vastgelopen verschijnen in Analytics, bewaking of Dashboard.

### <a name="causes"></a>Oorzaken
* Voor het oplossen van Controleer toepassing is vastgelopen in Analytics, bewaking of Dashboard weergegeven. of de releaseopmerkingen voor bekende problemen met eerdere versies van de SDK.
* Om verder op te lossen uitvoeren vastlopen van de toepassing een gebeurtenis vanaf een testapparaat waarop de toepassing is geïnstalleerd en zoekt u naar uw apparaat-ID in de sectie 'Gebeurtenissen controleren –' van de gebruikersinterface van Azure Mobile Engagement. Voer de gebeurtenis die wordt veroorzaakt door uw toepassing crashes en aanvullende informatie opzoeken in de sectie 'Monitor – crashes' van de gebruikersinterface van Azure Mobile Engagement. 

