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
ms.date: 05/07/2018
ms.author: danlep
ms.openlocfilehash: 8c9f772c9d3908e450961239797f6ce2bd4982e4
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="run-container-applications-on-azure-batch"></a>Containertoepassingen worden uitgevoerd op Azure Batch

Azure Batch kunt u uitvoeren en schalen van grote aantallen computing taken op Azure batch. Tot op heden Batch-taken rechtstreeks op de virtuele machines (VM's) in een Batch-pool hebt uitgevoerd, maar u kunt nu een Batch-pool instellen om uit te voeren taken in Docker-containers. In dit artikel ziet u hoe u met de Batch .NET SDK een pool van rekenknooppunten die ondersteuning bieden voor lopende taken van de container maken en het uitvoeren van taken van de container op de groep.

Met behulp van de containers biedt een eenvoudige manier om het Batch-taken uitvoeren zonder het beheren van een omgeving en afhankelijkheden voor toepassingen. Containers implementeren van toepassingen als lichtgewicht, draagbare, vergaderverzoeken verwerken eenheden die kunnen worden uitgevoerd in verschillende omgevingen. U kunt bijvoorbeeld bouwen en testen van een container lokaal en vervolgens de container installatiekopie uploaden naar een register in Azure of ergens anders. Het implementatiemodel container zorgt ervoor dat de runtime-omgeving van uw toepassing altijd correct is geïnstalleerd en geconfigureerd, ongeacht waar de toepassing wordt gehost. Op basis van een container taken in Batch kunnen ook te profiteren van functies van niet-container taken, waaronder toepassingspakketten en beheer van bronbestanden en uitvoerbestanden. 

In dit artikel wordt ervan uitgegaan dat bekend bent met concepten voor Docker-container en het maken van een Batch-pool en de taak met de .NET SDK. De codefragmenten zijn bedoeld om te worden gebruikt in een clienttoepassing die vergelijkbaar is met de [DotNetTutorial voorbeeld](batch-dotnet-get-started.md), en vindt u voorbeelden van code die u moet om containertoepassingen te ondersteunen in een Batch.


## <a name="prerequisites"></a>Vereisten

* **SDK-versies**: de Batch-SDK's ondersteuning container installatiekopieën vanaf de volgende versies:
    * Batch REST-API versie 2017-09-01.6.0
    * Batch .NET SDK versie 8.0.0
    * Batch Python SDK versie 4.0
    * Batch Java SDK versie 3.0
    * Batch Node.js SDK versie 3.0

* **Accounts**: In uw Azure-abonnement, moet u een Batch-account en eventueel een Azure Storage-account maken.

* **Een ondersteunde VM-installatiekopie**: Containers worden alleen ondersteund in toepassingen die zijn gemaakt met de virtuele-machineconfiguratie van afbeeldingen in de volgende sectie wordt beschreven, 'ondersteund installatiekopieën van virtuele machines." Als u een aangepaste installatiekopie opgeeft, uw toepassing moet worden gebruikt voor het gebruik van Azure Active Directory [(Azure AD) verificatie](batch-aad-auth.md) om te kunnen uitvoeren op basis van een container werkbelastingen. 


## <a name="supported-virtual-machine-images"></a>Installatiekopieën van ondersteunde virtuele machines

U moet gebruiken een ondersteunde Windows of Linux-afbeelding voor het maken van een groep VM rekenknooppunten voor werkbelastingen van de container.

### <a name="windows-images"></a>Windows-installatiekopieën

Batch ondersteunt momenteel aangepaste installatiekopieën die u maakt van virtuele machines die worden uitgevoerd op Windows-Docker voor Windows-container werkbelasting of kunt u Windows Server 2016 Datacenter met de installatiekopie van de Containers uit Azure Marketplace. Deze installatiekopie is compatibel met de `batch.node.windows amd64` knooppunt agent SKU-ID. Het type van de container ondersteund is momenteel beperkt tot Docker.

### <a name="linux-images"></a>Linux-installatiekopieën

Linux-container werkbelastingen Batch ondersteunt momenteel alleen aangepaste installatiekopieën die u maakt van virtuele machines met Docker op de volgende Linux-distributies: Ubuntu 16.04 LTS of CentOS 7.3. Als u mogelijk uw eigen aangepaste Linux-installatiekopie wilt, raadpleegt u de instructies in [een beheerde aangepaste installatiekopie gebruiken voor het maken van een pool van virtuele machines](batch-custom-images.md).

Installeren voor ondersteuning van Docker [Docker Community Edition (CE)](https://www.docker.com/community-edition) of [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Als u profiteren van de GPU-prestaties van Azure NC of NV VM-grootten wilt, moet u NVIDIA stuurprogramma's installeren op de installatiekopie. Moet u ook wilt installeren en uitvoeren van het hulpprogramma voor Docker-Engine voor NVIDIA GPU's, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

Gebruik voor toegang tot het netwerk van Azure RDMA, RDMA-compatibele VM-grootten, zoals A8, A9, H16r, H16mr of NC24r. Benodigde RDMA-stuurprogramma's worden geïnstalleerd in de afbeeldingen CentOS 7.3 HPC en Ubuntu 16.04 TNS vanuit Azure Marketplace. Uitvoeren van MPI belastingen mogelijk aanvullende configuratie nodig. Zie [gebruik RDMA-compatibele of GPU ingeschakeld-exemplaren in de Batch-pool](batch-pool-compute-intensive-sizes.md).


## <a name="limitations"></a>Beperkingen

* Batch biedt ondersteuning voor RDMA alleen voor containers die worden uitgevoerd op Linux-groepen.


## <a name="authenticate-using-azure-active-directory"></a>Verifiëren met Azure Active Directory

Als u een aangepaste VM-installatiekopie gebruikt om de Batch-pool te maken, de clienttoepassing moet worden geverifieerd met behulp van Azure AD-geïntegreerde verificatie (verificatie met een gedeelde sleutel niet werkt). Voordat de toepassing wordt uitgevoerd, moet dat u deze te registreren in Azure AD een identiteit voor het maken en op te geven van de machtigingen voor andere toepassingen.

Wanneer u een aangepaste VM-installatiekopie gebruikt, moet u ook verlenen IAM-toegangsbeheer voor de toepassing toegang krijgen tot de VM-installatiekopie. Klik in de Azure-portal op **alle resources**, selecteer de installatiekopie van de container en van de **toegangsbeheer (IAM)** sectie van de installatiekopie-pagina, klikt u op **toevoegen**. In de **machtigingen toevoegen** pagina, geeft u een **rol**in **toewijzen van toegang tot**, selecteer **Azure AD-gebruiker, groep of toepassing**, klik dan in  **Selecteer** Voer de toepassingsnaam in.

In uw toepassing een Azure AD-verificatietoken worden doorgegeven bij het maken van de Batch-client. Als u met de Batch .NET SDK ontwikkelt, gebruikt u [BatchClient.Open](/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_), zoals beschreven in [oplossingen van de service Batch verifiëren met Active Directory](batch-aad-auth.md).


## <a name="reference-a-vm-image-for-pool-creation"></a>Verwijst naar een VM-installatiekopie voor het maken van de groep van toepassingen

Geef een verwijzing naar de VM-installatiekopie moet worden gebruikt bij het maken van de rekenknooppunten van de toepassingen in uw toepassingscode. U dit doen door het maken van een [ImageReference](/dotnet/api/microsoft.azure.batch.imagereference) object. U kunt opgeven dat de afbeelding moet worden gebruikt in een van de volgende manieren:

* Als u een aangepaste installatiekopie gebruikt, bieden u een Azure Resource Manager-bron-id voor de installatiekopie van de virtuele machine. De id van de installatiekopie heeft de padindeling van een zoals weergegeven in het volgende voorbeeld:

  ```csharp
  // Provide a reference to a custom image using an image ID
  ImageReference imageReference = new ImageReference("/subscriptions/<subscription-ID>/resourceGroups/<resource-group>/providers/Microsoft.Compute/images/<imageName>");
  ```

    Als u deze afbeeldings-ID van de Azure-portal, opent u **alle resources**, selecteert u de aangepaste installatiekopie en van de **overzicht** sectie kopiëren van de pagina de installatiekopie van het pad in **Resource-ID**.

* Als u een [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute?page=1&subcategories=windows-based) installatiekopie, geeft u een groep van parameters met een beschrijving van de afbeelding: de uitgever, het aanbiedingtype, SKU en versie van de afbeelding, zoals vermeld in [lijst van installatiekopieën van virtuele machines](batch-linux-nodes.md#list-of-virtual-machine-images):

  ```csharp
  // Provide a reference to an Azure Marketplace image for
  // "Windows Server 2016 Datacenter with Containers"
  ImageReference imageReference = new ImageReference(
    publisher: "MicrosoftWindowsServer",
    offer: "WindowsServer",
    sku: "2016-Datacenter-with-Containers",
    version: "latest");
  ```


## <a name="container-configuration-for-batch-pool"></a>De configuratie van de container voor Batch-pool

Om een Batch-pool container werkbelasting uitvoeren, moet u [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) instellingen in de groep [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) object.

U kunt een pool container ingeschakeld maken met of zonder tabelruimte container installatiekopieën, zoals wordt weergegeven in de volgende voorbeelden. Het proces pull (of prefetch) kunt u vooraf laden van installatiekopieën van de container van Docker-Hub of een andere container register op het Internet. Het voordeel van installatiekopieën van de container veelgevraagde is dat wanneer taken de eerste keer uitgevoerd niet hoeven te worden wacht tot de container-installatiekopie te downloaden. Wanneer de groep is gemaakt, haalt de configuratie van de container container installatiekopieën naar de virtuele machines. Taken die worden uitgevoerd op de groep kunnen vervolgens verwijzen naar de lijst met afbeeldingen container en container opties uitgevoerd.



### <a name="pool-without-prefetched-container-images"></a>Toepassingen zonder afbeeldingen tabelruimte container

Voor het configureren van een groep container ingeschakeld zonder tabelruimte container afbeeldingen definiëren `ContainerConfiguration` en `VirtualMachineConfiguration` objecten, zoals wordt weergegeven in het volgende voorbeeld. Dit en de volgende voorbeelden wordt ervan uitgegaan dat u van een aangepaste installatiekopie van Ubuntu 16.04 TNS met Docker-Engine is geïnstalleerd gebruikmaakt.

```csharp
// Specify container configuration. This is required even though there are no prefetched images.
ContainerConfiguration containerConfig = new ContainerConfiguration();

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

// Commit pool creation
pool.Commit();
```


### <a name="prefetch-images-for-container-configuration"></a>Prefetch-installatiekopieën voor de configuratie van de container

Als u wilt prefetch installatiekopieën van de container voor de toepassingen, de lijst van installatiekopieën van de container toevoegen (`containerImageNames`) naar de `ContainerConfiguration`, en geef een naam op voor de lijst met afbeeldingen. Het volgende voorbeeld wordt ervan uitgegaan dat u van een aangepaste Ubuntu 16.04 TNS installatiekopie gebruikmaakt en prefetch van de installatiekopie van een TensorFlow van [Docker Hub](https://hub.docker.com). Dit voorbeeld bevat een begintaak die in de VM-host wordt uitgevoerd op de knooppunten van de groep van toepassingen. U kunt bijvoorbeeld een begintaak op de host uitvoeren om te koppelen van een bestandsserver die toegankelijk is vanaf de containers.

```csharp
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

// Commit pool creation
pool.Commit();
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Prefetch installatiekopieën van een privé-container-register

U kunt ook prefetch-installatiekopieën van de container door de verificatie bij een privé-container register-server. In het volgende voorbeeld wordt de `ContainerConfiguration` en `VirtualMachineConfiguration` objecten een aangepaste installatiekopie van het Ubuntu 16.04 TNS gebruiken en de installatiekopie van een persoonlijke TensorFlow prefetch uit een container voor persoonlijke Azure-register.

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

// Commit pool creation
pool.Commit();
```


## <a name="container-settings-for-the-task"></a>Instellingen van de container voor de taak

Container taken op de rekenknooppunten worden uitgevoerd, moet u container-specifieke instellingen zoals taak opties te gebruiken installatiekopieën en register wordt uitgevoerd.

Gebruik de `ContainerSettings` eigenschap van de klassen van de taak voor het configureren van de container-specifieke instellingen. Deze instellingen zijn gedefinieerd door de [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) klasse.

Als u taken op de container-installatiekopieën uitvoeren, de [cloud taak](/dotnet/api/microsoft.azure.batch.cloudtask) en [jobbeheertaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) container-instellingen voor vereisen. Echter, de [begintaak](/dotnet/api/microsoft.azure.batch.starttask), [jobvoorbereidingstaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), en [jobvrijgevingstaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) vereisen geen instellingen van de container (dat wil zeggen, kunnen ze worden uitgevoerd binnen de context van een container of rechtstreeks op het knooppunt).

Wanneer u de instellingen van de container, alle mappen recursief onderstaande configureert de `AZ_BATCH_NODE_ROOT_DIR` (de hoofdmap van Azure Batch-mappen op het knooppunt) worden toegewezen aan de container, alle taak omgeving variabelen worden toegewezen aan de container en de opdrachtregel van de taak wordt uitgevoerd in de container.

In het voorbeeld in [Prefetch-installatiekopieën voor de configuratie van de container](#prefetch-images-for-container-configuration) hebt u geleerd hoe u de configuratie van een container voor een begintaak opgeven. Het volgende codevoorbeeld ziet u hoe u de configuratie van de container voor een cloud-taak opgeven:

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
