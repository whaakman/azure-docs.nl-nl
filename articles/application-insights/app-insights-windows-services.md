---
title: Azure Application Insights voor Windows-server en -werkrollen | Microsoft Docs
description: De Application Insights SDK handmatig toevoegen aan uw ASP.NET-toepassing om gebruik, beschikbaarheid en prestaties te analyseren.
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 29598f052778759ed362e3aa4b997acb799717ef
ms.lasthandoff: 04/13/2017


---
# <a name="manually-configure-application-insights-for-aspnet-applications"></a>Application Insights handmatig configureren voor ASP.NET-toepassingen
[Application Insights](app-insights-overview.md) is een uitbreidbaar hulpprogramma voor webontwikkelaars dat u helpt de prestaties en het gebruik van uw live-toepassing te bewaken. U kunt het hulpprogramma handmatig configureren voor het bewaken van uw Windows-server, werkrollen en andere ASP.NET-toepassingen. Voor web-apps is handmatige configuratie een alternatief voor de [automatische configuratie](app-insights-asp-net.md) die wordt aangeboden door Visual Studio.

![Voorbeeld van grafieken met prestatiebewaking](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Voordat u begint
U hebt de volgende zaken nodig:

* Een abonnement op [Microsoft Azure](http://azure.com). Als uw team of organisatie een Azure-abonnement heeft, kan de eigenaar u toevoegen met behulp van uw [Microsoft-account](http://live.com).
* Visual Studio 2013 of later.

## <a name="add"></a>1. Een Application Insights-resource maken
Meld u aan bij de [Azure Portal](https://portal.azure.com/) en maak een nieuwe Application Insights-resource. Kies ASP.NET als het toepassingstype.

![Klik op Nieuw > Application Insights](./media/app-insights-windows-services/01-new-asp.png)

Een [resource](app-insights-resources-roles-access-control.md) in Azure is een exemplaar van een service. In deze resource wordt de telemetrie van uw app geanalyseerd en aan u gepresenteerd.

Op basis van het gekozen toepassingstype wordt de standaardinhoud van de resourceblades bepaald, net als de eigenschappen die zichtbaar zijn in [Metrics Explorer](app-insights-metrics-explorer.md).

#### <a name="copy-the-instrumentation-key"></a>De instrumentatiesleutel kopiëren
De sleutel geeft aan wat de resource is. U installeert de sleutel in het begin in de SDK om gegevens om te leiden naar de resource.

![Op Eigenschappen klikken, de sleutel selecteren en op Ctrl + C drukken](./media/app-insights-windows-services/02-props-asp.png)

De stappen die u zojuist hebt gevolgd om een nieuwe resource te maken, vormen ook een goed startpunt om te beginnen met het bewaken van een toepassing. U kunt er nu gegevens naar verzenden.

## <a name="sdk"></a>2. Het Application Insights-pakket in uw toepassing installeren
De installatie en configuratie van het Application Insights-pakket varieert, afhankelijk van het platform waarmee u werkt. Voor ASP.NET-apps is het eenvoudig.

1. Bewerk in Visual Studio de NuGet-pakketten van uw web-app-project.
   
    ![Klik met de rechtermuisknop op het project en selecteer NuGet-pakketten beheren](./media/app-insights-windows-services/03-nuget.png)
2. Installeer het Application Insights-pakket voor Windows-server-apps.
   
    ![Naar Application Insights zoeken](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Kan ik andere pakketten gebruiken?*
   
    Ja. Kies de Core API (Microsoft.ApplicationInsights) als u de API alleen wilt gebruiken voor het verzenden van uw eigen telemetrie. Het pakket voor Windows Server omvat automatisch de Core API plus een aantal andere pakketten zoals prestatiemeterverzameling en afhankelijkheidsbewaking. 

#### <a name="to-upgrade-to-future-package-versions"></a>Upgraden naar toekomstige pakketversies
Van tijd tot tijd brengen we een nieuwe versie van de SDK uit.

Als u wilt upgraden naar een [nieuwe release van het pakket](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), opent u NuGet-pakketbeheer opnieuw en filtert u op geïnstalleerde pakketten. Selecteer **Microsoft.ApplicationInsights.WindowsServer** en kies **Upgraden**.

Als u aanpassingen in ApplicationInsights.config hebt aangebracht, slaat u hiervan een kopie op voordat u de upgrade uitvoert. Voeg vervolgens uw wijzigingen samen in de nieuwe versie.

## <a name="3-send-telemetry"></a>3. Telemetrie verzenden
**Als u alleen het Core API-pakket hebt geïnstalleerd:**

* Stel de instrumentatiesleutel in code in, bijvoorbeeld in`main()`: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "` *uw sleutel* `";` 
* [Schrijf uw eigen telemetrie met behulp van de API](app-insights-api-custom-events-metrics.md#ikey).

**Als u andere Application Insights-pakketten hebt geïnstalleerd** kunt u, indien gewenst, het .config-bestand gebruiken om de instrumentatiesleutel in te stellen:

* Bewerk ApplicationInsights.config (toegevoegd tijdens de NuGet-installatie). Voeg dit vlak vóór de afsluitcode in:
  
    `<InstrumentationKey>` *de instrumentatiesleutel die u hebt gekopieerd* `</InstrumentationKey>`
* Zorg ervoor dat de eigenschappen van ApplicationInsights.config in Solution Explorer zijn ingesteld op **Buildactie = Inhoud, Naar uitvoermap kopiëren = Kopiëren**.

Het is handig om de instrumentatiesleutel in code in te stellen als u [de sleutel voor verschillende buildconfiguraties wilt toepassen](app-insights-separate-resources.md). Als u de sleutel in code instelt, hoeft u deze niet in het `.config`-bestand in te stellen.

## <a name="run"></a> Uw project uitvoeren
Gebruik **F5** om uw toepassing te starten en uit te proberen. Open verschillende pagina’s om telemetrie te genereren.

In Visual Studio wordt bijgehouden hoeveel gebeurtenissen er zijn verzonden.

![Het aantal gebeurtenissen in Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Uw telemetrie weergeven
Ga naar de [Azure Portal](https://portal.azure.com/) en blader naar uw Application Insights-resource.

Zoek naar gegevens in de overzichtsgrafieken. Aanvankelijk ziet u slechts één of twee punten. Bijvoorbeeld:

![Klik verder voor meer gegevens](./media/app-insights-windows-services/12-first-perf.png)

Klik in een grafiek voor gedetailleerdere metrische gegevens. [Meer informatie over metrische gegevens.](app-insights-web-monitor-performance.md)

#### <a name="no-data"></a>Zijn er geen gegevens?
* Gebruik de toepassing om verschillende pagina's te openen, zodat er telemetrie wordt gegenereerd.
* Open de tegel [Zoeken](app-insights-diagnostic-search.md) om afzonderlijke gebeurtenissen te bekijken. Soms kan het even duren voordat de metrische gegevens van gebeurtenissen zijn opgehaald.
* Wacht een paar seconden en klik op **Vernieuwen**. De grafieken worden regelmatig vernieuwd, maar u kunt ze ook handmatig vernieuwen als u op bepaalde gegevens wacht.
* Zie [Probleemoplossing](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Uw app publiceren
Implementeer nu uw toepassing naar uw server of naar Azure en bekijk hoe de gegevens worden verzameld.

![Visual Studio gebruiken om uw app te publiceren](./media/app-insights-windows-services/15-publish.png)

Wanneer u de foutopsporingsmodus gebruikt, wordt de telemetrie direct doorgegeven, zodat u binnen enkele seconden gegevens ziet verschijnen. Wanneer u uw app in de Release-configuratie implementeert, duurt het langer om gegevens te verzamelen.

#### <a name="no-data-after-you-publish-to-your-server"></a>Ziet u geen gegevens nadat u uw app naar uw server hebt gepubliceerd?
Open poorten voor uitgaand verkeer in de firewall van uw server. Raadpleeg [deze pagina](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) voor de lijst met vereiste adressen 

#### <a name="trouble-on-your-build-server"></a>Problemen op uw buildserver?
Raadpleeg dit artikel voor [Probleemoplossing](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Als uw app veel telemetrie genereert (en u de ASP.NET-SDK-versie 2.0.0-beta3 of later gebruikt), beperkt de adaptieve steekproefmodule automatisch het volume dat naar de portal wordt verzonden door alleen een representatieve fractie van de gebeurtenissen te sturen. Gebeurtenissen die betrekking hebben op dezelfde aanvraag, worden echter als groep geselecteerd of gedeselecteerd, zodat u tussen gerelateerde gebeurtenissen kunt navigeren. 
> [Meer informatie over steekproeven](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen
* [Voeg meer telemetrie](app-insights-asp-net-more.md) toe om een volledig inzicht in uw toepassing te krijgen.


