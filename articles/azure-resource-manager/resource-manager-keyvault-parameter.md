---
title: Key Vault-geheim met Azure Resource Manager-sjabloon | Microsoft Docs
description: Laat zien hoe een geheim uit key vault als een parameter doorgeven tijdens implementatie.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/09/2018
ms.author: tomfitz
ms.openlocfilehash: 3a29319a0d478537dfc4905ee77865b8fea64587
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38598404"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Azure Key Vault gebruiken voor veilige parameterwaarde doorgeven tijdens implementatie

Als u een veilige waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie wilt, haalt u de waarde van een [Azure Key Vault](../key-vault/key-vault-whatis.md). Haalt u de waarde door te verwijzen naar de sleutelkluis en geheim in de parameter-bestand. De waarde is nooit beschikbaar gemaakt, omdat u alleen verwijzen naar de sleutelkluis-ID. De key vault kan bestaan in een ander abonnement dan de resourcegroep die u implementeert.

## <a name="enable-access-to-the-secret"></a>Toegang tot het geheim inschakelen

Er zijn twee belangrijke voorwaarden voor toegang tot een key vault tijdens de sjabloonimplementatie van de moeten bestaan:

1. De eigenschap sleutelkluis `enabledForTemplateDeployment` moet `true`.
2. De gebruiker voor het implementeren van de sjabloon moet toegang hebben tot de geheime sleutel. De gebruiker moet beschikken over de `Microsoft.KeyVault/vaults/deploy/action` machtiging voor de key vault. De [eigenaar](../role-based-access-control/built-in-roles.md#owner) en [Inzender](../role-based-access-control/built-in-roles.md#contributor) beide rollen deze toegang verlenen.

Wanneer u een Key Vault met de sjabloon voor een [beheerde toepassingen](../managed-applications/overview.md), u moet toegang verlenen tot de **toestel Resource Provider** service-principal. Zie voor meer informatie, [toegang tot Key Vault-geheim bij het implementeren van Azure Managed Applications](../managed-applications/key-vault-access.md).


## <a name="deploy-a-key-vault-and-secret"></a>Een sleutelkluis en een geheim implementeren

Voor het maken van een sleutelkluis en een geheim, gebruikt u Azure CLI of PowerShell. U ziet dat de key vault is ingeschakeld voor sjabloonimplementatie. 

Gebruik voor Azure CLI:

```azurecli-interactive
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create \
  --name $vaultname \
  --resource-group examplegroup \
  --location 'South Central US' \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Gebruik voor PowerShell:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault `
  -VaultName $vaultname `
  -ResourceGroupName examplegroup `
  -Location "South Central US" `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="reference-a-secret-with-static-id"></a>Verwijzen naar een geheim met de statische-ID

De sjabloon die u een key vault-geheim ontvangt is vergelijkbaar met een andere sjabloon. Dat komt doordat **verwijst u naar de key vault in het parameterbestand, niet de sjabloon.** De volgende afbeelding ziet u hoe de parameterbestand verwijst naar het geheim en wordt die waarde doorgegeven aan de sjabloon.

![Statische-ID](./media/resource-manager-keyvault-parameter/statickeyvault.png)

Bijvoorbeeld, de [na sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) implementeert u een SQL-database met een administrator-wachtwoord. De parameter password is ingesteld op een beveiligde tekenreeks. Maar de sjabloon geeft geen waarop deze waarde is gebaseerd.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Maak nu een parameterbestand voor de voorgaande sjabloon. Geef een parameter die overeenkomt met de naam van de parameter in de sjabloon in het parameterbestand. De waarde van parameter verwijst naar het geheim uit key vault. Het geheim verwijst u naar door door te geven van de resource-id van de key vault en de naam van de geheime sleutel. In de [volgende parameterbestand](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.parameters.json), de key vault-geheim moet al bestaan en u een statische waarde opgeven voor de resource-ID. Kopieer dit bestand lokaal op en stel de abonnements-ID, de naam van de kluis en de naam van SQL server.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "examplesecret"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Als u moet een versie van het geheim dan de huidige versie te gebruiken, gebruikt de `secretVersion` eigenschap.

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Nu de sjabloon implementeren en in het parameterbestand doorgeven. U kunt de voorbeeldsjabloon gebruiken, maar moet u een lokale parameterbestand met de waarden die zijn ingesteld op uw omgeving.

Gebruik voor Azure CLI:

```azurecli-interactive
az group create --name datagroup --location "South Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "South Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Verwijzen naar een geheim met de dynamische-ID

De vorige sectie hebt u geleerd hoe u een statische resource-ID voor de key vault-geheim doorgeven. In sommige scenario's moet u echter om te verwijzen naar een key vault-geheim varieert op basis van de huidige implementatie. In dit geval niet programmeren de resource-ID in het parameterbestand. Helaas, u kan niet dynamisch genereren de resource-ID in het parameterbestand Sjabloonexpressies zijn niet toegestaan in het parameterbestand.

Voor het dynamisch genereren van de resource-ID voor een key vault-geheim, moet u de resource die de geheime sleutel in een gekoppelde sjabloon moet verplaatsen. In de bovenliggende sjabloon u de gekoppelde sjabloon toevoegen en doorgeven in een parameter die de dynamisch gegenereerde resource-id bevat. De volgende afbeelding ziet u hoe een parameter in de gekoppelde sjabloon verwijst naar de geheime sleutel.

![Dynamische-ID](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

De gekoppelde sjabloon moet beschikbaar zijn via een externe URI zijn. Normaal gesproken u uw sjabloon toevoegen aan een opslagaccount en toegang te verkrijgen via de URI zoals `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json`.

De [na sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json) dynamisch wordt gemaakt van de key vault-ID en wordt doorgegeven als parameter. Het is gekoppeld aan een [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) in GitHub.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "vaultResourceGroup": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      },
      "adminLogin": {
        "type": "string"
      },
      "sqlServerName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(subscription().subscriptionId,  parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          },
          "adminLogin": { "value": "[parameters('adminLogin')]" },
          "sqlServerName": {"value": "[parameters('sqlServerName')]"}
        }
      }
    }],
    "outputs": {}
}
```

De voorgaande sjabloon implementeren, en geef waarden voor de parameters. Kunt u de voorbeeldsjabloon, maar u moet de parameterwaarden opgeven voor uw omgeving.

Gebruik voor Azure CLI:

```azurecli-interactive
az group create --name datagroup --location "South Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json \
    --parameters vaultName=<your-vault> vaultResourceGroup=examplegroup secretName=examplesecret adminLogin=exampleadmin sqlServerName=<server-name>
```

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "South Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json `
  -vaultName <your-vault> -vaultResourceGroup examplegroup -secretName examplesecret -adminLogin exampleadmin -sqlServerName <server-name>
```

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor algemene informatie over sleutelkluizen [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md).
* Zie voor volledige voorbeelden van het verwijst naar een sleutel geheimen [Key Vault voorbeelden](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
