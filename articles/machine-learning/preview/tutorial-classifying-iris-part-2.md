---
title: Een model bouwen voor Azure Machine Learning-services (preview) | Microsoft Docs
description: Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel 2 over experimenteren.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/25/2017
ms.openlocfilehash: 5d86f3bdf19603d2f92fc1a704376beefd7323c0
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="classifying-iris-part-2-build-a-model"></a>Classificeren van Iris deel 2: een model bouwen
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is deel 2 van een serie van drie. In dit gedeelte van de zelfstudie wordt uitgelegd hoe u Azure Machine Learning-services (preview) gebruikt om deze bewerkingen uit te voeren:

> [!div class="checklist"]
> * Werken in Azure Machine Learning Workbench
> * Scripts openen en code controleren
> * Scripts uitvoeren in een lokale omgeving
> * Uitvoeringsgeschiedenis controleren
> * Scripts uitvoeren in een lokale Docker-omgeving
> * Scripts uitvoeren in een lokaal Azure CLI-venster
> * Scripts uitvoeren in een remote Docker-omgeving
> * Scripts uitvoeren in een HDInsight-cloudomgeving

In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/Iris_flower_data_set) gebruikt om dingen overzichtelijk te houden. De schermafbeeldingen zijn specifiek voor Windows, maar de ervaring voor macOS is bijna identiek.

## <a name="prerequisites"></a>Vereisten
U moet het eerste deel van deze zelfstudie hebben voltooid. Volg de zelfstudie [Gegevens voorbereiden](tutorial-classifying-iris-part-1.md) om Azure Machine Learning-resources te maken en de toepassing Azure Machine Learning Workbench te installeren voordat u begint aan deze zelfstudie.

U kunt ook experimenteren met het uitvoeren van scripts voor een lokale Docker-container. Daarvoor moet u lokaal op uw Windows- of macOS-computer een Docker-engine (Community Edition is voldoende) hebben geïnstalleerd en gestart. Meer informatie over [Docker-installatie-instructies](https://docs.docker.com/engine/installation/).

Als u wilt experimenteren met het verzenden van scripts voor uitvoering in een Docker-container in een externe virtuele Azure-machine of een HDInsight Spark-cluster, volg dan de [Instructies voor het maken van een op Ubuntu gebaseerde Azure Data Science Virtual Machine, of HDI-cluster](how-to-create-dsvm-hdi.md) .

## <a name="review-irissklearnpy-and-configuration-files"></a>Bestand iris_sklearn.py en configuratiebestanden controleren
1. Start de toepassing **Azure Machine Learning Workbench** en open het project **myIris** dat u in deel 1 van de serie hebt gemaakt.

2. Zodra het project is geopend, klikt u op de knop **Files** (mappictogram) op de werkbalk aan de linkerkant in Azure Machine Learning Workbench om de lijst met bestanden te openen in de projectmap.

3. Selecteer het bestand **iris_sklearn.py** om de Python code weer te geven op een nieuw tabblad van de teksteditor in de Workbench.

   ![bestand openen](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Het is mogelijk dat de code die u ziet en de code in de zelfstudie niet exact overeenkomen. De reden hiervoor is dat dit voorbeeldproject regelmatig wordt bijgewerkt.

4. Bekijk de code van het Python script om vertrouwd te raken met de stijl van coderen. U ziet dat met het script de volgende taken worden uitgevoerd:

   - Het DataPrep-pakket **iris.dprep** wordt geladen om een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) te maken. 

        >[!NOTE]
        >We gebruiken het DataPrep-pakket `iris.dprep` dat deel uitmaakt van het voorbeeldproject. Dit pakket moet overeenkomen met het bestand `iris-1.dprep` dat u in deel 1 van deze zelfstudie hebt gemaakt.

   - Er worden willekeurige functies toegevoegd om het oplossen van het probleem te bemoeilijken. (Randomisering is nodig omdat Iris een kleine gegevensset is die eenvoudig kan worden geclassificeerd met bijna 100% nauwkeurigheid.)

   - De machine learning-bibliotheek [scikit-learn](http://scikit-learn.org/stable/index.html) wordt gebruikt voor het bouwen van een eenvoudig logistiek regressiemodel. 

   - Het model wordt met behulp van de [pickle](https://docs.python.org/2/library/pickle.html)-bibliotheek geserialiseerd in een bestand in de map `outputs`, waarna het model gedeserialiseerd weer in het geheugen wordt geladen.

   - Het gedeserialiseerde model wordt gebruikt om een voorspelling te geven voor een nieuwe record. 

   - Er worden met behulp van de [matplotlib](https://matplotlib.org/)-bibliotheek twee grafieken uitgezet (verwarringsmatrix en meerklassige ROC-curve), die worden opgeslagen in de map `outputs`.

   - Het object `run_logger` wordt overal in het script gebruikt om de regularisatiefrequentie vast te leggen en nauwkeurigheid te modelleren in logboeken, die automatisch worden uitgezet in de uitvoeringsgeschiedenis.


## <a name="execute-irissklearnpy-script-in-local-environment"></a>Het script iris_sklearn.py uitvoeren in lokale omgeving

We gaan nu de voorbereidingen treffen om het script **iris_sklearn.py** voor het eerst uit te voeren. Dit script vereist de pakketten scikit-learn en matplotlib. **scikit-learn** is al geïnstalleerd door de Azure ML Workbench. Het pakket **matplotlib** moet echter nog wel worden geïnstalleerd. 

1. Klik in Azure Machine Learning Workbench op het menu **File** en kies **Open Command Prompt** om de opdrachtprompt te starten. We verwijzen naar dit venster van de opdrachtregelinterface als Azure Machine Learning Workbench CLI-venster, of kortweg CLI-venster.

2. Typ in het CLI-venster de volgende opdracht om het Python-pakket **matplotlib** te installeren. De installatie duurt als het goed is minder dan een minuut.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Als u de bovenstaande opdracht `pip install` overslaat, wordt de code in `iris_sklearn.py` wel gewoon uitgevoerd, maar bestaat de uitvoer niet uit de verwarringsmatrix en meerklassige ROC-curve die zichtbaar zijn in de geschiedenisvisualisaties.

3. Ga terug naar het venster van de Workbench-app. 

4. Klik in de linkerbovenhoek van het tabblad **iris_sklearn.py**, naast het pictogram voor opslaan, op de vervolgkeuzelijst om de optie **Run Configuration** te selecteren.  Kies **local** als de uitvoeringsomgeving en `iris_sklearn.py` als het uit te voeren script.

5. Ga vervolgens op hetzelfde tabblad naar het veld **Arguments** en vul hier de waarde `0.01` in. 

   ![afbeelding](media/tutorial-classifying-iris/run_control.png)

6. Klik op de knop **Run**. Er wordt direct een taak gepland. De taak wordt vermeld in het deelvenster **Jobs** aan de rechterkant van het Workbench-venster. 

7. Na enkele ogenblikken verandert de status van de taak van **Submitting** in **Running** en tot slot in **Completed**.

   ![sklearn uitvoeren](media/tutorial-classifying-iris/run_sklearn.png)

8. Klik op het woord **Completed** in de taakstatustekst in het deelvenster Jobs. Er wordt een pop-upvenster geopend met de standaarduitvoertekst (stdout) van het script dat wordt uitgevoerd. Sluit dit venster door in de rechterbovenhoek op de knop **X** te klikken.

9. Klik in dezelfde taakstatus in het deelvenster Jobs op de blauwe tekst **iris_sklearn.py [n]** (_n_ is het uitvoeringsnummer) net boven de status **Completed** en de begintijd. De pagina **Run Properties** wordt geopend en u ziet informatie over de uitvoeringseigenschappen, zoals de uitvoerbestanden bij **Outputs**, eventuele visualisaties bij **Visualizations** en logboeken voor die uitvoering bij **Logs**. 

   Als de uitvoering is voltooid, bevat het pop-upvenster de volgende resultaten:

   >[!NOTE]
   >Aangezien we eerder in de zelfstudie randomisering hebben geïntroduceerd, kunnen de exacte resultaten iets verschillen.

   ```text
   Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
   
   Iris dataset shape: (150, 5)
   Regularization rate is 0.01
   LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
          penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
          verbose=0, warm_start=False)
   Accuracy is 0.6792452830188679
   
   ==========================================
   Serialize and deserialize using the outputs folder.
   
   Export the model to model.pkl
   Import the model from model.pkl
   New sample: [[3.0, 3.6, 1.3, 0.25]]
   Predicted class is ['Iris-setosa']
   Plotting confusion matrix...
   Confusion matrix in text:
   [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
   Confusion matrix plotted.
   Plotting ROC curve....
   ROC curve plotted.
   Confusion matrix and ROC curve plotted. See them in Run History details page.
   ```

10. Sluit het tabblad **Run Properties** en ga terug naar het tabblad **iris_sklearn.py**. 

11. Voer aanvullende runs uit. 

    Geef een reeks andere numerieke waarden op in het veld **Arguments**, variërend van `0.001` tot `10`. Klik op **Run** om de code nog een paar keer uit te voeren. De argumentwaarde die u elke keer wijzigt, wordt doorgegeven aan het algoritme voor logistieke regressie, waardoor de resultaten telkens anders zijn.

## <a name="review-run-history-in-detail"></a>Uitvoeringsgeschiedenis in detail bekijken
In Azure Machine Learning Workbench wordt elke uitvoering van een script vastgelegd in een record met de uitvoeringsgeschiedenis. U kunt de uitvoeringsgeschiedenis van een bepaald script weergeven door de weergave **Runs** te openen.

1. Klik op de werkbalk links op de knop **Runs** (klokpictogram) om een lijst met**** uitvoeringen te openen. Klik vervolgens op **iris_sklearn.py** om het **Run Dashboard** van `iris_sklearn.py` weer te geven.

   ![afbeelding](media/tutorial-classifying-iris/run_view.png)

2. Het tabblad **Run Dashboard** wordt geopend. Bekijk de statistieken die zijn vastgelegd voor verschillende runs. Bovenaan het tabblad staan grafieken en in de tabel aan de onderzijde van de pagina worden voor elke genummerde run allerlei details vermeld.

   ![afbeelding](media/tutorial-classifying-iris/run_dashboard.png)

3. Filter de tabel en klik in de grafieken om interactief gegevens te bekijken van status, duur, nauwkeurigheid en regularisatiefrequentie van elke run. 

4. Selecteer twee of drie runs in de tabel **Runs** en klik op de knop **Compare** om een gedetailleerde vergelijkingspagina te openen. U kunt de gegevens van de verschillende runs nu naast elkaar bekijken. Klik in de linkerbovenhoek van de vergelijkingspagina op de pijl-links naast **Run List**om terug te keren naar het **Run Dashboard**.

5. Klik op een afzonderlijke run om de detailweergave van de run te zien. U ziet dat de statistieken voor de geselecteerde run worden vermeld in de sectie _Run Properties_. De bestanden die worden weggeschreven naar de uitvoermap, worden vermeld in de sectie **Outputs** en kunnen worden gedownload.

   ![afbeelding](media/tutorial-classifying-iris/run_details.png)

   De twee grafieken, de verwarringsmatrix en de meerklassige ROC-curve, ziet u in de sectie **Visualizations**. Alle logboekbestanden zijn beschikbaar in de sectie **Logs**.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Scripts uitvoeren in de lokale Docker-omgeving

In Azure ML kunt u gemakkelijk extra uitvoeringsomgevingen configureren, zoals Docker, en een script uitvoeren in die omgevingen. 

>[!IMPORTANT]
>U kunt deze procedure alleen uitvoeren als de Docker-engine lokaal is geïnstalleerd en is gestart. Raadpleeg de installatiehandleiding voor meer informatie.

1. Selecteer op de werkbalk links het mappictogram om het overzicht **Files** te openen voor uw project. Vouw de map `aml_config` uit. 

2. U ziet dat er verschillende omgevingen vooraf zijn geconfigureerd, zoals **docker-python**, **docker-spark** en **local**. 

   Elke omgeving heeft twee bestanden, zoals `docker-python.compute` en `docker-python.runconfig`. Open elk type bestand om te zien welke opties kunnen geconfigureerd in de teksteditor.  

   Sluit (X) de tabbladen van geopende teksteditors om het scherm wat overzichtelijker te maken.

3. Voer het script **iris_sklearn.py** uit in de omgeving **docker-python**. 

   - Klik op de werkbalk links op het klokpictogram om het deelvenster **Runs** te openen. Klik op **All Runs**. 
   - Kies bovenaan het tabblad **All Runs** de omgeving **docker-python** in plaats van de standaardomgeving **local**. 
   - Ga naar rechts en kies daar **iris_sklearn.py** als het script dat u wilt uitvoeren. 
   - Laat het veld **Arguments** leeg aangezien in het script een standaardwaarde is opgegeven. 
   - Klik op de knop **Run**.

4. U ziet dat er aan de rechterkant van het venster van de workbench een nieuwe taak wordt gestart in het deelvenster **Jobs**.

   Als u een script voor het eerst uitvoert in Docker, duurt het proces een paar minuten langer. 

   Achter de schermen wordt door Azure Machine Learning Workbench namelijk een nieuw Docker-bestand gemaakt dat verwijst naar de basisinstallatiekopie voor Docker die is opgegeven in het bestand `docker.compute` en naar benodigde Python-pakketten die zijn opgegeven in het bestand `conda_dependencies.yml`. De Docker-engine downloadt vervolgens de basisinstallatiekopie uit Azure, installeert de Python-pakketten die zijn opgegeven in het bestand `conda_dependencies.yml` en start vervolgens een Docker-container. Daarna wordt de lokale kopie van de projectmap gekopieerd en wordt het script `iris_sklearn.py` uitgevoerd. Afhankelijk van de configuratie van de uitvoering, kan er ook worden alleen worden verwezen naar de kopie. Uiteindelijk moet het resultaat er precies hetzelfde uitzien als voor de omgeving **local**.

5. Laten we nu Spark eens proberen. De Docker-basisinstallatiekopie bevat een vooraf geïnstalleerde en geconfigureerde Spark-instantie. Dit betekent dat u een PySpark-script kunt uitvoeren in Docker. Dit is een eenvoudige manier om een Spark-programma te ontwikkelen en testen zonder tijd te hoeven te besteden aan installatie en configuratie van Spark. 

   Open het `iris_pyspark.py`-bestand. Met dit script wordt het gegevensbestand `iris.csv` geladen, waarna het algoritme voor logistieke regressie uit de Spark ML-bibliotheek wordt gebruikt voor het classificeren van de Iris-gegevensset. Wijzig nu de uitvoeringsomgeving in **docker-spark**, het script in **iris_pyspark.py** en voer het script opnieuw uit. Dit duurt iets langer omdat er een Spark-sessie moet worden gemaakt en gestart binnen de Docker-container. U ziet ook dat de stdout anders is dan de stdout van `iris_pyspark.py`.

6. Voer nog een paar runs uit en experimenteer met verschillende argumenten. 

7. Open het bestand `iris_pyspark.py` om het eenvoudige logistieke regressiemodel te zien dat is gebouwd met behulp van de Spark ML-bibliotheek. 

8. Probeer wat dingen uit in het deelvenster **Jobs**, bekijk de uitvoeringsgeschiedenis en open de detailweergave van de runs die zijn uitgevoerd in verschillende omgevingen.

## <a name="execute-scripts-in-the-azure-ml-cli-window"></a>Scripts uitvoeren in het CLI-venster van Azure ML

1. Open het opdrachtregelvenster vanuit Azure Machine Learning Workbench door te klikken op het menu **File** en vervolgens **Open Command Prompt** te kiezen. De opdrachtprompt wordt gestart in de projectmap met de prompt `C:\Temp\myIris\>`.

   >[!Important]
   >U moet het opdrachtregelvenster (gestart vanuit de Workbench) gebruiken voor het uitvoeren van de volgende stappen:

2. Gebruik de opdrachtprompt (CLI) om u aan te melden bij Azure. 

   De Workbench-app en de CLI gebruiken onafhankelijk referentiecaches voor verificatie bij Azure-resources. U hoeft dit slechts één keer te doen, totdat het token in de cache verloopt. Gebruik de opdracht **az account list** om een lijst met abonnementen op te vragen die u kunt gebruiken voor aanmelding. Als u uit meerdere abonnementen kunt kiezen, gebruikt u de id-waarde van het gewenste abonnement en stelt u dat abonnement in als het standaardaccount voor gebruik met de opdracht **az set account -s**, waarbij u id-waarde van het abonnement opgeeft. Bevestig de instelling vervolgens met de opdracht account show.

   ```azurecli
   REM login using aka.ms/devicelogin site.
   az login
   
   REM list all Azure subscriptions you have access to. 
   az account list -o table
   
   REM set the current Azure subscription to the one you want to use.
   az set account -s <subscriptionId>
   
   REM verify your current subscription is set correctly
   az account show
   ```

3. Als u bent geverifieerd en de huidige context van het Azure-abonnement is ingesteld, typt u de volgende opdrachten in het CLI-venster om matplotlib installeren, en het python-script te versturen als een experiment dat moet worden uitgevoerd.

   ```azurecli
   REM You don't need to do this if you have installed matplotlib locally from the previous steps.
   pip install matplotlib
   
   REM Kick off an execution of the iris_sklearn.py file against local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Controleer de uitvoer. U ziet dat de uitvoer en het resultaat overeenkomen met de uitvoer en het resultaat van het script dat u eerder in deze zelfstudie hebt uitgevoerd met behulp van de Workbench. 

5. Voer hetzelfde script uit met behulp van de Docker-uitvoeringsomgeving, als Docker op uw computer is geïnstalleerd.

   ```azurecli
   REM Execute iris_sklearn.py in local Docker container Python environment.
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM Execute iris_pyspark.py in local Docker container Spark environment.
   az ml experiment submit -c docker-spark .\iris_pyspark.py 0.1
   ```
6. Klik in de Azure Machine Learning Workbench op het mappictogram op de werkbalk links om de projectbestanden weer te geven en open vervolgens het Python-script met de naam **run.py**. 

   Dit script is handig om verschillende regularisatiefrequenties te doorlopen en het experiment meerdere keren met deze frequenties uit te voeren. Met dit script wordt een `iris_sklearn.py`-taak gestart met een regularisatiefrequentie van `10.0` (een belachelijk hoge waarde). Start vervolgens steeds nieuwe runs, waarbij u de frequentie halveert totdat deze uiteindelijk nog maar `0.005` bedraagt. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   U kunt het script **run.py** starten vanaf de opdrachtregel door de volgende opdrachten uit te voeren:

   ```cmd
   REM Submit iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   Wanneer `run.py` is voltooid, ziet u een grafiek in de uitvoeringsgeschiedenis in de Azure Machine Learning Workbench.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Uitvoeren in een Docker-container op een externe computer
Als u het script wilt uitvoeren in een Docker-container op een externe Linux-computer, moet u SSH-toegang (gebruikersnaam en wachtwoord) hebben tot die externe computer. Bovendien moet op die externe computer de Docker-engine zijn geïnstalleerd en worden uitgevoerd. De eenvoudigste manier om een dergelijke Linux-machine in te richten, is door een [op Ubuntu gebaseerde DSVM (Data Science Virtual Machine)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) te maken in Azure. (De op CentOS gebaseerde DSVM wordt NIET ondersteund.) 

1. Zodra de VM is gemaakt, kunt u de VM koppelen als een uitvoeringsomgeving door het genereren van een tweetal bestanden `.runconfig` en `.compute`. Dit kan met de onderstaande opdracht. We noemen de nieuwe omgeving voor het gemak `myvm`.
 
   ```azurecli
   REM create myvm compute target
   az ml computetarget attach --name myvm --address <IP address> --username <username> --password <password> --type remotedocker
   ```
   
   >[!NOTE]
   >Het gebied IP Address kan ook een vrij adresseerbare FQDN (Fully Qualified Domain Name) zijn, zoals `vm-name.southcentralus.cloudapp.azure.com`. Het is raadzaam om de FQDN toe te voegen aan uw DSVM en deze hier te gebruiken in plaats van een IP-adres, aangezien de kans bestaat dat u de VM op een bepaald moment moet uitschakelen om kosten te besparen. Bovendien is het zo dat het IP-adres mogelijk is gewijzigd als u de VM de volgende keer start.

   Voer vervolgens de volgende opdracht uit om de Docker-installatiekopie voor te bereiden in de VM zodat deze klaar is voor het uitvoeren van de scripts.
   
   ```azurecli
   REM prepare the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >U kunt ook de waarde van `PrepareEnvironment` in `myvm.runconfig` wijzigen van de standaardwaarde `false` in `true`. De Docker-container wordt dan automatisch voorbereid bij de eerste run.

2. Bewerk het gegenereerde bestand `myvm.runconfig` onder `aml_config` en wijzig het framework van de standaardwaarde `PySpark` in `Python`:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Als u het framework op PySpark laat staan, werkt dit trouwens ook goed. Het is echter ietwat inefficiënt als niet echt een Spark-sessie nodig hebt om het Python-script uit te voeren.

3. Gebruik dezelfde opdracht als eerder in het CLI-venster, maar dit keer voor de omgeving _myvm_:
   ```azurecli
   REM execute iris_sklearn.py in remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   De opdracht wordt uitgevoerd alsof u een `docker-python`-omgeving gebruikt, behalve dat de uitvoering wordt uitgevoerd op de externe Linux-VM. Het CLI-venster bevat dezelfde uitvoergegevens.

4. Laten we eens kijken of Spark werkt in de container. Open Verkenner (u kunt dit ook doen vanuit het CLI-venster als u bekend bent met de basisopdrachten voor bestandsbeheer). Maak een kopie van het bestand `myvm.runconfig` en wijzig de naam in `myvm-spark.runconfig`. Bewerk het nieuwe bestand om de instelling van `Framework` te wijzigen van `Python` in `PySpark`:
   ```yaml
   "Framework": "PySpark"
   ```
   Laat het bestand `myvm.compute` ongewijzigd. Voor de uitvoering van Spark gebruiken we namelijk dezelfde installatiekopie van Docker op dezelfde VM. In het nieuwe bestand `myvy-spark.runconfig` verwijst het veld `target` naar hetzelfde bestand `myvm.compute`, via de naam `myvm`.

5. Typ de volgende opdracht om het script uit te voeren in de Spark-instantie in de externe Docker-container:
   ```azureli
   REM execute iris_pyspark.py in Spark instance on remote Docker container
   az ml experiment submit -c myvm-spark .\iris_pyspark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Script uitvoeren in een HDInsight-cluster
U kunt dit script ook uitvoeren in een echt Spark-cluster. 

1. Als u toegang hebt tot een cluster van Spark voor Azure HDInsight, genereert u een opdracht voor het uitvoeren van een HDI-configuratie zoals hieronder wordt weergegeven. Geef de naam op van het HDInsight-cluster, uw HDInsight-gebruikersnaam en het wachtwoord op als parameters. Gebruik de volgende opdracht:

   ```azurecli
   REM create a compute target that points to a HDI cluster
   az ml computetarget attach --name myhdi --address <cluster head node FQDN> --username <username> --password <password> --type cluster

   REM prepare the HDI cluster
   az ml experiment prepare -c myhdi
   ```

   De FQDN van het hoofdknooppunt van het cluster is meestal `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >De `username` is de SSH-gebruikersnaam van het cluster. De standaardwaarde is `sshuser` als u deze niet wijzigt tijdens het inrichten van HDI. Het is niet `admin`, wat de andere gebruiker is die wordt gemaakt tijdens het inrichten om toegang te kunnen hebben tot de beheerwebsite van het cluster. 

2. Voer de volgende opdracht uit en het script wordt uitgevoerd in het HDInsight-cluster:

   ```azurecli
   REM execute iris_pyspark on the HDI cluster
   az ml experiment submit -c myhdi .\iris_pyspark.py
   ```

   >[!NOTE]
   >Wanneer u het script uitvoert in een extern HDI-cluster, kunt u ook de uitvoeringsdetails van de YARN-taak bekijken op `https://<cluster_name>.azurehdinsight.net/yarnui`. Gebruik hiervoor het gebruikersaccount `admin`.


## <a name="next-steps"></a>Volgende stappen
In dit tweede deel van deze zelfstudie hebt u geleerd hoe u Azure Machine Learning-services kunt gebruiken voor deze bewerkingen:
> [!div class="checklist"]
> * Werken in Azure Machine Learning Workbench
> * Scripts openen en code controleren
> * Scripts uitvoeren in een lokale omgeving
> * Uitvoeringsgeschiedenis controleren
> * Scripts uitvoeren in een lokale Docker-omgeving
> * Scripts uitvoeren in een lokaal Azure CLI-venster
> * Scripts uitvoeren in een remote Docker-omgeving
> * Scripts uitvoeren in een HDInsight-cloudomgeving

U kunt nu verder met het derde deel van deze zelfstudie. Aangezien we nu beschikken over een logistiek regressiemodel, is het een goed idee om dit model te gaan implementeren als een realtime webservice.

> [!div class="nextstepaction"]
> [Een model implementeren](tutorial-classifying-iris-part-3.md)
