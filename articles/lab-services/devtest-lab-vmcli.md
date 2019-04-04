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
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: 48a30ef86cdb10b540ffe1231294542ccff87255
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895627"
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Maken en beheren van virtuele machines met DevTest Labs met de Azure CLI
In deze quickstart begeleidt u bij het maken, starten, verbinding te maken, bijwerken en opschonen van een ontwikkelcomputer in uw testomgeving. 

Voordat u begint:

* Als u een lab niet is gemaakt, instructies hiervoor vindt [hier](devtest-lab-create-lab.md).

* [De Azure CLI installeren](/cli/azure/install-azure-cli). Als u wilt starten, az login voor het maken van een verbinding met Azure worden uitgevoerd. 

## <a name="create-and-verify-the-virtual-machine"></a>Maken en controleer of de virtuele machine 
Voordat u DevTest Labs-gerelateerde opdrachten hebt uitgevoerd, stelt u de juiste Azure context met behulp van de `az account set` opdracht:

```azurecli
az account set --subscription 11111111-1111-1111-1111-111111111111
```

De opdracht voor het maken van een virtuele machine is: `az lab vm create`. De resourcegroep voor de labnaam van de testomgeving, en de naam van de virtuele machine zijn alle vereiste. De rest van de argumenten veranderen, afhankelijk van het type van de virtuele machine.

De volgende opdracht maakt een Windows-installatiekopie uit Azure Marketplace. De naam van de installatiekopie is hetzelfde als zou u bij het maken van een virtuele machine met behulp van de Azure portal. 

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "Visual Studio Community 2017 on Windows Server 2016 (x64)" --image-type gallery --size 'Standard_D2s_v3’ --admin-username 'AdminUser' --admin-password 'Password1!'
```

De volgende opdracht maakt u een virtuele machine op basis van een aangepaste installatiekopie die beschikbaar zijn in de testomgeving:

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "My Custom Image" --image-type custom --size 'Standard_D2s_v3' --admin-username 'AdminUser' --admin-password 'Password1!'
```

De **afbeeldingstype** argument is gewijzigd van **galerie** naar **aangepaste**. De naam van de afbeelding overeenkomt met wat u ziet als u de virtuele machine maakt in Azure portal.

De volgende opdracht maakt u een virtuele machine vanuit een marketplace-installatiekopie met ssh verificatie:

```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```

U kunt ook maken met virtuele machines op basis van formules door in te stellen de **afbeeldingstype** parameter **formule**. Als u nodig hebt om een specifieke virtuele netwerk voor uw virtuele machine te kiezen, gebruikt u de **vnet-naam** en **subnet** parameters. Zie voor meer informatie, [az lab vm maken](/cli/azure/lab/vm#az-lab-vm-create).

## <a name="verify-that-the-vm-is-available"></a>Controleer of de virtuele machine beschikbaar is.
Gebruik de `az lab vm show` opdracht uit om te controleren of de virtuele machine beschikbaar is, voordat u begint en er verbinding mee maken. 

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
De volgende opdracht wordt een virtuele machine gestart:

```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```

Verbinding maken met een virtuele machine: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) of [extern bureaublad](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>De virtuele machine bijwerken
De volgende voorbeeldopdracht van artefacten toepassing aan een virtuele machine:

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
De volgende voorbeeldopdracht wordt een virtuele machine gestopt.

```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Een virtuele machine verwijderen.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende inhoud: [Azure CLI-documentatie voor Azure DevTest Labs](/cli/azure/lab?view=azure-cli-latest). 