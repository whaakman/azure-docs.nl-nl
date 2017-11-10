---
title: Sleutelkluis geheim met Azure Resource Manager-sjabloon | Microsoft Docs
description: Laat zien hoe een geheim van een sleutelkluis als parameter doorgeven tijdens de implementatie.
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2017
ms.author: tomfitz
ms.openlocfilehash: e789a234979be877d990665902fd6219ae7ec40b
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Gebruik Azure Sleutelkluis beveiligde parameterwaarde worden doorgegeven tijdens de implementatie

Wanneer u een beveiligde waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie moet, kunt u de waarde van de ophalen een [Azure Key Vault](../key-vault/key-vault-whatis.md). De waarde wordt opgehaald door te verwijzen naar de sleutelkluis en geheim in de parameter-bestand. De waarde is nooit beschikbaar gemaakt, omdat u alleen verwijzen naar de sleutelkluis-ID. U hoeft niet de waarde handmatig invoeren voor het geheim telkens wanneer die u de resources implementeren. De sleutelkluis kan bestaan in een ander abonnement dan de resourcegroep die u implementeert. Wanneer u verwijst naar de sleutelkluis, omvatten u de abonnement-ID.

Wanneer u de sleutelkluis maakt, stelt de *enabledForTemplateDeployment* eigenschap *true*. Deze waarde instelt op true, toestaan u toegang van Resource Manager-sjablonen tijdens de implementatie.

## <a name="deploy-a-key-vault-and-secret"></a>Een sleutelkluis en geheim implementeren

Maakt een sleutelkluis en een geheim, gebruikmaken van Azure CLI of PowerShell. U ziet dat de sleutelkluis is ingeschakeld voor de sjabloonimplementatie van de. 

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

## <a name="enable-access-to-the-secret"></a>Toegang tot het geheim inschakelen

Of u een nieuwe sleutelkluis of een bestaande gebruikt, zorg ervoor dat de implementatie van de sjabloon gebruiker toegang het geheim tot. Het implementeren van een sjabloon die verwijst naar een geheim gebruiker moet beschikken over de `Microsoft.KeyVault/vaults/deploy/action` machtiging voor de sleutelkluis. De [eigenaar](../active-directory/role-based-access-built-in-roles.md#owner) en [Inzender](../active-directory/role-based-access-built-in-roles.md#contributor) beide rollen deze toegang verlenen.

## <a name="reference-a-secret-with-static-id"></a>Verwijst naar een geheim met statische-ID

De sjabloon die u een geheim sleutelkluis ontvangt is vergelijkbaar met een andere sjabloon. Dat komt doordat **u verwijzen naar de sleutelkluis in de parameter-bestand niet in de sjabloon.** Bijvoorbeeld, implementeert u de volgende sjabloon een SQL-database met een administrator-wachtwoord. De wachtwoordparameter is ingesteld op een veilige tekenreeks. Maar de sjabloon geeft geen waar die waarde worden opgehaald.

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

Maak nu een parameterbestand voor de voorgaande sjabloon. Geef in het parameterbestand een parameter die overeenkomt met de naam van de parameter in de sjabloon. De waarde van parameter verwijst naar het geheim van de sleutelkluis. U verwijzen naar het geheim door het doorgeven van de resource-id van de sleutelkluis en de naam van het geheim. In het volgende voorbeeld wordt het geheim sleutelkluis moet al bestaan en u een statische waarde opgeven voor de bron-ID.

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

Nu de sjabloon implementeert en in het parameterbestand. Gebruik voor Azure CLI:

```azurecli-interactive
az group create --name datagroup --location "Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-file sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateFile sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Verwijst naar een geheim met dynamische ID

De vorige sectie hebt u geleerd hoe u een statisch resource-ID voor de sleutelkluis-geheim doorgeven. In sommige gevallen moet u echter verwijzen naar een sleutelkluis geheim dat varieert op basis van de huidige implementatie. In dat geval kunt u niet vastleggen de bron-ID in het parameterbestand. Helaas kan niet u dynamisch genereren de resource-ID in het parameterbestand Sjabloonexpressies zijn niet toegestaan in het parameterbestand.

Als de resource-ID voor een geheim sleutelkluis dynamisch worden gegenereerd, moet u de resource die het geheim in een geneste sjabloon moet verplaatsen. In de sjabloon master u de geneste sjabloon toevoegen en geeft u in een parameter die de dynamisch gegenereerde resource-id bevat. De geneste sjabloon moet beschikbaar zijn via een externe URI zijn. De rest van dit artikel wordt ervan uitgegaan dat u de voorgaande sjabloon naar een opslagaccount hebt toegevoegd en is beschikbaar in de URI - `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json`.

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
          "uri": "https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json",
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

Implementeer de voorgaande sjabloon en geef waarden op voor de parameters.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor algemene informatie over sleutelkluizen [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md).
* Zie voor volledige voorbeelden voor het verwijzen naar de sleutel geheimen [Sleutelkluis voorbeelden](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
