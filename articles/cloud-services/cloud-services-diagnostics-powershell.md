---
title: Schakel diagnostische gegevens in Azure Cloud Services met behulp van PowerShell | Microsoft Docs
description: Informatie over het inschakelen van diagnostische gegevens voor cloudservices met behulp van PowerShell
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 66e08754-8639-4022-ae18-4237749ba17d
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: adegeo
ms.openlocfilehash: 8dd9724981860c9cd4ccc443cc2bfdc465811e7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Diagnostische gegevens in Azure Cloud Services met behulp van PowerShell inschakelen
U kunt diagnostische gegevens, zoals Logboeken, verzamelen prestatiemeteritems enzovoort van een Cloudservice met de extensie Azure Diagnostics. In dit artikel wordt beschreven hoe de Azure Diagnostics-extensie inschakelen voor een Cloudservice met behulp van PowerShell.  Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview) voor de vereisten die nodig zijn voor dit artikel.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>De extensie voor diagnostische gegevens inschakelen als onderdeel van het implementeren van een cloudservice
Deze aanpak is van toepassing op type continue integratie van scenario's, waarbij de extensie voor diagnostische gegevens kan worden ingeschakeld als onderdeel van de implementatie van de cloudservice. Bij het maken van een nieuwe Cloudservice-implementatie kunt u de extensie voor diagnostische gegevens inschakelen door door te geven in de *ExtensionConfiguration* -parameter voor de [nieuw AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet. De *ExtensionConfiguration* parameter neemt een matrix van configuraties voor diagnostische gegevens die kunnen worden gemaakt met de [nieuw AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.

Het volgende voorbeeld ziet hoe u diagnostische gegevens voor een cloudservice met een WebRole en WorkerRole, elk met de configuratie van een andere diagnostische kunt inschakelen.

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

Als het configuratiebestand van de diagnostische gegevens wordt een `StorageAccount` element met de naam van een opslagaccount, wordt de `New-AzureServiceDiagnosticsExtensionConfig` cmdlet automatisch gebruikmaken van dit opslagaccount. Dit werkt, moet het storage-account zich in hetzelfde abonnement als de Cloud-Service wordt geïmplementeerd.

Azure SDK versie 2.6 bewerken publiceren van de extensieconfiguratiebestanden die worden gegenereerd door het MSBuild bevat Doeluitvoer de naam van de opslag op basis van de configuratietekenreeks voor diagnostische gegevens opgegeven in het configuratiebestand (.cscfg) van de service. Het onderstaande script laat zien hoe u de configuratiebestanden voor de uitbreiding van de doel-uitvoer publiceren parseren en het configureren van extensie voor diagnostische gegevens voor elke rol bij het implementeren van de cloudservice.

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

Visual Studio Online maakt gebruik van een soortgelijke benadering voor geautomatiseerde implementaties van Cloud Services met de extensie voor diagnostische gegevens. Zie [publiceren AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) voor een compleet voorbeeld.

Als er geen `StorageAccount` is opgegeven in de configuratie van diagnostische gegevens, moet u doorgeeft in de *StorageAccountName* parameter aan de cmdlet. Als de *StorageAccountName* parameter wordt opgegeven, wordt de cmdlet gebruik altijd het opslagaccount dat is opgegeven in de parameter en niet de naam die is opgegeven in het configuratiebestand van de diagnostische gegevens.

Als het opslagaccount voor diagnostische gegevens in een ander abonnement uit de Cloudservice is, moet u expliciet de *StorageAccountName* en *StorageAccountKey* parameters aan de cmdlet. De *StorageAccountKey* parameter is niet nodig als het opslagaccount voor diagnostische gegevens in hetzelfde abonnement behoren, aangezien de cmdlet kan automatisch een query en de waarde van de sleutel bij het inschakelen van de extensie voor diagnostische gegevens. Evenwel opgeven als het opslagaccount voor diagnostische gegevens in een ander abonnement en vervolgens de cmdlet mogelijk niet de sleutel automatisch ophalen en u expliciet moet de sleutel via de *StorageAccountKey* parameter.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>De extensie voor diagnostische gegevens inschakelen voor een bestaande cloudservice
U kunt de [Set AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet inschakelen of werk de configuratie van diagnostische gegevens op een Cloudservice die al wordt uitgevoerd.

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
Gebruik de [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet ophalen van de huidige configuratie van diagnostische gegevens voor een cloudservice.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>De extensie voor diagnostische gegevens verwijderen
Uitschakelen van diagnostische gegevens op een cloudservice kunt u de [verwijderen AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Als u de extensie voor diagnostische gegevens met behulp van ingeschakeld *Set AzureServiceDiagnosticsExtension* of de *nieuw AzureServiceDiagnosticsExtensionConfig* zonder de *rol*u vervolgens een parameter kan de extensie verwijderen met *verwijderen AzureServiceDiagnosticsExtension* zonder de *rol* parameter. Als de *rol* parameter is gebruikt bij het inschakelen van de uitbreiding wordt moet ook worden gebruikt bij het verwijderen hiervan.

De extensie voor diagnostische gegevens verwijderen voor elke afzonderlijke rol:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het gebruik van Azure diagnoses en andere technieken voor het oplossen van problemen [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](cloud-services-dotnet-diagnostics.md).
* De [Diagnostics configuratieschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) worden de verschillende XML-configuraties opties voor de extensie voor diagnostische gegevens.
* Zie voor meer informatie over het inschakelen van de extensie voor diagnostische gegevens voor virtuele Machines, [een Windows virtuele machine met de controle en diagnostische gegevens met Azure Resource Manager-sjabloon maken](../virtual-machines/windows/extensions-diagnostics-template.md)
