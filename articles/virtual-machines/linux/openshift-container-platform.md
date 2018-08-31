---
title: OpenShift Containerplatform in Azure implementeren | Microsoft Docs
description: OpenShift Containerplatform in Azure implementeren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: a275df4567053149688694315ff24ac1ad7f711f
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43186911"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>OpenShift Containerplatform in Azure implementeren

U kunt een van de verschillende methoden voor het implementeren van OpenShift Container Platform in Azure:

- U kunt handmatig implementeren van de onderdelen die nodig zijn Azure-infrastructuur en voer vervolgens de OpenShift Container Platform [documentatie](https://docs.openshift.com/container-platform/3.10/welcome/index.html).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-container-platform/) die vereenvoudigt de implementatie van het cluster OpenShift Container Platform.
- Een andere optie is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Een Red Hat-abonnement is vereist voor alle opties. De Red Hat Enterprise Linux-exemplaar is tijdens de implementatie aan het Red Hat-abonnement is geregistreerd en die is gekoppeld aan de groeps-ID die de rechten voor OpenShift Container Platform bevat.
Zorg ervoor dat u een geldige Red Hat abonnement Manager (RHSM)-gebruikersnaam, wachtwoord en groep-ID hebt. U kunt deze informatie controleren door aan te melden bij https://access.redhat.com.

## <a name="deploy-by-using-the-openshift-container-platform-resource-manager-template"></a>Implementeren met behulp van de OpenShift Container Platform Resource Manager-sjabloon

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, kunt u een parameterbestand gebruiken om op te geven van de invoerparameters. Voor het aanpassen van een van de implementatie-items die niet worden gedekt door met behulp van de invoerparameters die zijn opgegeven, de GitHub-opslagplaats splitst en de juiste items wijzigen.

Sommige algemene aanpassingsopties bevatten, maar zijn niet beperkt tot:

- Virtueel netwerk CIDR (variabele in azuredeploy.json)
- Bastionhost VM-grootte (variabele in azuredeploy.json)
- Naamconventies (variabelen in azuredeploy.json)
- OpenShift-cluster-specifieke instellingen, worden gewijzigd via het hosts-bestand (deployOpenShift.sh)

### <a name="configure-the-parameters-file"></a>Het parameterbestand configureren

Gebruik de `appId` waarde van de service-principal die u eerder hebt gemaakt voor de `aadClientId` parameter. 

Het volgende voorbeeld wordt een parameterbestand met de naam azuredeploy.parameters.json met de vereiste invoer.

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

Vervang de items tussen vierkante haken met uw specifieke gegevens.

### <a name="deploy-by-using-azure-cli"></a>Implementeren met behulp van Azure CLI

> [!NOTE] 
> De volgende opdracht gebruikmaken van Azure CLI 2.0.8 of hoger. U kunt controleren of de CLI-versie met de `az --version` opdracht. Zie voor het bijwerken van de CLI-versie, [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Het volgende voorbeeld wordt het cluster OpenShift en alle gerelateerde resources in een resourcegroep met de naam myResourceGroup, met de implementatienaam van een van myOpenShiftCluster geïmplementeerd. De sjabloon wordt verwezen naar rechtstreeks vanuit de GitHub-opslagplaats en een lokale parameters bestand met de naam van bestand azuredeploy.parameters.json wordt gebruikt.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 30 minuten om te voltooien, afhankelijk van het totale aantal geïmplementeerde knooppunten. De URL van de console OpenShift en de DNS-naam van de OpenShift master af te drukken naar de terminal wanneer de implementatie is voltooid.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-by-using-the-openshift-container-platform-azure-marketplace-offer"></a>Implementeren met behulp van de aanbieding voor OpenShift Container Platform Azure Marketplace

De eenvoudigste manier voor het implementeren van OpenShift Container Platform in Azure te gebruiken is de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Dit is de eenvoudigste optie, maar deze ook beperkte aanpassingsmogelijkheden. De aanbieding omvat drie configuratieopties:

- **Kleine**: implementeert een niet-high availability (HA)-cluster met één hoofdknooppunt, infrastructuur voor één knooppunt, twee knooppunten van de toepassing en een bastionhost knooppunt. Alle knooppunten zijn standaard DS2v2 VM-grootten. Dit cluster vereist 10 totaal aantal kerngeheugens en is ideaal voor het testen van kleinschalige.
- **Gemiddeld**: implementeert een HA-cluster met drie hoofdknooppunten, twee infrastructuurknooppunten, vier knooppunten van de toepassing en een bastionhost knooppunt. Alle knooppunten, met uitzondering van het knooppunt van de bastionomgeving zijn standaard DS3v2 VM-grootten. Het knooppunt van de bastionomgeving is een standaard DS2v2. Dit cluster vereist 38 kernen.
- **Grote**: implementeert een HA-cluster met drie hoofdknooppunten, twee infrastructuurknooppunten, zes knooppunten van de toepassingen en één bastionhost knooppunt. De hoofd- en -infrastructuur-knooppunten zijn standaard DS3v2 VM-grootten. De knooppunten van de toepassingen zijn standaard DS4v2 VM-grootten en het knooppunt van de bastionomgeving is een standaard DS2v2. In dit cluster zijn 70 kerngeheugens nodig.

Configuratie van Azure Cloud Solution Provider is optioneel voor middelgrote en grote clustergrootten. De grootte klein cluster geeft geen een optie voor het configureren van Azure Cloud Solution Provider.

## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Wanneer de implementatie is voltooid, met de console OpenShift met uw browser te maken met de `OpenShift Console Uri`. U kunt ook u kunt verbinding maken met de master OpenShift met behulp van de volgende opdracht uit:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az group delete](/cli/azure/group#az_group_delete) opdracht voor het verwijderen van de resourcegroep, OpenShift-cluster en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie in Azure oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
