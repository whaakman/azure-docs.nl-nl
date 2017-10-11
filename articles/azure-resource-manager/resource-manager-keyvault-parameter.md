---
title: Sleutelkluis geheim met Resource Manager-sjabloon | Microsoft Docs
description: Laat zien hoe een geheim van een sleutelkluis als parameter doorgeven tijdens de implementatie.
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 1ca72599e67e79d42a3d430dbb13e89ea7265334
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a>De Sleutelkluis gebruiken voor veilige parameterwaarde worden doorgegeven tijdens de implementatie

Wanneer u een beveiligde waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie moet, kunt u de waarde van de ophalen een [Azure Key Vault](../key-vault/key-vault-whatis.md). De waarde wordt opgehaald door te verwijzen naar de sleutelkluis en geheim in de parameter-bestand. De waarde is nooit beschikbaar gemaakt, omdat u alleen verwijzen naar de sleutelkluis-ID. U hoeft niet de waarde handmatig invoeren voor het geheim telkens wanneer die u de resources implementeren. De sleutelkluis kan bestaan in een ander abonnement dan de resourcegroep die u implementeert. Wanneer u verwijst naar de sleutelkluis, omvatten u de abonnement-ID.

Wanneer u de sleutelkluis maakt, stelt de *enabledForTemplateDeployment* eigenschap *true*. Deze waarde instelt op true, toestaan u toegang van Resource Manager-sjablonen tijdens de implementatie.  

## <a name="deploy-a-key-vault-and-secret"></a>Een sleutelkluis en geheim implementeren

Maakt een sleutelkluis en een geheim, gebruikmaken van Azure CLI of PowerShell. U ziet dat de sleutelkluis is ingeschakeld voor de sjabloonimplementatie van de. 

Gebruik voor Azure CLI:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Gebruik voor PowerShell:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Toegang tot het geheim inschakelen

Of u een nieuwe sleutelkluis of een bestaande gebruikt, zorg ervoor dat de implementatie van de sjabloon gebruiker toegang het geheim tot. Het implementeren van een sjabloon die verwijst naar een geheim gebruiker moet beschikken over de `Microsoft.KeyVault/vaults/deploy/action` machtiging voor de sleutelkluis. De [eigenaar](../active-directory/role-based-access-built-in-roles.md#owner) en [Inzender](../active-directory/role-based-access-built-in-roles.md#contributor) beide rollen deze toegang verlenen. U kunt ook maken een [aangepaste rol](../active-directory/role-based-access-control-custom-roles.md) die deze machtiging verleent en de gebruiker toevoegen aan die rol. Zie voor meer informatie over het toevoegen van een gebruiker aan een rol [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Verwijst naar een geheim met statische-ID

De sjabloon die u een geheim sleutelkluis ontvangt is vergelijkbaar met een andere sjabloon. Dat komt doordat **u verwijzen naar de sleutelkluis in de parameter-bestand niet in de sjabloon.** Bijvoorbeeld, implementeert u de volgende sjabloon een SQL-database met een administrator-wachtwoord. De wachtwoordparameter is ingesteld op een veilige tekenreeks. Maar de sjabloon geeft geen waar die waarde worden opgehaald.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

Maak nu een parameterbestand voor de voorgaande sjabloon. Geef in het parameterbestand een parameter die overeenkomt met de naam van de parameter in de sjabloon. De waarde van parameter verwijst naar het geheim van de sleutelkluis. U verwijzen naar het geheim door het doorgeven van de resource-id van de sleutelkluis en de naam van het geheim. In het volgende voorbeeld wordt het geheim sleutelkluis moet al bestaan en u een statische waarde opgeven voor de bron-ID.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a>Verwijst naar een geheim met dynamische ID

De vorige sectie hebt u geleerd hoe u een statisch resource-ID voor de sleutelkluis-geheim doorgeven. In sommige gevallen moet u echter verwijzen naar een sleutelkluis geheim dat varieert op basis van de huidige implementatie. In dat geval kunt u niet vastleggen de bron-ID in het parameterbestand. Helaas kan niet u dynamisch genereren de resource-ID in het parameterbestand Sjabloonexpressies zijn niet toegestaan in het parameterbestand.

Als de resource-ID voor een geheim sleutelkluis dynamisch worden gegenereerd, moet u de resource die het geheim in een geneste sjabloon moet verplaatsen. In de sjabloon master u de geneste sjabloon toevoegen en geeft u in een parameter die de dynamisch gegenereerde resource-id bevat.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
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
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor algemene informatie over sleutelkluizen [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md).
* Zie voor volledige voorbeelden voor het verwijzen naar de sleutel geheimen [Sleutelkluis voorbeelden](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

