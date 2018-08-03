---
title: Prognose voor terabytes aan gegevens - Azure serverworkload | Microsoft Docs
description: Klik hier voor meer informatie over het trainen van een machine learning-model van big data met behulp van Azure Machine Learning Workbench.
services: machine-learning
documentationcenter: ''
author: daden
manager: mithal
editor: daden
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: daden
ms.openlocfilehash: 7a13cafd3dcfb4637a5deae2c678c518019ad168
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39460245"
---
# <a name="server-workload-forecasting-on-terabytes-of-data"></a>Prognose voor serverworkload per terabytes aan gegevens

In dit artikel bevat informatie over hoe data scientists Azure Machine Learning Workbench kunnen gebruiken voor het ontwikkelen van oplossingen waarvoor het gebruik van big data. U kunt starten vanuit een voorbeeld van een grote gegevensset, doorlopen van de gegevens voor te bereiden, feature-engineering en machine learning en vervolgens het proces voor de hele grote gegevensset kunnen worden uitgebreid. 

U krijgt informatie over de volgende belangrijke mogelijkheden van Machine Learning Workbench:
* Eenvoudig schakelen tussen compute-doelen. U kunt verschillende compute-doelen instellen en deze gebruiken in experimenten. In dit voorbeeld gebruikt u een Ubuntu-DSVM en een Azure HDInsight-cluster als de compute-doelen. U kunt ook de compute-doelen, afhankelijk van de beschikbaarheid van resources configureren. U kunt in het bijzonder na het schalen van het Spark-cluster met meer worker-knooppunten, de resources via Machine Learning Workbench gebruiken om experimenten te versnellen.
* Tracering van geschiedenis worden uitgevoerd. U kunt Machine Learning Workbench gebruiken voor het bijhouden van de prestaties van machine learning-modellen en andere metrische gegevens van belang zijn.
* De uitoefening van het machine learning-model. De ingebouwde hulpprogramma's in Machine Learning Workbench kunt u het getrainde machine learning-model als een webservice in Azure Container Service implementeren. U kunt de webservice ook gebruiken om op te halen Mini batch voorspellingen via REST API-aanroepen. 
* Ondersteuning voor terabytes gegevens.

> [!NOTE]
> Zie voor voorbeelden van code en andere materialen die betrekking hebben op dit voorbeeld [GitHub](https://github.com/Azure/MachineLearningSamples-BigData).
> 

## <a name="use-case-overview"></a>Gebruik case-overzicht

Prognose van de werkbelasting op servers is een algemene zakelijke behoeften voor technologiebedrijven die hun eigen infrastructuur beheren. Als u wilt verkleinen infrastructuur hoeft te investeren, moeten services die worden uitgevoerd op servers onder gebruikt om uit te voeren op een kleiner aantal machines worden gegroepeerd. Services die worden uitgevoerd op overbelaste servers moeten krijgen meer virtuele machines om uit te voeren. 

In dit scenario, u zich richten op werkbelasting voorspelling voor elke machine (of server). In het bijzonder, kunt u de sessiegegevens op elke server in de toekomst voorspellen van de werkbelastingsklasse van de server. Classificeren van de belasting van elke server in de laag, Gemiddeld of hoog klassen met behulp van de willekeurige Forest classificatie in [Apache Spark ML](https://spark.apache.org/docs/2.1.1/ml-guide.html). De machine learning-technieken en werkstroom in dit voorbeeld kan eenvoudig worden uitgebreid naar andere vergelijkbare problemen. 


## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn er als volgt uit:

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).
* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). Het programma te installeren en een werkruimte maken, Zie de [Quick Start-installatiehandleiding](../service/quickstart-installation.md). Als u meerdere abonnementen hebt, kunt u [het gewenste abonnement om te worden van het huidige actieve abonnement instellen](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set).
* Windows 10 (de instructies in dit voorbeeld zijn in het algemeen hetzelfde voor macOS-systemen).
* Een Data Science Virtual Machine (DSVM) voor Linux (Ubuntu), bij voorkeur in de regio VS-Oost waar de gegevens zoekt. U kunt een Ubuntu-DSVM inrichten door [deze instructies](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). U kunt ook zien [in deze Quick Start](https://ms.portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu). Het is raadzaam om met behulp van een virtuele machine met ten minste 8 kerngeheugens en 32 GB aan geheugen. 

Ga als volgt de [instructie](../service/known-issues-and-troubleshooting-guide.md#remove-vm-execution-error-no-tty-present) om in te schakelen zonder wachtwoord sudoer toegang op de virtuele machine voor AML-Workbench.  U kunt gebruiken [SSH-sleutel gebaseerde verificatie voor het maken en gebruiken van de virtuele machine in de Workbench AML](experimentation-service-configuration.md#using-ssh-key-based-authentication-for-creating-and-using-compute-targets). In dit voorbeeld gebruiken we wachtwoord voor toegang tot de virtuele machine.  Opslaan in de volgende tabel met de gegevens van de DSVM voor de latere fasen:

 Veldnaam| Waarde |  
 |------------|------|
DSVM-IP-adres | xxx|
 Gebruikersnaam  | xxx|
 Wachtwoord   | xxx|


 U kunt het gebruik van een virtuele machine met [Docker-Engine](https://docs.docker.com/engine/) geïnstalleerd.

* Een HDInsight Spark-Cluster, met Hortonworks Data Platform-versie 3.6 en Spark-versie 2.1.x, bij voorkeur in de regio VS-Oost waar de gegevens zoekt. Ga naar [een Apache Spark-cluster maken in Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters) voor meer informatie over het maken van HDInsight-clusters. Het is raadzaam om met behulp van een cluster met drie-worker, met elke werknemer met 16 kernen en 112 GB aan geheugen. Of u kunt alleen VM-type `D12 V2` voor hoofdknooppunt, en `D14 V2` voor het worker-knooppunt. De implementatie van het cluster duurt ongeveer 20 minuten. U moet de clusternaam, het SSH-gebruikersnaam en het wachtwoord voor het uitproberen van dit voorbeeld. Opslaan in de volgende tabel met de gegevens van de Azure HDInsight-cluster voor de latere fasen:

 Veldnaam| Waarde |  
 |------------|------|
 Clusternaam| xxx|
 Gebruikersnaam  | xxx (sshuser standaard)|
 Wachtwoord   | xxx|


* Een Azure Storage-account. U kunt volgen [deze instructies](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) een te maken. Maak ook twee persoonlijke blob-containers met de namen `fullmodel` en `onemonthmodel` in dit opslagaccount. Het opslagaccount dat wordt gebruikt om op te slaan tussenliggende compute-resultaten en machine learning-modellen. U moet de naam en opslagaccountsleutel voor het uitproberen van dit voorbeeld. Opslaan in de volgende tabel met de gegevens van de Azure storage-account voor de latere fasen:

 Veldnaam| Waarde |  
 |------------|------|
 Naam van opslagaccount| xxx|
 Toegangssleutel  | xxx|


De Ubuntu-DSVM en de Azure HDInsight-cluster gemaakt in de lijst met vereiste zijn compute-doelen. COMPUTE-doelen worden de compute-resource in de context van Machine Learning Workbench, die kan afwijken van de computer waarop de Workbench wordt uitgevoerd.   

## <a name="create-a-new-workbench-project"></a>Een nieuw Workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Machine Learning Workbench.
2.  Op de **projecten** weergeeft, schakelt de **+** Meld u aan en selecteer **nieuw Project**.
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project.
4.  In de **zoeken naar projectsjablonen** zoekvak, type **prognose voor Terabytes gegevens Serverworkload**, en selecteer de sjabloon.
5.  Selecteer **Maken**.

U kunt een Workbench-project maken met een vooraf gemaakte git-opslagplaats door [deze instructie](./tutorial-classifying-iris-part-1.md).  
Voer `git status` om te controleren van de status van de bestanden voor versie bijhouden.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevens die worden gebruikt in dit voorbeeld is Werkbelastinggegevens kunstmatige-server. Deze wordt gehost in een Azure Blob storage-account dat openbaar toegankelijk is in de regio VS-Oost. De accountgegevens voor specifieke opslag te vinden in de `dataFile` veld [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json) in de notatie ' wasb: / /<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>'. U kunt de gegevens rechtstreeks vanuit de Blob-opslag gebruiken. Als de opslag wordt gebruikt door veel gebruikers gelijktijdig, kunt u [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) voor het downloaden van de gegevens in uw eigen opslag voor een betere ervaring voor experimenten. 

De totale gegevensgrootte is ongeveer 1 TB. Elk bestand is ongeveer 1-3 GB, en is in CSV-bestandsindeling, zonder koptekst. Elke rij gegevens vertegenwoordigt de belasting van een transactie op een bepaalde server. De gedetailleerde gegevens van het schema is als volgt:

Kolom getal | Veldnaam| Type | Beschrijving |  
|------------|------|-------------|---------------|
1  | `SessionStart` | Datum en tijd |    Begintijd sessie
2  |`SessionEnd`    | Datum en tijd | Eindtijd van sessie
3 |`ConcurrentConnectionCounts` | Geheel getal | Aantal gelijktijdige verbindingen
4 | `MbytesTransferred` | Double-waarde | Genormaliseerde gegevens overgebracht in megabytes
5 | `ServiceGrade` | Geheel getal |  Klasse van de service voor de sessie
6 | `HTTP1` | Geheel getal|  Sessie HTTP1 of HTTP2 gebruikt
7 |`ServerType` | Geheel getal   |Servertype
8 |`SubService_1_Load` | Double-waarde |   Subservice 1 laden
9 | `SubService_2_Load` | Double-waarde |  Subservice 2 laden
10 | `SubService_3_Load` | Double-waarde |     Subservice 3 laden
11 |`SubService_4_Load` | Double-waarde |  Subservice 4 laden
12 | `SubService_5_Load`| Double-waarde |      Subservice 5 laden
13 |`SecureBytes_Load`  | Double-waarde | Beveiligde bytes laden
14 |`TotalLoad` | Double-waarde | Totale belasting van server
15 |`ClientIP` | Reeks|    IP-adres van client
16 |`ServerIP` | Reeks|    Het IP-adres



Houd er rekening mee dat de verwachte gegevenstypen worden vermeld in de voorgaande tabel. Vanwege de ontbrekende waarden en problemen met vervuilde gegevens is er geen garantie dat de gegevenstypen daadwerkelijk zijn zoals verwacht. Gegevensverwerking moet deze in overweging nemen. 


## <a name="scenario-structure"></a>Scenario-structuur

De bestanden in dit voorbeeld zijn als volgt ingedeeld.

| Bestandsnaam | Type | Beschrijving |
|-----------|------|-------------|
| `Code` | Map | De map bevat de code in het voorbeeld. |
| `Config` | Map | De map bevat de configuratiebestanden. |
| `Image` | Map | De map die wordt gebruikt voor het opslaan van installatiekopieën voor het Leesmij-bestand. |
| `Model` | Map | De map die wordt gebruikt om op te slaan modelbestanden gedownload van Blob-opslag. |
| `Code/etl.py` | Python-bestand | Het Python-bestand gebruikt voor het voorbereiden van gegevens en feature-engineering. |
| `Code/train.py` | Python-bestand | Het Python-bestand gebruikt voor een drie-klasse multi-classfication-model te trainen.  |
| `Code/webservice.py` | Python-bestand | Het Python-bestand gebruikt voor uitoefening.  |
| `Code/scoring_webservice.py` | Python-bestand |  Het Python-bestand gebruikt voor gegevenstransformatie en de webservice aan te roepen. |
| `Code/O16Npreprocessing.py` | Python-bestand | Het Python-bestand met de voorverwerking van de gegevens voor scoring_webservice.py gebruikt.  |
| `Code/util.py` | Python-bestand | Het Python-bestand met de code voor het lezen en schrijven van Azure-blobs.  
| `Config/storageconfig.json` | JSON-bestand | Het configuratiebestand voor de Azure blob-container waarin de tussenliggende resultaten genereren en het model voor verwerking en training van gegevens van één maand. |
| `Config/fulldata_storageconfig.json` | JSON-bestand | Het configuratiebestand voor de Azure blob-container waarin de tussenliggende resultaten genereren en het model voor verwerking en training van een volledige gegevensset.|
| `Config/webservice.json` | JSON-bestand | Het configuratiebestand voor scoring_webservice.py.|
| `Config/conda_dependencies.yml` | YAML-bestand | Het Conda-afhankelijkheidsbestand. |
| `Config/conda_dependencies_webservice.yml` | YAML-bestand | Het Conda-afhankelijkheidsbestand voor de webservice.|
| `Config/dsvm_spark_dependencies.yml` | YAML-bestand | De Spark-afhankelijkheidsbestand voor Ubuntu-DSVM. |
| `Config/hdi_spark_dependencies.yml` | YAML-bestand | Het Spark-afhankelijkheid-bestand voor het HDInsight Spark-cluster. |
| `README.md` | Markdown-bestand | Het Leesmij-bestand markdown-bestand. |
| `Code/download_model.py` | Python-bestand | Het Python-bestand voor het downloaden van de modelbestanden met de Azure-blob naar een lokale schijf. |
| `Docs/DownloadModelsFromBlob.md` | Markdown-bestand | Het markdown-bestand die instructies bevat voor het uitvoeren van `Code/download_model.py`. |



### <a name="data-flow"></a>Gegevensstroom

De code in [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) laadt gegevens van de container openbaar toegankelijk is (`dataFile` veld [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json)). Het bevat gegevens voor te bereiden en feature-engineering en slaat de resultaten van de tussenliggende compute en modellen op uw eigen privé-container. De code in [ `Code/train.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/train.py) laadt de tussenliggende compute-resultaten van de privé-container, traint het classificatiemodel ROC-en het getrainde machine learning-model schrijft naar de privé-container. 

U moet een container voor experimenten op de gegevensset van één maand en één voor experimenten van de volledige gegevensset. Omdat de gegevens en modellen worden opgeslagen als Parquet-bestand, wordt elk bestand daadwerkelijk een map in de container, met meerdere blobs. De resulterende container ziet er als volgt uit:

| De naam van de BLOB-voorvoegsel | Type | Beschrijving |
|-----------|------|-------------|
| featureScaleModel | Parquet | Standard scaler-model voor numerieke functies. |
| stringIndexModel | Parquet | Indexeerfunctie model voor niet-numerieke functies de tekenreeks.|
| oneHotEncoderModel|Parquet | Een hot encoder-model voor categorische functies. |
| mlModel | Parquet | Getrainde machine learning-model. |
| informatie| Python pickle-bestand | Informatie over de getransformeerde gegevens, inclusief training start, einde van de training, duur, de tijdstempel voor trainen en testen splitsen en kolommen om te indexeren en een hot-codering.

Alle bestanden en blobs in de voorgaande tabel worden gebruikt voor uitoefening.


### <a name="model-development"></a>Model-ontwikkeling

#### <a name="architecture-diagram"></a>Architectuurdiagram


Het volgende diagram toont de end-to-end-werkstroom van het gebruik van Machine Learning Workbench het model te ontwikkelen: ![architectuur](media/scenario-big-data/architecture.PNG)

In de volgende secties laten we zien de model-ontwikkeling met behulp van de functionaliteit van externe compute-doel in Machine Learning Workbench. We eerst een kleine hoeveelheid voorbeeldgegevens te laden en voer het script [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) op een Ubuntu-DSVM voor snelle iteratie. We kunnen verder te beperken het werk doen we in [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) door door te geven van een extra argument voor snellere iteratie. In het einde gebruiken we een HDInsight-cluster te trainen met volledige gegevens.     

De [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) bestand wordt geladen en de gegevens voorbereid en feature-engineering uitvoert. Deze twee argumenten geaccepteerd:
* Een configuratiebestand voor de Blob storage-container voor het opslaan van de tussenliggende compute-resultaten en modellen.
* Een configuratie-argument voor foutopsporing voor snellere iteratie.

Het eerste argument `configFilename`, is een lokale configuratiebestand waar u de Blob storage-gegevens opslaan en geef op waar de gegevens te laden. Dit is standaard [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/storageconfig.json), en een RDP-moet worden gebruikt in de gegevens één maand is uitgevoerd. We ook [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json), die u wilt gebruiken op de volledige gegevensset uitgevoerd. De inhoud in de configuratie is als volgt: 

| Veld | Type | Beschrijving |
|-----------|------|-------------|
| storageAccount | Reeks | Azure Storage-accountnaam |
| storageContainer | Reeks | Container in Azure Storage-account voor het opslaan van tussenliggende resultaten genereren |
| storageKey | Reeks |Azure toegangssleutel voor Opslagaccount |
| Gegevensbestand|Reeks | De bronbestanden van gegevens  |
| duur| Reeks | Duur van de gegevens in de bronbestanden van de gegevens|

Wijzigen van beide `Config/storageconfig.json` en `Config/fulldata_storageconfig.json` om de storage-account, opslagsleutel en de blob-container voor het opslaan van de tussenliggende resultaten te configureren. De blob-container voor de gegevens die één maand uitvoeren is standaard `onemonthmodel`, en wordt de blob-container voor de volledige gegevensset uitgevoerd `fullmodel`. Zorg ervoor dat u deze twee containers maken in uw storage-account. De `dataFile` veld [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) configureert welke gegevens worden geladen [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py). De `duration` veld configureert het bereik voor de gegevens bevatten. Als de duur is ingesteld op ONE_MONTH, moet de geladen gegevens slechts één CSV-bestand onder de zeven bestanden van de gegevens voor juni 2016. Als de duur van de volledige is, wordt de volledige gegevensset (1 TB) is geladen. U hoeft niet te wijzigen `dataFile` en `duration` in deze twee configuratiebestanden.

Het tweede argument wordt FOUTOPSPORING. Instellen op FILTER_IP, kunt een snellere iteratie. Gebruik van deze parameter is nuttig wanneer u fouten wilt opsporen van uw script.

> [!NOTE]
> In alle van de volgende opdrachten, kunt u een variabele argument vervangen door de werkelijke waarde.
> 


#### <a name="model-development-on-the-docker-of-ubuntu-dsvm"></a>Model-ontwikkeling op de Docker van Ubuntu DSVM

#####  <a name="1-set-up-the-compute-target"></a>1. Instellen van de compute-doel

Selecteer eerst de opdrachtregel van Machine Learning Workbench **bestand** > **Open Command Prompt**. Voer vervolgens 

```az ml computetarget attach remotedocker --name dockerdsvm --address $DSVMIPaddress  --username $user --password $password ```

De volgende twee bestanden worden gemaakt in de map aml_config van uw project:

-  dockerdsvm.COMPUTE: dit bestand bevat de verbinding en configuratie-informatie voor een doel voor uitvoering op afstand.
-  dockerdsvm.runconfig: dit bestand is een set met uitvoeren opties gebruikt in de Workbench-toepassing.

Blader naar dockerdsvm.runconfig en wijzig de configuratie van deze velden in de volgende:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/dsvm_spark_dependencies.yml

De projectomgeving voorbereiden door uit te voeren:

```az ml experiment prepare -c dockerdsvm```


Met `PrepareEnvironment` ingesteld op true, Machine Learning Workbench de runtime-omgeving wordt gemaakt wanneer u een taak verzenden. `Config/conda_dependencies.yml` en `Config/dsvm_spark_dependencies.yml` bevatten de aanpassing van de runtime-omgeving. U kunt altijd de Conda-afhankelijkheden, de configuratie van een Spark en Spark-afhankelijkheden wijzigen door deze twee YMAL-bestanden te bewerken. In dit voorbeeld hebben we toegevoegd `azure-storage` en `azure-ml-api-sdk` als extra Python-pakketten in `Config/conda_dependencies.yml`. We hebben ook toegevoegd `spark.default.parallelism`, `spark.executor.instances`, en `spark.executor.cores` in `Config/dsvm_spark_dependencies.yml`. 

#####  <a name="2-data-preparation-and-feature-engineering-on-dsvm-docker"></a>2. Gegevens voor te bereiden en feature-engineering op DSVM Docker

Voer het script `etl.py` op DSVM Docker. Gebruik een debug-parameter waarmee de geladen gegevens met specifieke IP-adressen worden gefilterd:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FILTER_IP```

Blader naar het paneel aan clientzijde, en selecteer **uitvoeren** om te zien van de uitvoeringsgeschiedenis van `etl.py`. U ziet dat de uitvoeringstijd ongeveer twee minuten is. Als u van plan bent om uw code om op te nemen van nieuwe functies te wijzigen, biedt bieden FILTER_IP als het tweede argument een snellere iteratie. Mogelijk moet u deze stap meerdere keren worden uitgevoerd wanneer er sprake is van uw eigen machine learning van problemen voor het verkennen van de gegevensset of het maken van nieuwe functies. 

Met de aangepaste beperking op welke gegevens moeten worden geladen en verder filteren welke gegevens moeten worden verwerkt, kunt u het iteratie-proces versnellen in de ontwikkeling van uw model. Als u het experiment is, moet u regelmatig de wijzigingen opslaan in uw code wordt de Git-opslagplaats. Houd er rekening mee dat we hebben de volgende code in gebruikt `etl.py` voor de toegang tot de privé-container:

```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)

# attach the blob storage to the spark cluster or VM so that the storage can be accessed by the cluster or VM        
attach_storage_container(spark, storageAccount, storageKey)
```


Voer vervolgens het script `etl.py` op DSVM Docker zonder de parameter foutopsporing FILTER_IP:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FALSE```

Blader naar het paneel aan clientzijde, en selecteer **uitvoeren** om te zien van de uitvoeringsgeschiedenis van `etl.py`. U ziet dat de uitvoeringstijd ongeveer vier minuten is. De verwerkte resultaat van deze stap wordt opgeslagen in de container en wordt geladen voor training in train.py. Bovendien worden de tekenreeks indexeerfuncties, de encoder-pijplijnen en standard scalers opgeslagen in de privé-container. Deze worden gebruikt in uitoefening. 


##### <a name="3-model-training-on-dsvm-docker"></a>3. Modeltraining op DSVM Docker

Voer het script `train.py` op DSVM Docker:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/train.py ./Config/storageconfig.json```

Deze stap wordt de tussenliggende compute-resultaten van het uitvoeren van de geladen `etl.py`, en traint u een machine learning-model. Deze stap duurt ongeveer twee minuten.

Wanneer u klaar bent met succes de experimenten op het kleine gegevens, kunt u doorgaan met de experimenten van de volledige gegevensset. U kunt starten met behulp van dezelfde code, en vervolgens experimenteren met het argument en compute-doel wijzigingen.  

####  <a name="model-development-on-the-hdinsight-cluster"></a>Model-ontwikkeling op het HDInsight-cluster

##### <a name="1-create-the-compute-target-in-machine-learning-workbench-for-the-hdinsight-cluster"></a>1. De compute-doel in Machine Learning Workbench voor het HDInsight-cluster maken

```az ml computetarget attach cluster --name myhdi --address $clustername-ssh.azurehdinsight.net --username $username --password $password```

De volgende twee bestanden worden gemaakt in de map aml_config:
    
-  myhdi.COMPUTE: dit bestand bevat informatie over verbinding en configuratie voor een doel voor uitvoering op afstand.
-  myhdi.runconfig: dit bestand is een set uitvoeren opties gebruikt in de Workbench-toepassing.


Blader naar myhdi.runconfig en wijzig de configuratie van deze velden in de volgende:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/hdi_spark_dependencies.yml

De projectomgeving voorbereiden door uit te voeren:

```az ml experiment prepare -c myhdi```

Deze stap kan maximaal zeven minuten duren.

##### <a name="2-data-preparation-and-feature-engineering-on-hdinsight-cluster"></a>2. Gegevens voor te bereiden en feature-engineering op HDInsight-cluster

Voer het script `etl.py`, met de volledige gegevens op het HDInsight-cluster:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/etl.py Config/fulldata_storageconfig.json FALSE```

Omdat deze taak duurt voor een relatief lange tijd (ongeveer twee uur), kunt u `-a` om uit te schakelen streaming-uitvoer. Wanneer de taak is voltooid, in **Run History**, vindt u logboeken van de stuurprogramma- en -controller. Als u een groter cluster hebt, kunt u altijd opnieuw configureren de configuraties in `Config/hdi_spark_dependencies.yml` meer exemplaren of cores te gebruiken. Als u een cluster met vier worker-knooppunten hebt, kunt u bijvoorbeeld de waarde van verhogen `spark.executor.instances` van 5 tot en met 7. U ziet de uitvoer van deze stap in de **fullmodel** container in uw opslagaccount. 


##### <a name="3-model-training-on-hdinsight-cluster"></a>3. Modeltraining op HDInsight-cluster

Voer het script `train.py` op HDInsight-cluster:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/train.py Config/fulldata_storageconfig.json```

Omdat deze taak duurt voor een relatief lange tijd (ongeveer 30 minuten), kunt u `-a` om uit te schakelen streaming-uitvoer.

#### <a name="run-history-exploration"></a>Verkenning van de geschiedenis uitvoeren

Uitvoeringsgeschiedenis is een functie waarmee u gegevens van uw experimenten in Machine Learning Workbench. Standaard worden de duur van de experimenten bijgehouden. In dit specifieke voorbeeld als we naar de volledige gegevensset naar `Code/etl.py` in de experimenten, we ziet u dat duur aanzienlijk toeneemt. U kunt ook specifieke metrische gegevens voor traceringsdoeleinden vastleggen. Voeg de volgende regels code naar de kop van de Python-bestand zodat metrische gegevens bijhouden:
```python
# import logger
from azureml.logging import get_azureml_logger

# initialize logger
run_logger = get_azureml_logger()
```
Hier volgt een voorbeeld voor het bijhouden van specifieke metrische gegevens:

```python
run_logger.log("Test Accuracy", testAccuracy)
```

Blader op de juiste zijbalk van de Workbench naar **uitvoeringen** om te zien van de uitvoeringsgeschiedenis voor elke Python-bestand. U kunt ook gaan naar uw GitHub-opslagplaats. Een nieuwe vertakking wordt met de naam die begint met 'AMLHistory,' gemaakt voor het volgen van de wijziging in het script in elke uitvoering. 


### <a name="operationalize-the-model"></a>Het model operationeel maken

In deze sectie maakt uitvoeren u het model die u in de vorige stappen hebt gemaakt als een webservice. U leert ook hoe u gebruik van de webservice om te voorspellen werkbelasting. Gebruik machinetaal uitoefening opdrachtregelinterfaces (CLI) pakket van de code en afhankelijkheden als Docker-installatiekopieën, en publiceer het model als een webservice in containers.

U kunt de opdrachtregelprompt in Machine Learning Workbench gebruiken om uit te voeren van de CLI's gebruikt.  U kunt ook de CLI op Ubuntu Linux uitvoeren door de [installatiehandleiding](./deployment-setup-configuration.md#using-the-cli). 

> [!NOTE]
> In de volgende opdrachten, kunt u een variabele argument vervangen door de werkelijke waarde. Het duurt ongeveer 40 minuten in deze sectie voltooid.
> 

Kies een unieke tekenreeks als de omgeving voor uitoefening. Hier gebruiken we de tekenreeks '[unieke]' om weer te geven van de tekenreeks die u kiest.

1. De omgeving voor uitoefening maken en de resourcegroep maken.

        az ml env setup -c -n [unique] --location eastus2 --cluster -z 5 --yes

   Houd er rekening mee dat u Container Service gebruiken als de omgeving met behulp van kunt `--cluster` in de `az ml env setup` opdracht. U kunt het machine learning-model uitvoeren op [Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-intro-kubernetes). Hierbij [Kubernetes](https://kubernetes.io/) voor het automatiseren van implementatie, schaling en beheer van toepassingen in containers. Met deze opdracht duurt ongeveer 20 minuten om uit te voeren. Gebruik de volgende om te controleren als de implementatie is voltooid: 

        az ml env show -g [unique]rg -n [unique]

   De implementatieomgeving instellen als het account dat u zojuist hebt gemaakt door het uitvoeren van de volgende:

        az ml env set -g [unique]rg -n [unique]

2. Maak en gebruik een Modelbeheer-account. Voor het maken van een Modelbeheer-account, voert u het volgende uit:

        az ml account modelmanagement create --location  eastus2 -n [unique]acc -g [unique]rg --sku-instances 4 --sku-name S3 

   Gebruik de Modelbeheer voor uitoefening door het uitvoeren van de volgende:

        az ml account modelmanagement set  -n [unique]acc -g [unique]rg  

   Een account voor Modelbeheer dat wordt gebruikt voor het beheren van modellen en webservices. In de Azure-portal ziet u dat een nieuwe Modelbeheer-account is gemaakt. U ziet de modellen, manifesten, Docker-installatiekopieën en services die zijn gemaakt met behulp van deze Modelbeheer-account.

3. Downloaden en registreer de modellen.

   Download de modellen in de **fullmodel** container naar uw lokale computer in de directory van code. De parquet-gegevensbestand met de naam 'vmlSource.parquet.' niet downloaden Het is niet een modelbestand; het is een tussenliggende compute-resultaat. U kunt ook de modelbestanden dat is opgenomen in de Git-opslagplaats opnieuw te gebruiken. Zie voor meer informatie, [GitHub](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Docs/DownloadModelsFromBlob.md). 

   Ga naar de `Model` map in de CLI en registreer de modellen als volgt:

        az ml model register -m  mlModel -n vmlModel -t fullmodel
        az ml model register -m  featureScaleModel -n featureScaleModel -t fullmodel
        az ml model register -m  oneHotEncoderModel -n  oneHotEncoderModel -t fullmodel
        az ml model register -m  stringIndexModel -n stringIndexModel -t fullmodel
        az ml model register -m  info -n info -t fullmodel

   De uitvoer van elke opdracht biedt een model-ID, die nodig in de volgende stap is. Opslaan in een tekstbestand voor toekomstig gebruik.

4. Maak een manifest voor de webservice.

   Een manifest bevat de code voor de webservice, het machine learning-modellen en afhankelijkheden voor runtime-omgeving. Ga naar de `Code` map in de CLI, en voer de volgende opdracht uit:

        az ml manifest create -n $webserviceName -f webservice.py -r spark-py -c ../Config/conda_dependencies_webservice.yml -i $modelID1 -i $modelID2 -i $modelID3 -i $modelID4 -i $modelID5

   De uitvoer geeft een manifest-ID voor de volgende stap. 

   Blijf de `Code` en u kunt webservice.py kunt testen door het uitvoeren van de volgende: 

        az ml experiment submit -t dockerdsvm -c dockerdsvm webservice.py

5. Een Docker-installatiekopie maken. 

        az ml image create -n [unique]image --manifest-id $manifestID

   De uitvoer biedt een afbeeldings-ID voor de volgende stap, deze docker-installatiekopie wordt gebruikt in Container Service. 

6. De webservice implementeert in de Container Service-cluster.

        az ml service create realtime -n [unique] --image-id $imageID --cpu 0.5 --memory 2G

   De uitvoer geeft een service-ID. U moet deze gebruiken om de autorisatiesleutel ophalen en service-URL.

7. Roep de webservice in Python-code om te beoordelen in mini-batches.

   Gebruik de volgende opdracht om op te halen van de autorisatiesleutel:

         az ml service keys realtime -i $ServiceID 

   Gebruik de volgende opdracht om op te halen van de service scoring-URL:

        az ml service usage realtime -i $ServiceID

   Wijzigen van de inhoud in `./Config/webservice.json` met de juiste service scoring-URL en autorisatie-sleutel. Houd de 'Bearer' in het oorspronkelijke bestand en vervang het gedeelte 'xxx'. 
   
   Ga naar de hoofdmap van uw project en testen van de webservice voor Mini batchscore met behulp van de volgende:

        az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/scoring_webservice.py ./Config/webservice.json

8. De webservice worden geschaald. 

   Zie voor meer informatie, [uitoefening op uw Azure Container Service-cluster schalen](how-to-scale-clusters.md).
 

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld wordt uitgelegd hoe u Machine Learning Workbench gebruiken met het trainen van een machine learning-model van big data en operationeel maken van het getrainde model. In het bijzonder, hebt u geleerd hoe u wilt configureren en gebruiken verschillende compute-doelen, en de uitvoeringsgeschiedenis van metrische gegevens en andere wordt uitgevoerd.

U kunt de code voor het verkennen van kruisvalidatie en hyper-parameter afstemmen uitbreiden. Zie voor meer informatie over het afstemmen van kruisvalidatie en hyper-parameter, [deze GitHub-resource](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning).  

Zie voor meer informatie over het maken van time series-prognoses, [deze GitHub-resource](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting).
