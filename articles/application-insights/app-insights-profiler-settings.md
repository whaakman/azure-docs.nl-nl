---
title: Blade instellingen van Azure Application Insights Profiler | Microsoft Docs
description: Profiler-status zien en sessies profileren starten
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: db3142837c5bb2c5758e47ddf017da284b2e0f0e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017732"
---
# <a name="configure-application-insights-profiler"></a>Application Insights Profiler configureren

## <a name="profiler-settings-page"></a>De pagina instellingen Profiler

De pagina profiler-instellingen van de prestaties van Application Insights-pagina kan worden geopend door te drukken de **Profiler** knop.

![profiler deelvenster vermelding configureren][configure-profiler-entry]

De pagina configureren van Application Insights Profiler bevat vier onderdelen: 
1. **Profiel nu** -met deze knop zorgt ervoor dat profilering sessies te starten voor alle apps die zijn gekoppeld aan dit exemplaar van Application Insights
1. **Gekoppelde apps** -lijst met toepassingen profiler te verzenden naar deze Application Insights-resource
1. **Sessies Bezig** : wanneer u op de toets **profiel nu**, de status van de sessie wordt hier weergegeven)
1. **Recente sessies profilering** -bevat informatie over de afgelopen profilering sessies.

![Profiler op aanvraag][profiler-on-demand]

## <a name="app-service-environments-ase"></a>App Service-omgevingen (ASE)
Afhankelijk van hoe de as-omgeving is geconfigureerd, kan de aanroep om te controleren op de status van de agent worden geblokkeerd. Deze pagina wordt zeggen dat de agent wordt niet uitgevoerd wanneer deze in feite is. U kunt controleren of de webtaak op uw toepassing controleren. Maar als alle appinstellingen juist zijn ingesteld en de App Insights-site-extensie is geïnstalleerd op uw toepassing, de profiler zal worden uitgevoerd en ziet u recente profilering sessies in de lijst als er voldoende verkeer naar uw toepassing.

## <a id="profileondemand"></a> Profiler handmatig activeren

Profiler kan handmatig met één klik worden geactiveerd. Stel dat u een test voor de prestaties van webtoepassingen worden uitgevoerd. Moet u traceringen om te begrijpen hoe uw web-app wordt uitgevoerd onder belasting. Controle over wanneer traceringen worden vastgelegd met is cruciaal omdat u weet wanneer belastingstest wordt uitgevoerd, maar het interval van steekproeven mogelijk worden gemist.
De volgende stappen laten zien hoe dit scenario werkt:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Optioneel) Stap 1: Verkeer naar uw web-app genereren op basis van een WebTest voor prestaties

Als uw web-app is al het inkomende verkeer of als u alleen wilt handmatig genereren verkeer, kunt u deze sectie overslaan en doorgaan naar stap 2.

Ga naar Application Insights-portal **configureren > prestatietests**. Klik op de knop Nieuw om een nieuwe prestatietest te starten.

![maken van nieuwe prestatietest][create-performance-test]

In de **nieuwe prestatietest** in het deelvenster voor het configureren van de test doel-URL. Accepteer alle standaardinstellingen en start de load-test uitvoert.

![Configureren van load test][configure-performance-test]

U ziet dat de nieuwe test staat in de wachtrij eerst, gevolgd door de status wordt uitgevoerd.

![belastingstest wordt verzonden en in de wachtrij geplaatst][load-test-queued]

![belastingstest wordt uitgevoerd in voortgang][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Stap 2: Profiler op aanvraag starten

Zodra de load-test wordt uitgevoerd, kunnen we beginnen profiler voor het vastleggen van traces op de web-app tijdens het laden wordt ontvangen.
Navigeer naar het deelvenster Profiler configureren:


### <a name="step-3-view-traces"></a>Stap 3: Weergave traceringen

Nadat de profiler is voltooid, volg de instructies op de melding om naar de pagina en de weergave prestatietraces te gaan.

## <a name="troubleshooting-on-demand-profiler"></a>Profiler voor het oplossen van problemen op aanvraag

U ziet soms Profiler time-out foutbericht weergegeven na een sessie op aanvraag:

![Profiler-time-outfout][profiler-timeout]

Er zijn twee redenen waarom u deze fout ziet:

1. De sessie op aanvraag profiler is voltooid, maar de Application Insights duurde langer om de verzamelde gegevens te verwerken. Als gegevens is niet voltooid binnen 15 minuten wordt verwerkt, weergegeven de portal bericht met een time-out. Hoewel het na enige tijd, worden Profiler-traceringen weergegeven. Als dit gebeurt, moet u alleen het foutbericht voor nu negeren. We werken actief aan een oplossing.

1. Uw web-app heeft een oudere versie van Profiler-agent die beschikt niet over de functie op aanvraag. Als u Application Insights-profiel eerder ingeschakeld, is het waarschijnlijk moet u bijwerken van uw Profiler-agent te starten met de functie op aanvraag.
  
Volg deze stappen om te controleren en installeren van de meest recente Profiler:

1. Ga naar App-instellingen voor App-Services en controleer of de volgende instellingen zijn geconfigureerd:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Vervangen door de juiste instrumentatiesleutel voor Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 als een van deze instellingen zijn niet ingesteld, gaat u naar het deelvenster voor het inschakelen van Application Insights voor het installeren van de recentste site-extensie.

1. Ga naar Application Insights-venster in App Services-portal.

    ![Application Insights inschakelen vanuit de portal voor App Services][enable-app-insights]

1. Als u een knop 'Update' in de volgende pagina ziet, klikt u erop om bij te werken van Application Insights-extensie voor site waarop de meest recente Profiler-agent wordt geïnstalleerd.

    ![Update-site-uitbreiding][update-site-extension]

1. Klik vervolgens op **wijzigen** om te zorgen dat de Profiler is ingeschakeld en selecteer **OK** de wijzigingen op te slaan.

    ![Wijzigen en opslaan van app insights][change-and-save-appinsights]

1. Ga terug naar **App-instellingen** tabblad voor de App Service om te controleren van de volgende items voor de app-instellingen zijn ingesteld:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: Vervangen door de juiste instrumentatiesleutel voor application insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![App-instellingen voor profiler][app-settings-for-profiler]

1. (Optioneel) Controleer de versie van de extensie en ervoor te zorgen dat er is geen update beschikbaar.

    ![selectievakje voor het bijwerken van de extensie][check-for-extension-update]

## <a name="next-steps"></a>Volgende stappen
[Traceringen Profiler inschakelen en weergeven](app-insights-profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/app-insights-profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler-settings/configure-profiler-entry.png
[create-performance-test]: ./media/app-insights-profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/app-insights-profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler-settings/profiler-timeout.png