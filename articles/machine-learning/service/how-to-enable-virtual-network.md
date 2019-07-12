---
title: Experimenten en Deductie uitvoeren in een virtueel netwerk
titleSuffix: Azure Machine Learning service
description: Machine learning-experimenten en Deductie beveiligen in een Azure-netwerk worden uitgevoerd. Informatie over het maken van de compute-doelen voor modeltraining en Deductie binnen een virtueel netwerk. Meer informatie over vereisten voor beveiligde virtuele netwerken, zoals vereisen binnenkomende en uitgaande poorten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 07/10/2019
ms.openlocfilehash: 06004f766cb8e9b12c2353bbe5e432e77df03cee
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797703"
---
# <a name="securely-run-experiments-and-inference-inside-an-azure-virtual-network"></a>Veilig kunt uitvoeren van experimenten en Deductie in een Azure-netwerk

In dit artikel leert u hoe u uw experimenten en Deductie binnen een virtueel netwerk uit te voeren. Een virtueel netwerk fungeert als een beveiligingsgrens isoleren van uw Azure-resources op het openbare internet. U kunt ook een Azure-netwerk toevoegen aan uw on-premises netwerk. Hiermee kunt u veilig uw modellen trainen en toegang tot uw geïmplementeerde modellen voor Deductie. Deductie of het model scoren, is de fase waarin het gedistribueerde model wordt gebruikt voor voorspellingen, meestal op productiegegevens.

De Azure Machine Learning-service is afhankelijk van andere Azure-services voor compute-resources. COMPUTE-resources (compute-doelen) worden gebruikt om te trainen en modellen te implementeren. Deze compute-doelen kunnen worden gemaakt binnen een virtueel netwerk. Bijvoorbeeld, kunt u de Microsoft Data Science Virtual Machine naar een model te trainen en implementeer vervolgens het model naar Azure Kubernetes Service (AKS). Zie voor meer informatie over virtuele netwerken, de [overzicht van Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

## <a name="prerequisites"></a>Vereisten

Dit document wordt ervan uitgegaan dat u bekend bent met Azure Virtual Networks en IP-netwerken in het algemeen. Dit document wordt ook van uitgegaan dat u een virtueel netwerk en subnet gebruiken met uw rekenresources hebt gemaakt. Als u niet bekend met Azure Virtual Networks bent, leest u de volgende artikelen voor meer informatie over de service:

* [IP-adressering](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)
* [Beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview)
* [Snelstart: Een virtueel netwerk maken](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)
* [Netwerkverkeer filteren](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

## <a name="storage-account-for-your-workspace"></a>Storage-account voor uw werkruimte

> [!IMPORTANT]
> De __storage-account standaard__ voor uw Azure Machine Learning kan de service worden geplaatst in een virtueel netwerk __alleen tijdens het uitvoeren van experimenten__.
>
> Voor __niet-standaard-storage-accounts voor experimenten__, of als u gebruikmaakt van een opslagaccount voor __Deductie__, hebt u __onbeperkte toegang tot het opslagaccount__.
> 
> Als u niet zeker weet of u deze instellingen hebt gewijzigd of niet, Zie __wijzigen van de standaardtoegangsregel voor netwerk__ in [Azure Storage configureren van firewalls en virtuele netwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security). Gebruik de stappen op toegang via alle netwerken toestaan tijdens Deductie of model scoren.

De standaard Azure Storage-account voor de werkruimte in een virtueel netwerk gebruiken voor het gebruik van plaats de volgende stappen uit:

1. Maak een experimenten-compute ex. Machine Learning-Computing achter een virtueel netwerk of een experimenten-compute koppelen aan de werkruimte ex. HDInsight-cluster of virtuele machine. Zie voor meer informatie, [gebruik Machine Learning-Computing](#use-machine-learning-compute) en [een virtuele machine of de HDInsight-cluster](#use-a-virtual-machine-or-hdinsight-cluster) secties in dit document
2. Ga naar de opslag die is gekoppeld aan de werkruimte. ![Afbeelding van de Azure-portal geeft Azure-opslag die is gekoppeld aan de werkruimte van de Azure Machine Learning-service](./media/how-to-enable-virtual-network/workspace-storage.png)
3. Selecteer op de pagina Azure Storage __Firewalls en virtuele netwerken__. ![Afbeelding van de Azure portal met Firewalls en virtuele netwerken sectie in Azure Storage-pagina](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)
4. Op de __Firewalls en virtuele netwerken__ pagina selecteert u de volgende items:
    - Selecteer __Geselecteerde netwerken__.
    - Onder __virtuele netwerken__, selecteer __bestaand virtueel netwerk toevoegen__ om toe te voegen van het virtuele netwerk waarin uw experimenten-compute zich bevindt. (Zie stap 1.)
    - Selecteer __vertrouwde Microsoft-services voor toegang tot dit storage-account toestaan__.
![Afbeelding van de Azure portal met Firewalls en virtuele netwerken pagina onder Azure Storage](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png) 

5. Tijdens het uitvoeren van experiment, in de code van uw experimenten, wijzigt u de run-configuratie voor het gebruik van blob-opslag:
    ```python
    run_config.source_directory_data_store = "workspaceblobstore"
    ```
    
## <a name="key-vault-for-your-workspace"></a>Key vault voor uw werkruimte
Key Vault-instantie die is gekoppeld aan de werkruimte wordt gebruikt door Azure Machine Learning-service voor het opslaan van referenties van verschillende typen:
* De verbindingsreeks van de gekoppelde storage-account
* Wachtwoorden van opslagplaats voor Azure Container instances
* Verbinding tekenreeksen met gegevens opslaat. 

Voor het gebruik van Azure Machine Learning-experimenten stappen mogelijkheden met Key Vault achter een virtueel netwerk de volgende:
1. Ga naar de Sleutelkluis die is gekoppeld aan de werkruimte. ![Afbeelding van de Azure-portal met Key Vault die is gekoppeld aan de werkruimte van de Azure Machine Learning-service](./media/how-to-enable-virtual-network/workspace-key-vault.png)
2. Selecteer op de pagina Key Vault __Firewalls en virtuele netwerken__ sectie. ![Afbeelding van de Azure portal met Firewalls en virtuele netwerken sectie over Key Vault-pagina](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)
3. Op de __Firewalls en virtuele netwerken__ pagina selecteert u de volgende items:
    - Selecteer __Geselecteerde netwerken__.
    - Onder de __virtuele netwerken__, selecteer __bestaande virtuele netwerken toevoegen__ om toe te voegen van het virtuele netwerk waarin uw experimenten-compute zich bevindt.
    - Selecteer __vertrouwde Microsoft-services voor het overslaan van de firewall toestaan__.
![Afbeelding van de Azure portal met Firewalls en virtuele netwerken pagina onder Key Vault](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png) 


## <a name="use-machine-learning-compute"></a>Gebruik van Machine Learning-Computing

Voor het gebruik van Azure Machine Learning-Computing in een virtueel netwerk, gebruikt u de volgende informatie over vereisten:

- Het virtuele netwerk moet zich in hetzelfde abonnement en dezelfde regio als de werkruimte van de Azure Machine Learning-service.

- Het subnet opgegeven voor de Machine Learning-Computing-cluster moeten hebben onvoldoende vrije IP-adressen voor het aantal virtuele machines het doel voor het cluster. Als het subnet niet genoeg niet-toegewezen IP-adressen, wordt het cluster gedeeltelijk toegewezen.

- Als u van plan bent het virtuele netwerk beveiligen door het beperken van verkeer, laat u bepaalde poorten geopend zijn voor de service Machine Learning-Computing. Zie voor meer informatie, [poorten vereist](#mlcports).

- Controleer of uw beveiligingsbeleid of de vergrendelingen op het abonnement of de resourcegroep van het virtuele netwerk machtigingen voor het beheren van het virtuele netwerk beperken.

- Als u plaatsen van meerdere Machine Learning-Computing-clusters in één virtueel netwerk wilt, moet u mogelijk een verhoging aanvragen voor een of meer van uw resources.

    Machine Learning-Computing, wijst automatisch extra netwerkresources in de resourcegroep waarin het virtuele netwerk toe. Voor elk cluster Machine Learning-Computing wijst de service Azure Machine Learning in de volgende bronnen:

    - Een netwerkbeveiligingsgroep (NSG)

    - Een openbaar IP-adres

    - Een load balancer

  De beperkingen die voor deze resources gelden, worden bepaald door de [resourcequota](https://docs.microsoft.com/azure/azure-subscription-service-limits) van het abonnement.

### <a id="mlcports"></a> Vereiste poorten

Machine Learning-Computing, gebruikt momenteel de Azure Batch-service voor het inrichten van virtuele machines in de opgegeven virtuele netwerk. Het subnet moet binnenkomende communicatie van de Batch-service toestaan. Deze communicatie wordt gebruikt om te plannen wordt uitgevoerd op de Machine Learning-Computing-knooppunten en om te communiceren met Azure Storage en andere bronnen. Batch voegt nsg's toe op het niveau van netwerkinterfaces (NIC's) die zijn gekoppeld aan virtuele machines. Met deze netwerkbeveiligingsgroepen worden automatisch binnenkomende en uitgaande regels geconfigureerd om het volgende verkeer toe te staan:

- Binnenkomend TCP-verkeer op poort 29876 en 29877 uit een __servicetag__ van __BatchNodeManagement__.

    ![Afbeelding van de Azure-portal geeft een binnenkomende regel met behulp van de servicetag BatchNodeManagement](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)
 
- (optioneel) Binnenkomend TCP-verkeer op poort 22 om externe toegang te verlenen. Deze poort is alleen nodig als u wilt verbinding maken met behulp van SSH op het openbare IP-adres.
 
- Uitgaand verkeer op een willekeurige poort naar het virtuele netwerk.

- Uitgaand verkeer op een willekeurige poort naar internet. 

Wees voorzichtig als u wijzigt of regels voor binnenkomend en uitgaand in Batch geconfigureerd nsg's toevoegen. Als een NSG-blokken-communicatie naar de rekenknooppunten, stelt de Machine Learning-Computing services de status van de rekenknooppunten op onbruikbaar.

U hoeft niet te geven van nsg's op het subnetniveau omdat Batch configureert u een eigen nsg's. Echter, als het opgegeven subnet zijn gekoppeld nsg's en/of een firewall, configureren de binnenkomende en uitgaande beveiligingsregels zoals eerder vermeld. 

De volgende schermafbeelding ziet u hoe de configuratie van de Netwerkbeveiligingsgroepregels eruit ziet in de Azure-portal:

![Schermafbeelding van inkomende NSG-regels voor Machine Learning-Computing](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Schermafbeelding van uitgaande NSG-regels voor Machine Learning-Computing](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a> Uitgaande connectiviteit van het virtuele netwerk te beperken

Als u niet wilt gebruiken de standaard regels voor uitgaand verkeer en de uitgaande toegang van uw virtuele netwerk wilt beperken, volgt u de volgende stappen uit:

- Uitgaande internetverbinding met behulp van de NSG-regels weigeren 

- Beperken van uitgaand verkeer naar Azure Storage (met behulp van __servicetag__ van __Storage.Region_Name__ ex. Storage.EastUS), Azure Container Registry (met behulp van __servicetag__ van __AzureContainerRegistry.Region_Name__ ex. AzureContainerRegistry.EastUS), en Azure Machine Learning-service (met behulp van __servicetag__ van __AzureMachineLearning__)

De volgende schermafbeelding ziet u hoe de configuratie van de Netwerkbeveiligingsgroepregels eruit ziet in de Azure-portal:

![Schermafbeelding van uitgaande NSG-regels voor Machine Learning-Computing](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)

### <a name="user-defined-routes-for-forced-tunneling"></a>Gebruiker gedefinieerde routes voor geforceerde tunneling

Als u van geforceerde tunnels met Azure Machine Learning-Computing gebruikmaakt, moet u toevoegen [gebruiker gedefinieerde routes (UDR)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) aan het subnet waarin de compute-resource.

* Een door de gebruiker gedefinieerde route voor elk IP-adres gebruikt door de Azure Batch-service in de regio waar uw resources bestaan. Deze udr's inschakelen voor de batch-service om te communiceren met rekenknooppunten voor het plannen van taken. Als u een lijst met IP-adressen van de Batch-service, neem contact op met ondersteuning voor Azure.

* Uitgaand verkeer naar Azure Storage (vooral URL's van het formulier `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`, en `<account>.blob.core.windows.net`) moet niet worden geblokkeerd door uw on-premises netwerk-apparaat.

Wanneer u de gebruiker gedefinieerde routes toevoegen, de route voor elk gerelateerde Batch-IP-adresvoorvoegsel definiëren en stel __volgende hoptype__ naar __Internet__. De volgende afbeelding toont een voorbeeld van deze UDR in Azure portal:

![Voorbeeld van de gebruiker gedefinieerde route voor een adresvoorvoegsel](./media/how-to-enable-virtual-network/user-defined-route.png)

Zie voor meer informatie de [maken van een Azure Batch-pool in een virtueel netwerk](../../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling) artikel.

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Machine Learning-Computing in een virtueel netwerk maken

Voor het maken van een Machine Learning-Computing-cluster met behulp van Azure portal, de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com), selecteert u de werkruimte van uw Azure Machine Learning-service.

1. In de __toepassing__ sectie, selecteer __Compute__. Selecteer vervolgens __compute toevoegen__. 

    ![Een compute in Azure Machine Learning-service toevoegen](./media/how-to-enable-virtual-network/add-compute.png)

1. Gebruik deze opties voor het configureren van deze compute-resource voor het gebruik van een virtueel netwerk:

    - __Netwerkconfiguratie__: Selecteer __Geavanceerd__.

    - __Resourcegroep__: Selecteer de resourcegroep waarin het virtuele netwerk.

    - __Virtueel netwerk__: Selecteer het virtuele netwerk waarin het subnet.

    - __Subnet__: Selecteer het subnet te gebruiken.

   ![Een schermafbeelding van virtuele-netwerkinstellingen voor machine learning-Computing](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

U kunt ook een Machine Learning-Computing-cluster maken met behulp van de SDK van Azure Machine Learning. De volgende code maakt een nieuwe Machine Learning-Computing-cluster in de `default` subnet van een virtueel netwerk met de naam `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")
    
    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name = vnet_resourcegroup_name,
                                                           vnet_name = vnet_name,
                                                           subnet_name = subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    
    # Wait for the cluster to complete, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Wanneer het proces voor het maken is voltooid, kunt u uw model te trainen met behulp van het cluster. Zie voor meer informatie, [selecteert en een compute-doel gebruiken voor training](how-to-set-up-training-targets.md).

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Een virtuele machine of de HDInsight-cluster

Als u een virtuele machine of een Azure HDInsight-cluster in een virtueel netwerk met uw werkruimte, de volgende stappen uit:

> [!IMPORTANT]
> De service Azure Machine Learning biedt alleen ondersteuning voor virtuele machines met Ubuntu.

1. Een virtuele machine of de HDInsight-cluster maken met behulp van de Azure portal of Azure CLI en plaatst deze in een Azure-netwerk. Zie de volgende documenten voor meer informatie:
    * [Maken en beheren van virtuele Azure-netwerken voor virtuele Linux-machines](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [HDInsight met behulp van een Azure-netwerk uitbreiden](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network) 

1. Als u wilt toestaan dat de service Azure Machine Learning om te communiceren met de SSH-poort op de virtuele machine of het cluster, moet u een vermelding configureren voor de Netwerkbeveiligingsgroep. De SSH-poort is meestal poort 22. Als u wilt dat verkeer van deze bron, gebruik de volgende informatie:

    * __Bron__: selecteer __Servicetag__.

    * __Bronservicetag__: Select __AzureMachineLearning__.

    * __Poortbereiken van bron__: Selecteer __*__ .

    * __Bestemming__: Selecteer __eventuele__.

    * __Poortbereiken van doel__: Selecteer __22__.

    * __Protocol__: Selecteer __eventuele__.

    * __Actie__: selecteer __Toestaan__.

   ![Schermafbeelding van regels voor binnenkomende verbindingen voor het uitvoeren van experimenten op een virtuele machine of de HDInsight-cluster in een virtueel netwerk](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Behoud de standaardwaarde regels voor uitgaand verkeer voor de Netwerkbeveiligingsgroep. Zie voor meer informatie, de standaardregels voor beveiliging in [beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).

    Als u niet wilt gebruiken de standaard regels voor uitgaand verkeer en wilt beperken van de uitgaande toegang tot uw virtuele netwerk, Zie [uitgaande connectiviteit van het virtuele netwerk te beperken](#limiting-outbound-from-vnet)
    
1. De virtuele machine of de HDInsight-cluster koppelen aan uw werkruimte van Azure Machine Learning-service. Zie voor meer informatie, [instellen van compute-doelen voor modeltraining](how-to-set-up-training-targets.md).

## <a name="use-azure-kubernetes-service"></a>Azure Kubernetes-Service gebruiken

> [!IMPORTANT]
> Controleer de vereisten en IP-adressen voor uw cluster voordat u doorgaat met de stappen van plan bent. Zie voor meer informatie, [configureren geavanceerde netwerken in Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/configure-advanced-networking).
> 
>
> Behoud de standaardwaarde regels voor uitgaand verkeer voor de Netwerkbeveiligingsgroep. Zie voor meer informatie, de standaardregels voor beveiliging in [beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).
>
> Azure Kubernetes Service en de Azure-netwerk moeten zich in dezelfde regio bevinden.

Azure Kubernetes Service in een virtueel netwerk toevoegen aan uw werkruimte, volg deze stappen in de Azure-portal:

1. Zorg ervoor dat NSG-groep die binnenkomende regel ingeschakeld voor het gebruik van Azure Machine Learning-service van de besturingselementen voor het virtuele netwerk heeft __servicetag__ van __AzureMachineLearning__

    ![Een compute in Azure Machine Learning-service toevoegen](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)     
 
1. In de [Azure-portal](https://portal.azure.com), selecteert u de werkruimte van uw Azure Machine Learning-service.

1. In de __toepassing__ sectie, selecteer __Compute__. Selecteer vervolgens __compute toevoegen__. 

    ![Een compute in Azure Machine Learning-service toevoegen](./media/how-to-enable-virtual-network/add-compute.png)

1. Gebruik deze opties voor het configureren van deze compute-resource voor het gebruik van een virtueel netwerk:

    - __Netwerkconfiguratie__: Selecteer __Geavanceerd__.

    - __Resourcegroep__: Selecteer de resourcegroep waarin het virtuele netwerk.

    - __Virtueel netwerk__: Selecteer het virtuele netwerk waarin het subnet.

    - __Subnet__: Selecteer het subnet.

    - __Kubernetes-Service-adresbereik__: Selecteer het adresbereik van Kubernetes-service. Dit adresbereik maakt gebruik van een CIDR-notatie IP-adresbereik voor het definiëren van de IP-adressen beschikbaar zijn voor het cluster. Deze mag niet overlappen met subnet IP-adresbereiken. Bijvoorbeeld: 10.0.0.0/16.

    - __IP-adres van Kubernetes DNS-service__: Selecteer het IP-adres van de Kubernetes-DNS-service. Dit IP-adres is toegewezen aan de Kubernetes-DNS-service. Het moet binnen het adresbereik van Kubernetes-service. Bijvoorbeeld: 10.0.0.10.

    - __Docker bridge-adres__: Selecteer het Docker bridge-adres. Dit IP-adres is toegewezen aan Docker Bridge. Het moet niet in elk subnet IP-adresbereiken of het adresbereik van Kubernetes-service. Bijvoorbeeld: 172.17.0.1/16.

   ![Azure Machine Learning-service: Machine Learning-Computing virtuele netwerkinstellingen](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Zorg ervoor dat NSG-groep die besturingselementen voor die het virtuele netwerk heeft de binnenkomende regel voor het scoring-eindpunt ingeschakeld zodat deze kan worden aangeroepen vanuit buiten het virtuele netwerk

    ![Een compute in Azure Machine Learning-service toevoegen](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)

    > [!TIP]
    > Als u al een AKS-cluster in een virtueel netwerk hebt, kunt u deze koppelen aan de werkruimte. Zie voor meer informatie, [implementeren naar AKS](how-to-deploy-to-aks.md).

U kunt ook de **SDK van Azure Machine Learning** Azure Kubernetes Service toevoegen in een virtueel netwerk. De volgende code maakt een nieuw Azure Kubernetes Service-exemplaar in de `default` subnet van een virtueel netwerk met de naam `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace = ws,
                                  name = "myaks",
                                  provisioning_configuration = config)
```

Wanneer het proces voor het maken is voltooid, kunt u Deductie/score op een AKS-cluster achter een virtueel netwerk. Zie voor meer informatie, [implementeren naar AKS](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Volgende stappen

* [Training omgevingen instellen](how-to-set-up-training-targets.md)
* [Waar u modellen implementeren](how-to-deploy-and-where.md)
* [Veilig Implementeer modellen met SSL](how-to-secure-web-service.md)

