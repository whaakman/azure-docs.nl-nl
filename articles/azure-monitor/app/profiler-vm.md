---
title: Profileren van web-apps die worden uitgevoerd op een Azure VM met Application Insights Profiler | Microsoft Docs
description: Web-apps voor het profiel op een Azure-VM met Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: e8f80e7d19a961c22b4e1e88556ac165d2558034
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54082740"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-virtual-machine-scale-set-with-application-insights-profiler"></a>Profiel web-apps die worden uitgevoerd op een Azure-machine of virtuele-machineschaalset met Application Insights Profiler
U kunt Application Insights profiler voor deze services ook implementeren:
* [Azure App Service](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](profiler-cloudservice.md ?toc=/azure/azure-monitor/toc.json)
* [Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-scale-set"></a>Profiler op een virtuele Machine implementeert of Virtual Machine Scale sets
Deze pagina leidt u door de stappen die nodig zijn om op te halen die worden uitgevoerd op uw virtuele Azure-machine of de schaal van de virtuele machine van Azure Application Insights-profiler instellen. Application Insights Profiler is geïnstalleerd met de Windows Azure Diagnostics-extensie voor VM's. De extensie moet worden geconfigureerd voor het uitvoeren van de profiler en de App Insights-SDK in uw toepassing moeten worden gebouwd.

1. Application Insights-SDK toevoegen aan uw [ASP.Net-toepassing](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) of gewone [.NET-toepassing.](https://docs.microsoft.com/azure/application-insights/app-insights-windows-services?toc=/azure/azure-monitor/toc.json) U moet verzenden van aanvraagtelemetrie naar Application Insights Zie profielen voor uw aanvragen.
1. Windows Azure Diagnostics-extensie installeren op uw virtuele machine. Zie voor sjabloonvoorbeelden in de volledige Resource Manager:  
    * [Virtuele machine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Virtuele-machineschaalset](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
    Het belangrijkste deel is de ApplicationInsightsProfilerSink in de WadCfg. Een andere sink toevoegen aan deze sectie om te vertellen WAD om in te schakelen de profiler gegevens verzenden naar uw iKey.
    ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "ApplicationInsightsSink",
            "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
          },
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
          }
        ]
      },
    ```

1. De implementatiedefinitie gewijzigde omgeving implementeren.  

   Om toe te passen de wijzigingen, omvat deze doorgaans een implementatie van de volledige sjabloon of een cloudservice op basis van publiceren via PowerShell-cmdlets of Visual Studio.  

   De volgende powershell-opdrachten zijn een alternatieve benadering voor bestaande virtuele machines die alleen de Azure Diagnostics-extensie is van toepassing. U hoeft alleen de ProfilerSink, zoals hierboven wordt vermeld op de configuratie die is geretourneerd door de opdracht Get-AzureRmVMDiagnosticsExtension toevoegen. Vervolgens moet u de bijgewerkte configuratie doorgeven aan de opdracht Set-AzureRmVMDiagnosticsExcension.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Als de bedoelde toepassing wordt uitgevoerd via [IIS](https://www.microsoft.com/web/downloads/platform.aspx), schakel de `IIS Http Tracing` Windows-functie.

   a. Externe toegang tot de omgeving tot stand brengen en gebruik vervolgens de [Windows toevoegen functies]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) -venster, of Voer de volgende opdracht in PowerShell (als beheerder):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Als het tot stand brengen van externe toegang is een probleem, kunt u [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) de volgende opdracht uit te voeren:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Implementeer uw toepassing.

## <a name="can-profiler-run-on-on-premises-servers"></a>Kan de profiler uitvoeren op on-premises servers?
Er zijn geen plannen voor de ondersteuning van Application Insights Profiler voor on-premises servers.

## <a name="next-steps"></a>Volgende stappen

- Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
- Zie [Profiler-traceringen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) in Azure portal.
- Hulp bij het oplossen van problemen met de profiler in [Profiler probleemoplossing](profiler-troubleshooting.md ?toc=/azure/azure-monitor/toc.json).
