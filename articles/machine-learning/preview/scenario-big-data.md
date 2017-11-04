---
title: Server-werkbelasting prognose op terabytes aan gegevens - Azure | Microsoft Docs
description: Klik hier voor meer informatie over het trainen van een machine learning-model van big data met behulp van Azure Machine Learning-Workbench.
services: machine-learning
documentationcenter: 
author: daden
manager: mithal
editor: daden
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: daden
ms.openlocfilehash: b962ad3da6d5daff2c8b2524828a9450da702abb
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="server-workload-forecasting-on-terabytes-of-data"></a>Prognose voor serverworkload per terabytes aan gegevens

In dit artikel bevat informatie over hoe gegevenswetenschappers Azure Machine Learning Workbench kunnen gebruiken voor het ontwikkelen van oplossingen waarvoor het gebruik van big data. U kunt starten vanuit een steekproef van een grote gegevensset, doorlopen voorbereiden van gegevens, functie-engineering en machine learning en vervolgens het proces voor de volledige gegevensset grote uitbreiden. 

U leert over de volgende belangrijke mogelijkheden van Machine Learning-Workbench:
* Eenvoudig schakelen tussen compute doelen. U kunt verschillende rekenscenario doelen instellen en deze gebruiken in experimenteren. In dit voorbeeld gebruikt u een Ubuntu DSVM en een Azure HDInsight-cluster als de compute-doelen. U kunt ook de compute-doelen, afhankelijk van de beschikbaarheid van resources configureren. In het bijzonder na het uitbreiden van het Spark-cluster met meer worker-knooppunten, kunt u de resources via Machine Learning-Workbench om te versnellen experiment wordt uitgevoerd.
* Bijhouden van de geschiedenis uitvoeren. Machine Learning-Workbench kunt u de prestaties van machine learning-modellen en andere metrische gegevens van belang bijhouden.
* Uitoefening van de machine learning-model. U kunt de ingebouwde hulpprogramma's in Machine Learning-Workbench gebruiken voor het implementeren van het getrainde machine learning-model als een webservice op Azure Container Service. U kunt ook de web-service gebruiken om op te halen Mini batch voorspellingen via REST API-aanroepen. 
* Ondersteuning voor terabyte gegevens.

> [!NOTE]
> Zie voor voorbeelden van code en andere materialen die betrekking hebben op dit voorbeeld [GitHub](https://github.com/Azure/MachineLearningSamples-BigData).
> 

## <a name="use-case-overview"></a>Gebruik van de case-overzicht

De werkbelasting op servers prognose is een algemene zakelijke behoeften voor technologiebedrijven die hun eigen infrastructuur beheren. Als u wilt infrastructuurkosten, moeten services die worden uitgevoerd op servers onder gebruikt om te worden uitgevoerd op een kleiner aantal machines zijn gegroepeerd. Services die worden uitgevoerd op overbelaste servers moeten worden genomen meer machines dat wordt uitgevoerd. 

In dit scenario u zich richten op werkbelasting voorspelling voor elke computer (of server). In het bijzonder, kunt u de sessiegegevens op elke server in de toekomst voorspellen van de werkbelastingsklasse van de server. Classificeren van de belasting van elke server in de laag, gemiddeld en hoge klassen met behulp van de willekeurige Forest classificatie in [Apache Spark ML](https://spark.apache.org/docs/2.1.1/ml-guide.html). De machine learning-technieken en werkstroom in dit voorbeeld kan gemakkelijk worden uitgebreid naar andere vergelijkbare problemen. 


## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn als volgt:

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
* Een geïnstalleerde kopie van [Machine Learning Workbench](./overview-what-is-azure-ml.md). Om het programma te installeren en een werkruimte maken, Zie de [Quick Start-installatiehandleiding](./quickstart-installation.md).
* Windows 10 (de instructies in dit voorbeeld zijn in het algemeen hetzelfde voor Mac OS-systemen).
* Een Data wetenschappelijke virtuele Machine (DSVM) voor Linux (Ubuntu). U kunt een Ubuntu DSVM inrichten door [deze instructies](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm). U ziet ook [deze snelstartgids](https://ms.portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu). U wordt aangeraden een virtuele machine gebruiken met ten minste 8 kernen en 32 GB geheugen. U moet de DSVM IP-adres, de gebruikersnaam en het wachtwoord voor het uitproberen van dit voorbeeld. Opslaan in de volgende tabel met de DSVM-gegevens voor de volgende stappen:

 Veldnaam| Waarde |  
 |------------|------|
DSVM IP-adres | xxx|
 Gebruikersnaam  | xxx|
 Wachtwoord   | xxx|

 U kunt gebruiken met een virtuele machine [Docker-Engine](https://docs.docker.com/engine/) geïnstalleerd.

* Een HDInsight Spark-Cluster met Hortonworks Data Platform 3.6 en Spark versie 2.1.x. Ga naar [een Apache Spark-cluster maken in Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql) voor meer informatie over het maken van HDInsight-clusters. Het is raadzaam om gebruik van een cluster drie worker met elke werknemer 16 kernen en 112 GB aan geheugen hebben. Of u kunt alleen VM type `D12 V2` voor hoofdknooppunt, en `D14 V2` voor het werkrolknooppunt. De implementatie van het cluster duurt ongeveer 20 minuten. U moet de clusternaam, het SSH-gebruikersnaam en het wachtwoord voor het uitproberen van dit voorbeeld. Opslaan in de volgende tabel met de gegevens van de Azure HDInsight-cluster voor de volgende stappen:

 Veldnaam| Waarde |  
 |------------|------|
 Clusternaam| xxx|
 Gebruikersnaam  | xxx (sshuser standaard)|
 Wachtwoord   | xxx|


* Een Azure Storage-account. U kunt volgen [deze instructies](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) een maken. Maak ook twee particuliere blob-containers met de namen `fullmodel` en `onemonthmodel` in dit opslagaccount. Het opslagaccount wordt gebruikt voor het opslaan van resultaten van de tussenliggende compute en machine learning-modellen. U moet de naam en opslagaccountsleutel uitproberen in dit voorbeeld. Opslaan in de volgende tabel met de gegevens van de Azure-opslag voor de volgende stappen:

 Veldnaam| Waarde |  
 |------------|------|
 Naam van het opslagaccount| xxx|
 Toegangstoets  | xxx|


De Ubuntu DSVM en het Azure HDInsight-cluster gemaakt in de lijst met vereiste zijn compute-doelen. COMPUTE doelen worden de rekenresources in de context van Machine Learning Workbench, die afwijken van de computer waarop de Workbench wordt uitgevoerd.   

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project met behulp van dit voorbeeld als sjabloon:
1.  Open de Machine Learning-Workbench.
2.  Op de **projecten** pagina de  **+**  ondertekenen en selecteer **nieuw Project**.
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project.
4.  In de **zoeken projectsjablonen** zoekvak, type **werkbelasting prognose van Terabytes gegevens**, en selecteer de sjabloon.
5.  Selecteer **Maken**.

U kunt een Workbench-project maken met een vooraf gemaakte git-opslagplaats door [deze instructie](./tutorial-classifying-iris-part-1.md).  
Voer `git status` om te controleren van de status van de bestanden voor versie bijhouden.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevens die worden gebruikt in dit voorbeeld is server gesyntheseerde Werkbelastinggegevens. Deze wordt gehost in een Azure Blob storage-account dat openbaar toegankelijk is. De accountgegevens voor specifieke opslag kunt u vinden in de `dataFile` veld [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json). U kunt de gegevens rechtstreeks vanuit de Blob-opslag gebruiken. Als de opslag wordt gebruikt door veel gebruikers tegelijk, kunt u [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) voor het downloaden van de gegevens in de eigen opslag. 

De totale gegevensgrootte is ongeveer 1 TB. Elk bestand is ongeveer 1-3 GB en CSV-bestandsindeling, zonder koptekst. Elke rij gegevens vertegenwoordigt de belasting van een transactie op een bepaalde server. De gedetailleerde informatie van het schema van de is als volgt:

Kolomnummer | Veldnaam| Type | Beschrijving |  
|------------|------|-------------|---------------|
1  | `SessionStart` | Datum/tijd |    Begintijd sessie
2  |`SessionEnd`    | Datum/tijd | Eindtijd van sessie
3 |`ConcurrentConnectionCounts` | Geheel getal | Aantal gelijktijdige verbindingen
4 | `MbytesTransferred` | dubbele | Genormaliseerde gegevens overgebracht in MB
5 | `ServiceGrade` | Geheel getal |  Klasse van de service voor de sessie
6 | `HTTP1` | Geheel getal|  Sessie gebruikt HTTP1 of HTTP2
7 |`ServerType` | Geheel getal   |Servertype
8 |`SubService_1_Load` | dubbele |   Subservice 1 laden
9 | `SubService_1_Load` | dubbele |  Subservice 2 laden
10 | `SubService_1_Load` | dubbele |     Subservice 3 laden
11 |`SubService_1_Load` | dubbele |  Subservice 4 laden
12 | `SubService_1_Load`| dubbele |      Subservice 5 laden
13 |`SecureBytes_Load`  | dubbele | Beveiligde bytes laden
14 |`TotalLoad` | dubbele | Totale belasting van server
15 |`ClientIP` | Tekenreeks|    IP-clientadres
16 |`ServerIP` | Tekenreeks|    Het IP-adres



Houd er rekening mee dat de typen van de verwachte gegevens worden weergegeven in de voorgaande tabel. Er is geen garantie dat de gegevenstypen daadwerkelijk zijn zoals verwacht als gevolg van ontbrekende waarden en problemen met onverwacht gegevens. Gegevensverwerking moet dit in acht nemen. 


## <a name="scenario-structure"></a>Scenario-structuur

De bestanden in dit voorbeeld zijn als volgt worden ingedeeld.

| Bestandsnaam | Type | Beschrijving |
|-----------|------|-------------|
| `Code` | Map | De map bevat de code in het voorbeeld. |
| `Config` | Map | De map bevat de configuratiebestanden. |
| `Image` | Map | De map die wordt gebruikt voor het opslaan van afbeeldingen voor het Leesmij-bestand. |
| `Model` | Map | De map die wordt gebruikt voor het opslaan van modelbestanden gedownload van Blob-opslag. |
| `Code/etl.py` | Python-bestand | Het Python-bestand gebruikt voor het voorbereiden van gegevens en functie-engineering. |
| `Code/train.py` | Python-bestand | Het Python-bestand gebruikt een drie-klasse multi-classfication-model te trainen.  |
| `Code/webservice.py` | Python-bestand | Het Python-bestand voor uitoefening gebruikt.  |
| `Code/scoring_webservice.py` | Python-bestand |  Het Python-bestand gebruikt voor gegevenstransformatie en het aanroepen van de webservice. |
| `Code/O16Npreprocessing.py` | Python-bestand | Het Python-bestand gebruikt voor het verwerken van de gegevens voor scoring_webservice.py.  |
| `Code/util.py` | Python-bestand | Het Python-bestand met code voor het lezen en schrijven van Azure blobs.  
| `Config/storageconfig.json` | JSON-bestand | Het configuratiebestand voor de Azure blob-container die de tussenliggende resultaten en het model voor verwerking en training van één maand gegevens opslaat. |
| `Config/fulldata_storageconfig.json` | JSON-bestand | Het configuratiebestand voor de Azure blob-container die de tussenliggende resultaten en het model voor verwerking en training op een volledige gegevensset opslaat.|
| `Config/webservice.json` | JSON-bestand | Het configuratiebestand voor scoring_webservice.py.|
| `Config/conda_dependencies.yml` | YAML-bestand | Het bestand dat Conda afhankelijkheid. |
| `Config/conda_dependencies_webservice.yml` | YAML-bestand | De afhankelijkheid Conda-bestand voor de webservice.|
| `Config/dsvm_spark_dependencies.yml` | YAML-bestand | Het bestand van de afhankelijkheid Spark voor Ubuntu DSVM. |
| `Config/hdi_spark_dependencies.yml` | YAML-bestand | Het bestand van de afhankelijkheid Spark voor HDInsight Spark-cluster. |
| `README.md` | Markdown-bestand | Het Leesmij-markdown-bestand. |
| `Code/download_model.py` | Python-bestand | Het Python-bestand dat is gebruikt voor het downloaden van de modelbestanden van de Azure blob naar een lokale schijf. |
| `Docs/DownloadModelsFromBlob.md` | Markdown-bestand | De markdown-bestand met instructies voor het uitvoeren van `Code/download_model.py`. |



### <a name="data-flow"></a>Gegevensstroom

De code in [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) gegevens worden geladen vanuit de container openbaar toegankelijk (`dataFile` veld [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json)). Het omvat het voorbereiden van gegevens en functie-engineering en slaat de resultaten van de tussenliggende compute en modellen op uw eigen privé-container. De code in [ `Code/train.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/train.py) laadt de tussenliggende compute-resultaten van de privé-container, het model meerdere klasse classificatie traint en schrijft het getrainde machine learning-model in de privé-container. 

U moet een container voor experimenteren op de gegevensset één maand en een andere naam voor de volledige gegevensset experimenten gebruiken. Omdat de gegevens en modellen worden opgeslagen als parketvloeren bestand, is elk bestand daadwerkelijk een map in de container, met meerdere blobs. De resulterende container ziet er als volgt uit:

| BLOB-voorvoegsel | Type | Beschrijving |
|-----------|------|-------------|
| featureScaleModel | Parketvloeren | Standaard scaler model voor numerieke onderdelen. |
| stringIndexModel | Parketvloeren | Model van de indexeerfunctie voor niet-numerieke onderdelen een tekenreeks.|
| oneHotEncoderModel|Parketvloeren | Een hot encoder model voor categorische functies. |
| mlModel | Parketvloeren | Getrainde machine learning-model. |
| Info| Python pickle-bestand | Informatie over de getransformeerde gegevens, inclusief training start, einde van de training, duur, het tijdstempel voor train-test splitsen en kolommen voor indexering en één hot codering.

Alle bestanden en blobs in de voorgaande tabel worden gebruikt voor uitoefening.


### <a name="model-development"></a>Model-ontwikkeling

#### <a name="architecture-diagram"></a>Architectuurdiagram


Het volgende diagram toont de end-to-end werkstroom van het gebruik van Machine Learning Workbench voor het ontwikkelen van het model: ![architectuur](media/scenario-big-data/architecture.PNG)

In de volgende secties laten we zien de model-ontwikkeling met behulp van de functionaliteit van externe compute doel in Machine Learning-Workbench. We eerst een kleine hoeveelheid voorbeeldgegevens laden en voer het script [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) op een Ubuntu DSVM voor snelle iteratie. We kunnen nog verder beperken we het werk in [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) door een extra argument voor snellere herhaling wordt doorgegeven. In het einde gebruiken we een HDInsight-cluster te trainen met volledige gegevens.     

De [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) bestand wordt geladen en bereidt de gegevens en functie-engineering uitvoert. Deze accepteert twee argumenten:
* Een configuratiebestand voor de Blob storage-container voor het opslaan van de tussenliggende compute-resultaten en modellen.
* Een argument van de configuratie foutopsporing voor snellere iteratie.

Het eerste argument `configFilename`, is een lokale configuratiebestand waar u de Blob storage-gegevens opslaan en geef aan waar de gegevens te laden. Dit is standaard [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/storageconfig.json), en deze zullen worden gebruikt in de één maand gegevens uitvoert. We ook [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json), die u wilt gebruiken op de volledige gegevensset uitgevoerd. De inhoud in de configuratie is als volgt: 

| Veld | Type | Beschrijving |
|-----------|------|-------------|
| StorageAccount | Tekenreeks | Naam van een Azure Storage-account |
| storageContainer | Tekenreeks | De container in Azure Storage-account voor het opslaan van tussenliggende resultaten |
| storageKey | Tekenreeks |Azure toegangssleutel voor Opslagaccount |
| DataFile|Tekenreeks | Gegevensbronbestanden  |
| Duur| Tekenreeks | duur van de gegevens in de bronbestanden van de gegevens|

Wijzigen van beide `Config/storageconfig.json` en `Config/fulldata_storageconfig.json` voor het configureren van het opslagaccount, opslagsleutel en de blob-container voor het opslaan van de tussenliggende resultaten. De blob-container voor de één maand gegevens uitvoert is standaard `onemonthmodel`, en de blob-container voor een volledige gegevensset uitgevoerd is `fullmodel`. Zorg ervoor dat u deze twee containers maken in uw opslagaccount. De `dataFile` veld [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) configureert welke gegevens worden geladen [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py). De `duration` veld configureert u het bereik dat de gegevens bevatten. Als de duur is ingesteld op ONE_MONTH, moet de geladen gegevens slechts één CSV-bestand tussen de zeven bestanden van de gegevens voor juni 2016. Als de duur van de volledige is, wordt de volledige gegevensset (1 TB) is geladen. U hoeft niet te wijzigen `dataFile` en `duration` in deze configuratie met twee bestanden.

Het tweede argument is FOUTOPSPORING. Instellen op FILTER_IP, kunt een snellere iteratie. Gebruik van deze parameter is handig als u fouten opsporen in uw script wilt maken.

> [!NOTE]
> In alle van de volgende opdrachten, kunt u een variabele argument vervangen door de werkelijke waarde.
> 


#### <a name="model-development-on-the-docker-of-ubuntu-dsvm"></a>Model ontwikkeling op het Docker Ubuntu DSVM

#####  <a name="1-set-up-the-compute-target"></a>1. Instellen van de compute-doel

Starten van de opdrachtregel van Machine Learning-Workbench door te selecteren **bestand** > **opdrachtprompt openen**. Voer vervolgens: 

```az ml computetarget attach --name dockerdsvm --address $DSVMIPaddress  --username $user --password $password --type remotedocker```

De volgende twee bestanden worden gemaakt in de map aml_config van uw project:

-  dockerdsvm.COMPUTE: dit bestand bevat de verbinding en -configuratie-informatie voor een doel voor uitvoering op afstand.
-  dockerdsvm.runconfig: dit bestand is een verzameling run opties gebruikt in de Workbench-toepassing.

Blader naar dockerdsvm.runconfig en wijzig de configuratie van deze velden in het volgende:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/dsvm_spark_dependencies.yml

De project-omgeving voorbereiden door te voeren:

```az ml experiment prepare -c dockerdsvm```


Met `PrepareEnvironment` ingesteld op true, Machine Learning Workbench de runtime-omgeving wordt gemaakt wanneer het verzenden van een taak. `Config/conda_dependencies.yml`en `Config/dsvm_spark_dependencies.yml` bevatten de aanpassing van de runtime-omgeving. U kunt de Conda afhankelijkheden, Spark-configuratie en Spark afhankelijkheden altijd aanpassen door deze twee YMAL-bestanden te bewerken. In dit voorbeeld hebben we toegevoegd `azure-storage` en `azure-ml-api-sdk` als extra Python-pakketten in `Config/conda_dependencies.yml`. We hebben ook toegevoegd `spark.default.parallelism`, `spark.executor.instances`, en `spark.executor.cores` in `Config/dsvm_spark_dependencies.yml`. 

#####  <a name="2-data-preparation-and-feature-engineering-on-dsvm-docker"></a>2. Voorbereiden van gegevens en functie-engineering op DSVM Docker

Voer het script `etl.py` op DSVM Docker. Gebruik een debug-parameter die de geladen gegevens met specifieke server IP-adressen worden gefilterd:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FILTER_IP```

Blader naar het paneel aan clientzijde en selecteer **uitvoeren** om te zien van de uitvoeringsgeschiedenis van `etl.py`. U ziet dat de uitvoeringstijd ongeveer twee minuten is. Als u van plan bent om uw code nieuwe functies te wijzigen, biedt bieden FILTER_IP als het tweede argument een snellere iteratie. Mogelijk moet u deze stap meerdere keren worden uitgevoerd wanneer het omgaan met uw eigen machine learning-problemen, om te verkennen van de gegevensset of maken van nieuwe functies. 

Met de aangepaste beperking op welke gegevens moeten worden geladen en verdere filterfunctie van welke gegevens worden verwerkt, kunt u het proces herhaling versnellen bij de ontwikkeling van uw model. Als u experimenteren, moet u regelmatig de wijzigingen opslaan in uw code aan de Git-opslagplaats. Houd er rekening mee dat we de volgende code in gebruikt `etl.py` voor de toegang tot de privé-container:

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

Blader naar het paneel aan clientzijde en selecteer **uitvoeren** om te zien van de uitvoeringsgeschiedenis van `etl.py`. U ziet dat de uitvoeringstijd ongeveer vier minuten is. De verwerkte resultaat van deze stap wordt opgeslagen in de container en is geladen voor training in train.py. Bovendien worden de tekenreeks indexeerfuncties, encoder pijplijnen en standaard scalers opgeslagen in de privé-container. Deze worden gebruikt in uitoefening. 


##### <a name="3-model-training-on-dsvm-docker"></a>3. Model training op DSVM Docker

Voer het script `train.py` op DSVM Docker:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/train.py ./Config/storageconfig.json```

Deze stap wordt de tussenliggende compute-resultaten van het uitvoeren van geladen `etl.py`, en traint u een machine learning-model. Deze stap duurt ongeveer twee minuten.

Wanneer u klaar bent met succes de experimenteren op de kleine hoeveelheden gegevens, kunt u blijven de experimenteren op de volledige gegevensset uitgevoerd. U kunt beginnen met de dezelfde code en vervolgens experimenteren met het argument en compute doel wijzigingen.  

####  <a name="model-development-on-the-hdinsight-cluster"></a>Model ontwikkeling op het HDInsight-cluster

##### <a name="1-create-the-compute-target-in-machine-learning-workbench-for-the-hdinsight-cluster"></a>1. De compute-doel in Machine Learning Workbench voor het HDInsight-cluster maken

```az ml computetarget attach --name myhdi --address $clustername-ssh.azurehdinsight.net --username $username --password $password --type cluster```

De volgende twee bestanden worden gemaakt in de map aml_config:
    
-  myhdo.COMPUTE: dit bestand bevat informatie over verbinding en configuratie voor een doel voor uitvoering op afstand.
-  myhdi.runconfig: dit bestand is een set uitvoeren opties die worden gebruikt in de Workbench-toepassing.


Blader naar myhdi.runconfig en wijzig de configuratie van deze velden in het volgende:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/hdi_spark_dependencies.yml

De project-omgeving voorbereiden door te voeren:

```az ml experiment prepare -c myhdi```

Deze stap kan maximaal zeven minuten duren.

##### <a name="2-data-preparation-and-feature-engineering-on-hdinsight-cluster"></a>2. Voorbereiden van gegevens en functie-engineering op HDInsight-cluster

Voer het script `etl.py`, met volledige gegevens op het HDInsight-cluster:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/etl.py Config/fulldata_storageconfig.json FALSE```

Omdat deze taak duurt relatief lange tijd (ongeveer twee uur), kunt u `-a` uitvoer streaming uitschakelen. Wanneer de taak is voltooid, in **geschiedenis uitvoeren**, vindt u de logboeken stuurprogramma en de domeincontroller. Als u een groter cluster hebt, kunt u altijd de configuraties in configureren `Config/hdi_spark_dependencies.yml` meer exemplaren of -kernen te gebruiken. Als u een cluster met vier worker-knooppunten hebt, kunt u bijvoorbeeld de waarde van verhogen `spark.executor.instances` van 5 tot en met 7. U ziet de uitvoer van deze stap in de **fullmodel** container in uw opslagaccount. 


##### <a name="3-model-training-on-hdinsight-cluster"></a>3. Model training op HDInsight-cluster

Voer het script `train.py` op HDInsight-cluster:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/train.py Config/fulldata_storageconfig.json```

Omdat deze taak duurt relatief lange tijd (ongeveer 30 minuten), kunt u `-a` uitvoer streaming uitschakelen.

#### <a name="run-history-exploration"></a>Verkenning van de geschiedenis uitvoeren

Uitvoeringsgeschiedenis is een functie tracering van uw experimenten in Machine Learning-Workbench. Standaard worden de duur van de experimenteren bijgehouden. In dit specifieke voorbeeld wanneer we naar de volledige gegevensset voor `Code/etl.py` in de experimenteren we merken duur aanzienlijk meer. U kunt ook specifieke metrische gegevens voor traceringsdoeleinden vastleggen. Voeg de volgende regels code naar de kop van Python-bestand zodat de metrische gegevens bijhouden:
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

Blader op de juiste zijbalk van de Workbench naar **wordt uitgevoerd** om te zien van de uitvoeringsgeschiedenis voor elk Python-bestand. U kunt ook gaan naar uw GitHub-opslagplaats. Een nieuwe vertakking is met de naam beginnen met 'AMLHistory', gemaakt voor het bijhouden van de wijziging in het script in elke uitvoering. 


### <a name="operationalize-the-model"></a>Het model operationeel maken

In deze sectie maakt operationeel u het model dat u in de vorige stappen hebt gemaakt als een webservice. U leert ook hoe u met de webservice werkbelasting voorspellen. Gebruik Machine Language uitoefening opdrachtregelinterfaces (CLIs) aan het pakket van de code en de afhankelijkheden als Docker-installatiekopieën en voor het publiceren van het model als een beperkte webservice. Zie voor meer informatie [dit overzicht](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/operationalization-overview.md).

U kunt de opdrachtprompt in Machine Learning-Workbench om uit te voeren van de CLIs gebruiken.  U kunt ook de CLIs op Ubuntu Linux uitvoeren door de [installatiehandleiding](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/install-on-ubuntu-linux.md). 

> [!NOTE]
> In de volgende opdrachten, kunt u een variabele argument vervangen door de werkelijke waarde. Het duurt ongeveer 40 minuten voor het voltooien van deze sectie.
> 

Kies een unieke tekenreeks als de omgeving voor uitoefening. We gebruiken hier de tekenreeks "[unique]" vertegenwoordigt de tekenreeks die u kiest.

1. De omgeving voor uitoefening maken en de resourcegroep maken.

        az ml env setup -c -n [unique] --location eastus2 --cluster -z 5 --yes

   Opmerking u Container Service kunt gebruiken als de omgeving met behulp van `--cluster` in de `az ml env setup` opdracht. U kunt het machine learning-model op operationeel [Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-intro-kubernetes). Hierbij [Kubernetes](https://kubernetes.io/) voor het automatiseren van de implementatie, schaalbaarheid en beheer van beperkte toepassingen. Met deze opdracht duurt ongeveer 20 minuten om uit te voeren. Gebruik de volgende om te controleren als de implementatie is voltooid: 

        az ml env show -g [unique]rg -n [unique]

   De implementatieomgeving instellen als het account dat u zojuist hebt gemaakt door het uitvoeren van het volgende:

        az ml env set -g [unique]rg -n [unique]

2. Maak en gebruik van een model-management-account. Een model management om account te maken, voert u de volgende:

        az ml account modelmanagement create --location  eastus2 -n [unique]acc -g [unique]rg --sku-instances 4 --sku-name S3 

   Gebruik de model-management voor uitoefening door het uitvoeren van het volgende:

        az ml account modelmanagement set  -n [unique]acc -g [unique]rg  

   Een model-management-account wordt gebruikt voor het beheren van modellen en webservices. Vanuit de Azure-portal ziet u dat een nieuw model-management-account is gemaakt. U ziet de modellen, manifesten, Docker-installatiekopieën en services die zijn gemaakt met behulp van dit model-management-account.

3. Download en registreer de modellen.

   Download de modellen in de **fullmodel** container op uw lokale computer in de map van code. Het gegevensbestand parketvloeren met de naam 'vmlSource.parquet.' niet downloaden Het is niet een modelbestand; het is een tussenliggende compute-resultaat. U kunt ook de modelbestanden dat is opgenomen in de Git-opslagplaats hergebruiken. Zie voor meer informatie [GitHub](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Docs/DownloadModelsFromBlob.md). 

   Ga naar de `Model` map in de CLI en registreer de modellen als volgt:

        az ml model register -m  mlModel -n vmlModel -t fullmodel
        az ml model register -m  featureScaleModel -n featureScaleModel -t fullmodel
        az ml model register -m  oneHotEncoderModel -n  oneHotEncoderModel -t fullmodel
        az ml model register -m  stringIndexModel -n stringIndexModel -t fullmodel
        az ml model register -m  info -n info -t fullmodel

   De uitvoer van elke opdracht resulteert in een model-ID die nodig is in de volgende stap. Ze opslaan in een tekstbestand voor toekomstig gebruik.

4. Maak een manifest voor de webservice.

   Een manifest bevat de code voor de webservice, de machine learning-modellen en afhankelijkheden voor runtime-omgeving. Ga naar de `Code` map in de CLI en voer de volgende opdracht:

        az ml manifest create -n $webserviceName -f webservice.py -r spark-py -c ../Config/conda_dependencies_webservice.yml -i $modelID1 -i $modelID2 -i $modelID3 -i $modelID4 -i $modelID5

   De uitvoer geeft een manifest ID voor de volgende stap. 

   Blijven de `Code` map en u kunt webservice.py testen door het uitvoeren van de volgende: 

        az ml experiment submit -t dockerdsvm -c dockerdsvm webservice.py

5. Maak een Docker-installatiekopie. 

        az ml image create -n [unique]image --manifest-id $manifestID

   De uitvoer biedt een afbeeldings-ID voor de volgende stap, deze docker-installatiekopie wordt gebruikt in een Container Service. 

6. De webservice implementeren in de Container Service-cluster.

        az ml service create realtime -n [unique] --image-id $imageID --cpu 0.5 --memory 2G

   De uitvoer biedt een service-ID. U moet gebruiken voor het ophalen van de autorisatiesleutel en service-URL.

7. De webservice-aanroep in Python-code te beoordelen in mini-batches.

   Gebruik de volgende opdracht om de autorisatiesleutel:

         az ml service keys realtime -i $ServiceID 

   Gebruik de volgende opdracht om de score berekenen voor URL-service:

        az ml service usage realtime -i $ServiceID

   Wijzigen van de inhoud in `./Config/webservice.json` met de juiste service score berekenen voor URL en autorisatie-sleutel. De 'Bearer' in het oorspronkelijke bestand behouden en vervang het gedeelte 'xxx'. 
   
   Ga naar de hoofdmap van uw project en testen van de webservice voor Mini score berekenen met behulp van de volgende:

        az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/scoring_webservice.py ./Config/webservice.json

8. De webservice worden geschaald. 

   Zie voor meer informatie [uitoefening op uw Azure Container Service-cluster schalen](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/how-to-scale.md).
 

## <a name="next-steps"></a>Volgende stappen

Dit voorbeeld illustreert hoe u met Machine Learning-Workbench trainen van een machine learning-model van big data en operationeel maken het getrainde model. In het bijzonder u geleerd hoe u wilt configureren en gebruiken verschillende rekenscenario doelen, en voer de geschiedenis van metrische gegevens bijhouden en andere wordt uitgevoerd.

U kunt de code om te verkennen kruisvalidatie en hyper-parameter afstemmen uitbreiden. Zie voor meer informatie over het afstemmen van kruisvalidatie en hyper-parameter, [deze GitHub-bron](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning).  

Zie voor meer informatie over de tijdreeks prognose, [deze GitHub-bron](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting).
