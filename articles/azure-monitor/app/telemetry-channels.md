---
title: Telemetrie-kanalen in Azure Application Insights | Microsoft Docs
description: Over het aanpassen van de kanalen telemetrie in Application Insights-SDK's van Azure voor .NET en .NET Core.
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
ms.openlocfilehash: af00641123354831c7bf174a743ded2886343579
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561353"
---
# <a name="telemetry-channels-in-application-insights"></a>Kanalen telemetrie in Application Insights

Telemetrie-kanalen zijn een integraal onderdeel van de [Azure Application Insights-SDK's](../../azure-monitor/app/app-insights-overview.md). Deze beheren buffer en verzenden van telemetrie naar de Application Insights-service. De .NET en .NET Core-versies van de SDK's hebben twee ingebouwde telemetrie kanalen: `InMemoryChannel` en `ServerTelemetryChannel`. Dit artikel wordt beschreven elk kanaal in detail, met inbegrip van het kanaal gedrag aanpassen.

## <a name="what-are-telemetry-channels"></a>Wat zijn telemetrie kanalen?

Telemetrie-kanalen zijn verantwoordelijk voor het bufferen van telemetrie-items en deze te verzenden naar de Application Insights-service, waar ze zijn opgeslagen voor het uitvoeren van query's en -analyse. Een kanaal telemetrie is een klasse die de [ `Microsoft.ApplicationInsights.ITelemetryChannel` ](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet) interface.

De `Send(ITelemetry item)` methode van een telemetrie-kanaal wordt aangeroepen nadat alle telemetrie initializers en telemetrie-processors worden genoemd. Alle items verwijderd door een telemetrie-processor wordt niet het kanaal dus niet bereiken. `Send()` geen meestal stuurt de items naar de back-end direct. Normaal gesproken deze in het geheugen buffert en stuurt in batches gaat doen, voor de overdracht is efficiënter.

[Live Metrics Stream](live-stream.md) heeft ook een aangepast kanaal dat wordt gebruikt door het live streamen van telemetrie. Dit kanaal is onafhankelijk van het kanaal reguliere Telemetrie en dit document niet van toepassing op het.

## <a name="built-in-telemetry-channels"></a>Ingebouwde telemetrie kanalen

De Application Insights .NET en .NET Core SDK's wordt geleverd met twee ingebouwde kanalen:

* `InMemoryChannel`: Een lichtgewicht kanaal dat items in het geheugen buffert totdat deze zijn verzonden. Items worden gebufferd in het geheugen en dat elke 30 seconden wordt leeggemaakt, of wanneer 500 items in een buffer opgeslagen. Dit kanaal biedt minimale betrouwbaarheid garanties omdat deze niet opnieuw om telemetrie te verzenden na een storing proberen. Dit kanaal ook geen items op schijf bewaren, zodat alle niet-verzonden items verloren gaan permanent bij afsluiten van de toepassing zijn (vensters of niet). Dit kanaal implementeert een `Flush()` methode die kan worden gebruikt om synchroon een geforceerde-leegmaken van alle items in het geheugen telemetrie te maken. Dit kanaal is heel geschikt voor korte toepassingen waarbij een synchrone leegmaken is ideaal.

    Dit kanaal maakt deel uit van de grotere Microsoft.ApplicationInsights NuGet-pakket en is het standaardkanaal dat u de SDK gebruikt wanneer niets anders is geconfigureerd.

* `ServerTelemetryChannel`: Een meer geavanceerde kanaal met beleid voor opnieuw proberen en de mogelijkheid voor het opslaan van gegevens op een lokale schijf. Dit kanaal pogingen om telemetrie te verzenden als tijdelijke fouten optreden. Dit kanaal gebruikt lokale schijfopslag ook items op schijf behouden tijdens netwerkstoringen of telemetrie van hoge volumes. Vanwege deze mechanismen voor nieuwe pogingen en de lokale schijfopslag, dit kanaal wordt beschouwd als betrouwbaarder en wordt aanbevolen voor alle scenario's voor productie. Dit kanaal is de standaardinstelling voor [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) en [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) toepassingen die zijn geconfigureerd op basis van de officiële documentatie. Dit kanaal is geoptimaliseerd voor server-scenario's met langlopende processen. De [ `Flush()` ](#which-channel-should-i-use) methode die wordt geïmplementeerd door dit kanaal is niet synchroon.

    Dit kanaal wordt verzonden als de Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel NuGet-pakket en automatisch wordt verkregen wanneer u de Microsoft.ApplicationInsights.Web of Microsoft.ApplicationInsights.AspNetCore NuGet gebruiken het pakket.

## <a name="configure-a-telemetry-channel"></a>Configureer een telemetrie-kanaal

U configureren een kanaal telemetrie door deze te stellen aan de actieve telemetrie-configuratie. Voor ASP.NET-toepassingen, configuratie moet u de telemetrie channel-instantie instellen op `TelemetryConfiguration.Active`, of door het wijzigen van `ApplicationInsights.config`. Voor ASP.NET Core-toepassingen moet configuratie u het kanaal toe te voegen aan de afhankelijkheid injectie-Container.

De volgende secties ziet u voorbeelden van het configureren van de `StorageFolder` instellen voor het kanaal in verschillende toepassingstypen. `StorageFolder` is slechts een van de configureerbare instellingen. Zie voor de volledige lijst van configuratie-instellingen, [het gedeelte instellingen](telemetry-channels.md#configurable-settings-in-channels) verderop in dit artikel.

### <a name="configuration-by-using-applicationinsightsconfig-for-aspnet-applications"></a>Configuratie met behulp van ApplicationInsights.config voor ASP.NET-toepassingen

De volgende sectie van [ApplicationInsights.config](configuration-with-applicationinsights-config.md) ziet u de `ServerTelemetryChannel` kanaal dat is geconfigureerd met `StorageFolder` ingesteld op een aangepaste locatie:

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!-- Telemetry processors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

### <a name="configuration-in-code-for-aspnet-applications"></a>De configuratie in de code voor ASP.NET-toepassingen

De volgende code stelt u een 'ServerTelemetryChannel'-exemplaar met `StorageFolder` ingesteld op een aangepaste locatie. Voeg deze code aan het begin van de toepassing, meestal toe `Application_Start()` methode in Global.aspx.cs.

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

### <a name="configuration-in-code-for-aspnet-core-applications"></a>De configuratie in de code voor ASP.NET Core-toepassingen

Wijzig de `ConfigureServices` -methode van de `Startup.cs` klasse zoals hier wordt weergegeven:

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

> [!IMPORTANT]
> Het kanaal configureren met behulp van `TelemetryConfiguration.Active` wordt niet aanbevolen voor ASP.NET Core-toepassingen.

### <a name="configuration-in-code-for-netnet-core-console-applications"></a>Configuratie in de code waarmee consoletoepassingen.NET/.NET Core

Voor console-apps is de code hetzelfde voor .NET en .NET Core:

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>Operationele details van ServerTelemetryChannel

`ServerTelemetryChannel` winkels die binnenkomen items in een buffer in het geheugen. De items die zijn geserialiseerd, gecomprimeerd en opgeslagen in een `Transmission` exemplaar elke 30 seconden, of als buffer 500 items hebben is opgeslagen. Een enkel `Transmission` exemplaar bevat maximaal 500 items en een batch van telemetrie die is verzonden via een enkel HTTPS-oproep naar de Application Insights-service vertegenwoordigt.

Standaard maximaal 10 `Transmission` exemplaren kunnen parallel worden verzonden. Als telemetrie sneller tarieven binnenkomen, of als het netwerk of de Application Insights-back-end traag is, `Transmission` exemplaren worden opgeslagen in het geheugen. De standaardcapaciteit van deze in-memory `Transmission` buffer is 5 MB. Wanneer de capaciteit in het geheugen is overschreden, `Transmission` exemplaren worden opgeslagen op lokale schijf tot een maximum van 50 MB. `Transmission` exemplaren worden ook op de lokale schijf opgeslagen wanneer er problemen met het netwerk zijn. Alleen de items die zijn opgeslagen op een lokale schijf overleven een crash van de toepassing. Deze zijn verzonden wanneer de toepassing opnieuw wordt gestart.

## <a name="configurable-settings-in-channels"></a>Configureerbare instellingen voor kanalen

Voor de volledige lijst met configureerbare instellingen voor elk kanaal, Zie:

* [InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

* [ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

Hier volgen de meest gebruikte instellingen voor `ServerTelemetryChannel`:

1. `MaxTransmissionBufferCapacity`: De maximale hoeveelheid geheugen in bytes, dat wordt gebruikt door het kanaal verzendingen buffer in het geheugen. Wanneer deze capaciteit is bereikt, worden nieuwe items rechtstreeks naar de lokale schijf opgeslagen. De standaardwaarde is 5 MB. Instellen van een hogere waarde leidt tot minder gebruik van de schijf, maar houd er rekening mee dat items in het geheugen worden verbroken als de toepassing vastloopt.

1. `MaxTransmissionSenderCapacity`: Het maximum aantal `Transmission` exemplaren die worden verzonden naar Application Insights op hetzelfde moment. De standaardwaarde is 10. Deze instelling kan worden geconfigureerd op een hoger getal, dit wordt aanbevolen wanneer er een grote hoeveelheid telemetrie wordt gegenereerd. Hoog volume treedt meestal op tijdens het testen van de belasting of wanneer steekproeven is uitgeschakeld.

1. `StorageFolder`: De map die wordt gebruikt door het kanaal voor het opslaan van items naar de schijf naar behoefte. In Windows, % LOCALAPPDATA % of % TEMP % gebruikt als er geen ander pad expliciet is opgegeven. In omgevingen dan Windows, moet u dat een geldige locatie of de telemetriegegevens worden niet bewaard op lokale schijf.

## <a name="which-channel-should-i-use"></a>Kanalen moet ik gebruiken?

`ServerTelemetryChannel` wordt aanbevolen voor de meeste scenario's voor productie met betrekking tot langlopende toepassingen. De `Flush()` methode geïmplementeerd door `ServerTelemetryChannel` is niet synchroon, en biedt het ook geen garantie voor het verzenden van alle in behandeling items uit het geheugen of schijfruimte. Als u dit kanaal in scenario's waar de toepassing wordt afgesloten, raden wij aan dat u enige vertraging na het aanroepen introduceren `Flush()`. De exacte hoeveelheid vertraging die u mogelijk niet voorspelbaar. Dit is afhankelijk van factoren, zoals het aantal items of `Transmission` -instanties zijn in het geheugen, het aantal op schijf zijn, hoeveel zijn ze worden verzonden naar de back-end en of het kanaal is in het midden van exponentieel uitstel scenario's.

Als u doen synchrone leeggemaakt wilt, raden wij aan dat u `InMemoryChannel`.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="does-the-application-insights-channel-guarantee-telemetry-delivery-if-not-what-are-the-scenarios-in-which-telemetry-can-be-lost"></a>Garandeert de Application Insights-kanaal telemetrie levering? Als dat niet het geval is, wat zijn de scenario's waarin telemetrie kunt verloren?

De korte antwoord is dat geen van de ingebouwde kanalen gegarandeerd transactietype telemetrie te leveren aan de back-end. `ServerTelemetryChannel` is meer geavanceerde in vergelijking met `InMemoryChannel` voor betrouwbare bezorging, maar het is ook alleen een best-effort poging om telemetrie te verzenden. Telemetrie kan nog steeds verloren gaan in verschillende situaties, met inbegrip van deze algemene scenario's:

1. Items in het geheugen bevinden, verloren wanneer de toepassing vastloopt.

1. Telemetrie is verloren gegaan gedurende langere perioden van problemen met het netwerk. Telemetrie wordt naar de lokale schijf opgeslagen tijdens netwerkstoringen of wanneer er problemen optreden met de back-end van de Application Insights. Items die ouder zijn dan 24 uur worden echter verwijderd.

1. De standaardlocaties voor de schijf voor het opslaan van telemetriegegevens in Windows zijn % LOCALAPPDATA % of % TEMP %. Deze locaties liggen meestal lokaal op de computer. Als de toepassing worden gemigreerd fysiek van de ene locatie naar een andere, is geen telemetriegegevens die zijn opgeslagen in de oorspronkelijke locatie gaan verloren.

1. In de Web-Apps op Windows is de standaardlocatie voor schijfopslag D:\local\LocalAppData. Deze locatie is niet persistent gemaakt. Deze zijn gewist in de app opnieuw wordt opgestart, schaal-outs en andere dergelijke bewerkingen, leiden tot verlies van alle telemetrie die is opgeslagen. U kunt deze standaardinstelling negeren en opslag aan een permanente locatie zoals D:\home opgeven. Echter dergelijke persistente locaties door externe opslag worden aangeboden en dus kunnen traag zijn.

### <a name="does-servertelemetrychannel-work-on-systems-other-than-windows"></a>Werkt ServerTelemetryChannel op systemen dan Windows?

Hoewel de naam van het pakket en de naamruimte 'WindowsServer' bevat, wordt dit kanaal wordt ondersteund op systemen dan Windows, met de volgende uitzondering. Het kanaal maken niet een lokale opslag-map standaard op systemen dan Windows. U moet een map met lokale opslag maken en configureren van het kanaal om deze te gebruiken. Nadat de lokale opslag is geconfigureerd, wordt in het kanaal op dezelfde manier werkt op alle systemen.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>Maakt de SDK tijdelijk lokale opslag? Worden de gegevens versleuteld tegen?

Tijdens het problemen met het netwerk of beperking, de SDK telemetrie-items opslaat in de lokale opslag. Deze gegevens wordt niet lokaal versleuteld.

Voor Windows-systemen, de SDK automatisch wordt gemaakt van een lokale tijdelijke map in de map % LOCALAPPDATA % of % TEMP % en beperkt de toegang voor beheerders en alleen de huidige gebruiker.

Er is geen lokale opslag wordt automatisch gemaakt door de SDK voor systemen dan Windows, en dus geen gegevens lokaal opgeslagen standaard. U kunt zelf een opslag-directory maken en configureren van het kanaal om deze te gebruiken. In dit geval bent u verantwoordelijk om ervoor te zorgen dat de map is beveiligd.
Meer informatie over [gegevensbescherming en privacy](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage).

## <a name="open-source-sdk"></a>Open-source-SDK
Kanalen zijn net als elke SDK voor Application Insights, open-source. Lees- en bijdragen aan de code of problemen te melden bij [de officiële GitHub-opslagplaats](https://github.com/Microsoft/ApplicationInsights-dotnet).

## <a name="next-steps"></a>Volgende stappen

* [Steekproeven](../../azure-monitor/app/sampling.md)
* [Het oplossen van SDK](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
