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
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 93b92a8a3b8aacd1f665725643314858fe92ad3c
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233765"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Azure Key Vault gebruiken voor veilige parameterwaarde doorgeven tijdens implementatie

In plaats van hiervoor een beveiligde waarde (zoals een wachtwoord) rechtstreeks in uw parameterbestand, haalt u de waarde van een [Azure Key Vault](../key-vault/key-vault-whatis.md) tijdens een implementatie. Haalt u de waarde door te verwijzen naar de sleutelkluis en geheim in de parameter-bestand. De waarde zelf wordt nooit getoond, omdat u alleen verwijst naar de sleutelkluis-id. De key vault kan bestaan in een ander abonnement dan de resourcegroep die u implementeert.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-key-vaults-and-secrets"></a>Sleutelkluizen en geheimen implementeren

Het maken van Sleutelkluizen en geheimen toevoegen, Zie:

- [Instellen en ophalen van een geheim met behulp van CLI](../key-vault/quick-create-cli.md)
- [Instellen en ophalen van een geheim met behulp van Powershell](../key-vault/quick-create-powershell.md)
- [Instellen en ophalen van een geheim met behulp van de portal](../key-vault/quick-create-portal.md)
- [Instellen en ophalen van een geheim met behulp van .NET](../key-vault/quick-create-net.md)
- [Instellen en ophalen van een geheim met behulp van Node.js](../key-vault/quick-create-node.md)

Er zijn enkele aanvullende overwegingen en vereisten bij het integreren van Key Vault met de sjabloonimplementatie van Resource Manager:

- `enabledForTemplateDeployment` is een eigenschap van de sleutelkluis. Toegang tot de geheimen in deze Key Vault van Resource Manager-implementatie, `enabledForTemplateDeployment` moet `true`. 
- Als u niet de eigenaar van de key vault, moet de eigenaar van de instellingen voor beveiligingsbeleid voor u om toe te voegen geheimen sleutelkluis bijwerken.

De volgende Azure CLI en Azure PowerShell-voorbeelden laten zien hoe dit werkt:

```azurecli
# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list
```

```azurepowershell
New-AzKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
```

## <a name="grant-access-to-the-secrets"></a>Toegang verlenen tot de geheimen

Er moet de gebruiker de sjabloon implementeert de `Microsoft.KeyVault/vaults/deploy/action` machtiging voor het bereik waarin de Key Vault met inbegrip van de resourcegroep en de Key Vault. De [eigenaar](../role-based-access-control/built-in-roles.md#owner) en [Inzender](../role-based-access-control/built-in-roles.md#contributor) beide rollen deze toegang verlenen. Als u de Sleutelkluis maakt, bent u de eigenaar, zodat u de machtiging hebt. Als de Key Vault met een ander abonnement is, moet de eigenaar van de Key Vault grand de toegang.

De volgende procedure laat zien over het maken van een rol met de minimale machtigingen en de gebruiker toewijzen

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
    New-AzRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    De `New-AzRoleAssignment` voorbeeld wijst u de aangepaste rol die aan de gebruiker op het niveau van de resourcegroep.  

Wanneer u een Key Vault met de sjabloon voor een [beheerde toepassingen](../managed-applications/overview.md), u moet toegang verlenen tot de **toestel Resource Provider** service-principal. Zie voor meer informatie, [toegang tot Key Vault-geheim bij het implementeren van Azure Managed Applications](../managed-applications/key-vault-access.md).

## <a name="reference-secrets-with-static-id"></a>Naslaginformatie over geheimen met statische-ID

Met deze methode, verwijst u naar de key vault in het parameterbestand, niet de sjabloon. De volgende afbeelding ziet u hoe de parameterbestand verwijst naar het geheim en wordt die waarde doorgegeven aan de sjabloon.

![Statische-ID een diagram van Resource Manager sleutelkluis-integratie](./media/resource-manager-keyvault-parameter/statickeyvault.png)

[Zelfstudie: Azure Key Vault integreren in de implementatie van Resource Manager-sjabloon](./resource-manager-tutorial-use-key-vault.md) maakt gebruik van deze methode. De zelfstudie implementeert een virtuele machine wat een administrator-wachtwoord bevat. De parameter password is ingesteld op een beveiligde tekenreeks:

![Resource Manager sleutelkluis-integratie-sjabloonbestand met statische-ID](./media/resource-manager-keyvault-parameter/resource-manager-key-vault-static-id-template-file.png)

Maak nu een parameterbestand voor de voorgaande sjabloon. Geef een parameter die overeenkomt met de naam van de parameter in de sjabloon in het parameterbestand. De waarde van parameter verwijst naar het geheim uit key vault. U verwijst naar het geheim door door te geven van de resource-id van de key vault en de naam van het geheim:

![Resource Manager sleutelkluis-integratie-parameterbestand met statische-ID](./media/resource-manager-keyvault-parameter/resource-manager-key-vault-static-id-parameter-file.png)

Als u moet een versie van het geheim dan de huidige versie te gebruiken, gebruikt de `secretVersion` eigenschap.

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

De sjabloon implementeren en door te geven in het parameterbestand:

Gebruik voor Azure CLI:

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri <The Template File URI> \
    --parameters <The Parameter File>
```

Gebruik voor PowerShell:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri <The Template File URI> `
  -TemplateParameterFile <The Parameter File>
```

## <a name="reference-secrets-with-dynamic-id"></a>Naslaginformatie over geheimen met dynamische-ID

De vorige sectie hebt u geleerd hoe u een statische resource-ID voor de key vault-geheim van de parameter doorgegeven. In sommige scenario's moet u echter om te verwijzen naar een key vault-geheim varieert op basis van de huidige implementatie. Of, kunt u parameterwaarden doorgeven aan de sjabloon in plaats van een referentieparameter maken in het parameterbestand. In beide gevallen kunt u de resource-ID voor een key vault-geheim dynamisch genereren met behulp van een gekoppelde sjabloon.

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

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=$keyVaultName vaultResourceGroupName=examplegroup secretName=examplesecret
```

Gebruik voor PowerShell:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName $keyVaultName -vaultResourceGroupName $keyVaultResourceGroupName -secretName $secretName
```

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg voor algemene informatie over sleutelkluizen [wat is Azure Key Vault?](../key-vault/key-vault-overview.md).
- Zie voor volledige voorbeelden van het verwijst naar een sleutel geheimen [Key Vault voorbeelden](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
