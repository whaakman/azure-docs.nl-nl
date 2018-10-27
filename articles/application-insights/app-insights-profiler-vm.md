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
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 10cd05bd40262815e3b27c861982debc18e5b4f3
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142382"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-virtual-machine-scale-set-with-application-insights-profiler"></a>Profiel web-apps die worden uitgevoerd op een Azure-machine of virtuele-machineschaalset met Application Insights Profiler
U kunt Application Insights profiler voor deze services ook implementeren:
* [Azure Web Apps](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-scale-set"></a>Profiler op een virtuele Machine implementeert of Virtual Machine Scale sets
Deze pagina leidt u door de stappen die nodig zijn om op te halen die worden uitgevoerd op uw virtuele Azure-machine of de schaal van de virtuele machine van Azure Application Insights-profiler instellen. Application Insights Profiler is geïnstalleerd met de Windows Azure Diagnostics-extensie voor VM's. U moet de extensie voor het uitvoeren van de profiler en hebben de App Insights-SDK die is ingebouwd in uw toepassing om op te halen van profielen voor uw web-apps die worden uitgevoerd op een virtuele machine configureren.

1. Application Insights-SDK toevoegen aan uw [ASP.Net-toepassing](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) of gewone [.NET-toepassing.](https://docs.microsoft.com/azure/application-insights/app-insights-windows-services?toc=/azure/azure-monitor/toc.json) U moet verzenden van aanvraagtelemetrie naar Application Insights Zie profielen voor uw aanvragen.
1. Windows Azure Diagnostics-extensie installeren op uw virtuele machine. Zie voor sjabloonvoorbeelden in de volledige Resource Manager:  
    * [Virtuele machine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Virtuele-machineschaalset](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
1. De implementatiedefinitie gewijzigde omgeving implementeren.  

   De wijzigingen toepassen gewoonlijk betrekking heeft op de implementatie van een volledige sjabloon of een cloud service op basis van publiceren via PowerShell-cmdlets of Visual Studio.  

   De volgende powershell-opdrachten zijn een alternatieve benadering voor bestaande virtuele machines die alleen de Azure Diagnostics-extensie is van toepassing:  

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

1. Implementeer uw toepassing.

## <a name="enable-profiler-on-on-premises-servers"></a>Profiler inschakelen op on-premises servers

Profiler inschakelen op een on-premises server is ook wel bekend als actieve Application Insights Profiler in de zelfstandige modus. Het niet gekoppeld aan Azure Diagnostics-extensie wijzigingen.

Er zijn geen plannen voor de ondersteuning van officieel Profiler voor on-premises servers. Als u geïnteresseerd bent in te experimenteren met dit scenario, kunt u [downloaden ondersteuningscode](https://github.com/ramach-msft/AIProfiler-Standalone). We zijn *niet* die verantwoordelijk is voor het onderhouden van die code of voor het reageren op problemen en functieaanvragen die betrekking op de code hebben.

## <a name="next-steps"></a>Volgende stappen

- Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
- Zie [Profiler-traceringen](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) in Azure portal.
- Hulp bij het oplossen van problemen met de profiler in [Profiler probleemoplossing](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
