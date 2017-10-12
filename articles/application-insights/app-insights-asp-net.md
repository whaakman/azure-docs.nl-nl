---
title: Web-app-analyse voor ASP.NET instellen met Azure Application Insights | Microsoft Docs
description: Configureer prestaties, beschikbaarheid en gebruiksanalyse voor uw ASP.NET-website die on-premises of in Azure wordt gehost.
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: cb247ee68da88265f7c51258644064463d44f8b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Application Insights instellen voor uw ASP.NET-website

Met deze procedure wordt uw ASP.NET web-app zo geconfigureerd dat telemetrie wordt verzonden naar de [Azure Application Insights](app-insights-overview.md)-service. Dit werkt voor ASP.NET-apps die worden gehost op uw eigen IIS-server of in de cloud. U beschikt over grafieken en een krachtige querytaal waarmee u meer inzicht krijgt in de prestaties van uw app en hoe mensen deze gebruiken. Daarnaast ontvangt u meldingen over fouten of prestatieproblemen. Veel ontwikkelaars gebruiken deze functies graag, maar u kunt de telemetrie desgewenst uitbreiden en aanpassen.

Voor de Setup zijn maar een paar klikken nodig in Visual Studio. U kunt kosten besparen door de hoeveelheid telemetrie te beperken. Hiermee kunt u experimenteren en fouten opsporen of een site met weinig gebruikers bewaken. Wanneer u besluit dat u de productiesite wilt bewaken, kunt u de limiet later eenvoudig verhogen.

## <a name="before-you-start"></a>Voordat u begint
U hebt de volgende zaken nodig:

* Visual Studio 2013 update 3 of later. Later is beter.
* Een abonnement op [Microsoft Azure](http://azure.com). Als uw team of organisatie een Azure-abonnement heeft, kan de eigenaar u toevoegen met behulp van uw [Microsoft-account](http://live.com).

Er zijn ook andere onderwerpen die u kunt bekijken als u geïnteresseerd bent in:

* [Een web-app instrumenteren tijdens runtime](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a> Stap 1: de Application Insights-SDK toevoegen

Klik in Solution Explorer met de rechtermuisknop op het web-app-project. Kies **Toevoegen** > **Application Insights Telemetry...** of **Application Insights configureren**.

![Schermopname van Solution Explorer waarin Application Insights Telemetry toevoegen is gemarkeerd](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(In Visual Studio 2015 is er ook een optie om Application Insights toe te voegen aan het dialoogvenster Nieuw project.)

Ga door naar de pagina voor Application Insights-configuratie:

![Schermopname van de pagina Uw app registreren bij Application Insights](./media/app-insights-asp-net/visual-studio-register-dialog.png)

**a.** Selecteer het account en het abonnement die u gebruikt voor toegang tot Azure.

**b.** Selecteer de resource in Azure die u wilt gebruiken om de gegevens van uw app te bekijken. Meestal:

* Gebruik [één resource voor verschillende onderdelen](app-insights-monitor-multi-role-apps.md) van één toepassing. 
* Maak afzonderlijke resources voor niet-gerelateerde toepassingen.
 
Als u een resourcegroep of locatie wilt instellen voor het opslaan van uw gegevens, klikt u op **Instellingen configureren**. Resourcegroepen worden gebruikt om toegang tot gegevens te beheren. Als u verschillende apps hebt die deel uitmaken van hetzelfde systeem, kunt u hun Application Insights-gegevens in dezelfde resourcegroep plaatsen.

**c.** Stel de limiet in op het gratis gegevensvolume om kosten te voorkomen. Application Insights is gratis te gebruiken, tot een bepaald telemetrievolume. Wanneer de resource is gemaakt, kunt u de selectie in de portal wijzigen door naar **Functies en prijzen** > **Gegevensvolumebeheer** > **Dagelijkse volumelimiet** te gaan.

**d.** Klik op **Registreren** om Application Insights te configureren voor uw web-app. Er wordt telemetrie verzonden naar [Azure Portal](https://portal.azure.com), zowel tijdens de foutopsporing als na het publiceren van de app.

**e.** Als u geen telemetrie naar de portal wilt verzenden tijdens de foutopsporing, voegt u de Application Insights SDK toe aan de app, maar configureert u geen resource in de portal. U kunt de telemetrie in Visual Studio bekijken tijdens de foutopsporing. U kunt later terugkeren naar deze configuratiepagina, of u kunt wachten tot de app is geïmplementeerd en [telemetrie inschakelen tijdens runtime](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a> Stap 2: uw app uitvoeren
Voer uw app uit met F5. Open verschillende pagina's om telemetrie te genereren.

In Visual Studio ziet u het aantal gebeurtenissen dat is geregistreerd.

![Schermopname van Visual Studio. Tijdens het opsporen van fouten wordt de knop Application Insights weergegeven.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a>Stap 3: uw telemetrie weergeven
U ziet uw telemetrie in Visual Studio of in de Application Insights-webportal. Zoek telemetrie in Visual Studio om fouten in de app op te sporen. Bewaak de prestaties en het gebruik in de webportal wanneer het systeem live is. 

### <a name="see-your-telemetry-in-visual-studio"></a>De telemetrie bekijken in Visual Studio

Open het venster Application Insights in Visual Studio. Klik op de knop **Application Insights** of klik met de rechtermuisknop op uw project in Solution Explorer, selecteer vervolgens **Application Insights** en klik daarna op **Search Live Telemetry** (Live telemetrie zoeken).

Ga in het venster Visual Studio Application Insights naar de weergave **Gegevens van foutopsporingssessie** voor telemetrie die is gegenereerd aan de serverzijde van uw app. Experimenteer met de filters en klik op een gebeurtenis voor meer details.

![Schermopname van de weergave Gegevens van foutopsporingssessie in het venster Application Insights.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Als u geen gegevens ziet, controleert u of het tijdsbereik correct is en klikt u op het pictogram Zoeken.

[Meer informatie over Application Insights-hulpprogramma's in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Telemetrie bekijken in de webportal

U kunt de telemetrie ook in de Application Insights-webportal bekijken (tenzij u ervoor hebt gekozen alleen de SDK te installeren). De portal bevat meer grafieken, analysefuncties en weergaven met meerdere onderdelen dan Visual Studio. De portal bevat ook waarschuwingen.

Open uw Application Insights-resource. Meld u aan bij [Azure Portal](https://portal.azure.com/) en zoek de resource daar of klik met de rechtermuisknop op het project in Visual Studio en laat u daarheen brengen.

![Schermopname van Visual Studio die toont hoe u de Application Insights-portal kunt openen](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Als u een toegangsfout ontvangt: hebt u meer dan één set Microsoft-referenties en bent u aangemeld met de verkeerde set? Meld u af en opnieuw aan in de portal.

De portal wordt geopend met een weergave van de telemetrie van uw app.

![Schermopname van Application Insights-overzichtspagina](./media/app-insights-asp-net/66.png)

Klik in de portal op een tegel of grafiek om meer details te bekijken.

[Meer informatie over het gebruik van Application Insights in Azure Portal](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Stap 4: uw app publiceren
Publiceer uw app op de IIS-server of op Azure. Bekijk de livestream met metrische gegevens in [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) om te controleren of alles goed werkt.

Uw telemetrie wordt opgebouwd in de Application Insights-portal, waar u metrische gegevens kunt controleren, uw telemetrie kunt doorzoeken en [dashboards](app-insights-dashboards.md) kunt instellen. U kunt ook de krachtige [querytaal van Log Analytics](https://docs.loganalytics.io/) gebruiken om gebruik en prestaties te analyseren of om specifieke gebeurtenissen te zoeken.

U kunt uw telemetrie ook blijven analyseren in [Visual Studio](app-insights-visual-studio.md) met hulpprogramma's voor diagnostisch zoeken en [Trends](app-insights-visual-studio-trends.md).

> [!NOTE]
> Als uw app zoveel telemetrie verzendt dat de[beperkingslimieten](app-insights-pricing.md#limits-summary) worden benaderd, worden automatisch [steekproeven](app-insights-sampling.md) ingeschakeld. Met steekproeven vermindert u de hoeveelheid telemetrie die vanuit uw app wordt verzonden, maar behoudt u gecorreleerde gegevens voor diagnostische doeleinden.
>
>

## <a name="land"></a> U bent nu klaar

Gefeliciteerd. U hebt het pakket Application Insights geïnstalleerd in uw app en dit zo geconfigureerd dat telemetrie wordt verzonden naar de Application Insights-service in Azure.

![Diagram van verplaatsing van telemetrie](./media/app-insights-asp-net/01-scheme.png)

De Azure-resource die de telemetrie van uw app ontvangt, wordt aangeduid met een *instrumentatiesleutel*. U vindt deze sleutel in het bestand ApplicationInsights.config.


## <a name="upgrade-to-future-sdk-versions"></a>Upgraden naar toekomstige SDK-versies
Als u wilt upgraden naar een [nieuwe release van de SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), opent u **NuGet-pakketbeheer** opnieuw en filtert u op geïnstalleerde pakketten. Selecteer **Microsoft.ApplicationInsights.Web** en kies **Upgraden**.

Als u aanpassingen in ApplicationInsights.config hebt aangebracht, slaat u hiervan een kopie op voordat u de upgrade uitvoert. Voeg de wijzigingen vervolgens samen in de nieuwe versie.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen

### <a name="more-telemetry"></a>Meer telemetrie

* **[Laadgegevens voor browser en pagina](app-insights-javascript.md)**: voeg een codefragment in uw webpagina's in.
* **[Gedetailleerde bewaking van afhankelijkheid en uitzonderingen](app-insights-monitor-performance-live-website-now.md)**: installeer Status Monitor op uw server.
* **[Codeer aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md)** om gebruikersacties te tellen, te meten of de tijdsduur hiervan te bepalen.
* **[Haal logboekgegevens op](app-insights-asp-net-trace-logs.md)**: koppel logboekgegevens aan uw telemetrie.

### <a name="analysis"></a>Analyse

* **[Met Application Insights werken in Visual Studio](app-insights-visual-studio.md)**<br/>Bevat informatie over foutopsporing met telemetrie, het doorzoeken van diagnostische gegevens en het in detail analyseren van code.
* **[Werken met de Application Insights-portal](app-insights-dashboards.md)**<br/> Bevat informatie over dashboards, krachtige hulpprogramma's voor diagnose en analyse, waarschuwingen, een live afhankelijkheidskaart van uw toepassing en exportmogelijkheden voor telemetrie.
* **[Analyse](app-insights-analytics-tour.md)**: de krachtige querytaal.

### <a name="alerts"></a>Waarschuwingen

* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md): maak tests om ervoor te zorgen dat uw site zichtbaar is op internet.
* [Slimme diagnostische gegevens](app-insights-proactive-diagnostics.md): deze tests worden automatisch uitgevoerd, zodat u niets hoeft te doen om ze in te stellen. Deze geeft aan of een app een ongebruikelijk aantal mislukte aanvragen heeft.
* [Metrische waarschuwingen](app-insights-alerts.md): stel deze in om u te waarschuwen als een metriek een drempelwaarde overschrijdt. U kunt deze instellen op aangepaste metrische gegevens die u in uw app codeert.

### <a name="automation"></a>Automation

* [Het maken van een Application Insights-resource automatiseren](app-insights-powershell.md)
