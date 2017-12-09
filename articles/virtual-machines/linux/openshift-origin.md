---
title: OpenShift oorsprong in Azure implementeren | Microsoft Docs
description: OpenShift oorsprong in Azure implementeren.
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
ms.openlocfilehash: d593b011ce4d0998ef07bc257d3c26f1286e3b0e
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>OpenShift oorsprong in Azure implementeren

U kunt twee manieren OpenShift oorsprong in Azure implementeren:

- U kunt handmatig implementeren alle onderdelen van de Azure-infrastructuur nodig en voer vervolgens de oorsprong OpenShift [documentatie](https://docs.openshift.org/3.6/welcome/index.html).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-origin) die vereenvoudigt de implementatie van het cluster OpenShift oorsprong.

## <a name="deploy-by-using-the-openshift-origin-template"></a>Implementeren met behulp van de sjabloon OpenShift oorsprong

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
> De volgende opdracht vereist Azure CLI 2.0.8 of hoger. U kunt controleren of de versie van de CLI met de `az --version` opdracht. Zie voor het bijwerken van de versie van de CLI [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Het volgende voorbeeld wordt het cluster OpenShift en alle bijbehorende resources geïmplementeerd in een resourcegroep met de naam myResourceGroup, met de implementatienaam van een van myOpenShiftCluster. De sjabloon wordt verwezen rechtstreeks uit de GitHub-opslagplaats met behulp van een lokale parameterbestand met de naam azuredeploy.parameters.json.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 25 minuten duren, afhankelijk van het totale aantal knooppunten dat is geïmplementeerd. De URL van de console OpenShift en de DNS-naam van de master afdrukken OpenShift aan de terminal wanneer de implementatie is voltooid.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Wanneer de implementatie is voltooid, met de console OpenShift met uw browser te maken met de `OpenShift Console Uri`. U kunt u ook naar het hoofdniveau OpenShift verbinden met de volgende opdracht:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az groep verwijderen](/cli/azure/group#delete) opdracht de resourcegroep, OpenShift cluster verwijderen en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [Problemen met OpenShift implementatie oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html)
