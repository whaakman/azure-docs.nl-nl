---
title: Azure Machine Learning experimenten-Service-configuratiebestanden
description: In dit document worden de configuratie-instellingen voor Azure ML experimenten-Service.
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: 43bee297b917143c9014b28049c6dfa28727b757
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646417"
---
# <a name="azure-machine-learning-experimentation-service-configuration-files"></a>Azure Machine Learning experimenten-Service-configuratiebestanden

Wanneer u een script uitgevoerd in Azure Machine Learning (Azure ML) Workbench, het gedrag van de uitvoering wordt bepaald door de bestanden in de **aml_config** map. Deze map is onder de hoofdmap van uw project. Het is belangrijk dat u begrijpt de inhoud van deze bestanden om te realiseren van het gewenste resultaat voor de uitvoering op optimale wijze.

Hieronder vindt u de relevante bestanden in deze map:
- conda_dependencies.yml
- spark_dependencies.yml
- COMPUTE doelbestanden
    - \<COMPUTE doelnaam > .compute
- configuratiebestanden uitvoeren
    - \<naam van de configuratie uitvoeren > .runconfig

>[!NOTE]
>U doorgaans een compute-doel-bestand hebt en u voert configuratiebestand voor elke compute-doel dat u hebt gemaakt. U kunt echter deze bestanden onafhankelijk te maken en meerdere uitvoeren configuratiebestanden die verwijst naar de dezelfde compute-doel.

## <a name="condadependenciesyml"></a>conda_dependencies.yml
Dit bestand is een [conda omgevingsbestand](https://conda.io/docs/using/envs.html#create-environment-file-by-hand) die Hiermee geeft u de Python-runtime-versie en pakketten die afhankelijk van uw code. Wanneer u een script wordt uitgevoerd in een Docker-container of een HDInsight-cluster van Azure ML Workbench, maakt het een [conda-omgeving](https://conda.io/docs/using/envs.html) voor het script uit te voeren op. 

In dit bestand geeft u de Python-pakketten die het script voor uitvoering moet. Azure ML experimenten-Service maakt het conda-omgeving op basis van de lijst met afhankelijkheden. Pakketten die hier worden vermeld moeten kunnen worden bereikt door de engine voor het uitvoeren via de kanalen, zoals:

* [continuum.io](https://anaconda.org/conda-forge/repo)
* [PyPI](https://pypi.python.org/pypi)
* een eindpunt dat openbaar toegankelijk is (URL)
* of een lokaal bestandspad
* andere bereikbare door de engine voor het uitvoeren

>[!NOTE]
>Bij het uitvoeren van op HDInsight-cluster, maakt Azure ML Workbench een conda-omgeving voor uw specifieke uitvoering. Hierdoor kunnen andere gebruikers om uit te voeren op verschillende python-omgevingen in hetzelfde cluster.  

Hier volgt een voorbeeld van een typische **conda_dependencies.yml** bestand.
```yaml
name: project_environment
dependencies:
  # Python version
  - python=3.5.2
  
  # some conda packages
  - scikit-learn
  - cryptography
  
  # use pip to install some more packages
  - pip:
     # a package in PyPi
     - azure-storage
     
     # a package hosted in a public URL endpoint
     - https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
     
     # a wheel file available locally on disk (this only works if you are executing against local Docker target)
     - C:\temp\my_private_python_pkg.whl
```

Azure ML Workbench maakt gebruik van dezelfde conda-omgeving zonder herbouwen zo lang de **conda_dependencies.yml** blijft hetzelfde. Als de afhankelijkheden wijzigt, wordt deze uw omgeving opnieuw opgebouwd.

>[!NOTE]
>Als u zich richten op worden uitgevoerd op basis van _lokale_ compute-context, **conda_dependencies.yml** bestand **niet** gebruikt. Pakketafhankelijkheden voor uw lokale Azure ML Workbench Python-omgeving moeten handmatig worden geïnstalleerd.

## <a name="sparkdependenciesyml"></a>spark_dependencies.yml
Dit bestand bevat de naam van de Spark-toepassing wanneer u een PySpark-script- en Spark-pakketten die moeten worden geïnstalleerd verzendt. U kunt ook een openbare opslagplaats met Maven, evenals de Spark-pakketten die kunnen worden gevonden in deze opslagplaatsen Maven opgeven.

Hier volgt een voorbeeld:

```yaml
configuration:
  # Spark application name
  "spark.app.name": "ClassifyingIris"
  
repositories:
  # Maven repository hosted in Azure CDN
  - "https://mmlspark.azureedge.net/maven"
  
  # Maven repository hosted in spark-packages.org
  - "https://spark-packages.org/packages"
  
packages:
  # MMLSpark package hosted in the Azure CDN Maven
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.5"
    
  # spark-sklearn packaged hosted in the spark-packages.org Maven
  - group: "databricks"
    artifact: "spark-sklearn"
    version: "0.2.0"
```

>[!NOTE]
>Cluster-parameters zoals werkrolgrootte en kernen afstemmen moet worden geplaatst in de sectie "configuratie" in het bestand spark_dependecies.yml 

>[!NOTE]
>Als u het script in Python-omgeving uitvoert *spark_dependencies.yml* bestand wordt genegeerd. Dit wordt alleen gebruikt als u op basis van Spark (hetzij op Docker of het HDInsight-Cluster uitvoeren).

## <a name="run-configuration"></a>Configuratie van de uitvoering
Als u een specifieke configuratie uitvoeren, moet u een bestand .compute en een .runconfig-bestand. Deze worden doorgaans gegenereerd met behulp van een CLI-opdracht. U kunt ook klonen zijn afgesloten, de naam wijzigen en ze bewerken.

```azurecli
# create a compute target pointing to a VM via SSH
$ az ml computetarget attach remotedocker -n <compute target name> -a <IP address or FQDN of VM> -u <username> -w <password>

# create a compute context pointing to an HDI cluster head-node via SSH
$ az ml computetarget attach cluster -n <compute target name> -a <IP address or FQDN of HDI cluster> -u <username> -w <password> 
```

Deze opdracht maakt u een tweetal bestanden op basis van de compute-doel opgegeven. Stel dat u de naam van uw compute-doel _foo_. Met deze opdracht genereert _foo.compute_ en _foo.runconfig_ in uw **aml_config** map.

>[!NOTE]
> _lokale_ of _docker_ voor de run-configuratiebestanden willekeurige zijn namen. Azure ML Workbench wordt toegevoegd dat deze twee configuraties worden uitgevoerd wanneer u een leeg project voor uw gemak maken. U kunt de naam '<run configuration name>.runconfig "bestanden die worden geleverd met de projectsjabloon, maken of een nieuwe maken met elke gewenste naam.

### <a name="compute-target-namecompute"></a>\<COMPUTE doelnaam > .compute
_\<COMPUTE doelnaam > .compute_ bestand-verbinding en configuratie-informatie voor de compute-doel wordt aangegeven. Het is een lijst met naam / waarde-paren. Hier volgen de ondersteunde instellingen:

**type**: Type van de compute-omgeving. Ondersteunde waarden zijn:
  - lokaal
  - externe
  - Docker
  - remotedocker
  - cluster

**baseDockerImage**: de Docker-installatiekopie die wordt gebruikt de Python/PySpark-script uit te voeren. De standaardwaarde is _microsoft/mmlspark:plus-0.7.91_. We ondersteunen ook een andere afbeelding: _microsoft/mmlspark:plus-gpu-0.7.91_, waardoor u GPU toegang hebben tot de hostcomputer (als GPU aanwezig is).

**adres**: het IP-adres of FQDN-naam (volledig gekwalificeerde domeinnaam) van de virtuele machine, of de HDInsight-cluster hoofdknooppunt.

**gebruikersnaam**: de SSH-gebruikersnaam voor toegang tot de virtuele machine of de HDInsight-hoofdknooppunt.

**wachtwoord**: het versleutelde wachtwoord voor de SSH-verbinding.

**sharedVolumes**: markering om aan te geven die kan worden uitgevoerd-engine moet Docker gebruiken gedeeld volume functie om projectbestanden heen en terug af te leveren. Met deze vlag ingeschakeld sneller kan worden uitgevoerd omdat Docker hebben toegang tot projecten rechtstreeks zonder de noodzaak om te kopiëren. Het is raadzaam om in te stellen _false_ als de Docker-engine wordt uitgevoerd op Windows omdat het volume delen voor Docker op Windows flaky kan zijn. Stel deze in op _waar_ als deze op Mac OS of Linux wordt uitgevoerd.

**nvidiaDocker**: deze optie als de waarde _waar_, geeft u de Azure ML experimenten-Service te gebruiken _nvidia docker-_ opdracht, in plaats van de normale _docker_opdracht, voor het starten van de Docker-installatiekopie. De _nvidia docker_ engine staat de Docker-container voor toegang tot GPU hardware. De instelling is vereist als u wilt uitvoeren van GPU-uitvoering in de Docker-container. Biedt ondersteuning voor Linux-host _nvidia docker_. Bijvoorbeeld, DSVM op basis van Linux in Azure wordt geleverd met _nvidia docker_. _docker-NVIDIA_ vanaf nu wordt niet ondersteund op Windows.

**nativeSharedDirectory**: deze eigenschap geeft u de basismap (bijvoorbeeld: _~/.azureml/share/_) waarin bestanden kunnen worden opgeslagen om te worden gedeeld met wordt uitgevoerd op de dezelfde compute-doel. Als deze instelling wordt gebruikt wanneer u een Docker-container _sharedVolumes_ moet worden ingesteld op true. Anders mislukt de uitvoering.

**userManagedEnvironment**: deze eigenschap geeft aan of deze compute-doel rechtstreeks worden beheerd door de gebruiker of beheerd via experimenten-service.  

**pythonLocation**: deze eigenschap geeft u de locatie van de python-runtime moet worden gebruikt voor de compute-doel van de gebruiker programma uit te voeren. 

### <a name="run-configuration-namerunconfig"></a>\<naam van de configuratie uitvoeren > .runconfig
_\<naam van de configuratie uitvoeren > .runconfig_ Hiermee geeft u de Azure ML uitvoeringsgedrag experimenteren. U kunt uitvoeringsgedrag zoals bijhouden uitvoeringsgeschiedenis configureren of wat compute-doel te gebruiken, samen met vele andere. De namen van de run-configuratiebestanden worden gebruikt voor het vullen van de vervolgkeuzelijst van de context kan worden uitgevoerd in de Azure ML Workbench-bureaubladtoepassing.

**ArgumentVector**: in deze sectie wordt het script moet worden uitgevoerd als onderdeel van deze uitvoering en de parameters voor het script. Bijvoorbeeld, als u het volgende codefragment in uw "<run configuration name>.runconfig" bestand 

```
 "ArgumentVector":[
  - "myscript.py"
  - 234
  - "-v" 
 ] 
```
_"az ml experiment verzenden foo.runconfig"_ automatisch wordt uitgevoerd de opdracht met _myscript.py_ bestand doorgeven in een 234 als een parameter en stelt de--uitgebreide vlag.

**Doel**: deze parameter is de naam van de _.compute_ bestand dat de _runconfig_ verwijst naar het bestand. In het algemeen verwijst de _foo.compute_ bestand, maar u kunt bewerken om te verwijzen naar een andere compute-doel.

**Omgevingsvariabelen**: in deze sectie kan gebruikers omgevingsvariabelen worden ingesteld als onderdeel van hun wordt uitgevoerd. Gebruiker kan met behulp van de naam / waarde-paren in de volgende indeling omgevingsvariabelen opgeven:
```
EnvironmentVariables:
  "EXAMPLE_ENV_VAR1": "Example Value1"
  "EXAMPLE_ENV_VAR2": "Example Value2"
```

Deze omgevingsvariabelen kunnen worden geopend in de code van de gebruiker. Bijvoorbeeld: de omgevingsvariabele met de naam 'EXAMPLE_ENV_VAR' in deze Python-code worden afgedrukt
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

**Framework**: deze eigenschap geeft u als Azure ML Workbench een Spark-sessie om uit te voeren van het script moet starten. De standaardwaarde is _PySpark_. Stel deze in op _Python_ als u niet PySpark-code, die u kan helpen bij het starten van de taak met minder overhead sneller worden uitgevoerd.

**CondaDependenciesFile**: deze eigenschap verwijst naar het bestand dat Hiermee geeft u de omgeving conda-afhankelijkheden in de *aml_config* map. Indien ingesteld op _null_, deze verwijst naar het standaardpictogram **conda_dependencies.yml** bestand.

**SparkDependenciesFile**: deze eigenschap verwijst naar het bestand waarin de Spark-afhankelijkheden in de **aml_config** map. Deze is ingesteld op _null_ standaard en deze verwijst naar het standaardpictogram **spark_dependencies.yml** bestand.

**PrepareEnvironment**: deze eigenschap, als de waarde _waar_, geeft de experimenten-Service naar het conda-omgeving op basis van het conda-afhankelijkheden die is opgegeven als onderdeel van uw eerste uitvoering voorbereiden. Deze eigenschap is alleen effectief als u op basis van een Docker-omgeving uitvoeren. Deze instelling heeft geen effect als u uitvoeren op basis van een _lokale_ omgeving. 

**TrackedRun**: met deze markering geeft de experimenten-Service al dan niet om bij te houden van de uitvoering in Azure ML Workbench geschiedenis infrastructuur worden uitgevoerd. De standaardwaarde is _waar_. 

**UseSampling**: _UseSampling_ geeft aan of de actieve voorbeeldgegevenssets voor gegevensbronnen die worden gebruikt voor de uitvoering. Indien ingesteld op _false_, gegevensbronnen opnemen en gebruik de volledige gegevens lezen uit het gegevensarchief. Indien ingesteld op _waar_, actieve voorbeelden worden gebruikt. Gebruikers kunnen gebruiken de **DataSourceSettings** om op te geven welke specifieke voorbeelden van gegevenssets te gebruiken als ze willen de actieve voorbeeld overschrijven. 

**DataSourceSettings**: deze sectie van de configuratie van de instellingen voor gegevensbron bevat. In deze sectie geeft gebruiker aan welke bestaande voorbeeld van de gegevens voor een bepaalde gegevensbron wordt gebruikt als onderdeel van de uitvoering. 

De volgende configuratie-instelling geeft aan dat voorbeeld met de naam 'MySample' wordt gebruikt voor de gegevensbron met de naam "MyDataSource"
```
DataSourceSettings:
    MyDataSource.dsource:
    Sampling:
    Sample: MySample
```

**DataSourceSubstitutions**: Data source vervangingen kunnen worden gebruikt wanneer de gebruiker wil overschakelen van de ene gegevensbron naar een andere zonder de code te wijzigen. Bijvoorbeeld: gebruikers van een steekproef omlaag, lokale bestand kunnen overschakelen naar de oorspronkelijke, grotere gegevensset die door het wijzigen van de verwijzing naar gegevensbron opgeslagen in Azure Blob. Wanneer een vervanging wordt gebruikt, wordt uw gegevensbronnen en de voorbereiding van gegevenspakketten van Azure ML Workbench uitgevoerd door te verwijzen naar de gegevensbron vervangen.

Het volgende voorbeeld worden de verwijzingen 'mylocal.datasource' in Azure ML-gegevensbronnen en voorbereiding gegevenspakketten vervangen door 'myremote.dsource'. 
 
```
DataSourceSubstitutions:
    mylocal.dsource: myremote.dsource
```

Op basis van de bovenstaande vervanging, leest het volgende codevoorbeeld nu van 'myremote.dsource' in plaats van 'mylocal.dsource' zonder dat gebruikers hun code wijzigen.
```
df = datasource.load_datasource('mylocal.dsource')
```
## <a name="next-steps"></a>Volgende stappen
Meer informatie over [experimenten-Service configuration](experimentation-service-configuration.md).
