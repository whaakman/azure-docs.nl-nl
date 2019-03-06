---
title: OpenShift Containerplatform in Azure implementeren | Microsoft Docs
description: OpenShift Containerplatform in Azure implementeren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/04/2018
ms.author: haroldw
ms.openlocfilehash: aaf8ea70254e0edb66c6b88e6303c91b26de2e9b
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453555"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>OpenShift Containerplatform in Azure implementeren

U kunt een van de verschillende methoden voor het implementeren van OpenShift Container Platform in Azure:

- U kunt handmatig implementeren van de onderdelen die nodig zijn Azure-infrastructuur en voer vervolgens de [documentatie voor OpenShift Container Platform](https://docs.openshift.com/container-platform).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-container-platform/) die vereenvoudigt de implementatie van het cluster OpenShift Container Platform.
- Een andere optie is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Een Red Hat-abonnement is vereist voor alle opties. De Red Hat Enterprise Linux-exemplaar is tijdens de implementatie aan het Red Hat-abonnement is geregistreerd en die is gekoppeld aan de groeps-ID die de rechten voor OpenShift Container Platform bevat.
Zorg ervoor dat u hebt een geldige gebruikersnaam, wachtwoord en groep-ID in Red Hat abonnement Manager (RHSM) U kunt een activeringscode, organisatie-ID en groep-ID. U kunt deze informatie controleren door aan te melden bij https://access.redhat.com.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Implementeren met behulp van de OpenShift Container Platform Resource Manager-sjabloon

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, kunt u een parameterbestand gebruiken om op te geven van de invoerparameters. Voor het verder aanpassen van de implementatie, de GitHub-opslagplaats splitst en de juiste items wijzigen.

Sommige algemene opties voor het aanpassen bevatten, maar niet beperkt tot:

- Bastionhost VM-grootte (variabele in azuredeploy.json)
- Naamconventies (variabelen in azuredeploy.json)
- OpenShift-cluster-specifieke instellingen, worden gewijzigd via het hosts-bestand (deployOpenShift.sh)

### <a name="configure-the-parameters-file"></a>Het parameterbestand configureren

De [sjabloon van OpenShift Container Platform](https://github.com/Microsoft/openshift-container-platform) heeft meerdere branches die beschikbaar zijn voor verschillende versies van OpenShift Container Platform.  Op basis van uw behoeften, kunt u rechtstreeks vanuit de opslagplaats implementeren of u kunt de opslagplaats splitst en aangepaste wijzigingen aanbrengen in de sjablonen of scripts voordat u implementeert.

Gebruik de `appId` waarde van de service-principal die u eerder hebt gemaakt voor de `aadClientId` parameter.

Het volgende voorbeeld ziet een parameterbestand met de naam azuredeploy.parameters.json met de vereiste invoer.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_D4s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_D4s_v3"
        },
        "cnsVmSize": {
            "value": "Standard_E4s_v3"
        },
        "osImageType": {
            "value": "defaultgallery"
        },
        "marketplaceOsImage": {
            "value": {
                "publisher": "RedHat",
                "offer": "RHEL",
                "sku": "7-RAW",
                "version": "latest"
            }
        },
        "storageKind": {
            "value": "managed"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
        },
        "rhsmUsernameOrOrgId": {
            "value": "{RHSM Username}"
        },
        "rhsmPasswordOrActivationKey": {
            "value": "{RHSM Password}"
        },
        "rhsmPoolId": {
            "value": "{Pool ID}"
        },
        "rhsmBrokerPoolId": {
            "value": "{Pool ID}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "masterClusterDnsType": {
            "value": "default"
        },
        "masterClusterDns": {
            "value": "console.contoso.com"
        },
        "routingSubDomainType": {
            "value": "nipio"
        },
        "routingSubDomain": {
            "value": "routing.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "openshiftvnet"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "mastersubnet"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "infrasubnet"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "nodesubnet"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/masterinfrasubnet"
        },
        "existingCnsSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/cnssubnet"
        },
        "existingNodeSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/nodesubnet"
        },
        "masterClusterType": {
            "value": "public"
        },
        "masterPrivateClusterIp": {
            "value": "10.1.0.200"
        },
        "routerClusterType": {
            "value": "public"
        },
        "routerPrivateClusterIp": {
            "value": "10.2.0.201"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        },
        "proxySettings": {
            "value": "none"
        },
        "httpProxyEntry": {
            "value": "none"
        },
        "httpsProxyEntry": {
            "value": "none"
        },
        "noProxyEntry": {
            "value": "none"
        }
    }
}
```

Vervang de parameters door uw specifieke gegevens.

Andere versies mogelijk verschillende parameters, dus controleer of de vereiste parameters voor de vertakking die u gebruikt.

### <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI

> [!NOTE] 
> De volgende opdracht gebruikmaken van Azure CLI 2.0.8 of hoger. U kunt controleren of de CLI-versie met de `az --version` opdracht. Zie voor het bijwerken van de CLI-versie, [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Het volgende voorbeeld wordt het cluster OpenShift en alle gerelateerde resources in een resourcegroep met de naam openshiftrg, met de naam van een implementatie van myOpenShiftCluster geïmplementeerd. De sjabloon wordt verwezen naar rechtstreeks vanuit de GitHub-opslagplaats en een lokale parameters bestand met de naam van bestand azuredeploy.parameters.json wordt gebruikt.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 30 minuten om te voltooien, op basis van het totale aantal knooppunten geïmplementeerd en opties die zijn geconfigureerd. De Bastionomgeving DNS FQDN-naam en de URL van de console OpenShift af te drukken naar de terminal wanneer de implementatie is voltooid.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Als u niet bezighouden met de opdrachtregel die wachten op de implementatie is voltooid wilt, toe te voegen `--no-wait` als een van de opties voor de implementatie van de groep. De uitvoer van de implementatie kan worden opgehaald uit Azure portal in de Implementatiesectie voor de resourcegroep.
 
## <a name="deploy-using-the-openshift-container-platform-azure-marketplace-offer"></a>Implementeren met behulp van de aanbieding voor OpenShift Container Platform Azure Marketplace

De eenvoudigste manier voor het implementeren van OpenShift Container Platform in Azure te gebruiken is de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Dit is de eenvoudigste optie, maar deze ook beperkte aanpassingsmogelijkheden. De aanbieding van Marketplace bevat de volgende configuratieopties:

- **Master knooppunten**: Drie (3) Master knooppunten met configureerbare exemplaar typt.
- **Infra knooppunten**: Drie (3) Infra knooppunten met configureerbare exemplaar typt.
- **Knooppunten**: Het aantal knooppunten kan worden geconfigureerd (tussen 2 en 9) en het Instantietype.
- **Schijftype**: Beheerde schijven worden gebruikt.
- **Netwerken**: Ondersteuning voor nieuwe of bestaande netwerk, evenals aangepaste CIDR-bereik.
- **CNS**: CNS kan worden ingeschakeld.
- **Metrische gegevens**: Metrische gegevens kan worden ingeschakeld.
- **Logboekregistratie**: Logboekregistratie kan worden ingeschakeld.
- **Azure Cloud Provider**: Kan worden ingeschakeld.

## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Als de implementatie is voltooid, haalt u de verbinding in het gedeelte van de uitvoer van de implementatie. Verbinding maken met de console OpenShift met uw browser met behulp van de `OpenShift Console URL`. U kunt ook SSH naar de bastionhost. Hieronder volgt een voorbeeld waarbij de beheerdersgebruikersnaam clusteradmin en het openbare IP voor bastionhost DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az group delete](/cli/azure/group) opdracht voor het verwijderen van de resourcegroep, OpenShift-cluster en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie in Azure oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform)

### <a name="documentation-contributors"></a>Documentatie-inzenders

Hartelijk dank Vincent Power (vincepower) en Alfred Sin (asinn826) voor hun bijdragen aan deze documentatie up-to-date te houden.
