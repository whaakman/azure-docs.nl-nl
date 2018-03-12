---
title: Application Insights Profiler inschakelen voor toepassingen die worden gehost op Azure Cloud Services-bronnen | Microsoft Docs
description: Informatie over het instellen van Application Insights Profiler op een toepassing die wordt uitgevoerd op Azure Cloud Services.
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
ms.openlocfilehash: a24695f7bbb5fb0546e27c934319a60a3418b9e1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-azure-cloud-services"></a>Application Insights Profiler inschakelen voor virtuele machines in Azure Service Fabric en Azure-Cloudservices

In dit artikel laat zien hoe Azure Application Insights Profiler inschakelen op een ASP.NET-toepassing die wordt gehost door een Azure Cloud Services-resource.

De voorbeelden in dit artikel zijn ondersteuning voor Azure Virtual Machines, virtuele-machineschaalsets, Azure Service Fabric en Azure Cloud Services. De voorbeelden zijn afhankelijk van de sjablonen die ondersteuning bieden voor de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) implementatiemodel.  


## <a name="overview"></a>Overzicht

De volgende afbeelding toont hoe Application Insights Profiler werkt met toepassingen die worden gehost op Azure Cloud Services-bronnen. Azure Cloud Services-resources omvatten virtuele Machines, schaalsets, cloudservices en Service Fabric-clusters. De afbeelding wordt een virtuele machine van Azure als voorbeeld.  

  ![Diagram die weergeeft hoe Application Insights Profiler werkt met Azure Cloud Services-bronnen](./media/enable-profiler-compute/overview.png)

U kunt Profiler, moet u de configuratie op drie locaties:

* De Application Insights exemplaar deelvenster in de Azure portal.
* De broncode toepassing (bijvoorbeeld, een ASP.NET-webtoepassing).
* De omgeving implementatie definitie broncode (bijvoorbeeld een Azure Resource Manager-sjabloon in het .json-bestand).


## <a name="set-up-the-application-insights-instance"></a>De Application Insights-instantie instellen

1. [Maak een nieuwe Application Insights-resource](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-create-new-resource), of een bestaande set selecteren. 

2. Ga naar uw Application Insights-resource en vervolgens de instrumentatiesleutel kopiëren.

   ![Locatie van de instrumentatiesleutel](./media/enable-profiler-compute/CopyAIKey.png)

3. Voor het voltooien van de Application Insights-exemplaar in te stellen voor de Profiler, voert u de procedure die wordt beschreven in [Profiler inschakelen. U hoeft niet te koppelen van de web-apps, omdat de stappen specifiek voor de resource voor de app-services zijn. Zorg ervoor dat de Profiler is ingeschakeld in de **Profiler configureren** deelvenster.


## <a name="set-up-the-application-source-code"></a>De broncode van de toepassing instellen

### <a name="aspnet-web-applications-azure-cloud-services-web-roles-or-the-service-fabric-aspnet-web-front-end"></a>ASP.NET-webtoepassingen, Azure Cloud Services-web-rollen of de Service Fabric-ASP.NET-webfront-end
Instellen van uw toepassing telemetriegegevens te verzenden naar een Application Insights-exemplaar op elk `Request` bewerking.  

Voeg de [Application Insights-SDK](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) aan uw toepassingsproject. Zorg ervoor dat de NuGet-pakket-versies als volgt zijn:  
  - Voor ASP.NET-toepassingen: [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 of hoger.
  - Voor ASP.NET Core toepassingen: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 of hoger.
  - Voor andere toepassingen .NET en .NET Core (bijvoorbeeld een stateless Service Fabric-service of een werkrol Cloud Services): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) of [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 of hoger.  

### <a name="azure-cloud-services-worker-roles-or-the-service-fabric-stateless-back-end"></a>Azure Cloud Services-werkrollen of de Service Fabric staatloze back-end
Naast de vorige stap is voltooid als de toepassing is *niet* een ASP.NET- of ASP.NET Core toepassing (bijvoorbeeld, als dit is een Azure Cloud Services-werkrol of stateless Service Fabric-API's), het volgende doen:  

  1. Voeg de volgende code vroeg in de levensduur van de toepassing:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Zie voor meer informatie over de configuratie van deze globale instrumentation [gebruik Service Fabric met Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. Voor elk deel van de code die u wilt softwareontwikkelaars, het toevoegen van een `StartOperation<RequestTelemetry>` **USING** instructie omheen, zoals wordt weergegeven in het volgende voorbeeld:

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

De omgeving waarin Profiler en het uitvoeren van uw toepassing kunnen een virtuele machine, een virtuele-machineschaalset, een Service Fabric-cluster of een cloud services-exemplaar.  

### <a name="virtual-machines-scale-sets-or-service-fabric"></a>Virtuele machines, schaalsets of Service Fabric

Zie voor volledige voorbeelden:  
  * [Virtuele machine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Virtuele-machineschaalset](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric-cluster](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Ga als volgt te werk om uw omgeving instellen:
1. Om ervoor te zorgen dat u gebruikt [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger, is voldoende om te bevestigen dat het geïmplementeerde besturingssysteem `Windows Server 2012 R2` of hoger.

2. Zoeken naar de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) extensie in de sjabloon voor de implementatie-bestand en voeg vervolgens de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`. Vervang de `ApplicationInsightsProfiler` eigenschapswaarde met uw eigen Application Insights-instrumentatiesleutel:  

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

> [!TIP]
> Voor virtuele Machines een alternatief voor de bovenstaande json op basis van stappen is om te navigeren in de Azure portal naar **virtuele Machines** > **diagnostische instellingen**  >   **Sinks** > Set diagnostische gegevens verzenden naar Application Insights naar **ingeschakeld** en selecteer een Application Insights-account of een specifieke ikey.

### <a name="azure-cloud-services"></a>Azure Cloud Services

1. Om ervoor te zorgen dat u gebruikt [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger, is voldoende om te bevestigen dat de *serviceconfiguration zijn.\*. cscfg* bestanden hebben een `osFamily` waarde van '5' of hoger.

2. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* bestand voor de toepassingsrol van uw, zoals hier wordt weergegeven:  

   ![Locatie van het configuratiebestand van de diagnostische gegevens](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

   Als u het bestand niet vinden, voor meer informatie over het inschakelen van de extensie voor diagnostische gegevens in uw Azure Cloud Services-project, raadpleegt u [diagnostics instellen voor Azure Cloud Services en virtuele machines](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

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
> Als de *diagnostics.wadcfgx* bestand bevat ook een andere sink van het type `ApplicationInsights`, moet overeenkomen met alle drie de volgende instrumentatiesleutels:  
>  * De sleutel die wordt gebruikt door uw toepassing.  
>  * De sleutel die wordt gebruikt door de `ApplicationInsights` sink.  
>  * De sleutel die wordt gebruikt door de `ApplicationInsightsProfiler` sink.  
>
> U vindt de daadwerkelijke instrumentation sleutelwaarde die wordt gebruikt door de `ApplicationInsights` sink in de *serviceconfiguration zijn.\*. cscfg* bestanden.  
> Na de release van de Visual Studio 15,5 Azure SDK wordt alleen de instrumentatiesleutels die worden gebruikt door de toepassing en de `ApplicationInsightsProfiler` sink moeten met elkaar overeen.


## <a name="configure-environment-deployment-and-runtime"></a>Implementatie van de omgeving en runtime configureren

1. De implementatiedefinitie gewijzigde omgeving implementeren.  

   De wijzigingen toepassen betekent doorgaans een volledige sjabloon-implementatie of een cloud-services op basis via PowerShell-cmdlets of Visual Studio publiceren.  

   Hier volgt een alternatieve methode voor bestaande virtuele machines die alleen de extensie Azure Diagnostics raakt:  

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

2. Als de gewenste toepassing wordt uitgevoerd via [IIS](https://www.microsoft.com/web/platform/server.aspx), schakel de `IIS Http Tracing` Windows-functie door het volgende te doen:  

   a. Externe toegang tot de omgeving tot stand brengen en gebruik vervolgens de [toevoegen Windows-onderdelen]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) venster uit of Voer de volgende opdracht uit in PowerShell (als administrator):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Als het tot stand brengen van externe toegang is een probleem, kunt u [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) de volgende opdracht uitvoeren:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-profiler-on-on-premises-servers"></a>Profiler op lokale servers inschakelen

Profiler wordt ingeschakeld op een on-premises server wordt ook wel actief Application Insights Profiler in de zelfstandige modus. Het niet gekoppeld aan Azure Diagnostics extensie wijzigingen.

We hebben geen plan Profiler officiële ondersteuning voor on-premises servers. Als u geïnteresseerd bent in experimenteren met dit scenario, kunt u [downloaden ondersteuningscode](https://github.com/ramach-msft/AIProfiler-Standalone). We zijn *niet* die verantwoordelijk is voor het onderhouden van die code of voor het reageren op problemen en functie-aanvragen die gerelateerd zijn aan de code.

## <a name="next-steps"></a>Volgende stappen

- Genereren van het verkeer naar uw toepassing (bijvoorbeeld starten een [beschikbaarheidstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Wacht 10 tot 15 minuten aan traceringen worden verzonden naar de Application Insights-exemplaar gestart.
- Zie [Profiler traceringen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) in de Azure portal.
- Hulp bij het oplossen van problemen in profiler [Profiler probleemoplossing](app-insights-profiler.md#troubleshooting).
- Meer informatie over de Profiler in [Application Insights Profiler](app-insights-profiler.md).
