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
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: a885fda23bb76091705ebe388f40a6eae7b56416
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54351506"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Azure Key Vault gebruiken voor veilige parameterwaarde doorgeven tijdens implementatie

Als u een veilige waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie wilt, haalt u de waarde van een [Azure Key Vault](../key-vault/key-vault-whatis.md). Haalt u de waarde door te verwijzen naar de sleutelkluis en geheim in de parameter-bestand. De waarde is nooit beschikbaar gemaakt, omdat u alleen verwijzen naar de sleutelkluis-ID. De key vault kan bestaan in een ander abonnement dan de resourcegroep die u implementeert.

## <a name="deploy-a-key-vault-and-secret"></a>Een sleutelkluis en een geheim implementeren

Voor het maken van een sleutelkluis en een geheim, gebruikt u Azure CLI of PowerShell. `enabledForTemplateDeployment` is een eigenschap van de sleutelkluis. Toegang tot de geheimen in deze Key Vault van Resource Manager-implementatie, `enabledForTemplateDeployment` moet `true`. 

De volgende Azure PowerShell en Azure CLI-voorbeeldscript laat zien hoe u een Key Vault en een geheim maken.

Gebruik voor Azure CLI:

```azurecli-interactive
keyVaultName='{your-unique-vault-name}'
resourceGroupName='{your-resource-group-name}'
location='centralus'
userPrincipalName='{your-email-address-associated-with-your-subscription}'

# Create a resource group
az group create --name $resourceGroupName --location $location

# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

# Create a secret with the name, vmAdminPassword
password=$(openssl rand -base64 32)
echo $password
az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
```

Gebruik voor PowerShell:

```azurepowershell-interactive
$keyVaultName = "{your-unique-vault-name}"
$resourceGroupName="{your-resource-group-name}"
$location='Central US'
$userPrincipalName='{your-email-address-associated-with-your-subscription}'

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

New-AzureRmKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list

$password = openssl rand -base64 32
echo $password
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretvalue
```

Als de PowerShell-script buiten de Cloud Shell is uitgevoerd, gebruikt u de volgende opdracht uit in plaats daarvan wachtwoord worden gegenereerd:

```powershell
Add-Type -AssemblyName System.Web
[System.Web.Security.Membership]::GeneratePassword(16,3)
```

Voor het gebruik van Resource Manager-sjabloon: Zie [zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie](./resource-manager-tutorial-use-key-vault.md#prepare-the-key-vault) voor meer informatie.

> [!NOTE]
> Elke Azure-service heeft eigen wachtwoordvereisten. Bijvoorbeeld, de vereisten van de Azure virtuele machine kunnen worden gevonden op [wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="enable-access-to-the-secret"></a>Toegang tot het geheim inschakelen

Anders dan instellen `enabledForTemplateDeployment` naar `true`, het implementeren van de sjabloon gebruikers beschikken over de `Microsoft.KeyVault/vaults/deploy/action` machtiging voor het bereik waarin de Key Vault met inbegrip van de resourcegroep en de Key Vault. De [eigenaar](../role-based-access-control/built-in-roles.md#owner) en [Inzender](../role-based-access-control/built-in-roles.md#contributor) beide rollen deze toegang verlenen. Als u de Sleutelkluis maakt, bent u de eigenaar, zodat u de machtiging hebt. Als de Key Vault met een ander abonnement is, moet de eigenaar van de Key Vault de toegang te verlenen.

De volgende procedure beschrijft hoe u een gebruikersrol maakt met de minimale machtigingen en de gebruiker toewijzen
1. Maak een aangepaste rol definitie JSON-bestand:

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    "00000000-0000-0000-0000-000000000000" vervangen door de abonnements-ID van de gebruiker die nodig zijn om de sjablonen te implementeren.

2. De nieuwe rol met behulp van het JSON-bestand maken:

    ```azurepowershell
    $resourceGroupName= "<Resource Group Name>" # the resource group which contains the Key Vault
    $userPrincipalName = "<Email Address of the deployment operator>"
    New-AzureRmRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzureRmRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    De `New-AzureRmRoleAssignment` voorbeeld van de aangepaste rol toewijzen aan de gebruiker op het niveau van de resourcegroep.  

Wanneer u een Key Vault met de sjabloon voor een [beheerde toepassingen](../managed-applications/overview.md), u moet toegang verlenen tot de **toestel Resource Provider** service-principal. Zie voor meer informatie, [toegang tot Key Vault-geheim bij het implementeren van Azure Managed Applications](../managed-applications/key-vault-access.md).

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
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Gebruik voor PowerShell:

```powershell-interactive
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Verwijzen naar een geheim met de dynamische-ID

De vorige sectie hebt u geleerd hoe u een statische resource-ID voor de key vault-geheim van de parameter doorgegeven. In sommige scenario's moet u echter om te verwijzen naar een key vault-geheim varieert op basis van de huidige implementatie. Of u kunt gewoon parameterwaarden doorgeven aan de sjabloon in plaats van een referentieparameter maken in het parameterbestand. In beide gevallen kunt u de resource-ID voor een key vault-geheim dynamisch genereren met behulp van een gekoppelde sjabloon.

U kan niet dynamisch genereren de resource-ID in het parameterbestand Sjabloonexpressies zijn niet toegestaan in het parameterbestand. 

In de bovenliggende sjabloon u de gekoppelde sjabloon toevoegen en doorgeven in een parameter die de dynamisch gegenereerde resource-id bevat. De volgende afbeelding ziet u hoe een parameter in de gekoppelde sjabloon verwijst naar de geheime sleutel.

![Dynamische-ID](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

De [na sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-key-vault-use-dynamic-id) dynamisch wordt gemaakt van de key vault-ID en wordt doorgegeven als parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location where the resources will be deployed."
            }
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the keyvault that contains the secret."
            }
        },
        "secretName": {
            "type": "string",
            "metadata": {
                "description": "The name of the secret."
            }
        },
        "vaultResourceGroupName": {
            "type": "string",
            "metadata": {
                "description": "The name of the resource group that contains the keyvault."
            }
        },
        "vaultSubscription": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "The name of the subscription that contains the keyvault."
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located. When the template is deployed using the accompanying scripts, a private location in the subscription will be used and this value will be automatically generated."
            },
            "defaultValue": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/"
        },
        "_artifactsLocationSasToken": {
            "type": "securestring",
            "metadata": {
                "description": "The sasToken required to access _artifactsLocation.  When the template is deployed using the accompanying scripts, a sasToken will be automatically generated."
            },
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "dynamicSecret",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "contentVersion": "1.0.0.0",
                    "uri": "[uri(parameters('_artifactsLocation'), concat('./nested/sqlserver.json', parameters('_artifactsLocationSasToken')))]"
                },
                "parameters": {
                    "location": {
                        "value": "[parameters('location')]"
                    },
                    "adminLogin": {
                        "value": "ghuser"
                    },
                    "adminPassword": {
                        "reference": {
                            "keyVault": {
                                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                            },
                            "secretName": "[parameters('secretName')]"
                        }
                    }
                }
            }
        }
    ],
    "outputs": {
        "sqlFQDN": {
            "type": "string",
            "value": "[reference('dynamicSecret').outputs.sqlFQDN.value]"
        }
    }
}
```

De voorgaande sjabloon implementeren, en geef waarden voor de parameters. Kunt u de voorbeeldsjabloon, maar u moet de parameterwaarden opgeven voor uw omgeving.

Gebruik voor Azure CLI:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=<your-vault> vaultResourceGroupName=examplegroup secretName=examplesecret
```

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName <your-vault> -vaultResourceGroupName examplegroup -secretName examplesecret
```

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor algemene informatie over sleutelkluizen [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md).
* Zie voor volledige voorbeelden van het verwijst naar een sleutel geheimen [Key Vault voorbeelden](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
