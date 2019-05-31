---
title: Telemetrie-kanalen in Azure Application Insights | Microsoft Docs
description: Over het aanpassen van telemetrie kanalen in Azure Application Insights-SDK's voor.NET/.NET Core.
services: application-insights
documentationcenter: .net
author: cijothomas
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/14/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: fa99e29e9ec6c2bef7296a50dd381ee5fc60a1cb
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255808"
---
# <a name="telemetrychannel-in-application-insights"></a>TelemetryChannel in Application Insights

TelemetryChannel is een integraal onderdeel van [Azure Application Insights-SDK's](../../azure-monitor/app/app-insights-overview.md). Hiermee worden beheerd buffer en verzenden van telemetrie naar de Application Insights-service. De .NET en .NET Core-versies van de SDK's hebben twee ingebouwde TelemetryChannels - `InMemoryChannel` en `ServerTelemetryChannel`. Dit artikel wordt beschreven elk kanaal in detail, met inbegrip van hoe gebruikers kanaal gedrag kunnen aanpassen.

## <a name="what-is-a-telemetrychannel"></a>Wat is een TelemetryChannel?

`TelemetryChannel` is verantwoordelijk voor het bufferen van en het verzenden van telemetrie-items naar Application Insights-service, waar deze zijn opgeslagen voor het uitvoeren van query's en -analyse. Het is een klasse die de interface implementeren [`Microsoft.ApplicationInsights.ITelemetryChannel`](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet)

De `Send(ITelemetry item)` -methode van TelemetryChannel heet nadat alle `TelemetryInitializer`s en `TelemetryProcessor`s worden genoemd. Dit betekent dat alle items verwijderd door `TelemetryProcessor` het kanaal wordt niet bereiken. `Send()` Normaal gesproken verzendt geen items onmiddellijk naar de back-end. Ze zijn doorgaans gebufferde in het geheugen en in batches, voor de overdracht is efficiënt verzonden.

[LiveMetrics](live-stream.md) heeft ook een aangepast kanaal, dat wordt gebruikt door het live streamen van telemetrie. Dit kanaal is onafhankelijk van het kanaal reguliere Telemetrie en dit document geldt niet voor het kanaal dat wordt gebruikt door `LiveMetrics`.

## <a name="built-in-telemetrychannels"></a>Ingebouwde TelemetryChannels

Application Insights.NET/.NET Core-SDK wordt geleverd met twee ingebouwde kanalen:

* **InMemoryChannel** 
 `InMemoryChannel` is een lichte-kanaal, dat items in het geheugen buffert totdat deze wordt verzonden. Items worden gebufferd in het geheugen en dat elke 30 seconden wordt leeggemaakt, of wanneer 500 items hebben gebufferd. Dit kanaal biedt minimale betrouwbaarheid garanties als deze niet opnieuw om telemetrie te verzenden van fouten proberen. Dit kanaal behouden niet items op schijf, zodat alle niet-verzonden items verloren gaan permanent bij afsluiten van de toepassing zijn (zonder problemen of niet). Er is een `Flush()` methode die is geïmplementeerd door dit kanaal, dat kan worden gebruikt voor synchroon een geforceerde-leegmaken van alle items in het geheugen telemetrie te maken. Dit is heel geschikt voor korte toepassingen waarbij een synchrone leegmaken is ideaal.

    Dit kanaal is geleverd als onderdeel van de `Microsoft.ApplicationInsights` nuget pakket zelf, en het kanaal is standaard de SDK gebruikt wanneer niets anders is geconfigureerd.

* **ServerTelemetryChannel** 
 `ServerTelemetryChannel` is een meer geavanceerde kanaal met beleid voor opnieuw proberen en de mogelijkheid voor het opslaan van gegevens op de lokale schijf. Dit kanaal pogingen om telemetrie te verzenden, als tijdelijke fouten optreden. Dit kanaal gebruikt lokale schijfopslag ook items op schijf behouden tijdens netwerkstoringen of telemetrie van hoge volumes. Vanwege deze mechanismen voor nieuwe pogingen en de lokale schijfopslag, dit kanaal wordt beschouwd als betrouwbaarder en wordt aanbevolen voor alle scenario's voor productie. Dit kanaal is de standaardinstelling voor [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) en [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) toepassingen die aan de hand van de gekoppelde officiële documenten zijn geconfigureerd. Dit kanaal is geoptimaliseerd voor scenario's voor server van langlopende processen. De [ `Flush()` ](#which-channel-should-i-use) methode die is geïmplementeerd door dit kanaal is niet synchroon.

    Dit kanaal wordt verzonden als de NuGet-pakket `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel`, en is automatisch geplaatst wanneer u een van de NuGet-pakketten `Microsoft.ApplicationInsights.Web` of `Microsoft.ApplicationInsights.AspNetCore`.

## <a name="configuring-telemetrychannel"></a>TelemetryChannel configureren

Telemetrie-kanaal kan worden geconfigureerd door in te stellen de gewenste `TelemetryChannel` op de actieve `TelemetryConfiguration`. Voor Asp.Net-toepassingen, configuratie omvat de volgende instelling `TelemetryChannel` op `TelemetryConfiguration.Active`, of wijzigen van `ApplicationInsights.config`. Voor ASP.NET Core-toepassingen moet configuratie u het gewenste kanaal toe te voegen aan de afhankelijkheid injectie-Container.

Hieronder ziet u een voorbeeld waarin de gebruiker is configureren de `StorageFolder` voor het kanaal. `StorageFolder` is slechts een van de configureerbare instellingen. De volledige lijst met configuratie-instellingen wordt beschreven [in de sectie instellingen](telemetry-channels.md#configurable-settings-in-channel).

## <a name="configuration-using-applicationinsightsconfig-for-aspnet-applications"></a>Configuratie met behulp van ApplicationInsights.Config voor ASP.NET-toepassingen

De volgende sectie van [ApplicationInsights.config](configuration-with-applicationinsights-config.md) wordt geconfigureerd met ServerTelemetryChannel `StorageFolder` ingesteld op een aangepaste locatie.

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!--TelemetryProcessors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

## <a name="configuration-in-code-for-aspnet-applications"></a>Configuratie in de code voor ASP.NET-toepassingen

De volgende code stelt u ServerTelemetryChannel met `StorageFolder` ingesteld op een aangepaste locatie. Deze code moet worden toegevoegd aan het begin van de toepassing, meestal in de methode Application_Start() in `Global.aspx.cs`

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
protected void Application_Start()
{
    var serverTelemetryChannel = new ServerTelemetryChannel();
    serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
    serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
    TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
}
```

## <a name="configuration-in-code-for-aspnet-core-applications"></a>Configuratie in de code voor ASP.NET Core-toepassingen

Wijzigen `ConfigureServices` -methode van `Startup.cs` klasse zoals hieronder wordt weergegeven.

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

public void ConfigureServices(IServiceCollection services)
{
    // This sets up ServerTelemetryChannel with StorageFolder set to a custom location.
    services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel() {StorageFolder = @"d:\temp\applicationinsights" });

    services.AddApplicationInsightsTelemetry();
}

```

> [!NOTE]
> Het is belangrijk te weten dat het kanaal via configureren `TelemetryConfiguration.Active` wordt niet aanbevolen voor ASP.NET Core-toepassingen.

## <a name="configuring-telemetrychannel-in-code-for-netnet-core-console-applications"></a>TelemetryChannel in de code voor.NET/.NET Core-Console-toepassingen configureren

Voor apps van de Console, de code is hetzelfde voor .NET en .NET Core en wordt hieronder weergegeven.

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>Operationele details van ServerTelemetryChannel

De `ServerTelemetryChannel` binnenkomen items in een buffer in het geheugen buffert. Het is geserialiseerd, gecomprimeerd en opgeslagen in `Transmission` exemplaar nadat elke 30 seconden of wanneer 500 items in een buffer opgeslagen. Een enkel `Transmission` exemplaar bevat maximaal 500 items en een batch van telemetrie wordt verzonden via een enkel https-oproep naar de Application Insights-service vertegenwoordigt. Standaard kunnen er maximaal 10 `Transmission`parallel s worden verzonden. Als telemetrie sneller tarieven binnenkomen of als back-end netwerk/Application Insights traag, klikt u vervolgens is `Transmission`s ophalen die zijn opgeslagen in het geheugen. De standaardcapaciteit van deze overdracht-buffer in het geheugen is 5 MB. Wanneer in het geheugen capaciteit overschrijdt, `Transmission`s worden opgeslagen op lokale schijf voor maximaal 50 MB. `Transmission`s worden opgeslagen op lokale schijf, terwijl er netwerkproblemen ook zijn. Alleen de items die zijn opgeslagen in de lokale schijf overleven vastlopen van een toepassing die worden verzonden wanneer de toepassing opnieuw wordt gestart.

## <a name="configurable-settings-in-channel"></a>Configureerbare instellingen in het kanaal

De volledige lijst met configureerbare instellingen voor elk kanaal zijn hier:

[InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

[ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

Instellingen voor het meest gebruikt `ServerTelemetryChannel` worden hieronder vermeld:

1. `MaxTransmissionBufferCapacity` -De maximale hoeveelheid geheugen in bytes, dat wordt gebruikt door het kanaal verzendingen buffer in het geheugen. Wanneer deze capaciteit is bereikt, worden nieuwe items rechtstreeks naar de lokale schijf opgeslagen. De standaardwaarde is 5 MB. Een hogere waarde leads wordt ingesteld op minder gebruik van de schijf, maar het is belangrijk te weten dat items in het geheugen verbroken worden als toepassing vastloopt.

2. `MaxTransmissionSenderCapacity` -De maximale hoeveelheid `Transmission`s die worden verzonden naar Application Insights op hetzelfde moment. De standaardwaarde is 10, maar deze kan worden geconfigureerd op een hogere getal. Dit wordt aanbevolen wanneer er een grote hoeveelheid telemetrie wordt gegenereerd, doorgaans bij het maken van de belastingstests en/of wanneer steekproeven is uitgeschakeld.

3. `StorageFolder` -De map die wordt gebruikt door het kanaal voor het opslaan van items naar de schijf naar behoefte. In Windows, % LocalAppData % of % Temp % gebruikt, als er niets expliciet is geconfigureerd. In omgevingen met niet-Windows-, gebruiker **moet** configureren van een geldige locatie, zonder dat telemetrie wordt niet worden opgeslagen op lokale schijf.

## <a name="which-channel-should-i-use"></a>Kanalen moet ik gebruiken?

`ServerTelemetryChannel` wordt aanbevolen voor de meeste scenario's voor productie van langlopende toepassingen. De `Flush()` methode-implementatie van `ServerTelemetryChannel` is niet synchroon, en `Flush()` is geen garantie voor het verzenden van alle in behandeling items van het geheugen/schijf. Als dit kanaal wordt gebruikt in scenario's waar de toepassing wordt afgesloten en vervolgens het verdient aanbeveling te doen namelijk altijd een vertraging na het aanroepen `Flush()` op dit kanaal. De exacte hoeveelheid vertraging die nodig is niet voorspelbaar is, omdat deze is afhankelijk van factoren, zoals het aantal objecten of `Transmissions` zijn in het geheugen, hoeveel zijn op de schijf, hoeveel zijn worden verzonden naar een back-gemaakt, en als het kanaal is in het midden van exponentieel uitstel scenario's. Als er behoefte aan synchrone leegmaken, klikt u vervolgens `InMemoryChannel` wordt aanbevolen.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="does-applicationinsights-channel-offer-guaranteed-telemetry-delivery-or-what-are-the-scenarios-where-telemetry-can-be-lost"></a>*Biedt Application Insights-kanaal telemetrie gegarandeerde levering of wat zijn de scenario's waarbij telemetrie verloren kan gaan?*

* Korte antwoord is dat geen van de ingebouwde kanalen transactie type garantie over telemetrie te leveren aan de back-end bieden. Terwijl `ServerTelemetryChannel` is meer geavanceerde in vergelijking met `InMemoryChannel` voor de levering van betrouwbare telemetrie, maakt het ook een best-effort poging om telemetrie te verzenden en telemetrie nog steeds verloren kan gaan in verschillende scenario's. De algemene scenario's waarbij telemetrie verloren gegaan is zijn onder andere:

1. Items in het geheugen gaan verloren wanneer de toepassing vastloopt.
1. Telemetrie wordt naar de lokale schijf opgeslagen tijdens netwerkstoringen of problemen met de Application Insights back-end. Items die ouder zijn dan 24 uur worden echter verwijderd. Telemetrie is dus verloren gaan tijdens de langere periode van netwerkproblemen.
1. De standaardlocaties voor de schijf voor het opslaan van telemetriegegevens in Windows zijn % LocalAppData % of % Temp %. Deze locaties liggen meestal lokaal op de computer. Als de toepassing worden gemigreerd fysiek van de ene locatie naar een andere, is geen telemetriegegevens die zijn opgeslagen op deze locatie gaan verloren.
1. In Azure Web Apps (Windows) is de standaardlocatie van de schijf 'D:\local\LocalAppData'. Deze locatie is niet permanent en in de app opnieuw opstarten is gewist, schaal outs enzovoort, leiden tot verlies van telemetriegegevens die zijn opgeslagen op deze locaties. Gebruikers kunnen de opslag naar een permanente locatie, zoals 'D:\home' negeren, maar deze persistente locaties onder door externe opslag worden verwerkt en kunnen traag zijn.

### <a name="does-servertelemetrychannel-work-in-non-windows-systems"></a>*Werkt ServerTelemetryChannel in niet-Windows-systemen?*

* Dit kanaal wordt ondanks de naam van het pakket/naamruimte WindowsServer wordt ondersteund in niet-Windows-systemen met de volgende uitzondering. Het kanaal niet een map met lokale opslag in niet-Windows-, standaard gemaakt. Gebruikers moeten een map met lokale opslag maken en configureren van het kanaal om deze te gebruiken. Wanneer lokale opslag is geconfigureerd, werkt het kanaal hetzelfde in Windows en niet-Windows-systemen.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>*Maakt de SDK tijdelijk lokale opslag? Worden de gegevens versleuteld tegen?*

* Tijdens het netwerkproblemen of beperking, SDK telemetrie-items opslaat in de lokale opslag. Deze gegevens worden niet lokaal versleuteld.
Voor Windows-systemen, de SDK automatisch wordt gemaakt van een lokale tijdelijke map in de map TEMP of APPDATA en beperkt de toegang voor beheerders en alleen de huidige gebruiker.
Er is geen lokale opslag wordt automatisch gemaakt door de SDK voor niet-Windows, en kan daarom geen gegevens lokaal opgeslagen standaard. Gebruikers kunnen zelf een opslagmap maken en configureren van het kanaal om deze te gebruiken. In dit geval is de gebruiker verantwoordelijk voor dat deze map wordt beveiligd.
Meer informatie over [gegevensbescherming en privacy](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage).

## <a name="open-source-sdk"></a>Open-source-SDK
Kanalen zijn net als elke Application Insights SDK's, ook open-source. Lezen en bijdragen aan de code of problemen melden [de officiële GitHub-opslagplaats](https://github.com/Microsoft/ApplicationInsights-dotnet).

## <a name="next-steps"></a>Volgende stappen

* [Steekproeven](../../azure-monitor/app/sampling.md)
* [Het oplossen van SDK](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
