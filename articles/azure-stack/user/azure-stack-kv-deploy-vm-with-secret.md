---
title: Een virtuele machine met veilig opgeslagen wachtwoord op Azure-Stack implementeren | Microsoft Docs
description: Informatie over het implementeren van een virtuele machine met een wachtwoord opgeslagen in Azure Stack Sleutelkluis
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 23322a49-fb7e-4dc2-8d0e-43de8cd41f80
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/07/2018
ms.author: mabrigg
ms.openlocfilehash: 4239eb31afd4abc8b3555f0ee353f5d96716d623
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="create-a-virtual-machine-using-a-secure-password-stored-in-azure-stack-key-vault"></a>Maken van een virtuele machine via een beveiligd wachtwoord opgeslagen in Azure Stack Sleutelkluis

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

In dit artikel wordt stapsgewijs instructies voor het implementeren van een Windows Server-virtuele machine met een wachtwoord opgeslagen in Azure Stack Sleutelkluis. Een sleutelkluis-wachtwoord is veiliger dan het doorgeven van een wachtwoord als tekst zonder opmaak.

## <a name="overview"></a>Overzicht

U kunt waarden zoals een wachtwoord opslaan als een geheim in de sleutelkluis voor een Azure-Stack. Nadat u een geheim hebt gemaakt, kunt u het verwijzen naar in Azure Resource Manager-sjablonen. Met behulp van geheimen met Resource Manager biedt de volgende voordelen:

* U hoeft niet te geheim handmatig invoeren telkens wanneer die u een resource implementeert.
* U kunt opgeven welke gebruikers of service-principals toegang krijgen een geheim tot.

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met de Sleutelkluis-service.
* [Installeer PowerShell voor Azure-Stack.](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving.](azure-stack-powershell-configure-user.md)

De volgende stappen beschrijven het proces is vereist voor het maken van een virtuele machine door op te halen van het wachtwoord dat is opgeslagen in een Sleutelkluis:

1. Een Sleutelkluis geheim maken.
2. Het bestand azuredeploy.parameters.json bijwerken.
3. De sjabloon implementeert.

>[OPMERKING] U kunt deze stappen van de Azure-Stack Development Kit of van een externe client gebruiken als u via VPN-verbinding verbonden bent.

## <a name="create-a-key-vault-secret"></a>Een Sleutelkluis geheim maken

Het volgende script maakt een sleutelkluis en een wachtwoord wordt opgeslagen in de sleutelkluis als een geheim. Gebruik de `-EnabledForDeployment` parameter als u de sleutelkluis. Deze parameter zorgt ervoor dat de sleutelkluis kan worden verwezen vanuit de Azure Resource Manager-sjablonen.

```powershell

$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "MySecret"

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location
  -EnabledForTemplateDeployment

$secretValue = ConvertTo-SecureString -String '<Password for your virtual machine>' -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
  -SecretValue $secretValue

```

Wanneer u het vorige script uitvoert, wordt de uitvoer de geheime URI bevat. Noteer deze URI. U moet verwijzen naar deze in de [Windows implementeren virtuele machine met een wachtwoord in de sjabloon sleutelkluis](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv). Download de [101-vm-beveiligd-wachtwoord](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) map op uw ontwikkelcomputer. Deze map bevat de `azuredeploy.json` en `azuredeploy.parameters.json` bestanden, wat u in de volgende stappen moet.

Wijzig de `azuredeploy.parameters.json` bestand volgens de Omgevingswaarden in de. De parameters van speciaal belang zijn de kluisnaam van de, de resourcegroep voor de kluis en het geheim URI (zoals gegenereerd door het vorige script). Het volgende bestand is een voorbeeld van een parameterbestand:

## <a name="update-the-azuredeployparametersjson-file"></a>Het bestand azuredeploy.parameters.json bijwerken

Het bestand azuredeploy.parameters.json bijwerken met de KeyVault-URI, secretName, adminUsername van de waarden van de virtuele machine aan de hand van uw omgeving. Het volgende JSON-bestand ziet u een voorbeeld van de sjabloon parameters:

```json
{
    "$schema":  "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion":  "1.0.0.0",
    "parameters":  {
       "adminUsername":  {
         "value":  "demouser"
          },
         "adminPassword":  {
           "reference":  {
              "keyVault":  {
                "id":  "/subscriptions/xxxxxx/resourceGroups/RgKvPwd/providers/Microsoft.KeyVault/vaults/KvPwd"
                },
              "secretName":  "MySecret"
           }
         },
       "dnsLabelPrefix":  {
          "value":  "mydns123456"
        },
        "windowsOSVersion":  {
          "value":  "2016-Datacenter"
        }
    }
}

```

## <a name="template-deployment"></a>Sjabloonimplementatie

Nu u de sjabloon implementeren met behulp van de volgende PowerShell-script:

```powershell
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Wanneer de sjabloon wordt geïmplementeerd, resulteert dit in de volgende uitvoer:

![Implementatie-uitvoer](media/azure-stack-kv-deploy-vm-with-secret/deployment-output.png)

## <a name="next-steps"></a>Volgende stappen

[Een voorbeeld-app met Sleutelkluis implementeren](azure-stack-kv-sample-app.md)

[Een virtuele machine met een certificaat Sleutelkluis implementeren](azure-stack-kv-push-secret-into-vm.md)
