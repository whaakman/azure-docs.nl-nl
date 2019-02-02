---
title: Verwijzingen naar Key Vault - Azure App Service | Microsoft Docs
description: Conceptuele verwijzing en setup-handleiding voor Azure Key Vault-verwijzingen in Azure App Service en Azure Functions
services: app-service
author: mattchenderson
manager: jeconnoc
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/20/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 662260c3cf37f8f8a675c522f3d3dea41153e485
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663560"
---
# <a name="use-key-vault-references-for-app-service-and-azure-functions-preview"></a>Verwijzingen van de Key Vault gebruiken voor App Service en Azure Functions (preview)

> [!NOTE] 
> Key Vault verwijzingen zijn momenteel in preview.

Dit onderwerp ziet u hoe u werkt met geheimen in Azure Key Vault in uw App Service of Azure Functions-toepassing zonder codewijzigingen. [Azure Key Vault](../key-vault/key-vault-overview.md) is een service die voorziet in gecentraliseerde geheimen, met volledig beheer over de geschiedenis voor het beleid en de controle van toegang.

## <a name="granting-your-app-access-to-key-vault"></a>Uw apptoegang verlenen tot Key Vault

Om te kunnen lezen geheimen uit Key Vault, moet u een kluis hebt gemaakt en uw app machtigt om deze te openen.

1. Een sleutelkluis maken door de volgende de [Key Vault-snelstartgids](../key-vault/quick-create-cli.md).

1. Maak een [systeem toegewezen identiteit beheerd](overview-managed-identity.md) voor uw toepassing.

   > [!NOTE] 
   > Key Vault verwijst momenteel naar beheerde identiteiten alleen ondersteuning door het systeem toegewezen. Gebruiker toegewezen identiteiten kunnen niet worden gebruikt.

1. Maak een [toegangsbeleid in Key Vault](../key-vault/key-vault-secure-your-key-vault.md#key-vault-access-policies) voor de toepassingsidentiteit die u eerder hebt gemaakt. De geheime 'Get'-machtiging voor dit beleid inschakelen. De 'geautoriseerd toepassing' niet configureert of `appliationId` instellingen, als dit is niet compatibel met een beheerde identiteit.

## <a name="reference-syntax"></a>De syntaxis

De verwijzing naar een Key Vault heeft de vorm `@Microsoft.KeyVault({referenceString})`, waarbij `{referenceString}` wordt vervangen door een van de volgende opties:

> [!div class="mx-tdBreakAll"]
> | Referentie-tekenreeks                                                            | Description                                                                                                                                                                                 |
> |-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | SecretUri=_secretUri_                                                       | De **SecretUri** moet de volledige data plane-URI van een geheim in Key Vault, met inbegrip van een versie, bijvoorbeeld https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931  |
> | VaultName =_vaultName_; SecretName =_secretName_; SecretVersion =_secretVersion_ | De **VaultName** moet de naam van uw Key Vault-resource. De **SecretName** moet de naam van het geheim van het doel. De **SecretVersion** moet de versie van het geheim te gebruiken. |

> [!NOTE] 
> In de huidige preview zijn-versies vereist. Wanneer u geheimen, moet u de versie in de configuratie van uw toepassing bij te werken.

Bijvoorbeeld, een volledig overzicht zou de volgende notatie:

```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931)
```

U kunt ook:

```
@Microsoft.KeyVault(VaultName=myvault;SecretName=mysecret;SecretVersion=ec96f02080254f109c51a1f14cdb1931)
```


## <a name="source-application-settings-from-key-vault"></a>Instellingen voor gegevensbron toepassing vanuit Key Vault

Verwijzingen naar de Key Vault kunnen worden gebruikt als waarden voor [toepassingsinstellingen](web-sites-configure.md#app-settings), zodat u kunt het bewaren van geheimen in Key Vault in plaats van de site-configuratie. Toepassingsinstellingen veilig in rust worden versleuteld, maar als u geheime beheermogelijkheden moet, moeten ze worden geplaatst in Key Vault.

Voor het gebruik van een Key Vault-verwijzing voor de toepassingsinstelling van een, moet u de verwijzing instellen als de waarde van de instelling. Uw app kunt verwijzen naar het geheim via de sleutel als normaal. Er zijn geen codewijzigingen vereist.

> [!TIP]
> De meeste toepassingsinstellingen in met Key Vault-verwijzingen moeten worden gemarkeerd als site-instellingen, als u moet beschikken over afzonderlijke kluizen voor elke omgeving.

### <a name="azure-resource-manager-deployment"></a>Implementatie van Azure Resource Manager

Bij het automatiseren van implementaties van resource via Azure Resource Manager-sjablonen, moet u mogelijk uw afhankelijkheden in een bepaalde volgorde om deze functie werkt sequentiëren. Van de opmerking, hoeft u uw om toepassingsinstellingen te definiëren als hun eigen resource, in plaats van een `siteConfig` eigenschap in het sitedefinitie van de. Dit is omdat de site eerst worden gedefinieerd moet, zodat het systeem toegewezen identiteit met deze is gemaakt en kan worden gebruikt in het toegangsbeleid.

Een voorbeeld-psuedo-sjabloon voor een functie-app kan er als volgt uitzien:

```json
{
    //...
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            //...
        },
        {
            "type": "Microsoft.Insights/components",
            "name": "[variables('appInsightsName')]",
            //...
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[variables('functionAppName')]",
            "identity": {
                "type": "SystemAssigned"
            },
            //...
            "resources": [
                {
                    "type": "config",
                    "name": "appsettings",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('storageConnectionStringName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('appInsightsKeyName'))]"
                    ],
                    "properties": {
                        "AzureWebJobsStorage": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "APPINSIGHTS_INSTRUMENTATIONKEY": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('appInsightsKeyResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_ENABLE_SYNC_UPDATE_SITE": "true"
                        //...
                    }
                },
                {
                    "type": "sourcecontrols",
                    "name": "web",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.Web/sites/config', variables('functionAppName'), 'appsettings')]"
                    ],
                }
            ]
        },
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[variables('keyVaultName')]",
            //...
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]"
            ],
            "properties": {
                //...
                "accessPolicies": [
                    {
                        "tenantId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').tenantId]",
                        "objectId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').principalId]",
                        "permissions": {
                            "secrets": [ "get" ]
                        }
                    }
                ]
            },
            "resources": [
                {
                    "type": "secrets",
                    "name": "[variables('storageConnectionStringName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
                    ],
                    "properties": {
                        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountResourceId'),'2015-05-01-preview').key1)]"
                    }
                },
                {
                    "type": "secrets",
                    "name": "[variables('appInsightsKeyName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
                    ],
                    "properties": {
                        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
                    }
                }
            ]
        }
    ]
}
```

> [!NOTE] 
> In dit voorbeeld is de implementatie van bron afhankelijk van instellingen voor de toepassing. Dit is normaal gesproken onveilige gedrag, zoals het bijwerken van de app-instelling asynchroon zich gedraagt. Echter, omdat we hebben opgenomen de `WEBSITE_ENABLE_SYNC_UPDATE_SITE` toepassingsinstelling, de update is synchroon. Dit betekent dat de implementatie van bron alleen wordt gestart zodra de toepassingsinstellingen volledig zijn bijgewerkt.
