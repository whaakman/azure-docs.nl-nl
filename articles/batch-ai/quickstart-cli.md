---
title: 'Azure-snelstart: CNTK-training met Batch AI - Azure CLI | Microsoft Docs'
description: Leer snel hoe u een CNTK-trainingstaak uitvoert met Batch AI met behulp van Azure CLI
services: batch-ai
documentationcenter: na
author: AlexanderYukhanov
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 06/14/2018
ms.author: Alexander.Yukhanov
ms.openlocfilehash: eb00c1d4ec74b5268a1497b11087030ab6a86e5a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294070"
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Een CNTK-trainingstaak uitvoeren met behulp van Azure CLI

Met behulp van Azure CLI 2.0 kunt u resources van Batch AI maken en beheren, bestandsservers en -clusters van Batch AI maken en verwijderen, en trainingstaken verzenden, beëindigen en bewaken.

Deze snelstartgids laat zien hoe u een GPU-cluster maakt en een trainingstaak uitvoert met Microsoft Cognitive Toolkit (CNTK).

Het trainingsscript [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) is beschikbaar op de Batch AI-pagina van GitHub. Dit script traint een convolutioneel neurale netwerk van de MNIST-database op het herkennen van met de hand geschreven cijfers.

Het officiële CNTK voorbeeld is aangepast voor het accepteren van de locatie van de gegevensset voor de training en van de uitvoermap via opdrachtregelargumenten.

## <a name="quickstart-overview"></a>Overzicht van snelstartgids

* Een GPU-cluster met één knooppunt maken (met de VM-grootte `Standard_NC6`) met de naam `nc6`
* Een opslagaccount maken voor het opslaan van taakinvoer en -uitvoer
* Een Azure-bestandsshare maken met twee mappen, `logs` en `scripts`, voor het opslaan van taakuitvoer en trainingsscripts
* Een Azure Blob-Container `data` maken voor het opslaan van trainingsgegevens
* Het trainingsscript en de trainingsgegevens implementeren naar de gemaakte bestandsshare en container
* De taak configureren voor het koppelen van de Azure-bestandsshare en Azure Blob-container op het knooppunt van het cluster en de onderdelen als een gewoon bestandssysteem beschikbaar maken op `$AZ_BATCHAI_JOB_MOUNT_ROOT/logs`, `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts` en `$AZ_BATCHAI_JOB_MOUNT_ROOT/data`.
`AZ_BATCHAI_JOB_MOUNT_ROOT` is een omgevingsvariabele die door Batch AI wordt ingesteld voor de taak.
* De uitvoering van de taak bewaken door de standaarduitvoer van de taak te streamen
* Als de taak is voltooid, de uitvoer en gegenereerde modellen controleren
* Alle toegewezen resources verwijderen

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* Toegang tot Azure CLI 2.0 met versie 0.3 of hoger van de batchai-module. U kunt Azure CLI 2.0 gebruiken in [Azure Cloud Shell](../cloud-shell/overview.md) of lokaal installeren met [deze instructies](/cli/azure/install-azure-cli?view=azure-cli-latest).

  Als u Cloud-Shell gebruikt, wijzigt u de werkmap in `/usr/$USER/clouddrive` omdat de basismap geen lege ruimte heeft:

  ```azurecli
  cd /usr/$USER/clouddrive
  ```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container voor het implementeren en beheren van Azure-resources. Met de volgende opdracht maakt u een nieuwe resourcegroep `batchai.quickstart` op de locatie East US:

```azurecli
az group create -n batchai.quickstart -l eastus
```
## <a name="create-batch-ai-workspace"></a>Batch AI-werkruimte maken

Met de volgende opdracht maakt u een Batch AI-werkruimte in de resourcegroep. Een Batch AI-werkruimte is een verzameling op het hoogste niveau van alle soorten AI Batch-resources.

```azurecli
az batchai workspace create -g batchai.quickstart -n quickstart
```

## <a name="create-gpu-cluster"></a>GPU-cluster maken

Met de volgende opdracht maakt u in de werkruimte een GPU-cluster met één knooppunt (VM-grootte is Standard_NC6) met Ubuntu DSVM (Data Science Virtual Machine) als de installatiekopie van het besturingssysteem:

```azurecli
az batchai cluster create -n nc6 -g batchai.quickstart -w quickstart -s Standard_NC6 -i UbuntuDSVM -t 1 --generate-ssh-keys
```

Ubuntu DSVM stelt u in staat om trainingstaken uit te voeren in docker-containers en om de meest gebruikte frameworks voor deep learning rechtstreeks op de VM uit te voeren.

Met de optie `--generate-ssh-keys` geeft u Azure CLI opdracht persoonlijke en openbare ssh-sleutels te genereren als u deze nog niet hebt. U kunt toegang krijgen tot de clusterknooppunten met behulp van de huidige gebruikersnaam en de gegenereerde ssh-sleutel.

Als u Cloud-Shell gebruikt, is het raadzaam om een back-up van de map ~/.ssh te maken in permanente opslag.

Voorbeelduitvoer:
```json
{
  "allocationState": "steady",
  "allocationStateTransitionTime": "2018-04-11T21:17:26.345000+00:00",
  "creationTime": "2018-04-11T20:12:10.758000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/clusters/nc6",
  "location": "eastus",
  "name": "nc6",
  "nodeSetup": null,
  "nodeStateCounts": {
    "additionalProperties": {},
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T20:12:11.445000+00:00",
  "resourceGroup": "batchai.quickstart",
  "scaleSettings": {
    "additionalProperties": {},
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": null,
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "additionalProperties": {},
    "adminUserName": "myuser",
    "adminUserPassword": null,
    "adminUserSshPublicKey": "<YOUR SSH PUBLIC KEY HERE>"
  },
  "virtualMachineConfiguration": {
    "additionalProperties": {},
    "imageReference": {
      "additionalProperties": {},
      "offer": "linux-data-science-vm-ubuntu",
      "publisher": "microsoft-ads",
      "sku": "linuxdsvmubuntu",
      "version": "latest",
      "virtualMachineImageId": null
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
}
```

## <a name="create-a-storage-account"></a>Create a storage account

Met de volgende opdracht maakt u een opslagaccount in de resourcegroep die ook is gebruikt om het Batch AI-cluster te maken. U gebruikt het opslagaccount voor het opslaan van taakinvoer en -uitvoer. Werk de opdracht bij met de naam van een uniek opslagaccount.

```azurecli
az storage account create -n <storage account name> --sku Standard_LRS -g batchai.quickstart
```


## <a name="deploy-data"></a>Gegevens implementeren

### <a name="download-the-training-script-and-training-data"></a>Trainingsscript en trainingsgegevens downloaden

* Download een voorbewerkte MNIST-database van [deze locatie](https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D) en pak de database uit in de huidige map.

Voor GNU/Linux of Cloud Shell:

```azurecli
wget "https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D" -O mnist_dataset.zip
unzip mnist_dataset.zip
```

Het kan zijn dat u `unzip` moet installeren als deze opdracht niet beschikbaar is in uw GNU/Linux-distributie.

* Download het voorbeeldscript [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) naar de huidige map:

Voor GNU/Linux of Cloud Shell:

```azurecli
wget https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py
```

### <a name="create-azure-file-share-and-deploy-the-training-script"></a>Azure-bestandsshare maken en trainingsscript implementeren

Met de volgende opdrachten maakt u de Azure-bestandsshares `scripts` en `logs` en kopieert u het trainingsscript naar de map `cntk` in de share `scripts`:

```azurecli
az storage share create -n scripts --account-name <storage account name>
az storage share create -n logs --account-name <storage account name>
az storage directory create -n cntk -s scripts --account-name <storage account name>
az storage file upload -s scripts --source ConvNet_MNIST.py --path cntk --account-name <storage account name> 
```

### <a name="create-a-blob-container-and-deploy-training-data"></a>Een blob-container maken en trainingsgegevens implementeren

Met de volgende opdrachten maakt u een Azure-blob-container met de naam `data` en kopieert u trainingsgegevens naar de map `mnist_cntk`:

```azurecli
az storage container create -n data --account-name <storage account name>
az storage blob upload-batch -s . --pattern '*28x28_cntk*' --destination data --destination-path mnist_cntk --account-name <storage account name>
```

## <a name="submit-training-job"></a>Trainingstaak verzenden

### <a name="create-a-batch-ai-experiment"></a>Een Batch AI-experiment maken

Een experiment is een logische container voor verwante Batch AI-taken. Gebruik de volgende opdracht om een experiment te maken in uw werkruimte:

```azurecli
az batchai experiment create -g batchai.quickstart -w quickstart -n quickstart
```

### <a name="prepare-job-configuration-file"></a>Taakconfiguratiebestand voorbereiden

Maak een configuratiebestand voor de trainingstaak `job.json` met de volgende inhoud. Gebruik de naam van uw opslagaccount in de opdracht.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-03-01/cntk.json",
    "properties": {
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs"
        }],
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<YOUR_STORAGE_ACCOUNT>.file.core.windows.net/logs",
                    "relativeMountPath": "logs"
                },
                {
                    "azureFileUrl": "https://<YOUR_STORAGE_ACCOUNT>.file.core.windows.net/scripts",
                    "relativeMountPath": "scripts"
                }
            ],
            "azureBlobFileSystems": [
                {
                    "accountName": "<YOUR_STORAGE_ACCOUNT>",
                    "containerName": "data",
                    "relativeMountPath": "data"
                }
            ]
        }
    }
}
```

Dit configuratiebestand bevat de volgende items:

* `nodeCount` -aantal knooppunten dat vereist is door de taak (1 voor deze snelstartgids)
* `cntkSettings` -het pad van het trainingsscript en de opdrachtregelargumenten. De opdrachtregelargumenten geven het pad naar de trainingsgegevens aan en het doelpad voor het opslaan van gegenereerde modellen. `AZ_BATCHAI_OUTPUT_MODEL` is een omgevingsvariabele die door Batch AI wordt ingesteld op basis van de configuratie van een uitvoermap (zie hieronder)
* `stdOutErrPathPrefix` -pad waar Batch AI mappen gaat maken met de uitvoer en logboeken van de taak
* `outputDirectories` -verzameling uitvoermappen die moeten worden gemaakt door Batch AI. Voor elke map maakt Batch AI een omgevingsvariabele met de naam `AZ_BATCHAI_OUTPUT_<id>`, waarbij `<id>` de map-id is
* `mountVolumes` -lijst met bestandssystemen die moeten worden gekoppeld tijdens uitvoering van de taak. De bestandssystemen worden gekoppeld onder `AZ_BATCHAI_JOB_MOUNT_ROOT/<relativeMountPath>`. `AZ_BATCHAI_JOB_MOUNT_ROOT` is een omgevingsvariabele die door Batch AI wordt ingesteld
* `<AZURE_BATCHAI_STORAGE_ACCOUNT>` -de naam van het opslagaccount die tijdens het indienen van de taak wordt opgegeven via de omgevingsvariabele `--storage-account-name parameter` of `AZURE_BATCHAI_STORAGE_ACCOUNT` op uw computer.

### <a name="submit-the-job"></a>De taak verzenden

```azurecli
az batchai job create -n cntk_python_1 -c nc6 -g batchai.quickstart -w quickstart -e quickstart  -f job.json --storage-account-name <storage account name>
```

Voorbeelduitvoer:
```
{
  "caffe2Settings": null,
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/clusters/nc6",
    "resourceGroup": "batchai.quickstart"
  },
  "cntkSettings": {
    "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py"
  },
  "constraints": {
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": null,
  "creationTime": "2018-06-14T22:22:57.543000+00:00",
  "customMpiSettings": null,
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": {
    "endTime": null,
    "errors": null,
    "exitCode": null,
    "startTime": "2018-06-14T22:22:59.838000+00:00"
  },
  "executionState": "running",
  "executionStateTransitionTime": "2018-06-14T22:22:59.838000+00:00",
  "horovodSettings": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1",
  "inputDirectories": null,
  "jobOutputDirectoryPathSegment": "00000000-0000-0000-0000-000000000000/batchai.quickstart/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1/f2d6ff09-7549-4e1a-8cd8-ec839f042a61",
  "jobPreparation": null,
  "mountVolumes": {
    "azureBlobFileSystems": [
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "containerName": "data",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "mountOptions": null,
        "relativeMountPath": "data"
      }
    ],
    "azureFileShares": [
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "azureFileUrl": "https://<YOUR STORAGE ACCOUNT NAME>.file.core.windows.net/logs",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "logs"
      },
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "azureFileUrl": "https://<YOUR STORAGE ACCOUNT NAME>.file.core.windows.net/scripts",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "scripts"
      }
    ],
    "fileServers": null,
    "unmanagedFileSystems": null
  },
  "name": "cntk_python_1",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
      "pathSuffix": null
    }
  ],
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-06-14T22:22:58.625000+00:00",
  "pyTorchSettings": null,
  "resourceGroup": "danlep0614b",
  "schedulingPriority": "normal",
  "secrets": null,
  "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
  "tensorFlowSettings": null,
  "toolType": "cntk",
  "type": "Microsoft.BatchAI/workspaces/experiments/jobs"
}

```

## <a name="monitor-job-execution"></a>Taakuitvoering controleren

Het trainingsscript meldt de voortgang van de training in het bestand `stderr.txt` in de standaardmap voor uitvoer. U kunt de voortgang bijhouden met de volgende opdracht:

```azurecli
az batchai job file stream -j cntk_python_1 -g batchai.quickstart -w quickstart -e quickstart -f stderr.txt
```

Voorbeelduitvoer:
```
File found with URL "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>/stdouterr/stderr.txt?sv=2016-05-31&sr=f&sig=n86JK9YowV%2BPQ%2BkBzmqr0eud%2FlpRB%2FVu%2FFlcKZx192k%3D&se=2018-04-11T23%3A05%3A54Z&sp=rl". Start streaming
Selected GPU[0] Tesla K80 as the process wide default device.
-------------------------------------------------------------------
Build info:

        Built time: Jan 31 2018 15:03:41
        Last modified date: Tue Jan 30 03:26:13 2018
        Build type: release
        Build target: GPU
        With 1bit-SGD: no
        With ASGD: yes
        Math lib: mkl
        CUDA version: 9.0.0
        CUDNN version: 7.0.4
        Build Branch: HEAD
        Build SHA1: a70455c7abe76596853f8e6a77a4d6de1e3ba76e
        MPI distribution: Open MPI
        MPI version: 1.10.7
-------------------------------------------------------------------
Training 98778 parameters in 10 parameter tensors.

Learning rate per 1 samples: 0.001
Momentum per 1 samples: 0.0
Finished Epoch[1 of 40]: [Training] loss = 0.405960 * 60000, metric = 13.01% * 60000 21.741s (2759.8 samples/s);
Finished Epoch[2 of 40]: [Training] loss = 0.106030 * 60000, metric = 3.09% * 60000 3.638s (16492.6 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.078542 * 60000, metric = 2.32% * 60000 3.477s (17256.3 samples/s);
...
Final Results: Minibatch[1-11]: errs = 0.62% * 10000
```

Het streamen stopt wanneer de taak is voltooid (geslaagd of mislukt).

## <a name="inspect-generated-model-files"></a>Gegenereerde modelbestanden controleren

De uitvoer van de taak bestaat uit modelbestanden in de uitvoermap met het kenmerk `id` ingesteld op `MODEL`. Gebruik de volgende opdracht om de modelbestanden en download-URL's weer te geven:

```azurecli
az batchai job file list -j cntk_python_1 -w quickstart -e quickstart -g batchai.quickstart -d MODEL
```

Voorbeelduitvoer:
```
[
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:51+00:00",
    "name": "ConvNet_MNIST_0.dnn"
  },
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:55+00:00",
    "name": "ConvNet_MNIST_1.dnn"
  },
...

```

U kunt ook Azure Portal of Azure Storage Explorer gebruiken om de gegenereerde bestanden te controleren. Om de uitvoer van de verschillende taken te onderscheiden, maakt Batch AI een unieke mapstructuur voor elke taak. U vindt het pad naar de map met de uitvoer aan de hand van het kenmerk `jobOutputDirectoryPathSegment` van de verzonden taak:

```azurecli
az batchai job show -n cntk_python_1 -g batchai.quickstart -w quickstart -e quickstart --query jobOutputDirectoryPathSegment
```

Voorbeelduitvoer:
```
"00000000-0000-0000-0000-000000000000/batchai.quickstart/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1/<JOB's UUID>"
```

## <a name="clean-up-resources"></a>Resources opschonen

Voer de volgende opdracht uit om de resourcegroep en alle toegewezen resources te verwijderen:

```azurecli
az group delete -n batchai.quickstart -y
```
