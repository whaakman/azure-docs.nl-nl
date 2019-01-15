---
title: Een VM implementeren met het wachtwoord veilig opgeslagen op Azure Stack | Microsoft Docs
description: Informatie over het implementeren van een virtuele machine met behulp van een wachtwoord opgeslagen in Azure Stack Key Vault
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/14/2019
ms.author: mabrigg
ms.openlocfilehash: 8d853ec6b81340577598a66e048d69fe9b15a83c
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302626"
---
# <a name="create-a-virtual-machine-using-a-secure-password-stored-in-azure-stack-key-vault"></a>Een virtuele machine met behulp van een veilig wachtwoord opgeslagen in Azure Stack Key Vault maken

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In dit artikel wordt stapsgewijs uitgelegd voor het implementeren van een Windows Server virtuele machine met behulp van een wachtwoord opgeslagen in Azure Stack Key Vault. Met behulp van een key vault-wachtwoord is veiliger dan het doorgeven van een wachtwoord als tekst zonder opmaak.

## <a name="overview"></a>Overzicht

U kunt waarden, zoals een wachtwoord opslaan als een geheim in een Azure Stack-sleutelkluis. Nadat u een geheim hebt gemaakt, kunt u ernaar kunt verwijzen in Azure Resource Manager-sjablonen. Met behulp van geheimen met Resource Manager biedt de volgende voordelen:

* U hebt geen handmatig geheim invoeren telkens wanneer die u een resource implementeert.
* U kunt opgeven welke gebruikers of service-principals toegang krijgen een geheim tot.

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met de service Key Vault.
* [Installeer PowerShell voor Azure Stack.](azure-stack-powershell-install.md)
* [Configureer uw PowerShell-omgeving.](azure-stack-powershell-configure-user.md)

De volgende stappen beschrijven het proces dat is vereist voor het maken van een virtuele machine door op te halen van het wachtwoord die zijn opgeslagen in een Key Vault:

1. Maak een Key Vault geheim.
2. Het bestand azuredeploy.parameters.json bijwerken.
3. Implementeer de sjabloon.

> [!NOTE]  
> U kunt deze stappen van de Azure Stack Development Kit, of van een externe client gebruiken als u via VPN-verbinding verbonden bent.

## <a name="create-a-key-vault-secret"></a>Maak een Key Vault secret

Het volgende script maakt een key vault en een wachtwoord wordt opgeslagen in de key vault als een geheim. Gebruik de `-EnabledForDeployment` parameter bij het maken van de key vault. Deze parameter zorgt ervoor dat de key vault kan worden verwezen vanuit Azure Resource Manager-sjablonen.

```PowerShell

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

Wanneer u het vorige script uitvoert, wordt in de uitvoer de geheime URI bevat. Maak een notitie van deze URI. U moet verwijzen naar deze in de [implementeren Windows virtuele machine met een wachtwoord in key vault sjabloon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv). Download de [101-vm-beveiligde-wachtwoord](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) map op uw ontwikkelcomputer. Deze map bevat de `azuredeploy.json` en `azuredeploy.parameters.json` bestanden, die u in de volgende stappen.

Wijzig de `azuredeploy.parameters.json` bestand op basis van de omgevingswaarden van uw. De parameters van speciaal belang zijn de naam van de kluis, de resourcegroep van de kluis en de geheime URI (zoals die worden gegenereerd door het vorige script). Het volgende bestand is een voorbeeld van een parameterbestand:

## <a name="update-the-azuredeployparametersjson-file"></a>Het bestand azuredeploy.parameters.json bijwerken

Het bestand azuredeploy.parameters.json bijwerken met de KeyVault-URI, secretName, adminUsername van de waarden van de virtuele machine aan de hand van uw omgeving. De volgende JSON-bestand toont een voorbeeld van het bestand met sjabloonparameters:

```json
{
    "$schema":  "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

De sjabloon nu implementeren met behulp van de volgende PowerShell-script:

```PowerShell  
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Wanneer de sjabloon is geïmplementeerd, resulteert dit in de volgende uitvoer:

![Implementatie-uitvoer](media/azure-stack-key-vault-deploy-vm-with-secret/deployment-output.png)

## <a name="next-steps"></a>Volgende stappen

* [Implementeer een voorbeeld-app met Key Vault](azure-stack-key-vault-sample-app.md)
* [Een VM implementeren met een Key Vault-certificaat](azure-stack-key-vault-push-secret-into-vm.md)
