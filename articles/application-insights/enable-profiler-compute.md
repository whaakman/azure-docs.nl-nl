---
title: Application Insights Profiler inschakelen voor Azure Compute-toepassingen | Microsoft Docs
description: Informatie over het instellen van Application Insights Profiler op een toepassing die wordt uitgevoerd in Azure Compute.
services: application-insights
documentationcenter: 
author: ramach-msft
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach
ms.openlocfilehash: d4559007aece8850b4c2d707686effd706ec468c
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-cloud-services"></a>Inschakelen van Application Insights Profiler voor virtuele machines in Azure, Service Fabric en Cloud Services

In dit artikel laat zien hoe Azure Application Insights Profiler inschakelen op een ASP.NET-toepassing die wordt gehost door een Azure compute-resource.

De voorbeelden in dit artikel zijn ondersteuning voor Azure Virtual Machines, virtuele-machineschaalsets, Azure Service Fabric en Azure Cloud Services. De voorbeelden zijn afhankelijk van de sjablonen die ondersteuning bieden voor de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) implementatiemodel.  


## <a name="overview"></a>Overzicht

De volgende afbeelding toont hoe de profiler Application Insights werkt met Azure Compute-bronnen. Azure Compute-bronnen virtuele Machines, virtuele-Machineschaalsets, Cloud Services bevatten en Service Fabric-clusters. De afbeelding wordt een virtuele machine van Azure als voorbeeld.  

  ![Overzicht](./media/enable-profiler-compute/overview.png)

U kunt de profiler, moet u de configuratie op drie locaties:

* De Application Insights exemplaar blade in de Azure portal.
* De broncode toepassing (bijvoorbeeld, een ASP.NET-webtoepassing).
* De omgeving implementatie definitie broncode (bijvoorbeeld een Azure Resource Manager-sjabloon in .json-bestand).


## <a name="set-up-the-application-insights-instance"></a>De Application Insights-instantie instellen

[Maak een nieuwe Application Insights-resource](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-create-new-resource) of een bestaande set selecteren.
Navigeer naar uw Application Insights-resource en de Instrumentatiesleutel kopiëren.

  ![Locatie van de belangrijkste instrumentation](./media/enable-profiler-compute/CopyAIKey.png)

Voltooi de stappen die worden beschreven in [inschakelen van de profiler](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler) voor het voltooien van de Application Insights-exemplaar in te stellen voor de profiler. U hoeft niet te koppelen van web-apps, zoals aan App Services resource specifieke stappen zijn. NET ervoor te zorgen dat de profiler is ingeschakeld in de *configureren* Profiler blade.


## <a name="set-up-the-application-source-code"></a>De broncode van de toepassing instellen

### <a name="aspnet-web-applications-cloud-services-web-roles-or-service-fabric-aspnet-web-frontend"></a>ASP.NET-webtoepassingen, Cloud Services-Web-rollen of -Service Fabric ASP.NET Web Frontend
Instellen van uw toepassing telemetriegegevens te verzenden naar een Application Insights-exemplaar op elk `Request` bewerking:  

Voeg de [Application Insights-SDK](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) aan uw toepassingsproject. Zorg ervoor dat de NuGet-pakket-versies als volgt zijn:  
  - Voor ASP.NET-toepassingen: [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 of hoger.
  - Voor ASP.NET Core toepassingen: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 of hoger.
  - Voor andere toepassingen .NET en .NET Core (bijvoorbeeld een stateless Service Fabric-service of een werkrol Cloud Services): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) of [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 of hoger.  

### <a name="cloud-services-worker-roles-or-service-fabric-stateless-backend"></a>Cloud Services-werkrollen of Service Fabric staatloze back-end
Als uw toepassing *niet* een ASP.NET- of ASP.NET Core-toepassing (bijvoorbeeld als een Cloud Services-werkrol of stateless Service Fabric-API's), de volgende extra instrumentation-instellingen zijn vereist, naast welke stap boven:  

  1. Voeg de volgende code vroeg in de levensduur van de toepassing:  

    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    ...
    // Replace with your own Application Insights instrumentation key.
    TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
    ```
  Zie voor meer informatie over de configuratie van deze globale instrumentation [gebruik Service Fabric met Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. Voor elk deel van de code die u wilt softwareontwikkelaars, het toevoegen van een `StartOperation<RequestTelemetry>` **USING** instructie omheen, zoals in het volgende voorbeeld:

    ```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;
    ...
    var client = new TelemetryClient();
    ...
    using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
    {
      // ... Code I want to profile.
    }
    ```

  Het aanroepen van `StartOperation<RequestTelemetry>` binnen een andere `StartOperation<RequestTelemetry>` bereik wordt niet ondersteund. U kunt `StartOperation<DependencyTelemetry>` in de geneste bereik in plaats daarvan. Bijvoorbeeld:  

    ```csharp
    using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
    {
      try
      {
        ProductDetail details = new ProductDetail() { Id = productId };
        getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();

        // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
        using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
        {
            double price = await _priceDataBase.GetAsync(productId);
            if (IsTooCheap(price))
            {
                throw new PriceTooLowException(productId);
            }
            details.Price = price;
        }

        // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
        using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
        {
            details.Reviews = await _reviewDataBase.GetAsync(productId);
        }

        getDetailsOperation.Telemetry.Success = true;
        return details;
      }
      catch(Exception ex)
      {
        getDetailsOperation.Telemetry.Success = false;

        // This exception gets linked to the 'GetProductDetails' request telemetry.
        client.TrackException(ex);
        throw;
      }
    }
    ```


## <a name="set-up-the-environment-deployment-definition"></a>De definitie van de implementatie van de omgeving instellen

De omgeving waarin de profiler en het uitvoeren van uw toepassing mag een virtuele machine, een virtuele-machineschaalset, een Service Fabric-cluster of een exemplaar van Cloud-Services.  

### <a name="virtual-machines-virtual-machine-scale-sets-or-service-fabric"></a>Virtuele machines, virtuele-machineschaalsets of Service Fabric

Volledige voorbeelden:  
  * [Virtuele machine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Virtuele-machineschaalset](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric-cluster](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

1. Om ervoor te zorgen dat [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger is in gebruik is, het is voldoende om te bevestigen dat het geïmplementeerde besturingssysteem `Windows Server 2012 R2` of hoger.

2. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) extensie in de sjabloon voor de implementatie-bestand en voeg vervolgens de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`. Vervang de `ApplicationInsightsProfiler` eigenschapswaarde met uw eigen Application Insights-instrumentatiesleutel:  
  ```json
  "SinksConfig": {
    "Sink": [
      {
        "name": "MyApplicationInsightsProfilerSink",
        "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
      }
    ]
  }
  ```

  Zie voor meer informatie over het toevoegen van de extensie voor diagnostische gegevens naar uw implementatiesjabloon [Gebruik controle en diagnostische gegevens met een virtuele machine van Windows en Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


### <a name="cloud-services"></a>Cloud Services

1. Om ervoor te zorgen dat [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger is in gebruik is, het is voldoende om te bevestigen dat serviceconfiguration zijn.\*. cscfg-bestanden hebben een `osFamily` waarde van **'5'** of hoger.

2. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) diagnostics.wadcfgx-bestand voor de toepassingsrol van uw:  
  ![Locatie van het configuratiebestand van de diagnostische gegevens](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  
  Als u het bestand niet vinden, voor meer informatie over het inschakelen van de extensie voor diagnostische gegevens in uw Cloud Services-project, raadpleegt u [diagnostics instellen voor Azure Cloud Services en virtuele machines](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

3. Voeg de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`:  
  ```xml
  <WadCfg>
    <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
    <SinksConfig>
      <Sink name="MyApplicationInsightsProfiler">
        <!-- Replace with your own Application Insights instrumentation key. -->
        <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
      </Sink>
    </SinksConfig>
  </WadCfg>
  ```

> [!NOTE]  
> Als de `diagnostics.wadcfgx` bestand bevat ook een andere sink van het type `ApplicationInsights`, moet overeenkomen met alle drie deze instrumentatiesleutels:  
>  * De instrumentatiesleutel die wordt gebruikt door de toepassing.  
>  * De instrumentatiesleutel die wordt gebruikt door de `ApplicationInsights` sink.  
>  * De instrumentatiesleutel die wordt gebruikt door de `ApplicationInsightsProfiler` sink.  
>
> U vindt de daadwerkelijke instrumentation sleutelwaarde die wordt gebruikt door de `ApplicationInsights` sink in de serviceconfiguration zijn.\*. cscfg-bestanden.  
> Na de release van Visual Studio 15,5 Azure SDK wordt alleen de instrumentatiesleutels gebruikt door de toepassing en `ApplicationInsightsProfiler` sink moeten met elkaar overeen.


## <a name="environment-deployment-and-runtime-configurations"></a>Omgeving implementatie en runtime-configuraties

1. De implementatiedefinitie gewijzigde omgeving implementeren.  

  Om de wijzigingen toepast, doorgaans een volledige sjabloon-implementatie of een cloud-services publiceren via PowerShell-cmdlets of Visual Studio is betrokken.  

  Hier volgt een alternatieve methode voor bestaande virtuele machines die alleen de extensie Azure Diagnostics raakt:  
  ```powershell
  $ConfigFilePath = [IO.Path]::GetTempFileName()
  # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
  (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
  # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
  # if your original diagnostics configuration had the storageAccountName property in the protectedSettings section
  # (which is not downloadable). Make sure to pass the same original value you had in this cmdlet call.
  Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
  ```

2. Als de gewenste toepassing wordt uitgevoerd via [IIS](https://www.microsoft.com/web/platform/server.aspx), schakel de `IIS Http Tracing` Windows-functie:  

  1. Externe toegang tot de omgeving tot stand brengen en gebruik vervolgens de [Windows-onderdelen toevoegen]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) venster uit of Voer de volgende opdracht uit in PowerShell (als administrator):  
    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
  2. Als het tot stand brengen van externe toegang is een probleem, kunt u [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) de volgende opdracht uitvoeren:  
    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-the-profiler-on-on-premises-servers"></a>De profiler op lokale servers inschakelen

Inschakelen van de profiler op een on-premises server wordt ook wel actief Application Insights Profiler in de zelfstandige modus (dit niet gekoppeld aan Azure Diagnostics extensie wijzigingen).

We hebben geen plan officiële ondersteuning voor de profiler voor on-premises servers. Als u geïnteresseerd bent in experimenteren met dit scenario, kunt u [downloaden ondersteuningscode](https://github.com/ramach-msft/AIProfiler-Standalone). We zijn *niet* die verantwoordelijk is voor het onderhouden van die code of voor het reageren op problemen en functieaanvragen in verband met de code.

## <a name="next-steps"></a>Volgende stappen

- Genereren van het verkeer naar uw toepassing (bijvoorbeeld starten een [beschikbaarheidstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Wacht 10 tot 15 minuten aan traceringen worden verzonden naar de Application Insights-exemplaar gestart.
- Zie [Profiler traceringen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) in de Azure portal.
- Hulp bij het oplossen van problemen in profiler [Profiler probleemoplossing](app-insights-profiler.md#troubleshooting).
- Meer informatie over de profiler in [Application Insights Profiler](app-insights-profiler.md).
