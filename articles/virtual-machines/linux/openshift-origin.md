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
ms.openlocfilehash: 1a40c4cc064b32aced7e976f40f6ed6a57e62204
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>OpenShift oorsprong in Azure implementeren

Er zijn meerdere manieren voor het implementeren van de oorsprong OpenShift in Azure. U kunt handmatig implementeren alle onderdelen van de Azure-infrastructuur nodig en voer vervolgens de oorsprong OpenShift [documentatie](https://docs.openshift.org/3.6/welcome/index.html).
U kunt ook een bestaande Resource Manager-sjabloon die de implementatie van het cluster OpenShift oorsprong vereenvoudigt gebruiken. Als deze sjabloon gevonden is [hier](https://github.com/Microsoft/openshift-origin).

## <a name="deploy-using-the-openshift-origin-template"></a>Implementeren met behulp van de sjabloon OpenShift oorsprong

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

### <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI


> [!NOTE] 
> De volgende opdracht vereist Azure CLI 2.0.8 of hoger. U kunt controleren of de az CLI versie met de `az --version` opdracht. Zie voor het bijwerken van de versie van de CLI [2.0 voor Azure CLI installeren]( /cli/azure/install-azure-cli).

Het volgende voorbeeld wordt het cluster OpenShift en alle bijbehorende resources geïmplementeerd in een resourcegroep met de naam myResourceGroup met de implementatienaam van een van myOpenShiftCluster. De sjabloon rechtstreeks vanuit de github-repo wordt verwezen en de naam van een lokale parameterbestand **azuredeploy.parameters.json** -bestand wordt gebruikt.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 25 minuten, afhankelijk van het totale aantal knooppunten dat is geïmplementeerd. De URL van de OpenShift-console en de DNS-naam van de master OpenShift wordt afgedrukt op de terminal wanneer de implementatie is voltooid.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Wanneer de implementatie is voltooid, verbinding maken met de OpenShift-console met behulp van de browser de `OpenShift Console Uri`. U kunt ook verbinding maken met het OpenShift-model met de volgende opdracht:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#delete) opdracht de resourcegroep, OpenShift cluster verwijderen en alle bijbehorende resources.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

- [Taken voor na implementatie](./openshift-post-deployment.md)
- [Problemen met OpenShift implementatie oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift oorsprong](https://docs.openshift.org/latest/getting_started/index.html)
