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
ms.openlocfilehash: 81d1e2a92a24d43c6324b4fe026680c379e656da
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-openshift-container-platform-in-azure"></a>OpenShift Container Platform in Azure implementeren

U kunt een van de verschillende methoden OpenShift Container Platform in Azure implementeren:

- U kunt handmatig implementeren van de onderdelen van de Azure-infrastructuur nodig en voer vervolgens het Platform van de Container OpenShift [documentatie](https://docs.openshift.com/container-platform/3.6/welcome/index.html).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-container-platform/) die vereenvoudigt de implementatie van het cluster OpenShift Container Platform.
- Een andere optie is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Een abonnement Red Hat is vereist voor alle opties. De Red Hat Enterprise Linux-exemplaar is tijdens de implementatie voor het abonnement Red Hat geregistreerd en gekoppeld aan de Pool-ID die de rechten voor OpenShift Container Platform bevat.
Zorg ervoor dat er een geldige Red Hat abonnement Manager (RHSM) gebruikersnaam, wachtwoord en groep-ID. U kunt deze informatie controleren door de aanmelding bij https://access.redhat.com.

## <a name="deploy-by-using-the-openshift-container-platform-resource-manager-template"></a>Implementeren met behulp van de OpenShift Container Platform Resource Manager-sjabloon

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, kunt u een parameterbestand gebruiken om op te geven van de invoerparameters. Voor het aanpassen van de implementatie-items die niet wordt gedekt door invoerparameters, de GitHub-repo vertakken en de juiste items wijzigen.

Sommige algemene aanpassingsopties omvatten, maar zijn niet beperkt tot:

- Virtueel netwerk CIDR (variabele in azuredeploy.json)
- VM-grootte voor de bastionomgeving (variabele in azuredeploy.json)
- Naamconventies (variabelen in azuredeploy.json)
- Details van de cluster OpenShift, gewijzigd via de hosts-bestand (deployOpenShift.sh)

### <a name="configure-the-parameters-file"></a>Het parameterbestand configureren

Gebruik de `appId` waarde van de service-principal die u eerder hebt gemaakt voor de `aadClientId` parameter. 

Het volgende voorbeeld maakt een parameterbestand met de naam azuredeploy.parameters.json met de vereiste invoerwaarden.

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

De items tussen vierkante haken met uw specifieke gegevens vervangen.

### <a name="deploy-by-using-azure-cli"></a>Implementeren met behulp van Azure CLI

> [!NOTE] 
> De volgende opdracht vereist Azure CLI 2.0.8 of hoger. U kunt controleren of de versie van de CLI met de `az --version` opdracht. Zie voor het bijwerken van de versie van de CLI [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Het volgende voorbeeld wordt het cluster OpenShift en alle bijbehorende resources geïmplementeerd in een resourcegroep met de naam myResourceGroup, met de implementatienaam van een van myOpenShiftCluster. De sjabloon wordt rechtstreeks vanuit een lokale parameters bestand azuredeploy.parameters.json-bestand met de naam wordt gebruikt en de GitHub-repo verwezen.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 30 minuten duren, afhankelijk van het totale aantal knooppunten dat is geïmplementeerd. De URL van de console OpenShift en de DNS-naam van de master afdrukken OpenShift aan de terminal wanneer de implementatie is voltooid.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-by-using-the-openshift-container-platform-azure-marketplace-offer"></a>Implementeren met behulp van de OpenShift Container Platform Azure Marketplace-aanbieding

De eenvoudigste manier om OpenShift Container Platform in Azure implementeert, is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Dit is de eenvoudigste optie, maar deze ook beperkte aanpassingsmogelijkheden heeft. De aanbieding omvat drie opties:

- **Kleine**: implementeert een niet-hoge beschikbaarheid (HA)-cluster met één hoofdknooppunt, infrastructuur met één knooppunt, twee knooppunten van de toepassing en een bastionomgeving knooppunt. Alle knooppunten zijn standaard DS2v2 VM-grootten. Dit cluster vereist 10 totaal aantal kernen en is ideaal voor kleine schaal.
- **Gemiddeld**: implementeert een HA-cluster met drie hoofdknooppunten, twee infrastructuurknooppunten vier knooppunten van de toepassing en een bastionomgeving knooppunt. Alle knooppunten, behalve het knooppunt van de bastionomgeving zijn standaard DS3v2 VM-grootten. Het knooppunt van de bastionomgeving is een standaard DS2v2. Dit cluster vereist 38 cores.
- **Grote**: implementeert een HA-cluster met drie hoofdknooppunten, twee infrastructuurknooppunten zes knooppunten van de toepassingen en bastionomgeving met één knooppunt. De hoofd- en infrastructuur knooppunten zijn standaard DS3v2 VM-grootten. De knooppunten van de toepassingen zijn standaard DS4v2 VM-grootten en het knooppunt van de bastionomgeving is een standaard DS2v2. Dit cluster vereist 70 cores.

Configuratie van Azure Cloud Solution Provider is optioneel voor middelgrote en grote clusters. De grootte klein cluster geeft geen een optie voor het configureren van Azure Cloud Solution Provider.

## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Wanneer de implementatie is voltooid, met de console OpenShift met uw browser te maken met de `OpenShift Console Uri`. U kunt u ook naar het hoofdniveau OpenShift verbinden met de volgende opdracht:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az groep verwijderen](/cli/azure/group#delete) opdracht de resourcegroep, OpenShift cluster verwijderen en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [Problemen met implementatie OpenShift in Azure oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
