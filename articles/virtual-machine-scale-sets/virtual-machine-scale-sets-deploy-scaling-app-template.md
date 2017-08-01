---
title: Een app implementeren op een virtuele-machineschaalset in Azure | Microsoft Docs
description: Informatie over het implementeren van een eenvoudige, automatisch schalende toepassing op een virtuele-machineschaalset met een Azure Resource Manager-sjabloon.
services: virtual-machine-scale-sets
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/4/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 74f34bdbf5707510c682814716aa0b95c19a5503
ms.openlocfilehash: b2b0bbe2c8e07a9ee2f21983262a948acb90d03d
ms.contentlocale: nl-nl
ms.lasthandoff: 06/09/2017


---

# <a name="deploy-an-autoscaling-app-using-a-template"></a>Een automatisch schalende app implementeren met een sjabloon

[Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) zijn bijzonder handig om groepen gerelateerde resources te implementeren. In deze zelfstudie wordt verder ingegaan op [Een eenvoudige schaalset implementeren](virtual-machine-scale-sets-mvss-start.md) en wordt beschreven hoe u een eenvoudige, automatisch schalende toepassing kunt implementeren op een schaalset met een Azure Resource Manager-sjabloon.  U kunt automatisch schalen ook instellen met PowerShell, CLI of de portal. Zie het [overzicht van automatisch schalen](virtual-machine-scale-sets-autoscale-overview.md) voor meer informatie.

## <a name="two-quickstart-templates"></a>Twee snelstartsjablonen
Wanneer u een schaalset implementeert, kunt u nieuwe software installeren op een platforminstallatiekopie met een [VM-extensie](../virtual-machines/virtual-machines-windows-extensions-features.md). Een VM-extensie is een kleine toepassing waarmee configuratie- en automatiseringstaken na de implementatie worden uitgevoerd op virtuele Azure-machines, zoals het implementeren van een app. In [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) vindt u twee voorbeeldsjablonen waarin u kunt zien hoe u een automatisch schalende toepassing implementeert op een schaalset met VM-extensies.

### <a name="python-http-server-on-linux"></a>Python HTTP-server op Linux
Met de voorbeeldsjabloon [Python HTTP-server op Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) wordt een eenvoudige, automatisch schalende toepassing geïmplementeerd die wordt uitgevoerd op een Linux-schaalset.  Op elke VM in de schaalset worden [Bottle](http://bottlepy.org/docs/dev/), een Python-webframework, en een eenvoudige HTTP-server geïmplementeerd met behulp van een VM-extensie met een aangepast script. De schaalset schaalt omhoog wanneer gemiddeld CPU-gebruik over alle VM's hoger is dan 60% en schaalt omlaag wanneer het gemiddelde CPU-gebruik lager is dan 30%.

Naast de schaalsetresource worden met de voorbeeldsjabloon *azuredeploy.json* ook de resources voor het virtuele netwerk, het openbare IP-adres, de load balancer en instellingen voor automatisch schalen gedeclareerd.  Zie [Linux-schaalset met automatisch schalen](virtual-machine-scale-sets-linux-autoscale.md) voor meer informatie over het maken van deze resources in een sjabloon.

In de sjabloon *azuredeploy.json* wordt met de eigenschap `extensionProfile` van de resource `Microsoft.Compute/virtualMachineScaleSets` een aangepaste scriptextensie opgegeven. `fileUris` geeft de locatie van het script/de scripts aan. Dat zijn in dit geval twee bestanden: *workserver.py*, waarmee een eenvoudige HTTP-server wordt gedefinieerd, en *installserver.sh*, waarmee Bottle wordt geïnstalleerd en de HTTP-server wordt gestart. `commandToExecute` geeft de opdracht aan die moet worden uitgevoerd nadat de schaalset is geïmplementeerd.

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "lapextension",
                "properties": {
                  "publisher": "Microsoft.Azure.Extensions",
                  "type": "CustomScript",
                  "typeHandlerVersion": "2.0",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "fileUris": [
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
                    ],
                    "commandToExecute": "bash installserver.sh"
                  }
                }
              }
            ]
          }
```

### <a name="aspnet-mvc-application-on-windows"></a>ASP.NET MVC-toepassing in Windows
Met de voorbeeldsjabloon [ASP.NET MVC-toepassing in Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) wordt een eenvoudige ASP.NET MVC-app geïmplementeerd die wordt uitgevoerd in IIS op een Windows-schaalset.  IIS en de MVC-app worden geïmplementeerd met de VM-extensie [PowerShell Desired State Configuration (DSC)](virtual-machine-scale-sets-dsc.md).  De schaalset schaalt omhoog (één VM-exemplaar tegelijk) wanneer het CPU-gebruik gedurende vijf minuten hoger is dan 50%. 

Naast de schaalsetresource worden met de voorbeeldsjabloon *azuredeploy.json* ook de resources voor het virtuele netwerk, het openbare IP-adres, de load balancer en instellingen voor automatisch schalen gedeclareerd. In deze sjabloon kunt u ook een toepassingsupgrade bekijken.  Zie [Windows-schaalset met automatisch schalen](virtual-machine-scale-sets-windows-autoscale.md) voor meer informatie over het maken van deze resources in een sjabloon.

In de sjabloon *azuredeploy.json* wordt met de eigenschap `extensionProfile` van de resource `Microsoft.Compute/virtualMachineScaleSets` een [Desired State Configuration](virtual-machine-scale-sets-dsc.md)-extensie (DSC) opgegeven waarmee IIS en een standaardweb-app worden geïnstalleerd vanuit een WebDeploy-pakket.  Met het script *IISInstall.ps1* wordt IIS geïnstalleerd op de virtuele machine. Dit script kunt u vinden in de map *DSC*.  De MVC-web-app bevindt zich in de map *WebDeploy*.  De paden naar het installatiescript en de web-app zijn gedefinieerd in de parameters `powershelldscZip` en `webDeployPackage` in het bestand *azuredeploy.parameters.json*. 

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Powershell.DSC",
                "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.9",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('powershelldscUpdateTagVersion')]",
                  "settings": {
                    "configuration": {
                      "url": "[variables('powershelldscZipFullPath')]",
                      "script": "IISInstall.ps1",
                      "function": "InstallIIS"
                    },
                    "configurationArguments": {
                      "nodeName": "localhost",
                      "WebDeployPackagePath": "[variables('webDeployPackageFullPath')]"
                    }
                  }
                }
              }
            ]
          }
```

## <a name="deploy-the-template"></a>De sjabloon implementeren
U kunt de sjabloon [Python HTTP-server op Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) of [ASP.NET MVC-toepassing in Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) het eenvoudigst implementeren met de knop **Implementeren in Azure** in de Leesmij-bestanden in GitHub.  U kunt ook PowerShell of Azure CLI gebruiken om de voorbeeldsjablonen te implementeren.

### <a name="powershell"></a>PowerShell
Kopieer de bestanden voor [Python HTTP-server op Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) of [ASP.NET MVC-toepassing in Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) van de GitHub-opslagplaats naar een map op uw lokale computer.  Open het bestand *azuredeploy.parameters.json* en werk de standaardwaarden van de parameters `vmssName`, `adminUsername` en `adminPassword` bij. Sla het volgende PowerShell-script voor *deploy.ps1* op in dezelfde map als de sjabloon *azuredeploy.json*. Als u de voorbeeldsjabloon wilt implementeren, voert u het script *deploy.ps1* uit vanuit een PowerShell-opdrachtvenster.

```powershell
param(
 [Parameter(Mandatory=$True)]
 [string]
 $subscriptionId,

 [Parameter(Mandatory=$True)]
 [string]
 $resourceGroupName,

 [string]
 $resourceGroupLocation,

 [Parameter(Mandatory=$True)]
 [string]
 $deploymentName,

 [string]
 $templateFilePath = "azuredeploy.json",

 [string]
 $parametersFilePath = "azuredeploy.parameters.json"
)

<#
.SYNOPSIS
    Registers RPs
#>
Function RegisterRP {
    Param(
        [string]$ResourceProviderNamespace
    )

    Write-Host "Registering resource provider '$ResourceProviderNamespace'";
    Register-AzureRmResourceProvider -ProviderNamespace $ResourceProviderNamespace;
}

#******************************************************************************
# Script body
# Execution begins here
#******************************************************************************
$ErrorActionPreference = "Stop"

# sign in
Write-Host "Logging in...";
Login-AzureRmAccount;

# select subscription
Write-Host "Selecting subscription '$subscriptionId'";
Select-AzureRmSubscription -SubscriptionID $subscriptionId;

# Register RPs
$resourceProviders = @("microsoft.resources");
if($resourceProviders.length) {
    Write-Host "Registering resource providers"
    foreach($resourceProvider in $resourceProviders) {
        RegisterRP($resourceProvider);
    }
}

#Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
if(!$resourceGroup)
{
    Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start the deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

