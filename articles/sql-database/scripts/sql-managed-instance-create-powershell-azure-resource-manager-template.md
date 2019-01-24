---
title: 'PowerShell-voorbeeld: Een MI maken | Microsoft Docs'
description: Azure PowerShell-voorbeeldscript voor het maken van een MI
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: ''
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: bbb22f8d5eab3e60eba13bb642edb8f8d190cf67
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54387940"
---
# <a name="use-powershell-with-azure-resource-manager-template-to-create-an-azure-sql-database-managed-instance"></a>PowerShell gebruiken met een Azure Resource Manager-sjabloon om een MI te maken

Een MI kan worden gemaakt met behulp van een Azure PowerShell-bibliotheek en Azure Resource Manager-sjablonen. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

Azure PowerShell-opdrachten kunnen een implementatie starten met behulp van een vooraf gedefinieerde Azure Resource Manager-sjabloon. De volgende eigenschappen kunnen worden opgegeven in de sjabloon:
- Exemplaarnaam
- Gebruikersnaam en wachtwoord van SQL-beheerder. 
- Grootte van het exemplaar (aantal kernen en maximale opslaggrootte).
- VNet en subnet waar het exemplaar wordt geplaatst.
- Sortering op serverniveau van het exemplaar (preview).

Exemplaarnaam, gebruikersnaam van SQL-beheerder, VNet/subnet en sortering kunnen later niet worden gewijzigd. Andere exemplaareigenschappen kunnen worden gewijzigd.

## <a name="prerequisites"></a>Vereisten

In dit voorbeeld wordt ervan uitgegaan dat u[ een geldige netwerkomgeving hebt gemaakt](../sql-database-managed-instance-create-vnet-subnet.md) of [ een bestaand VNet](../sql-database-managed-instance-configure-vnet-subnet.md) hebt gewijzigd van uw beheerde exemplaar. In het voorbeeld worden de commandlets [New-AzureRmResourceGroupDeployment](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) en [Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) gebruikt, dus zorg ervoor dat u de volgende PowerShell-modules hebt geïnstalleerd:

```
Install-Module AzureRM.Network
Install-Module AzureRM.Resources
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon

De volgende inhoud moet worden geplaatst in een bestand dat een sjabloon representeert die wordt gebruikt om het exemplaar te maken:
```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "instance": {
            "type": "string"
        },
        "user": {
            "type": "string"
        },
        "pwd": {
            "type": "securestring"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('instance')]",
            "location": "West Central US",
            "tags": {
                "Description":"GP Instance with custom instance collation - Serbian_Cyrillic_100_CS_AS"
            },          
            "sku": {
                "name": "GP_Gen4",
                "tier": "GeneralPurpose"
            },
            "properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen4", 
                "collation": "Serbian_Cyrillic_100_CS_AS"
            },
            "type": "Microsoft.Sql/managedInstances",
            "identity": {
                "type": "SystemAssigned"
            },
            "apiVersion": "2015-05-01-preview"
        }
    ]
}
```
Hierbij wordt verondersteld dat Azure VNet al bestaat met het correct geconfigureerde subnet. Als dit niet het geval is, bereid de netwerkomgeving dan voor met behulp van een afzonderlijke [Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) die onafhankelijk of vanuit deze sjabloon kan worden uitgevoerd.

Sla de inhoud van dit bestand op als een JSON-bestand, plaats het bestandspad in het volgende PowerShell-script en verander de namen van de objecten in het script: 

```powershell
$subscriptionId = "ed827499-xxxx-xxxx-xxxx-xxxxxxxxxx"
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Managed Instance properties
$resourceGroup = "rg_mi"
$location = "West Central US"
$name = "managed-instance-name"
$user = "miSqlAdmin"
$secpasswd = ConvertTo-SecureString "<Put some strong password here>" -AsPlainText -Force

# Network configuration
$vNetName = "my_vnet"
$vNetResourceGroup = "rg_mi_vnet"
$subnetName = "ManagedInstances"
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $vNetResourceGroup
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vNet
$subnetId = $subnet.Id

# Deploy Instance using Azure Resource Manager template:
New-AzureRmResourceGroupDeployment  -Name MyDeployment -ResourceGroupName $resourceGroup  `
                                    -TemplateFile 'C:\...\create-managed-instance.json' `
                                    -instance $name -user $user -pwd $secpasswd -subnetId $subnetId
```
Nadat het script is uitgevoerd, is de SQL Database toegankelijk via alle Azure-services en het geconfigureerde IP-adres. 

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).



