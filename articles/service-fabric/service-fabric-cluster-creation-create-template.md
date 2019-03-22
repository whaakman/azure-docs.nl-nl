---
title: Een sjabloon voor Azure Service Fabric-cluster maken | Microsoft Docs
description: Informatie over het maken van een Resource Manager-sjabloon voor een Service Fabric-cluster. Configureer beveiliging, Azure Key Vault en Azure Active Directory (Azure AD) voor clientauthenticatie.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/16/2018
ms.author: aljo
ms.openlocfilehash: 8bfd6bd7e42d5978f6db2c2a6bdb634661c6bfed
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886742"
---
# <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Een Service Fabric-cluster Resource Manager-sjabloon maken

Een [Azure Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een netwerk verbonden reeks virtuele machines waarop uw microservices worden geïmplementeerd en beheerd. Een Service Fabric-cluster worden uitgevoerd in Azure is een Azure-resource en is geïmplementeerd, beheerd en bewaakt met behulp van de Resource Manager.  Dit artikel wordt beschreven hoe een Resource Manager-sjabloon voor een Service Fabric-cluster worden uitgevoerd in Azure maken.  Als u de sjabloon is voltooid, kunt u [implementeren van het cluster op Azure](service-fabric-cluster-creation-via-arm.md).

Clusterbeveiliging is geconfigureerd als een cluster eerst is ingesteld en kan later worden gewijzigd. Lees voordat u een cluster instelt, [Service Fabric-clusterbeveiligingsscenario's][service-fabric-cluster-security]. In Azure, Service Fabric gebruikt x509-certificaat voor het beveiligen van uw cluster en de eindpunten, verifiëren van clients en -gegevens versleutelen. Azure Active Directory wordt ook aanbevolen voor beveiligde toegang tot eindpunten voor beheer. Azure AD-tenants en gebruikers moeten worden gemaakt voordat het cluster te maken.  Lees voor meer informatie, [instellen van Azure AD om clients te verifiëren](service-fabric-cluster-creation-setup-aad.md).

Voordat u een productiecluster voor het uitvoeren van productieworkloads implementeert, moet u eerst lezen de [productie gereedheid controlelijst](service-fabric-production-readiness-checklist.md).

## <a name="create-the-resource-manager-template"></a>Het Resource Manager-sjabloon maken
Voorbeeld van Resource Manager-sjablonen zijn beschikbaar in de [Azure-voorbeelden op GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Deze sjablonen kunnen worden gebruikt als uitgangspunt voor de clustersjabloon voor het.

In dit artikel wordt de [beveiligde cluster met vijf knooppunten] [ service-fabric-secure-cluster-5-node-1-nodetype] van de voorbeeldsjabloon en sjabloonparameters. Download *azuredeploy.json* en *azuredeploy.parameters.json* op uw computer en opent u beide bestanden in uw favoriete teksteditor.

> [!NOTE]
> Voor nationale clouds (Azure Government, China, Azure, Azure Duitsland), moet u ook het volgende toevoegen `fabricSettings` aan uw sjabloon: `AADLoginEndpoint`, `AADTokenEndpointFormat` en `AADCertEndpointFormat`.

## <a name="add-certificates"></a>Certificaten toevoegen
U kunt certificaten toevoegen aan een cluster Resource Manager-sjabloon door te verwijzen naar de sleutelkluis waarin de certificaatsleutels. Deze sleutelkluis parameters en waarden toevoegen in een bestand met Sjabloonparameters Resource Manager (*azuredeploy.parameters.json*).

### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Alle certificaten toevoegen aan de virtuele machine scale set osProfile
Elk certificaat dat geïnstalleerd in het cluster moet worden geconfigureerd in de **osProfile** schaalsetresource sectie van de schaal (Microsoft.Compute/virtualMachineScaleSets). Deze actie geeft de resourceprovider voor het installeren van het certificaat op de virtuele machines. Deze installatie bevat zowel het clustercertificaat en application security certificaten die u wilt gebruiken voor uw toepassingen:

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

### <a name="configure-the-service-fabric-cluster-certificate"></a>Het Service Fabric-cluster-certificaat configureren

Het verificatiecertificaat van het cluster moet worden geconfigureerd in zowel de Service Fabric-clusterresource (Microsoft.ServiceFabric/clusters) en de Service Fabric-extensie voor virtuele-machineschaalset wordt ingesteld in de virtual machine scale set-resource. Deze benadering kunt de Service Fabric-resourceprovider te configureren voor gebruik voor cluster-verificatie en -serververificatie voor eindpunten voor beheer.

#### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>De gegevens van het certificaat de virtuele-machineschaalset ingesteld resource toevoegen

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "commonNames": ["[parameters('certificateCommonName')]"],
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

#### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Informatie over het certificaat toevoegen aan de Service Fabric-cluster-bron

```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
    ...
  }
}
```

## <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure Active Directory-configuratie toevoegen voor het gebruik van Azure Active Directory voor clienttoegang

U kunt de Azure AD-configuratie toevoegen aan cluster Resource Manager-sjabloon door te verwijzen naar de sleutelkluis waarin de certificaatsleutels. Toevoegen van deze Azure AD-parameters en waarden in een bestand met Sjabloonparameters Resource Manager (*azuredeploy.parameters.json*). 

> [!NOTE]
> Azure AD-tenants en gebruikers moeten worden gemaakt voordat het cluster te maken.  Lees voor meer informatie, [instellen van Azure AD om clients te verifiëren](service-fabric-cluster-creation-setup-aad.md).

```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

## <a name="populate-the-parameter-file-with-the-values"></a>Vul in het parameterbestand met de waarden

Gebruik tot slot de uitvoerwaarden van de van de key vault en Azure AD PowerShell-opdrachten voor het vullen van het parameterbestand.

Als u van plan bent de Azure service fabric DB PowerShell-modules gebruiken, klikt u vervolgens hoeft niet u voor het vullen van de gegevens van het cluster. Als u wilt dat het systeem voor het genereren van de eigen ondertekend certificaat voor clusterbeveiliging, houd ze net als null. 

> [!NOTE]
> Voor de RM-modules te halen en deze leeg parameterwaarden te vullen, de namen van parameters veel overeenkomen met de namen hieronder

```json
"clusterCertificateThumbprint": {
    "value": ""
},
"certificateCommonName": {
    "value": ""
},
"clusterCertificateUrlValue": {
    "value": ""
},
"sourceVaultvalue": {
    "value": ""
},
```

Als u van certificaten van de toepassing gebruikmaakt of een bestaand cluster dat u hebt geüpload naar de key vault gebruikt, moet u deze gegevens te halen en deze vullen.

De RM-modules hebt niet de mogelijkheid voor het genereren van de configuratie van de Azure AD bij u past, dus als u van plan bent de Azure AD gebruiken voor toegang van clients, moet u deze vullen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```

## <a name="test-your-template"></a>De sjabloon voor het testen
Gebruik de volgende PowerShell-opdracht voor het testen van uw Resource Manager-sjabloon met een parameterbestand:

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Als u problemen ondervindt en cryptisch berichten ophalen, gebruikt u '-fouten opsporen in ' als een optie.

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Het volgende diagram illustreert waar uw key vault en Azure AD-configuratie in de Resource Manager-sjabloon aanpassen.

![Kaart van de Resource Manager-afhankelijkheden][cluster-security-arm-dependency-map]

## <a name="next-steps"></a>Volgende stappen
Nu u een sjabloon voor uw cluster hebt, kunt u leren hoe u [het cluster implementeren in Azure](service-fabric-cluster-creation-via-arm.md).  Als u niet hebt gedaan, leest de [productie gereedheid controlelijst](service-fabric-production-readiness-checklist.md) voordat u implementeert een productiecluster.

Voor meer informatie over de JSON-syntaxis en de eigenschappen voor de resources die zijn geïmplementeerd in dit artikel, Zie:

* [Microsoft.ServiceFabric/clusters](/azure/templates/microsoft.servicefabric/clusters)
* [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers)
* [Microsoft.Compute/virtualMachineScaleSets](/azure/templates/microsoft.compute/virtualmachinescalesets)

<!-- Links -->
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-create-template/cluster-security-arm-dependency-map.png
