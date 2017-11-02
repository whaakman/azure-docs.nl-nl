---
title: Azure Application Insights voor Windows-server en -werkrollen | Microsoft Docs
description: De Application Insights SDK handmatig toevoegen aan uw ASP.NET-toepassing om gebruik, beschikbaarheid en prestaties te analyseren.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: bf7921926c69fff214e94a2d0edc22b28fb8eac0
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a>Application Insights handmatig configureren voor .NET-toepassingen

U kunt [Application Insights](app-insights-overview.md) configureren voor het bewaken van allerlei verschillende toepassingen of toepassingsrollen, onderdelen of microservices. Voor web-apps en services biedt Visual Studio [configuratie in één stap](app-insights-asp-net.md). Voor andere typen .NET-toepassingen, zoals back-end-serverfuncties of -desktoptoepassingen, kunt u Application Insights handmatig configureren.

![Voorbeeld van grafieken met prestatiebewaking](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Voordat u begint

U hebt de volgende zaken nodig:

* Een abonnement op [Microsoft Azure](http://azure.com). Als uw team of organisatie een Azure-abonnement heeft, kan de eigenaar u toevoegen met behulp van uw [Microsoft-account](http://live.com).
* Visual Studio 2013 of later.

## <a name="add"></a>1. Een Application Insights-resource kiezen

De 'resource' is de plek waar uw gegevens worden verzameld en weergegeven in Azure Portal. U moet beslissen of u een nieuwe resource maakt of een bestaande gaat delen.

### <a name="part-of-a-larger-app-use-existing-resource"></a>Onderdeel van een grotere app: bestaande resource gebruiken

Als de webtoepassing verschillende onderdelen heeft (zoals een front-end web-app en een of meer back-end services), moet u telemetriegegevens van alle onderdelen naar dezelfde resource verzenden. Op deze manier kunnen de onderdelen worden weergegeven in één toepassingsoverzicht en is het mogelijk om een aanvraag van het ene naar het andere onderdeel te traceren.

Dus als u al andere onderdelen van deze app bewaakt, gebruikt u gewoon dezelfde resource.

Open de resource in [Azure Portal](https://portal.azure.com/). 

### <a name="self-contained-app-create-a-new-resource"></a>Zelfstandige app: nieuwe resource maken

Als de nieuwe app helemaal losstaat van andere toepassingen, moet de app een eigen resource hebben.

Meld u aan bij de [Azure Portal](https://portal.azure.com/) en maak een nieuwe Application Insights-resource. Kies ASP.NET als het toepassingstype.

![Klik op Nieuw > Application Insights](./media/app-insights-windows-services/01-new-asp.png)

Het gekozen toepassingstype bepaalt de standaardinhoud van de resourceblades.

## <a name="2-copy-the-instrumentation-key"></a>2. De instrumentatiesleutel kopiëren
De sleutel identificeert de bron. U gaat de sleutel straks installeren in de SDK om gegevens om te leiden naar de resource.

![Op Eigenschappen klikken, de sleutel selecteren en op Ctrl + C drukken](./media/app-insights-windows-services/02-props-asp.png)

## <a name="sdk"></a>3. Het Application Insights-pakket in uw toepassing installeren
De installatie en configuratie van het Application Insights-pakket varieert, afhankelijk van het platform waarmee u werkt. 

1. Klik in Visual Studio met de rechtermuisknop op het project en kies **NuGet-pakketten beheren**.
   
    ![Klik met de rechtermuisknop op het project en selecteer NuGet-pakketten beheren](./media/app-insights-windows-services/03-nuget.png)
2. Installeer het Application Insights-pakket voor Windows Server-apps, 'Microsoft.ApplicationInsights.WindowsServer'.
   
    ![Naar Application Insights zoeken](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Welke versie?*

    Selecteer **Inclusief prerelease** als u onze nieuwste functies wilt proberen. In de relevante documenten of blogs wordt vermeld of u een prerelease-versie nodig hebt.
    
    *Kan ik andere pakketten gebruiken?*
   
    Ja. Kies Microsoft.ApplicationInsights als u de API alleen wilt gebruiken voor het verzenden van uw eigen telemetriegegevens. Het pakket voor Windows Server bevat de API plus een aantal andere pakketten, zoals prestatiemeterverzameling en afhankelijkheidsbewaking. 

### <a name="to-upgrade-to-future-package-versions"></a>Upgraden naar toekomstige pakketversies
Van tijd tot tijd brengen we een nieuwe versie van de SDK uit.

Als u wilt upgraden naar een [nieuwe release van het pakket](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), opent u NuGet-pakketbeheer opnieuw en filtert u op geïnstalleerde pakketten. Selecteer **Microsoft.ApplicationInsights.WindowsServer** en kies **Upgraden**.

Als u aanpassingen in ApplicationInsights.config hebt aangebracht, slaat u hiervan een kopie op voordat u de upgrade uitvoert. Voeg vervolgens uw wijzigingen samen in de nieuwe versie.

## <a name="4-send-telemetry"></a>4. Telemetrie verzenden
**Als u alleen het API-pakket hebt geïnstalleerd:**

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

### <a name="no-data"></a>Zijn er geen gegevens?
* Gebruik de toepassing om verschillende pagina's te openen, zodat er telemetrie wordt gegenereerd.
* Open de tegel [Zoeken](app-insights-diagnostic-search.md) om afzonderlijke gebeurtenissen te bekijken. Soms kan het even duren voordat de metrische gegevens van gebeurtenissen zijn opgehaald.
* Wacht een paar seconden en klik op **Vernieuwen**. De grafieken worden regelmatig vernieuwd, maar u kunt ze ook handmatig vernieuwen als u op bepaalde gegevens wacht.
* Zie [Probleemoplossing](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Uw app publiceren
Implementeer nu uw toepassing naar uw server of naar Azure en bekijk hoe de gegevens worden verzameld.

![Visual Studio gebruiken om uw app te publiceren](./media/app-insights-windows-services/15-publish.png)

Wanneer u de foutopsporingsmodus gebruikt, wordt de telemetrie direct doorgegeven, zodat u binnen enkele seconden gegevens ziet verschijnen. Wanneer u uw app in de Release-configuratie implementeert, duurt het langer om gegevens te verzamelen.

### <a name="no-data-after-you-publish-to-your-server"></a>Ziet u geen gegevens nadat u uw app naar uw server hebt gepubliceerd?
Open poorten voor uitgaand verkeer in de firewall van uw server. Raadpleeg [deze pagina](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) voor de lijst met vereiste adressen 

### <a name="trouble-on-your-build-server"></a>Problemen op uw buildserver?
Raadpleeg dit artikel voor [Probleemoplossing](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Als uw app veel telemetriegegevens genereert, beperkt de adaptieve steekproefmodule automatisch het volume dat naar de portal wordt verzonden door alleen een representatieve fractie van de gebeurtenissen te sturen. Gebeurtenissen die betrekking hebben op dezelfde aanvraag, worden echter als groep geselecteerd of gedeselecteerd, zodat u tussen gerelateerde gebeurtenissen kunt navigeren. 
> [Meer informatie over steekproeven](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen
* [Voeg meer telemetrie](app-insights-asp-net-more.md) toe om een volledig inzicht in uw toepassing te krijgen.

