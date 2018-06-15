---
title: Gebruik Azure beheerde Service-identiteit in Azure API Management | Microsoft Docs
description: Informatie over het gebruik van Azure Managed Service-identiteit in API Management
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
ms.openlocfilehash: 98aa70935a3efbbe2edb07aade85fa3ea17ce786
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32150427"
---
# <a name="use-azure-managed-service-identity-in-azure-api-management"></a>Azure beheerde Service-identiteit gebruiken in Azure API Management

In dit artikel leest u de identiteit van een beheerde service voor een service-exemplaar van API Management maken en toegang tot andere bronnen. Een beheerde service-identiteit die is gegenereerd door Azure Active Directory (Azure AD) kunt uw exemplaar van API Management eenvoudig en veilig toegang krijgen tot andere Azure AD-beveiligde bronnen, zoals Azure Sleutelkluis. Deze beheerde service-identiteit wordt beheerd door Azure en vereist niet dat u in te richten of geen geheimen draaien. Zie voor meer informatie over Azure Managed Service-identiteit [Service-identiteit voor Azure-resources beheerd](../active-directory/msi-overview.md).

## <a name="create-a-managed-service-identity-for-an-api-management-instance"></a>De identiteit van een beheerde service voor een exemplaar van API Management maken

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

Als u een beheerde service-identiteit in de portal instelt, wordt u eerst maken als normaal exemplaar van API Management en schakel vervolgens de functie.

1. Exemplaar van API Management maken in de portal, zoals u dat gewend bent. Ga naar het in de portal.
2. Selecteer **beheerde service-identiteit**.
3. Activeer registreren met Azure Active Directory op. Klik op opslaan.

![MSI inschakelen](./media/api-management-msi/enable-msi.png)

### <a name="using-the-azure-resource-manager-template"></a>Met behulp van de Azure Resource Manager-sjabloon

U kunt een exemplaar van API Management met een identiteit maken door de volgende eigenschap in de resourcedefinitie, waaronder: 

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Dit vertelt Azure maken en beheren van de identiteit voor uw exemplaar van API Management. 

Een volledige Azure Resource Manager-sjabloon kan er bijvoorbeeld als volgt uitzien:

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
> Beheerde service-identiteit kan op dit moment worden gebruikt om certificaten te verkrijgen van Azure Sleutelkluis voor aangepaste domeinnamen van API Management. Meer scenario's zal binnenkort worden ondersteund.
> 
>


### <a name="obtain-a-certificate-from-azure-key-vault"></a>Een certificaat verkrijgen van Azure Sleutelkluis

#### <a name="prerequisites"></a>Vereisten
1. De Sleutelkluis met het pfx-certificaat moet zich in dezelfde Azure-abonnement en dezelfde resourcegroep bevinden als de API Management-service. Dit is een vereiste van de Azure Resource Manager-sjabloon. 
2. Het inhoudstype van het geheim moet *x-toepassing-pkcs12*. U kunt het volgende script gebruiken om het certificaat te uploaden:

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
> Als de Objectversie van het certificaat niet is opgegeven, wordt API Management automatisch de nieuwere versie van het certificaat verkrijgen nadat deze is geüpload naar Sleutelkluis. 

Het volgende voorbeeld ziet u een Azure Resource Manager-sjabloon met de volgende stappen uit:

1. Exemplaar van API Management maken met een beheerde service-identiteit.
2. Werk het toegangsbeleid van een exemplaar van Azure Sleutelkluis en toestaan dat het exemplaar van API Management geheimen verkrijgen.
3. Werk het exemplaar van API Management door in te stellen van een aangepaste domeinnaam via een certificaat van de Sleutelkluis-exemplaar.

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

Meer informatie over Azure Managed Service-identiteit:

* [Beheerde Service-identiteit voor Azure-resources](../active-directory/msi-overview.md)
* [Azure Resource Manager-sjablonen](https://github.com/Azure/azure-quickstart-templates)

