---
title: Azure-snelstart - Training voor deep learning - Azure CLI | Microsoft Docs
description: Snelstart - Leer snel hoe u een neuraal netwerk voor deep learning van TensorFlow op één GPU met Batch AI traint met behulp van de Azure CLI
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
ms.openlocfilehash: 99d864a5d519ce56a559bea4db7fe89a113e47b9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157919"
---
# <a name="quickstart-train-a-deep-learning-model-with-batch-ai"></a>Snelstart: Een deep learning-model trainen met Batch AI

Deze snelstart laat zien hoe u een deep learning-voorbeeldmodel traint op een voor GPU geschikte virtuele machine die wordt beheerd met Batch AI. Batch AI is een beheerde service voor gegevenswetenschappers en AI-onderzoekers om AI- en machine learning-modellen op schaal te trainen in clusters virtuele Azure-machines. 

In dit voorbeeld gebruikt u de Azure CLI om Batch AI te configureren voor het trainen van een neuraal [TensorFlow](https://www.tensorflow.org/)-voorbeeldnetwerk op de [MNIST-database](http://yann.lecun.com/exdb/mnist/) van handgeschreven tekens. Na deze snelstart weet u hoe u Batch AI moet gebruiken voor het trainen van een AI- of machine learning-model, en bent u gereed om verschillende modellen op grotere schaal te trainen.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.38 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

In deze snelstart wordt ervan uitgegaan dat u opdrachten uitvoert in een Bash-shell, in Cloud Shell of op uw lokale computer. Als u de snelstart voor het [maken van een Batch AI-cluster met de Azure CLI](quickstart-create-cluster-cli.md) al hebt voltooid, slaat u de eerste twee stappen voor het maken van een resourcegroep en een Batch AI-cluster over.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht `az group create`. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus2*. Kies de locatie US - oost 2, of een andere locatie waar de Batch AI-service beschikbaar is. 

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

Gebruik de opdracht `az batchai cluster create` om een Batch AI-cluster te maken. In het volgende voorbeeld wordt een cluster met één knooppunt gemaakt met de volgende eigenschappen:

* De VM-grootte is NC6. Deze heeft één NVIDIA Tesla K80-GPU. Azure biedt verschillende VM-grootten met verschillende NVIDIA-GPU's.
* Standaard wordt een Ubuntu Server-installatiekopie voor containerhosttoepassingen uitgevoerd. U kunt de meeste trainingsworkloads uitvoeren op dit distributiepunt. 
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

De uitvoer van de opdracht geeft de clustereigenschappen weer. Het duurt enkele minuten om het knooppunt te maken en te starten. Voer de opdracht `az batchai cluster show` uit om de status van het cluster weer te geven.

```azurecli-interactive
az batchai cluster show \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --output table
```

Vroeg in het proces waarin het cluster wordt gemaakt, ziet de uitvoer er ongeveer zo uit en heeft het cluster de status `resizing`:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```

Voer de volgende stappen uit om het trainingsscript te uploaden en de trainingstaak te maken terwijl de cluster van status verandert. U kunt de trainingstaak uitvoeren zodra het cluster de status `steady` heeft en het knooppunt `Idle` is.

## <a name="upload-training-script"></a>Trainingsscript uploaden

Gebruik de `az storage account create`-opdracht om een opslagaccount voor uw trainingsscript en de uitvoer van de training te maken.

```azurecli-interactive
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus2 \
    --sku Standard_LRS
```

Maak een Azure-bestandsshare met de naam `myshare` in het account met behulp van de `az storage share create`-opdracht:

```azurecli-interactive
az storage share create \
    --name myshare \
    --account-name mystorageaccount
```

Maak mappen in de Azure-bestandsshare met de opdracht `az storage directory create`. Maak de map `scripts` voor het trainingsscript en `logs` voor de uitvoer van de training:

```azurecli-interactive
# Create /scripts directory in file share
az storage directory create \
    --name scripts \
    --share-name myshare \
    --account-name mystorageaccount

# Create /logs directory in file share 
az storage directory create \
    --name logs \
    --share-name myshare \
    --account-name mystorageaccount
```

Maak in de Bash-shell een lokale werkmap maken en download het TensorFlow-voorbeeld [convolutional.py](https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py). Het Python-script traint een convolutioneel neuraal netwerk op de MNIST-installatiekopieset met 60.000 met de hand geschreven cijfers (0-9). Vervolgens wordt het model getest op een set met testvoorbeelden.

```bash
wget https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py
```

Upload het script naar de map `scripts` in de share met behulp van de opdracht `az storage file upload`.

```azurecli-interactive
az storage file upload \
    --share-name myshare \
    --path scripts \
    --source convolutional.py \
    --account-name mystorageaccount
```

## <a name="submit-training-job"></a>Trainingstaak verzenden

Maak eerst een Batch AI-*experiment* in uw werkruimte met behulp van de opdracht `az batchai experiment create`. Een experiment is een logische container voor verwante Batch AI-taken.

```azurecli-interactive
az batchai experiment create \
    --name myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Maak in uw werkmap een configuratiebestand voor de trainingstaak `job.json` met de volgende inhoud. U geeft dit configuratiebestand door wanneer u de trainingstaak verzendt.

Dit `job.json`-bestand bevat instellingen voor de locatie en uitvoering van het Python-scriptbestand in een TensorFlow-container op het GPU-knooppunt. In het bestand staat ook aangegeven waar de uitvoerbestanden van de taak moeten worden opgeslagen in Azure Storage. `<AZURE_BATCHAI_STORAGE_ACCOUNT>` geeft aan dat de naam van het opslagaccount wordt opgegeven bij het verzenden van de taak.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 1,
        "tensorFlowSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/scripts/convolutional.py"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/logs",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Gebruik de opdracht `az batchai job create` voor het verzenden van de taak op het knooppunt, en geef daarbij het configuratiebestand `job.json` en de naam van uw storage-account op:

```azurecli-interactive
az batchai job create \
    --name myjob \
    --cluster mycluster \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --config-file job.json \
    --storage-account-name mystorageaccount
```

De opdracht retourneert de taakeigenschappen. Het duurt vervolgens enkele minuten voordat de taak is voltooid. U kunt de voortgang van deze taak controleren met de opdracht `az batchai job file stream` om het bestand `stdout-wk-0.txt` te streamen vanuit de standaarduitvoermap op het knooppunt. Het trainingsscript genereert dit bestand nadat de taak is gestart.  

```azurecli-interactive
az batchai job file stream \
    --job myjob \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --file-name stdout-wk-0.txt
```

Voorbeelduitvoer:

```
File found with URL "https://mystorageaccount.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/myResourceGroup/workspaces/myworkspace/experiments/myexperiment/jobs/myjob/<JOB_ID>/stdouterr/stdout-wk-0.txt?sv=2016-05-31&sr=f&sig=Kih9baozMao8Ugos%2FVG%2BcsVsSeY1O%2FTocCNvLQhwtx4%3D&se=2018-06-20T22%3A07%3A30Z&sp=rl". Start streaming
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Step 0 (epoch 0.00), 14.9 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 9.7 ms
Minibatch loss: 3.240, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.7%
Step 200 (epoch 0.23), 8.3 ms
Minibatch loss: 3.335, learning rate: 0.010000
Minibatch error: 7.8%
Validation error: 4.5%
Step 300 (epoch 0.35), 8.3 ms
Minibatch loss: 3.157, learning rate: 0.010000
Minibatch error: 3.1%
...
Step 8500 (epoch 9.89), 8.3 ms
Minibatch loss: 1.605, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.9%
Test error: 0.8%
```

Het streamen stopt wanneer de taak is voltooid. Via het voorbeeldscript wordt het model in 10 *tijdvakken* getraind of de gegevensset voor de training doorlopen. In dit voorbeeld is de training van model in 10 tijdvakken voltooid met een testfoutpercentage van slechts 0,8%.

## <a name="get-job-output"></a>Taakuitvoer ophalen

Batch AI maakt een unieke mappenstructuur in het opslagaccount voor de uitvoer van elke taak. Stel de omgevingsvariabele JOB_OUTPUT_PATH in met dit pad. Geef vervolgens een lijst met de uitvoerbestanden in de opslag weer met behulp van de opdracht `az storage file list`:

```azurecli-interactive
export JOB_OUTPUT_PATH=$(az batchai job show --name myjob --experiment myexperiment --workspace myworkspace --resource-group myResourceGroup --query jobOutputDirectoryPathSegment --output tsv)

az storage file list \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr \
    --output table
```

De uitvoer ziet er ongeveer zo uit:

```
Name               Content Length  Type    Last Modified
---------------  ----------------  ------  ---------------
execution.log               14866  file
stderr-wk-0.txt              1527  file
stdout-wk-0.txt             11027  file
```

Gebruik de opdracht `az storage file download` om een of meer bestanden te downloaden naar uw lokale werkmap. Bijvoorbeeld:

```azurecli-interactive
az storage file download \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr/stdout-wk-0.txt
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt doorgaan met Batch AI-zelfstudies en -voorbeelden, gebruikt u de Batch AI-werkruimte, het cluster en het opslagaccount die u in deze snelstart hebt gemaakt. 

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
    --resource-group myResourceGroup
```

U kunt de opdracht `az group delete` gebruiken om de resourcegroep voor de Batch AI-en opslagresources te verwijderen als u deze niet meer nodig hebt. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen
In deze snelstart hebt u geleerd hoe u Batch AI gebruikt voor het trainen van een TensorFlow-voorbeeldmodel voor deep learning op een VM met één GPU met behulp van de Azure CLI. Voor meer informatie over gedistribueerde modeltraining in een groter GPU-cluster, gaat u naar de Batch AI-zelfstudie.

> [!div class="nextstepaction"]
> [Zelfstudie over gedistribueerde training](./tutorial-horovod-tensorflow.md)