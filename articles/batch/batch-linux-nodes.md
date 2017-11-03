---
title: Voer Linux op de virtuele machine rekenknooppunten - Azure Batch | Microsoft Docs
description: Informatie over het verwerken van uw parallelle compute-workloads op groepen van Linux virtuele machines in Azure Batch.
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b2257917e2368478beb75957677de23d4157865
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Linux-rekenknooppunten in de Batch-pools inrichten

U kunt Azure Batch uitvoeren van parallelle compute-workloads op Linux- en Windows virtuele machines. Dit artikel wordt uitgelegd pools van Linux-rekenknooppunten in de Batch-service maken met behulp van zowel de [Batch Python] [ py_batch_package] en [Batch .NET] [ api_net] clientbibliotheken.

> [!NOTE]
> Toepassingspakketten worden ondersteund in alle Batch-pools die na 5 juli 2017 zijn gemaakt. De pakketten worden ondersteund in Batch-pools die zijn gemaakt tussen 10 maart 2016 en 5 juli 2017, maar alleen als de pool is gemaakt met behulp van een cloudservice-configuratie. Batch-pools die zijn gemaakt vóór 10 maart 2016 bieden geen ondersteuning voor toepassingspakketten. Zie [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md) (Toepassingen implementeren naar rekenknooppunten met Batch-toepassingspakketten) voor meer informatie over het gebruiken van toepassingspakketten om toepassingen te implementeren naar Batch-knooppunten.
>
>

## <a name="virtual-machine-configuration"></a>Configuratie van virtuele machine
Wanneer u een pool van rekenknooppunten in een Batch maakt, hebt u twee opties waaruit u de grootte van knooppunt en het besturingssysteem selecteren: configuratie voor Cloud-Services en virtuele-machineconfiguratie.

**Cloud Services-configuratie** biedt *alleen* Windows rekenknooppunten. Beschikbare compute-knooppuntgrootten worden vermeld in [grootten voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md), en beschikbare besturingssystemen worden vermeld in de [Azure Gast OS releases en SDK compatibiliteit matrix](../cloud-services/cloud-services-guestos-update-matrix.md). Wanneer u een groep met Azure Cloud Services-knooppunten maakt, geeft u de grootte van het knooppunt en de OS-familie die worden beschreven in de eerder genoemde artikelen. Voor groepen van Windows rekenknooppunten, wordt er meestal Cloud Services gebruikt.

**Virtuele-machineconfiguratie** biedt Linux- en Windows-installatiekopieën voor rekenknooppunten. Beschikbare compute-knooppuntgrootten worden vermeld in [grootten voor virtuele machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) en [grootten voor virtuele machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). Wanneer u een groep die virtuele-machineconfiguratie knooppunten bevat maakt, moet u de grootte van de knooppunten, de verwijzing van de installatiekopie van virtuele machine en de Batch knooppunt agent SKU worden geïnstalleerd op de knooppunten.

### <a name="virtual-machine-image-reference"></a>Verwijzing naar afbeelding van virtuele machine
De Batch-service wordt gebruikt [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor Linux-rekenknooppunten. U kunt opgeven dat een installatiekopie van het [Azure Marketplace][vm_marketplace], of geef een aangepaste installatiekopie die u hebt voorbereid. Zie [Grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md#pool) voor meer informatie over aangepaste installatiekopieën.

Wanneer u een verwijzing van de installatiekopie van virtuele machine configureert, geeft u de eigenschappen van de installatiekopie van de virtuele machine. De volgende eigenschappen zijn vereist wanneer u een verwijzing van de installatiekopie van virtuele machine maken:

| **Eigenschappen van de verwijzing naar afbeelding** | **Voorbeeld** |
| --- | --- |
| Uitgever |Canonical |
| Aanbieding |UbuntuServer |
| SKU |14.04.4-LTS |
| Versie |meest recente |

> [!TIP]
> U kunt meer informatie over deze eigenschappen en hoe u Marketplace-installatiekopieën in [navigeren door en selecteer installatiekopieën van Linux virtuele machines in Azure met CLI of PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Houd er rekening mee dat niet alle installatiekopieën van Marketplace momenteel compatibel met Batch zijn. Zie voor meer informatie [knooppunt agent SKU](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Knooppunt agent SKU
De Batch-knooppunt-agent is een programma dat wordt uitgevoerd op elk knooppunt in de groep en de opdracht en controle interface vormt tussen het knooppunt en de Batch-service. Er zijn verschillende implementaties van de agent van het knooppunt, SKU's, ook wel voor verschillende besturingssystemen. In wezen, wanneer u de configuratie van een virtuele Machine maakt, u eerst de verwijzing van de installatiekopie van virtuele machine opgeven en vervolgens geeft u de knooppunt-agent installeren op de installatiekopie. Normaal gesproken elke agent knooppunt SKU is compatibel met meerdere installatiekopieën van virtuele machines. Hier volgen enkele voorbeelden van knooppunt agent SKU's:

* batch.node.Ubuntu 14.04
* batch.node.centos 7
* batch.node.Windows amd64

> [!IMPORTANT]
> Niet alle installatiekopieën van virtuele machines die beschikbaar in de Marketplace zijn zijn compatibel met de momenteel beschikbare Batch knooppunt agents. Gebruik de Batch-SDK's voor een lijst met de agent beschikbaar knooppunt SKU's en installatiekopieën van virtuele machines waarmee ze compatibel zijn. Zie de [installatiekopieën van de lijst van de virtuele Machine](#list-of-virtual-machine-images) verderop in dit artikel voor meer informatie en voorbeelden van hoe u een lijst met geldige afbeeldingen tijdens runtime ophaalt.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Een groep met Linux maken: Batch Python
Het volgende codefragment toont een voorbeeld van het gebruik van de [bibliotheek van Microsoft Azure Batch-Client voor Python] [ py_batch_package] voor het maken van een pool van Ubuntu Server rekenknooppunten. Documentatie voor de Batch Python-module kan worden gevonden op [azure.batch pakket] [ py_batch_docs] op de documenten lezen.

In dit fragment maakt een [ImageReference] [ py_imagereference] expliciet en geeft u op elk van de eigenschappen (uitgever, aanbieding, SKU, versie). In de productiecode moet echter raadzaam dat u de [list_node_agent_skus] [ py_list_skus] methode om te bepalen, en selecteer in de beschikbare installatiekopie en knooppunt agent SKU combinaties tijdens runtime.

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

Zoals eerder vermeld, wordt aangeraden in plaats van het maken van de [ImageReference] [ py_imagereference] expliciet, gebruikt u de [list_node_agent_skus] [ py_list_skus] methode dynamisch kiezen uit de combinaties van ondersteunde knooppunt agent/Marketplace-installatiekopie. Het volgende Python-fragment toont hoe u deze methode gebruikt.

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

## <a name="create-a-linux-pool-batch-net"></a>Een groep met Linux maken: Batch .NET
Het volgende codefragment toont een voorbeeld van het gebruik van de [Batch .NET] [ nuget_batch_net] -clientbibliotheek voor het maken van een pool van Ubuntu Server rekenknooppunten. U vindt de [Batch .NET-naslagdocumentatie] [ api_net] op MSDN.

De volgende code codefragment gebruikt de [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] methode te selecteren in de lijst met momenteel ondersteund Marketplace installatiekopie en knooppunt agent SKU combinaties. Deze techniek is het wenselijk omdat de lijst met ondersteunde combinaties van tijd tot tijd kan veranderen. Meest voorkomende worden ondersteunde combinaties toegevoegd.

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

Hoewel het vorige codefragment gebruikt de [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] methode dynamisch weergeven en selecteren van ondersteunde installatiekopie en knooppunt agent SKU combinaties (aanbevolen), kunt u ook configureren een [ImageReference] [ net_imagereference] expliciet:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Lijst van installatiekopieën van virtuele machines
De volgende tabel bevat de Marketplace-installatiekopieën voor virtuele machine die compatibel met de beschikbare Batch knooppunt agents zijn wanneer dit artikel voor het laatst is bijgewerkt. Het is belangrijk te weten dat deze lijst is geen definitieve omdat installatiekopieën en agents knooppunt kunnen worden toegevoegd of verwijderd op elk gewenst moment. Het is raadzaam dat uw Batch-toepassingen en services gebruik altijd [list_node_agent_skus] [ py_list_skus] (Python) en [ListNodeAgentSkus] [ net_list_skus] (Batch .NET) om te bepalen en selecteer in de momenteel beschikbare SKU's.

> [!WARNING]
> De volgende lijst kan op elk gewenst moment wijzigen. Gebruik altijd de **lijst knooppunt agent SKU** methoden die beschikbaar zijn in de Batch-API's voor een lijst met compatibele virtuele machine en knooppunt agent SKU's wanneer u uw Batch-taken uitvoeren.
>
>

| **Uitgever** | **Aanbieding** | **Afbeelding SKU** | **Versie** | **Knooppunt agent SKU-ID** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| Canonical | UbuntuServer | 14.04.5-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonical | UbuntuServer | 16.04.0-LTS | meest recente | batch.node.Ubuntu 16.04 |
| Credativ | Debian | 8 | meest recente | batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.1 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | meest recente | batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | meest recente | batch.node.centos 7 |
| Oracle | Oracle Linux | 7.2 | meest recente | batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | meest recente | batch.node.opensuse 13.2 |
| SUSE | openSUSE Leap | 42.1 | meest recente | batch.node.opensuse 42,1 |
| SUSE | SLES | 12-SP1 | meest recente | batch.node.opensuse 42,1 |
| SUSE | SLES HPC | 12-SP1 | meest recente | batch.node.opensuse 42,1 |
| Microsoft-advertenties | Linux-gegevens-wetenschappelijke-vm | linuxdsvm | meest recente | batch.node.centos 7 |
| Microsoft-advertenties | Standard-gegevens-wetenschappelijke-vm | Standard-gegevens-wetenschappelijke-vm | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter | meest recente | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter met Containers | meest recente | batch.node.Windows amd64 |

## <a name="connect-to-linux-nodes-using-ssh"></a>Verbinding maken met gebruik van SSH Linux-knooppunten
Tijdens de ontwikkeling of bij het oplossen van problemen soms is het nodig zijn om te melden bij de knooppunten in de pool. In tegenstelling tot Windows rekenknooppunten, kunt u Remote Desktop Protocol (RDP) niet gebruiken voor het verbinding maken met Linux-knooppunten. De Batch-service kan in plaats daarvan SSH-toegang op elk knooppunt voor externe verbinding.

Het volgende Python-codefragment maakt een gebruiker op elk knooppunt in een groep die vereist voor externe verbinding is. De verbindingsgegevens secure shell (SSH) voor elk knooppunt wordt afgedrukt.

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

Hier volgt een voorbeeld van uitvoer voor de vorige code voor een pool met vier knooppunten voor Linux:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

U kunt een openbare SSH-sleutel in plaats van een wachtwoord opgeven bij het maken van een gebruiker op een knooppunt. In de SDK voor Python, gebruikt u de **ssh_public_key** parameter op [ComputeNodeUser][py_computenodeuser]. Gebruik in .NET de [ComputeNodeUser][net_computenodeuser].[ SshPublicKey] [ net_ssh_key] eigenschap.

## <a name="pricing"></a>Prijzen
Azure Batch is gebouwd op Azure Cloud Services en virtuele Machines van Azure-technologie. De Batch-service zelf wordt aangeboden zonder kosten, wat betekent dat u in rekening worden gebracht alleen voor de rekenresources dat dat uw Batch-oplossingen gebruiken. Wanneer u de optie **Cloud Services-configuratie**, u in rekening worden gebracht op basis van de [Cloud Services-prijzen] [ cloud_services_pricing] structuur. Wanneer u de optie **Virtuele-machineconfiguratie**, u in rekening worden gebracht op basis van de [virtuele Machines prijzen] [ vm_pricing] structuur. 

Als u toepassingen implementeert voor uw Batch-knooppunten met behulp van [toepassingspakketten](batch-application-packages.md), er zijn ook in rekening gebracht voor de Azure Storage-resources dat uw toepassingspakketten gebruiken. De Azure Storage-kosten zijn in het algemeen minimale. 

## <a name="next-steps"></a>Volgende stappen
### <a name="batch-python-tutorial"></a>Zelfstudie over Batch Python
Bekijk voor een uitgebreidere zelfstudie over het werken met Batch met behulp van Python [aan de slag met de Azure Batch Python-client](batch-python-tutorial.md). De aanvullende [codevoorbeeld] [ github_samples_pyclient] bevat een helperfunctie `get_vm_config_for_distro`, die een andere methode om op te halen van de configuratie van een virtuele machine weergeeft.

### <a name="batch-python-code-samples"></a>Batch Python-codevoorbeelden
De [Python-codevoorbeelden] [ github_samples_py] in de [azure-batch-samples] [ github_samples] opslagplaats op GitHub scripts bevatten die ziet u hoe algemene Batch-bewerkingen, zoals toepassingen, taak en het maken van de taak uitvoeren. De [Leesmij] [ github_py_readme] die wordt meegestuurd met de Python voorbeelden bevat informatie over het installeren van de vereiste pakketten.

### <a name="batch-forum"></a>Batch-forum
De [Azure Batch-Forum] [ forum] is een goede plaats om te bespreken Batch en vragen over de service op MSDN. Lees handig 'vastgemaakt' geboekt en stel uw vragen wanneer deze zich voordoen tijdens het bouwen van uw Batch-oplossingen.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
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
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
