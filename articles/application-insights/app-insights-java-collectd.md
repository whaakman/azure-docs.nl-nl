---
title: Java-web-appprestaties op Linux - Azure controleren | Microsoft Docs
description: Uitgebreide bewaking van toepassingsprestaties van uw Java-website met de invoegtoepassing CollectD voor Application Insights.
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: cde0fc020f1774e0e7669e7573e4aaff3534b34c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>collectd: Linux maatstaven voor prestaties in Application Insights


Om te verkennen systeemprestaties in Linux [Application Insights](app-insights-overview.md), installeren [collectd](http://collectd.org/), samen met de Application Insights-invoegtoepassing. Deze oplossing open source worden verschillende statistieken voor systeem- en netwerkgegevens verzameld.

Doorgaans kunt u collectd als u al hebt [uw Java-webservice met Application Insights geïnstrumenteerd][java]. Dit biedt u meer gegevens kunt verbeteren de prestaties van uw app of het analyseren van problemen. 

![voorbeeldgrafieken](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>De instrumentatiesleutel ophalen
In de [Microsoft Azure-portal](https://portal.azure.com), open de [Application Insights](app-insights-overview.md) resource waar u de gegevens wilt weergeven. (Of [Maak een nieuwe resource](app-insights-create-new-resource.md).)

Duren voordat u een kopie van de instrumentatiesleutel die de resource identificeert.

![Alles bekijken, opent u de bron en klik vervolgens in de vervolgkeuzelijst Essentials selecteren en de Instrumentatiesleutel kopiëren](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a>Installeer collectd en de invoegtoepassing
Op de Linux-servers:

1. Installeer [collectd](http://collectd.org/) versie 5.4.0 of hoger.
2. Download de [Application Insights-invoegtoepassing voor collectd writer](https://aka.ms/aijavasdk). Noteer het versienummer.
3. Kopieer de invoegtoepassing JAR in `/usr/share/collectd/java`.
4. Bewerken `/etc/collectd/collectd.conf`:
   * Zorg ervoor dat [de Java-invoegtoepassing](https://collectd.org/wiki/index.php/Plugin:Java) is ingeschakeld.
   * De JVMArg voor de java.class.path zodanig dat de volgende JAR bijwerken. Update het versienummer overeenkomen met de naam die u hebt gedownload:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * In dit fragment, met behulp van de Instrumentatiesleutel van uw resource toevoegen:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Dit is onderdeel van een voorbeeldconfiguratiebestand:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Configureer andere [collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), dat kan verschillende gegevens verzamelen van verschillende gegevensbronnen.

Opnieuw opstarten collectd volgens de [handmatige](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-the-data-in-application-insights"></a>De gegevens in Application Insights weergeven
Open in uw Application Insights-resource [Metrics Explorer en grafieken toe te voegen][metrics], de metrische gegevens die u wilt zien, uit de categorie Aangepast selecteren.

![](./media/app-insights-java-collectd/result.png)

Standaard worden de metrische gegevens geaggregeerd alle host machines waaruit de metrische gegevens zijn verzameld. Om weer te geven de metrische gegevens per host in de grafiek details blade, schakelt u Grouping en kies vervolgens groeperen op CollectD-Host.

## <a name="to-exclude-upload-of-specific-statistics"></a>Uploaden van specifieke statistieken uitsluiten
De Application Insights-invoegtoepassing verzendt standaard alle gegevens die door de ingeschakelde collectd lezen plugins verzameld. 

Gegevens uitsluiten van specifieke invoegtoepassingen of gegevensbronnen:

* Het configuratiebestand bewerken. 
* In `<Plugin ApplicationInsightsWriter>`, richtlijn regels als volgt toevoegen:

| Richtlijn | Effect |
| --- | --- |
| `Exclude disk` |Uitsluiten van alle gegevens die worden verzameld door de `disk` invoegtoepassing |
| `Exclude disk:read,write` |Uitsluiten van de bronnen met de naam `read` en `write` van de `disk` invoegtoepassing. |

Afzonderlijke richtlijnen met een nieuwe regel.

## <a name="problems"></a>Problemen?
*Gegevens in de portal weergegeven niet*

* Open [Search] [ diagnostic] om te zien als de onbewerkte gebeurtenissen zijn ontvangen. Soms duurt langer in metrics explorer wordt weergegeven.
* Mogelijk moet u [firewalluitzonderingen voor uitgaande gegevens instellen](app-insights-ip-addresses.md)
* Tracering in de Application Insights-invoegtoepassing inschakelen. Deze regel in te voegen `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Open een terminal en collectd starten in de uitgebreide modus om te zien of er problemen zijn meldt:
  * `sudo collectd -f`

## <a name="known-issue"></a>Bekende problemen

Het schrijven van Application Insights-invoegtoepassing is niet compatibel met bepaalde invoegtoepassingen lezen. Sommige invoegtoepassingen verzenden soms 'NaN', waarbij de Application Insights-invoegtoepassing een getal met drijvende komma verwacht.

Symptoom: Het logboek collectd bevat fouten die zijn 'AI:... SyntaxError: onverwacht token N ".

Tijdelijke oplossing: Uitsluiten gegevens die door het probleem schrijven plugins verzameld. 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


