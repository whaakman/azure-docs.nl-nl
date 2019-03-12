---
title: Voer Linux op virtuele machine-rekenknooppunten - Azure Batch | Microsoft Docs
description: Leer hoe u uw parallelle compute-workloads in pools van virtuele Linux-machines in Azure Batch worden verwerkt.
services: batch
documentationcenter: python
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: na
ms.date: 06/01/2018
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e228e73283685988247c8d419ba0a97b8c7b2974
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57546962"
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Linux-rekenknooppunten in Batch-pools inrichten

U kunt Azure Batch gebruiken voor het uitvoeren van parallelle compute-workloads op zowel Windows als Linux-machines. Dit artikel wordt uitgelegd hoe u pools van Linux-rekenknooppunten in de Batch-service maken met zowel de [Batch Python] [ py_batch_package] en [Batch .NET] [ api_net]-clientbibliotheken.

> [!NOTE]
> Toepassingspakketten worden ondersteund in alle Batch-pools die na 5 juli 2017 zijn gemaakt. De pakketten worden ondersteund in Batch-pools die zijn gemaakt tussen 10 maart 2016 en 5 juli 2017, maar alleen als de pool is gemaakt met behulp van een cloudservice-configuratie. Batch-pools die zijn gemaakt vóór 10 maart 2016 bieden geen ondersteuning voor toepassingspakketten. Zie [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md) (Toepassingen implementeren naar rekenknooppunten met Batch-toepassingspakketten) voor meer informatie over het gebruiken van toepassingspakketten om toepassingen te implementeren naar Batch-knooppunten.
>
>

## <a name="virtual-machine-configuration"></a>Virtuele-machineconfiguratie
Wanneer u een pool van rekenknooppunten in Batch maakt, hebt u twee opties waaruit u de knooppuntgrootte en het besturingssysteem selecteren: Cloud Services-configuratie en de configuratie virtuele Machine.

**Cloud Services-configuratie** biedt *alleen* Windows rekenknooppunten. Grootte van beschikbare rekenknooppunten worden vermeld in [groottes voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md), en beschikbare besturingssystemen worden vermeld de [Azure Guest OS releases en SDK compatibiliteitsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md). Wanneer u een pool met Azure Cloud Services-knooppunten maakt, geeft u de grootte van het knooppunt en de OS-familie, die worden beschreven in de eerder genoemde artikelen. Voor pools met Windows rekenknooppunten, wordt er meestal Cloud Services gebruikt.

**Virtuele-machineconfiguratie** biedt zowel Windows als Linux-installatiekopieën voor rekenknooppunten. Grootte van beschikbare rekenknooppunten worden vermeld in [grootten voor virtuele machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) en [grootten voor virtuele machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). Wanneer u een pool met virtuele-machineconfiguratie knooppunten maakt, moet u de grootte van de knooppunten, de verwijzing naar de installatiekopie van de virtuele machine en de Batch-knooppuntagent SKU moet worden geïnstalleerd op de knooppunten opgeven.

### <a name="virtual-machine-image-reference"></a>Verwijzing naar de installatiekopie van de virtuele machine
De Batch-service gebruikt [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor compute-knooppunten in de configuratie van de virtuele Machine. Kunt u een installatiekopie van het [Azure Marketplace][vm_marketplace], of geef een aangepaste installatiekopie die u hebt voorbereid. Zie voor meer informatie over aangepaste installatiekopieën [een pool maken met een aangepaste installatiekopie](batch-custom-images.md).

Wanneer u een verwijzing naar de installatiekopie van de virtuele machine configureert, geeft u de eigenschappen van de installatiekopie van de virtuele machine. De volgende eigenschappen zijn vereist wanneer u een verwijzing naar de installatiekopie van de virtuele machine maken:

| **Naslaginformatie over eigenschappen van de installatiekopie** | **Voorbeeld** |
| --- | --- |
| Uitgever |Canonical |
| Aanbieding |UbuntuServer |
| SKU |14.04.4-LTS |
| Versie |meest recente |

> [!TIP]
> U kunt meer informatie over deze eigenschappen en het aanbieden van Marketplace-installatiekopieën in [navigeren door en selecteren installatiekopieën van Linux-machines in Azure met CLI of PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Houd er rekening mee dat niet alle Marketplace-installatiekopieën momenteel compatibel met Batch zijn. Zie voor meer informatie, [knooppuntagent-SKU](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Knooppuntagent-SKU
De Batch-knooppuntagent is een programma dat wordt uitgevoerd op elk knooppunt in de groep van toepassingen en biedt de opdracht en controle-interface tussen het knooppunt en de Batch-service. Er zijn verschillende implementaties van de knooppuntagent, SKU's, ook wel voor verschillende besturingssystemen. In feite, als u de configuratie van een virtuele Machine maakt, u eerst de verwijzing naar de installatiekopie van de virtuele machine opgeven, en vervolgens geeft u het knooppuntagent te installeren op de afbeelding. Normaal gesproken elke knooppuntagent SKU is compatibel met meerdere installatiekopieën van virtuele machines. Hier volgen enkele voorbeelden van knooppuntagent-SKU's:

* batch.node.ubuntu 14.04
* batch.node.centos 7
* batch.node.Windows amd64

> [!IMPORTANT]
> Niet alle installatiekopieën voor virtuele machines die beschikbaar in de Marketplace zijn zijn compatibel met de momenteel beschikbare Batch knooppunt agents. De Batch-SDK's gebruikt om de beschikbare knooppuntagent-SKU's en de installatiekopieën van virtuele machines waarmee ze compatibel zijn. Zie de [lijst van de virtuele Machine-installatiekopieën](#list-of-virtual-machine-images) verderop in dit artikel voor meer informatie en voorbeelden van hoe u een lijst met geldige afbeeldingen tijdens runtime ophaalt.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Een Linux-groep maken: Batch Python
Het volgende codefragment toont een voorbeeld van het gebruik van de [Microsoft Azure Batch-clientbibliotheek voor Python] [ py_batch_package] compute-knooppunten van een groep van Ubuntu-Server maken. Referentiedocumentatie voor de Batch Python-module kan worden gevonden op [azure.batch pakket] [ py_batch_docs] op de documenten lezen.

Dit fragment maakt een [ImageReference] [ py_imagereference] expliciet en Hiermee geeft u op elk van de eigenschappen (uitgever, aanbieding, SKU, versie). Bij de productiecode, raden wij aan dat u de [list_node_agent_skus] [ py_list_skus] methode om te bepalen en selecteer in de beschikbare afbeeldings- en knooppunt agent SKU combinaties tijdens runtime.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Zoals eerder vermeld, raden wij aan dat in plaats van het maken van de [ImageReference] [ py_imagereference] expliciet aanroepen, gebruikt u de [list_node_agent_skus] [ py_list_skus] methode dynamisch software selecteren uit de momenteel ondersteunde knooppunt agent/Marketplace-installatiekopie combinaties. Het volgende fragment van Python ziet u hoe u deze methode.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Een Linux-groep maken: Batch .NET
Het volgende codefragment toont een voorbeeld van het gebruik van de [Batch .NET] [ nuget_batch_net] -clientbibliotheek voor compute-knooppunten van een pool van Ubuntu-Server maken. U vindt de [Batch .NET-referentiedocumentatie] [ api_net] op docs.microsoft.com.

De volgende code codefragment wordt de [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] methode om te selecteren in de lijst op dit moment ondersteund Marketplace-installatiekopie en node agent SKU combinaties. Deze techniek is het wenselijk omdat de lijst met ondersteunde combinaties van tijd tot tijd kan veranderen. Ondersteunde combinaties worden meestal toegevoegd.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit the pool to the Batch service
await pool.CommitAsync();
```

Hoewel het vorige fragment wordt de [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] methode voor dynamisch weergeven en selecteren van ondersteunde afbeeldings- en knooppunt agent SKU combinaties (aanbevolen), kunt u ook configureren een [ImageReference] [ net_imagereference] expliciet:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Lijst met installatiekopieën van virtuele machines
De volgende tabel bevat de Marketplace-installatiekopieën voor virtuele machines die compatibel met de beschikbare agents van de Batch-knooppunt zijn wanneer dit artikel voor het laatst is bijgewerkt. Het is belangrijk te weten dat deze lijst is geen definitieve omdat afbeeldingen en knooppunt-agents kunnen worden toegevoegd of verwijderd op elk gewenst moment. Het is raadzaam dat uw Batch-toepassingen en services altijd gebruiken [list_node_agent_skus] [ py_list_skus] (Python) of [ListNodeAgentSkus] [ net_list_skus] () Batch .NET) om te bepalen en selecteer in de momenteel beschikbare SKU's.

> [!WARNING]
> De volgende lijst kan op elk gewenst moment wijzigen. Gebruik altijd de **lijst knooppuntagent-SKU** methoden die beschikbaar zijn in de Batch-API's om de compatibele virtuele machine en knooppuntagent-SKU's wanneer u uw Batch-taken uitvoert.
>
>

| **Publisher** | **Aanbieding** | **Installatiekopie-SKU** | **Versie** | **Knooppuntagent-SKU-ID** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| batch | rendering-centos73 | rendering | meest recente | batch.node.centos 7 |
| batch | rendering-windows2016 | rendering | meest recente | batch.node.Windows amd64 |
| Canonical | UbuntuServer | 16.04-LTS | meest recente | batch.node.ubuntu 16.04 |
| Canonical | UbuntuServer | 14.04.5-LTS | meest recente | batch.node.ubuntu 14.04 |
| credativ | Debian | 9 | meest recente | batch.node.debian 9 |
| credativ | Debian | 8 | meest recente | batch.node.debian 8 |
| microsoft-ads | linux-data-science-vm | linuxdsvm | meest recente | batch.node.centos 7 |
| microsoft-ads | Standard-data-science-vm | Standard-data-science-vm | meest recente | batch.node.Windows amd64 |
| microsoft-azure-batch | centos-container | 7-4 | meest recente | batch.node.centos 7 |
| microsoft-azure-batch | centos-container-rdma | 7-4 | meest recente | batch.node.centos 7 |
| microsoft-azure-batch | ubuntu-server-container | 16-04-lts | meest recente | batch.node.ubuntu 16.04 |
| microsoft-azure-batch | ubuntu-server-container-rdma | 16-04-lts | meest recente | batch.node.ubuntu 16.04 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter-smalldisk | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter met Containers | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter-smalldisk | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter-smalldisk | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008-R2-SP1 | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008-R2-SP1-smalldisk | meest recente | batch.node.Windows amd64 |
| OpenLogic | CentOS | 7.4 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.4 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.3 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.1 | meest recente | batch.node.centos 7 |
| Oracle | Oracle-Linux | 7.4 | meest recente | batch.node.centos 7 |
| SUSE | SLES HPC | 12 SP2 | meest recente | batch.node.opensuse 42.1 |

## <a name="connect-to-linux-nodes-using-ssh"></a>Verbinding maken met Linux-knooppunten via SSH
Tijdens de ontwikkeling of bij het oplossen van problemen vindt u het kan nodig zijn om aan te melden bij de knooppunten in uw pool. In tegenstelling tot Windows-rekenknooppunten, kunt u Remote Desktop Protocol (RDP) niet gebruiken voor het verbinding maken met Linux-knooppunten. De Batch-service kan in plaats daarvan SSH-toegang voor externe verbinding op elk knooppunt.

Het volgende Python-codefragment maakt een gebruiker op elk knooppunt in een pool, die vereist voor externe verbinding is. De verbindingsgegevens van SSH (secure shell) voor elk knooppunt wordt afgedrukt.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Hier volgt een voorbeeld van uitvoer voor de vorige code voor een groep met vier knooppunten voor Linux:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

U kunt in plaats van een wachtwoord, een openbare SSH-sleutel opgeven wanneer u een gebruiker op een knooppunt. In de Python-SDK gebruikt de **ssh_public_key** parameter op [ComputeNodeUser][py_computenodeuser]. In .NET, gebruiken de [ComputeNodeUser][net_computenodeuser].[ SshPublicKey] [ net_ssh_key] eigenschap.

## <a name="pricing"></a>Prijzen
Azure Batch is gebouwd op Azure Cloud Services en virtuele Machines van Azure-technologie. De Batch-service zelf wordt aangeboden zonder kosten, wat betekent dat u betaalt alleen voor de compute-resources dat die uw Batch-oplossingen worden gebruikt. Als u ervoor kiest **Cloud Services-configuratie**, de kosten worden berekend op basis van de [prijzen voor Cloud Services] [ cloud_services_pricing] structuur. Als u ervoor kiest **Virtuele-machineconfiguratie**, de kosten worden berekend op basis van de [prijzen voor Virtual Machines] [ vm_pricing] structuur. 

Als u toepassingen implementeert voor uw Batch-knooppunten met behulp van [toepassingspakketten](batch-application-packages.md), er zijn ook in rekening gebracht voor de Azure Storage-resources dat de toepassingspakketten gebruiken. De Azure Storage-kosten zijn in het algemeen is minimaal. 

## <a name="next-steps"></a>Volgende stappen

De [codevoorbeelden voor Python] [ github_samples_py] in de [azure-batch-samples] [ github_samples] -bibliotheek op GitHub scripts bevatten die laten zien u hoe u kunt uitvoeren algemene batchbewerkingen, zoals toepassingen, job en het maken van taak. De [Leesmij-bestand] [ github_py_readme] die meegestuurd met de Python voorbeelden vindt u informatie over het installeren van de vereiste pakketten.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: https://azure-sdk-for-python.readthedocs.io/batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: https://docs.microsoft.com/python/api/azure.batch.models.computenodeuser
[py_imagereference]: https://docs.microsoft.com/python/api/azure.mgmt.batch.models.imagereference
[py_list_skus]: https://docs.microsoft.com/python/api/azure-batch/azure.batch.operations.AccountOperations?view=azure-python
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
