---
title: Maken en beheren van virtuele machines in DevTest Labs met Azure CLI | Microsoft Docs
description: Informatie over het gebruik van Azure DevTest Labs maken en beheren van virtuele machines met Azure CLI 2.0
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: v-craic
ms.openlocfilehash: e73ddeba56c779d9fb1be77a50cbae5111de03c4
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Maken en beheren van virtuele machines met DevTest Labs met de Azure CLI
Deze snel starten begeleidt u bij het maken, starten, verbinding te maken, bijwerken en een ontwikkelcomputer in uw testomgeving opruimen. 

Voordat u begint:

* Als een lab niet gemaakt is, vindt u instructies [hier](devtest-lab-create-lab.md).

* [CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). Als u wilt starten, az aanmelding voor het maken van een verbinding met Azure worden uitgevoerd. 

## <a name="create-and-verify-the-virtual-machine"></a>Maken en controleer of de virtuele machine 
Een virtuele machine maken vanuit een marketplace-installatiekopie met ssh verificatie.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> Plaats de **lab van resourcegroep** naam in de--resourcegroep parameter.
>

Als u wilt maken van een virtuele machine met een formule, gebruik de parameter van de--formule in [az lab vm maken](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create).


Controleer of de virtuele machine beschikbaar is.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-to-the-virtual-machine"></a>Start en verbinding met de virtuele machine
Een virtuele machine start.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> Plaats de **lab van resourcegroep** naam in de--resourcegroep parameter.
>

Verbinding maken met een virtuele machine: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) of [extern bureaublad](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>De virtuele machine bijwerken
Artefacten toepassen op een virtuele machine.
```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Lijst met artefacten is beschikbaar in de testomgeving.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a>Stoppen en verwijderen van de virtuele machine    
Een virtuele machine stoppen.
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Verwijderen van een virtuele machine.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]