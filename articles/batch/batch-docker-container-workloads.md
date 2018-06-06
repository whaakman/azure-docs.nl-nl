---
title: Container werkbelastingen op Azure Batch | Microsoft Docs
description: Informatie over het uitvoeren van toepassingen van installatiekopieën van de container op Azure Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 06/04/2018
ms.author: danlep
ms.openlocfilehash: 4ee8425bb5c3830b029b766aad464df0ffb15f41
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801112"
---
# <a name="run-container-applications-on-azure-batch"></a>Containertoepassingen worden uitgevoerd op Azure Batch

Azure Batch kunt u uitvoeren en schalen van grote aantallen computing taken op Azure batch. Batch-taken rechtstreeks op de virtuele machines (knooppunten) in een Batch-pool kunnen uitvoeren, maar u kunt een Batch-pool instellen Docker-compatibele containers taken uitgevoerd op de knooppunten. In dit artikel leest u hoe een pool van rekenknooppunten die ondersteunen actieve taken van de container en voer vervolgens container taken op de groep te maken. 

U moet bekend bent met concepten container en het maken van een Batch-pool en de taak. De codevoorbeelden Batch .NET en Python SDK's gebruiken. U kunt ook andere Batch-SDK's en hulpprogramma's, waaronder de Azure-portal gebruiken voor het maken van de Batch-pools container is ingeschakeld en voor het uitvoeren van taken van de container.

## <a name="why-use-containers"></a>Waarom containers gebruiken?

Met behulp van de containers biedt een eenvoudige manier om het Batch-taken uitvoeren zonder het beheren van een omgeving en afhankelijkheden voor toepassingen. Containers implementeren van toepassingen als lichtgewicht, draagbare, vergaderverzoeken verwerken eenheden die kunnen worden uitgevoerd in verschillende omgevingen. U kunt bijvoorbeeld bouwen en testen van een container lokaal en vervolgens de container installatiekopie uploaden naar een register in Azure of ergens anders. Het implementatiemodel container zorgt ervoor dat de runtime-omgeving van uw toepassing altijd correct is geïnstalleerd en geconfigureerd wanneer u de toepassing wordt gehost. Op basis van een container taken in Batch kunnen ook te profiteren van functies van niet-container taken, waaronder toepassingspakketten en beheer van bronbestanden en uitvoerbestanden. 

## <a name="prerequisites"></a>Vereisten

* **SDK-versies**: de Batch-SDK's ondersteuning container installatiekopieën vanaf de volgende versies:
    * Batch REST-API versie 2017-09-01.6.0
    * Batch .NET SDK versie 8.0.0
    * Batch Python SDK versie 4.0
    * Batch Java SDK versie 3.0
    * Batch Node.js SDK versie 3.0

* **Accounts**: In uw Azure-abonnement, moet u een Batch-account en eventueel een Azure Storage-account maken.

* **Een ondersteunde VM-installatiekopie**: Containers worden alleen ondersteund in toepassingen die zijn gemaakt met de virtuele-machineconfiguratie van afbeeldingen in de volgende sectie wordt beschreven, 'ondersteund installatiekopieën van virtuele machines." Als u een aangepaste installatiekopie opgeeft, Zie de overwegingen in de volgende sectie en de vereisten in [een beheerde aangepaste installatiekopie gebruiken voor het maken van een pool van virtuele machines](batch-custom-images.md). 

### <a name="limitations"></a>Beperkingen

* Batch biedt ondersteuning voor RDMA alleen voor containers die worden uitgevoerd op Linux-groepen.

## <a name="supported-virtual-machine-images"></a>Installatiekopieën van ondersteunde virtuele machines

Gebruik een van de volgende ondersteund Windows of Linux-installatiekopieën maken van een groep VM rekenknooppunten voor werkbelastingen van de container. Zie voor meer informatie over installatiekopieën van Marketplace die compatibel met Batch zijn [lijst van installatiekopieën van virtuele machines](batch-linux-nodes.md#list-of-virtual-machine-images). 

### <a name="windows-images"></a>Windows-installatiekopieën

Voor Windows-container werkbelasting Batch ondersteunt momenteel de **Windows Server 2016 Datacenter met Containers** installatiekopie in Azure Marketplace. Alleen de afbeeldingen van Docker-container worden ondersteund in Windows.

U kunt ook aangepaste installatiekopieën maken van VM's met Docker Windows.

### <a name="linux-images"></a>Linux-installatiekopieën

Batch ondersteunt momenteel de volgende Linux installatiekopieën die zijn gepubliceerd door Microsoft Azure Batch in Azure Marketplace voor Linux-container-werkbelastingen:

* **CentOS voor groepen van Azure Batch-container**

* **CentOS (met RDMA-stuurprogramma's) voor Azure Batch-container pools**

* **Ubuntu Server voor groepen van Azure Batch-container**

* **Ubuntu Server (met RDMA-stuurprogramma's) voor Azure Batch-container pools**

Deze installatiekopieën worden alleen ondersteund voor gebruik in Azure Batch-pools. Ze bieden:

* Een vooraf geïnstalleerde [Moby](https://github.com/moby/moby) container runtime 

* Vooraf geïnstalleerde NVIDIA GPU-stuurprogramma's voor het stroomlijnen van implementatie op virtuele machines in Azure N-serie

* Afbeeldingen met of zonder vooraf geïnstalleerde stuurprogramma's voor RDMA; Deze stuurprogramma's toestaan knooppunten van de groep toegang tot het netwerk van Azure RDMA wanneer geïmplementeerd op de RDMA-compatibele VM-grootte  

U kunt ook aangepaste installatiekopieën maken van virtuele machines met Docker op een van de Linux-distributies die compatibel is met de Batch. Als u mogelijk uw eigen aangepaste Linux-installatiekopie wilt, raadpleegt u de instructies in [een beheerde aangepaste installatiekopie gebruiken voor het maken van een pool van virtuele machines](batch-custom-images.md).

Installeren voor Docker-ondersteuning voor een aangepaste installatiekopie [Docker Community Edition (CE)](https://www.docker.com/community-edition) of [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Aanvullende overwegingen voor het gebruik van een aangepaste installatiekopie voor Linux:

* Als u wilt profiteren van de GPU-prestaties van Azure N-serie grootten wanneer u een aangepaste installatiekopie, moet u vooraf NVIDIA stuurprogramma's installeren. Bovendien moet u het hulpprogramma voor Docker-Engine installeren voor NVIDIA GPU's, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

* Gebruik voor toegang tot het netwerk van Azure RDMA moet een RDMA-compatibele VM-grootte. Benodigde RDMA-stuurprogramma's worden geïnstalleerd in de CentOS HPC en Ubuntu-installatiekopieën ondersteund door de Batch. Uitvoeren van MPI belastingen mogelijk aanvullende configuratie nodig. Zie [gebruik RDMA-compatibele of GPU ingeschakeld-exemplaren in de Batch-pool](batch-pool-compute-intensive-sizes.md).


## <a name="container-configuration-for-batch-pool"></a>De configuratie van de container voor Batch-pool

Om een Batch-pool container werkbelasting uitvoeren, moet u [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) instellingen in de groep [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) object. (In dit artikel bevat koppelingen naar de Batch .NET API-verwijzing. Bijbehorende instellingen zijn de [Batch Python](/python/api/azure.batch) API.)

U kunt een pool container ingeschakeld maken met of zonder tabelruimte container installatiekopieën, zoals wordt weergegeven in de volgende voorbeelden. Het proces pull (of prefetch) kunt u vooraf laden van installatiekopieën van de container van Docker-Hub of een andere container register op het Internet. Voor de beste prestaties gebruikt een [Azure container register](../container-registry/container-registry-intro.md) in dezelfde regio bevinden als het Batch-account.

Het voordeel van installatiekopieën van de container veelgevraagde is dat wanneer taken de eerste keer uitgevoerd niet hoeven te worden wacht tot de container-installatiekopie te downloaden. Wanneer de groep is gemaakt, haalt de configuratie van de container container installatiekopieën naar de virtuele machines. Taken die worden uitgevoerd op de groep kunnen vervolgens verwijzen naar de lijst met afbeeldingen container en container opties uitgevoerd.


### <a name="pool-without-prefetched-container-images"></a>Toepassingen zonder afbeeldingen tabelruimte container

Voor het configureren van een groep container ingeschakeld zonder tabelruimte container afbeeldingen definiëren `ContainerConfiguration` en `VirtualMachineConfiguration` objecten, zoals wordt weergegeven in het volgende voorbeeld van Python. Dit voorbeeld wordt de Ubuntu Server voor Azure Batch-container pools installatiekopie van de Marketplace.


```python
image_ref_to_use = batch.models.ImageReference(
        publisher='microsoft-azure-batch',
        offer='ubuntu-server-container',
        sku='16-04-lts',
        version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            container_configuration=container_conf,
            node_agent_sku_id='batch.node.ubuntu 16.04'),
        vm_size='STANDARD_D1_V2',
        target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Prefetch-installatiekopieën voor de configuratie van de container

Als u wilt prefetch installatiekopieën van de container voor de toepassingen, de lijst van installatiekopieën van de container toevoegen (`container_image_names`, in Python) naar de `ContainerConfiguration`. 

De volgende basic Python-voorbeeld laat zien hoe een standaardinstallatiekopie van de container Ubuntu van prefetch [Docker Hub](https://hub.docker.com).

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


Het volgende voorbeeld C#-voorbeeld wordt ervan uitgegaan dat u wilt de installatiekopie van een TensorFlow van prefetch [Docker Hub](https://hub.docker.com). Dit voorbeeld bevat een begintaak die in de VM-host wordt uitgevoerd op de knooppunten van de groep van toepassingen. U kunt bijvoorbeeld een begintaak op de host uitvoeren om te koppelen van een bestandsserver die toegankelijk is vanaf de containers.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Prefetch installatiekopieën van een privé-container-register

U kunt ook prefetch-installatiekopieën van de container door de verificatie bij een privé-container register-server. In het volgende voorbeeld wordt de `ContainerConfiguration` en `VirtualMachineConfiguration` objecten prefetch een persoonlijke TensorFlow afbeelding uit een container voor persoonlijke Azure-register. De verwijzing naar afbeelding is hetzelfde als in het vorige voorbeeld.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```


## <a name="container-settings-for-the-task"></a>Instellingen van de container voor de taak

Container taken op de rekenknooppunten worden uitgevoerd, moet u container-specifieke instellingen zoals taak opties te gebruiken installatiekopieën en register wordt uitgevoerd.

Gebruik de `ContainerSettings` eigenschap van de klassen van de taak voor het configureren van de container-specifieke instellingen. Deze instellingen zijn gedefinieerd door de [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) klasse.

Als u taken op de container-installatiekopieën uitvoeren, de [cloud taak](/dotnet/api/microsoft.azure.batch.cloudtask) en [jobbeheertaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) container-instellingen voor vereisen. Echter, de [begintaak](/dotnet/api/microsoft.azure.batch.starttask), [jobvoorbereidingstaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), en [jobvrijgevingstaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) vereisen geen instellingen van de container (dat wil zeggen, kunnen ze worden uitgevoerd binnen de context van een container of rechtstreeks op het knooppunt).

Wanneer u de instellingen van de container, alle mappen recursief onderstaande configureert de `AZ_BATCH_NODE_ROOT_DIR` (de hoofdmap van Azure Batch-mappen op het knooppunt) worden toegewezen aan de container, alle taak omgeving variabelen worden toegewezen aan de container en de opdrachtregel van de taak wordt uitgevoerd in de container.

Het volgende Python-fragment toont een basic opdrachtregel die wordt uitgevoerd in een Ubuntu-container die is opgehaald uit de Docker-Hub. De container uitvoeren-opties zijn extra argumenten voor de `docker create` opdracht die de taak wordt uitgevoerd. Hier de `--rm` optie wordt de container verwijderd nadat de taak is voltooid.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='ubuntu', 
    container_run_options='--rm')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='echo hello',
    container_settings=task_container_settings
)

```

De volgende C#-voorbeeld toont basic container-instellingen voor een cloud-taak:

```csharp
// Simple container task command

string cmdLine = "<my-command-line>";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu",
    containerRunOptions: "--rm --read-only"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## <a name="next-steps"></a>Volgende stappen

* Zie ook de [Batch scheepswerf](https://github.com/Azure/batch-shipyard) toolkit voor een gemakkelijke implementatie van de container werkbelastingen op Azure Batch via [scheepswerf recepten](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* Zie voor meer informatie over het installeren en gebruiken van Docker CE op Linux, de [Docker](https://docs.docker.com/engine/installation/) documentatie.

* Zie voor meer informatie over het gebruik van aangepaste installatiekopieën [een beheerde aangepaste installatiekopie gebruiken voor het maken van een pool van virtuele machines ](batch-custom-images.md).

* Meer informatie over de [Moby project](https://mobyproject.org/), een framework voor het maken van de container-systemen.