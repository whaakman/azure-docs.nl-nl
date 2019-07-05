---
title: Azure Monitor - Azure Application Insights overschrijven SDK Standaardeindpunten | Microsoft Docs
description: Azure Application Insights-SDK Standaardeindpunten voor regio's, zoals Azure Government wijzigen.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mbullwin
ms.openlocfilehash: d086815373b84c0f2a70144a505108875fc04981
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443322"
---
 # <a name="application-insights-overriding-default-endpoints"></a>Application Insights Standaardeindpunten overschrijven

Voor het verzenden van gegevens uit Application Insights in bepaalde regio's, moet u de standaard-eindpuntadressen overschrijven. Elke SDK vereist een wijziging van iets anders, die allemaal worden beschreven in dit artikel. Deze wijzigingen vereisen voor het aanpassen van de voorbeeldcode en vervang de tijdelijke aanduiding voor waarden voor `QuickPulse_Endpoint_Address`, `TelemetryChannel_Endpoint_Address`, en `Profile_Query_Endpoint_address` met de werkelijke eindpuntadressen voor uw specifieke regio. Het einde van dit artikel bevat koppelingen naar de eindpuntadressen voor regio's waar deze configuratie vereist is.

## <a name="sdk-code-changes"></a>Wijzigingen in de code SDK

### <a name="net-with-applicationinsightsconfig"></a>.NET met applicationinsights.config

> [!NOTE]
> Het bestand applicationinsights.config overschreven automatisch op elk moment een upgrade van een SDK wordt uitgevoerd. Zorg dat u de waarden van de specifieke eindpunt regio opnieuw invoeren na het uitvoeren van een upgrade van de SDK.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

### <a name="net-core"></a>.NET Core

Het bestand appsettings.json in uw project als volgt om aan te passen van het eindpunt van de belangrijkste te wijzigen:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

De waarden voor Live metrische gegevens en het eindpunt van de Query-profiel kunnen alleen worden ingesteld via code. Als u wilt overschrijven de standaardwaarden voor alle waarden van het eindpunt via code, breng de volgende wijzigingen in de `ConfigureServices` -methode van de `Startup.cs` bestand:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton(new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //place in ConfigureServices method. If present, place this prior to   services.AddApplicationInsightsTelemetry("instrumentation key");
```

### <a name="java"></a>Java

Wijzigen van het applicationinsights.xml-bestand als u wilt wijzigen van het adres van het standaard-eindpunt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

Wijzig de `application.properties` bestand en voeg toe:

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

De eindpunten kunnen ook worden geconfigureerd via omgevingsvariabelen:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

### <a name="javascript"></a>Javascript

```javascript
<script type="text/javascript">
   var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){
      function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t
   }({
      instrumentationKey:"INSTRUMENTATION_KEY"
      endpointUrl: "TelemetryChannel_Endpoint_Address"
   });

   window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

## <a name="regions-that-require-endpoint-modification"></a>Regio's die moeten worden aangepast eindpunt

De enige regio waarvoor endpoint wijzigingen zijn momenteel [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights) en [Azure China](https://docs.microsoft.com/azure/china/resources-developer-guide).

|Regio |  De naam van eindpunt | Value |
|-----------------|:------------|:-------------|
| Azure China | Telemetrie-kanaal | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure China | QuickPulse (Live metrische gegevens) |`https://quickpulse.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure China | Profiel-Query |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Government | Telemetrie-kanaal |`https://dc.applicationinsights.us/v2/track` |
| Azure Government | QuickPulse (Live metrische gegevens) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Government | Profiel-Query |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

> [!NOTE]
> Zonder code agentextensie op basis van bewaking voor Azure App Services is **momenteel niet ondersteund** in deze regio's. In dit artikel wordt bijgewerkt zodra deze functionaliteit beschikbaar wordt.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over de wijzigingen voor Azure Government, raadpleegt u de gedetailleerde richtlijnen voor [Azure controle en beheer](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights).
- Voor meer informatie over Azure China, raadpleegt u de [Azure China Playbook](https://docs.microsoft.com/azure/china/).