---
title: Experimenten en interferentie uitvoeren in een virtueel netwerk
titleSuffix: Azure Machine Learning service
description: Voer machine learning experimenten uit en verlaag de beveiliging in een virtueel Azure-netwerk. Meer informatie over het maken van reken doelen voor model training en het afwijzen van een virtueel netwerk. Meer informatie over de vereisten voor beveiligde virtuele netwerken, zoals het vereisen van binnenkomende en uitgaande poorten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 08/05/2019
ms.openlocfilehash: 7c4c4ff611b35cac9aa8be1a9697a0d11bc4dc8b
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815961"
---
# <a name="securely-run-experiments-and-inference-inside-an-azure-virtual-network"></a>Veilig experimenten en demijnen uitvoeren in een virtueel Azure-netwerk

In dit artikel leert u hoe u uw experimenten en interferentie kunt uitvoeren in een virtueel netwerk. Een virtueel netwerk fungeert als beveiligings grens en isoleert uw Azure-resources van het open bare Internet. U kunt ook een virtueel Azure-netwerk toevoegen aan uw on-premises netwerk. Het biedt u de mogelijkheid om uw modellen veilig te trainen en toegang te krijgen tot uw geïmplementeerde modellen. Defactorion of model Score is de fase waarin het geïmplementeerde model wordt gebruikt voor de voor spelling, meestal op productie gegevens.

De Azure Machine Learning-service is afhankelijk van andere Azure-Services voor reken resources. Reken bronnen (Compute-doelen) worden gebruikt om modellen te trainen en te implementeren. Deze reken doelen kunnen binnen een virtueel netwerk worden gemaakt. U kunt bijvoorbeeld de micro soft-Data Science Virtual Machine gebruiken om een model te trainen en het model vervolgens te implementeren in azure Kubernetes service (AKS). Zie [overzicht van Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)voor meer informatie over virtuele netwerken.

In dit artikel vindt u gedetailleerde informatie over **Geavanceerde beveiligings instellingen**en is niet nodig voor Basic-en experimentele gebruiks scenario's. In de secties in dit artikel vindt u informatie over de configuratie voor verschillende scenario's, maar hoeft u niet in de juiste volg orde of volledig te worden voltooid.

## <a name="prerequisites"></a>Vereisten

Maak een Azure Machine Learning service- [werk ruimte](setup-create-workspace.md) als u er nog geen hebt. In dit document wordt ervan uitgegaan dat u bekend bent met virtuele Azure-netwerken en IP-netwerken in het algemeen. In dit document wordt ook ervan uitgegaan dat u een virtueel netwerk en subnet hebt gemaakt voor gebruik met uw reken resources. Als u niet bekend bent met virtuele Azure-netwerken, raadpleegt u de volgende artikelen voor meer informatie over de service:

* [IP-adressering](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)
* [Beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/security-overview)
* [Snelstart: Een virtueel netwerk maken](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)
* [Netwerkverkeer filteren](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

## <a name="storage-account-for-your-workspace"></a>Opslag account voor uw werk ruimte

Voer de volgende stappen uit om de standaard Azure Storage-account te gebruiken voor de werk ruimte in een virtueel netwerk:

1. Maak een experimenteel reken proces af. Machine Learning Compute achter een virtueel netwerk of koppel een experimenten-Compute aan de werk ruimte af. HDInsight-cluster of virtuele machine. Zie [machine learning Compute gebruiken](#use-machine-learning-compute) en [een virtuele machine of HDInsight-cluster](#use-a-virtual-machine-or-hdinsight-cluster) secties in dit document gebruiken voor meer informatie.
2. Ga naar de opslag die is gekoppeld aan de werk ruimte. ![Afbeelding van de Azure Portal met Azure Storage die is gekoppeld aan de werk ruimte van de Azure Machine Learning service](./media/how-to-enable-virtual-network/workspace-storage.png)
3. Selecteer op de pagina Azure Storage __firewalls en virtuele netwerken__. ![Afbeelding van het gedeelte Azure Portal weer geven firewalls en virtuele netwerken op Azure Storage pagina](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)
4. Selecteer op de pagina __firewalls en virtuele netwerken__ de volgende vermeldingen:
    - Selecteer __Geselecteerde netwerken__.
    - Selecteer onder __virtuele netwerken__ __bestaande virtuele netwerk toevoegen__ om het virtuele netwerk toe te voegen waarin uw experimenten worden berekend. (Zie stap 1.)
    - Selecteer __vertrouwde micro soft-Services toegang geven tot dit opslag account__.
![Afbeelding van de Azure Portal pagina firewalls en virtuele netwerken weer geven onder Azure Storage](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)

5. Tijdens het uitvoeren van een experiment wijzigt u in de code voor het uitvoeren van de run-configuratie voor het gebruik van Blob-opslag:
    ```python
    run_config.source_directory_data_store = "workspaceblobstore"
    ```

> [!IMPORTANT]
> Het __standaard opslag account__ voor uw Azure machine learning-service kan alleen in een virtueel netwerk worden geplaatst __tijdens het experimenteren__.
>
> Voor __niet-standaard opslag accounts voor experimenten__of als u een opslag account gebruikt voor demijnen, moet u __onbeperkte toegang hebben tot het opslag account__.
>
> Als u niet zeker weet of u deze instellingen hebt gewijzigd, raadpleegt u __de standaard netwerk toegangs regel wijzigen__ in [Azure Storage firewalls en virtuele netwerken configureren](https://docs.microsoft.com/azure/storage/common/storage-network-security). Gebruik de stappen om toegang tot alle netwerken te verlenen tijdens de interferentie of het model leren.

## <a name="key-vault-for-your-workspace"></a>Sleutel kluis voor uw werk ruimte

Het Key Vault exemplaar dat is gekoppeld aan de werk ruimte wordt gebruikt door de Azure Machine Learning-service om referenties van verschillende soorten op te slaan:
* Het gekoppelde opslag account connection string
* Wacht woorden voor Azure container repository-instanties
* Verbindings reeksen voor gegevens archieven.

Als u Azure Machine Learning experimenten wilt gebruiken met Key Vault achter een virtueel netwerk, gebruikt u de volgende stappen:
1. Ga naar de Key Vault die aan de werk ruimte is gekoppeld. ![Afbeelding van de Azure Portal met Key Vault die is gekoppeld aan de werk ruimte Azure Machine Learning service](./media/how-to-enable-virtual-network/workspace-key-vault.png)
2. Selecteer op de pagina Key Vault de sectie __firewalls en virtuele netwerken__ . ![Afbeelding van het gedeelte Azure Portal weer geven firewalls en virtuele netwerken op Key Vault pagina](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)
3. Selecteer op de pagina __firewalls en virtuele netwerken__ de volgende vermeldingen:
    - Selecteer __Geselecteerde netwerken__.
    - Onder de __virtuele netwerken__selecteert u __bestaande virtuele netwerken toevoegen__ om het virtuele netwerk toe te voegen waarin uw experimenten reken kracht zich bevindt.
    - Selecteer __vertrouwde micro soft-Services toestaan deze firewall te omzeilen__.
![Afbeelding van de Azure Portal pagina firewalls en virtuele netwerken weer geven onder Key Vault](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)


## <a name="use-machine-learning-compute"></a>Machine Learning Compute gebruiken

Als u Azure Machine Learning Compute in een virtueel netwerk wilt gebruiken, moet u rekening houden met de volgende informatie over netwerk vereisten:

- Het virtuele netwerk moet zich in hetzelfde abonnement en dezelfde regio bevinden als de Azure Machine Learning service-werk ruimte.

- Het opgegeven subnet voor het berekenings cluster moet voldoende niet-toegewezen IP-adressen hebben om het aantal Vm's voor het cluster te kunnen ondersteunen. Als het subnet onvoldoende niet-toegewezen IP-adressen heeft, wordt het cluster gedeeltelijk toegewezen.

- Als u het virtuele netwerk wilt beveiligen door verkeer te beperken, moet u sommige poorten voor de compute-service geopend laten. Zie [vereiste poorten](#mlcports)voor meer informatie.

- Controleer of het beveiligings beleid of de vergren delingen in het abonnement of de resource groep van het virtuele netwerk beperken machtigingen voor het beheren van het virtuele netwerk.

- Als u meerdere reken clusters in één virtueel netwerk wilt plaatsen, moet u mogelijk een quotum verhoging aanvragen voor een of meer van uw resources.

    Azure Machine Learning Compute wijst automatisch extra netwerk bronnen toe aan de resource groep die het virtuele netwerk bevat. Voor elk berekenings cluster wijst de service de volgende resources toe:

    - Eén netwerk beveiligings groep

    - Eén openbaar IP-adres

    - Een load balancer

  De beperkingen die voor deze resources gelden, worden bepaald door de [resourcequota](https://docs.microsoft.com/azure/azure-subscription-service-limits) van het abonnement.

### <a id="mlcports"></a>Vereiste poorten

Machine Learning Compute maakt momenteel gebruik van de Azure Batch-service om virtuele machines in het opgegeven virtuele netwerk in te richten. Het subnet moet inkomende communicatie vanuit de batch-service toestaan. Deze communicatie wordt gebruikt voor het plannen van uitvoeringen op de Machine Learning Compute knooppunten en om te communiceren met Azure Storage en andere resources. Batch voegt netwerk beveiligings groepen (**NSG**) toe op het niveau van de netwerk interfaces (**NIC**) die zijn gekoppeld aan vm's. Met deze netwerkbeveiligingsgroepen worden automatisch binnenkomende en uitgaande regels geconfigureerd om het volgende verkeer toe te staan:

- Binnenkomend TCP-verkeer op de poorten 29876 en 29877 van een __service label__ van __BatchNodeManagement__.

    ![Afbeelding van de Azure Portal een regel voor binnenkomende verbindingen met de code van de BatchNodeManagement-service weer geven](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- Beschrijving Binnenkomend TCP-verkeer op poort 22 om externe toegang toe te staan. Deze poort is alleen nodig als u verbinding wilt maken met behulp van SSH op het open bare IP-adres.

- Uitgaand verkeer op een willekeurige poort naar het virtuele netwerk.

- Uitgaand verkeer op een willekeurige poort naar internet.

Wees voorzichtig als u regels voor binnenkomend/uitgaand verkeer wijzigt of toevoegt in batch-geconfigureerde Nsg's. Als een NSG communicatie met de reken knooppunten blokkeert, stelt compute-service de status van de reken knooppunten in op onbruikbaar.

U hoeft geen Nsg's op te geven op het niveau van het subnet omdat Azure Batch-service zijn eigen Nsg's configureert. Als het opgegeven subnet echter is gekoppeld Nsg's en/of een firewall, configureert u de regels voor binnenkomende en uitgaande verbindingen zoals eerder beschreven.

In de volgende scherm afbeelding ziet u hoe de NSG-regel configuratie in het Azure Portal ziet:

![Scherm opname van binnenkomende NSG-regels voor Machine Learning Compute](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Scherm opname van regels voor uitgaande NSG voor Machine Learning Compute](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a>Uitgaande verbindingen van het virtuele netwerk beperken

Als u de standaard regels voor uitgaande verbindingen niet wilt gebruiken en de uitgaande toegang tot het virtuele netwerk wilt beperken, voert u de volgende stappen uit:

- Uitgaande Internet verbinding weigeren met de NSG-regels

- Beperk het uitgaande verkeer tot Azure Storage (met behulp van het __service label__ __Storage. Region_Name__ ex. Opslag. Oostus), Azure Container Registry (met behulp van het __service label__ __AzureContainerRegistry. Region_Name__ ex. AzureContainerRegistry. Eastus) en Azure Machine Learning service (met behulp van het __service label__ __AzureMachineLearning__)

In de volgende scherm afbeelding ziet u hoe de NSG-regel configuratie in het Azure Portal ziet:

![Scherm opname van regels voor uitgaande NSG voor Machine Learning Compute](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)

### <a name="user-defined-routes-for-forced-tunneling"></a>Door de gebruiker gedefinieerde routes voor geforceerde tunneling

Als u gebruikmaakt van geforceerde tunneling met Azure Machine Learning compute, moet u door de [gebruiker gedefinieerde routes (UDR)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) toevoegen aan het subnet dat de reken resource bevat.

* Er moet een door de gebruiker gedefinieerde route worden ingesteld voor elk IP-adres dat wordt gebruikt door de Azure Batch-service in de regio waar uw resources bestaan. Deze Udr's inschakelen de batch-service om te communiceren met reken knooppunten voor het plannen van taken. Neem contact op met de ondersteuning van Azure voor een lijst met de IP-adressen van de batch-service.

* Uitgaand verkeer naar Azure Storage (met name url's van `<account>.table.core.windows.net`het `<account>.queue.core.windows.net`formulier, `<account>.blob.core.windows.net`en) moet niet worden geblokkeerd door uw on-premises netwerk apparaat.

Wanneer u de door de gebruiker gedefinieerde routes toevoegt, definieert u de route voor elk gerelateerde IP-adres voorvoegsel voor batch en stelt u het __type volgende hop__ in op __Internet__. In de volgende afbeelding ziet u een voor beeld van deze UDR in de Azure Portal:

![Voor beeld van een door de gebruiker gedefinieerde route voor een adres voorvoegsel](./media/how-to-enable-virtual-network/user-defined-route.png)

Zie de [groep een Azure batch maken in een virtueel netwerk](../../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling) voor meer informatie.

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Machine Learning Compute in een virtueel netwerk maken

Als u een Azure Machine Learning Compute-cluster wilt maken met behulp van de Azure Portal, gebruikt u de volgende stappen:

1. Selecteer in de [Azure Portal](https://portal.azure.com)uw Azure machine learning service-werk ruimte.

1. Selecteer in de sectie __toepassing__ __Compute__. Selecteer vervolgens __Compute toevoegen__.

1. Als u deze reken resource wilt configureren voor het gebruik van een virtueel netwerk, gebruikt u de volgende opties:

    - __Netwerk configuratie__: Selecteer __Geavanceerd__.

    - __Resourcegroep__: Selecteer de resource groep die het virtuele netwerk bevat.

    - __Virtueel netwerk__: Selecteer het virtuele netwerk dat het subnet bevat.

    - __Subnet__: Selecteer het subnet dat u wilt gebruiken.

   ![Een scherm opname met de instellingen van het virtuele netwerk voor machine learning compute](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

U kunt ook een Machine Learning Compute cluster maken met behulp van de Azure Machine Learning SDK. Met de volgende code wordt een nieuw machine learning Compute Cluster gemaakt `default` in het subnet van een virtueel `mynetwork`netwerk met de naam:

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
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to complete, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Wanneer het maken van het proces is voltooid, traint u uw model met behulp van het cluster in een experiment. Zie voor meer informatie [een berekenings doel selecteren en gebruiken voor training](how-to-set-up-training-targets.md).

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Een virtuele machine of een HDInsight-cluster gebruiken

Als u een virtuele machine of een Azure HDInsight-cluster in een virtueel netwerk met uw werk ruimte wilt gebruiken, gebruikt u de volgende stappen:

1. Maak een VM-of HDInsight-cluster met behulp van de Azure Portal of Azure CLI en plaats het in een virtueel Azure-netwerk. Raadpleeg de volgende documenten voor meer informatie:
    * [Virtuele Azure-netwerken voor Linux-Vm's maken en beheren](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [HDInsight uitbreiden met behulp van een virtueel Azure-netwerk](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Als u wilt dat de Azure Machine Learning-service kan communiceren met de SSH-poort op de virtuele machine of het cluster, moet u een bron vermelding voor de netwerk beveiligings groep configureren. De SSH-poort is doorgaans poort 22. Gebruik de volgende stappen om verkeer van deze bron toe te staan:

    * __Bron__: selecteer __Servicetag__.

    * __Bronservicetag__: Selecteer __AzureMachineLearning__.

    * __Poort bereik van bron__: Selecteren __*__ .

    * __Doel__: Selecteer __een__.

    * __Poortbereiken van doel__: Selecteer __22__.

    * __Protocol__: Selecteer __een__.

    * __Actie__: selecteer __Toestaan__.

   ![Scherm opname van regels voor binnenkomende verbindingen voor het uitvoeren van experimenten op een VM of HDInsight-cluster in een virtueel netwerk](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Behoud de standaard regels voor uitgaande verbindingen voor de netwerk beveiligings groep. Zie de standaard beveiligings regels in [beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)voor meer informatie.

    Als u de standaard regels voor uitgaand verkeer niet wilt gebruiken en de uitgaande toegang tot het virtuele netwerk wilt beperken, raadpleegt u [uitgaande verbindingen beperken vanuit het virtuele netwerk](#limiting-outbound-from-vnet)

1. Koppel de virtuele machine of het HDInsight-cluster aan uw Azure Machine Learning service-werk ruimte. Zie COMPUTE- [doelen voor model training instellen](how-to-set-up-training-targets.md)voor meer informatie.

> [!IMPORTANT]
> De Azure Machine Learning-service ondersteunt alleen virtuele machines waarop Ubuntu wordt uitgevoerd.

## <a name="use-azure-kubernetes-service"></a>Azure Kubernetes-service gebruiken

Als u de Azure Kubernetes-service in een virtueel netwerk wilt toevoegen aan uw werk ruimte, voert u de volgende stappen uit in de Azure Portal:

1. Zorg ervoor dat voor de netwerk beveiligings groep (NSG) die het virtuele netwerk beheert, een binnenkomende regel is ingeschakeld voor Azure Machine Learning service met behulp van __AzureMachineLearning__ als de **bron**.

    ![Een compute toevoegen in Azure Machine Learning-service](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)

1. Selecteer in de [Azure Portal](https://portal.azure.com)uw Azure machine learning service-werk ruimte.

1. Selecteer in de sectie __toepassing__ __Compute__. Selecteer vervolgens __Compute toevoegen__.

1. Als u deze reken resource wilt configureren voor het gebruik van een virtueel netwerk, gebruikt u de volgende opties:

    - __Netwerk configuratie__: Selecteer __Geavanceerd__.

    - __Resourcegroep__: Selecteer de resource groep die het virtuele netwerk bevat.

    - __Virtueel netwerk__: Selecteer het virtuele netwerk dat het subnet bevat.

    - __Subnet__: Selecteer het subnet.

    - __Adres bereik__van de Kubernetes-service: Selecteer het adres bereik van de Kubernetes-service. Dit adres bereik maakt gebruik van een CIDR-notatie IP-bereik voor het definiëren van de beschik bare IP-adressen voor het cluster. Het mag niet overlappen met subnet IP-bereiken. Bijvoorbeeld: 10.0.0.0/16.

    - __IP-adres van Kubernetes DNS-service__: Selecteer het IP-adres van de DNS-service Kubernetes. Dit IP-adres wordt toegewezen aan de DNS-service Kubernetes. Deze moet zich in het adres bereik van de Kubernetes-service bevallen. Bijvoorbeeld: 10.0.0.10.

    - __Docker Bridge-adres__: Selecteer het adres van de docker-brug. Dit IP-adres wordt toegewezen aan docker-brug. De waarde mag zich niet in een subnet-IP-bereik of het adres bereik van de Kubernetes-service bevinden. Bijvoorbeeld: 172.17.0.1/16.

   ![Azure Machine Learning-service: Instellingen van het virtuele netwerk Machine Learning Compute](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Zorg ervoor dat voor de NSG-groep die het virtuele netwerk beheert een binnenkomende beveiligings regel is ingeschakeld voor het Score-eind punt, zodat deze kan worden aangeroepen buiten het virtuele netwerk.

    ![Een compute toevoegen in Azure Machine Learning-service](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)

    > [!TIP]
    > Als u al een AKS-cluster in een virtueel netwerk hebt, kunt u het toevoegen aan de werk ruimte. Zie [implementeren op AKS](how-to-deploy-to-aks.md)voor meer informatie.

> [!IMPORTANT]
> Controleer de vereisten en plan IP-adres sering voor uw cluster voordat u verdergaat met de bovenstaande stappen. Zie [geavanceerde netwerken configureren in azure Kubernetes service](https://docs.microsoft.com/azure/aks/configure-advanced-networking)voor meer informatie.
>
>
> Behoud de standaard regels voor uitgaande verbindingen voor de NSG. Zie de standaard beveiligings regels in [beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)voor meer informatie.
>
> De Azure Kubernetes-service en het virtuele Azure-netwerk moeten zich in dezelfde regio bevinden.

U kunt ook de **Azure machine learning SDK** gebruiken om de Azure Kubernetes-service toe te voegen aan een virtueel netwerk. Met de volgende code wordt een nieuw exemplaar van de Azure Kubernetes `default` -service gemaakt in het subnet `mynetwork`van een virtueel netwerk met de naam:

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
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

Wanneer het maken van het proces is voltooid, kunt u een AKS-cluster buiten gebruik stellen achter een virtueel netwerk. Zie [implementeren op AKS](how-to-deploy-to-aks.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* [Trainings omgevingen instellen](how-to-set-up-training-targets.md)
* [Waar u modellen implementeren](how-to-deploy-and-where.md)
* [Modellen veilig implementeren met SSL](how-to-secure-web-service.md)

