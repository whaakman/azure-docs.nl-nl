---
title: Taak voor het opslaan van Batch AI-invoer en uitvoer met Azure Storage | Microsoft Docs
description: Azure Storage gebruiken met Batch AI voor snelle en eenvoudige cloudopslag van invoer en uitvoer bestanden
services: batch-ai
documentationcenter: ''
author: kevwang1
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 42697f7f4bb8c6b9ef785eef0fe2f5f33b2b38a7
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615600"
---
# <a name="store-batch-ai-job-input-and-output-with-azure-storage"></a>Batch AI-Taakinvoer en uitvoer met Azure Storage Store

Deze handleiding wordt beschreven hoe u Azure Storage gebruiken voor het opslaan van invoer en uitvoer bestanden wanneer een taak wordt uitgevoerd. Azure Storage is een van de verschillende opties voor opslag wordt ondersteund door Batch AI. Batch AI kan worden geïntegreerd met Azure Storage door het koppelen van Azure Storage-systemen om een Batch AI-taak of het cluster bestandssysteem, waardoor naadloze toegang tot bestanden die zijn opgeslagen in de cloud. 

## <a name="introduction-to-azure-storage"></a>Kennismaking met Azure Storage

Azure Storage is Microsoft's oplossing voor opslag in de cloud. Batch AI biedt ondersteuning voor het koppelen van Azure Blob-containers en Azure-bestandsshares aan Batch AI-taken of clusters, waarmee bestanden worden geopend vanaf een taak alsof ze zich in de systeemeigen bestandssysteem. Batch AI koppelt een Azure Blob-containers met [blobfuse](https://github.com/Azure/azure-storage-fuse), en Azure-bestandsshares via het SMB-protocol. Zie voor meer informatie over Azure Storage [Inleiding tot Azure Storage](../storage/common/storage-introduction.md).

## <a name="store-datasets-and-input-scripts-in-azure-storage"></a>Store-gegevenssets en invoer-scripts in Azure Storage

Als u Azure Storage voor uw omgeving Batch AI kiest, wordt aangeraden dat u uw invoerbestanden (zoals gegevenssets) opslaan in een Blob-container met hogere doorvoer en u uw training-uitvoer opslaan in een bestandsshare, die ondersteuning biedt voor streaming (zodat lezen van logboeken met uitvoergegevens terwijl de taak wordt gelijktijdig uitgevoerd). 

Voordat u Azure Storage gebruiken kunt, moet u [maken van een Azure Storage-account](../storage/common/storage-quickstart-create-account.md). Batch AI biedt ondersteuning voor de volumes koppelen van beide voor algemeen gebruik v1 (GPv1) en algemeen gebruik v2 (GPv2) Azure Storage-accounts. De Azure Storage-account kan meerdere Blob-containers houdt of instanties van de bestandsshare-bestand. Houd rekening met de vereisten van uw kosten en prestaties bij het kiezen van het type van de storage-account te maken. Zie [Overzicht van Azure-opslagaccount](../storage/common/storage-account-overview.md) voor meer informatie. 

Een Blob-container maken en uploaden van uw gegevensset naar een Azure Blob-container, kies een van de volgende methoden:
- [Azure-portal](../storage/blobs/storage-quickstart-blobs-portal.md) voor het uploaden van met een web gebaseerde gebruikersinterface geopend. Als u wilt een klein aantal bestanden uploaden, biedt Azure portal de eenvoudigste bewerking.
- [Azure Storage-CLI](../storage/blobs/storage-quickstart-blobs-cli.md) voor het uploaden via de opdrachtregel (uploaden ondersteunt directory). Gebruik om te uploaden mappen van bestanden, `az storage blob upload-batch`.
- [andere technieken](../storage/common/storage-moving-data.md), inclusief het gebruik van application SDK's.

Op dezelfde manier voor het maken van een Azure-bestandsshare, kies een van de volgende methoden:
- [Azure Portal](../storage/files/storage-how-to-use-files-portal.md)
- [Azure CLI-opslag](../storage/files/storage-how-to-use-files-cli.md)
- [andere technieken](../storage/common/storage-moving-data.md)

### <a name="auto-storage-with-batch-ai"></a>Automatisch opslaan met Batch AI

U kunt ook een Azure Storage-account maken met een Azure-bestandsshare en Blob-container (en automatisch koppelen van deze volumes in een Batch AI-cluster) met behulp van de `--use-auto-storage` parameter met de `az batchai cluster create`. Zie voor meer informatie, [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#auto-storage-account).

## <a name="mount-azure-storage"></a>Azure Storage koppelen 

### <a name="mount-volumes-to-a-job"></a>Koppelen van volumes aan een taak

Koppelen van een Azure Storage-volume kan worden geopend via het bestandssysteem gemaakt voor elke taak. Een taak kan daarom lezen en schrijven van bestanden naadloos cloudopslag alsof het lokale bestanden.

Voor het koppelen van een Azure Storage-volume aan een job gemaakt met de Azure CLI, gebruikt u de `mountVolumes` eigenschap in uw `job.json` bij het uitvoeren van het bestand `az batchai job create`. Zie voor een voorbeeld: de [Tensorflow GPU gedistribueerd recept](https://github.com/Azure/BatchAI/blob/master/recipes/TensorFlow/TensorFlow-GPU-Distributed/job.json). Het schema voor `mountVolumes` is:
```json
{
    "mountVolumes": {
        "azureFileShares": [{
            "azureFileUrl": "https://<STORAGE_ACCOUNT_NAME>.file.core.windows.net/<FILE_SHARE_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }],
        "azureBlobFileSystems": [{
            "accountName": "<STORAGE_ACCOUNT_NAME>",
            "containerName": "<BLOB_CONTAINER_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }]
    }
}
```

`azureFileShares` en `azureBlobFileSystems` zijn beide matrices van objecten die staan voor de volumes te koppelen. Beschrijvingen van de tijdelijke aanduidingen:

- < RELATIVE_MOUNT_PATH > - het volume wordt gekoppeld aan dit pad. Bijvoorbeeld, als `relativeMountPath` is `jobs`, het volume worden geplaatst op `$AZ_BATCHAI_JOB_MOUNT_ROOT/jobs`)
- < naam_opslagaccount > - de naam van de Azure Storage-account waarin de bestandsshare of de Blob-container
- < FILE_SHARE_NAME - > de naam van de bestandsshare
- < BLOB_CONTAINER_NAME - > de naam van de Blob-container

Voor het koppelen van Azure Storage-volumes met de Azure Batch AI SDK's, stel de `mount_volumes` (Python) of `MountVolumes` (C#, Java) op de eigenschap `JobCreateParameters`. Tijdens het koppelen van volumes met Azure Batch AI SDK's, moet u de storage-account referenties opgeven. De schema's voor het koppelen van volumes in weergeven [Python](https://docs.microsoft.com/python/api/azure-mgmt-batchai/azure.mgmt.batchai.models.MountVolumes?view=azure-python), [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.batchai.models.mountvolumes?view=azure-dotnet), en [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.management.batchai._mount_volumes?view=azure-java-stable).

### <a name="mount-volumes-to-a-cluster"></a>Koppelen van volumes in een cluster

Batch AI biedt ook ondersteuning voor het koppelen van Azure Storage-volumes in een Batch AI-cluster. Wanneer een volume is gekoppeld aan een cluster, kunnen alle taken die worden uitgevoerd op dat cluster volumes die zijn gekoppeld aan het cluster gebruikt. Tijdens het koppelen van de taak op serverniveau biedt de meeste flexibiliteit (zodat elke taak hebben verschillende gekoppelde volumes), is het mogelijk dat niveau van het cluster koppelen voldoende voor het eenvoudige scenario's.

Voor het koppelen van een Azure Storage-volume naar een cluster gemaakt met de Azure CLI, gebruikt u de `mountVolumes` eigenschap in uw `cluster.json` bij het uitvoeren van het bestand `az batchai cluster create`. Het schema voor `mountVolumes` wanneer het koppelen van een cluster is hetzelfde als bij het koppelen aan een taak. 

Op deze manier kunt u de `mount_volumes` (Python) of `MountVolumes` (C#, Java) op de eigenschap `ClusterCreateParameters` tijdens het koppelen met de Azure Batch AI SDK's. 

## <a name="access-mounted-filesystem-in-a-job"></a>Toegang tot het gekoppelde bestandssysteem in een taak

### <a name="azbatchaijobmountroot-environment-variable"></a>$AZ_BATCHAI_JOB_MOUNT_ROOT-omgevingsvariabele

In de uitvoeringsomgeving van de taak, de map met de gekoppelde storage-systemen kan worden geopend met de `$AZ_BATCHAI_JOB_MOUNT_ROOT` omgevingsvariabele (als u koppelen van de taak op serverniveau). Als u koppelen van de cluster-niveau, deze omgevingsvariabele is `$AZ_BATCHAI_MOUNT_ROOT`. De volgende voorbeelden wordt ervan uitgegaan dat u taakniveau koppelen.

Voor het pad van de gegevens in een gekoppeld volume, gebruikt u de omgevingsvariabele `$AZ_BATCHAI_JOB_MOUNT_ROOT` samen met de gekoppelde pad. Bijvoorbeeld als de trainingsscript `train.py` daadwerkelijk is geüpload naar een Azure File share gekoppeld op relatief koppelingspad `scripts`, het bestand is beschikbaar op `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py`.

Als uw trainingsscript kennis van een pad vereist, moet u deze als een opdrachtregelargument doorgeven. Bijvoorbeeld, als u uw gegevens opgeslagen in een map met de naam `train_data` gekoppeld in een Azure Blob-container op pad `data`, kunt u doorgeven `--data-dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/data/train_data` als een opdrachtregelargument voor uw script. Daarom moet u het script voor het accepteren van opdrachtregelargumenten wijzigen.

### <a name="abbreviate-input-paths"></a>Invoerpaden afkorten

Als u wilt ook invoerpaden opgeven als een omgevingsvariabele, gebruikt u de `inputDirectories` eigenschap van uw `job.json` bestand (of `models.JobCreateParameters.input_directories` als met de Batch AI SDK). Het schema van `inputDirectories` is:

```json
{
    "inputDirectories": [{
        "id": "<ID>",
        "path": "<PATH>" 
    }]
}
```

Elk pad opgegeven worden geplaatst in een omgevingsvariabele met de naam `$AZ_BATCHAI_INPUT_<ID>`. Met deze methode kan de paden voor het invoeren van bestanden of mappen vereenvoudigen. Als u bijvoorbeeld op het pad naar een trainingsscript afkorten: als `"id"` is `"SCRIPT"` en `"path"` is `"$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py"`, en vervolgens het opgegeven pad is beschikbaar op `$AZ_BATCHAI_INPUT_SCRIPT` in de uitvoeringsomgeving van de taak.

Zie voor meer informatie, [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#input-directories).

### <a name="abbreviate-output-paths"></a>Uitvoer paden afkorten

Als u wilt ook uitvoer paden opgeven als een omgevingsvariabele, gebruikt u de `outputDirectories` eigenschap van uw `job.json` bestand (of `models.JobCreateParameters.output_directories` als met de Batch AI SDK). Met deze methode kan de paden voor de uitvoerbestanden vereenvoudigen. Het schema van `outputDirectories` is:

```json
{
    "outputDirectories": [{
        "id": "<ID>",
        "pathPrefix": "<PATH_PREFIX>",
        "pathSuffix": "<PATH_SUFFIX>"
    }]
}
```

Elk pad opgegeven worden geplaatst in een omgevingsvariabele met de naam `$AZ_BATCHAI_OUTPUT_<ID>`. De `pathPrefix` bepaalt het gekoppelde volume voor het opslaan van de uitvoer (bijvoorbeeld `"$AZ_BATCHAI_JOB_MOUNT_ROOT/output"`). De `pathSuffix` bepaalt de naam van de map van de uitvoer (bijvoorbeeld `"logs"`, `"checkpoints"`). Het werkelijke pad van de uitvoer is een samenvoeging van `pathPrefix`, `jobOutputDirectoryPathSegment` (automatisch gegenereerde voor elke taak) en `pathSuffix`.

Zie voor meer informatie, [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#output-directories).

## <a name="retrieve-job-output-from-azure-storage"></a>Taakuitvoer uit Azure Storage ophalen

### <a name="use-azure-portal"></a>Azure Portal gebruiken

De Azure portal is een handige manier om de uitvoer van taken met een GUI-Verkenner weer te geven. Echter, als u wilt weergeven van de uitvoer van Stdout en Stderr, of van een pad in `outputDirectories`, de bestanden worden geplaatst in een automatisch gegenereerde pad in uw Azure Storage-volume. Kijk hieronder voor meer informatie.

### <a name="access-stdout-and-stderr-output"></a>Toegang tot Stdout en Stderr-uitvoer

Gebruik de `stdOutErrPathPrefix` eigenschap van `job.json` vertellen dat de taak waar plaatst u de logboekbestanden voor het uitvoeren van de taak en de Stdout en Stderr-uitvoer. Bijvoorbeeld, als u een bestandsshare op relatief koppelingspad gekoppeld `outputs`, en u geeft de `stdOutErrPathPrefix` moet `"$AZ_BATCHAI_JOB_MOUNT_ROOT/outputs"`, wordt de taakuitvoer van Stdout en Stderr beschikbaar op `{subscription id}/{resource group}/workspaces/{workspace name}/experiments/{experiment name}/jobs/{job name}/{job uuid}/stdouterr` in die gekoppeld volume. Dit pad automatisch gegenereerde wordt gebruikt om te voorkomen dat uitvoer conflicten tussen taken met dezelfde naam.

Zie voor meer informatie, [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#standard-and-error-output).

### <a name="list-the-output-files"></a>Lijst van de uitvoerbestanden

U kunt de Azure CLI gebruiken om de beschikbare uitvoerbestanden van een taak weer te geven de `az batchai job file list` opdracht. Bijvoorbeeld, u kunt de bestanden in de map met de standaard uitvoer van een taak gebruiken `az batchai job file list -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME>`.

Zie voor meer informatie en voorbeelden, [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

### <a name="stream-output-files"></a>Stream-uitvoerbestanden

U kunt de Azure CLI gebruiken voor stream-bestanden met de `az batchai job file stream` opdracht. Als u bijvoorbeeld om weer te geven:
- STDOUT: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stdout.txt`
- Stderr zijn: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stderr.txt`

Zie voor meer informatie en voorbeelden, [hier](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over de CLI-opdrachten waarmee de interface met Azure Storage, geven de [documentatie van Azure Batch AI CLI](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md).
- Ga voor meer voorbeelden van het gebruik van Batch AI, met inbegrip van opslag koppelen en lezen van de uitvoerbestanden, Zie de [Jupyter-Notebook recepten voor Batch AI](https://github.com/Azure/BatchAI).
- Verken andere opties voor het koppelen van opslag, met inbegrip van [koppelen van een NFS-server](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs) en [koppelen van uw eigen cluster NFS, cifs of GlusterFS](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs)