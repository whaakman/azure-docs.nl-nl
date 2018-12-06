---
title: Compute-doelen voor modeltraining met Azure Machine Learning-service instellen | Microsoft Docs
description: Informatie over het selecteren en configureren van de training-omgevingen (compute-doelen) gebruikt voor het trainen van uw machine learning-modellen. De service Azure Machine Learning kunt u eenvoudig over training van omgeving. Start lokaal training en als u nodig hebt om uit te schalen, gaat u naar een cloud-gebaseerde compute-doel.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: 45a5e4c895a0c7a8f76bb34aa5aaf22fa31f4333
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864856"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Selecteer en gebruik een compute-doel aan uw model te trainen

Met de Azure Machine Learning-service, kunt u uw model op verschillende compute-resources te trainen. Deze compute-resources, met de naam __compute-doelen__, lokaal of in de cloud kan zijn. In dit document leert u over de ondersteunde compute-doelen en het gebruik ervan.

Een compute-doel is een resource waar uw trainingsscript wordt uitgevoerd, of het model bij de implementatie als een webservice wordt gehost. U kunt maken en beheren van een compute-doel met behulp van de SDK van Azure Machine Learning, Azure-portal of Azure CLI. Als u de compute-doelen die zijn gemaakt via een andere service (bijvoorbeeld: een HDInsight-cluster) hebt, kunt u ze kunt gebruiken door ze te koppelen aan uw werkruimte van Azure Machine Learning-service.

Er zijn drie hoofdcategorieën worden onderverdeeld van compute-doelen die ondersteuning biedt voor Azure Machine Learning:

* __Lokale__: de lokale computer of een cloud-gebaseerde VM die u gebruikt als een dev/experimentele omgeving. 

* __Beheerde Compute__: Azure Machine Learning-Computing is een compute-aanbieding die wordt beheerd door de Azure Machine Learning-service. Hiermee kunt u één of meerdere node compute voor trainingen, testen en batch inferentietaken eenvoudig kunt maken.

* __Gekoppelde Compute__: U kunt ook doen om uw eigen Azure-cloud-computing en verbindt u deze met Azure Machine Learning. Lees meer hierover op ondersteunde rekentypen en het gebruik ervan.


## <a name="supported-compute-targets"></a>Ondersteunde compute-doelen

Azure Machine Learning-service heeft verschillende ondersteuning voor de verschillende compute-doelen. Een typische model ontwikkelingscyclus begint met dev/experimenten op een kleine hoeveelheid gegevens. In deze fase, wordt u aangeraden een lokale omgeving. Bijvoorbeeld, de lokale computer of een cloud-gebaseerde VM. Als u uw training voor grotere gegevenssets opschalen of gedistribueerde training doen, wordt u aangeraden een één of meerdere node cluster maken dat automatisch wordt geschaald telkens wanneer die u een uitvoering verzenden met Azure Machine Learning-Computing. U kunt ook uw eigen compute-resource koppelen, hoewel ondersteuning voor verschillende scenario's als variëren kunnen hieronder uitgelegd:

|COMPUTE-doel| GPU-versnelling | Geautomatiseerde hyperparameter afstemmen | Geautomatiseerde Machine Learning | Geschikt voor pijplijn|
|----|:----:|:----:|:----:|:----:|
|[Lokale computer](#local)| Misschien | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning-Computing](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Externe virtuele machine](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ Azure Databricks en Azure Data Lake Analytics kunt __alleen__ worden gebruikt in een pijplijn. Zie voor meer informatie over pijplijnen, de [pijplijnen in Azure Machine Learning](concept-ml-pipelines.md) document.

> [!IMPORTANT]
> Azure Machine Learning-Computing moet worden gemaakt uit in een werkruimte. U kunt bestaande exemplaren niet koppelen aan een werkruimte.
>
> Andere compute-doelen moeten worden gemaakt van buiten Azure Machine Learning en vervolgens gekoppeld aan uw werkruimte.

> [!NOTE]
> Sommige compute-doelen afhankelijk zijn van Docker-containerinstallatiekopieën bij het trainen van een model. De GPU-basisinstallatiekopie moet alleen worden gebruikt op Microsoft Azure-Services. Voor het trainen van het model, worden deze services zijn:
>
> * Azure Machine Learning-Computing
> * Azure Kubernetes Service
> * De Data Science Virtual Machine.

## <a name="workflow"></a>Werkstroom

De werkstroom voor het ontwikkelen en implementeren van een model met Azure Machine Learning met de volgende stappen:

1. Machine learning-scripts in Python training te ontwikkelen.
1. Maken en configureren of koppelen van een bestaande compute-doel.
1. Verzenden van de trainingsscripts naar de compute-doel.
1. Inspecteer de resultaten om het beste model.
1. Registreer het model in het register model.
1. Het model implementeren.

> [!IMPORTANT]
> Uw trainingsscript is niet gebonden aan een specifieke compute-doel. U kunt in eerste instantie trainen op uw lokale computer en vervolgens overschakelen van compute-doelen zonder dat u hoeft te herschrijven van het trainingsscript.

> [!TIP]
> Wanneer u een compute-doel aan uw werkruimte koppelt, door het maken van beheerde compute of koppelen van bestaande compute, moet u een naam voor uw computer op te geven. Dit moet tussen 2 en 16 tekens lang zijn.

Overstappen van een compute-doel naar een andere omvat het maken van een [uitvoerconfiguratie](concept-azure-machine-learning-architecture.md#run-configuration). De configuratie van de uitvoering wordt gedefinieerd hoe u het script uitvoert op de compute-doel.

## <a name="training-scripts"></a>Trainingsscripts

Wanneer u een uitvoering training start, wordt een momentopname van de map met uw trainingsscripts gemaakt en verzonden naar de compute-doel. Zie voor meer informatie, [momentopnamen](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Lokale computer

Bij het trainen van lokaal, kunt u de SDK gebruiken om in te dienen de trainingsbewerking. U kunt trainen met behulp van een gebruiker of systeem beheerde omgeving.

### <a name="user-managed-environment"></a>Gebruiker-beheerde omgeving

In een omgeving gebruiker wordt beheerd bent u verantwoordelijk om ervoor te zorgen dat de vereiste pakketten beschikbaar in de Python-omgeving die u uitvoeren van het script zijn wilt. Het volgende codefragment wordt een voorbeeld van de configuratie van de training voor een gebruiker beheerde omgeving:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Systeem-beheerde omgeving

Door systeem beheerde omgevingen, is afhankelijk van conda om de afhankelijkheden te beheren. Conda maakt u een bestand met de naam `conda_dependencies.yml` die een lijst van de afhankelijkheden bevat. Vervolgens kunt u het systeem een nieuwe conda-omgeving bouwen en uitvoeren van scripts in deze vragen. Door systeem beheerde omgevingen kunnen zich opnieuw gebruikte later, zolang de `conda_dependencies.yml` bestanden ongewijzigd blijft. 

De eerste installatie van een nieuwe omgeving kan enige tijd duren om uit te voeren, afhankelijk van de grootte van de vereiste afhankelijkheden. Het volgende codefragment ziet u het maken van een systeem-beheerde omgeving die afhankelijk is van scikit-informatie:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Azure Machine Learning-Computing

Azure Machine Learning-Computing is een beheerde rekeninfrastructuur waarmee de gebruiker één - op meerdere - node compute eenvoudig kunt maken. Deze is gemaakt __binnen de regio van uw werkruimte__ en is een resource die kan worden gedeeld met andere gebruikers in uw werkruimte. Het schalen van automatisch als een taak wordt verzonden en kan worden geplaatst in een Azure-netwerk. Deze wordt uitgevoerd een __beperkte omgeving__, verpakking van uw model afhankelijkheden in een Docker-container.

U kunt Azure Machine Learning-Computing gebruiken voor het distribueren van het trainingsproces in een cluster van de CPU of GPU-computerknooppunten in de cloud. Zie voor meer informatie over de VM-grootten die GPU's omvatten de [GPU VM-grootten geoptimaliseerd](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu) documentatie.

> [!NOTE]
> Azure Machine Learning-Computing kent standaardlimieten voor zaken zoals het aantal kernen dat kan worden toegewezen. Zie voor meer informatie de [beheren en aanvraag quota's voor Azure-resources](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas) document.

U kunt Azure Machine Learning-Computing op aanvraag maken bij het plannen van een uitvoering, of als een permanente resource.

### <a name="run-based-creation"></a>Uitvoeren op basis van het maken

U kunt Azure Machine Learning-Computing maken als een compute-doel tijdens de uitvoering. In dit geval de rekenresources voor uw uitvoering, schaalbaar tot max_nodes die u in de configuratie uitvoeren opgeeft, wordt automatisch gemaakt en wordt vervolgens __automatisch verwijderd__ nadat de uitvoering is voltooid.

Deze functionaliteit is momenteel beschikbaar als Preview en werkt niet met Hyperparameter afstemmen of Machine Learning geautomatiseerde taken.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

#Let us first list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use AmlCompute to execute script.
run_config.target = "amlcompute"

# AmlCompute will be created in the same region as workspace. Set vm size for AmlCompute from the list returned above
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Permanente compute (basis)

Een permanente Azure Machine Learning-Computing kan worden hergebruikt voor meerdere taken. Deze kan worden gedeeld met andere gebruikers in de werkruimte en tussen taken wordt bewaard.

Als u wilt een permanente bron voor Azure Machine Learning-Computing maakt, geeft u de `vm_size` en `max_nodes` parameters. Azure Machine Learning maakt vervolgens gebruik van slimme standaardinstellingen voor de rest van de parameters.  Bijvoorbeeld, is de compute ingesteld op automatisch schalen naar nul knooppunten wanneer niet wordt gebruikt en te maken van de toegewezen virtuele machines om uit te voeren van uw taken, indien nodig. 

* **vm_size**: VM-reeks van de knooppunten die zijn gemaakt door Azure Machine Learning-Computing.
* **max_nodes**: maximum aantal knooppunten automatisch te schalen, terwijl een taak wordt uitgevoerd op Azure Machine Learning-Computing.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Permanente compute (Geavanceerd)

U kunt ook verschillende geavanceerde eigenschappen configureren bij het maken van Azure Machine Learning-Computing.  Deze eigenschappen kunnen u een permanente cluster vaste grootte of binnen een bestaand virtueel Azure-netwerk maken in uw abonnement.

Naast `vm_size` en `max_nodes`, kunt u de volgende eigenschappen:

* **min_nodes**: minimale knooppunten (standaard 0 knooppunten) te verkleinen terwijl een taak wordt uitgevoerd op Azure Machine Learning-Computing.
* **vm_priority**: kiezen tussen 'toegewezen' (standaard) en 'lowpriority' virtuele machines bij het maken van Azure Machine Learning-Computing. VM's met lage prioriteit overtollige capaciteit van Azure gebruiken en zijn dus goedkoper maar risico's van uw bezet wordt uitgevoerd.
* **idle_seconds_before_scaledown**: niet-actieve tijd (standaard 120 seconden) moet worden gewacht na voltooiing van uitvoering voor automatisch schalen naar min_nodes.
* **vnet_resourcegroup_name**: resourcegroep van de __bestaande__ virtueel netwerk. Azure Machine Learning-Computing wordt gemaakt in dit virtuele netwerk.
* **vnet_name**: naam van het virtuele netwerk. Het virtuele netwerk moet zich in dezelfde regio als uw Azure Machine Learning-werkruimte.
* **subnet_name**: naam van subnet binnen het virtuele netwerk. Azure Machine Learning-Computing-resources worden toegewezen IP-adressen uit dit subnetbereik.

> [!TIP]
> Wanneer u een permanente resource van Azure Machine Learning-Computing maakt hebt u ook de mogelijkheid om de eigenschappen, zoals de min_nodes of de max_nodes te werken. Roep de `update()` functie voor het.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           vm_priority='lowpriority',
                                                           min_nodes=2,
                                                           max_nodes=4,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```


## <a id="vm"></a>Externe virtuele machine

Azure Machine Learning ondersteunt ook uw eigen compute-resource halen en deze te koppelen aan uw werkruimte. Een dergelijke resourcetype is een willekeurige externe virtuele machine als deze toegankelijk vanuit Azure Machine Learning-service is. Het kan een Azure-VM of een externe server in uw organisatie of on-premises zijn. Specifiek, gezien het IP-adres en referenties (gebruikersnaam en wachtwoord of SSH-sleutel), kunt u elke VM die toegankelijk is voor extern worden uitgevoerd.
U kunt een systeem gebouwd conda-omgeving, een al bestaande Python-omgeving of een Docker-container. Kan worden uitgevoerd met behulp van Docker-container vereist dat u Docker-Engine die wordt uitgevoerd op de virtuele machine. Deze functionaliteit is vooral nuttig als u wilt dat een meer flexibele, cloud-gebaseerde dev/experimentele omgeving dan uw lokale computer.

> [!TIP]
> Het is raadzaam om met behulp van de Data Science Virtual Machine als de Azure-VM van keuze voor dit scenario. Het is een vooraf geconfigureerde datatechnologie en AI ontwikkelomgeving in Azure met een gecureerde keuze van hulpprogramma's en frameworks voor de volledige levenscyclus van ML-ontwikkeling. Zie voor meer informatie over het gebruik van de Data Science Virtual Machine met Azure Machine Learning, de [een ontwikkelomgeving configureren](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm) document.

> [!WARNING]
> Azure Machine Learning biedt alleen ondersteuning voor virtuele machines met Ubuntu. Wanneer u een virtuele machine of Selecteer een bestaande, moet u een selecteren die gebruikmaakt van Ubuntu.

De volgende stappen uit de SDK gebruiken om een Data Science Virtual Machine (DSVM) configureren als een doel voor training:

1. Als u wilt koppelen van een bestaande virtuele machine als een compute-doel, moet u de volledig gekwalificeerde domeinnaam, de aanmeldingsnaam en het wachtwoord opgeven voor de virtuele machine.  Vervang in het voorbeeld ```<fqdn>``` met openbare volledig gekwalificeerde domeinnaam van de virtuele machine of het openbare IP-adres. Vervang ```<username>``` en ```<password>``` met de SSH-gebruiker en het wachtwoord voor de virtuele machine:

    ```python
    from azureml.core.compute import RemoteCompute

    dsvm_compute = RemoteCompute.attach(ws,
                                    name="attach-dsvm",
                                    username='<username>',
                                    address="<fqdn>",
                                    ssh_port=22,
                                    password="<password>")

    dsvm_compute.wait_for_completion(show_output=True)

1. Create a configuration for the DSVM compute target. Docker and conda are used to create and configure the training environment on DSVM:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image
    # If you want to use GPU in DSVM, you must also use GPU base Docker image azureml.core.runconfig.DEFAULT_GPU_IMAGE
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks is een omgeving op basis van Apache Spark in de Azure-cloud. Het kan worden gebruikt als een compute-doel bij het trainen van met een Azure Machine Learning-pijplijn modellen.

> [!IMPORTANT]
> Een Azure Databricks compute-doel kan alleen worden gebruikt in een Machine Learning-pijplijn.
>
> U moet een Azure Databricks-werkruimte maken voordat u deze gebruikt met het trainen van uw model. Zie voor het maken van deze resource, de [een Spark-taak uitvoeren op Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) document.

Als u wilt koppelen Azure Databricks als een compute-doel, moet u de Azure Machine Learning-SDK gebruiken en geef de volgende informatie:

* __De naam van COMPUTE__: de naam die u wilt toewijzen aan deze compute-resource.
* __Resource-ID__: de resource-ID van de Azure Databricks-werkruimte. De volgende tekst is een voorbeeld van de indeling voor deze waarde:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.Databricks/workspaces/<databricks-workspace-name>
    ```

    > [!TIP]
    > Als u de resource-ID, gebruikt u de volgende Azure CLI-opdracht. Vervang `<databricks-ws>` met de naam van uw Databricks-werkruimte:
    > ```azurecli-interactive
    > az resource list --name <databricks-ws> --query [].id
    > ```

* __Toegangstoken__: het toegangstoken dat wordt gebruikt om te verifiëren met Azure Databricks. Zie voor het genereren van een toegangstoken de [verificatie](https://docs.azuredatabricks.net/api/latest/authentication.html) document.

De volgende code ziet u hoe u Azure Databricks als een compute-doel toevoegen:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_resource_id = os.environ.get("AML_DATABRICKS_RESOURCE_ID", "<databricks_resource_id>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_resource_id {}'.format(databricks_resource_id))
    print('databricks_access_token {}'.format(databricks_access_token))
    databricks_compute = DatabricksCompute.attach(
             workspace=ws,
             name=databricks_compute_name,
             resource_id=databricks_resource_id,
             access_token=databricks_access_token
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics is een platform voor big data-analyses in de Azure-cloud. Het kan worden gebruikt als een compute-doel bij het trainen van met een Azure Machine Learning-pijplijn modellen.

> [!IMPORTANT]
> Een Azure Data Lake Analytics compute-doel kan alleen worden gebruikt in een Machine Learning-pijplijn.
>
> U moet een Azure Data Lake Analytics-account maken voordat u deze gebruikt met het trainen van uw model. Zie voor het maken van deze resource, de [aan de slag met Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) document.

Als u wilt koppelen Data Lake Analytics als een compute-doel, moet u de Azure Machine Learning-SDK gebruiken en geef de volgende informatie:

* __De naam van COMPUTE__: de naam die u wilt toewijzen aan deze compute-resource.
* __Resource-ID__: de resource-ID van het Data Lake Analytics-account. De volgende tekst is een voorbeeld van de indeling voor deze waarde:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.DataLakeAnalytics/accounts/<datalakeanalytics-name>
    ```

    > [!TIP]
    > Als u de resource-ID, gebruikt u de volgende Azure CLI-opdracht. Vervang `<datalakeanalytics>` met de naam van de naam van uw Data Lake Analytics-account:
    > ```azurecli-interactive
    > az resource list --name <datalakeanalytics> --query [].id
    > ```

De volgende code ziet u hoe u Data Lake Analytics als een compute-doel toevoegen:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_id = os.environ.get("AML_ADLA_RESOURCE_ID", "<adla_resource_id>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_id))
    adla_compute = AdlaCompute.attach(
             workspace=ws,
             name=adla_compute_name,
             resource_id=adla_resource_id
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning-pijplijnen kunnen uitsluitend worden gebruikt met gegevens die zijn opgeslagen in het standaardarchief van gegevens van het Data Lake Analytics-account. Als de gegevens die u nodig hebt om te werken met in een niet-standaard-archief is, kunt u een [ `DataTransferStep` ](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) om de gegevens voordat een training te kopiëren.

## <a id="hdinsight"></a>Azure HDInsight 

HDInsight is een populair platform voor big data-analyses. Het biedt Apache Spark, die kunnen worden gebruikt om uw model te trainen.

> [!IMPORTANT]
> Voordat u deze gebruikt met het trainen van uw model, moet u het HDInsight-cluster maken. Zie voor informatie over het maken van een Spark op HDInsight-cluster de [maken van een Spark-Cluster in HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql) document.
>
> Wanneer het cluster te maken, moet u een SSH-gebruikersnaam en wachtwoord opgeven. Noteer deze waarden, zoals u ze nodig hebt bij het gebruik van HDInsight als een compute-doel.
>
> Zodra het cluster is gemaakt, heeft een volledig gekwalificeerde domeinnaam (FQDN) van `<clustername>.azurehdinsight.net`, waarbij `<clustername>` is de naam die u hebt opgegeven voor het cluster. U moet dit adres (of het openbare IP-adres van het cluster) als een compute-doel te gebruiken

Als u wilt configureren HDInsight als een compute-doel, moet u de volledig gekwalificeerde domeinnaam, cluster-aanmeldingsnaam en wachtwoord opgeven voor het HDInsight-cluster. Het volgende voorbeeld wordt de SDK te koppelen van een cluster aan uw werkruimte. Vervang in het voorbeeld `<fqdn>` met de openbare volledig gekwalificeerde domeinnaam van het cluster of het openbare IP-adres. Vervang `<username>` en `<password>` met de SSH-gebruiker en het wachtwoord voor het cluster:

> [!NOTE]
> De FQDN-naam voor uw cluster vinden, gaat u naar de Azure portal en selecteer uw HDInsight-cluster. Uit de __overzicht__ sectie, de FQDN-naam maakt deel uit van de __URL__ vermelding. Verwijdert de `https://` vanaf het begin van de waarde.
>
> ![Schermopname van het overzicht van HDInsight-cluster met de URL-vermelding is gemarkeerd](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute

try:
    # Attaches a HDInsight cluster as a compute target.
    HDInsightCompute.attach(ws,name = "myhdi",
                            address = "<fqdn>",
                            username = "<username>",
                            password = "<password>")
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Indienen van training uitvoeren

Er zijn twee manieren om in te dienen een training uitvoeren:

* Indienen van een `ScriptRunConfig` object.
* Indienen van een `Pipeline` object.

> [!IMPORTANT]
> De Azure Databricks en Azure Lake Analytics compute-doelen kunnen alleen worden gebruikt in een pijplijn.
> De lokale compute-doel kan niet worden gebruikt in een pijplijn.

### <a name="submit-using-scriptrunconfig"></a>Indienen met behulp van `ScriptRunConfig`

Het patroon van de code voor het indienen van een training wordt uitgevoerd met behulp van `ScriptRunConfig` is hetzelfde, ongeacht de compute-doel:

* Maak een `ScriptRunConfig` object met behulp van de configuratie uitvoeren voor de compute-doel.
* De uitvoering verzenden.
* Wacht totdat de uitvoering om te voltooien.

Het volgende voorbeeld wordt de configuratie voor het systeem beheerd lokale compute-doel eerder in dit document hebt gemaakt:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="submit-using-a-pipeline"></a>Indienen met behulp van een pijplijn

De code voor het indienen van een training wordt uitgevoerd met behulp van een pijplijn hetzelfde, ongeacht de compute-doel is patroon:

* Een stap toevoegen aan de pijplijn voor de compute-resource.
* Dien een uitvoeren met behulp van de pijplijn.
* Wacht totdat de uitvoering om te voltooien.

Het volgende voorbeeld wordt de Azure Databricks-compute-doel eerder in dit document hebt gemaakt:

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# list of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Zie voor meer informatie over machine learning-pijplijnen, de [pijplijnen en Azure Machine Learning](concept-ml-pipelines.md) document.

Bijvoorbeeld Jupyter-Notebooks die laten zien training over het gebruik van een pijplijn, Zie [ https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline ](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Weergeven en compute met behulp van de Azure-portal instellen

U kunt bekijken wat compute-doelen zijn gekoppeld aan uw werkruimte vanuit Azure portal. Als u aan de lijst, gebruikt u de volgende stappen uit:

1. Ga naar de [Azure-portal](https://portal.azure.com) en navigeer naar uw werkruimte.
2. Klik op de __Compute__ koppeling onder de __toepassingen__ sectie.

    ![Compute-tabblad weergave](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Een compute-doel maken

Volg de bovenstaande stappen voor het weergeven van de lijst met compute-doelen en vervolgens de volgende stappen gebruiken om een compute-doel:

1. Klik op de __+__ zich bij het toevoegen van een compute-doel.

    ![Compute toevoegen ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Voer een naam voor de compute-doel
1. Selecteer **Machine Learning-Computing** als het type compute moet worden gebruikt voor __Training__

    > [!IMPORTANT]
    > U kunt alleen maken Azure Machine Learning-Computing als de beheerde compute voor training

1. Vul de vereiste formuliergegevens met name de VM-reeks en het maximumaantal knooppunten moet worden gebruikt voor het implementeren van de compute 
1. Selecteer __Maken__
1. U kunt de status van de bewerking voor maken door het selecteren van de compute-doel in de lijst weergeven

    ![Compute-lijst weergeven](./media/how-to-set-up-training-targets/View_list.png)

1. Vervolgens ziet u de details voor de compute-doel.

    ![Details weergeven](./media/how-to-set-up-training-targets/compute-target-details.png)

1. Nu kunt u een uitvoering op basis van deze doelen als gedetailleerde bovenstaande indienen


### <a name="reuse-existing-compute-in-your-workspace"></a>Hergebruik bestaande compute in uw werkruimte

Volg de bovenstaande stappen voor het weergeven van de lijst met compute-doelen en vervolgens opnieuw gebruiken van compute-doel met de volgende stappen uit:

1. Klik op de **+** zich bij een compute-doel toevoegen
2. Voer een naam voor de compute-doel
3. Selecteer het type compute koppelen voor __Training__

    > [!IMPORTANT]
    > Niet alle compute-typen kunnen worden gekoppeld met behulp van de portal.
    > Momenteel zijn de typen die kunnen worden gekoppeld voor training:
    > 
    > * Externe virtuele machine
    > * Databricks
    > * Data Lake Analytics
    > * HDInsight

1. Vul het formulier vereist

    > [!NOTE]
    > Microsoft raadt aan dat u een SSH-sleutels gebruiken zoals ze veiliger dan wachtwoorden zijn. Wachtwoorden zijn kwetsbaar voor beveiligingsaanvallen, terwijl de SSH-sleutels zijn afhankelijk van de cryptografische handtekeningen. Zie de volgende documenten voor informatie over het maken van SSH-sleutels voor gebruik met Azure Virtual Machines:
    >
    > * [Maken en gebruiken van SSH-sleutels in Linux of macOS]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Maken en gebruiken van SSH-sleutels op Windows]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Selecteer koppelen
1. U kunt de status van de koppelingsbewerking door het selecteren van de compute-doel in de lijst weergeven
1. Nu kunt u een uitvoering op basis van deze doelen als gedetailleerde bovenstaande indienen

## <a name="examples"></a>Voorbeelden
Raadpleeg notebooks onder in de volgende locaties:
* [procedure-naar-gebruik-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)

* [zelfstudies/img-classificatie-deel 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

* [Naslaginformatie over Azure Machine Learning-SDK](https://aka.ms/aml-sdk)
* [Zelfstudie: een model trainen](tutorial-train-models-with-aml.md)
* [Waar u modellen implementeren](how-to-deploy-and-where.md)
* [Machine learning-pijplijnen met Azure Machine Learning-service bouwen](concept-ml-pipelines.md)
