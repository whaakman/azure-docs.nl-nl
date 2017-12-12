---
title: Azure Machine Learning-experimenten Service configureren | Microsoft Docs
description: In dit artikel biedt een overzicht van Azure Machine Learning experimenteren Service met instructies over het configureren van deze.
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: bd152cc79c08124a1acab2aefc8652c7d162ea2c
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configuring-azure-machine-learning-experimentation-service"></a>Azure Machine Learning-experimenten Service configureren

## <a name="overview"></a>Overzicht
Azure Machine Learning experimenteren Service kunt gegevenswetenschappers uit te voeren hun experimenten met behulp van de uitvoering van de Azure Machine Learning en beheerfuncties uitvoeren. Dit biedt een raamwerk voor flexibele experimenteren met snel herhalingen. Azure Machine Learning Workbench kunt u beginnen met lokale wordt uitgevoerd op uw computer en biedt een eenvoudige manier voor het schalen van en buiten in andere omgevingen zoals externe gegevens wetenschappelijke VMs met een GPU of met Spark HDInsight-Clusters.

Experimenteren Service is gebouwd voor het ontwikkelen van uw experimenten is geïsoleerd, reproduceerbare en consistente uitgevoerd. Dit helpt u bij uw doelen voor compute, omgevingen kan worden uitgevoerd, beheren en uitvoeren van configuraties. Met behulp van de Azure Machine Learning Workbench uitvoering en beheerfuncties uitvoeren, kunt u gemakkelijk verplaatsen tussen verschillende omgevingen. 

U kunt een Python of PySpark-script uitvoeren in een project Workbench lokaal of op de gewenste schaal in de cloud. 

U kunt uw scripts uitvoeren op: 

* (3.5.2) Python-omgeving op de lokale computer is geïnstalleerd door de Workbench.
* Conda Python-omgeving binnen een Docker-container op lokale computer
* Conda Python-omgeving binnen een Docker-container op een externe Linux-machine. Bijvoorbeeld, een [Ubuntu gebaseerde DSVM op Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight voor Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) op Azure

>[!IMPORTANT]
>Azure Machine Learning experimenteren Service ondersteunt momenteel Python 3.5.2 en Spark 2.1.11 als runtime-versies, Python en Spark, respectievelijk. 


### <a name="key-concepts-in-experimentation-service"></a>Belangrijkste concepten in experimenteren-Service
Het is belangrijk om te begrijpen van de volgende concepten in Azure Machine Learning-experiment worden uitgevoerd. In de volgende secties bespreken we het gebruik van deze concepten in detail. 

#### <a name="compute-target"></a>Doel berekenen
Een _doel compute_ geeft aan waar uw programma zoals uw bureaublad, externe Docker uitvoeren op een virtuele machine of een cluster. Een compute-doel moet adresseerbaar en toegankelijk zijn voor u. Workbench biedt u de mogelijkheid compute-doelen maken en beheren met behulp van de toepassing Workbench en de CLI. 

_AZ ml computetarget koppelen_ opdracht in de CLI kunt u voor het maken van een compute-doel dat u in uw wordt uitgevoerd gebruiken kunt.

Ondersteunde compute doelen zijn:
* Lokale omgeving Python (3.5.2) op uw computer is geïnstalleerd door de Workbench.
* Lokale Docker op uw computer
* Externe Docker op Linux-Ubuntu virtuele machines. Bijvoorbeeld, een [Ubuntu gebaseerde DSVM op Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight Spark-cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/) op Azure

Experimenteren Service momenteel ondersteunt Python 3.5.2 en Spark 2.1.11 als Python en runtime-versies, respectievelijk Spark. 

>[!IMPORTANT]
> Windows virtuele machines actief zijn Docker **niet** ondersteund als externe compute-doelen.

#### <a name="execution-environment"></a>Uitvoeringsomgeving
De _uitvoeringsomgeving_ definieert de configuratie van de runtime en de afhankelijkheden die nodig zijn voor het programma uitvoert in de Workbench.

Beheren van de lokale uitvoeringsomgeving met uw favoriete tools en pakket managers als u op de standaard Workbench runtime uitvoert. 

Conda wordt gebruikt voor het beheren van Docker lokale en externe Docker-uitvoeringen, evenals op basis van een HDInsight-uitvoeringen. Voor deze doelen Reken-, configuratie van de omgeving voor uitvoering wordt beheerd via **Conda_dependencies.yml** en **Spark_dependencies.yml bestanden**. Deze bestanden zijn de **aml_config** map in uw project.

**Ondersteunde runtimes voor uitvoeringsomgevingen zijn:**
* Python 3.5.2
* Spark 2.1.11

### <a name="run-configuration"></a>Configuratie uitvoeren
Naast de compute-omgeving voor doel en kan worden uitgevoerd, biedt Azure Machine Learning een framework om te definiëren en te wijzigen *configuraties uitvoeren*. Verschillende uitvoeringen van uw experiment wellicht andere configuratie als onderdeel van terugkerende experimenten nodig. U verstrekkende verschillende parameter bereiken, om de volgende met behulp van verschillende gegevensbronnen en het afstemmen van spark-parameters. Experimenteren-Service biedt een framework voor het beheren van configuraties uitvoeren.

Met _az ml computetarget koppelen_ opdracht geeft u twee bestanden in uw **aml_config** map in uw project: een .compute en een .runconfig volgen van deze overeenkomst: _< your_ computetarget_name > .compute_ en _< your_computetarget_name > .runconfig_. Het bestand .runconfig wordt automatisch gemaakt voor uw gemak bij het maken van een compute-doel. U kunt maken en beheren van andere uitvoeren configuraties met _az ml runconfigurations_ opdracht in de CLI. U kunt ook maken en deze te bewerken op het bestandssysteem.

Voer de configuratie in Workbench kunt u omgevingsvariabelen opgeven. U kunt omgevingsvariabelen opgeven en deze gebruiken in uw code door de volgende sectie toe te voegen in het bestand .runconfig. 

```
EnvironmentVariables:
"EXAMPLE_ENV_VAR1": "Example Value1"
"EXAMPLE_ENV_VAR2": "Example Value2"
```

Deze omgevingsvariabelen kunnen worden geopend in uw code. Bijvoorbeeld, de omgevingsvariabele met de naam 'EXAMPLE_ENV_VAR1' in dit codefragment phyton worden afgedrukt
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

_**De volgende afbeelding ziet de stroom op hoog niveau voor de eerste experiment uitvoeren.**_
![](media/experimentation-service-configuration/experiment-execution-flow.png)

## <a name="experiment-execution-scenarios"></a>Scenario's voor experiment uitvoeren
In deze sectie we Duik in scenario's worden uitgevoerd en informatie over hoe Azure Machine Learning wordt uitgevoerd experimenten, specifiek een experiment die lokaal wordt uitgevoerd, op een externe virtuele machine en op een HDInsight-Cluster. Deze sectie wordt een overzicht maken van een compute-doel voor het uitvoeren van uw experimenten vanaf.

>[!NOTE]
>Voor de rest van dit artikel gebruiken we de CLI (opdrachtregelinterface)-opdrachten voor het weergeven van de concepten en de mogelijkheden. Hier wordt beschreven mogelijkheden kunnen ook worden gebruikt vanuit Workbench.

## <a name="launching-the-cli"></a>Starten van de CLI
Een eenvoudige manier om te starten van de CLI openen van een project in de Workbench en te navigeren naar **File--> Open de opdrachtprompt**.

![](media/experimentation-service-configuration/opening-cli.png)

Met deze opdracht wordt een terminalvenster waarin kunt u opdrachten voor het uitvoeren van scripts in de huidige projectmap gestart. Deze terminalvenster is geconfigureerd met de 3.5.2 Python-omgeving, die wordt geïnstalleerd door de Workbench.

>[!NOTE]
> Wanneer u een uitvoert _az ml_ opdracht vanuit het opdrachtvenster hoeft te worden geverifieerd voor Azure. CLI maakt gebruik van een onafhankelijke verificatiecache en vervolgens de bureaublad-app en zich dus bij Workbench betekent niet dat u bent geverifieerd in uw omgeving CLI. Om te verifiëren, de volgende stappen uit te voeren. Verificatietoken cache lokaal voor een bepaalde periode zodat alleen u deze stappen herhalen moet wanneer het token is verlopen. Wanneer het token is verlopen of als u verificatiefouten ziet, voert u de volgende opdrachten uit:

```
# to authenticate 
$ az login

# to list subscriptions
$ az account list -o table

# to set current subscription to a particular subscription ID 
$ az account set -s <subscription_id>

# to verify your current Azure subscription
$ az account show
```

>[!NOTE] 
>Bij het uitvoeren van _az ml_ opdracht in een projectmap, zorg ervoor dat het project behoort tot een Azure Machine Learning-experimenten-account op de _huidige_ Azure-abonnement. Anders kan er uitvoeringsfouten optreden.


## <a name="running-scripts-and-experiments"></a>Uitvoeren van scripts en experimenten
Met Workbench, kunt u uw Python uitvoeren en scripts PySpark over diverse berekenen doelen met behulp van de _az ml experiment indienen_ opdracht. Deze opdracht moet een definitie van de configuratie uitvoeren. 

Workbench wordt een bijbehorende .runconfig-bestand gemaakt wanneer u een compute-doel maken, maar u kunt aanvullende run configuraties met maken _az ml runconfiguration maken_ opdracht. U kunt ook handmatig de configuratiebestanden uitvoeren bewerken.

Voer configuraties weergegeven als onderdeel van een experiment uitvoert in de Workbench. 

>[!NOTE]
>U kunt meer informatie over de uitgevoerde configuratiebestand in de [Experiment uitvoering Configuration Reference](experimentation-service-configuration-reference.md) sectie.

## <a name="running-a-script-locally-on-workbench-installed-runtime"></a>Een script lokaal uitgevoerd op de runtime-Workbench zijn geïnstalleerd
Workbench kunt u uw scripts rechtstreeks met de Workbench geïnstalleerd Python 3.5.2 runtime uitvoeren. Deze standaard runtime is geïnstalleerd op de installatietijd Workbench en Azure Machine Learning-bibliotheken en -afhankelijkheden bevat. Voer resultaten en artefacten voor lokale uitvoeringen worden nog steeds opgeslagen in de geschiedenis-Service worden uitgevoerd in de cloud.

In tegenstelling tot uitvoering op basis van Docker en deze configuratie is _niet_ beheerd door Conda. U moet handmatig inrichten pakketafhankelijkheden voor uw lokale Workbench Python-omgeving.

U kunt de volgende opdracht om uw script lokaal uitvoeren in de Workbench geïnstalleerd Python-omgeving uitvoeren. 

```
$az ml experiment submit -c local myscript.py
```

Het pad naar de standaard Python-omgeving kunt u vinden door de volgende opdracht te typen in het venster Workbench CLI.
```
$ conda env list
```

>[!NOTE]
>Met het PySpark lokaal uitvoeren rechtstreeks in lokale Spark omgevingen is momenteel **niet** ondersteund. Workbench ondersteunt PySpark scripts die worden uitgevoerd op de lokale Docker. Azure Machine Learning basisinstallatiekopie Docker wordt geleverd met Spark 2.1.11 vooraf zijn geïnstalleerd. 

_**Overzicht van de lokale uitvoering voor een pythonscript:**_
![](media/experimentation-service-configuration/local-native-run.png)

## <a name="running-a-script-on-local-docker"></a>Een script uitgevoerd op lokale Docker
U kunt ook uw projecten in een Docker-container op uw lokale machine uitvoeren via experimenteren-Service. Workbench biedt een basisinstallatiekopie van Docker die wordt geleverd met Azure Machine Learning-bibliotheken en ook als Spark 2.1.11 runtime dat gemakkelijker lokale Spark uitvoeringen. Docker moet al worden uitgevoerd op de lokale computer.

Uw Python of PySpark-script wordt uitgevoerd op de lokale Docker, kunt u de volgende opdrachten uitvoeren in de CLI.

```
$az ml experiment submit -c docker myscript.py
```
of
```
az ml experiment submit --run-configuration docker myscript.py
```

De omgeving worden uitgevoerd op de lokale Docker is voorbereid met behulp van de Azure Machine Learning-basisinstallatiekopie Docker. Deze afbeelding Workbench gedownload wanneer uitgevoerd voor de eerste keer en overlays met pakketten die zijn opgegeven in het bestand conda_dependencies.yml. Deze bewerking wordt de eerste uitgevoerd trager maar latere uitvoeringen zijn aanzienlijk sneller dankzij Workbench hergebruiken van lagen in de cache. 

>[!IMPORTANT]
>U moet uitvoeren _az ml experiment voorbereiden - c docker_ opdracht eerst naar de installatiekopie van het Docker voorbereiden voor de eerste keer uitvoert. U kunt ook instellen de **PrepareEnvironment** parameter op waar in uw bestand docker.runconfig. Deze actie wordt automatisch voorbereid voor uw omgeving als onderdeel van de uitvoering van uw uitvoeren.  

>[!NOTE]
>Als een PySpark-script uitgevoerd op Spark, wordt ook spark_dependencies.yml naast conda_dependencies.yml gebruikt.

Uw scripts uitgevoerd op een Docker-installatiekopie, hebt u de volgende voordelen:

1. Hiermee zorgt u ervoor dat uw script betrouwbaar kan worden uitgevoerd in andere omgevingen kan worden uitgevoerd. Uitgevoerd op een Docker-container, helpt u bij het detecteren en te voorkomen dat alle lokale verwijzingen die mogelijk van invloed draagbaarheid. 

2. Hiermee kunt u snel testen code op runtimes en frameworks die complex om te installeren en configureren, zoals Apache Spark, zonder dat ze zelf installeren.


_**Overzicht van de lokale Docker-uitvoering voor een pythonscript:**_
![](media/experimentation-service-configuration/local-docker-run.png)

## <a name="running-a-script-on-a-remote-docker"></a>Een script uitgevoerd op een externe Docker
In sommige gevallen bronnen beschikbaar zijn op uw lokale machine mogelijk niet voldoende is voor het gewenste model trainen. In dit geval kunnen experimenteren Service een eenvoudige manier om uw Python of PySpark-scripts uitvoeren op krachtiger virtuele machines met behulp van Docker uitvoering op afstand. 

Extern VM moet voldoen aan de volgende vereisten:
* Externe virtuele machine moet worden uitgevoerd Linux Ubuntu en moet toegankelijk zijn via SSH. 
* Extern VM moet beschikken over Docker uitgevoerd.

>[!IMPORTANT]
> VM's van Windows wordt uitgevoerd Docker is **niet** ondersteund als externe compute-doelen


De volgende opdracht kunt u zowel de compute-doel-definitie maken en uitvoeren van de configuratie voor externe op basis van Docker-uitvoeringen.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```

Zodra u de compute-doel hebt geconfigureerd, kunt u de volgende opdracht om uit te voeren van uw script.
```
$ az ml experiment submit -c remotevm myscript.py
```
>[!NOTE]
>Houd er rekening mee dat uitvoering omgeving hiervoor is geconfigureerd met behulp van de specificaties in conda_dependencies.yml. spark_dependencies.yml wordt ook gebruikt als PySpark-framework is opgegeven in .runconfig-bestand. 

Het proces van de constructie Docker voor externe VM's is precies hetzelfde als het proces voor het lokale Docker wordt uitgevoerd, zodat u een vergelijkbare ervaring voor de uitvoering hiervan mogen verwachten.

>[!TIP]
>Als u liever om te voorkomen dat de latentie geïntroduceerd door het bouwen van de Docker-afbeelding voor de eerste keer uitvoert, kunt u de volgende opdracht voor het voorbereiden van het doel compute voordat het script wordt uitgevoerd. AZ ml experiment - c remotedocker voorbereiden


_**Overzicht van externe vm-uitvoering voor een pythonscript:**_
![](media/experimentation-service-configuration/remote-vm-run.png)


## <a name="running-a-script-on-an-hdinsight-cluster"></a>Een script uitgevoerd op een HDInsight-cluster
HDInsight is een populair platform voor big data-analyses Apache Spark ondersteunen. Workbench kunt experimenteren big Data met behulp van HDInsight Spark-clusters. 

>[!NOTE]
>Het HDInsight-cluster moet Azure Blob gebruiken als de primaire opslag. Het gebruik van Azure Data Lake-opslag wordt nog niet ondersteund.

U kunt een compute-doel maken en configuratie voor een HDInsight Spark-cluster met de volgende opdracht uitvoeren:

```
$ az ml computetarget attach cluster --name "myhdi" --address "<FQDN or IP address>" --username "sshuser" --password "sshpassword"  
```

>[!NOTE]
>Als u de FQDN-naam in plaats van een IP-adres gebruiken en de naam van uw Spark HDI-cluster is _foo_, het SSH-eindpunt is in het Stuurprogrammaknooppunt met de naam _foo-ssh.azurehdinsight.net_. Vergeet niet de **-ssh** postfix in de servernaam, wanneer u de FQDN-naam voor _--adres_ parameter.


Zodra u de compute-context hebt, kunt u de volgende opdracht uit te voeren van uw script PySpark kunt uitvoeren.

```
$ az ml experiment submit -c myhdi myscript.py
```

Workbench wordt voorbereid en uitvoeringsomgeving op HDInsight-cluster via Conda beheert. Configuratie wordt beheerd door _conda_dependencies.yml_ en _spark_dependencies.yml_ configuratiebestanden. 

SSH-toegang tot het HDInsight-cluster moet u om te experimenten in deze modus uitvoeren. 

>[!NOTE]
>Ondersteunde configuratie is dat actief Linux (Ubuntu met Python/PySpark 3.5.2 en Spark 2.1.11) HDInsight Spark-clusters.

_**Overzicht van HDInsight-kan worden uitgevoerd voor een PySpark-script**_
![](media/experimentation-service-configuration/hdinsight-run.png)


## <a name="running-a-script-on-gpu"></a>Een script uitgevoerd op GPU
Uw scripts op GPU uitgevoerd, kunt u de instructies in dit artikel:[GPU gebruiken in Azure Machine Learning](how-to-use-gpu.md)

## <a name="using-ssh-key-based-authentication-for-creating-and-using-compute-targets"></a>Met behulp van verificatie op basis van de SSH-sleutel voor het maken en gebruiken van compute-doelen
Azure Machine Learning Workbench kunt u maken en gebruiken van verificatie op basis van de SSH-sleutel naast het schema gebruikersnaam/wachtwoord gebaseerde compute-doelen. U kunt deze mogelijkheid gebruiken bij gebruik van remotedocker of het cluster als het doel-compute. Wanneer u dit schema gebruikt, wordt de Workbench maakt u een openbaar/persoonlijk sleutelpaar en rapporten weer de openbare sleutel. U toevoegen de openbare sleutel aan de ~/.ssh/authorized_keys-bestanden voor uw gebruikersnaam. Azure Machine Learning Workbench gebruikt vervolgens ssh verificatie op basis van een sleutel voor het gebruiken en op het doel van deze berekening wordt uitgevoerd. Omdat de persoonlijke sleutel voor de compute-doel wordt opgeslagen in de sleutelarchief voor de werkruimte, kunnen andere gebruikers van de werkruimte gebruiken de compute-doel dezelfde manier aan de hand van de gebruikersnaam die is opgegeven voor het maken van de compute-doel.  

U Volg deze stappen als deze functionaliteit wilt gebruiken. 

- Maak een compute-doelserver met behulp van een van de volgende opdrachten.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
```
of
```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" -k
```
- De openbare sleutel die is gegenereerd door de Workbench naar het bestand ~/.ssh/authorized_keys op het gekoppelde compute-doel toevoegen. 

>[!IMPORTANT]
>U moet de compute-doelserver met behulp van de gebruikersnaam die u gebruikt voor het maken van de compute-doel aanmelden. 

- U kunt nu voorbereiden en gebruiken van de compute-doelserver met behulp van SSH-verificatie op basis van sleutels.

```
az ml experiment prepare -c remotevm
```

## <a name="next-steps"></a>Volgende stappen
* [Maak en installeer Azure Machine Learning](quickstart-installation.md)
* [Model-Management](model-management-overview.md)
