---
title: Azure Machine Learning experimenten-Service configureren | Microsoft Docs
description: In dit artikel bevat een overzicht van Azure Machine Learning experimenten-Service met instructies over hoe u dit moet worden geconfigureerd.
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
ms.openlocfilehash: e79817ffad139e0a3bcb0ba32b9bc6e5666319d0
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643647"
---
# <a name="configuring-azure-machine-learning-experimentation-service"></a>Azure Machine Learning experimenten-Service configureren

## <a name="overview"></a>Overzicht
Azure Machine Learning experimenten-Service kunnen gegevenswetenschappers hun met behulp van de uitvoering van de Azure Machine Learning experimenten uitvoeren en beheerfuncties uitvoeren. Het biedt een raamwerk voor flexibele experimenteren met snel herhalingen. Azure Machine Learning Workbench kunt u beginnen met lokaal wordt uitgevoerd op uw computer en ook een eenvoudige manier voor het schalen en uitbreiden naar andere omgevingen zoals remote Data Science-VM's met GPU of met Spark voor HDInsight-Clusters.

Experimenten-Service is gebouwd voor het ontwikkelen van geïsoleerde, reproduceerbare en consistente uitvoeringen van de experimenten nodig hebben. Hiermee kunt u uw compute-doelen, uitvoeringsomgevingen, beheren en configuraties worden uitgevoerd. Met behulp van de uitvoering van Azure Machine Learning Workbench en mogelijkheden voor uitvoeren, kunt u eenvoudig verplaatsen tussen verschillende omgevingen. 

U kunt een Python of PySpark-script uitvoeren in een Workbench-project lokaal of op schaal in de cloud. 

U kunt uw scripts uitvoeren op: 

* Python (3.5.2)-omgeving op de lokale computer is geïnstalleerd in Workbench
* Conda-Python-omgeving binnen een Docker-container op lokale computer
* Op een Python-omgeving die u bezit en beheren op een externe Linux-computer
* Conda-Python-omgeving binnen een Docker-container op een externe Linux-computer. Bijvoorbeeld, een () [Ubuntu gebaseerde DSVM op Azure]https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) op Azure

>[!IMPORTANT]
>Azure Machine Learning experimenten-Service ondersteunt momenteel Python 3.5.2 gebruikt en Spark 2.1.11 als runtime-versies, Python en Spark, respectievelijk. 


### <a name="key-concepts-in-experimentation-service"></a>Belangrijke concepten in experimenten-Service
Het is belangrijk om te begrijpen van de volgende concepten in Azure Machine Learning-experiment worden uitgevoerd. In de volgende secties besproken over het gebruik van deze concepten in detail. 

#### <a name="compute-target"></a>COMPUTE-doel
Een _compute-doel_ geeft aan waar uw programma, zoals uw bureaublad, externe Docker op een virtuele machine of een cluster worden uitgevoerd. Een compute-doel moet worden opgevraagd en toegankelijk is voor u. Workbench kunt u de compute-doelen maken en beheren met behulp van de Workbench-toepassing en de CLI. 

_AZ ml computetarget koppelen_ opdracht in de CLI kunt u maken van een compute-doel die u in uw wordt uitgevoerd gebruiken kunt.

Ondersteunde compute-doelen zijn:
* Lokale omgeving voor Python (3.5.2 gebruikt) op uw computer geïnstalleerd in Workbench.
* Lokale Docker op uw computer
* Een beheerde gebruiker, Python-omgeving op externe Linux Ubuntu-VM's. Bijvoorbeeld, een [Ubuntu gebaseerde DSVM op Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* Externe Docker op virtuele Linux-Ubuntu-machines. Bijvoorbeeld, een [Ubuntu gebaseerde DSVM op Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight voor Spark-cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/) op Azure

Experimenten-Service op dit moment ondersteunt Python 3.5.2 gebruikt en Spark-2.1.11 als Python en Spark runtimeversies, respectievelijk. 

>[!IMPORTANT]
> Windows-VM's actief zijn Docker **niet** ondersteund als externe compute-doelen.

#### <a name="execution-environment"></a>Uitvoeringsomgeving
De _uitvoeringsomgeving_ definieert de runtime-configuratie en de afhankelijkheden die nodig zijn om uit te voeren van het programma in Workbench.

U beheert de lokale uitvoeringsomgeving met behulp van uw favoriete hulpprogramma's en Pakketbeheer, als u op de runtime van de standaard Workbench uitvoert. 

Conda wordt gebruikt voor het beheren van lokale Docker en externe Docker-uitvoeringen, evenals op basis van een HDInsight-uitvoeringen. Voor deze-doelen COMPUTE, de configuratie van de uitvoering omgeving wordt beheerd via **Conda_dependencies.yml** en **Spark_dependencies.yml bestanden**. Deze bestanden zijn de **aml_config** map in uw project.

**Ondersteunde runtimes voor uitvoeringsomgevingen zijn:**
* Python 3.5.2 gebruikt
* Spark 2.1.11

### <a name="run-configuration"></a>Configuratie van de uitvoering
Naast de compute-omgeving voor doel en de uitvoering, Azure Machine Learning biedt een raamwerk om te definiëren en te wijzigen *uitvoerconfiguraties*. Verschillende uitvoeringen van uw experiment mogelijk andere configuratie als onderdeel van iteratieve experimenten. U verstrekkende bereiken met verschillende parameters, om de volgende met behulp van verschillende gegevensbronnen en het afstemmen van de spark-parameters. Experimenten-Service biedt een raamwerk voor het beheren van configuraties uitvoeren.

Met _az ml computetarget koppelen_ opdracht geeft twee bestanden in uw **aml_config** map in uw project: een '.compute' en een '.runconfig' volgen van deze overeenkomst: _< your_ computetarget_name > .compute_ en _< your_computetarget_name > .runconfig_. Het bestand .runconfig wordt automatisch gemaakt voor uw gemak bij het maken van een compute-doel. U kunt maken en beheren van andere uitvoeren configuraties met behulp van _az ml runconfigurations_ opdracht in CLI. U kunt ook maken en deze te bewerken op uw bestandssysteem.

Uitvoeren van de configuratie in Workbench kunt u omgevingsvariabelen opgeven. U kunt omgevingsvariabelen opgeven en deze gebruiken in uw code door de volgende sectie in het bestand .runconfig toe te voegen. 

```
EnvironmentVariables:
    "EXAMPLE_ENV_VAR1": "Example Value1"
    "EXAMPLE_ENV_VAR2": "Example Value2"
```

Deze omgevingsvariabelen kunnen worden geopend in uw code. Bijvoorbeeld: de omgevingsvariabele met de naam 'EXAMPLE_ENV_VAR1' in dit codefragment phyton worden afgedrukt
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

_**De volgende afbeelding ziet u de stroom op hoog niveau voor de eerste experiment uitvoeren.**_
![](media/experimentation-service-configuration/experiment-execution-flow.png)

## <a name="experiment-execution-scenarios"></a>Scenario's voor het uitvoeren van experiment
In deze sectie we Duik in de uitvoering van scenario's en meer informatie over hoe Azure Machine Learning wordt uitgevoerd experimenten een experiment specifiek lokaal, worden uitgevoerd op een externe virtuele machine, en op een HDInsight-Cluster. In deze sectie is een overzicht vanaf het maken van een compute-doel op het uitvoeren van de experimenten nodig hebben.

>[!NOTE]
>Voor de rest van dit artikel gebruiken we de CLI (Command line interface)-opdrachten om de concepten en de mogelijkheden te geven. Hier wordt beschreven mogelijkheden kunnen ook worden gebruikt in Workbench.

## <a name="launching-the-cli"></a>Starten van de CLI
Een eenvoudige manier om de CLI starten openen van een project in Workbench en te navigeren naar **bestand--> opdrachtprompt openen**.

![](media/experimentation-service-configuration/opening-cli.png)

Met deze opdracht start een terminal-venster waarin u scripts uitvoeren in de huidige projectmap-opdrachten kunt invoeren. Dit terminalvenster is geconfigureerd met de omgeving Python 3.5.2 gebruikt, die is geïnstalleerd in Workbench.

>[!NOTE]
> Wanneer u een uitvoert _az ml_ opdracht vanuit het opdrachtvenster, u moet worden geverifieerd aan de hand van Azure. CLI maakt gebruik van een onafhankelijke verificatiecache en vervolgens de desktop-app en dus Meld u aan bij Workbench betekent niet dat u bent geverifieerd in de CLI-omgeving. Als u wilt verifiëren, gebruikt u de volgende stappen. Verificatietoken is lokaal cache voor een bepaalde periode, zodat u alleen Herhaal deze stappen wilt wanneer het token is verlopen. Wanneer het token is verlopen of als u verificatiefouten ziet, voert u de volgende opdrachten uit:

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
>Bij het uitvoeren van _az ml_ opdracht in een projectmap, zorg ervoor dat het project deel uitmaakt van een Azure Machine Learning experimenten-account op de _huidige_ Azure-abonnement. Anders kunnen worden uitgevoerd-fouten optreden.


## <a name="running-scripts-and-experiments"></a>Uitvoeren van scripts en experimenten
Workbench, kunt u uw Python uitvoeren en PySpark-scripts in diverse compute-doelen die met behulp van de _az ml experiment verzenden_ opdracht. Deze opdracht moet een definitie van de configuratie uitvoeren. 

Workbench wordt een bijbehorende runconfig-bestand gemaakt wanneer u een compute-doel maken, maar kunt u aanvullende configuraties die uitvoeren met behulp van _az ml runconfiguration maken_ opdracht. U kunt ook handmatig de run-configuratiebestanden bewerken.

Voer configuraties weergegeven als onderdeel van het experiment uitvoert in Workbench. 

>[!NOTE]
>U kunt meer informatie over het uitvoeren configuratiebestand in de [Experiment uitvoering configuratie verwijzing](experimentation-service-configuration-reference.md) sectie.

## <a name="running-a-script-locally-on-workbench-installed-runtime"></a>Een script lokaal wordt uitgevoerd op de runtime Workbench zijn geïnstalleerd
Workbench kunt u uw scripts rechtstreeks uitvoeren in de Workbench geïnstalleerd Python 3.5.2-runtime. Deze standaard runtime is geïnstalleerd op de installatietijd Workbench en Azure Machine Learning-bibliotheken en afhankelijkheden bevat. Resultaten van de uitvoering en artefacten voor lokale uitvoeringen worden nog steeds in de geschiedenis-Service worden uitgevoerd in de cloud opgeslagen.

In tegenstelling tot uitvoeringen Docker op basis van deze configuratie is _niet_ beheerd door Conda. U moet handmatig richt pakketafhankelijkheden voor uw lokale Workbench Python-omgeving.

U kunt de volgende opdracht om het script lokaal uitvoeren in de Workbench geïnstalleerd Python-omgeving uitvoeren. 

```
$az ml experiment submit -c local myscript.py
```

U kunt het pad naar de standaardomgeving Python vinden door de volgende opdracht te typen in de Workbench CLI-venster.
```
$ conda env list
```

>[!NOTE]
>Met het PySpark lokaal uitvoeren rechtstreeks in de lokale Spark omgevingen is momenteel **niet** ondersteund. Workbench biedt ondersteuning voor PySpark-scripts die worden uitgevoerd op een lokale Docker. Azure Machine Learning Docker-basisinstallatiekopie wordt geleverd met Spark 2.1.11 vooraf zijn geïnstalleerd. 

_**Overzicht van de lokale uitvoering voor een Python-script:**_
![](media/experimentation-service-configuration/local-native-run.png)

## <a name="running-a-script-on-local-docker"></a>Een script is uitgevoerd op een lokale Docker
U kunt ook uw projecten in een Docker-container uitvoeren op uw lokale computer via experimenten-Service. Workbench biedt een Docker-basisinstallatiekopie die wordt geleverd met Azure Machine Learning-bibliotheken en ook als Spark 2.1.11 runtime lokale Spark uitvoeringen om eenvoudig te maken. Docker moet al worden uitgevoerd op de lokale computer.

Voor uw Python of PySpark-script is uitgevoerd op een lokale Docker, kunt u de volgende opdrachten uitvoeren in CLI.

```
$az ml experiment submit -c docker myscript.py
```
of
```
az ml experiment submit --run-configuration docker myscript.py
```

De omgeving worden uitgevoerd op een lokale Docker is voorbereid met behulp van de Azure Machine Learning-basisinstallatiekopie Docker. Workbench wordt deze installatiekopie gedownload wanneer uitvoeren voor de eerste keer en overlays met pakketten die zijn opgegeven in het bestand conda_dependencies.yml. Met deze bewerking wordt de eerste uitgevoerd tragere maar volgende uitvoeringen zijn aanzienlijk sneller dankzij de Workbench opnieuw gebruiken in de cache lagen. 

>[!IMPORTANT]
>U moet uitvoeren _az ml experiment voorbereiden - c docker_ opdracht om de Docker-installatiekopie voorbereiden voor de eerste keer uitvoert. U kunt ook instellen de **PrepareEnvironment** parameter op waar in uw bestand docker.runconfig. Deze actie wordt automatisch voorbereid voor uw omgeving als onderdeel van uw uitgevoerd kan worden uitgevoerd.  

>[!NOTE]
>Als u een PySpark-script op Spark is uitgevoerd, wordt ook spark_dependencies.yml naast conda_dependencies.yml gebruikt.

Uw scripts uitgevoerd op een Docker-installatiekopie biedt u de volgende voordelen:

1. Het zorgt ervoor dat uw script op betrouwbare wijze kan worden uitgevoerd in andere omgevingen worden uitgevoerd. Wordt uitgevoerd op een Docker-container, kunt u detecteren en te voorkomen dat een lokale referenties weergegeven die mogelijk van invloed op draagbaarheid. 

2. Hiermee kunt u snel testen van code op runtimes en frameworks die complex om te installeren en configureren zijn, zoals Apache Spark, zonder dat ze zelf installeren.


_**Overzicht van de lokale Docker-uitvoering voor een Python-script:**_
![](media/experimentation-service-configuration/local-docker-run.png)

## <a name="running-a-script-on-a-remote-docker"></a>Een script uitgevoerd op een externe Docker
In sommige gevallen resources beschikbaar zijn op uw lokale computer mogelijk niet voldoende zijn voor het gewenste model te trainen. In dit geval kan experimenten-Service een eenvoudige manier om uw Python of PySpark-scripts uitvoeren op krachtige virtuele machines met behulp van de externe Docker-uitvoering. 

Externe virtuele machine moet voldoen aan de volgende vereisten:
* Externe virtuele machine moet worden uitgevoerd Linux Ubuntu en moet toegankelijk zijn via SSH. 
* Externe virtuele machine moet hebben met Docker.

>[!IMPORTANT]
> Windows-VM's actief Docker is **niet** ondersteund als externe compute-doelen


De volgende opdracht kunt u zowel de compute-doel-definitie maken en uitvoeren van de configuratie voor externe op basis van een Docker-uitvoeringen.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```

Nadat u de compute-doel hebt geconfigureerd, kunt u de volgende opdracht uit uw script uit te voeren.
```
$ az ml experiment submit -c remotevm myscript.py
```
>[!NOTE]
>Houd er rekening mee dat uitvoering omgeving is geconfigureerd met behulp van de specificaties in conda_dependencies.yml. spark_dependencies.yml wordt ook gebruikt als PySpark-framework in .runconfig bestand is opgegeven. 

Het proces voor het bouwen van Docker voor externe virtuele machines is precies hetzelfde als het proces voor het lokale Docker wordt uitgevoerd, zodat u kunt een vergelijkbare ervaring voor de uitvoering verwachten.

>[!TIP]
>Als u liever om te voorkomen dat de latentie minimaal door het bouwen van de Docker-installatiekopie voor de eerste keer uitvoert, kunt u de volgende opdracht uit om voor te bereiden van de compute-doel voordat het script wordt uitgevoerd. AZ ml-experiment - c remotedocker voorbereiden

_**Overzicht van de uitvoering van externe virtuele machine voor een Python-script:**_
![](media/experimentation-service-configuration/remote-vm-run.png)

## <a name="running-a-script-on-a-remote-vm-targeting-user-managed-environments"></a>Een script is uitgevoerd op een externe virtuele machine die zijn gericht op gebruikers beheerde omgevingen
Experimenten-service biedt ook ondersteuning voor het uitvoeren van een script op de Python-omgeving van gebruiker in een externe virtuele Ubuntu-machine. Hiermee kunt u uw eigen omgeving voor uitvoering beheren en nog steeds gebruikmaken van Azure Machine Learning-mogelijkheden. 

Volg de volgende stappen om uw script uitvoeren op uw eigen omgeving.
* Bereid uw Python-omgeving op een externe Ubuntu-VM of een installatie van uw afhankelijkheden DSVM.
* Installeer Azure Machine Learning eisen aan de hand van de volgende opdracht uit.

```
pip install -I --index-url https://azuremldownloads.azureedge.net/python-repository/preview --extra-index-url https://pypi.python.org/simple azureml-requirements
```

>[!TIP]
>In sommige gevallen moet u mogelijk met deze opdracht uitvoeren in de sudo-modus, afhankelijk van uw bevoegdheden. 
```
sudo pip install -I --index-url https://azuremldownloads.azureedge.net/python-repository/preview --extra-index-url https://pypi.python.org/simple azureml-requirements
```
 
* Gebruik de volgende opdracht om de definitie van de compute-doel en de uitvoeringsconfiguratie voor beheerde gebruiker worden uitgevoerd op externe uitvoeringen van de virtuele machine te maken.
```
az ml computetarget attach remote --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```
>[!NOTE]
>Hierdoor wordt de parameter 'userManagedEnvironment' ingesteld in uw configuratiebestand .compute op ' True '.

* Locatie van uw Python-runtime uitvoerbare instellen in uw bestand .compute. U moet verwijzen naar het volledige pad van het uitvoerbare bestand python. 
```
pythonLocation: python3
```

Nadat u de compute-doel hebt geconfigureerd, kunt u de volgende opdracht uit uw script uit te voeren.
```
$ az ml experiment submit -c remotevm myscript.py
```

>[!NOTE]
> Als u op een DSVM uitvoert, moet u de volgende opdrachten gebruiken

Als u rechtstreeks op de DSVM globale python-omgeving uitvoeren wilt, moet u deze opdracht uitvoeren.
```
sudo /anaconda/envs/py35/bin/pip install <package>
```


## <a name="running-a-script-on-an-hdinsight-cluster"></a>Een script is uitgevoerd op een HDInsight-cluster
HDInsight is een populair platform voor big data-analyse ondersteunen van Apache Spark. Workbench kunt experimenten van big data met HDInsight Spark-clusters. 

>[!NOTE]
>Het HDInsight-cluster moet Azure Blob als primaire opslag gebruiken. Het gebruik van Azure Data Lake-opslag wordt nog niet ondersteund.

U kunt een compute-doel maken en uitvoeren van de configuratie voor een HDInsight Spark-cluster met behulp van de volgende opdracht uit:

```
$ az ml computetarget attach cluster --name "myhdi" --address "<FQDN or IP address>" --username "sshuser" --password "sshpassword"  
```

>[!NOTE]
>Als u de FQDN-naam in plaats van een IP-adres gebruiken en uw HDI Spark-cluster met de naam _foo_, het SSH-eindpunt is in het Stuurprogrammaknooppunt met de naam _foo-ssh.azurehdinsight.net_. Vergeet niet de **-ssh** achtervoegsel in de naam van de server bij het gebruik van de FQDN-naam voor _--adres_ parameter.


Zodra u de compute-context hebt, kunt u de volgende opdracht uit te voeren van de PySpark-script kunt uitvoeren.

```
$ az ml experiment submit -c myhdi myscript.py
```

Workbench voorbereidt en uitvoeringsomgeving op HDInsight-cluster met behulp van Conda beheert. De configuratie wordt beheerd door _conda_dependencies.yml_ en _spark_dependencies.yml_ -configuratiebestanden. 

SSH-toegang tot het HDInsight-cluster hebt u nodig om experimenten in deze modus uitvoeren. 

>[!NOTE]
>Ondersteunde configuratie is dat HDInsight Spark-clusters actieve Linux (Ubuntu met Python/PySpark 3.5.2 gebruikt en Spark 2.1.11).

_**Overzicht van HDInsight op basis van voor het uitvoeren van een PySpark-script**_
![](media/experimentation-service-configuration/hdinsight-run.png)


## <a name="running-a-script-on-gpu"></a>Uitvoeren van een script op GPU
Als u wilt uw scripts uitvoeren op GPU, kunt u de instructies in dit artikel volgen:[GPU in Azure Machine Learning gebruiken](how-to-use-gpu.md)

## <a name="using-ssh-key-based-authentication-for-creating-and-using-compute-targets"></a>Met behulp van verificatie op basis van de SSH-sleutel voor het maken en gebruiken van compute-doelen
Azure Machine Learning Workbench kunt u maken en gebruiken met behulp van verificatie op basis van de SSH-sleutel naast het schema gebruikersnaam/wachtwoord gebaseerde compute-doelen. U kunt deze mogelijkheid kunt gebruiken bij het gebruik van remotedocker of het cluster als uw compute-doel. Wanneer u dit schema gebruikt, wordt de Workbench maakt u een openbaar/persoonlijk sleutelpaar en rapporten weer de openbare sleutel. U toevoegen de openbare sleutel aan de ~/.ssh/authorized_keys-bestanden voor uw gebruikersnaam. Azure Machine Learning Workbench gebruikt vervolgens ssh-sleutel gebaseerde verificatie voor het raadplegen en uitvoeren op deze compute-doel. Omdat de persoonlijke sleutel voor de compute-doel wordt opgeslagen in het archief met sleutel voor de werkruimte, andere gebruikers van de werkruimte kunnen gebruiken de compute-doel dezelfde manier door te geven van de gebruikersnaam die is opgegeven voor het maken van de compute-doel.  

U Volg deze stappen voor het gebruik van deze functionaliteit. 

- Maak een compute-doel met behulp van een van de volgende opdrachten.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
```
of
```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" -k
```
- De openbare sleutel die is gegenereerd door de Workbench naar het bestand ~/.ssh/authorized_keys op de gekoppelde compute-doel toevoegen. 

>[!IMPORTANT]
>U moet zich op de compute-doel met behulp van de gebruikersnaam die u gebruikt voor het maken van de compute-doel. 

- U kunt nu voorbereiden en gebruiken van de compute-doel met behulp van SSH-sleutel gebaseerde verificatie.

```
az ml experiment prepare -c remotevm
```

## <a name="next-steps"></a>Volgende stappen
* [Maak en installeer Azure Machine Learning](../service/quickstart-installation.md)
* [Modelbeheer](model-management-overview.md)
