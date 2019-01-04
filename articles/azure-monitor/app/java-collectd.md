---
title: Bewaken van prestaties van een Java-web-apps, op Linux - Azure | Microsoft Docs
description: Uitgebreide bewaking van toepassingsprestaties van Java-website met de invoegtoepassing verzamelde voor Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: a57422b4bbac4b929a67495bfffeec7bf0476e9c
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000220"
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>verzamelde: Linux-prestatiegegevens in Application Insights


Om te verkennen van metrische gegevens voor prestaties Linux-systeem in [Application Insights](../../application-insights/app-insights-overview.md), installeer [verzamelde](https://collectd.org/), samen met de bijbehorende Application Insights-invoegtoepassing. Deze open-source-oplossing worden verschillende systeem- en statistische gegevens verzameld.

Doorgaans gebruikt u verzamelde als u al hebt [uw Java-webservice met Application Insights geïnstrumenteerd][java]. Dit biedt u meer gegevens u helpen bij het verbeteren van de prestaties van uw app of problemen diagnosticeren. 

![Voorbeeldgrafieken](./media/java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>De instrumentatiesleutel ophalen
In de [Microsoft Azure portal](https://portal.azure.com), open de [Application Insights](../../application-insights/app-insights-overview.md) resource waar u de gegevens worden weergegeven. (Of [Maak een nieuwe resource](../../application-insights/app-insights-create-new-resource.md).)

Neemt een kopie van de instrumentatiesleutel die de resource.

![Door alles bladeren, opent u de bron en klik vervolgens in de vervolgkeuzelijst Essentials Selecteer en kopieer de Instrumentatiesleutel](./media/java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a>Verzamelde en de invoegtoepassing installeren
Op uw Linux-server-machines:

1. Installeer [verzamelde](https://collectd.org/) versie 5.4.0 of hoger.
2. Download de [Application Insights verzamelde schrijver invoegtoepassing](https://aka.ms/aijavasdk). Houd er rekening mee het versienummer.
3. Kopieer de invoegtoepassing JAR in `/usr/share/collectd/java`.
4. Bewerken `/etc/collectd/collectd.conf`:
   * Zorg ervoor dat [de Java-invoegtoepassing](https://collectd.org/wiki/index.php/Plugin:Java) is ingeschakeld.
   * De JVMArg voor de java.class.path om op te nemen van de volgende JAR bijwerken. Update het versienummer overeenkomen met de naam die u hebt gedownload:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Dit fragment met behulp van de Instrumentatiesleutel van uw resource toevoegen:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Hier maakt deel uit van een voorbeeld-configuratiebestand:

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

Configureer andere [verzamelde invoegtoepassingen](https://collectd.org/wiki/index.php/Table_of_Plugins), dat kan verschillende gegevens verzamelen uit verschillende bronnen.

Opnieuw opstarten van verzamelde volgens de [handmatige](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-the-data-in-application-insights"></a>De gegevens in Application Insights weergeven
Open in uw Application Insights-resource [Metrics Explorer en grafieken toevoegen][metrics], de metrische gegevens die u wilt zien van de aangepaste categorie selecteren.

![](./media/java-collectd/result.png)

Standaard worden de metrische gegevens worden samengevoegd voor alle hostmachines waarvan de metrische gegevens zijn verzameld. Als u wilt weergeven van de metrische gegevens per host, op de blade grafiek, schakelt u groeperen en kies vervolgens te groeperen op verzamelde-Host.

## <a name="to-exclude-upload-of-specific-statistics"></a>Uploaden van specifieke statistieken uitsluiten
Standaard verzendt de Application Insights-invoegtoepassing voor alle gegevens die door alle ingeschakelde verzamelde lezen invoegtoepassingen worden verzameld. 

Gegevens uitsluiten van specifieke invoegtoepassingen of gegevensbronnen:

* Het configuratiebestand bewerken. 
* In `<Plugin ApplicationInsightsWriter>`, voeg richtlijn regels als volgt toe:

| Richtlijn | Effect |
| --- | --- |
| `Exclude disk` |Uitsluiten van alle gegevens die zijn verzameld door de `disk` invoegtoepassing |
| `Exclude disk:read,write` |Uitsluiten van de bronnen die met de naam `read` en `write` uit de `disk` invoegtoepassing. |

Afzonderlijke richtlijnen met een nieuwe regel.

## <a name="problems"></a>Problemen?
*Ik zie niet de gegevens in de portal*

* Open [zoeken] [ diagnostic] om te zien als de ruwe gebeurtenissen zijn ontvangen. Soms duurt langer in metrics explorer worden weergegeven.
* U moet mogelijk [instellen van firewalluitzonderingen voor uitgaande gegevens](../../azure-monitor/app/ip-addresses.md)
* Tracering inschakelen in de Application Insights-invoegtoepassing. Voeg deze regel in `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Open een terminal en verzamelde starten in de uitgebreide modus, om te zien van problemen met die het rapport is:
  * `sudo collectd -f`

## <a name="known-issue"></a>Bekend probleem

De invoegtoepassing Application Insights schrijven is niet compatibel met bepaalde invoegtoepassingen lezen. Sommige invoegtoepassingen verzenden soms "NaN" waar de Application Insights-invoegtoepassing wordt verwacht een getal met drijvende komma dat.

Probleem: Het logboek verzamelde bevat fouten die zijn 'AI:... SyntaxError: Onverwacht token N".

Tijdelijke oplossing: Gegevens die zijn verzameld door de probleem-invoegtoepassingen schrijven uitsluiten. 

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md


