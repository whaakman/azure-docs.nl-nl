---
title: Application Insights Profiler inschakelen voor toepassingen die worden gehost op Azure Cloud Services-resources | Microsoft Docs
description: Meer informatie over het instellen van Application Insights Profiler in een toepassing die wordt uitgevoerd op Azure Cloud Services.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/16/2017
ms.reviewer: ramach
ms.author: mbullwin
ms.openlocfilehash: eb2ec0c0b77e71a54d1e7f852a22d82203abf7b6
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091977"
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-azure-cloud-services"></a>Application Insights Profiler inschakelen voor Azure-machines, Service Fabric en Azure Cloudservices

In dit artikel laat zien hoe Azure Application Insights Profiler inschakelen voor een ASP.NET-toepassing die wordt gehost door een Azure Cloud Services-resource.

De voorbeelden in dit artikel zijn onder meer ondersteuning voor Azure Virtual Machines, virtuele-machineschaalsets, Azure Service Fabric en Azure Cloud Services. De voorbeelden zijn afhankelijk van de sjablonen die ondersteuning bieden voor de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) implementatiemodel.  


## <a name="overview"></a>Overzicht

De volgende afbeelding ziet u hoe Application Insights Profiler werkt met toepassingen die worden gehost op Azure Cloud Services-resources. Azure Cloud Services-resources zijn virtuele Machines, schaalsets, cloudservices en Service Fabric-clusters. De afbeelding wordt een virtuele machine van Azure als voorbeeld gebruikt.  

  ![Diagram waarin wordt getoond hoe Application Insights Profiler werkt met Azure Cloud Services-resources](./media/enable-profiler-compute/overview.png)

Om in te schakelen volledig Profiler, moet u de configuratie op drie locaties:

* Het deelvenster van de instantie Application Insights in Azure portal.
* De toepassingsbroncode (bijvoorbeeld: een ASP.NET-webtoepassing).
* De omgeving implementatie definitie broncode (bijvoorbeeld een Azure Resource Manager-sjabloon in de JSON-bestand).


## <a name="set-up-the-application-insights-instance"></a>De Application Insights-instantie instellen

1. [Maak een nieuwe Application Insights-resource](https://docs.microsoft.com/azure/application-insights/app-insights-create-new-resource), of Selecteer een bestaande resourcegroep. 

1. Ga naar uw Application Insights-resource en kopieer de instrumentatiesleutel.

   ![Locatie van de instrumentatiesleutel](./media/enable-profiler-compute/CopyAIKey.png)

1. Voor het voltooien van de Application Insights-exemplaar in te stellen voor Profiler, voert u de procedure die wordt beschreven in [Profiler inschakelen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler). U hoeft te koppelen van de web-apps, omdat de stappen specifiek voor de resource voor de app-services zijn. Zorg ervoor dat de Profiler is ingeschakeld in de **Profiler configureren** deelvenster.


## <a name="set-up-the-application-source-code"></a>De broncode van de toepassing instellen

### <a name="aspnet-web-applications-azure-cloud-services-web-roles-or-the-service-fabric-aspnet-web-front-end"></a>ASP.NET-webtoepassingen, Azure Cloud Services-webrollen of de Service Fabric ASP.NET web-front-end
Instellen van uw toepassing telemetrische gegevens verzenden naar een Application Insights-exemplaar op elk `Request` bewerking.  

Voeg de [Application Insights-SDK](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) aan uw project een toepassing. Zorg ervoor dat de NuGet-pakket-versies zijn als volgt:  
  - Voor ASP.NET-toepassingen: [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 of hoger.
  - Voor ASP.NET Core-toepassingen: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 of hoger.
  - Voor andere .NET en .NET Core-toepassingen (bijvoorbeeld, een Service Fabric stateless service of een werkrol Cloud Services): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) of [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 of hoger.  

### <a name="azure-cloud-services-worker-roles-or-the-service-fabric-stateless-back-end"></a>Azure Cloud Services-werkrollen of de Service Fabric stateless back-end
Naast het voltooien van de vorige stap, als uw toepassing is *niet* een ASP.NET of ASP.NET Core-toepassing (bijvoorbeeld als er een Azure Cloud Services-werkrol of stateless Service Fabric-API's), het volgende doen:  

  1. Voeg de volgende code al vroeg in de levensduur van de toepassing:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Zie voor meer informatie over de configuratie van deze globale instrumentation [met Service Fabric met Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. Voor elk stukje code dat u wilt instrumenteren, voegt u een `StartOperation<RequestTelemetry>` **met** instructie rond het, zoals wordt weergegeven in het volgende voorbeeld:

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

        Aanroepen van `StartOperation<RequestTelemetry>` binnen een andere `StartOperation<RequestTelemetry>` bereik wordt niet ondersteund. U kunt `StartOperation<DependencyTelemetry>` in de geneste scope in plaats daarvan. Bijvoorbeeld:  
        
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

De omgeving waarin Profiler en het uitvoeren van uw toepassing kunnen een virtuele machine, een virtuele-machineschaalset, een Service Fabric-cluster, of een cloud services-exemplaar.  

### <a name="virtual-machines-scale-sets-or-service-fabric"></a>Virtuele machines, schaalsets of Service Fabric

Zie, voor volledige voorbeelden:  
  * [Virtuele machine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Virtuele-machineschaalset](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric-cluster](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Als u uw omgeving instelt, het volgende doen:
1. Om ervoor te zorgen dat u [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger, is voldoende om te bevestigen dat het geïmplementeerde besturingssysteem `Windows Server 2012 R2` of hoger.

1. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) -extensie in de sjabloon voor de implementatie-bestand en voeg vervolgens de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`. Vervang de `ApplicationInsightsProfiler` eigenschapswaarde met uw eigen Application Insights-instrumentatiesleutel:  

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

      Zie voor meer informatie over het toevoegen van de extensie voor diagnostische gegevens naar uw implementatiesjabloon [Gebruik controle en diagnostische gegevens met een Windows-VM en Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Voor virtuele Machines een alternatief voor de bovenstaande json op basis van stappen is om te navigeren in de Azure portal om **virtuele Machines** > **diagnostische instellingen**  >   **Sinks** > Set diagnostische gegevens verzenden naar Application Insights aan **ingeschakeld** en selecteert u een Application Insights-account of een specifieke ikey.

### <a name="azure-cloud-services"></a>Azure Cloud Services

1. Om ervoor te zorgen dat u [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) of hoger, is voldoende om te bevestigen dat de *ServiceConfiguration.\*.cscfg* bestanden hebben een `osFamily` waarde van '5' of hoger.

1. Zoek de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* bestand voor de toepassingsrol van uw, zoals hier wordt weergegeven:  

   ![Locatie van het configuratiebestand van de diagnostische gegevens](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

   Als u het bestand niet vinden, voor informatie over het inschakelen van de extensie voor diagnostische gegevens in uw Azure Cloud Services-project, Zie [diagnose instellen voor Azure Cloud Services en virtuele machines](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

1. Voeg de volgende `SinksConfig` sectie als een onderliggend element van `WadCfg`:  

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
> Als de *diagnostics.wadcfgx* -bestand bevat ook een andere sink van het type `ApplicationInsights`, alle drie van de volgende instrumentatiesleutels moet overeenkomen met:  
>  * De sleutel die wordt gebruikt door uw toepassing.  
>  * De sleutel die wordt gebruikt door de `ApplicationInsights` sink.  
>  * De sleutel die wordt gebruikt door de `ApplicationInsightsProfiler` sink.  
>
> U vindt de daadwerkelijke instrumentation sleutelwaarde die wordt gebruikt door de `ApplicationInsights` sink in de *ServiceConfiguration.\*.cscfg* bestanden.  
> Na de release van Visual Studio 15.5 Azure SDK, alleen de instrumentatiesleutels die worden gebruikt door de toepassing en de `ApplicationInsightsProfiler` sink moeten met elkaar overeen.


## <a name="configure-environment-deployment-and-runtime"></a>Implementatie van de omgeving en runtime configureren

1. De implementatiedefinitie gewijzigde omgeving implementeren.  

   De wijzigingen toepassen gewoonlijk betrekking heeft op de implementatie van een volledige sjabloon of een cloud services op basis van via PowerShell-cmdlets of Visual Studio publiceren.  

   Hier volgt een alternatieve benadering voor bestaande virtuele machines die alleen de Azure Diagnostics-extensie is van toepassing:  

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Als de bedoelde toepassing wordt uitgevoerd via [IIS](https://www.microsoft.com/web/downloads/platform.aspx), schakel de `IIS Http Tracing` Windows-functie als volgt:  

   a. Externe toegang tot de omgeving tot stand brengen en gebruik vervolgens de [Windows toevoegen functies]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) -venster, of Voer de volgende opdracht in PowerShell (als beheerder):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Als het tot stand brengen van externe toegang is een probleem, kunt u [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) de volgende opdracht uit te voeren:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-profiler-on-on-premises-servers"></a>Profiler inschakelen op on-premises servers

Profiler inschakelen op een on-premises server is ook wel bekend als actieve Application Insights Profiler in de zelfstandige modus. Het niet gekoppeld aan Azure Diagnostics-extensie wijzigingen.

Er zijn geen plannen voor de ondersteuning van officieel Profiler voor on-premises servers. Als u geïnteresseerd bent in te experimenteren met dit scenario, kunt u [downloaden ondersteuningscode](https://github.com/ramach-msft/AIProfiler-Standalone). We zijn *niet* die verantwoordelijk is voor het onderhouden van die code of voor het reageren op problemen en functieaanvragen die betrekking op de code hebben.

## <a name="next-steps"></a>Volgende stappen

- Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
- Zie [Profiler-traceringen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) in Azure portal.
- Hulp bij het oplossen van problemen met de profiler in [Profiler probleemoplossing](app-insights-profiler.md#troubleshooting).
- Meer informatie over Profiler in [Application Insights Profiler](app-insights-profiler.md).
