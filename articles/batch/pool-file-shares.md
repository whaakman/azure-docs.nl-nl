---
title: Azure file share voor een Azure Batch-pools | Microsoft Docs
description: Klik hier voor meer informatie over het koppelen van een Azure Files-share van rekenknooppunten in een Linux- of Windows-groep in Azure Batch.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/24/2018
ms.author: lahugh
ms.custom: ''
ms.openlocfilehash: 13ed2caa5ae547747707c368246ea23486dbed72
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469563"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Een Azure-bestandsshare aan een Batch-pool gebruiken

[Azure Files](../storage/files/storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het protocol Server Message Block (SMB). Dit artikel bevat informatie en codevoorbeelden voor koppelen en gebruiken van een Azure-bestandsshare op de rekenknooppunten pool. De codevoorbeelden gebruiken de Batch .NET en Python-SDK's, maar u kunt vergelijkbare bewerkingen met behulp van andere Batch-SDK's en hulpprogramma's uitvoeren.

Batch biedt systeemeigen API-ondersteuning voor het gebruik van Azure Storage-blobs te lezen en schrijven van gegevens. In sommige gevallen kunt u echter toegang krijgen tot een Azure-bestandsshare uit de pool-compute-knooppunten. Bijvoorbeeld, u hebt een verouderde werkbelasting die is gebouwd op een SMB-bestandsshare of uw taken moeten toegang tot gedeelde gegevens of gedeelde uitvoer produceren. 

## <a name="considerations-for-use-with-batch"></a>Overwegingen voor gebruik met Batch

* Overweeg het gebruik van een Azure-bestandsshare als u pools met een relatief klein aantal parallelle taken. Controleer de [prestaties en schaal doelen](../storage/files/storage-files-scale-targets.md) om te bepalen als Azure-bestanden (die gebruikmaakt van een Azure Storage-account) moet worden gebruikt, de grootte van de verwachte en het aantal assetbestanden gegeven. 

* Azure-bestandsshares zijn [betaalbare](https://azure.microsoft.com/pricing/details/storage/files/) en kunnen worden geconfigureerd met gegevens replicatie naar een andere regio zijn wereldwijd redundante. 

* U kunt tegelijkertijd een Azure-bestandsshare koppelen vanuit een on-premises computer.

* Zie ook de algemene [planningsoverwegingen](../storage/files/storage-files-planning.md) voor Azure-bestandsshares.


## <a name="create-a-file-share"></a>Een bestandsshare maken

[Een bestandsshare maken](../storage/files/storage-how-to-create-file-share.md) in een opslagaccount dat is gekoppeld aan uw Batch-account, of in een afzonderlijk opslagaccount.

## <a name="mount-a-share-on-a-windows-pool"></a>Koppelen van een share op een Windows-groep

Deze sectie vindt u stappen en voorbeelden van code om te koppelen en gebruiken van een Azure-bestand delen op een pool van Windows-knooppunten. Zie voor aanvullende achtergrondinformatie de [documentatie](../storage/files/storage-how-to-use-files-windows.md) voor het koppelen van een Azure-bestand delen in Windows. 

In een Batch moet u de bestandsshare telkens wanneer een taak wordt uitgevoerd op een Windows-knooppunt koppelen. Het is momenteel niet mogelijk om vast te leggen van de netwerkverbinding tussen taken op Windows-knooppunten.

Zo bevatten een `net use` opdracht voor het koppelen van de bestandsshare als onderdeel van de opdrachtregel voor elke taak. Als u wilt de bestandsshare koppelt, worden de volgende referenties nodig:

* **Gebruikersnaam**: AZURE\\\<storageaccountname\>, bijvoorbeeld AZURE\\*mystorageaccountname*
* **Wachtwoord**: < StorageAccountKeyWhichEnds in == >, bijvoorbeeld *XXXXXXXXXXXXXXXXXXXXX ==*

De volgende opdracht een bestandsshare koppelt *myfileshare* in storage-account *mystorageaccountname* als de *S:* station:

```
net use S: \\mystorageaccountname.file.core.windows.net\myfileshare /user:AZURE\mystorageaccountname XXXXXXXXXXXXXXXXXXXXX==
```

Voor het gemak doorgeven deze voorbeelden van de referenties rechtstreeks in de tekst. In de praktijk, wordt aangeraden de referenties met behulp van omgevingsvariabelen, certificaten of een oplossing zoals Azure Key Vault beheren.

Ter vereenvoudiging van de koppelingsbewerking (optioneel) de referenties persistent gemaakt op de knooppunten. Vervolgens kunt u de share zonder referenties koppelen. Voer de volgende twee stappen uit:

1. Voer de `cmdkey` opdrachtregelprogramma met behulp van een starttaak in de configuratie van de pool. Het probleem blijft bestaan de referenties op elk Windows-knooppunt. De taak starten vanaf de opdrachtregel is vergelijkbaar met:

  ```
  cmd /c "cmdkey /add:mystorageaccountname.file.core.windows.net /user:AZURE\mystorageaccountname /pass:XXXXXXXXXXXXXXXXXXXXX=="

  ```

2. De bestandsshare koppelen op elk knooppunt als onderdeel van het gebruik van elke taak `net use`. Bijvoorbeeld, de opdrachtregel van de volgende taak koppelt u de bestandsshare als de *S:* station. Dit zou worden gevolgd door een opdracht of script die verwijst naar de share. In de cache opgeslagen referenties worden gebruikt in de aanroep naar `net use`. Deze stap wordt ervan uitgegaan dat u gebruikt de dezelfde gebruikers-id voor de taken die u hebt gebruikt in de begintaak voor de pool, is niet geschikt is voor alle scenario's.

  ```
  cmd /c "net use S: \\mystorageaccountname.file.core.windows.net\myfileshare" 
  ```

### <a name="c-example"></a>C#-voorbeeld
De volgende C# voorbeeld laat zien hoe de referenties op een Windows-toepassingen met behulp van een begintaak persistent maken. De naam van de service opslag-bestand en storage-referenties worden doorgegeven als gedefinieerde constanten. De begintaak wordt hier uitgevoerd onder een standaard (niet-beheerder) automatisch-gebruikersaccount met een bereik van de groep van toepassingen.

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

Gebruik nadat de referenties op te slaan, uw opdrachtregels naar de bestandsshare koppelen en verwijzen naar de share in lezen of schrijven. Als een eenvoudige voorbeeld wordt de opdrachtregel van de taak in het volgende fragment maakt gebruik van de `dir` opdracht aan de lijst met bestanden in de bestandsshare. Zorg ervoor dat u het uitvoeren van de taak van elke taak met behulp van dezelfde [gebruikersidentiteit](batch-user-accounts.md) u gebruikt voor het uitvoeren van de begintaak opgegeven in de groep. 

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

Azure-bestandsshares kunnen worden gekoppeld in Linux-distributies, met de [CIFS kernelclient](https://wiki.samba.org/index.php/LinuxCIFS). Het volgende voorbeeld ziet hoe u een bestandsshare op een pool van rekenknooppunten voor Ubuntu 16.04 LTS koppelen. Als u een andere Linux-distributie, worden de algemene stappen zijn vergelijkbaar, maar de package manager die geschikt is voor de distributie gebruiken. Zie voor meer informatie en extra voorbeelden [Azure Files gebruiken met Linux](../storage/files/storage-how-to-use-files-linux.md).

Installeer eerst onder een gebruikers-id van de beheerder de `cifs-utils` verpakt en maken van het koppelpunt (bijvoorbeeld */mnt/MyAzureFileShare*) in het lokale bestandssysteem. Een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar wel algemene naamgeving om aan te maken onder de `/mnt` map. Zorg ervoor dat geen te maken van een koppelpunt rechtstreeks op `/mnt` (op Ubuntu) of `/mnt/resource` (in een andere distributie).

```
apt-get update && apt-get install cifs-utils && sudo mkdir -p /mnt/MyAzureFileShare
```

Voer de `mount` opdracht voor het koppelen van de bestandsshare, deze referenties op te geven:

* **Gebruikersnaam**: \<storageaccountname\>, bijvoorbeeld *mystorageaccountname*
* **Wachtwoord**: < StorageAccountKeyWhichEnds in == >, bijvoorbeeld *XXXXXXXXXXXXXXXXXXXXX ==*

De volgende opdracht een bestandsshare koppelt *myfileshare* in storage-account *mystorageaccountname* op */mnt/MyAzureFileShare*: 

```
mount -t cifs //mystorageaccountname.file.core.windows.net/myfileshare /mnt/MyAzureFileShare -o vers=3.0,username=mystorageaccountname,password=XXXXXXXXXXXXXXXXXXXXX==,dir_mode=0777,file_mode=0777,serverino && ls /mnt/MyAzureFileShare
```

Voor het gemak doorgeven deze voorbeelden van de referenties rechtstreeks in de tekst. In de praktijk, wordt aangeraden de referenties met behulp van omgevingsvariabelen, certificaten of een oplossing zoals Azure Key Vault beheren.

U kunt op een Linux-groep, Combineer alle volgende stappen uit in een enkele begintaak of voer ze uit in een script. De begintaak uitgevoerd als een beheerder in de pool. De begintaak moet worden gewacht worden voltooid voordat u verdere taken uitvoert in de pool die verwijzen naar de share instellen.

### <a name="python-example"></a>Python-voorbeeld

De volgende Python-voorbeeld laat zien hoe een Ubuntu-adresgroep voor het koppelen van de share in een begintaak configureren. Het koppelpunt, file share eindpunt en storage-referenties worden doorgegeven als gedefinieerde constanten. De begintaak wordt uitgevoerd onder een beheerdersaccount automatisch-gebruiker met een bereik van de groep van toepassingen.

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

Na het koppelen van de share en het definiëren van een taak, de share gebruiken die in uw opdrachtregels. De volgende eenvoudige opdracht gebruikt bijvoorbeeld `ls` bestanden vermelden in de bestandsshare.

```python
...
task = batch.models.TaskAddParameter(
    id='mytask',
    command_line="/bin/bash -c \"ls {}\"".format(_COMPUTE_NODE_MOUNT_POINT))

batch_service_client.task.add(job_id, task)
```


## <a name="next-steps"></a>Volgende stappen

* Zie voor andere opties om te lezen en schrijven van gegevens in Batch de [overzicht van Batch-functies](batch-api-basics.md) en [opdracht- en taakuitvoer behouden](batch-task-output.md).

* Zie ook de [Batch scheepswerf](https://github.com/Azure/batch-shipyard) toolkit, waaronder [scheepswerf recepten](https://github.com/Azure/batch-shipyard/tree/master/recipes) bestandssystemen voor Batch-containerwerkbelastingen te implementeren.