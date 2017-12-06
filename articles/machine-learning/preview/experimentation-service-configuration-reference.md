---
title: Azure Machine Learning-experimenten Service configuratiebestanden
description: In dit document worden de configuratie-instellingen voor Azure ML experimenteren Service.
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: 16c72f8c22307a124fdb670aabca771084c0d1ec
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="azure-machine-learning-experimentation-service-configuration-files"></a>Azure Machine Learning-experimenten Service configuratiebestanden

Wanneer u een script in Azure Machine Learning (Azure ML) Workbench uitvoert, het gedrag van de uitvoering wordt bepaald door de bestanden in de **aml_config** map. Deze map bevindt zich onder de hoofdmap van uw project map. Het is belangrijk dat u begrijpt de inhoud van deze bestanden om het gewenste resultaat voor de uitvoering optimaal te bereiken.

Hieronder vindt u de relevante bestanden onder deze map:
- conda_dependencies.yml
- spark_dependencies.yml
- rekenuren voor doelbestanden
    - \<COMPUTE doelnaam > .compute
- configuratiebestanden uitvoeren
    - \<configuratienaam uitvoeren > .runconfig

>[!NOTE]
>U doorgaans een compute-doelbestand hebben en uitvoeren van configuratiebestand voor elke compute doel dat u maakt. U kunt echter afzonderlijk maken van deze bestanden en meerdere uitvoeren configuratiebestanden die verwijzen naar hetzelfde compute doel hebben.

## <a name="condadependenciesyml"></a>conda_dependencies.yml
Dit bestand is een [conda omgeving bestand](https://conda.io/docs/using/envs.html#create-environment-file-by-hand) die aangeeft dat de runtime-versie van Python en pakketten die afhankelijk zijn van uw code. Wanneer u een script wordt uitgevoerd in een Docker-container of de HDInsight-cluster van Azure ML-Workbench, maakt deze een [conda omgeving](https://conda.io/docs/using/envs.html) voor uw script uit te voeren. 

In dit bestand geeft u Python-pakketten die uw script nodig voor uitvoering heeft. Azure ML experimenteren Service maakt de conda-omgeving in de installatiekopie van het Docker volgens uw lijst met afhankelijkheden. De lijst met pakketten hier moet bereikbaar zijn door de engine voor het uitvoeren. Om die reden moeten pakketten worden weergegeven in de kanalen, zoals:

* [continuum.IO](https://anaconda.org/conda-forge/repo)
* [PyPI](https://pypi.python.org/pypi)
* een openbaar toegankelijk eindpunt (URL)
* of een lokaal pad
* anderen kunnen worden bereikt door de engine voor het uitvoeren

>[!NOTE]
>Wanneer op HDInsight-cluster wordt uitgevoerd, maakt Azure ML-Workbench een conda omgeving voor uw uitvoeren. Hierdoor kunnen andere gebruikers worden uitgevoerd op verschillende python-omgevingen in hetzelfde cluster.  

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
     
     # a wheel file available locally on disk (this only works if you are executing against local target)
     - C:\temp\my_private_python_pkg.whl
```

Azure ML-Workbench maakt gebruik van dezelfde conda omgeving zonder opnieuw opbouwen, zolang de **conda_dependencies.yml** blijft intact. Als er iets verandert in dit bestand, resulteert het echter in de opbouw van de Docker-installatiekopie.

>[!NOTE]
>Als u richt de uitvoering tegen _lokale_ context, compute **conda_dependencies.yml** bestand is **niet** gebruikt. Pakketafhankelijkheden voor uw lokale Azure ML-Workbench Python-omgeving moeten handmatig worden geïnstalleerd.

## <a name="sparkdependenciesyml"></a>spark_dependencies.yml
Dit bestand bevat de naam van de toepassing Spark wanneer u een PySpark-script en Spark-pakketten die moet worden geïnstalleerd. U kunt ook een openbare opslagplaats met Maven, evenals een Spark-pakket dat kan worden gevonden in deze Maven-opslagplaatsen opgeven.

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
>Cluster afstemmen parameters, zoals de grootte van de werknemer, kernen moeten gaan naar de sectie "configuratie" in het bestand spark_dependecies.yml 

>[!NOTE]
>Als u bij het uitvoeren van script in Python-omgeving, *spark_dependencies.yml* bestand genegeerd. Dit heeft alleen effect als u werkt met tegen Spark (zowel op Docker of HDInsight-Cluster).

## <a name="run-configuration"></a>Configuratie uitvoeren
Geef een specifieke configuratie uitvoeren door is een combinatie van bestanden vereist. Ze worden doorgaans gegenereerd met een opdracht CLI. Maar u kunt ook klonen zijn afgesloten, wijzigen en deze bewerken.

```azurecli
# create a compute target pointing to a VM via SSH
$ az ml computetarget attach remotedocker -n <compute target name> -a <IP address or FQDN of VM> -u <username> -w <password>

# create a compute context pointing to an HDI cluster head-node via SSH
$ az ml computetarget attach cluster -n <compute target name> -a <IP address or FQDN of HDI cluster> -u <username> -w <password> 
```

Deze opdracht maakt u een combinatie van bestanden op basis van het opgegeven doel voor de berekening. Stel dat u het doel-berekeningen met de naam _foo_. Met deze opdracht genereert _foo.compute_ en _foo.runconfig_ in uw **aml_config** map.

>[!NOTE]
> _lokale_ of _docker_ namen voor de uitvoering configuratiebestanden willekeurige zijn. Azure ML-Workbench voegt dat deze twee configuraties worden uitgevoerd wanneer u een leeg project voor uw gemak maken. U kunt de naam '<run configuration name>.runconfig ' bestanden die worden geleverd met de projectsjabloon of nieuwe maken met de naam die u wilt.

### <a name="compute-target-namecompute"></a>\<COMPUTE doelnaam > .compute
_\<COMPUTE doelnaam > .compute_ bestand geeft verbinding en configuratie-informatie voor de compute-doel. Het is een lijst met naam-waardeparen. Dit zijn de ondersteunde instellingen.

**type**: Type van de compute-omgeving. Ondersteunde waarden zijn:
  - lokale
  - Docker
  - remotedocker
  - cluster

**baseDockerImage**: de Docker-installatiekopie gebruikt de Python/PySpark-script uit te voeren. De standaardwaarde is _microsoft/mmlspark:plus-0.7.91_. We bieden ook ondersteuning voor een andere afbeelding: _microsoft/mmlspark:plus-gpu-0.7.91_, die hebt u GPU toegang tot de hostcomputer (als GPU aanwezig).

**adres**: het IP-adres of FQDN-naam (fully qualified domain name) van de virtuele machine of de HDInsight-cluster hoofdknooppunt.

**gebruikersnaam**: de SSH-gebruikersnaam voor toegang tot de virtuele machine of de HDInsight-hoofdknooppunt.

**wachtwoord**: het versleutelde wachtwoord voor de SSH-verbinding.

**sharedVolumes**: markering om aan te geven die engine voor het uitvoeren moet gebruiken Docker gedeeld volume onderdeel projectbestanden heen en weer worden geleverd. Met deze vlag ingeschakeld kunt uitvoering versnellen omdat Docker toegang tot projecten rechtstreeks zonder de noodzaak om te kopiëren. Het is raadzaam om in te stellen _false_ als de Docker-engine wordt uitgevoerd op Windows omdat het volume voor Docker onder Windows kan flaky delen. Stel deze in op _true_ als deze op Mac OS- of Linux wordt uitgevoerd.

**nvidiaDocker**: deze optie als de waarde _waar_, ziet u de Azure ML experimenteren Service moet gebruiken _nvidia docker_ opdracht, in plaats van de normale _docker_ opdracht starten van de Docker-installatiekopie. De _nvidia docker_ engine staat de Docker-container op toegang GPU hardware. De instelling is vereist als u wilt uitvoeren van GPU in de Docker-container. Biedt ondersteuning voor Linux-host _nvidia docker_. Bijvoorbeeld: DSVM op basis van Linux in Azure wordt geleverd met _nvidia docker_. _NVIDIA docker_ vanaf nu wordt niet ondersteund in Windows.

**nativeSharedDirectory**: deze eigenschap geeft u het basisobject in Active directory (bijvoorbeeld: _~/.azureml/share/_) waar de bestanden kunnen worden opgeslagen om te worden gedeeld tussen wordt uitgevoerd op hetzelfde doel compute. Als deze instelling wordt gebruikt wanneer u gebruikmaakt van een Docker-container _sharedVolumes_ moet worden ingesteld op true. Anders mislukt de uitvoering.

### <a name="run-configuration-namerunconfig"></a>\<configuratienaam uitvoeren > .runconfig
_\<configuratienaam uitvoeren > .runconfig_ Hiermee geeft u de Azure ML uitvoeringsgedrag experimenteren. U kunt uitvoering gedrag zoals uitvoeringsgeschiedenis bijhouden configureren of wat het doel moet worden gebruikt samen met vele andere compute. De namen van de configuratiebestanden uitvoeren die worden gebruikt voor het vullen van de vervolgkeuzelijst van de context kan worden uitgevoerd in de Azure ML-Workbench bureaubladtoepassing.

**ArgumentVector**: in deze sectie geeft het script moet worden uitgevoerd als onderdeel van deze uitvoering en de parameters voor het script. Bijvoorbeeld, als u het volgende fragment in uw '<run configuration name>.runconfig ' bestand 

```
 "ArgumentVector":[
  - "myscript.py"
  - 234
  - "-v" 
 ] 
```
_'az ml experiment indienen foo.runconfig'_ automatisch wordt uitgevoerd de opdracht met _myscript.py_ bestand doorgeven in een 234 als een parameter en stelt de--uitgebreide vlag.

**Doel**: deze parameter is de naam van de _.compute_ bestand dat de _runconfig_ verwijst naar het bestand. Deze in het algemeen verwijst de _foo.compute_ bestand, maar u kunt bewerken om te verwijzen naar een andere compute-doel.

**Omgevingsvariabelen**: in deze sectie kan gebruikers omgevingsvariabelen worden ingesteld als onderdeel van hun wordt uitgevoerd. Gebruiker kan met de naam / waarde-paren in de volgende indeling omgevingsvariabelen opgeven:
```
EnvironmentVariables:
"EXAMPLE_ENV_VAR1": "Example Value1"
"EXAMPLE_ENV_VAR2": "Example Value2"
```

Deze omgevingsvariabelen kunnen worden geopend in de code van de gebruiker. Bijvoorbeeld, de omgevingsvariabele met de naam 'EXAMPLE_ENV_VAR' in deze code phyton worden afgedrukt
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

**Framework**: deze eigenschap geeft u als Azure ML-Workbench een Spark-sessie het script uitvoeren te openen. De standaardwaarde is _PySpark_. Stel deze in op _Python_ als u niet PySpark-code, die kan helpen bij het starten van de taak met minder overhead sneller worden uitgevoerd.

**CondaDependenciesFile**: deze eigenschap verwijst naar het bestand waarin de afhankelijkheden conda omgeving in de *aml_config* map. Indien ingesteld op _null_, deze verwijst naar het standaardpictogram **conda_dependencies.yml** bestand.

**SparkDependenciesFile**: deze eigenschap verwijst naar het bestand waarin de Spark-afhankelijkheden in de **aml_config** map. Deze waarde is ingesteld op _null_ standaard en deze verwijst naar het standaardpictogram **spark_dependencies.yml** bestand.

**PrepareEnvironment**: deze eigenschap, als de waarde _true_, ziet u de Service experimenteren de conda-omgeving op basis van de afhankelijkheden conda is opgegeven als onderdeel van uw eerste uitvoering voorbereiden. Deze eigenschap is alleen effectief als u tegen een Docker-omgeving uitvoeren. Deze instelling heeft geen effect als u worden uitgevoerd voor een _lokale_ omgeving. 

**TrackedRun**: met deze markering geeft de experimenteren-Service al dan niet voor het bijhouden van de groep in de Azure ML-Workbench geschiedenis infrastructuur worden uitgevoerd. De standaardwaarde is _true_. 

**UseSampling**: _UseSampling_ bepaalt of de actieve voorbeeldgegevenssets voor gegevensbronnen worden gebruikt voor de verwerking. Indien ingesteld op _false_, gegevensbronnen opnemen en gebruik van de volledige gegevens lezen uit de gegevensopslag. Indien ingesteld op _true_, actieve voorbeelden worden gebruikt. Gebruikers kunnen gebruiken de ** DataSourceSettings ' om op te geven welke specifieke voorbeeldgegevenssets te gebruiken als ze willen de actieve steekproef overschrijven. 

**DataSourceSettings**: deze configuratiesectie bevat instellingen voor de gegevensbron. In deze sectie gebruiker geeft op welke bestaande steekproef voor een bepaalde gegevensbron wordt gebruikt als onderdeel van de uitvoering. 

De volgende configuratie-instelling geeft aan dat voorbeeld met de naam 'MySample' wordt gebruikt voor de gegevensbron met de naam 'MyDataSource'
```
DataSourceSettings:
    MyDataSource.dsource:
    Sampling:
    Sample: MySample
```

**DataSourceSubstitutions**: Data source vervangingen kunnen worden gebruikt wanneer de gebruiker wil overschakelen van een gegevensbron naar een andere zonder de code te wijzigen. Gebruikers kunnen bijvoorbeeld uit een bestand met door actieve omlaag, lokale schakelen naar de oorspronkelijke, grotere gegevensset opgeslagen in Azure Blob door het wijzigen van de verwijzing naar de gegevensbron. Wanneer een vervanging wordt gebruikt, voert Azure ML-Workbench uw gegevensbronnen en gegevens voorbereiding pakketten door te verwijzen naar de gegevensbron substitute.

Het volgende voorbeeld worden de verwijzingen 'mylocal.datasource' in de Azure ML-gegevensbronnen en gegevens voorbereiding pakketten vervangen door 'myremote.dsource'. 
 
```
DataSourceSubstitutions:
    myocal.dsource: myremote.dsource
```

Op basis van de bovenstaande vervanging, leest het volgende codevoorbeeld nu uit 'myremote.dsource' in plaats van 'mylocal.dsource' zonder gebruikers hun code wijzigen.
```
df = datasource.load_datasource('mylocal.dsource')
```
## <a name="next-steps"></a>Volgende stappen
Meer informatie over [experimenteren serviceconfiguratie](experimentation-service-configuration.md).
