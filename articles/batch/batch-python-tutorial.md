---
title: Zelfstudie - De Azure Batch-SDK voor Python gebruiken | Microsoft Docs
description: Leer de basisconcepten van Azure Batch en bouw een eenvoudige oplossing met behulp van Python.
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: 42cae157-d43d-47f8-88f5-486ccfd334f4
ms.service: batch
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd5a977c10d3955639beb893cd7a37581b14f7c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-batch-sdk-for-python"></a>Aan de slag met de Batch-SDK voor Python

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

Leer de basisbeginselen van [Azure Batch][azure_batch] en de [Batch Python][py_azure_sdk]-client bij de toelichting van een kleine Batch-toepassing die in Python is geschreven. U ontdekt hier hoe twee scripts gebruikmaken van de Batch-service om een parallelle workload op virtuele Linux-machines in de cloud te verwerken, en hoe deze scripts met [Azure Storage](../storage/common/storage-introduction.md) communiceren voor het faseren en ophalen van bestanden. U maakt kennis met een gangbare werkstroom voor Batch-toepassingen en krijgt hier ook elementair inzicht in de belangrijkste onderdelen van Batch, zoals jobs, taken, pools en rekenknooppunten.

![Werkstroom van de Batch-oplossing (basis)][11]<br/>

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u praktische kennis hebt van Python en vertrouwd bent met Linux. Er wordt ook van uitgegaan dat u voldoet aan de vereisten voor het maken van een account die hieronder zijn opgegeven voor Azure en de Batch- en Storage-services.

### <a name="accounts"></a>Accounts
* **Azure-account**: als u nog geen Azure-abonnement hebt, [maakt u een gratis Azure-account][azure_free_account].
* **Batch-account**: als u al een Azure-abonnement hebt, [maakt u een Azure Batch-account](batch-account-create-portal.md).
* **Opslagaccount**: zie [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](../storage/common/storage-create-storage-account.md).

### <a name="code-sample"></a>Codevoorbeeld
Het Python-[codevoorbeeld][github_article_samples] in de zelfstudie is een van de vele Batch-codevoorbeelden die beschikbaar zijn in de opslagplaats [azure-batch-samples][github_samples] in GitHub. U kunt alle voorbeelden downloaden door te klikken op **Clone or download > Download ZIP** (Klonen of downloaden > ZIP downloaden) op de introductiepagina van de opslagplaats of door te klikken op de koppeling van de directe download [azure batch-samples-master.zip][github_samples_zip]. Nadat u de inhoud van het ZIP-bestand hebt uitgepakt, vindt u de twee scripts voor deze zelfstudie in de map `article_samples`:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python-omgeving
Als u het voorbeeldscript *python_tutorial_client.py* op uw lokale werkstation wilt uitvoeren, hebt u een **Python-interpreter** nodig die compatibel is met versie **2.7** of **3.3+**. Het script is getest op Linux en Windows.

### <a name="cryptography-dependencies"></a>cryptografie-afhankelijkheden
U moet de afhankelijkheden voor de [cryptografie][crypto]-bibliotheek installeren die zijn vereist door de `azure-batch`- en `azure-storage`-pakketten voor Python. Voer een van de volgende bewerkingen uit die geschikt is voor uw platform of raadpleeg de [cryptografie-installatie][crypto_install]details voor meer informatie:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* CentOS

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* Windows

    `pip install cryptography`

> [!NOTE]
> Als u voor Python 3.3+ op Linux installeert, gebruikt u de python3-equivalenten voor de Python-afhankelijkheden. Bijvoorbeeld op Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`
>
>

### <a name="azure-packages"></a>Azure-pakketten
Installeer daarna de **Azure Batch**- en **Azure Storage**-pakketten voor Python. U kunt beide pakketten installeren met behulp van **pip** en de *requirements.txt* die u hier kunt vinden:

`/azure-batch-samples/Python/Batch/requirements.txt`

Geef de volgende **pip**-opdracht om de Batch- en Storage-pakketten te installeren:

`pip install -r requirements.txt`

U kunt de [azure batch][pypi_batch]- en [azure-opslag][pypi_storage]-pakketten voor Python ook handmatig installeren:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> Mogelijk moet u aan uw opdrachten het voorvoegsel `sudo` toevoegen als u een niet-gemachtigd account gebruikt. Bijvoorbeeld `sudo pip install -r requirements.txt`. Zie [Installing Packages][pypi_install] (Pakketten installeren) op python.org voor meer informatie over het installeren van Python-pakketten.
>
>

## <a name="batch-python-tutorial-code-sample"></a>Batch-zelfstudiecodevoorbeeld voor Python
Het batch-zelfstudiecodevoorbeeld voor Python bestaat uit twee Python-scripts en enkele gegevensbestanden.

* **python_tutorial_client.py**: Communiceert met de Batch- en Storage-services voor het uitvoeren van een parallelle workload in rekenknooppunten (virtuele machines). Het script *python_tutorial_client.py* wordt uitgevoerd op uw lokale werkstation.
* **python_tutorial_task.py**: Het script dat wordt uitgevoerd op rekenknooppunten in Azure om het echte werk te verrichten. In het voorbeeld parseert *python_tutorial_task.py* de tekst in een bestand dat is gedownload van Azure Storage (het invoerbestand). Daarna creëert het een tekstbestand (het uitvoerbestand) met daarin een lijst van de eerste drie woorden die het invoerbestand bevat. Nadat het het uitvoerbestand heeft gemaakt, uploadt *python_tutorial_task.py* het bestand naar Azure Storage. Hierdoor kan het worden gedownload naar het clientscript dat op uw werkstation wordt uitgevoerd. Het script *python_tutorial_task.py* wordt parallel uitgevoerd in meerdere rekenknooppunten in de Batch-service.
* **./data/taskdata\*.txt**: deze drie tekstbestanden leveren de invoer voor de taken die op de rekenknooppunten worden uitgevoerd.

Het volgende diagram illustreert de primaire bewerkingen die door de client- en taakscripts worden uitgevoerd. Deze basiswerkstroom is typerend voor vele rekenoplossingen die met Batch worden gemaakt. Hoewel het niet elke functie beschikbaar in de Batch-service aantoont, omvat vrijwel elk Batch-scenario delen van deze werkstroom.

![Batch-voorbeeldwerkstroom][8]<br/>

[**Stap 1.**](#step-1-create-storage-containers) Maak **containers** in Azure Blob Storage.<br/>
[**Stap 2.**](#step-2-upload-task-script-and-data-files) Upload taakscript- en invoerbestanden naar containers.<br/>
[**Stap 3.**](#step-3-create-batch-pool) Maak een Batch-**pool**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** **StartTask** van de pool downloadt het taakscript (python_tutorial_task.py) naar knooppunten wanneer deze aan de pool worden toegevoegd.<br/>
[**Stap 4.**](#step-4-create-batch-job) Maak een Batch-**job**.<br/>
[**Stap 5.**](#step-5-add-tasks-to-job) Voeg **taken** toe aan de job.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** De taken worden gepland om op knooppunten te worden uitgevoerd.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Elke taak downloadt de bijbehorende invoergegevens uit Azure Storage en wordt daarna uitgevoerd.<br/>
[**Stap 6.**](#step-6-monitor-tasks) Controleer taken.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Nadat taken zijn voltooid, uploaden zij hun uitvoergegevens naar Azure Storage.<br/>
[**Stap 7.**](#step-7-download-task-output) Download taakuitvoer uit Storage.

Zoals vermeld voert niet elke Batch-oplossing deze exacte stappen uit en kunnen veel meer stappen nodig zijn, maar deze sjabloon toont gebruikelijke processen gevonden in een Batch-oplossing.

## <a name="prepare-client-script"></a>Clientscripts voorbereiden
Voordat u het voorbeeld uitvoert, voegt u de referenties van uw Batch- en Storage-account toe aan *python_tutorial_client.py*. Als u dit nog niet hebt gedaan, opent u het bestand in uw favoriete editor en werkt u de volgende regels bij met uw referenties.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
BATCH_ACCOUNT_NAME = ""
BATCH_ACCOUNT_KEY = ""
BATCH_ACCOUNT_URL = ""

# Storage account credentials
STORAGE_ACCOUNT_NAME = ""
STORAGE_ACCOUNT_KEY = ""
```

U vindt uw Batch- en Storage-accountreferenties op de accountblade van elke service in [Azure Portal][azure_portal]:

![Batch-referenties in de portal][9]
![Storage-referenties in de portal][10]<br/>

In de volgende secties analyseren we de stappen die wordt gebruikt door de scripts voor het verwerken van een workload die in de Batch-service. We raden u aan regelmatig te verwijzen naar de scripts in uw editor wanneer u zich door de rest van het artikel heen werkt.

Navigeer naar de volgende regel in **python_tutorial_client.py** om bij stap 1 te starten:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Stap 1: opslagcontainers maken
![Containers maken in Azure Storage][1]
<br/>

Batch bevat ingebouwde ondersteuning voor interactie met Azure Storage. Containers in uw opslagaccount bevatten de bestanden die nodig zijn voor de taken die in uw Batch-account worden uitgevoerd. De containers bieden ook een plaats voor de opslag van de uitvoergegevens die de taken opleveren. Het eerste wat het script *python_tutorial_client.py* doet, is drie containers in [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage) maken:

* **application**: deze container slaat het Python-script op dat door de taken wordt uitgevoerd, *python_tutorial_task.py*.
* **input**: taken downloaden de te verwerken gegevensbestanden uit de *invoer*container.
* **output**: nadat taken de verwerking van invoerbestanden hebben voltooid, uploaden zij de resultaten naar de *uitvoer* container.

Om te communiceren met een opslagaccount en containers te maken, gebruiken we het pakket [azure-storage][pypi_storage] om een [BlockBlobService][py_blockblobservice]-object te maken, de 'blob-client'. Daarna maken we drie containers in het opslagaccount met behulp van de blob-client.

```python
import azure.storage.blob as azureblob

# Create the blob client, for use in obtaining references to
# blob storage containers and uploading files to containers.
blob_client = azureblob.BlockBlobService(
    account_name=STORAGE_ACCOUNT_NAME,
    account_key=STORAGE_ACCOUNT_KEY)

# Use the blob client to create the containers in Azure Storage if they
# don't yet exist.
APP_CONTAINER_NAME = 'application'
INPUT_CONTAINER_NAME = 'input'
OUTPUT_CONTAINER_NAME = 'output'
blob_client.create_container(APP_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(INPUT_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(OUTPUT_CONTAINER_NAME, fail_on_exist=False)
```

Zodra de containers zijn gemaakt, kan de toepassing nu de bestanden uploaden die door de taken worden gebruikt.

> [!TIP]
> [How to use Azure Blob storage from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) (Azure Blob Storage gebruiken met Python) biedt een uitstekend overzicht van hoe u met Azure Storage-containers en -blobs werkt. Wanneer u met Batch begint te werken, moet dat artikel hoog in uw leeslijst zijn vermeld.
>
>

## <a name="step-2-upload-task-script-and-data-files"></a>Stap 2: taakscript- en gegevensbestanden uploaden
![Taaktoepassings- en invoer(gegevens)bestanden uploaden naar containers][2]
<br/>

In de bewerking voor het uploaden van bestanden definieert *python_tutorial_client.py* eerst verzamelingen van bestandspaden naar **toepassing**sbestanden en **invoer**bestanden op de lokale computer. Daarna worden deze bestanden geüpload naar de containers die u in de vorige stap hebt gemaakt.

```python
# Paths to the task script. This script will be executed by the tasks that
# run on the compute nodes.
application_file_paths = [os.path.realpath('python_tutorial_task.py')]

# The collection of data files that are to be processed by the tasks.
input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                    os.path.realpath('./data/taskdata2.txt'),
                    os.path.realpath('./data/taskdata3.txt')]

# Upload the application script to Azure Storage. This is the script that
# will process the data files, and is executed by each of the tasks on the
# compute nodes.
application_files = [
    upload_file_to_container(blob_client, APP_CONTAINER_NAME, file_path)
    for file_path in application_file_paths]

# Upload the data files. This is the data that will be processed by each of
# the tasks executed on the compute nodes in the pool.
input_files = [
    upload_file_to_container(blob_client, INPUT_CONTAINER_NAME, file_path)
    for file_path in input_file_paths]
```

Zoals dat bij lijsten gebruikelijk is, wordt de functie `upload_file_to_container` aangeroepen voor elk bestand in de verzamelingen en worden twee [ResourceFile][py_resource_file]-verzamelingen ingevuld. De functie `upload_file_to_container` wordt hieronder weergegeven:

```python
def upload_file_to_container(block_blob_client, container_name, path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """

    import datetime
    import azure.storage.blob as azureblob
    import azure.batch.models as batchmodels

    blob_name = os.path.basename(path)

    print('Uploading file {} to container [{}]...'.format(path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles
[ResourceFile][py_resource_file] levert aan taken in Batch de URL naar een bestand in Azure Storage dat naar een rekenknooppunt wordt gedownload voordat deze taak wordt uitgevoerd. De eigenschap [ResourceFile][py_resource_file].**blob_source** geeft de volledige URL van het bestand zoals dit zich in Azure Storage bevindt. De URL kan ook een Shared Access Signature (SAS) bevatten, die veilige toegang tot het bestand biedt. De meeste typen taken in Batch bevatten een eigenschap *ResourceFiles*, waaronder:

* [CloudTask][py_task]
* [StartTask][py_starttask]
* [JobPreparationTask][py_jobpreptask]
* [JobReleaseTask][py_jobreltask]

In dit voorbeeld worden de taaktypen JobPreparationTask of JobReleaseTask niet gebruikt. Meer informatie hierover vindt u echter in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md) (Jobvoorbereidings- en jobvrijgevingstaken uitvoeren op Azure Batch-rekenknooppunten).

### <a name="shared-access-signature-sas"></a>Shared Access Signature (SAS)
Shared Access Signatures zijn tekenreeksen die beveiligde toegang bieden tot containers en blobs in Azure Storage. Het script *python_tutorial_client.py* gebruikt Shared Access Signatures voor blobs en containers en laat zien hoe u deze SAS-tekenreeksen uit de Storage-service verkrijgt.

* **Shared Access Signatures voor blobs**: StartTask van de pool gebruikt Shared Access Signatures voor blobs wanneer deze het taakscript en de invoergegevensbestanden uit Storage downloadt (zie [Stap 3](#step-3-create-batch-pool) hieronder). De functie `upload_file_to_container` in *python_tutorial_client.py* bevat de code die de Shared Access Signature van elke blob verkrijgt. Dit gebeurt door het aanroepen van [BlockBlobService.make_blob_url][py_make_blob_url] in de Storage-module.
* **Shared Access Signatures voor containers**: Nadat elke taak in het rekenknooppunt zijn werk heeft verricht, uploadt elke taak zijn uitvoerbestand naar de *uitvoer*container in Azure Storage. Hiervoor maakt *python_tutorial_task.py* gebruik van een Shared Access Signature voor containers die schrijftoegang tot de container biedt. De functie `get_container_sas_token` in *python_tutorial_client.py* verkrijgt de Shared Access Signature van de container, die vervolgens als een opdrachtregelargument wordt doorgegeven aan de taken. In stap 5, [Taken toevoegen aan job](#step-5-add-tasks-to-job), wordt het gebruik van de Shared Access Signature voor containers besproken.

> [!TIP]
> Bekijk de tweedelige reeks over Shared Access Signatures [Part 1: Understanding the SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (Deel 1: inzicht in het Shared Access Signature (SAS)-model) en [Part 2: Create and use a SAS with the Blob service](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) (Deel 2: Een Shared Access Signature (SAS) maken en gebruiken met de Blob-service), voor meer informatie over het verstrekken van beveiligde toegang tot gegevens in uw Storage-account.
>
>

## <a name="step-3-create-batch-pool"></a>Stap 3: Batch-pool maken
![Een Batch-pool maken][3]
<br/>

Een Batch-**pool** is een verzameling rekenknooppunten (virtuele machines) waarop Batch de taken van een job uitvoert.

Nadat het het taakscript en gegevensbestanden naar het Storage-account heeft geüpload, start *python_tutorial_client.py* zijn interactie met de Batch-service met behulp van de Batch Python-module. Hiervoor wordt een [BatchServiceClient][py_batchserviceclient] gemaakt:

```python
# Create a Batch service client. We'll now be interacting with the Batch
# service in addition to Storage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

Daarna wordt een pool van rekenknooppunten gemaakt in het Batch-account met een aanroep naar `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Wanneer u een pool maakt, definieert u een [PoolAddParameter][py_pooladdparam] die verschillende eigenschappen voor de pool opgeeft:

* **Id** van de pool (*id* - vereist)<p/>Net als bij de meeste entiteiten in Batch, moet ook uw nieuwe pool een unieke id binnen uw Batch-account hebben. Uw code zal naar deze pool verwijzen met de bijbehorende id. Hiermee identificeert u ook de pool in Azure [Portal][azure_portal].
* **Aantal rekenknooppunten** (*target_dedicated* - vereist)<p/>Hiermee geeft u op hoeveel virtuele machines in de pool moeten worden geïmplementeerd. Houd er rekening mee dat alle Batch-accounts een standaard**quotum** hebben dat het aantal **kernen** (en dus rekenknooppunten) in een Batch-account beperkt. U vindt de standaardquota en instructies over het [verhogen van een quotum](batch-quota-limit.md#increase-a-quota) (zoals het maximum aantal kernen in uw Batch-account) in [Quotas and limits for the Azure Batch service](batch-quota-limit.md) (Quota en limieten voor de Azure Batch-service). Rijzen er vragen zoals "Waarom kan mijn pool niet meer dan X knooppunten bevatten?", dan is dit quotum voor kernen mogelijk de oorzaak.
* **Besturingssysteem** voor knooppunten (*virtual_machine_configuration* **of** *cloud_service_configuration* - vereist)<p/>In *python_tutorial_client.py* maken we een pool met Linux-knooppunten met behulp van een [VirtualMachineConfiguration][py_vm_config]. De `select_latest_verified_vm_image_with_node_agent_sku`-functie in `common.helpers` vereenvoudigt het gebruik van [Azure Virtual Machines Marketplace][vm_marketplace]-installatiekopieën. Zie [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) (Linux-rekenknooppunten in Azure Batch-pools inrichten) voor meer informatie over het gebruik van Marketplace-installatiekopieën.
* **Grootte van rekenknooppunten** (*vm_size* - vereist)<p/>Omdat we Linux-knooppunten opgeven voor onze [VirtualMachineConfiguration][py_vm_config], geven we een VM-grootte op (in dit voorbeeld `STANDARD_A1`) uit [Grootten voor virtuele machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Zie opnieuw [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) (Linux-rekenknooppunten in Azure Batch-pools inrichten) voor meer informatie.
* **Begintaak** (*start_task* - niet vereist)<p/>Samen met de bovenstaande fysieke knooppunteigenschappen, kunt u ook een [StartTask][py_starttask] voor de pool opgeven (dit is niet vereist). StartTask wordt in elk knooppunt uitgevoerd wanneer dat knooppunt aan de pool wordt toegevoegd en telkens wanneer dat knooppunt opnieuw wordt opgestart. StartTask is vooral nuttig voor de voorbereiding van rekenknooppunten voor het uitvoeren van taken, zoals het installeren van de toepassingen die door de taken worden uitgevoerd.<p/>In deze voorbeeldtoepassing kopieert StartTask de bestanden die het van Storage downloadt (die zijn opgegeven met behulp van de eigenschap **resource_files** van de StartTask) uit de StartTask-*werkmap* naar de *gedeelde* map waartoe alle taken die in het knooppunt worden uitgevoerd toegang hebben. In wezen wordt hierdoor `python_tutorial_task.py` gekopieerd naar de gedeelde map in elk knooppunt wanneer het knooppunt aan de pool wordt toegevoegd, zodat alle taken die in het knooppunt worden uitgevoerd er toegang toe hebben.

Misschien merkt u de aanroep naar de Help-functie `wrap_commands_in_shell` op. Deze functie gebruikt een verzameling afzonderlijke opdrachten en maakt een enkele opdrachtregel die geschikt is voor de opdrachtregeleigenschap van een taak.

Opmerkelijk in het bovenstaande codefragment is ook het gebruik van twee omgevingsvariabelen in de eigenschap **command_line** van de StartTask: `AZ_BATCH_TASK_WORKING_DIR` en `AZ_BATCH_NODE_SHARED_DIR`. Elk rekenknooppunt in een Batch-pool wordt automatisch geconfigureerd met meerdere omgevingsvariabelen die specifiek zijn voor Batch. Elke proces dat door een taak wordt uitgevoerd, heeft toegang tot deze omgevingsvariabelen.

> [!TIP]
> Zie de secties **Omgevingsinstellingen voor taken** en **Bestanden en mappen** in [Overzicht van Azure Batch-functies](batch-api-basics.md) voor meer informatie over de omgevingsvariabelen die beschikbaar zijn in rekenknooppunten in een Batch-pool en voor meer informatie over taakwerkmappen.
>
>

## <a name="step-4-create-batch-job"></a>Stap 4: Batch-job maken
![Een Batch-job maken][4]<br/>

Een Batch-**job** is een verzameling taken en is gekoppeld aan een pool van rekenknooppunten. De taken in een job worden uitgevoerd op de rekenknooppunten van de gekoppelde pool.

U kunt een job niet alleen gebruiken om taken in gerelateerde workloads te organiseren en te volgen, maar ook om bepaalde beperkingen op te leggen, zoals de maximale runtime voor de job (en, bij uitbreiding, dus ook voor de taken ervan) en de prioriteit van de job in verhouding tot andere jobs in de Batch-account. In dit voorbeeld wordt de job echter alleen gekoppeld aan de pool die in stap 3 is gemaakt. Er worden geen aanvullende eigenschappen geconfigureerd.

Alle Batch-jobs worden gekoppeld aan een specifieke pool. Deze koppeling geeft aan op welke knooppunten de taken van de job worden uitgevoerd. U geeft de pool op met behulp van de eigenschap [PoolInformation][py_poolinfo], zoals in het onderstaande codefragment wordt getoond.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Nu een job is gemaakt, worden er taken aan toegevoegd die het werk verrichten.

## <a name="step-5-add-tasks-to-job"></a>Stap 5: taken toevoegen aan job
![Taken toevoegen aan job][5]<br/>
*(1) Taken worden toegevoegd aan de job, (2) de taken worden gepland om op knooppunten te worden uitgevoerd en (3) de taken downloaden de te verwerken gegevensbestanden*

Batch-**taken** zijn de afzonderlijke werkeenheden die op de rekenknooppunten worden uitgevoerd. Een taak heeft een opdrachtregel en voert de scripts of uitvoerbare bestanden uit die u in die opdrachtregel opgeeft.

Om daadwerkelijk werk te verrichten, moeten de taken aan een job worden toegevoegd. Elke [CloudTask][py_task] wordt geconfigureerd met een opdrachtregeleigenschap en [ResourceFiles][py_resource_file] (net als bij de StartTask van de pool) die de taak downloadt naar het knooppunt voordat de opdrachtregel ervan automatisch wordt uitgevoerd. In het voorbeeld verwerkt elke taak slechts één bestand. Hierdoor bevat de verzameling ResourceFiles één element.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [!IMPORTANT]
> Wanneer ze toegang krijgen tot omgevingsvariabelen zoals `$AZ_BATCH_NODE_SHARED_DIR` of een toepassing uitvoeren die niet op het `PATH` van het knooppunt wordt gevonden, moeten taakopdrachtregels de shell expliciet aanroepen, zoals bij `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Deze vereiste is niet nodig als uw taken een toepassing uitvoeren in het `PATH` van het knooppunt en verwijzen niet naar omgevingsvariabelen.
>
>

In de `for`-lus in het bovenstaande codefragment kunt u zien dat de opdrachtregel voor de taak vijf opdrachtregelargumenten bevat die worden doorgegeven aan *python_tutorial_task.py*:

1. **filepath**: Dit is het lokale pad naar het bestand, zoals zich dit in het knooppunt bevindt. Toen hiervoor in stap 2 het ResourceFile-object `upload_file_to_container` werd gemaakt, werd de bestandsnaam voor deze eigenschap gebruikt (de parameter `file_path` in de constructor ResourceFile). Dit geeft aan dat het bestand kan worden gevonden in dezelfde map in het knooppunt als *python_tutorial_task.py*.
2. **numwords**: de bovenste *N* woorden moeten naar het uitvoerbestand worden geschreven.
3. **storageaccount**: de naam van het opslagaccount dat eigenaar is van de container waarnaar de uitvoer van de taak moet worden geüpload.
4. **storagecontainer**: de naam van de Storage-container waarnaar de uitvoerbestanden moeten worden geüpload.
5. **sastoken**: de Shared Access Signature (SAS) die schrijftoegang biedt tot de **uitvoer**container in Azure Storage. Het script *python_tutorial_task.py* gebruikt deze Shared Access Signature wanneer het de BlockBlobService-verwijzing maakt. Dit biedt schrijftoegang tot de container zonder dat een toegangssleutel voor het opslagaccount vereist is.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Stap 6: taken controleren
![Taken controleren][6]<br/>
*Het script (1) controleert taken op de status Voltooid en (2) de taken uploaden resultaatgegevens naar Azure Storage*

Wanneer aan een job taken worden toegevoegd, worden deze automatisch in de wachtrij geplaatst en gepland voor uitvoering op rekenknooppunten binnen de pool die aan de job is gekoppeld. Op basis van de instellingen die u opgeeft, zal Batch voor u alle taken in de wachtrij plaatsen, plannen en opnieuw proberen uit te voeren, evenals andere taakbeheerverrichtingen uitvoeren.

Er zijn veel manieren om de uitvoering van taken te controleren. De functie `wait_for_tasks_to_complete` in *python_tutorial_client.py* biedt een eenvoudig voorbeeld van de controle van taken op een bepaalde status, in dit geval de status [completed][py_taskstate] (voltooid).

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Stap 7: taakuitvoer downloaden
![Taakuitvoer downloaden uit Storage][7]<br/>

Nu de taak is voltooid, kan de uitvoer van de taken worden gedownload uit Azure Storage. Dat gebeurt met een aanroep naar `download_blobs_from_container` in *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [!NOTE]
> De aanroep naar `download_blobs_from_container` in *python_tutorial_client.py* geeft aan dat de bestanden naar de basismap moeten worden gedownload. U kunt eventueel deze uitvoerlocatie wijzigen.
>
>

## <a name="step-8-delete-containers"></a>Stap 8: containers verwijderen
Omdat gegevens die zich in Azure Storage bevinden, in rekening worden gebracht, doet u er altijd goed aan alle blobs te verwijderen die niet langer nodig zijn voor uw Batch-taken. In *python_tutorial_client.py* wordt dit gedaan met drie aanroepen naar [BlockBlobService.delete_container][py_delete_container]:

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Stap 9: de job en de pool verwijderen
In de laatste stap wordt u gevraagd de job en de pool te verwijderen die door het script *python_tutorial_client.py* zijn gemaakt. Hoewel jobs en taken zelf niet in rekening worden gebracht, worden rekenknooppunten *wel* in rekening gebracht. Daarom is het raadzaam om knooppunten alleen toe te wijzen als dat nodig is. Het verwijderen van ongebruikte pools kan een onderdeel zijn van uw onderhoudsprocedure.

[JobOperations][py_job] en [PoolOperations][py_pool] van BatchServiceClient hebben beide overeenkomstige verwijderingsmethoden, die worden aangeroepen wanneer u de verwijdering bevestigt:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> Houd er rekening mee dat rekenresources in rekening worden gebracht. Door ongebruikte pools te verwijderen, beperkt u uw kosten tot een minimum. Denk er ook aan dat een pool alle rekenknooppunten in die pool verwijdert en dat alle gegevens in de knooppunten onherstelbaar zijn nadat de pool wordt verwijderd.
>
>

## <a name="run-the-sample-script"></a>Het voorbeeldscript uitvoeren
Bij het uitvoeren van het *python_tutorial_client.py*-script uit de zelfstudie [voorbeeldcode][github_article_samples], lijkt de output van de console op het volgende. Bij `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` wordt gewacht wanneer de rekenknooppunten van de pool worden gemaakt en opgestart, en wanneer de opdrachten in de begintaak van de pool worden uitgevoerd. Gebruik [Azure Portal][azure_portal] om uw pool, rekenknooppunten, job en taken tijdens en na de uitvoering te controleren. Gebruik [Azure Portal][azure_portal] of [Microsoft Azure Storage Explorer][storage_explorer] om de Storage-resources (containers en blobs) weer te geven die door de toepassing zijn gemaakt.

> [!TIP]
> Voer het *python_tutorial_client.py*-script uit vanuit de `azure-batch-samples/Python/Batch/article_samples`-directory. Hierbij wordt een relatief pad voor de import van de `common.helpers`-module gebruikt, dus u kunt `ImportError: No module named 'common'` tegenkomen als u het script niet uitvoert in deze directory.
>
>

Wanneer u het voorbeeld uitvoert in de standaardconfiguratie, bedraagt de uitvoeringstijd doorgaans **ongeveer 5-7 minuten**.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen
Breng gerust wijzigingen aan in *python_tutorial_client.py* en *python_tutorial_task.py* om met verschillende rekenscenario's te experimenteren. Probeer bijvoorbeeld een uitvoeringsvertraging toe te voegen aan *python_tutorial_task.py* om langlopende taken te simuleren en ze te controleren in de portal. Probeer meer taken toe te voegen of het aantal rekenknooppunten aan te passen. Voeg logica toe om te controleren op een bestaande pool en het gebruik ervan toe te staan om de uitvoeringssnelheid te verhogen.

Nu u vertrouwd bent met de basiswerkstroom van een Batch-oplossing, is het tijd om kennis te maken met de aanvullende functies van de Batch-service.

* Lees het artikel [Overzicht van Azure Batch-functies](batch-api-basics.md). Dit is raadzaam als u niet vertrouwd bent met de service.
* Lees ook de andere artikelen over Batch-ontwikkeling die vermeld zijn onder **Ontwikkeling nader bekeken** in het [Batch-leertraject][batch_learning_path].
* Bekijk een andere implementatie van de verwerking van de workload 'eerste N woorden' met behulp van Batch in het voorbeeld [TopNWords][github_topnwords].

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage
[pypi_install]: https://packaging.python.org/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Containers maken in Azure Storage"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Taaktoepassings- en invoer(gegevens)bestanden uploaden naar containers"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Batch-pool maken"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Batch-job maken"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Taken toevoegen aan job"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Taken controleren"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Taakuitvoer downloaden uit Storage"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Werkstroom van de Batch-oplossing (volledige diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Batch-referenties in Portal"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Storage-referenties in Portal"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Werkstroom van de Batch-oplossing (minimaal diagram)"
