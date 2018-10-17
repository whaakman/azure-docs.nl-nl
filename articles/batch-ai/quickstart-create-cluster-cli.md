---
title: Azure-snelstart - Een Batch AI-cluster maken - Azure CLI | Microsoft Docs
description: Snelstart - Een Batch AI-cluster maken om machine learning en AI-modellen te trainen - Azure CLI
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 09/03/2018
ms.author: danlep
ms.openlocfilehash: 0d4ba7edfb22a6710222c854ceb2bf86284d2d77
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057589"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-cli"></a>Snelstart: een cluster maken voor Batch AI-trainingstaken met de Azure CLI

In deze snelstart leert u hoe u de Azure CLI gebruikt om een Batch AI-cluster te maken dat u kunt gebruiken om AI- en machine learning-modellen te trainen. Batch AI is een beheerde service voor gegevenswetenschappers en AI-onderzoekers om AI- en machine learning-modellen op schaal te trainen in clusters virtuele Azure-machines.

Het cluster heeft in eerste instantie een enkel GPU-knooppunt. Nadat u deze snelstart hebt voltooid, hebt u een cluster dat u kunt opschalen en gebruiken om uw modellen te trainen. Dien trainingstaken in het cluster in met Batch AI, hulpprogramma's voor [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) of de [Visual Studio-hulpprogramma's voor AI](https://github.com/Microsoft/vs-tools-for-ai).

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.38 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

In deze snelstart wordt ervan uitgegaan dat u opdrachten uitvoert in een Bash-shell, in Cloud Shell of op uw lokale computer.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht `az group create`. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus2*. Zorg ervoor dat u een locatie kiest waarin de Batch AI-service beschikbaar is, bijvoorbeeld US - oost 2.

```azurecli-interactive 
az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-a-batch-ai-cluster"></a>Een Batch AI-cluster maken

Gebruik eerst de opdracht `az batchai workspace create` om een Batch AI-*werkruimte* te maken. U hebt een werkruimte nodig om uw Batch AI-clusters en andere resources in te ordenen.

```azurecli-interactive
az batchai workspace create \
    --workspace myworkspace \
    --resource-group myResourceGroup 
```

Gebruik de opdracht `az batchai cluster create` om een Batch AI-cluster te maken. In het volgende voorbeeld wordt een cluster gemaakt met de volgende eigenschappen:

* Bevat een enkel knooppunt in VM-grootte NC6, dat een NVIDIA Tesla K80-GPU heeft. 
* Voert een standaard Ubuntu Server-installatiekopie uit dat is ontworpen om op container gebaseerde toepassingen te hosten, waarvoor u de meeste trainingsworkloads kunt gebruiken. 
* Voegt een gebruikersaccount toe met de naam *myusername* en genereert, als deze nog niet bestaan, SSH-sleutels op de standaard sleutellocatie (*~/.ssh*) in uw lokale omgeving. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --vm-size Standard_NC6 \
    --target 1 \
    --user-name myusername \
    --generate-ssh-keys
```

De uitvoer van de opdracht geeft de clustereigenschappen weer. Het duurt enkele minuten om het knooppunt te maken en te starten. Voer de opdracht `az batchai cluster show` uit om de status van het cluster te bekijken. 

```azurecli-interactive
az batchai cluster show \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --output table
```

Vroeg in het proces waarin het cluster wordt gemaakt, lijkt de uitvoer op het volgende voorbeeld, waar het cluster de status `resizing` heeft:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```
Het cluster is gebruiksklaar wanneer de status `steady` is en het enkele knooppunt `Idle`.

## <a name="list-cluster-nodes"></a>Clusterknooppunten vermelden 

Als u de clusterknooppunten (in dit geval een enkel knooppunt) moet verbinden om toepassingen te installeren of onderhoud uit te voeren, haalt u verbindingsinformatie op door de opdracht `az batchai cluster node list` uit te voeren:


```azurecli-interactive
az batchai cluster node list \
    --cluster mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup 
 ```

De JSON-uitvoer is vergelijkbaar met:

```JSON
[
  {
    "ipAddress": "40.68.254.143",
    "nodeId": "tvm-1816144089_1-20180626t233430z",
    "port": 50000.0
  }
]
```
Gebruik deze informatie om een SSH-verbinding met het knooppunt te maken. Wijzig bijvoorbeeld het juiste IP-adres van uw knooppunt met de volgende opdracht:

```bash
ssh myusername@40.68.254.143 -p 50000
``` 
Sluit de SSH-sessie om door te gaan.

## <a name="resize-the-cluster"></a>Het formaat van het cluster wijzigen

Wanneer u uw cluster gebruikt om een trainingstaak uit te voeren, hebt u mogelijk meer rekenresources nodig. Als u de grootte bijvoorbeeld wilt vergroten naar twee knooppunten voor een gedistribueerde trainingstaak, voert u de opdracht [batch ai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize) uit:

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 2
```

Het duurt een aantal minuten voordat het cluster in grootte is aangepast.

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt doorgaan met Batch AI-zelfstudies en -voorbeelden, gebruikt u de Batch AI-werkruimte die in deze snelstart hebt gemaakt. 

U betaalt voor het Batch AI-cluster als de knooppunten worden uitgevoerd. Als u de clusterconfiguratie wilt behouden wanneer u geen taken hebt om uit te voeren, verandert u de grootte van het cluster in 0 knooppunten. 

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 0
```

U kunt het formaat later wijzigen naar 1 of meer knooppunten om uw taken uit te voeren. Wanneer u een cluster niet langer nodig hebt, verwijdert u het met de opdracht `az batchai cluster delete`:

```azurecli-interactive
az batchai cluster delete \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
```

U kunt de opdracht `az group delete` gebruiken om de resourcegroep voor de Batch AI-resources te verwijderen als u deze niet meer nodig hebt. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u geleerd hoe u een Batch AI-cluster maakt met de Azure CLI. Voor meer informatie over hoe u een Batch AI-cluster kunt gebruiken om een model te trainen, gaat u door met de snelstart over de training van een deep learning-model.

> [!div class="nextstepaction"]
> [Een deep learning-model trainen](./quickstart-tensorflow-training-cli.md)
