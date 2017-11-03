---
title: Een virtuele machine maken met meerdere NIC's - Azure Resource Manager-sjabloon | Microsoft Docs
description: Een virtuele machine maken met meerdere NIC's met een Azure Resource Manager-sjabloon.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85bfa264c6cf2b0586816a47b3ab72f3aee8ec96
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a>Een virtuele machine met meerdere NIC's met een sjabloon maken
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md).  Dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het [klassieke implementatiemodel](virtual-network-deploy-multinic-classic-ps.md).
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

De volgende stappen gebruikt u een resourcegroep met de naam *IaaSStory* voor de webservers en een resourcegroep met de naam *IaaSStory-back-end* voor de database-servers.

## <a name="prerequisites"></a>Vereisten
Voordat u de database-servers maken kunt, moet u maken de *IaaSStory* resourcegroep met alle benodigde resources voor dit scenario. Voor het maken van deze bronnen, moet u de volgende stappen uitvoeren:

1. Navigeer naar [de sjabloonpagina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Op de sjabloonpagina rechts van **bovenliggende resourcegroep**, klikt u op **implementeren in Azure**.
3. Indien nodig, de parameterwaarden te wijzigen en volg de stappen in de Azure preview-portal voor het implementeren van de resourcegroep.

> [!IMPORTANT]
> Zorg ervoor dat de namen van opslagaccounts uniek zijn. Er kan geen dubbele opslagaccountnamen in Azure.
> 

## <a name="understand-the-deployment-template"></a>De sjabloon voor de implementatie begrijpen
Voordat u de sjabloon van deze documentatie implementeert, zorg er dan voor dat u begrijpt wat het doet. De volgende stappen bevatten een goed overzicht van de sjabloon:

1. Navigeer naar [de sjabloonpagina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klik op **azuredeploy.json** om de sjabloonbestand te openen.
3. U ziet de *besturingssysteemtype* parameter hieronder vermeld. Deze parameter wordt gebruikt om te selecteren welke VM-installatiekopie moet worden gebruikt voor de databaseserver, samen met meerdere besturingssysteem gerelateerde instellingen.

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS to use for VMs: Windows or Ubuntu."
      }
    },
    ```

4. Schuif helemaal naar de lijst met variabelen en controleer de definitie voor de **dbVMSetting** variabelen, hieronder vermeld. Het ontvangt een van de matrixelementen in de **dbVMSettings** variabele. Als u bekend met software development terminologie bent, vindt u de **dbVMSettings** variabele als hash-tabel of een woordenlijst.

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. Stel dat u wilt implementeren, VM's Windows SQL uitgevoerd in de back-end. Vervolgens de waarde voor **besturingssysteemtype** zou *Windows*, en de **dbVMSetting** variabele zou het hieronder vermelde element dat staat voor de eerste waarde in de bevatten**dbVMSettings** variabele.

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. U ziet de **vmSize** bevat de waarde *Standard_DS3*. Het gebruik van meerdere NIC's kunnen alleen bepaalde VM-grootten. U kunt controleren welke VM-grootten ondersteunen meerdere NIC's door te lezen van de [Windows VM-grootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [Linux VM-grootten](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikelen.

7. Schuif omlaag naar **resources** en Let op het eerste element. Een opslagaccount worden beschreven. Dit opslagaccount wordt gebruikt de gegevensschijven die wordt gebruikt door elke VM-database te onderhouden. In dit scenario wordt heeft elke database VM een besturingssysteemschijf opgeslagen in reguliere storage en twee gegevensschijven opgeslagen in (premium) SSD-opslag.

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. Schuif omlaag naar de volgende resource, zoals hieronder weergegeven. Deze bron vertegenwoordigt de NIC die wordt gebruikt voor toegang tot de database in elke VM-database. Let op het gebruik van de **kopie** functie in deze resource. De sjabloon kunt u implementeren als veel virtuele machines als u wilt, op basis van de **dbCount** parameter. Daarom moet u dezelfde hoeveelheid NIC's voor toegang tot de database, één voor elke virtuele machine maken.

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. Schuif omlaag naar de volgende resource, zoals hieronder weergegeven. Deze bron vertegenwoordigt de NIC die wordt gebruikt voor beheer in elke VM-database. Nogmaals, moet u een van deze NIC's voor elke database VM. U ziet de **networkSecurityGroup** element een NSG die toegang tot RDP/SSH op deze NIC alleen verleent koppelen.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. Schuif omlaag naar de volgende resource, zoals hieronder weergegeven. Deze bron vertegenwoordigt een beschikbaarheidsset moeten worden gedeeld door alle virtuele machines van database. Op die manier garandeert u dat er altijd worden één virtuele machine in de set uitvoeren tijdens onderhoud.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. Schuif omlaag naar de volgende resource. Deze bron vertegenwoordigt de database VM's, zoals weergegeven in de eerste paar regels die hieronder worden vermeld. Let op het gebruik van de **kopie** werken opnieuw, waarbij u ervoor zorgt dat meerdere virtuele machines worden gemaakt op basis van de **dbCount** parameter. Ook ziet de **dependsOn** verzameling. Hiermee geeft u twee NIC's die nodig zijn om te worden gemaakt voordat de virtuele machine is geïmplementeerd, samen met de beschikbaarheidsset en het opslagaccount wordt.

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. Schuif omlaag in de VM-resource toe aan de **Schaalaanpassingsset** element, zoals hieronder weergegeven. U ziet dat er twee NIC's wordt naslaginformatie voor elke virtuele machine. Als u meerdere NIC's voor een virtuele machine maakt, moet u instellen de **primaire** eigenschap van een van de NIC's voor *true*, en de rest aan *false*.

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>De ARM-sjabloon implementeren met klik om te implementeren

> [!IMPORTANT]
> Zorg ervoor dat u volgt de [vereisten](#Pre-requisites) stappen vóór de onderstaande instructies.
> 

De voorbeeldsjabloon in de openbare opslagplaats maakt gebruik van een parameterbestand dat de standaardwaarden bevat voor het genereren van het hierboven beschreven scenario. Volg voor het implementeren van deze sjabloon implementeren met Klik [deze koppeling](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), rechts van **back-end resourcegroep (Zie de documentatie)** klikt u op **implementeren in Azure**, vervang de standaard parameterwaarden indien nodig, en volg de instructies in de portal.

De afbeelding hieronder ziet de inhoud van de nieuwe resourcegroep na de implementatie.

![Back-end van resourcegroep](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren met PowerShell
Voor het implementeren van de sjabloon die u hebt gedownload met behulp van PowerShell PowerShell installeren en configureren via de stappen in de [PowerShell installeren en configureren](/powershell/azure/overview) en voltooi vervolgens de volgende stappen uit:

Voer de  **`New-AzureRmResourceGroup`**  cmdlet om een resourcegroep met de sjabloon te maken.

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

Verwachte uitvoer:

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon implementeren met de Azure CLI
Volg onderstaande stappen als u de sjabloon wilt implementeren met de Azure CLI.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../cli-install-nodejs.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voer de opdracht **`azure config mode`** uit om over te schakelen naar de modus Resource Manager, zoals hieronder weergegeven.

    ```azurecli
    azure config mode arm
    ```

    Hier volgt de verwachte uitvoer:

        info:    New mode is arm

3. Open de [parameterbestand](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), selecteer de inhoud en sla deze op een bestand op uw computer. Hier is het parameterbestand opgeslagen als *parameters.json*.
4. Voer de **`azure group deployment create`** cmdlet uit om de nieuwe VNet te implementeren met behulp van de sjabloon en de parameterbestanden die u hebt gedownload en hierboven zijn gewijzigd. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    Verwachte uitvoer:
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

