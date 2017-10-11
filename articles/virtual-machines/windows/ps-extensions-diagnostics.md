---
title: Azure PowerShell gebruiken voor diagnostische gegevens op een virtuele machine van Windows inschakelen | Microsoft Docs
services: virtual-machines-windows
documentationcenter: 
description: Informatie over het gebruik van PowerShell voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows
author: sbtron
manager: timlt
editor: 
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: d0be4a712657edfc516c5f32e66519f5d9486728
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>PowerShell gebruiken voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure Diagnostics is de functie binnen Azure waarmee het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens gebruiken voor het verzamelen van diagnostische gegevens, zoals toepassingslogboeken of prestatiemeteritems van een Azure virtuele machine (VM) waarop Windows wordt uitgevoerd. In dit artikel wordt beschreven hoe u Windows PowerShell gebruikt voor het inschakelen van de extensie voor diagnostische gegevens voor een virtuele machine. Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview) voor de vereisten die nodig zijn voor dit artikel.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>De extensie voor diagnostische gegevens inschakelen als u het implementatiemodel van Resource Manager gebruiken
Bij het maken van een virtuele Windows-machine via het Azure Resource Manager-implementatiemodel door de configuratie voor de uitbreiding aan de Resource Manager-sjabloon toe te voegen, kunt u de extensie voor diagnostische gegevens inschakelen. Zie [virtuele Windows-machine maken met controle en diagnostische gegevens met behulp van de Azure Resource Manager-sjabloon](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Als u wilt dat de extensie voor diagnostische gegevens op een bestaande virtuele machine die is gemaakt via het implementatiemodel van Resource Manager, kunt u de [Set AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell-cmdlet, zoals hieronder wordt weergegeven.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* is het pad naar het bestand met de configuratie van de diagnostische gegevens in XML, zoals beschreven in de [voorbeeld](#sample-diagnostics-configuration) hieronder.  

Als het configuratiebestand van de diagnostische gegevens wordt een **StorageAccount** element met de naam van een opslagaccount, wordt de *Set AzureRMVMDiagnosticsExtension* script stelt automatisch de diagnostics-extensie voor diagnostische gegevens verzenden naar dit opslagaccount. Dit werkt, moet het opslagaccount zich in hetzelfde abonnement als de virtuele machine.

Als er geen **StorageAccount** is opgegeven in de configuratie van diagnostische gegevens, moet u doorgeeft in de *StorageAccountName* parameter aan de cmdlet. Als de *StorageAccountName* parameter wordt opgegeven, wordt de cmdlet gebruik altijd het opslagaccount dat is opgegeven in de parameter en niet de naam die is opgegeven in het configuratiebestand van de diagnostische gegevens.

Als het opslagaccount voor diagnostische gegevens zich in een ander abonnement van de virtuele machine, moet u expliciet de *StorageAccountName* en *StorageAccountKey* parameters aan de cmdlet. De *StorageAccountKey* parameter is niet nodig als het opslagaccount voor diagnostische gegevens in hetzelfde abonnement behoren, aangezien de cmdlet kan automatisch een query en de waarde van de sleutel bij het inschakelen van de extensie voor diagnostische gegevens. Evenwel opgeven als het opslagaccount voor diagnostische gegevens in een ander abonnement en vervolgens de cmdlet mogelijk niet de sleutel automatisch ophalen en u expliciet moet de sleutel via de *StorageAccountKey* parameter.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Wanneer de extensie voor diagnostische gegevens op een virtuele machine is ingeschakeld, kunt u de huidige instellingen met behulp van de [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) cmdlet.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

De cmdlet retourneert *PublicSettings*, die de configuratie van diagnostische gegevens bevat. Er zijn twee soorten configuratie wordt ondersteund, WadCfg en xmlCfg. WadCfg configuratie uit JSON en xmlCfg XML-configuratie in een Base64-gecodeerde indeling. Als u wilt lezen van het XML-bestand, moet u dit decoderen.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

De [verwijderen AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) cmdlet kan de extensie voor diagnostische gegevens verwijderen uit de virtuele machine worden gebruikt.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>De extensie voor diagnostische gegevens inschakelen als u het klassieke implementatiemodel
U kunt de [Set AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet om in te schakelen van een extensie voor diagnostische gegevens op een virtuele machine die u via het klassieke implementatiemodel. Het volgende voorbeeld laat zien hoe een nieuwe virtuele machine via het klassieke implementatiemodel maken met de extensie voor diagnostische gegevens ingeschakeld.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Als u wilt dat de extensie voor diagnostische gegevens op een bestaande virtuele machine die is gemaakt via het klassieke implementatiemodel, voor het eerst gebruiken de [Get-AzureVM](/powershell/module/azure/get-azurevm) cmdlet ophalen van de VM-configuratie. Werk vervolgens het VM-configuratie zodanig dat de extensie voor diagnostische gegevens met behulp van de [Set AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet. Ten slotte de bijgewerkte configuratie van toepassing op de virtuele machine met behulp van [Update-AzureVM](/powershell/module/azure/update-azurevm).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>In de configuratie van diagnostische gegevens
De volgende XML-code kan worden gebruikt voor de openbare configuratie van diagnostische gegevens met de bovenstaande scripts. De voorbeeldconfiguratie van dit wordt verschillende prestatiemeteritems overbrengen naar het opslagaccount voor diagnostische gegevens, samen met fouten van de toepassings-, beveiligings- en systeemkanalen in de Windows-gebeurtenislogboeken en fouten van de infrastructuur diagnostische logboeken.

De configuratie moet worden bijgewerkt met het volgende:

* De *resourceID* kenmerk van de **metrische gegevens** element moet worden bijgewerkt met de bron-ID voor de virtuele machine.
  
  * De resource-ID kan worden samengesteld met behulp van het volgende patroon volgen: "/ subscriptions / {*abonnements-ID voor het abonnement met de virtuele machine*} /resourceGroups/ {*resourcegroup-naam voor de virtuele machine*} / providers/Microsoft.Compute/virtualMachines/ {*naam van de VM*} '.
  * Bijvoorbeeld, als het abonnement-ID voor het abonnement waarop de virtuele machine wordt uitgevoerd is **11111111-1111-1111-1111-111111111111**, de naam van de resourcegroep voor de resourcegroep is **MyResourceGroup**, en de naam van de VM is **MyWindowsVM**, moet u de waarde voor *resourceID* zou zijn:
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Voor meer informatie over de metrische gegevens worden gegenereerd op basis van de configuratie van de prestaties tellers en metrische gegevens, Zie [Azure Diagnostics metrische gegevens tabel in de opslag](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).
* De **StorageAccount** element moet worden bijgewerkt met de naam van het opslagaccount voor diagnostische gegevens.
  
    ```
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
* Zie voor meer informatie over het gebruik van de mogelijkheid Azure Diagnostics- en andere technieken voor het oplossen van problemen [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](../../cloud-services/cloud-services-dotnet-diagnostics.md).
* [Diagnostische gegevens configuraties schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) worden de verschillende XML-configuraties opties voor de extensie voor diagnostische gegevens.

