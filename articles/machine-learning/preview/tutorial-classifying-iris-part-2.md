---
title: Een model bouwen voor Azure Machine Learning-services (preview) | Microsoft Docs
description: Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel twee en hierin wordt het experimenteren besproken.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 11/06/2017
ms.openlocfilehash: 3337ba9fa6845deb98813259821f77ff713f9eb1
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="classify-iris-part-2-build-a-model"></a>Classificeren van Iris deel 2: een model bouwen
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is deel 2 van een driedelige reeks. In dit gedeelte van de zelfstudie wordt uitgelegd hoe u Azure Machine Learning-services (preview) gebruikt om:

> [!div class="checklist"]
> * Azure Machine Learning Workbench te gebruiken.
> * Scripts te openen en code te controleren.
> * Scripts uit te voeren in een lokale omgeving.
> * De uitvoeringsgeschiedenis te controleren.
> * Scripts uit te voeren in een lokale Docker-omgeving.
> * Scripts uit te voeren in een lokaal Azure CLI-venster.
> * Scripts uit te voeren in een externe Docker-omgeving.
> * Scripts uit te voeren in een HDInsight-cloudomgeving.

In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/Iris_flower_data_set) gebruikt. De schermopnamen zijn specifiek voor Windows, maar de Mac OS-versie is bijna identiek.

## <a name="prerequisites"></a>Vereisten
Voltooi het eerste deel van deze reeks zelfstudies. Volg de zelfstudie [Gegevens voorbereiden](tutorial-classifying-iris-part-1.md) om Azure Machine Learning-resources te maken en de toepassing Azure Machine Learning Workbench te installeren voordat u begint aan deze zelfstudie.

U kunt ook experimenteren met het uitvoeren van scripts voor een lokale Docker-container. Daarvoor moet op uw Windows- of Mac OS-computer lokaal een Docker-engine (Community Edition is voldoende) zijn geïnstalleerd en gestart. Zie [Installatie-instructies voor Docker](https://docs.docker.com/engine/installation/) voor meer informatie over het installeren van Docker.

Volg de [instructies voor het maken van een virtuele Azure Data Science-machine of een Azure HDInsight Spark-cluster](how-to-create-dsvm-hdi.md) als u wilt experimenteren met het verzenden van scripts die moeten worden uitgevoerd in een Docker-container in een externe virtuele Azure-machine of een HDInsight Spark-cluster.

## <a name="review-irissklearnpy-and-the-configuration-files"></a>Iris_sklearn.py en de configuratiebestanden controleren
1. Open de toepassing Azure Machine Learning Workbench en open het project **myIris** dat u in het vorige deel van de reeks zelfstudies hebt gemaakt.

2. Nadat u het project hebt geopend, selecteert u in het deelvenster uiterst links de knop **Files** (mappictogram) om de lijst met bestanden in de projectmap te openen.

3. Selecteer het bestand **iris_sklearn.py**. De Python-code wordt weergegeven op een nieuw tabblad van de teksteditor in Workbench.

   ![Een bestand openen](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Het is mogelijk dat de code die u ziet, en de code in de zelfstudie niet exact overeenkomen. De reden hiervoor is dat dit voorbeeldproject regelmatig wordt bijgewerkt.

4. Bekijk de code van het Python script om vertrouwd te raken met de stijl van coderen. Met het script worden de volgende taken uitgevoerd:

   - Het gegevensvoorbereidingspakket **iris.dprep** wordt geladen om een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) te maken. 

        >[!NOTE]
        >Gebruik het gegevensvoorbereidingspakket `iris.dprep` dat deel uitmaakt van het voorbeeldproject. Dit pakket moet overeenkomen met het bestand `iris-1.dprep` dat u in deel 1 van deze zelfstudie hebt gemaakt.

   - Er worden willekeurige functies toegevoegd om het oplossen van het probleem te bemoeilijken. Randomisering is nodig omdat Iris een kleine gegevensset is die eenvoudig kan worden geclassificeerd met bijna 100% nauwkeurigheid.

   - Gebruikt de Machine Learning-bibliotheek [scikit-learn](http://scikit-learn.org/stable/index.html) voor het bouwen van een eenvoudig logistiek regressiemodel. 

   - Serialiseert het model door de [pickle](https://docs.python.org/2/library/pickle.html)-bibliotheek in te voegen in een bestand in de map `outputs`. Met het script wordt het model vervolgens gedeserialiseerd in het geheugen.

   - Het gedeserialiseerde model wordt gebruikt om een voorspelling te geven voor een nieuwe record. 

   - Tekent twee grafieken, een verwarringsmatrix en een ROC-curve (Receiver Operating Characteristic) met meerdere klassen, met behulp van een [matplotlib](https://matplotlib.org/)-bibliotheek, en slaat deze vervolgens op in de map `outputs`.

   - Het object `run_logger` wordt gebruikt om de regularisatiefrequentie vast te leggen en om de nauwkeurigheid te modelleren in de logboeken. De logboeken worden automatisch in de uitvoeringsgeschiedenis getekend.


## <a name="execute-irissklearnpy-script-in-a-local-environment"></a>Het script iris_sklearn.py uitvoeren in een lokale omgeving

We gaan nu de voorbereidingen treffen om het script **iris_sklearn.py** voor het eerst uit te voeren. Voor dit script zijn de pakketten **scikit-learn** en **matplotlib** vereist. Het pakket **scikit-learn** pakket is al geïnstalleerd in Azure Machine Learning Workbench. U moet het pakket **matplotlib** wel nog installeren. 

1. Selecteer in Azure Machine Learning Workbench het menu **File** en selecteer vervolgens **Open Command Prompt** om de opdrachtprompt te openen. Er wordt naar dit venster van de opdrachtregelinterface verwezen als *Azure Machine Learning Workbench CLI-venster*, of kortweg *CLI-venster*.

2. Voer in het CLI-venster de volgende opdracht in om het Python-pakket **matplotlib** te installeren. De installatie duurt minder dan een minuut.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Als u de vorige `pip install`-opdracht overslaat, wordt de code in `iris_sklearn.py` volledig uitgevoerd. Als u alleen `iris_sklearn.py` uitvoert, worden met de code de verwarringsmatrix en de ROC-curve met meerdere klassen niet geproduceerd die zichtbaar zijn in de geschiedenisvisualisaties.

3. Ga terug naar het venster van de Workbench-app. 

4. Open in de werkbalk boven aan het tabblad **iris_sklearn.py** de vervolgkeuzelijst naast het pictogram **Save** en selecteer vervolgens **Run Configuration**. Selecteer **local** als de uitvoeringsomgeving en voer vervolgens `iris_sklearn.py` in als het uit te voeren script.

5. Ga daarna naar de rechterkant van de werkbalk en voer `0.01` in het veld **Arguments** in. 

   ![Besturingselement van run](media/tutorial-classifying-iris/run_control.png)

6. Selecteer de knop **Run**. Er wordt direct een taak gepland. De taak wordt vermeld in het deelvenster **Jobs** aan de rechterkant van het Workbench-venster. 

7. Na enkele ogenblikken verandert de status van de taak van **Submitting** in **Running** en vervolgens in **Completed**.

   ![Sklearn van run](media/tutorial-classifying-iris/run_sklearn.png)

8. Selecteer **Completed** in de taakstatustekst in het deelvenster **Jobs**. Er wordt een pop-upvenster geopend met de standaarduitvoertekst (stdout) van het script dat wordt uitgevoerd. Als u de stdout-tekst wilt sluiten, selecteert u in de rechterbovenhoek van het pop-upvenster de knop **Close** (**x**).

9. Selecteer in dezelfde taakstatus in het deelvenster **Jobs** de blauwe tekst **iris_sklearn.py [n]** (_n_ is het uitvoeringsnummer) net boven de status **Completed** en de begintijd. Het venster **Run Properties** wordt geopend en de volgende informatie wordt weergegeven voor de desbetreffende run:
   - Informatie in **Run Properties**
   - **Uitvoerbestanden**
   - **Visualisaties**, indien aanwezig
   - **Logboeken** 

   Als de run is voltooid, bevat het pop-upvenster de volgende resultaten:

   >[!NOTE]
   >Omdat we eerder in de zelfstudie randomisering hebben geïntroduceerd, kunnen de exacte resultaten verschillen met de resultaten die hier worden weergegeven.

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
   Confusion matrix and ROC curve plotted. See them in Run History details pane.
   ```

10. Sluit het tabblad **Run Properties** en ga vervolgens terug naar het tabblad **iris_sklearn.py**. 

11. Voer aanvullende runs uit. 

    Geef een reeks andere numerieke waarden op in het veld **Arguments**, variërend van `0.001` tot `10`. Selecteer **Run** om de code nog een paar keer uit te voeren. De argumentwaarde die u elke keer wijzigt, wordt doorgegeven aan het algoritme voor logistieke regressie, waardoor de resultaten telkens anders zijn.

## <a name="review-the-run-history-in-detail"></a>De uitvoeringsgeschiedenis in detail bekijken
In Azure Machine Learning Workbench wordt elke uitvoering van een script vastgelegd in een record met de uitvoeringsgeschiedenis. U kunt de uitvoeringsgeschiedenis van een bepaald script weergeven door de weergave **Runs** te openen.

1. Als u de lijst met **Runs** wilt openen, selecteert u in de linkerwerkbalk de knop **Runs** (klokpictogram). Selecteer vervolgens **iris_sklearn.py** om het **Run Dashboard** van `iris_sklearn.py` weer te geven.

   ![Weergave van run](media/tutorial-classifying-iris/run_view.png)

2. Het tabblad **Run Dashboard** wordt geopend. Bekijk de statistieken die zijn vastgelegd voor de verschillende runs. Grafieken worden boven aan het tabblad weergegeven. Elke run heeft een volgnummer en de details van de run worden vermeld in de tabel onder in het scherm.

   ![Dashboard van run](media/tutorial-classifying-iris/run_dashboard.png)

3. Filter de tabel en selecteer vervolgens een of meer grafieken om de status, duur, nauwkeurigheid en regularisatiefrequentie van elke run te bekijken. 

4. Selecteer twee of drie runs in de tabel **Runs** en selecteer de knop **Compare** om een gedetailleerd vergelijkingsdeelvenster te openen. U kunt de gegevens van de verschillende runs nu naast elkaar bekijken. Selecteer in de linkerbovenhoek van het **vergelijkingsdeelvenster** de pijl-links naast **Run List** om terug te keren naar het **Run Dashboard**.

5. Selecteer een afzonderlijke run om de detailweergave van de run te zien. Opmerking: de statistieken voor de geselecteerde run worden vermeld in de sectie **Run Properties**. De bestanden die worden weggeschreven naar de uitvoermap, worden vermeld in de sectie **Outputs** en kunnen vanuit deze sectie worden gedownload.

   ![Details van run](media/tutorial-classifying-iris/run_details.png)

   De twee grafieken, de verwarringsmatrix en de ROC-curve met meerdere klassen worden weergegeven in de sectie **Visualizations**. Alle logboekbestanden zijn beschikbaar in de sectie **Logs**.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Scripts uitvoeren in de lokale Docker-omgeving

Met Machine Learning kunt u gemakkelijk extra uitvoeringsomgevingen configureren, zoals Docker, en een script uitvoeren in deze omgevingen. 

>[!IMPORTANT]
>Er moet een Docker-engine lokaal zijn geïnstalleerd en gestart om deze stap te kunnen voltooien. Zie de installatie-instructies voor Docker voor meer informatie.

1. Selecteer in het linkerdeelvenster het pictogram **Map** om de lijst **Files** te openen voor uw project. Vouw de map `aml_config` uit. 

2. Er zijn vooraf verschillende omgevingen geconfigureerd, zoals **docker-python**, **docker-spark** en **local**. 

   Elke omgeving heeft twee bestanden, zoals `docker-python.compute` en `docker-python.runconfig`. Open elk bestand om te zien dat er bepaalde opties zijn geconfigureerd in de teksteditor.  

   Selecteer **Close** (**x**) op te tabbladen voor de geopende teksteditors.

3. Voer het script **iris_sklearn.py** uit met behulp van de omgeving **docker-python**: 

   - Selecteer op de linkerwerkbalk op het pictogram **Klok** om het deelvenster **Runs** te openen. Selecteer **All Runs**. 
   - Selecteer boven in het tabblad **All Runs** de omgeving **docker-python** als doelomgeving in plaats van de standaardomgeving **local**. 
   - Ga vervolgens naar de rechterkant en selecteer **iris_sklearn.py** als het uit te voeren script. 
   - Laat het veld **Arguments** leeg aangezien in het script een standaardwaarde is opgegeven. 
   - Selecteer de knop **Run**.

4. Er wordt nu een nieuwe taak gestart. Deze wordt weergegeven in het deelvenster **Jobs** aan de rechterkant van het Workbench-venster.

   Als u een script voor het eerst uitvoert in Docker, duurt het proces een paar minuten langer. 

   Op de achtergrond wordt in Azure Machine Learning Workbench een nieuw Docker-bestand gemaakt. 
   Het nieuwe bestand verwijst naar de Docker-basisinstallatiekopie die is opgegeven in het bestand `docker.compute`, en de Python-afhankelijkheidspakketten die zijn opgegeven in het bestand `conda_dependencies.yml`. 
   
   De Docker-engine heeft de volgende taken:

    - Downloaden van de basisinstallatiekopie uit Azure.
    - Installeren van de Python-pakketten die zijn opgegeven in het bestand `conda_dependencies.yml`.
    - Starten van een Docker-container.
    - Kopiëren van of verwijzen naar (afhankelijk van de configuratie van de run) de lokale kopie van de projectmap.      
    - Uitvoeren van de het script `iris_sklearn.py`.

   Uiteindelijk moet het resultaat er precies hetzelfde uitzien als voor de omgeving **local**.

5. Laten we nu Spark eens proberen. De Docker-basisinstallatiekopie bevat een vooraf geïnstalleerde en geconfigureerde Spark-instantie. Vanwege deze instantie kunt u hier een PySpark-script uitvoeren. Dit is een eenvoudige manier om een Spark-programma te ontwikkelen en te testen zonder tijd te hoeven te besteden aan het installeren en configureren van Spark. 

   Open het `iris_spark.py`-bestand. Met dit script wordt het gegevensbestand `iris.csv` geladen, waarna het algoritme voor logistieke regressie uit de Spark Machine Learning-bibliotheek wordt gebruikt voor het classificeren van de Iris-gegevensset. Wijzig nu de uitvoeringsomgeving in **docker-spark**, het script in **iris_spark.py** en voer het script opnieuw uit. Dit proces duurt iets langer omdat er een Spark-sessie moet worden gemaakt en gestart binnen de Docker-container. U ziet ook dat de stdout anders is dan de stdout van `iris_spark.py`.

6. Voer nog een paar runs uit en experimenteer met verschillende argumenten. 

7. Open het bestand `iris_spark.py` om het logistieke regressiemodel te zien dat is gebouwd met behulp van de Spark Machine Learning-bibliotheek. 

8. Probeer wat dingen uit in het deelvenster **Jobs**, bekijk de uitvoeringsgeschiedenis en open een detailweergave van de runs die zijn uitgevoerd in verschillende omgevingen.

## <a name="execute-scripts-in-the-azure-machine-learning-cli-window"></a>Scripts uitvoeren in het CLI-venster van Azure Machine Learning

1. Open het opdrachtregelvenster in Azure Machine Learning Workbench, selecteer het menu **File** en selecteer vervolgens **Open Command Prompt**. De opdrachtprompt wordt gestart in de projectmap met de prompt `C:\Temp\myIris\>`.

   >[!IMPORTANT]
   >U moet het opdrachtregelvenster (geopend vanuit Workbench) gebruiken om de volgende stappen uit te voeren.

2. Gebruik de opdrachtprompt om u aan te melden bij Azure. 

   De Workbench-app en de CLI gebruiken onafhankelijk referentiecaches voor verificatie bij Azure-resources. U hoeft dit slechts één keer te doen, totdat het token in de cache verloopt. Gebruik de opdracht **az account list** om een lijst met abonnementen op te vragen die u kunt gebruiken voor aanmelding. Als er meer dan één abonnement is, gebruikt u de id-waarde van het gewenste abonnement. Stel dit abonnement in als het standaardaccount voor gebruik met de opdracht **az account set -s** en geef vervolgens de abonnements-id op. Bevestig de instelling vervolgens met behulp van de accountopdracht **show**.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Als de verificatie is voltooid en de huidige context van het Azure-abonnement is ingesteld, voert u de volgende opdrachten in het CLI-venster uit om **matplotlib** te installeren, en verzendt u vervolgens het Python-script als een experiment dat moet worden uitgevoerd.

   ```azurecli
   REM you don't need to run this command if you have installed matplotlib locally from the previous steps
   pip install matplotlib
   
   REM kicks off an execution of the iris_sklearn.py file against the local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Controleer de uitvoer. U krijgt dezelfde uitvoer en resultaten als toen u Workbench gebruikte voor het uitvoeren van het script. 

5. Voer hetzelfde script opnieuw uit met behulp van de Docker-uitvoeringsomgeving, als Docker op de computer is geïnstalleerd.

   ```azurecli
   REM executes iris_sklearn.py in the local Docker container Python environment
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM executes iris_spark.py in the local Docker container Spark environment
   az ml experiment submit -c docker-spark .\iris_spark.py 0.1
   ```
6. Selecteer in Workbench het pictogram **Map** in het linkerdeelvenster om de projectbestanden weer te geven en open vervolgens het Python-script met de naam **run.py**. 

   Dit script is nuttig om verschillende regularisatiefrequenties te herhalen. Voer het experiment meerdere keren met uit deze frequenties. Met dit script wordt een taak `iris_sklearn.py` gestart met een regularisatiefrequentie van `10.0` (een heel groot aantal). Vervolgens wordt de frequentie in de volgende run gehalveerd, enzovoort, totdat de frequentie `0.005` is. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   U kunt het script **run.py** openen vanaf de opdrachtregel door de volgende opdrachten uit te voeren:

   ```cmd
   REM submits iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   Wanneer `run.py` is voltooid, ziet u een grafiek in de uitvoeringsgeschiedenis in Workbench.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Uitvoeren in een Docker-container op een externe computer
Als u het script wilt uitvoeren in een Docker-container op een externe Linux-computer, moet u SSH-toegang (gebruikersnaam en wachtwoord) hebben tot die externe computer. Bovendien moet op deze externe computer een Docker-engine zijn geïnstalleerd en worden uitgevoerd. De eenvoudigste manier om een dergelijke Linux-machine in te richten, is door een op Ubuntu gebaseerde DSVM (Data Science Virtual Machine) te maken in Azure. Leer [hoe u een Ubuntu-DSVM maakt die u in Azure ML Workbench kunt gebruiken](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal).

>[!NOTE] 
>De op CentOS gebaseerde DSVM wordt *niet* ondersteund.

1. Nadat de VM is gemaakt, kunt u de VM koppelen als een uitvoeringsomgeving als u twee bestanden `.runconfig` en `.compute` genereert. Gebruik de volgende opdracht om de bestanden te genereren. We noemen de nieuwe omgeving voor het gemak `myvm`.
 
   ```azurecli
   REM creates an myvm compute target
   az ml computetarget attach remotedocker --name myvm --address <IP address> --username <username> --password <password>
   ```
   
   >[!NOTE]
   >Het IP-adres kan ook een vrij adresseerbare FQDN (Fully Qualified Domain Name) zijn, zoals `vm-name.southcentralus.cloudapp.azure.com`. U wordt aangeraden om de FQDN toe te voegen aan de DSVM en hier te gebruiken in plaats van een IP-adres. Dit is nuttig omdat u de VM mogelijk op een later tijdstip wilt uitschakelen om kosten te besparen. Bovendien is het zo dat het IP-adres mogelijk is gewijzigd als u de VM de volgende keer start.

   Voer vervolgens de volgende opdracht uit om de Docker-installatiekopie voor te bereiden in de VM zodat deze klaar is voor het uitvoeren van de scripts:
   
   ```azurecli
   REM prepares the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >U kunt de waarde van `PrepareEnvironment` in `myvm.runconfig` ook wijzigen van de standaardwaarde `false` in `true`. Door deze wijziging wordt de Docker-container automatisch voorbereid bij de eerste run.

2. Bewerk het gegenereerde bestand `myvm.runconfig` onder `aml_config` en wijzig het framework van de standaardwaarde `PySpark` in `Python`:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Als u de frameworkinstelling laat staan op PySpark, werkt dit meestal ook. Het is echter efficiënter als u geen Spark-sessie nodig hebt om het Python-script uit te voeren.

3. Gebruik dezelfde opdracht als eerder in het CLI-venster, maar dit keer voor de omgeving _myvm_:
   ```azurecli
   REM executes iris_sklearn.py in a remote Docker container
   az ml experiment submit -c myvm iris_sklearn.py
   ```
   De opdracht wordt uitgevoerd alsof u een `docker-python`-omgeving gebruikt, behalve dat de uitvoering wordt uitgevoerd op de externe Linux-VM. Het CLI-venster bevat dezelfde uitvoergegevens.

4. We gaan nu Spark gebruiken in de container. Open Verkenner. U kunt dit ook doen vanuit het CLI-venster als u bekend bent met de basisopdrachten voor bestandsbeheer. Maak een kopie van het bestand `myvm.runconfig` en wijzig de naam in `myvm-spark.runconfig`. Bewerk het nieuwe bestand om de instelling van `Framework` te wijzigen van `Python` in `PySpark`:
   ```yaml
   "Framework": "PySpark"
   ```
   Laat het bestand `myvm.compute` ongewijzigd. Voor de uitvoering van Spark wordt dezelfde Docker-installatiekopie gebruikt op dezelfde VM. In het nieuwe bestand `myvm-spark.runconfig` verwijst het veld `target` naar hetzelfde bestand `myvm.compute`, via de naam `myvm`.

5. Typ de volgende opdracht om het script uit te voeren in de Spark-instantie in de externe Docker-container:
   ```azureli
   REM executes iris_spark.py in a Spark instance on a remote Docker container
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Script uitvoeren in een HDInsight-cluster
U kunt dit script ook uitvoeren in een HDInsight Spark-cluster. Leer [hoe u een HDInsight Spark-cluster maakt dat u kunt gebruiken in Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal).

>[!NOTE] 
>Het HDInsight-cluster moet Azure Blob als primaire opslag gebruiken. Het gebruik van Azure Data Lake-opslag wordt nog niet ondersteund.

1. Als u toegang hebt tot een Spark-cluster voor Azure HDInsight, genereert u een opdracht voor het uitvoeren van een HDInsight-configuratie zoals hieronder wordt weergegeven. Geef de naam op van het HDInsight-cluster, en uw HDInsight-gebruikersnaam en wachtwoord op als parameters. Gebruik de volgende opdracht:

   ```azurecli
   REM creates a compute target that points to a HDInsight cluster
   az ml computetarget attach cluster --name myhdi --address <cluster head node FQDN> --username <username> --password <password>

   REM prepares the HDInsight cluster
   az ml experiment prepare -c myhdi
   ```

   De FQDN van het clusterhoofdknooppunt is meestal `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >De `username` is de SSH-gebruikersnaam van het cluster. De standaardwaarde is `sshuser` als u deze niet wijzigt tijdens de HDInsight-installatie. De waarde is niet `admin`, wat de andere gebruiker is die tijdens de installatie wordt gemaakt voor toegang tot de beheerwebsite van het cluster. 

2. Voer de volgende opdracht uit en het script wordt uitgevoerd in het HDInsight-cluster:

   ```azurecli
   REM executes iris_spark on the HDInsight cluster
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Wanneer u het script uitvoert in een extern HDInsight-cluster, kunt u ook de uitvoeringsdetails van de YARN-taak (Yet Another Resource Negotiator) bekijken op `https://<cluster_name>.azurehdinsight.net/yarnui`. Gebruik hiervoor het gebruikersaccount `admin`.


## <a name="next-steps"></a>Volgende stappen
In dit tweede deel van deze driedelige reeks zelfstudies hebt u geleerd hoe u Azure Machine Learning-services kunt gebruiken om:
> [!div class="checklist"]
> * Azure Machine Learning Workbench te gebruiken.
> * Scripts te openen en code te controleren.
> * Scripts uit te voeren in een lokale omgeving.
> * De uitvoeringsgeschiedenis te controleren.
> * Scripts uit te voeren in een lokale Docker-omgeving.
> * Scripts uit te voeren in een lokaal Azure CLI-venster.
> * Scripts uit te voeren in een externe Docker-omgeving.
> * Scripts uitte voeren in een HDInsight-cloudomgeving.

U kunt nu verder met het derde deel van deze zelfstudie. Aangezien u nu beschikt over een logistiek regressiemodel, is het een goed idee om dit model te gaan implementeren als een realtime webservice.

> [!div class="nextstepaction"]
> [Een model implementeren](tutorial-classifying-iris-part-3.md)
