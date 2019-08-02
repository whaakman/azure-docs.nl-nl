---
title: Prestaties van toepassingen bewaken die worden gehost op Azure VM en virtuele-machine schaal sets van Azure | Microsoft Docs
description: Bewaking van toepassings prestaties voor Azure VM en virtuele-machine schaal sets van Azure. Grafiek belasting en respons tijd, afhankelijkheids informatie en waarschuwingen instellen voor prestaties.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: mbullwin
ms.openlocfilehash: f2c6b98fd0be2061e9d8cab5c063cafadf71476a
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597452"
---
# <a name="monitor-application-performance-hosted-on-azure-vm-and-azure-virtual-machine-scale-sets"></a>De prestaties van toepassingen bewaken die worden gehost op Azure VM-en Azure virtual machine-schaal sets

Het inschakelen van bewaking op uw .NET-webtoepassingen die worden uitgevoerd op [azure virtual machines](https://azure.microsoft.com/services/virtual-machines/) en [Azure virtual machine Scale sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) is nu eenvoudiger dan ooit. Profiteer van de voor delen van het gebruik van Application Insights zonder uw code te wijzigen.

Dit artikel begeleidt u bij het inschakelen van Application Insights bewaking met behulp van de ApplicationMonitoringWindows-extensie en voorziet in voorlopige richt lijnen voor het automatiseren van het proces voor grootschalige implementaties.

> [!IMPORTANT]
> De Azure ApplicationMonitoringWindows-extensie is momenteel beschikbaar als open bare preview.
> Deze preview-versie is beschikbaar zonder een service overeenkomst en wij raden deze niet aan voor productie werkbelastingen. Sommige functies worden mogelijk niet ondersteund en andere hebben mogelijk beperkte mogelijkheden.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="enable-application-insights"></a>Application Insights inschakelen

Er zijn twee manieren om toepassings bewaking in te scha kelen voor Azure VM en gehoste toepassingen voor virtuele-machine schaal sets van Azure:

* **Toepassings bewaking op basis van agents** (ApplicationMonitoringWindows-extensie).
    * Deze methode is het gemakkelijkst in te scha kelen en er is geen geavanceerde configuratie vereist. Dit wordt vaak aangeduid als runtime-bewaking. Voor Azure-Vm's en virtuele-machine schaal sets van Azure wordt het ten minste aanbevolen om dit bewakings niveau in te scha kelen. Daarna kunt u op basis van uw specifieke scenario evalueren of hand matige instrumentatie nodig is.
    * Momenteel worden alleen door .NET IIS gehoste toepassingen ondersteund.

* **Hand matig instrumenteer de toepassing via code** door de Application Insights SDK te installeren.

    * Deze benadering is veel meer aanpasbaar, maar het is wel nodig om [een afhankelijkheid toe te voegen aan de Application INSIGHTS SDK NuGet-pakketten](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Deze methode betekent ook dat u de updates voor de meest recente versie van de pakketten zelf moet beheren.

    * Als u aangepaste API-aanroepen wilt maken voor het bijhouden van gebeurtenissen/afhankelijkheden die niet standaard worden vastgelegd met bewaking op basis van agents, moet u deze methode gebruiken. Bekijk de [API voor het artikel aangepaste gebeurtenissen en metrische gegevens](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) voor meer informatie.

> [!NOTE]
> Als zowel bewaking op basis van de agent als hand matige instrumentatie op basis van SDK wordt gedetecteerd, worden alleen de instellingen voor hand matige instrumentatie gehonoreerd. Dit is om te voor komen dat dubbele gegevens worden verzonden. Raadpleeg de [sectie probleem oplossing](#troubleshooting) hieronder voor meer informatie.

## <a name="manage-agent-based-monitoring-for-net-applications-on-vm-using-powershell"></a>Bewaking op basis van agents voor .NET-toepassingen op virtuele machines beheren met Power shell

De uitbrei ding voor toepassings bewaking voor de VM installeren of bijwerken
```powershell
$publicCfgJsonString = '
{
  "RedfieldConfiguration": {
    "InstrumentationKeyMap": {
      "Filters": [
        {
          "AppFilter": ".*",
          "MachineFilter": ".*",
          "InstrumentationSettings" : {
            "InstrumentationKey": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          }
        }
      ]
    }
  }
}
';
$privateCfgJsonString = '{}';

Set-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Location "South Central US" -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -Version "2.8" -SettingString $publicCfgJsonString -ProtectedSettingString $privateCfgJsonString
```

Uitbrei ding toepassings bewaking verwijderen van VM
```powershell
Remove-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name "ApplicationMonitoring"
```

Uitbrei ding van de bewakings status van de query toepassing voor VM
```powershell
Get-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name ApplicationMonitoring -Status
```

Lijst met geïnstalleerde uitbrei dingen voor VM ophalen
```powershell
Get-AzResource -ResourceId "/subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions"

# Name              : ApplicationMonitoring
# ResourceGroupName : <myVmResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachines/extensions
# Location          : southcentralus
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions/ApplicationMonitoring
```

## <a name="manage-agent-based-monitoring-for-net-applications-on-azure-virtual-machine-scale-set-using-powershell"></a>Bewaking op basis van agents voor .NET-toepassingen op virtuele-machine schaal sets van Azure beheren met Power shell

De uitbrei ding voor toepassings bewaking installeren of bijwerken voor de schaalset van virtuele Azure-machines
```powershell
$publicCfgHashtable =
@{
  "RedfieldConfiguration"= @{
    "InstrumentationKeyMap"= @{
      "Filters"= @(
        @{
          "AppFilter"= ".*";
          "MachineFilter"= ".*";
          "InstrumentationSettings"= @{
            "InstrumentationKey"= "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"; # Application Insights Instrumentation Key, create new Application Insights resource if you don't have one. https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents
          }
        }
      )
    }
  }
};
$privateCfgHashtable = @{};

$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -TypeHandlerVersion "2.8" -Setting $publicCfgHashtable -ProtectedSetting $privateCfgHashtable

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

De uitbrei ding toepassings bewaking verwijderen uit de Azure virtual machine-schaalset
```powershell
$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring"

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Uitbrei ding van de bewakings status van de query toepassing voor Azure virtual machine-schaal sets
```powershell
# Not supported by extensions framework
```

Lijst met geïnstalleerde uitbrei dingen voor de schaalset van virtuele Azure-machines ophalen
```powershell
Get-AzResource -ResourceId /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions

# Name              : ApplicationMonitoringWindows
# ResourceGroupName : <myResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachineScaleSets/extensions
# Location          :
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions/ApplicationMonitoringWindows
```

## <a name="troubleshooting"></a>Problemen oplossen

Hieronder vindt u stapsgewijze richt lijnen voor het oplossen van problemen met bewaking voor .NET-toepassingen die worden uitgevoerd op Azure VM en virtuele-machine schaal sets van Azure.

> [!NOTE]
> .NET core-, Java-en node. js-toepassingen worden alleen ondersteund op Azure VM-en Azure virtual machine-schaal sets via hand matige instrumentatie op basis van SDK. Daarom zijn de volgende stappen niet van toepassing op deze scenario's.

Uitvoer voor uitvoering van extensie wordt vastgelegd in bestanden die in de volgende directory's zijn gevonden:
```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.ApplicationMonitoringWindows\<version>\
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe u [een toepassing implementeert in een Azure virtual machine](../../virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app.md)-schaalset.
* [Stel Beschik baarheid](monitor-web-app-availability.md) -webtesten in om te worden gewaarschuwd als uw eind punt niet beschikbaar is.
