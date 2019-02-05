---
title: OKD in Azure implementeren | Microsoft Docs
description: OKD in Azure implementeren.
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
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 571190324c5a0844624bd8a838cd103317fb53ca
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55729146"
---
# <a name="deploy-okd-in-azure"></a>OKD in Azure implementeren

U kunt een van twee manieren om OKD (voorheen OpenShift Origin) in Azure implementeren:

- U kunt alle onderdelen van de benodigde Azure-infrastructuur handmatig te implementeren en volg vervolgens de [OKD documentatie](https://docs.okd.io).
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-origin) die de implementatie van het cluster OKD vereenvoudigt.

## <a name="deploy-using-the-okd-template"></a>Implementeren met behulp van de sjabloon OKD

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, kunt u een parameterbestand gebruiken om op te geven van de invoerparameters. Voor het verder aanpassen van de implementatie, de GitHub-opslagplaats splitst en de juiste items wijzigen.

Sommige algemene opties voor het aanpassen bevatten, maar niet beperkt tot:

- Bastionhost VM-grootte (variabele in azuredeploy.json)
- Naamconventies (variabelen in azuredeploy.json)
- OpenShift-cluster-specifieke instellingen, worden gewijzigd via het hosts-bestand (deployOpenShift.sh)

De [OKD sjabloon](https://github.com/Microsoft/openshift-origin) heeft meerdere branches die beschikbaar zijn voor verschillende versies van OKD.  Op basis van uw behoeften, kunt u rechtstreeks vanuit de opslagplaats implementeren of u kunt de opslagplaats splitst en aangepaste wijzigingen aanbrengen voordat u implementeert.

Gebruik de `appId` waarde van de service-principal die u eerder hebt gemaakt voor de `aadClientId` parameter.

Hier volgt een voorbeeld van een parameterbestand met de naam azuredeploy.parameters.json met de vereiste invoer.

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
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "false"
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

Vervang de parameters door uw specifieke gegevens.

Andere versies mogelijk verschillende parameters dus controleer of de vereiste parameters voor de vertakking die u gebruikt.

### <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI


> [!NOTE] 
> De volgende opdracht gebruikmaken van Azure CLI 2.0.8 of hoger. U kunt controleren of de CLI-versie met de `az --version` opdracht. Zie voor het bijwerken van de CLI-versie, [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Het volgende voorbeeld wordt het cluster OKD en alle gerelateerde resources in een resourcegroep met de naam openshiftrg, met de naam van een implementatie van myOpenShiftCluster geïmplementeerd. De sjabloon wordt verwezen naar rechtstreeks vanuit de GitHub-opslagplaats tijdens het gebruik van een lokale parameterbestand met de naam azuredeploy.parameters.json.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

De implementatie duurt ten minste 30 minuten duren, afhankelijk van het totale aantal geïmplementeerde knooppunten. De URL van de console OpenShift en de DNS-naam van de OpenShift master af te drukken naar de terminal wanneer de implementatie is voltooid. U kunt ook de uitvoersectie van de implementatie van de Azure-portal weergeven.

```json
{
  "OpenShift Console Url": "http://openshiftlb.cloudapp.azure.com/console",
  "OpenShift Master SSH": "ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com"
}
```

Als u niet bezighouden met de opdrachtregel die wachten op de implementatie is voltooid wilt, toe te voegen `--no-wait` als een van de opties voor de implementatie van de groep. De uitvoer van de implementatie kan worden opgehaald uit Azure portal in de Implementatiesectie voor de resourcegroep.

## <a name="connect-to-the-okd-cluster"></a>Verbinding maken met het cluster OKD

Wanneer de implementatie is voltooid, verbinding maken met de console OpenShift met het gebruik van uw browser de `OpenShift Console Url`. U kunt ook SSH naar het hoofdniveau OKD. Hieronder volgt een voorbeeld waarin de uitvoer van de implementatie:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az group delete](/cli/azure/group) opdracht voor het verwijderen van de resourcegroep, OpenShift-cluster en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OKD](https://docs.okd.io)
