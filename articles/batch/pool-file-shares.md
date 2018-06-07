---
title: Azure-bestandsshare voor Azure Batch-pools | Microsoft Docs
description: Het koppelen van een share Azure-bestanden van rekenknooppunten in een Linux- of Windows-toepassingen in Azure Batch.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/24/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 88d7c0d033d7b517a396df27468de8be7ae20be9
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34811727"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Een Azure-bestandsshare gebruiken met een Batch-pool

[Azure Files](../storage/files/storage-files-introduction.md) biedt volledig beheerd bestandsshares in de cloud die toegankelijk zijn via het protocol Server Message Block (SMB). Dit artikel bevat informatie en code voorbeelden voor het koppelen en het gebruik van een Azure-bestandsshare op de rekenknooppunten pool. De codevoorbeelden gebruiken de Batch .NET- en Python SDK's, maar u kunt vergelijkbare bewerkingen met andere Batch-SDK's en hulpprogramma's kunt uitvoeren.

Batch biedt systeemeigen API-ondersteuning voor het gebruik van Azure Storage-blobs te lezen en schrijven van gegevens. In sommige gevallen kunt u echter toegang hebben tot een Azure-bestandsshare van de rekenknooppunten van de pool. Bijvoorbeeld: u hebt een oudere werkbelasting die afhankelijk zijn van een SMB-bestandsshare of uw taken moeten toegang tot gedeelde gegevens of gedeelde uitvoer te produceren. 

## <a name="considerations-for-use-with-batch"></a>Overwegingen voor gebruik met Batch

* Overweeg het gebruik van een Azure-bestandsshare als u groepen met een relatief klein aantal parallelle taken hebt. Controleer de [prestaties en schaalbaarheid doelen](../storage/files/storage-files-scale-targets.md) om te bepalen of Azure-bestanden (die gebruikmaakt van een Azure Storage-account) moet worden gebruikt, krijgt uw verwachte groepsgrootte en het aantal assetbestanden. 

* Azure-bestandsshares zijn [voordelige](https://azure.microsoft.com/pricing/details/storage/files/) en kan worden geconfigureerd met gegevens replicatie naar een andere regio zijn globaal redundant. 

* U kunt een Azure-bestandsshare gelijktijdig koppelen vanuit een on-premises computer.

* Zie ook de algemene [planningsoverwegingen](../storage/files/storage-files-planning.md) voor Azure-bestandsshares.


## <a name="create-a-file-share"></a>Een bestandsshare maken

[Een bestandsshare maken](../storage/files/storage-how-to-create-file-share.md) in een opslagaccount die is gekoppeld aan uw Batch-account of in een afzonderlijke opslagaccount.

## <a name="mount-a-share-on-a-windows-pool"></a>Koppelen van een share op een Windows-groep

Deze sectie vindt u stappen en codevoorbeelden koppelen en gebruiken van een Azure-bestand delen op een pool van Windows-knooppunten. Zie voor aanvullende achtergrondinformatie de [documentatie](../storage/files/storage-how-to-use-files-windows.md) voor het koppelen van een Azure bestandsshare in Windows. 

In Batch moet u de share telkens wanneer een taak wordt uitgevoerd op een Windows-knooppunt koppelen. Het is momenteel niet mogelijk om vast te leggen van de netwerkverbinding tussen de taken op Windows-knooppunten.

Bijvoorbeeld een `net use` opdracht naar de bestandsshare koppelen als onderdeel van de opdrachtregel voor elke taak. De bestandsshare koppelen, zijn de volgende referenties nodig:

* **Gebruikersnaam**: AZURE\\\<storageaccountname\>, bijvoorbeeld AZURE\\*mystorageaccountname*
* **Wachtwoord**: < StorageAccountKeyWhichEnds in == >, bijvoorbeeld *XXXXXXXXXXXXXXXXXXXXX ==*

De volgende opdracht koppelt een bestandsshare *myfileshare* in opslagaccount *mystorageaccountname* als de *S:* station:

```
net use S: \\mystorageaccountname.file.core.windows.net\myfileshare /user:AZURE\mystorageaccountname XXXXXXXXXXXXXXXXXXXXX==
```

Eenvoud, hier in de voorbeelden gebruikmaakt van de referenties in de tekst. In de praktijk, wordt aangeraden het beheren van de referenties met omgevingsvariabelen, certificaten of een oplossing zoals Azure Sleutelkluis.

Om te vereenvoudigen de koppelingsbewerking, moet u eventueel de referenties op de knooppunten bewaard. Vervolgens kunt u de share zonder referenties koppelen. Voer de volgende twee stappen:

1. Voer de `cmdkey` opdrachtregelprogramma met behulp van een begintaak in de configuratie van toepassingen. Het probleem blijft bestaan de referenties op elk knooppunt van Windows. De taak is gestart vanaf de opdrachtregel is vergelijkbaar met:

  ```
  cmd /c "cmdkey /add:mystorageaccountname.file.core.windows.net /user:AZURE\mystorageaccountname /pass:XXXXXXXXXXXXXXXXXXXXX=="

  ```

2. De share te koppelen op elk knooppunt als onderdeel van het gebruik van elke taak `net use`. De volgende opdrachtregel voor de taak koppelt u de bestandsshare als de *S:* station. Dit zou worden gevolgd door een opdracht of script dat verwijst naar de share. In de cache opgeslagen referenties worden gebruikt in de aanroep naar `net use`. Deze stap wordt ervan uitgegaan dat u gebruikt de dezelfde gebruikers-id voor de taken die u in de begintaak voor de toepassingen die geschikt is voor alle scenario's geen gebruikt.

  ```
  cmd /c "net use S: \\mystorageaccountname.file.core.windows.net\myfileshare" 
  ```

### <a name="c-example"></a>C#-voorbeeld
De volgende C#-voorbeeld laat zien hoe de referenties op een Windows-toepassingen met behulp van een begintaak persistent. De bestandsnaam van opslag voor service en storage-referenties worden doorgegeven als gedefinieerde constanten. De begintaak wordt hier uitgevoerd onder een standaard (niet-administrator) automatisch-gebruikersaccount met een bereik van de groep van toepassingen.

```csharp
...
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: PoolId,
    targetDedicatedComputeNodes: PoolNodeCount,
    virtualMachineSize: PoolVMSize,
    virtualMachineConfiguration: virtualMachineConfiguration);

// Start task to store credentials to mount file share
string startTaskCommandLine = String.Format("cmd /c \"cmdkey /add:{0} /user:AZURE\\{1} /pass:{2}\"", StorageFileService, StorageAccountName, StorageAccountKey);

pool.StartTask = new StartTask
{
    CommandLine = startTaskCommandLine,
    UserIdentity = new UserIdentity(new AutoUserSpecification(
        elevationLevel: ElevationLevel.NonAdmin, 
        scope: AutoUserScope.Pool))
};

pool.Commit();
```

Nadat de referenties op te slaan, uw taak opdrachtregels te gebruiken om te koppelen van de share en verwijzen naar de share in lezen of schrijfbewerkingen. Als een eenvoudige voorbeeld de opdrachtregel voor de taak in het volgende fragment maakt gebruik van de `dir` opdracht weergeven van bestanden in de bestandsshare. Voer elke projecttaak met behulp van dezelfde [gebruikersidentiteit](batch-user-accounts.md) u gebruikt voor het uitvoeren van de begintaak in de groep. 

```csharp
...
string taskId = "myTask";
string taskCommandLine = String.Format("cmd /c \"net use {0} {1} & dir {2}\"", ShareMountPoint, StorageFileShare, ShareMountPoint);

CloudTask task = new CloudTask(taskId, taskCommandLine);
task.UserIdentity = new UserIdentity(new AutoUserSpecification(
    elevationLevel: ElevationLevel.NonAdmin,
    scope: AutoUserScope.Pool));
tasks.Add(task);
```

## <a name="mount-a-share-on-a-linux-pool"></a>Koppelen van een share op een Linux-groep

Azure-bestandsshares kunnen worden gekoppeld in de Linux-distributies met behulp van de [CIFS kernelclient](https://wiki.samba.org/index.php/LinuxCIFS). Het volgende voorbeeld laat zien hoe een bestandsshare op een pool van rekenknooppunten Ubuntu 16.04 TNS te koppelen. Als u een ander Linux-distributiepunt gebruikt, wordt de algemene stappen zijn vergelijkbaar, maar de package manager geschikt is voor de distributie gebruiken. Zie voor meer informatie en aanvullende voorbeelden gegeven [gebruik Azure-bestanden met Linux](../storage/files/storage-how-to-use-files-linux.md).

Installeer eerst onder een identiteit van de gebruiker administrator de `cifs-utils` , en het koppelpunt maken (bijvoorbeeld */mnt/MyAzureFileShare*) in het lokale bestandssysteem. Een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar het is algemene conventie voor het maken van dit onder de `/mnt` map. Zorg ervoor dat geen te maken van een koppelpunt rechtstreeks op `/mnt` (op Ubuntu) of `/mnt/resource` (op andere distributies).

```
apt-get update && apt-get install cifs-utils && sudo mkdir -p /mnt/MyAzureFileShare
```

Voer vervolgens de `mount` opdracht om te koppelen van de bestandsshare deze referenties opgeeft:

* **Gebruikersnaam**: \<storageaccountname\>, bijvoorbeeld *mystorageaccountname*
* **Wachtwoord**: < StorageAccountKeyWhichEnds in == >, bijvoorbeeld *XXXXXXXXXXXXXXXXXXXXX ==*

De volgende opdracht koppelt een bestandsshare *myfileshare* in opslagaccount *mystorageaccountname* op */mnt/MyAzureFileShare*: 

```
mount -t cifs //mystorageaccountname.file.core.windows.net/myfileshare /mnt/MyAzureFileShare -o vers=3.0,username=mystorageaccountname,password=XXXXXXXXXXXXXXXXXXXXX==,dir_mode=0777,file_mode=0777,serverino && ls /mnt/MyAzureFileShare
```

Eenvoud, hier in de voorbeelden gebruikmaakt van de referenties in de tekst. In de praktijk, wordt aangeraden het beheren van de referenties met omgevingsvariabelen, certificaten of een oplossing zoals Azure Sleutelkluis.

U kunt op een Linux-groep, Combineer alle volgende stappen uit in een enkel begintaak of in een script worden uitgevoerd. De begintaak uitgevoerd als een beheerder op de groep. Stel uw begintaak moet worden gewacht worden voltooid voordat u verdere taken op de groep die verwijzen naar de share.

### <a name="python-example"></a>Python-voorbeeld

Het volgende Python-voorbeeld laat zien hoe een Ubuntu-toepassingen om te koppelen van de share in een begintaak configureren. Het koppelpunt, eindpunt van de share en storage-referenties worden doorgegeven als gedefinieerde constanten. De begintaak wordt uitgevoerd onder een beheerdersaccount voor de automatische-gebruiker met een bereik van de groep van toepassingen.

```python
pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration = batchmodels.VirtualMachineConfiguration(
        image_reference = batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="16.04.0-LTS",
            version="latest"),
        node_agent_sku_id = "batch.node.ubuntu 16.04"),
    vm_size=_POOL_VM_SIZE,
    target_dedicated_nodes=_POOL_NODE_COUNT,
    start_task=batchmodels.StartTask(
        command_line="/bin/bash -c \"apt-get update && apt-get install cifs-utils && mkdir -p {} && mount -t cifs {} {} -o vers=3.0,username={},password={},dir_mode=0777,file_mode=0777,serverino\"".format(_COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_SHARE_ENDPOINT, _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_NAME, _STORAGE_ACCOUNT_KEY),
        wait_for_success=True,
        user_identity=batchmodels.UserIdentity(
            auto_user=batchmodels.AutoUserSpecification(
                scope=batchmodels.AutoUserScope.pool,
                elevation_level=batchmodels.ElevationLevel.admin)),
    )
)
batch_service_client.pool.add(pool)
```

Na het koppelen van de share en het definiëren van een taak, de share in uw taak opdrachtregels te gebruiken. De volgende eenvoudige opdracht wordt bijvoorbeeld `ls` weergeven van bestanden in de bestandsshare.

```python
...
task = batch.models.TaskAddParameter(
    id='mytask',
    command_line="/bin/bash -c \"ls {}\"".format(_COMPUTE_NODE_MOUNT_POINT))

batch_service_client.task.add(job_id, task)
```


## <a name="next-steps"></a>Volgende stappen

* Zie voor andere opties om te lezen en schrijven in Batch de [overzicht Batch-functies](batch-api-basics.md) en [behouden van de taak en uitvoer](batch-task-output.md).

* Zie ook de [Batch scheepswerf](https://github.com/Azure/batch-shipyard) toolkit, waaronder [scheepswerf recepten](https://github.com/Azure/batch-shipyard/tree/master/recipes) bestandssystemen voor Batch-workloads van container implementeren.