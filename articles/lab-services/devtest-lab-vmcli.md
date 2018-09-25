---
title: Maken en beheren van virtuele machines in DevTest Labs met Azure CLI | Microsoft Docs
description: Informatie over het gebruik van Azure DevTest Labs maken en beheren van virtuele machines met Azure CLI
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: ccf9b08856fcc652e3ad4b2b31587d43d7ef9cca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995944"
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Maken en beheren van virtuele machines met DevTest Labs met de Azure CLI
In deze quick start begeleidt u bij het maken, starten, verbinding te maken, bijwerken en opschonen van een ontwikkelcomputer in uw testomgeving. 

Voordat u begint:

* Als u een lab niet is gemaakt, instructies hiervoor vindt [hier](devtest-lab-create-lab.md).

* [De Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). Als u wilt starten, az login voor het maken van een verbinding met Azure worden uitgevoerd. 

## <a name="create-and-verify-the-virtual-machine"></a>Maken en controleer of de virtuele machine 
Een virtuele machine maken vanuit een marketplace-installatiekopie met ssh verificatie.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> Plaats de **lab van resourcegroep** naam in de--resource-group-parameter.
>

Als u wilt een virtuele machine maken met behulp van een formule, gebruik de formule parameter--in [az lab vm maken](https://docs.microsoft.com/cli/azure/lab/vm#az-lab-vm-create).


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

## <a name="start-and-connect-to-the-virtual-machine"></a>Start en verbinding maken met de virtuele machine
Start een virtuele machine.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> Plaats de **lab van resourcegroep** naam in de--resource-group-parameter.
>

Verbinding maken met een virtuele machine: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) of [extern bureaublad](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>De virtuele machine bijwerken
Artefacten van toepassing op een virtuele machine.
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

De artefacten van de lijst is beschikbaar in het lab.
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

Een virtuele machine verwijderen.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
