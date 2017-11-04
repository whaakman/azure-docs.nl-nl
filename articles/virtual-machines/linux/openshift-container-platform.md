---
title: OpenShift Container Platform in Azure implementeren | Microsoft Docs
description: OpenShift Container Platform in Azure implementeren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: c91b7232b2f87e0b4b5e659126b96a6ef8b4202c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-container-platform-in-azure"></a>OpenShift Container Platform in Azure implementeren

Er zijn meerdere manieren voor het implementeren van OpenShift Container Platform in Azure. U kunt handmatig implementeren alle onderdelen van de Azure-infrastructuur nodig en voer vervolgens het Platform van de Container OpenShift [documentatie](https://docs.openshift.com/container-platform/3.6/welcome/index.html).
U kunt ook een bestaande Resource Manager-sjabloon die de implementatie van het cluster OpenShift Container Platform vereenvoudigt gebruiken. Als deze sjabloon gevonden is [hier](https://github.com/Microsoft/openshift-container-platform/).

Een andere optie is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Een Red Hat-abonnement is vereist voor beide opties. De RHEL-exemplaar is tijdens de implementatie voor het Red Hat abonnement geregistreerd en gekoppeld aan de Pool-ID die de rechten voor OpenShift Container Platform bevat.
Zorg ervoor dat u hebt een geldige Red Hat abonnement Manager Username, wachtwoord en groep-ID (RHSM gebruikersnaam, wachtwoord RHSM en groep-ID). U kunt de informatie door aan te melden bij https://access.redhat.com controleren.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Met de OpenShift Container Platform Resource Manager-sjabloon implementeren

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, wordt een parameterbestand gebruikt om op te geven van de invoerparameters. Als u wilt aanpassen van een van de implementatie-items die niet worden ondersteund met invoerparameters fork de github-repo en relevante items wijzigen.

Sommige algemene aanpassingsopties bevatten (maar niet beperkt tot):

- VNet CIDR [variabele in azuredeploy.json]
- VM-grootte voor de bastionomgeving [variabele in azuredeploy.json]
- Naamconventies [variabelen in azuredeploy.json]
- OpenShift cluster foutopsporingsgegevens - gewijzigd via het hosts-bestand [deployOpenShift.sh]

### <a name="configure-parameters-file"></a>Parameterbestand configureren

Gebruik de `appId` waarde van de service-principal die u eerder hebt gemaakt voor de `aadClientId` parameter. 

Het volgende voorbeeld maakt u een parameterbestand met de naam **azuredeploy.parameters.json** met de vereiste invoerwaarden.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
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
            "value": "true"
        },
        "enableCockpit": {
            "value": "false"
        },
        "rhsmUsernamePasswordOrActivationKey": {
            "value": "usernamepassword"
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
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

Vervang de items die zijn ingesloten in {} met de relevante informatie.

### <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI

> [!NOTE] 
> De volgende opdracht vereist Azure CLI 2.0.8 of hoger. U kunt controleren of de az CLI versie met de `az --version` opdracht. Zie voor het bijwerken van de versie van de CLI [2.0 voor Azure CLI installeren]( /cli/azure/install-azure-cli).

Het volgende voorbeeld wordt het cluster OpenShift en alle bijbehorende resources geïmplementeerd in een resourcegroep met de naam myResourceGroup met de implementatienaam van een van myOpenShiftCluster. De sjabloon rechtstreeks vanuit de github-repo wordt verwezen en de naam van een lokale parameterbestand **azuredeploy.parameters.json** -bestand wordt gebruikt.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 30 minuten duren, afhankelijk van het totale aantal knooppunten dat is geïmplementeerd. De URL van de OpenShift-console en de DNS-naam van de master OpenShift wordt afgedrukt op de terminal wanneer de implementatie is voltooid.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-using-openshift-container-platform-marketplace-offer"></a>Implementeren met behulp van OpenShift Container Platform marketplace-aanbieding

De eenvoudigste manier om OpenShift Container Platform in Azure implementeert, is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Deze optie is de eenvoudigste, maar ook beperkte aanpassingsmogelijkheden voor. De aanbieding omvat drie opties:

- Kleine: Implementeert een niet-HA-cluster met één knooppunt van het model, één knooppunt van de infrastructuur, twee knooppunten van de toepassing en één knooppunt van de Bastionomgeving. Alle knooppunten zijn standaard DS2v2 VM-grootten. Dit cluster vereist 10 totaal aantal kernen en is ideaal voor het testen op kleine schaal.
- Gemiddeld: Implementeert een HA-cluster met drie Master knooppunten, Infrastructuurknooppunten twee, vier knooppunten van de toepassing en één knooppunt van de Bastionomgeving. Alle knooppunten, met uitzondering van de Bastionomgeving zijn standaard DS3v2 VM-grootten. Het knooppunt van de Bastionomgeving is een standaard DS2v2. Dit cluster vereist 38 cores.
- Grote: Implementeert een Cluster met hoge beschikbaarheid met drie Master knooppunten, twee Infrastructuurknooppunten zes knooppunten van de toepassingen en één knooppunt van de Bastionomgeving. De hoofd- en Infrastructuurknooppunten zijn standaard DS3v2 VM-grootten, de knooppunten van de toepassingen zijn standaard DS4v2 VM-grootten en het knooppunt van de Bastionomgeving is een standaard DS2v2. Dit cluster vereist 70 cores.

Configuratie van Azure Cloud Provider is optioneel voor clusters Medium en Large. De grootte klein cluster geeft geen een optie voor het configureren van Azure Cloud Provider.

## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Wanneer de implementatie is voltooid, verbinding maken met de OpenShift-console met behulp van de browser de `OpenShift Console Uri`. U kunt ook verbinding maken met het OpenShift-model met de volgende opdracht:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#delete) opdracht de resourcegroep, OpenShift cluster verwijderen en alle bijbehorende resources.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- [Taken voor na implementatie](./openshift-post-deployment.md)
- [Problemen met OpenShift implementatie oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
