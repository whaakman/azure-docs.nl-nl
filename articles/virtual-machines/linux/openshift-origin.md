---
title: OKD in Azure implementeren | Microsoft Docs
description: OKD in Azure implementeren.
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
ms.openlocfilehash: 0d3a9f05802bef7d6dfc99fcfae6668044f214c8
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190301"
---
# <a name="deploy-okd-in-azure"></a>OKD in Azure implementeren

U kunt een van twee manieren om OKD (voorheen OpenShift Origin) in Azure implementeren:

- U kunt ook handmatig implementeren van alle onderdelen van de benodigde Azure-infrastructuur en volg vervolgens de OKD [documentatie](https://docs.okd.io/3.10/welcome/index.html).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-origin) die de implementatie van het cluster OKD vereenvoudigt.

## <a name="deploy-by-using-the-okd-template"></a>Implementeren met behulp van de sjabloon OKD

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

### <a name="deploy-by-using-azure-cli"></a>Implementeren met behulp van Azure CLI


> [!NOTE] 
> De volgende opdracht gebruikmaken van Azure CLI 2.0.8 of hoger. U kunt controleren of de CLI-versie met de `az --version` opdracht. Zie voor het bijwerken van de CLI-versie, [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Het volgende voorbeeld wordt het cluster OKD en alle gerelateerde resources in een resourcegroep met de naam myResourceGroup, met de implementatienaam van een van myOpenShiftCluster geïmplementeerd. De sjabloon wordt verwezen naar rechtstreeks vanuit de GitHub-opslagplaats met behulp van een lokale parameterbestand met de naam azuredeploy.parameters.json.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 25 minuten duren, afhankelijk van het totale aantal geïmplementeerde knooppunten. De URL van de console OKD en de DNS-naam van de OpenShift master af te drukken naar de terminal wanneer de implementatie is voltooid.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-okd-cluster"></a>Verbinding maken met het cluster OKD

Wanneer de implementatie is voltooid, verbinding met de console OKD met uw browser met behulp van de `OpenShift Console Uri`. U kunt ook u kunt verbinding maken met de master OKD met behulp van de volgende opdracht uit:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az group delete](/cli/azure/group#az_group_delete) opdracht voor het verwijderen van de resourcegroep, OpenShift-cluster en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OKD](https://docs.okd.io/latest/getting_started/index.html)
