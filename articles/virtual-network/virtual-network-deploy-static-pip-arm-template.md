---
title: Een virtuele machine maken met statische openbare IP-adres - Azure Resource Manager-sjabloon | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statische openbare IP-adres met een Azure Resource Manager-sjabloon.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f503aa60fdd9b7cf66ef482a1041e34c88e5c01
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Een virtuele machine maken met een statisch openbaar IP-adres met een Azure Resource Manager-sjabloon

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Sjabloon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel wordt behandeld met het implementatiemodel van Resource Manager, die Microsoft voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel aanbeveelt.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Openbare IP-adres resources in een sjabloonbestand
U kunt bekijken en download de [voorbeeldsjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

De volgende sectie ziet u de definitie van het openbare IP-resource, op basis van de bovenstaande scenario:

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

U ziet de **publicIPAllocationMethod** eigenschap die is ingesteld op *statische*. Deze eigenschap kan zijn *dynamische* (standaardwaarde) of *statische*. Instellen op statisch wordt gegarandeerd dat het openbare IP-adres toegewezen nooit wordt gewijzigd.

De volgende sectie ziet u de koppeling van het openbare IP-adres aan een netwerkinterface:

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

U ziet de **publicIPAddress** eigenschap die verwijst naar de **Id** van een resource met de naam **variables('webVMSetting').pipName**. Dat is de naam van het openbare IP-resource hierboven weergegeven.

Ten slotte de netwerkinterface die hierboven wordt vermeld in de **Schaalaanpassingsset** eigenschap van de virtuele machine wordt gemaakt.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met Klik om te implementeren

De voorbeeldsjabloon in de openbare opslagplaats maakt gebruik van een parameterbestand dat de standaardwaarden bevat voor het genereren van het hierboven beschreven scenario. Deze als sjabloon wilt implementeren met Klik om te implementeren, klikt u op **implementeren in Azure** in het bestand Readme.md voor de [VM met statische PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) sjabloon. De standaardwaarden voor de parameter vervangen indien gewenst en voer waarden in voor de lege parameters.  Volg de instructies in de portal voor het maken van een virtuele machine met een statisch openbaar IP-adres.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren met PowerShell

Volg onderstaande stappen als u de sjabloon die u hebt gedownload, wilt implementeren met PowerShell.

1. Als u Azure PowerShell nog nooit hebt gebruikt, voer de stappen in de [installeren en configureren van Azure PowerShell](/powershell/azure/overview) artikel.
2. Voer in een PowerShell-console de `New-AzureRmResourceGroup` cmdlet indien nodig een nieuwe resourcegroep maken. Als er al een resourcegroep hebt gemaakt, gaat u naar stap 3.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Verwachte uitvoer:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Voer in een PowerShell-console de `New-AzureRmResourceGroupDeployment` cmdlet om de sjabloon te implementeren.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Verwachte uitvoer:
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon implementeren met de Azure CLI
Als u wilt de sjabloon implementeren met behulp van de Azure CLI, moet u de volgende stappen uitvoeren:

1. Als u Azure CLI nog nooit hebt gebruikt, volg de stappen in de [installeren en configureren van de Azure CLI](../cli-install-nodejs.md) artikel om te installeren en configureren.
2. Voer de `azure config mode` opdracht overschakelen naar de modus Resource Manager, zoals hieronder wordt weergegeven.

    ```azurecli
    azure config mode arm
    ```

    De verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Open de [parameterbestand](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), selecteer de inhoud ervan en sla deze op een bestand op uw computer. Bijvoorbeeld, de parameters worden opgeslagen in een bestand met de naam *parameters.json*. De parameterwaarden in het bestand desgewenst wijzigen, maar minimaal wordt aanbevolen dat u de waarde voor de parameter adminPassword naar een unieke, complex wachtwoord wijzigen.
4. Voer de `azure group deployment create` cmd naar de nieuwe VNet te implementeren met behulp van de sjabloon en de parameterbestanden die u hebt gedownload en hierboven zijn gewijzigd. Vervang in de onderstaande opdracht <path> met het pad u het bestand moet worden opgeslagen. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Verwachte uitvoer (lijsten parameterwaarden gebruikt):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

