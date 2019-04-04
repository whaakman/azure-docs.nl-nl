---
title: Diagnostische gegevens in Azure Cloud Services met behulp van PowerShell inschakelen | Microsoft Docs
description: Meer informatie over het inschakelen van diagnostische gegevens voor cloudservices met behulp van PowerShell
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 66e08754-8639-4022-ae18-4237749ba17d
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: jeconnoc
ms.openlocfilehash: b20fa7a1f43369cde85c2535637eec7ceb1d3c29
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918328"
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Diagnostische gegevens in Azure Cloud Services met behulp van PowerShell inschakelen
U kunt diagnostische gegevens, zoals toepassingslogboeken, verzamelen prestatiemeteritems enzovoort uit een Cloudservice met de Azure Diagnostics-extensie. In dit artikel wordt beschreven hoe u de Azure Diagnostics-extensie inschakelen voor een Cloudservice met behulp van PowerShell.  Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) voor de vereisten voor dit artikel.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>De extensie voor diagnostische gegevens inschakelen als onderdeel van het implementeren van een cloudservice
Deze benadering is van toepassing op type continue integratie van scenario's, waar de diagnostics-extensie kan worden ingeschakeld als onderdeel van de implementatie van de cloudservice. Bij het maken van een nieuwe Cloudservice-implementatie kunt u de extensie voor diagnostische gegevens inschakelen door te geven in de *ExtensionConfiguration* parameter voor de [New-Azure-implementatie](/powershell/module/servicemanagement/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet. De *ExtensionConfiguration* parameter heeft een matrix met configuraties voor diagnostische gegevens die kunnen worden gemaakt met de [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/servicemanagement/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.

Het volgende voorbeeld ziet hoe u diagnostische gegevens voor een cloudservice met een WebRole en WorkerRole, elk met een andere diagnostische gegevens over configuratie kunt inschakelen.

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

Als het configuratiebestand van de diagnostische gegevens Hiermee geeft u een `StorageAccount` element met de naam van een opslagaccount, dan zal de `New-AzureServiceDiagnosticsExtensionConfig` cmdlet automatisch gebruikmaken van dat opslagaccount. Om dit te werken, moet het opslagaccount in hetzelfde abonnement als de Service in de Cloud wordt geïmplementeerd.

Doeluitvoer bevat op basis van de configuratie van diagnostische gegevens tekenreeks die is opgegeven in het serviceconfiguratiebestand (.cscfg) naam van het opslagaccount van Azure SDK 2.6 ten minste de configuratiebestanden van de extensie die worden gegenereerd door het MSBuild publiceren. Het onderstaande script laat zien hoe u de configuratiebestanden van de extensie uit de uitvoer van de doel-publiceren parseren en extensie voor diagnostische gegevens voor elke rol configureren bij het implementeren van de cloudservice.

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find the Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

Visual Studio Online gebruikmaakt van een soortgelijke benadering voor geautomatiseerde implementaties van Cloud Services met de extensie voor diagnostische gegevens. Zie [publiceren AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) voor een compleet voorbeeld.

Als er geen `StorageAccount` is opgegeven in de configuratie van diagnostische gegevens, moet u om door te geven de *StorageAccountName* parameter aan de cmdlet. Als de *StorageAccountName* parameter is opgegeven, wordt de cmdlet gebruikt altijd de storage-account dat is opgegeven in de parameter en niet de naam die is opgegeven in het configuratiebestand van de diagnostische gegevens.

Als het opslagaccount voor diagnostische gegevens in een ander abonnement via de Cloudservice is, dan u expliciet doorgeven moet de *StorageAccountName* en *StorageAccountKey* parameters aan de cmdlet. De *StorageAccountKey* parameter is niet nodig als het opslagaccount voor diagnostische gegevens in hetzelfde abonnement, aangezien de cmdlet kan automatisch query's uitvoeren en de waarde van de sleutel bij het inschakelen van de extensie voor diagnostische gegevens. Echter, als het opslagaccount voor diagnostische gegevens in een ander abonnement, en vervolgens de cmdlet mogelijk niet ophalen van de sleutel automatisch en u expliciet moet opgeven de sleutel via de *StorageAccountKey* parameter.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>De extensie voor diagnostische gegevens inschakelen voor een bestaande cloudservice
U kunt de [Set AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet in-of werk de configuratie van diagnostische gegevens op een Cloudservice die al wordt uitgevoerd.

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>De huidige configuratie van de extensie voor diagnostische gegevens ophalen
Gebruik de [Get-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet om op te halen van de huidige configuratie van diagnostische gegevens voor een cloudservice.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>De extensie voor diagnostische gegevens verwijderen
Uitschakelen van diagnostische gegevens op een cloudservice kunt u de [Remove-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Als u de extensie voor diagnostische gegevens met behulp van ingeschakeld *Set AzureServiceDiagnosticsExtension* of de *New-AzureServiceDiagnosticsExtensionConfig* zonder de *rol*parameter en u kunt de extensie verwijderen met *Remove-AzureServiceDiagnosticsExtension* zonder de *rol* parameter. Als de *rol* parameter is gebruikt bij het inschakelen van de extensie vervolgens het moet ook worden gebruikt bij het verwijderen van de extensie.

De extensie voor diagnostische gegevens verwijderen voor elke afzonderlijke rol:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het gebruik van Azure diagnostics en andere technieken voor het oplossen van problemen [diagnostische gegevens inschakelen in Azure Cloud Services en Virtual Machines](cloud-services-dotnet-diagnostics.md).
* De [configuratieschema van Diagnostics](/azure/azure-monitor/platform/diagnostics-extension-schema-1dot2) worden de verschillende XML-configuratie-opties voor de extensie voor diagnostische gegevens.
* Zie voor informatie over het inschakelen van de extensie voor diagnostische gegevens voor virtuele Machines, [maken van een virtuele Windows-machine met controle en diagnostiek met behulp van Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md)
