---
title: Overzicht van Azure App Service-diagnostics | Microsoft Docs
description: Meer informatie over hoe u problemen kunt oplossen met uw app met App Service-diagnostics.
keywords: appservice, azure appservice, diagnostische gegevens, ondersteuning, web-app, het oplossen van problemen zelfhulp
services: app-service
documentationcenter: ''
author: jen7714
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.custom: seodec18
ms.openlocfilehash: 3e304df51133d53adad50e672249bde6c9960712
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65539792"
---
# <a name="azure-app-service-diagnostics-overview"></a>Overzicht van Azure App Service-diagnostics

Wanneer u een web-App uitvoert, die u wilt worden voorbereid voor eventuele problemen die zich voordoen kunnen, van 500-fouten naar uw gebruikers om u aan te geven dat uw site niet actief is. App Service-diagnostics is een intelligente en interactieve ervaring om op te lossen van uw app geen configuratie vereist. Als u problemen met uw app uitvoert, wijst App Service-diagnostics wat er mis om u te helpen de juiste informatie snel en eenvoudig kunt oplossen en los het probleem is.

Hoewel deze ervaring handig is als u problemen met uw app in de afgelopen 24 uur ondervindt, zijn de diagnostische grafieken altijd beschikbaar voor u om te analyseren.

App Service-diagnostics werkt niet alleen uw app op Windows, maar ook apps op [Linux/containers](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro), [App Service-omgeving](https://docs.microsoft.com/azure/app-service/environment/intro), en [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview).

## <a name="open-app-service-diagnostics"></a>Open App Service-diagnostics

Voor toegang tot App Service-diagnostics, gaat u naar uw App Service-web-app of een App Service-omgeving in de [Azure-portal](https://portal.azure.com). Klik in het linkernavigatievenster op **vaststellen en oplossen van problemen met**.

Voor Azure Functions, gaat u naar uw functie-app en in de bovenste navigatiebalk, klikt u op **platformfuncties**, en selecteer **vaststellen en oplossen van problemen met** uit de **resourcebeheer** sectie.

In de startpagina van de diagnostische gegevens van App Service kunt u de categorie die het beste het probleem met uw app beschrijft met behulp van de trefwoorden in elke tegel startpagina. Bovendien is deze pagina waar u kunt vinden **diagnostische hulpprogramma's voor** voor Windows-apps. Zie [diagnostische hulpprogramma's (alleen voor Windows-app)](#diagnostic-tools-only-for-windows-app).

![Startpagina](./media/app-service-diagnostics/app-service-diagnostics-homepage-1.png)

## <a name="interactive-interface"></a>Interactieve interface

Wanneer u een startpagina-categorie die het beste bij de probleem van uw app aansluit selecteert, kunt u bij het onderzoeken en oplossen van probleem met uw app in App Service-diagnostics interactieve interface, Genie, leiden. U kunt de tegel snelkoppelingen geleverd door Genie gebruiken om weer te geven van de volledige diagnostisch rapport van de probleemcategorie dat u geïnteresseerd bent. De tegel sneltoetsen bieden u een directe manier om toegang tot uw diagnostische gegevens.

![Tegel snelkoppelingen](./media/app-service-diagnostics/tile-shortcuts-2.png)

Nadat de gebruiker op deze tegels, ziet u een lijst met onderwerpen met betrekking tot het probleem dat wordt beschreven in de tegel. Deze onderwerpen bevatten fragmenten van belangrijke gegevens van het volledige rapport. U kunt klikken op een van de volgende onderwerpen om de problemen verder te onderzoeken. U kunt ook klikken op **volledig rapport weergeven** alle onderwerpen verkennen op één pagina.

![Onderwerpen](./media/app-service-diagnostics/application-logs-insights-3.png)

![Volledig rapport weergeven](./media/app-service-diagnostics/view-full-report-4.png)

## <a name="diagnostic-report"></a>Diagnostisch rapport

Nadat u hebt gekozen voor het onderzoeken van het probleem verder door te klikken op een onderwerp, vindt u meer informatie over het onderwerp die vaak worden aangevuld met grafieken en promoties. Diagnostisch rapport is een krachtig hulpmiddel voor dicht van het probleem met uw app.

![Diagnostisch rapport](./media/app-service-diagnostics/full-diagnostic-report-5.png)

## <a name="health-checkup"></a>Gezondheidscontrole

Als u niet weet wat er mis is met uw app is of niet weet waar u uw problemen met het starten, is de gezondheidscontrole een goede plaats om te starten. De gezondheidscontrole analyseert uw toepassingen waarin u een snelle, interactieve overzicht die op wat in orde is en wat is het probleem aangegeven waar u wijst wilt zoeken voor het onderzoeken van het probleem. De intelligente en interactieve interface biedt u instructies voor het proces voor het oplossen van problemen. Gezondheidscontrole is geïntegreerd met de Genie-ervaring voor Windows-apps en web-app omlaag diagnostisch rapport voor Linux-apps.

### <a name="health-checkup-graphs"></a>Status checkup grafieken

Er zijn vier verschillende grafieken weergegeven in de gezondheidscontrole.

- **aanvragen en fouten:** Een grafiek waarin het aantal aanvragen in de afgelopen 24 uur, samen met HTTP-serverfouten.
- **App-prestaties:** Een grafiek met reactietijd in de afgelopen 24 uur voor groepen van verschillende percentiel.
- **CPU-gebruik:** Een grafiek waarin het totale percentage CPU-gebruik per exemplaar in de afgelopen 24 uur.  
- **geheugengebruik:** Een grafiek waarin het totale percentage fysieke geheugengebruik per exemplaar in de afgelopen 24 uur.

![Gezondheidscontrole](./media/app-service-diagnostics/health-checkup-6.png)

### <a name="investigate-application-code-issues-only-for-windows-app"></a>Onderzoeken van problemen met toepassingen code (alleen voor Windows-app)

Omdat veel app problemen zijn met betrekking tot problemen in uw toepassingscode, App Service-diagnostics kan worden geïntegreerd met [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) markeren van uitzonderingen en afhankelijkheidsproblemen in te correleren met de geselecteerde downtime. Application Insights heeft afzonderlijk worden ingeschakeld.

![Application Insights](./media/app-service-diagnostics/application-insights-7.png)

Als u wilt weergeven in Application Insights-uitzonderingen en afhankelijkheden, selecteert u de **web-app omlaag** of **WebApp is langzaam** snelkoppelingen tegel.

### <a name="troubleshooting-steps-only-for-windows-app"></a>Stappen voor probleemoplossing (alleen voor Windows-app)

Als er een probleem is gedetecteerd met een specifiek probleemcategorie in de afgelopen 24 uur, kunt u de volledige diagnostische rapport bekijken en App Service-diagnostics wordt u om weer te geven van advies voor meer probleemoplossing en de volgende stappen voor een meer begeleide ervaring mogelijk gevraagd.

![Application Insights en probleemoplossing en de volgende stappen](./media/app-service-diagnostics/troubleshooting-and-next-steps-8.png)

## <a name="diagnostic-tools-only-for-windows-app"></a>Diagnostische hulpprogramma's (alleen voor Windows-app)

Diagnostische hulpprogramma's omvatten meer geavanceerde diagnostische hulpprogramma's, dat hulp bij het onderzoeken van toepassing problemen, traagheid, verbindingsreeksen en meer code. en proactieve hulpmiddelen die u helpen oplossen van problemen met CPU-gebruik, aanvragen en geheugen.

### <a name="proactive-cpu-monitoring"></a>Proactieve controle van CPU

Proactieve controle van CPU, biedt u een eenvoudige, proactieve manier om een actie ondernemen wanneer uw app of een onderliggend proces voor uw app van de hoge CPU-resources gebruikmaakt al. U kunt uw eigen regels CPU drempelwaarde tijdelijk een hoog CPU om probleem te verhelpen totdat de echte oorzaak van het onverwachte probleem wordt aangetroffen instellen.

![Proactieve controle van CPU](./media/app-service-diagnostics/proactive-cpu-monitoring-9.png)

### <a name="proactive-auto-healing"></a>Proactieve automatisch herstel

Net als proactieve controle van CPU, biedt proactieve automatisch herstel een eenvoudige en proactieve benadering van beperkende onverwacht gedrag van uw app. U kunt uw eigen regels op basis van het aantal aanvragen, trage aanvragen, geheugenlimiet en HTTP-statuscode om acties starten-risicobeperking instellen. Dit hulpprogramma kan worden gebruikt om tijdelijk een onverwacht gedrag tot de echte oorzaak van het probleem wordt gevonden. Voor meer informatie over proactieve automatisch herstel, gaat u naar [aankondiging van de nieuwe automatische Retoucheerpenseel ervaring in app service-diagnostics](https://azure.github.io/AppService/2018/09/10/Announcing-the-New-Auto-Healing-Experience-in-App-Service-Diagnostics.html).

![Proactieve automatisch herstel](./media/app-service-diagnostics/proactive-auto-healing-10.png)

## <a name="change-analysis"></a>Wijzigingsanalyse

In een ontwikkelingsomgeving snelle kan soms het lastig te bijhouden van alle wijzigingen aangebracht in uw app en laat alleen pinpoint op een wijziging die het gedrag van een slechte status heeft veroorzaakt. Wijziging analyse kunt u op de wijzigingen in uw app om oplossen van problemen ervaring mogelijk te beperken. Analyse van de wijziging is ingesloten in diagnostisch rapport zoals **toepassing vastloopt** zodat u deze als andere metrische gegevens gebruiken kunt.

![Analyse van standaard-pagina wijzigen](./media/app-service-diagnostics/change-analysis-default-page-11.png)

![Diff-weergave](./media/app-service-diagnostics/diff-view-12.png)

Analyse van de wijziging moet worden ingeschakeld voordat u de functie. Voor meer informatie over analysis wijzigen, gaat u naar [aankondiging van de nieuwe ervaring van de analyse wijzigen in App Service-Diagnostics](https://azure.github.io/AppService/2019/05/07/Announcing-the-new-change-analysis-experience-in-App-Service-Diagnostics-Analysis.html).