---
title: Gebruik Azure beheerde Service-identiteit in Azure API Management | Microsoft Docs
description: Informatie over het gebruik van Azure beheerde Service-identiteit in API Management
services: api-management
documentationcenter: ''
author: miaojiang
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: 38c54995c5db90df11e57181e21347bee43a439a
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445819"
---
# <a name="use-azure-managed-service-identity-in-azure-api-management"></a>Azure beheerde Service-identiteit gebruiken in Azure API Management

In dit artikel leest u over het maken van een beheerde service-identiteit voor een exemplaar van API Management-service en voor toegang tot andere resources. Een beheerde service-identiteit die is gegenereerd door Azure Active Directory (Azure AD) kunt uw exemplaar van API Management eenvoudig en veilig toegang krijgen tot andere Azure AD-beveiligde bronnen, zoals Azure Key Vault. Deze beheerde service-identiteit wordt beheerd door Azure en vereist niet dat u in te richten of er geheimen draaien. Zie voor meer informatie over Azure beheerde Service-identiteit, [beheerde Service-identiteit voor Azure-resources](../active-directory/msi-overview.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="create-a-managed-service-identity-for-an-api-management-instance"></a>Maken van een beheerde service-identiteit voor een exemplaar van API Management

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

Als u een beheerde service-identiteit in de portal instelt, wordt u maakt eerst een exemplaar van API Management als normale en schakel vervolgens de functie.

1. API Management-exemplaar maken in de portal zoals u gewend bent. Navigeren in de portal.
2. Selecteer **beheerde service-identiteit**.
3. Registreren bij Azure Active Directory overschakelen naar op. Klik op Opslaan.

![Inschakelen van MSI](./media/api-management-msi/enable-msi.png)

### <a name="using-the-azure-resource-manager-template"></a>Met behulp van de Azure Resource Manager-sjabloon

U kunt een exemplaar van API Management met een identiteit maken door de volgende eigenschap te nemen in de resourcedefinitie: 

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Hiermee wordt aangegeven voor Azure maken en beheren van de identiteit voor uw exemplaar van API Management. 

Bijvoorbeeld, uitzien een volledige Azure Resource Manager-sjabloon als volgt:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0"
    },
    "resources": [
        {
            "apiVersion": "2017-03-01",
            "name": "contoso",
            "type": "Microsoft.ApiManagement/service",
            "location": "[resourceGroup().location]",
            "tags": {},
            "sku": {
                "name": "Developer",
                "capacity": "1"
            },
            "properties": {
                "publisherEmail": "admin@contoso.com",
                "publisherName": "Contoso"
            },
            "identity": { 
                "type": "systemAssigned" 
            }
        }
    ]
}
```
## <a name="use-the-managed-service-identity-to-access-other-resources"></a>De beheerde service-identiteit gebruiken voor toegang tot andere resources

> [!NOTE]
> Beheerde service-identiteit kan op dit moment worden gebruikt om certificaten te verkrijgen uit Azure Key Vault voor aangepaste domeinnamen voor API Management. Meer scenario's zal binnenkort worden ondersteund.
> 
>


### <a name="obtain-a-certificate-from-azure-key-vault"></a>Een certificaat verkrijgen van Azure Key Vault

#### <a name="prerequisites"></a>Vereisten
1. De Key Vault met het pfx-certificaat moet zich in hetzelfde Azure-abonnement en dezelfde resourcegroep bevinden als de API Management-service. Dit is een vereiste van het Azure Resource Manager-sjabloon. 
2. Het inhoudstype van de geheime sleutel moet *application/x-pkcs12*. U kunt het volgende script gebruiken om het certificaat te uploaden:

```powershell
$pfxFilePath = "PFX_CERTIFICATE_FILE_PATH" # Change this path 
$pwd = "PFX_CERTIFICATE_PASSWORD" # Change this password 
$flag = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable 
$collection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection 
$collection.Import($pfxFilePath, $pwd, $flag) 
$pkcs12ContentType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12 
$clearBytes = $collection.Export($pkcs12ContentType) 
$fileContentEncoded = [System.Convert]::ToBase64String($clearBytes) 
$secret = ConvertTo-SecureString -String $fileContentEncoded -AsPlainText –Force 
$secretContentType = 'application/x-pkcs12' 
Set-AzureKeyVaultSecret -VaultName KEY_VAULT_NAME -Name KEY_VAULT_SECRET_NAME -SecretValue $Secret -ContentType $secretContentType
```

> [!Important]
> Als de Objectversie van het certificaat niet opgegeven is, wordt API Management de nieuwere versie van het certificaat automatisch verkrijgen nadat deze is geüpload naar Key Vault. 

Het volgende voorbeeld ziet u een Azure Resource Manager-sjabloon met de volgende stappen uit:

1. Maak een exemplaar van API Management met een beheerde service-identiteit.
2. Bijwerken van het toegangsbeleid van een Azure Key Vault-instantie en de API Management-exemplaar te verkrijgen geheimen toestaan.
3. De API Management-exemplaar bijwerken door in te stellen van een aangepaste domeinnaam via een certificaat van de Key Vault-instantie.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
            
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",        
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    { 
      "apiVersion": "2017-05-10", 
      "name": "apimWithKeyVault", 
      "type": "Microsoft.Resources/deployments",
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": {
            "publisherEmail": { "value": "[parameters('publisherEmail')]"},
            "publisherName": { "value": "[parameters('publisherName')]"},
            "sku": { "value": "[parameters('sku')]"},
            "skuCount": { "value": "[parameters('skuCount')]"},
            "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
            "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
        }
      } 
    }]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure Managed Service Identity:

* [Beheerde Service-identiteit voor Azure-resources](../active-directory/msi-overview.md)
* [Azure Resource Manager-sjablonen](https://github.com/Azure/azure-quickstart-templates)

