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
ms.date: 10/06/2017
ms.author: Alexander.Yukhanov
ms.openlocfilehash: 82e3885021a2f2309dfed456d472e7027b8d6cf2
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Een CNTK-trainingstaak uitvoeren met behulp van Azure CLI

In deze snelstart wordt gedetailleerd beschreven hoe u de Azure-opdrachtregelinterface (CLI) gebruikt om een CNTK-trainingstaak (Microsoft Cognitive Toolkit) uit te voeren met behulp van de Batch AI-service. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts.

In dit voorbeeld gebruikt u de MNIST-database met handgeschreven installatiekopieën om een CNN (Convolutional Neural Network) te trainen op een GPU-cluster met één knooppunt dat wordt beheerd met Batch AI. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze snelstart moet u de nieuwste versie van Azure CLI uitvoeren. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

Ook moeten de Batch AI-resourceproviders één keer worden geregistreerd voor uw abonnement met behulp van Azure Cloud Shell of Azure CLI. Het registreren van een provider duurt maximaal 15 minuten.

```azurecli
az provider register -n Microsoft.BatchAI
az provider register -n Microsoft.Batch
```


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Batch AI-clusters en -taken zijn Azure-resources en moeten in een Azure-resourcegroep worden geplaatst.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create).

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*. Vervolgens wordt de opdracht [az configure](/cli/azure/reference-index#az_configure) gebruikt om deze resourcegroep en locatie in te stellen als de standaard.

```azurecli
az group create --name myResourceGroup --location eastus

az configure --defaults group=myResourceGroup

az configure --defaults location=eastus
```

>[!NOTE]
>Instellen van de standaardwaarden voor de opdracht `az` is een optionele stap. U kunt ervoor kiezen om de standaardwaarden niet in te stellen. Als u ervoor kiest om de standaardinstellingen wel in te stellen, moet u deze verwijderen nadat u de zelfstudie hebt voltooid. U verwijdert de standaardinstellingen met behulp van de volgende opdrachten:
>
>```azurecli
>az configure --defaults group=''
>
>az configure --defaults location=''
>```
>

## <a name="create-a-storage-account"></a>Een opslagaccount maken

In deze snelstart wordt gebruikgemaakt van een Azure-opslagaccount om gegevens en scripts voor de trainingstaak te hosten. Maak een opslagaccount met de opdracht [az storage account create](/cli/azure/storage/account#az_storage_account_create).

```azurecli
az storage account create --name mystorageaccount --sku Standard_LRS
```

>[!NOTE]
>Elk opslagaccount moet een unieke naam hebben. Vervang in de vorige opdracht `az` en in andere vergelijkbare opdrachten in deze zelfstudie de waarde bij de instelling voor `mystorageaccount` door de naam van uw opslagaccount.

## <a name="prepare-azure-file-share"></a>Azure-bestandsshare voorbereiden

Ter illustratie wordt in deze snelstart een Azure-bestandsshare gebruikt om de trainingsgegevens en -scripts voor de Learning-taak te hosten.

1. Maak een bestandsshare met de naam *batchaiquickstart* met behulp van de opdracht [az storage share create](/cli/azure/storage/share#az_storage_share_create).

  ```azurecli
  az storage share create --account-name mystorageaccount --name batchaiquickstart
  ```
2. Maak een map in de share met de naam *mnistcntksample* met behulp van de opdracht [az storage directory create](/cli/azure/storage/directory#az_storage_directory_create).

  ```azurecli
  az storage directory create --share-name batchaiquickstart  --name mnistcntksample
  ```

3. Download het [voorbeeldpakket](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) en pak dit uit. Upload de inhoud naar de map met behulp van de opdracht [az storage file upload](/cli/azure/storage/file#az_storage_file_upload):

  ```azurecli
  az storage file upload --share-name batchaiquickstart --source Train-28x28_cntk_text.txt --path mnistcntksample

  az storage file upload --share-name batchaiquickstart --source Test-28x28_cntk_text.txt --path mnistcntksample

  az storage file upload --share-name batchaiquickstart --source ConvNet_MNIST.py --path mnistcntksample
  ```


## <a name="create-gpu-cluster"></a>GPU-cluster maken
Gebruik de opdracht [az batchai cluster create](/cli/azure/batchai/cluster#az_batchai_cluster_create) om een Batch AI-cluster te maken met een enkel GPU VM-knooppunt. In dit voorbeeld wordt de Ubuntu LTS-standaardinstallatiekopie uitgevoerd op de VM. Geef in plaats hiervan `image UbuntuDSVM` op om Microsoft Deep Learning Virtual Machine uit te voeren die ondersteuning biedt voor alle trainingsframeworks. The NC6-grootte heeft één NVIDIA K80-GPU. Koppel de bestandsshare aan een map met de naam *azurefileshare*. Het volledige pad van deze map op het GPU-rekenknooppunt is $AZ_BATCHAI_MOUNT_ROOT/azurefileshare.


```azurecli
az batchai cluster create --name mycluster --vm-size STANDARD_NC6 --image UbuntuLTS --min 1 --max 1 --storage-account-name mystorageaccount --afs-name batchaiquickstart --afs-mount-path azurefileshare --user-name <admin_username> --password <admin_password>
```


Nadat het cluster is gemaakt, lijkt de uitvoer op het volgende:

```azurecli
{
  "allocationState": "resizing",
  "allocationStateTransitionTime": "2017-10-05T02:09:03.194000+00:00",
  "creationTime": "2017-10-05T02:09:01.998000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.BatchAI/clusters/mycluster",
  "location": "eastus",
  "name": "mycluster",
  "nodeSetup": {
    "mountVolumes": {
      "azureBlobFileSystems": null,
      "azureFileShares": [
        {
          "accountName": "batchaisamples",
          "azureFileUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart",
          "credentialsInfo": {
            "accountKey": null,
            "accountKeySecretUrl": null
          },
          "directoryMode": "0777",
          "fileMode": "0777",
          "relativeMountPath": "azurefileshare"
        }
      ],
      "fileServers": null,
      "unmanagedFileSystems": null
    },
    "setupTask": null
  },
  "nodeStateCounts": {
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2017-10-05T02:09:02.857000+00:00",
  "resourceGroup": "myresourcegroup",
  "scaleSettings": {
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": {
    "id": null
  },
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "adminUserName": "demoUser",
    "adminUserPassword": null,
    "adminUserSshPublicKey": null
  },
  "virtualMachineConfiguration": {
    "imageReference": {
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04-LTS",
      "version": "latest"
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
```
## <a name="get-cluster-status"></a>Clusterstatus ophalen

Voor een overzicht van de clusterstatus, voert u de opdracht [az batchai cluster list](/cli/azure/batchai/cluster#az_batchai_cluster_list) uit:

```azurecli
az batchai cluster list -o table
```

De uitvoer lijkt op het volgende:

```azurecli
Name        Resource Group    VM Size        State     Idle    Running    Preparing    Unusable    Leaving
---------   ----------------  -------------  -------   ------  ---------  -----------  ----------  --------
mycluster   myresourcegroup   STANDARD_NC6   steady    1       0          0            0            0
```

Voor meer details voert u de opdracht [az batchai cluster show](/cli/azure/batchai/cluster#az_batchai_cluster_show) uit. Hiermee worden alle clustereigenschappen geretourneerd die worden weergegeven na het maken van het cluster.

Het cluster is gereed wanneer de knooppunten zijn toegewezen en de voorbereiding is voltooid (zie het kenmerk `nodeStateCounts`). Als er iets is misgegaan, bevat het kenmerk `errors` de foutbeschrijving.

## <a name="create-training-job"></a>Trainingstaak maken

Nadat het cluster gereed is, configureert en verzendt u de Learning-taak.

1. Maak een JSON-sjabloonbestand om een taak te maken met de naam job.json:

  ```JSON
  {
    "properties": {
        "stdOutErrPathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
       "inputDirectories": [{
            "id": "SAMPLE",
            "path": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare/mnistcntksample"
        }],
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
            "pathSuffix": "model",
            "type": "custom"
        }],
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0"
            }
        },
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL"
        }
    }
  }
  ```
2. Maak een taak met de naam *myjob* om uit te voeren op het cluster met behulp van de opdracht [az batchai job create](/cli/azure/batchai/job#az_batchai_job_create):

  ```azurecli
  az batchai job create --name myjob --cluster-name mycluster --config job.json
  ```

De uitvoer lijkt op het volgende:

```azurecli
{
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.BatchAI/clusters/mycluster",
    "resourceGroup": "myresourcegroup"
  },
  "cntkSettings": {
    "commandLineArgs": "$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py"
  },
  "constraints": {
    "maxTaskRetryCount": null,
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": {
    "imageSourceRegistry": {
      "credentials": null,
      "image": "microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0",
      "serverUrl": null
    }
  },
  "creationTime": "2017-10-05T06:41:42.163000+00:00",
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": {
    "endTime": null,
    "errors": null,
    "exitCode": null,
    "lastRetryTime": null,
    "retryCount": null,
    "startTime": "2017-10-05T06:41:44.392000+00:00"
  },
  "executionState": "running",
  "executionStateTransitionTime": "2017-10-05T06:41:44.953000+00:00",
  "experimentName": null,
  "id": "/subscriptions/10d0b7c6-9243-4713-xxxx-xxxxxxxxxxxx/resourceGroups/demo/providers/Microsoft.BatchAI/jobs/myjob",
  "inputDirectories": [
    {
      "id": "SAMPLE",
      "path": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare/mnistcntksample"
    }
  ],
  "jobPreparation": null,
  "location": null,
  "name": "cntk_job",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "createNew": true,
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
      "pathSuffix": "model",
      "type": "Custom"
    }
  ],
  "priority": 0,
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2017-10-05T06:41:44.238000+00:00",
  "resourceGroup": "demo",
  "stdOutErrPathPrefix": "$AZ_BATCHAI_MOUNT_ROOT/azurefileshare",
  "tags": null,
  "tensorFlowSettings": null,
  "toolType": "CNTK",
  "type": "Microsoft.BatchAI/Jobs"
}
```

## <a name="monitor-job"></a>Taak controleren

Gebruik de opdracht [az batchai job list](/cli/azure/batchai/job#az_batchai_job_list) voor een overzicht van de taakstatus:

```azurecli
az batchai job list -o table
```

De uitvoer lijkt op het volgende:

```azurecli
Name        Resource Group    Cluster    Cluster RG      Nodes  State    Exit code
----------  ----------------  ---------  --------------- -----  -------  -----------
myjob       myresourcegroup   mycluster  myresourcegroup 1      running

```

Voer de opdracht [az batchai job show](/cli/azure/batchai/job#az_batchai_job_show) uit voor meer details.

De `executionState` bevat de huidige uitvoeringsstatus van de taak:

* `queued`: de taak wacht tot de clusterknooppunten beschikbaar zijn
* `running`: de taak wordt uitgevoerd
*   `succeeded` (of `failed`): de taak is voltooid en `executionInfo` bevat details over het resultaat


## <a name="list-stdout-and-stderr-output"></a>Stdout- en stderr-uitvoer weergeven
Gebruik de opdracht [az batchai job list-files](/cli/azure/batchai/job#az_batchai_job_list_files) om koppelingen weer te geven naar de stdout- en stderr-logboekbestanden:

```azurecli
az batchai job list-files --name myjob --output-directory-id stdouterr
```

De uitvoer lijkt op het volgende:

```azurecli
[
  {
    "contentLength": 733,
    "downloadUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart/10d0b7c6-9243-4713-91a9-2730375d3a1b/demo/jobs/cntk_job/stderr.txt?sv=2016-05-31&sr=f&sig=Rh%2BuTg9C1yQxm7NfA9YWiKb%2B5FRKqWmEXiGNRDeFMd8%3D&se=2017-10-05T07%3A44%3A38Z&sp=rl",
    "lastModified": "2017-10-05T06:44:38+00:00",
    "name": "stderr.txt"
  },
  {
    "contentLength": 300,
    "downloadUrl": "https://batchaisamples.file.core.windows.net/batchaiquickstart/10d0b7c6-9243-4713-91a9-2730375d3a1b/demo/jobs/cntk_job/stdout.txt?sv=2016-05-31&sr=f&sig=jMhJfQOGry9jr4Hh3YyUFpW5Uaxnp38bhVWNrTTWMtk%3D&se=2017-10-05T07%3A44%3A38Z&sp=rl",
    "lastModified": "2017-10-05T06:44:29+00:00",
    "name": "stdout.txt"
  }
]
```


## <a name="observe-output"></a>Uitvoer observeren

U kunt de uitvoerbestanden van een taak streamen of bijhouden terwijl de taak wordt uitgevoerd. In het volgende voorbeeld wordt de opdracht [az batchai job stream-file](/cli/azure/batchai/job#az_batchai_job_stream_file) gebruikt om het stderr.txt-logboekbestand te streamen:

```azurecli
az batchai job stream-file --job-name myjob --output-directory-id stdouterr --name stderr.txt
```

De uitvoer lijkt op het volgende. Onderbreek de uitvoer door te drukken op [Ctrl]-[C].

```azurecli
…
Finished Epoch[2 of 40]: [Training] loss = 0.104846 * 60000, metric = 3.00% * 60000 3.849s (15588.5 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.077043 * 60000, metric = 2.23% * 60000 3.902s (15376.7 samples/s);
Finished Epoch[4 of 40]: [Training] loss = 0.063050 * 60000, metric = 1.82% * 60000 3.811s (15743.9 samples/s);
…

```

## <a name="delete-resources"></a>Resources verwijderen

Gebruik de opdracht [az batchai job delete](/cli/azure/batchai/job#az_batchai_job_delete) om de taak te verwijderen:

```azurecli
az batchai job delete --name myjob
```
Gebruik de opdracht [az batchai cluster delete](/cli/azure/batchai/cluster#az_batchai_cluster_delete) om het cluster te verwijderen:

```azurecli
az batchai cluster delete --name mycluster
```

Gebruik de opdracht `az group delete` om de resourcegroep te verwijderen die u hebt gemaakt voor deze snelstart:

```azurecli
az group delete --name myResourceGroup
```

## <a name="restore-azure-cli-20-default-settings"></a>Azure CLI 2.0-standaardinstellingen herstellen

Verwijder de eerder geconfigureerde standaardinstellingen voor de locatie en resourcegroep:

```azurecli
az configure --defaults group=''

az configure --defaults location=''
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u geleerd hoe u een CNTK-trainingstaak kunt uitvoeren op een Batch AI-cluster, met behulp van Azure CLI. Zie de [trainingsrecepten](https://github.com/Azure/BatchAI) voor meer informatie over het gebruik van Batch AI met verschillende toolkits.

Zie de [GitHub-documentatie](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md) voor meer informatie over het gebruik van Azure CLI 2.0 voor het beheren van Batch AI.
