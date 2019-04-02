---
title: Bewaken van prestaties van Azure app services | Microsoft Docs
description: Prestaties controleren voor Azure app services. Grafiek laden en reactietijd, afhankelijkheidsinformatie en waarschuwingen kunt instellen op de prestaties.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: mbullwin
ms.openlocfilehash: 7386f6bd92143cf3fb7b37725900425f99371cd0
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804989"
---
# <a name="monitor-azure-app-service-performance"></a>Azure App Service-prestaties bewaken

Inschakelen van de controle van uw .NET en .NET Core op basis van webtoepassingen die worden uitgevoerd in Azure App Services is nu eenvoudiger dan ooit. Dat u eerder hebt u nodig hebt om een site-extensie handmatig installeren, worden de nieuwste extensie/agent nu in de installatiekopie van het app service standaard ingebouwd. In dit artikel wordt u stapsgewijs door het inschakelen van bewaking van Application Insights, evenals voorlopige richtlijnen voor het automatiseren van het proces voor grootschalige implementaties.

> [!NOTE]
> Handmatig toevoegen van een Application Insights-extensie voor site via **ontwikkeltools** > **extensies** is afgeschaft. De nieuwste stabiele versie van de extensie is nu [vooraf](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) als onderdeel van de installatiekopie van het App Service. De bestanden bevinden zich in `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent` en worden automatisch bijgewerkt met elke stabiele versie. Als u de agent op basis van-instructies voor het inschakelen van bewaking volgt hieronder, de afgeschafte extensie automatisch worden verwijderd voor u.

## <a name="enable-application-insights"></a>Application Insights inschakelen

Er zijn twee manieren om in te schakelen toepassingsbewaking voor toepassingen in Azure App Services die worden gehost:

* **Bewaking van toepassingen op basis van een agent** (ApplicationInsightsAgent).  
    * Deze methode is de eenvoudigste manier om in te schakelen en er zijn geen geavanceerde configuratie is vereist. Dit wordt vaak aangeduid als '' runtimecontrole. Voor Azure App Services we raden ten minste inschakelen dit niveau van controle en klik vervolgens op basis van uw specifieke scenario die u evalueren kunt of meer geavanceerde controle via handmatige instrumentation nodig is.

* **De toepassing via code handmatig te instrumenteren** door het installeren van de Application Insights-SDK.

    * Deze aanpak is veel meer kan worden aangepast, maar hiervoor [toevoegen van een afhankelijkheid op de Application Insights SDK NuGet-pakketten](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Deze methode betekent ook dat u hebt voor het beheren van de updates naar de nieuwste versie van de pakketten.

    * Als u maken van aangepaste API-aanroepen naar gebeurtenissen bijhouden/afhankelijkheden niet standaard wilt met het controleren op basis van een agent worden vastgelegd, moet u deze methode wilt gebruiken. Bekijk de [API voor aangepaste gebeurtenissen en metrische gegevens artikel](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) voor meer informatie.

> [!NOTE]
> Als de controle van de agent op basis van zowel handmatige SDK op basis van instrumentation wordt gedetecteerd dat alleen de instellingen voor handmatige instrumentation zal worden herkend. Zo wordt voorkomen dat dubbele gegevens verzonden. Voor meer informatie over deze controle uit de [sectie troubleshooting](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting) hieronder.

## <a name="enable-agent-based-monitoring-net"></a>Schakel bewaking .NET op basis van een agent

1. **Selecteer Application Insights** in de Azure-Configuratiescherm voor uw appservice.

    ![Kies Application Insights onder instellingen.](./media/azure-web-apps/settings-app-insights-01.png)

   * Te kiezen om een nieuwe resource te maken, tenzij u al een Application Insights-resource voor deze toepassing instellen. 

     > [!NOTE]
     > Wanneer u klikt op **OK** te maken van de nieuwe resource wordt u gevraagd naar **controle-instellingen toepassen**. Selecteren **doorgaan** uw nieuwe Application Insights-resource wordt gekoppeld aan uw appservice, doet dit het geval is, wordt ook **activeert u opnieuw opstarten van uw appservice**. 

     ![Uw web-app instrumenteren](./media/azure-web-apps/create-resource-01.png)

2. Na het op te geven welke resource moet worden gebruikt, kunt u kiezen hoe application insights voor het verzamelen van gegevens per platform voor uw toepassing. ASP.NET-app-controle op standaard wordt met twee verschillende niveaus van de verzameling.

    ![Kies opties per platform](./media/azure-web-apps/choose-options-new.png)

   * .NET **basic verzameling** niveau essentiële single instance APM mogelijkheden biedt.

   * .NET **verzameling aanbevolen** niveau:
       * Voegt de CPU, geheugen en i/o trends in gebruik.
       * Correleert microservices grenzen van de aanvraag/afhankelijkheid.
       * Trends in gebruik worden verzameld, en kunt correlatie van resultaten van beschikbaarheid voor transacties.
       * Verzamelt de uitzonderingen die niet is verwerkt door het hostproces.
       * Verbetert de nauwkeurigheid van de APM-metrische gegevens onder belasting, wanneer steekproeven wordt gebruikt.

3. Als u wilt configureren van instellingen, zoals steekproeven, die u eerder via het bestand applicationinsights.config beheren kan kunt u nu communiceert met die dezelfde instellingen via de toepassingsinstellingen in met een overeenkomende voorvoegsel. 

    * Als u wilt wijzigen van de eerste voorbeeldpercentage, kunt u bijvoorbeeld een toepassingsinstelling van maken: `MicrosoftAppInsights_AdaptiveSamplingTelemetryProcessor_InitialSamplingPercentage` en een waarde van `100`.

    * U kunt raadplegen voor een lijst van ondersteunde adaptieve steekproeven telemetrie processor-instellingen, de [code](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/master/src/ServerTelemetryChannel/AdaptiveSamplingTelemetryProcessor.cs) en [die zijn gekoppeld documentatie](https://docs.microsoft.com/azure/azure-monitor/app/sampling).

## <a name="enable-agent-based-monitoring-net-core"></a>Monitoring .NET Core op basis van een agent inschakelen

De volgende versies van .NET Core worden ondersteund: ASP.NET Core 2.0, ASP.NET Core, 2.1, ASP.NET Core 2.2

Die gericht is op het volledige framework vanaf .NET Core, onafhankelijke implementatie- en ASP.NET Core 3.0 zijn momenteel **niet ondersteund** met agentextensie op basis van bewaking. ([Handmatige instrumentation](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) via programmacode werkt in alle vorige scenario's.)

1. **Selecteer Application Insights** in de Azure-Configuratiescherm voor uw appservice.

    ![Kies Application Insights onder instellingen.](./media/azure-web-apps/settings-app-insights-01.png)

   * Te kiezen om een nieuwe resource te maken, tenzij u al een Application Insights-resource voor deze toepassing instellen. 

     > [!NOTE]
     > Wanneer u klikt op **OK** te maken van de nieuwe resource wordt u gevraagd naar **controle-instellingen toepassen**. Selecteren **doorgaan** uw nieuwe Application Insights-resource wordt gekoppeld aan uw appservice, doet dit het geval is, wordt ook **activeert u opnieuw opstarten van uw appservice**. 

     ![Uw web-app instrumenteren](./media/azure-web-apps/create-resource-01.png)

2. Na het op te geven welke resource moet worden gebruikt, kunt u kiezen hoe Application Insights voor het verzamelen van gegevens per platform voor uw toepassing. .NET core biedt **verzameling aanbevolen** of **uitgeschakelde** voor .NET Core 2.0 en 2.1 2.2.

    ![Kies opties per platform](./media/azure-web-apps/choose-options-new-net-core.png)

## <a name="enable-client-side-monitoring-net"></a>Client-side monitoring .NET inschakelen

Bewaking aan clientzijde is aanmelden voor ASP.NET. Aan de clientzijde bewaking inschakelen:

* Selecteer **instellingen** > ** ** toepassing instellingen ***
   * Voeg een nieuwe onder toepassingsinstellingen, **naam van de app-instelling** en **waarde**:

     Naam: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Waarde:`true`

   * Sla de instellingen op met **Opslaan** en start de app opnieuw met **Opnieuw opstarten**.

![Schermopname van de toepassing van instellingen voor gebruikersinterface](./media/azure-web-apps/appinsights-javascript-enabled.png)

Client-side bewaking een verwijderen van de bijbehorende sleutel-waardepaar uit de toepassingsinstellingen uitschakelen of de waarde ingesteld op false.

## <a name="enable-client-side-monitoring-net-core"></a>Client-side monitoring .NET Core inschakelen

Bewaking aan clientzijde is **standaard ingeschakeld** voor .NET Core-apps met **verzameling aanbevolen**, ongeacht of de app-instelling 'APPINSIGHTS_JAVASCRIPT_ENABLED' aanwezig is.

Als u wilt om een bepaalde reden Schakel bewaking aan clientzijde uit:

* Selecteer **instellingen** > **toepassingsinstellingen**
   * Voeg een nieuwe onder toepassingsinstellingen, **naam van de app-instelling** en **waarde**:

     Naam: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Waarde:`false`

   * Sla de instellingen op met **Opslaan** en start de app opnieuw met **Opnieuw opstarten**.

![Schermopname van de toepassing van instellingen voor gebruikersinterface](./media/azure-web-apps/appinsights-javascript-disabled.png)

## <a name="automate-monitoring"></a>Bewaking automatiseren

Alleen de instellingen van de toepassing moeten worden ingesteld zodat de verzameling van apparaattelemetrie met Application Insights:

   ![App Service-toepassing-instellingen met de beschikbare Application Insights-instellingen](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Definities van Application-instellingen

|Naam van de App-instelling |  Definitie | Value |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Belangrijkste uitbreiding, waarmee wordt bepaald runtimecontrole. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  In zijn de modus alleen essentiële functies ingeschakeld om te verzekeren van optimale prestaties. | `default` of `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | Bepaalt of de engine voor het binaire bestand herschrijven `InstrumentationEngine` wordt ingeschakeld. Deze instelling heeft gevolgen voor de prestaties en heeft gevolgen voor koude start/opstarttijd. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | Controleert of SQL en Azure-tabel tekst wordt samen met de afhankelijkheidsaanroepen worden vastgelegd. Van Prestatiewaarschuwing: deze instelling moet de `InstrumentationEngine`. | `~1` |

### <a name="app-service-application-settings-with-azure-resource-manager"></a>Instellingen voor App Service-toepassing met Azure Resource Manager

Toepassingsinstellingen voor App Services kunnen worden beheerd en geconfigureerd met [Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). Deze methode kan worden gebruikt bij het implementeren van nieuwe App Service-resources met Azure Resource Manager-automatisering, of voor het wijzigen van de instellingen van bestaande resources.

Lager is dan de basisstructuur van de toepassingsinstellingen JSON voor een appservice:

```JSON
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]"
          ],
          "tags": {
            "displayName": "Application Insights Settings"
          },
          "properties": {
            "key1": "value1",
            "key2": "value2"
          }
        }
      ]

```

Voor een voorbeeld van een Azure Resource Manager-sjabloon met toepassingsinstellingen geconfigureerd voor Application Insights, dit [sjabloon](https://github.com/Andrew-MSFT/BasicImageGallery) kan nuttig zijn, name de sectie vanaf [regel 238](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238).

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Automatiseer het maken van een Application Insights-resource en een koppeling naar de zojuist gemaakte App-Service.

Beginnen met het proces voor het maken van een Azure Resource Manager-sjabloon met alle standaard Application Insights instellingen die zijn geconfigureerd, als u van plan zijn een nieuwe Web-App maken met Application Insights is ingeschakeld.

Selecteer **opties voor Automation**

   ![Menu voor App Service web-app maken](./media/azure-web-apps/create-web-app.png)

Deze optie wordt de meest recente Azure Resource Manager-sjabloon gegenereerd met alle vereiste instellingen die zijn geconfigureerd.

  ![App Service web app-sjabloon](./media/azure-web-apps/arm-template.png)

Hieronder volgt een voorbeeld, Vervang alle exemplaren van `AppMonitoredSite` met de sitenaam van uw:

```json
{
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/sites",
            "properties": {
                "siteConfig": {
                    "appSettings": [
                        {
                            "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').InstrumentationKey]"
                        },
                        {
                            "name": "ApplicationInsightsAgent_EXTENSION_VERSION",
                            "value": "~2"
                        }
                    ]
                },
                "name": "[parameters('name')]",
                "serverFarmId": "[concat('/subscriptions/', parameters('subscriptionId'),'/resourcegroups/', parameters('serverFarmResourceGroup'), '/providers/Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "microsoft.insights/components/AppMonitoredSite"
            ],
            "apiVersion": "2016-03-01",
            "location": "[parameters('location')]"
        },
        {
            "apiVersion": "2016-09-01",
            "name": "[parameters('hostingPlanName')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "properties": {
                "name": "[parameters('hostingPlanName')]",
                "workerSizeId": "[parameters('workerSize')]",
                "numberOfWorkers": "1",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "sku": {
                "Tier": "[parameters('sku')]",
                "Name": "[parameters('skuCode')]"
            }
        },
        {
            "apiVersion": "2015-05-01",
            "name": "AppMonitoredSite",
            "type": "microsoft.insights/components",
            "location": "West US 2",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Request_Source": "IbizaWebAppExtensionCreate"
            }
        }
    ],
    "parameters": {
        "name": {
            "type": "string"
        },
        "hostingPlanName": {
            "type": "string"
        },
        "hostingEnvironment": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "sku": {
            "type": "string"
        },
        "skuCode": {
            "type": "string"
        },
        "workerSize": {
            "type": "string"
        },
        "serverFarmResourceGroup": {
            "type": "string"
        },
        "subscriptionId": {
            "type": "string"
        }
    },
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0"
}
```

> [!NOTE]
> De sjabloon wordt toepassingsinstellingen gegenereerd in de modus 'standaard'. Deze modus is geoptimaliseerd voor prestaties, maar u kunt de sjabloon voor het activeren van ongeacht welke functies u wilt aanpassen.

### <a name="enabling-through-powershell"></a>Inschakelen via PowerShell

De onderliggende toepassingsinstellingen moeten worden gewijzigd zodat de toepassing bewaken via PowerShell. Hieronder volgt een voorbeeld waarmee toepassingsbewaking voor een website met de naam 'AppMonitoredSite' in de resourcegroep 'AppMonitoredRG', en worden verzonden naar de instrumentatiesleutel '012345678-abcd-ef01-2345-6789abcd' configureert.

```powershell
$app = Get-AzureRmWebApp -ResourceGroupName "AppMonitoredRG" -Name "AppMonitoredSite" -ErrorAction Stop
$newAppSettings = @{} # case-insensitive hash map
$app.SiteConfig.AppSettings | %{$newAppSettings[$_.Name] = $_.Value} #preserve non Application Insights Application settings.
$newAppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"] = "012345678-abcd-ef01-2345-6789abcd"; # enable the ApplicationInsightsAgent
$newAppSettings["ApplicationInsightsAgent_EXTENSION_VERSION"] = "~2"; # enable the ApplicationInsightsAgent
$app = Set-AzureRmWebApp -AppSettings $newAppSettings -ResourceGroupName $app.ResourceGroup -Name $app.Name -ErrorAction Stop
```

## <a name="upgrade-monitoring-extensionagent"></a>Controle-extensie/agent bijwerken

### <a name="upgrading-from-versions-289-and-up"></a>Upgraden van versies 2.8.9 en omhoog

Upgraden vanaf versie 2.8.9 gebeurt automatisch, zonder extra handelingen. De nieuwe controle bits worden geleverd op de achtergrond voor de doel-app-service en toepassing opnieuw wordt opgestart ze opgehaald.

Als u wilt controleren welke versie van de extensie wordt bezoek uitgevoerd `http://yoursitename.scm.azurewebsites.net/ApplicationInsights`

![Schermopname van het url-pad http://yoursitename.scm.azurewebsites.net/ApplicationInsights](./media/azure-web-apps/extension-version.png)

### <a name="upgrade-from-versions-100---265"></a>Een upgrade uitvoert van versies 1.0.0 - 2.6.5

Vanaf versie 2.8.9 wordt de vooraf geïnstalleerde site-extensie gebruikt. Als u een eerdere versie, kunt u bijwerken via twee manieren:

* [Werk bij vóór inschakelen via de portal](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights). (Zelfs als u de extensie Application Insights voor Azure App Service is geïnstalleerd hebt, de gebruikersinterface geeft alleen **inschakelen** knop. Achter de schermen, wordt de oude persoonlijke site-extensie verwijderd.)

* [Bijwerken via PowerShell](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell):

    1. Stel de toepassingsinstellingen in de vooraf geïnstalleerde site-extensie ApplicationInsightsAgent inschakelen. Zie [inschakelen via powershell](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell).
    2. Verwijder handmatig de persoonlijke site-extensie met de naam van Application Insights-extensie voor Azure App Service.

Als de upgrade van een eerdere versie dan 2.5.1 is voltooid, controleert u dat de ApplicationInsigths dll-bestanden zijn verwijderd uit de map bin [Zie stappen voor probleemoplossing](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting).

## <a name="troubleshooting"></a>Problemen oplossen

Hieronder vindt u onze stapsgewijze gids voor probleemoplossing voor de extensie/agent op basis van bewaking van .NET en .NET Core-toepassingen uitvoeren in Azure App Services.

> [!NOTE]
> Java en Node.js-toepassingen worden alleen ondersteund in Azure App Services via handmatige op basis van SDK instrumentatie en daarom de onderstaande stappen niet van toepassing op deze scenario's.

1. Controleer of de toepassing wordt bewaakt via `ApplicationInsightsAgent`.
    * Controleer of ' ApplicationInsightsAgent_EXTENSION_AGENT app-instelling is ingesteld op een waarde van '~ 2'.
2. Zorg ervoor dat de toepassing voldoet aan de vereisten om te worden bewaakt.
    * Blader naar `https://yoursitename.scm.azurewebsites.net/ApplicationInsights`

    ![Schermafbeelding van https://yoursitename.scm.azurewebsites/applicationinsights pagina met zoekresultaten](./media/azure-web-apps/app-insights-sdk-status.png)

    * Bevestig dat de `Application Insights Extension Status` is `Pre-Installed Site Extension, version 2.8.12.1527, is running.`
        * Als deze niet wordt uitgevoerd, volgt u de [instructies controle van Application Insights inschakelen](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights)

    * Controleer of de status-bron bestaat en lijkt op: `Status source D:\home\LogFiles\ApplicationInsights\status\status_RD0003FF0317B6_4248_1.json`
        * Als een vergelijkbare waarde niet aanwezig is, betekent dat de toepassing is op dit moment niet uitgevoerd of wordt niet ondersteund. Om ervoor te zorgen dat de toepassing wordt uitgevoerd, opnieuw handmatig naar de eindpunten van de url/toepassing toepassingen, waarmee de runtime-gegevens beschikbaar.

    * Bevestig dat `IKeyExists` is `true`
        * Als deze ONWAAR is, voegt u toe ' APPINSIGHTS_INSTRUMENTATIONKEY met uw ikey guid naar de toepassingsinstellingen van uw.

    * Bevestig dat er zijn geen vermeldingen voor `AppAlreadyInstrumented`, `AppContainsDiagnosticSourceAssembly`, en `AppContainsAspNetTelemetryCorrelationAssembly`.
        * Als een van deze gegevens bestaat, verwijdert u de volgende pakketten van uw toepassing: `Microsoft.ApplicationInsights`, `System.Diagnostics.DiagnosticSource`, en `Microsoft.AspNet.TelemetryCorrelation`.

De onderstaande tabel bevat een meer gedetailleerde uitleg van de betekenis van deze waarden, de onderliggende zorgt ervoor dat en aanbevolen oplossingen:

|Probleem-waarde|Uitleg|Oplossen
|---- |----|---|
| `AppAlreadyInstrumented:true` | Deze waarde geeft aan dat de extensie wordt gedetecteerd dat een bepaald aspect van de SDK al aanwezig in de toepassing is, en wordt uitstel. Dit kan worden veroorzaakt door een verwijzing naar `System.Diagnostics.DiagnosticSource`, `Microsoft.AspNet.TelemetryCorrelation`, of `Microsoft.ApplicationInsights`  | Verwijder de verwijzingen. Sommige van deze verwijzingen worden standaard toegevoegd uit bepaalde Visual Studio-sjablonen en oudere versies van Visual Studio kunnen de verwijzingen naar toevoegen `Microsoft.ApplicationInsights`.
|`AppAlreadyInstrumented:true` | Deze waarde kan ook worden veroorzaakt door de aanwezigheid van de bovenstaande DLL-bestanden in de map app vanaf een eerdere implementatie. | Schoon de app-map om ervoor te zorgen dat deze DLL-bestanden worden verwijderd.|
|`AppContainsAspNetTelemetryCorrelationAssembly: true` | Deze waarde geeft aan dat extensie verwijzingen naar gedetecteerd `Microsoft.AspNet.TelemetryCorrelation` in de toepassing, en wordt uitstel. | Verwijder de verwijzing.
|`AppContainsDiagnosticSourceAssembly**:true`|Deze waarde geeft aan dat extensie verwijzingen naar gedetecteerd `System.Diagnostics.DiagnosticSource` in de toepassing, en wordt uitstel.| Verwijder de verwijzing.
|`IKeyExists:false`|Deze waarde geeft aan dat de instrumentatiesleutel is niet aanwezig in de instelling `APPINSIGHTS_INSTRUMENTATIONKEY`. Mogelijke oorzaken: De waarden mogelijk per ongeluk verwijderd, bent u vergeten om in te stellen van de waarden in het automation-script, enzovoort. | Zorg ervoor dat de instelling is aanwezig in de instellingen van de App Service-toepassing.

Voor de meest recente informatie over de Application Insights/agentextensie, bekijkt u de [opmerkingen bij de release](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/app-insights-web-app-extensions-releasenotes.md).

## <a name="next-steps"></a>Volgende stappen

* [Voer de profiler uit in uw live app](../../azure-monitor/app/profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): Azure Functions bewaken met Application Insights
* [Schakel diagnostische Azure-gegevens in](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md) om te verzenden naar Application Insights.
* [Controleer metrische gegevens voor servicestatus](../../azure-monitor/platform/data-collection.md) om ervoor te zorgen dat de service beschikbaar is en reageert.
* [Ontvang waarschuwingsmeldingen](../../azure-monitor/platform/alerts-overview.md) wanneer er operationele gebeurtenissen plaatsvinden of metrische gegevens een drempelwaarde overschrijden.
* [Stel webtests voor beschikbaarheid in](../../azure-monitor/app/monitor-web-app-availability.md) om te worden gewaarschuwd als uw site niet actief is.