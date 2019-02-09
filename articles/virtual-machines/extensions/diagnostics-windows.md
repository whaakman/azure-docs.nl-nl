---
title: Azure PowerShell gebruiken voor het inschakelen van diagnostische gegevens op een Windows-VM | Microsoft Docs
services: virtual-machines-windows
documentationcenter: ''
description: Informatie over het gebruik van PowerShell voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows
author: sbtron
manager: jeconnoc
editor: ''
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: 520211f3499931281d3ac86a1da1144564a8bb48
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55980751"
---
# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>PowerShell gebruiken voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows

Azure Diagnostics is de functie in Azure waarmee u het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de diagnostics-extensie gebruiken voor het verzamelen van diagnostische gegevens, zoals van toepassingslogboeken of prestatiemeteritems van een Azure virtuele machine (VM) waarop Windows wordt uitgevoerd. 

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>De extensie voor diagnostische gegevens inschakelen als u de Resource Manager-implementatiemodel
Bij het maken van een virtuele Windows-machine via het Azure Resource Manager-implementatiemodel door de configuratie van de extensie toe te voegen aan de Resource Manager-sjabloon, kunt u de extensie voor diagnostische gegevens inschakelen. Zie [maken van een Windows-machine met controle en diagnostiek met behulp van de Azure Resource Manager-sjabloon](diagnostics-template.md).

Om in te schakelen de extensie voor diagnostische gegevens op een bestaande virtuele machine die is gemaakt via het Resource Manager-implementatiemodel, kunt u de [Set AzVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiagnosticsextension) PowerShell-cmdlet, zoals hieronder weergegeven.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* is het pad naar het bestand met de configuratie van diagnostische gegevens in XML-bestand, zoals beschreven in de [voorbeeld](#sample-diagnostics-configuration) hieronder.  

Als het configuratiebestand van de diagnostische gegevens Hiermee geeft u een **StorageAccount** element met de naam van een opslagaccount, dan zal de *Set AzVMDiagnosticsExtension* script stelt automatisch de diagnostische gegevens de extensie voor diagnostische gegevens verzenden naar dat opslagaccount. Om dit te werken, moet het opslagaccount zich in hetzelfde abonnement als de virtuele machine.

Als er geen **StorageAccount** is opgegeven in de configuratie van diagnostische gegevens, moet u om door te geven de *StorageAccountName* parameter aan de cmdlet. Als de *StorageAccountName* parameter is opgegeven, wordt de cmdlet gebruikt altijd de storage-account dat is opgegeven in de parameter en niet de naam die is opgegeven in het configuratiebestand van de diagnostische gegevens.

Als het opslagaccount voor diagnostische gegevens in een ander abonnement van de virtuele machine, dan u expliciet doorgeven moet de *StorageAccountName* en *StorageAccountKey* parameters aan de cmdlet. De *StorageAccountKey* parameter is niet nodig als het opslagaccount voor diagnostische gegevens in hetzelfde abonnement, aangezien de cmdlet kan automatisch query's uitvoeren en de waarde van de sleutel bij het inschakelen van de extensie voor diagnostische gegevens. Echter, als het opslagaccount voor diagnostische gegevens in een ander abonnement, en vervolgens de cmdlet mogelijk niet ophalen van de sleutel automatisch en u expliciet moet opgeven de sleutel via de *StorageAccountKey* parameter.  

    Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Zodra de diagnostics-extensie is ingeschakeld op een virtuele machine, kunt u de huidige instellingen ophalen met behulp van de [Get-AzVmDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiagnosticsextension) cmdlet.

    Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

De cmdlet retourneert *PublicSettings*, die de configuratie van de diagnostische gegevens bevat. Er zijn twee soorten configuratie wordt ondersteund, WadCfg en xmlCfg. WadCfg is JSON-configuratie en xmlCfg XML-configuratie in een indeling Base64-gecodeerd is. Als u wilt lezen van het XML-bestand, moet u dit decoderen.

    $publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

De [Remove-AzVmDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdiagnosticsextension) cmdlet kan worden gebruikt voor het verwijderen van de extensie voor diagnostische gegevens van de virtuele machine.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>De extensie voor diagnostische gegevens inschakelen als u het klassieke implementatiemodel
U kunt de [Set AzureVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) cmdlet om in te schakelen van een extensie voor diagnostische gegevens op een virtuele machine die u hebt gemaakt via het klassieke implementatiemodel. Het volgende voorbeeld ziet hoe u een nieuwe virtuele machine via het klassieke implementatiemodel maakt met de extensie voor diagnostische gegevens ingeschakeld.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Om in te schakelen de extensie voor diagnostische gegevens op een bestaande virtuele machine die is gemaakt via het klassieke implementatiemodel, gebruikt u eerst de [Get-AzureVM](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azurevm) cmdlet om op te halen van de VM-configuratie. Werk vervolgens de VM-configuratie om op te nemen van de extensie voor diagnostische gegevens met behulp van de [Set AzureVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) cmdlet. Ten slotte de bijgewerkte configuratie van toepassing op de virtuele machine met behulp van [Update-AzureVM](https://docs.microsoft.com/powershell/module/servicemanagement/azure/update-azurevm).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>In de configuratie van diagnostische gegevens
Het volgende XML-bestand kan worden gebruikt voor de diagnostische gegevens over openbare configuratie met de bovenstaande scripts. Deze voorbeeldconfiguratie wordt verschillende prestatiemeters overbrengen naar het opslagaccount voor diagnostische gegevens, samen met fouten van de toepassings-, beveiligings- en systeemkanalen in de Windows-gebeurtenislogboeken en eventuele fouten in de logboeken met diagnostische gegevens over infrastructuur.

De configuratie moet worden bijgewerkt met het volgende:

* De *resourceID* kenmerk van de **metrische gegevens** element moet worden bijgewerkt met de resource-ID voor de virtuele machine.
  
  * De resource-ID kan worden samengesteld met behulp van het volgende patroon: ' / subscriptions / {*abonnements-ID voor het abonnement met de virtuele machine*} /resourceGroups/ {*de resourcegroup-naam voor de virtuele machine*} / providers/Microsoft.Compute/virtualMachines/ {*naam van de VM*} '.
  * Bijvoorbeeld, als het abonnement-ID voor het abonnement waarin de virtuele machine wordt uitgevoerd is **11111111-1111-1111-1111-111111111111**, de naam van de resourcegroep voor de resourcegroep is **MyResourceGroup**, en de De naam van de virtuele machine is **MyWindowsVM**, klikt u vervolgens de waarde voor *resourceID* zou zijn:
    
      ```xml
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Voor meer informatie over hoe u metrische gegevens worden gegenereerd op basis van de configuratie van tellers en metrische gegevens over prestaties, Zie [metrische gegevenstabel voor Azure Diagnostics in opslag](diagnostics-template.md#wadmetrics-tables-in-storage).
* De **StorageAccount** element moet worden bijgewerkt met de naam van het opslagaccount voor diagnostische gegevens.
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het gebruik van de mogelijkheden van Azure Diagnostics en andere technieken voor het oplossen van problemen [diagnostische gegevens inschakelen in Azure Cloud Services en Virtual Machines](../../cloud-services/cloud-services-dotnet-diagnostics.md).
* [Configuratieschema van Diagnostics](https://msdn.microsoft.com/library/azure/mt634524.aspx) worden de verschillende XML-configuratie-opties voor de extensie voor diagnostische gegevens.

