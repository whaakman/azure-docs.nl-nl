---
title: Het deelvenster van de instellingen van Azure Application Insights Profiler gebruiken | Microsoft Docs
description: Profiler-status zien en sessies profileren starten
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 9603c45443c6339a127f977600eeff2ba57a283f
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884156"
---
# <a name="configure-application-insights-profiler"></a>Application Insights Profiler configureren

## <a name="profiler-settings-pane"></a>Deelvenster Profiler-instellingen

Als u wilt openen in het deelvenster van de instellingen Azure Application Insights Profiler, gaat u naar het deelvenster Insights de prestaties van toepassingen en selecteer vervolgens de **Profiler** knop.

![Het deelvenster Profiler configureren][configure-profiler-entry]

De **voor Application Insights Profiler configureren** deelvenster bevat vier onderdelen: 
* **Profiel nu**: Start-sessies voor alle apps die zijn gekoppeld aan dit exemplaar van Application Insights-profilering.
* **Gekoppelde apps**: Een lijst met toepassingen die profilering verzendt gegevens naar deze Application Insights-resource.
* **Sessies Bezig**: Geeft de status van de sessie wanneer u selecteert **profiel nu**. 
* **Recente sessies profilering**: Geeft informatie weer over de afgelopen profilering sessies.

![Profiler op aanvraag][profiler-on-demand]

## <a name="app-service-environment"></a>App Service-omgeving
Afhankelijk van hoe uw Azure App Service-omgeving is geconfigureerd, kan de aanroep om te controleren op de agentstatus wordt geblokkeerd. Het deelvenster mogelijk een bericht dat de agent is niet actief, zelfs wanneer deze wordt uitgevoerd weergegeven. Om ervoor te zorgen dat het is, Controleer de webtaak op uw toepassing. Als alle waarden van de app-instellingen juist zijn en de Application Insights-site-extensie is geïnstalleerd op uw toepassing, wordt Profiler uitgevoerd. Als uw toepassing voldoende verkeer ontvangt, moeten in een lijst met recente profilering sessies weergegeven.

## <a id="profileondemand"></a> Profiler handmatig activeren

### <a name="minimum-requirements"></a>Minimale vereisten 
Voor een gebruiker handmatig activeren een profiler-sessie moeten ze ten minste "schrijftoegang' op hun rol voor de Application Insights-onderdeel. In de meeste gevallen krijgt u deze toegang automatisch en geen extra werk is vereist. Als u problemen ondervindt, zou de rol "Application Insights-Onderdeelinzender" zijn in het bereik abonnement de rol toe te voegen. [Zie voor meer informatie over rollen van toegangsbeheer met Azure Monitoring](https://docs.microsoft.com/en-us/azure/azure-monitor/app/resources-roles-access-control).

Met één klik kunt u Profiler handmatig activeren. Stel dat u een webprestatietesten uitvoert. Hebt u traceringen om te begrijpen hoe uw web-app wordt uitgevoerd onder belasting nodig. Controle over wanneer traceringen worden vastgelegd met is cruciaal omdat u weet wanneer de belastingstest wordt uitgevoerd. Maar het interval van steekproeven mogelijk worden gemist.

De volgende secties laten zien hoe dit scenario werkt:

### <a name="step-1-optional-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>Stap 1: (Optioneel) Verkeer naar uw web-app genereren op basis van een WebTest voor prestaties

Als uw web-app is al het inkomende verkeer of als u alleen wilt handmatig genereren verkeer, kunt u deze sectie overslaan en doorgaan naar stap 2.

1. Selecteer in de Application Insights-webportal **configureren** > **prestatietests**. 

1. Selecteer eerst een nieuwe prestatietest de **nieuw** knop.

   ![maken van nieuwe prestatietest][create-performance-test]

1. In de **nieuwe prestatietest** in het deelvenster voor het configureren van de test doel-URL. Accepteer alle standaardinstellingen, en selecteer vervolgens **uitvoeren test** beginnen met het uitvoeren van de belastingstest.

    ![Configureren van load test][configure-performance-test]

    De nieuwe test staat in de wachtrij eerst, gevolgd door de status van *Bezig*.

    ![belastingstest wordt verzonden en in de wachtrij geplaatst][load-test-queued]

    ![belastingstest wordt uitgevoerd in voortgang][load-test-in-progress]

### <a name="step-2-start-a-profiler-on-demand-session"></a>Stap 2: Een Profiler-op-verzoek-sessie starten

1. Wanneer de belastingstest wordt uitgevoerd, start u de Profiler voor het vastleggen van traces op de web-app tijdens het laden wordt ontvangen.

1. Ga naar de **Profiler configureren** deelvenster.


### <a name="step-3-view-traces"></a>Stap 3: Weergave traceringen

Nadat de Profiler is voltooid, volg de instructies op de melding om naar het deelvenster en prestatietraces te gaan.

## <a name="troubleshoot-the-profiler-on-demand-session"></a>Oplossen van de Profiler-sessie op aanvraag

Na een sessie op aanvraag ontvangt u mogelijk een foutbericht van Profiler time-out:

![Profiler-time-outfout][profiler-timeout]

U kunt dit foutbericht voor een van de volgende redenen:

* De sessie op aanvraag Profiler is voltooid, maar de Application Insights duurde langer dan verwacht om de verzamelde gegevens te verwerken.  

  Als de gegevens niet binnen 15 minuten verwerkt, wordt de portal een time-bericht. Na een tijdje worden echter worden Profiler-traceringen weergegeven. Als u een foutbericht ontvangt, kunt u deze voor nu negeren. We werken actief aan een oplossing.

* Uw web-app heeft een oudere versie van Profiler-agent die beschikt niet over de functie op aanvraag.  

  Als u Application Insights Profiler eerder ingeschakeld, moet u mogelijk uw Profiler-agent te starten met de functie op aanvraag bijwerken.
  
Ga naar de App-Services **App-instellingen** deelvenster en controleer of de volgende instellingen:
* **APPINSIGHTS_INSTRUMENTATIONKEY**: Vervangen door de juiste instrumentatiesleutel voor Application Insights.
* **APPINSIGHTS_PORTALINFO**: ASP.NET
* **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

Als een van de bovenstaande waarden niet zijn ingesteld, moet u de meest recente site-extensie installeren door het volgende te doen:

1. Ga naar de **Application Insights** deelvenster in de portal voor App Services.

    ![Application Insights inschakelen vanuit de portal voor App Services][enable-app-insights]

1. Als de **Application Insights** deelvenster wordt een **bijwerken** knop, selecteert u het bijwerken van de extensie van de Application Insights-site waarop de meest recente Profiler-agent wordt geïnstalleerd.

    ![Update-site-uitbreiding][update-site-extension]

1. Om ervoor te zorgen dat de Profiler is ingeschakeld, selecteert u **wijziging**, en selecteer vervolgens **OK** de wijzigingen op te slaan.

    ![Wijzigen en opslaan van app insights][change-and-save-appinsights]

1. Ga terug naar **App-instellingen** deelvenster voor het App Service om ervoor te zorgen dat de volgende waarden zijn ingesteld:
   * **APPINSIGHTS_INSTRUMENTATIONKEY**: Vervangen door de juiste instrumentatiesleutel voor application insights.
   * **APPINSIGHTS_PORTALINFO**: ASP.NET 
   * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

     ![App-instellingen voor Profiler][app-settings-for-profiler]

1. Selecteer desgewenst **extensies**, en controleer de versie van de extensie en bepalen of een update beschikbaar is.

    ![selectievakje voor het bijwerken van de extensie][check-for-extension-update]

## <a name="next-steps"></a>Volgende stappen
[Profiler inschakelen en weergeven van traceringen](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png
