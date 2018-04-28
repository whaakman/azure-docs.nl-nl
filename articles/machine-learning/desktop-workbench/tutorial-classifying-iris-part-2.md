---
title: Zelfstudie over het bouwen van een model voor Azure Machine Learning-services (preview) | Microsoft Docs
description: Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel twee en hierin wordt het experimenteren besproken.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 3/15/2018
ms.openlocfilehash: 24ac4d699a511cc99936cb2cd4b245de01984163
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-2-classify-iris---build-a-model"></a>Zelfstudie 2: Iris classificeren - Een model bouwen
Azure Machine Learning-services (preview) zijn een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is **deel 2 van een driedelige reeks**. In dit gedeelte van de zelfstudie wordt uitgelegd hoe u Azure Machine Learning-services voor het volgende gebruikt:

> [!div class="checklist"]
> * Scripts openen en code controleren
> * Scripts uitvoeren in een lokale omgeving
> * Uitvoeringsgeschiedenis beoordelen
> * Scripts uitvoeren in een lokaal Azure CLI-venster
> * Scripts uitvoeren in een lokale Docker-omgeving
> * Scripts uitvoeren in een remote Docker-omgeving
> * Scripts uitvoeren in een Azure HDInsight-cloudomgeving

In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/Iris_flower_data_set) gebruikt. 

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:
- Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 
- Een experimenteel account en een Azure Machine Learning Workbench zijn geïnstalleerd, zoals beschreven in deze [snelstart](../service/quickstart-installation.md)
- Het project en de voorbereide Iris-gegevens uit [Zelfstudie deel 1](tutorial-classifying-iris-part-1.md)
- Een Docker-engine moet zijn geïnstalleerd en lokaal worden uitgevoerd. De Community Edition van Docker is voldoende. Informatie over het installeren van Docker vindt u hier: https://docs.docker.com/engine/installation/.

## <a name="review-irissklearnpy-and-the-configuration-files"></a>Iris_sklearn.py en de configuratiebestanden controleren

1. Open de toepassing Azure Machine Learning Workbench.

1. Open vervolgens het project **myIris** dat u in [Deel 1 van de reeks zelfstudies hebt gemaakt](tutorial-classifying-iris-part-1.md).

2. Nadat u het project hebt geopend, selecteert u in het deelvenster uiterst links de knop **Files** (mappictogram) om de lijst met bestanden in de projectmap te openen.

   ![Het project in de Azure Machine Learning Workbench openen](media/tutorial-classifying-iris/2-project-open.png)

3. Selecteer het Python-scriptbestand **iris_sklearn.py**. 

   ![Kies een script](media/tutorial-classifying-iris/2-choose-iris_sklearn.png)

   De code wordt weergegeven op een nieuw tabblad van de teksteditor in Workbench. Dit is het script dat u in dit deel van de zelfstudie gebruikt. 

   >[!NOTE]
   >Het is mogelijk dat de code die u ziet, en de code in de zelfstudie niet exact overeenkomen. De reden hiervoor is dat dit voorbeeldproject regelmatig wordt bijgewerkt.
   
   ![Een bestand openen](media/tutorial-classifying-iris/open_iris_sklearn.png)

4. Bekijk de code van het Python-script om vertrouwd te raken met de stijl van coderen. 

   Met het script **iris_sklearn.py** worden de volgende taken uitgevoerd:

   * Het standaard gegevensvoorbereidingspakket **iris.dprep** wordt geladen om een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) te maken. 

   * Er worden willekeurige functies toegevoegd om het oplossen van het probleem te bemoeilijken. Randomness is nodig omdat Iris een kleine gegevensset is, die eenvoudig met bijna 100% nauwkeurigheid wordt geclassificeerd.

   * Gebruikt de Machine Learning-bibliotheek [scikit-learn](http://scikit-learn.org/stable/index.html) voor het bouwen van een eenvoudig logistiek regressiemodel.  Deze bibliotheek wordt standaard geleverd met Azure Machine Learning Workbench.

   * Serialiseert het model door de [pickle](https://docs.python.org/3/library/pickle.html)-bibliotheek in te voegen in een bestand in de map `outputs`. 
   
   * Het geserialiseerde model wordt geladen en daarna weer in het geheugen gedeserialiseerd.

   * Het gedeserialiseerde model wordt gebruikt om een voorspelling te geven voor een nieuwe record. 

   * Tekent twee grafieken, een verwarringsmatrix en een ROC-curve (Receiver Operating Characteristic-curve) met meerdere klassen, met behulp van een [matplotlib](https://matplotlib.org/)-bibliotheek, en slaat deze vervolgens op in de map `outputs`. Als u deze bibliotheek nog niet in uw omgeving hebt, kunt u deze installeren.

   * De nauwkeurigheid van de regulariseringsfrequentie en het model worden in de uitvoeringsgeschiedenis automatisch uitgezet. Het object `run_logger` wordt gebruikt om de regularisatiefrequentie en de nauwkeurigheid van het model in de logboeken vast te leggen. 


## <a name="run-irissklearnpy-in-your-local-environment"></a>Iris_sklearn.py uitvoeren in uw lokale omgeving

1. Start de Azure Machine Learning-opdrachtregelinterface (CLI):
   1. Start Azure Machine Learning Workbench.

   1. Selecteer in het menu Workbench **File** > **Open Command Prompt**. 
   
   Het venster Azure Machine Learning-opdrachtregelinterface (CLI) wordt gestart in de projectmap `C:\Temp\myIris\>` in Windows. Dit project is hetzelfde als de domeincontroller die u hebt gemaakt in Deel 1 van de zelfstudie.

   >[!IMPORTANT]
   >Om de volgende stappen uit te voeren, moet u dit CLI-venster gebruiken.

1. In het CLI-venster installeert u de Python-plottingbibliotheek **matplotlib**, als u deze bibliotheek nog niet hebt.

   Het script **iris_sklearn.py** is afhankelijk van twee Python-pakketten: **scikit-learn** en **matplotlib**.  Het pakket **scikit-learn** is al geïnstalleerd in Azure Machine Learning Workbench. Maar u moet **matplotlib** installeren als dit niet nog is geïnstalleerd.

   Als u doorgaat zonder dat u **matplotlib** installeert, kan de code in deze zelfstudie nog steeds met succes worden uitgevoerd. Het is voor de code echter niet mogelijk om de uitvoer van de verwarringsmatrix en de multiklasse ROC-curveplots die worden weergegeven in de geschiedenisvisualisaties te produceren.

   ```azurecli
   pip install matplotlib
   ```

   Deze installatie duurt ongeveer een minuut.

1. Ga terug naar de Workbench-toepassing. 

1. Zoek het tabblad met de naam **iris_sklearn.py**. 

   ![Zoek het tabblad met script](media/tutorial-classifying-iris/2-iris_sklearn-tab.png)

1. Selecteer in de werkbalk bovenaan het tabblad **local** als uitvoeringsomgeving en `iris_sklearn.py` als het uit te voeren script. Dit is mogelijk al ingeschakeld.

   ![Lokale en scriptkeuze](media/tutorial-classifying-iris/2-local-script.png)

1. Ga daarna naar de rechterkant van de werkbalk en voer `0.01` in het veld **Argumenten** in. 

   Deze waarde komt overeen met de regularisatiesnelheid van het logistieke regressiemodel.

   ![Lokale en scriptkeuze](media/tutorial-classifying-iris/2-local-script-arguments.png)

1. Selecteer de knop **Run**. Er wordt direct een taak gepland. De taak wordt vermeld in het deelvenster **Jobs** aan de rechterkant van het Workbench-venster. 

   ![Lokale en scriptkeuze](media/tutorial-classifying-iris/2-local-script-arguments-run.png)

   Na enkele ogenblikken verandert de status van de taak van **Submitting** in **Running** en uiteindelijk in **Completed**.

1. Selecteer **Completed** in de taakstatustekst in het deelvenster **Jobs**. 

   ![Sklearn van run](media/tutorial-classifying-iris/2-completed.png)

   Er wordt een pop-upvenster geopend met de standaarduitvoertekst (stdout) voor de run. Als u de stdout-tekst wilt sluiten, selecteert u in de rechterbovenhoek van het pop-upvenster de knop **Close** (**x**).

   ![Standaarduitvoer](media/tutorial-classifying-iris/2-standard-output.png)

9. Selecteer in dezelfde taakstatus in het deelvenster **Jobs** de blauwe tekst **iris_sklearn.py [n]** (_n_ is het uitvoeringsnummer) net boven de status **Completed** en de begintijd. Het venster **Run Properties** wordt geopend en de volgende informatie wordt weergegeven voor de desbetreffende run:
   - Informatie in **Run Properties**
   - **Uitvoer**
   - **Metrische gegevens**
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

11. Herhaal dit voor aanvullende runs. 

    Geef een reeks waarden op in het veld **Arguments**, variërend van `0.001` tot `10`. Selecteer **Run** om de code nog een paar keer uit te voeren. De argumentwaarde die u elke keer wijzigt, wordt doorgegeven aan het model voor logistieke regressie in de code, waardoor de resultaten telkens anders zijn.

## <a name="review-the-run-history-in-detail"></a>De uitvoeringsgeschiedenis in detail bekijken
In Azure Machine Learning Workbench wordt elke uitvoering van een script vastgelegd in een record met de uitvoeringsgeschiedenis. U kunt de uitvoeringsgeschiedenis van een bepaald script weergeven door de weergave **Runs** te openen.

1. Als u de lijst met **Runs** wilt openen, selecteert u in de linkerwerkbalk de knop **Runs** (klokpictogram). Selecteer vervolgens **iris_sklearn.py** om het **Run Dashboard** van `iris_sklearn.py` weer te geven.

   ![Weergave van run](media/tutorial-classifying-iris/run_view.png)

1. Het tabblad **Run Dashboard** wordt geopend. 

   Bekijk de statistieken die zijn vastgelegd voor de verschillende runs. Grafieken worden boven aan het tabblad weergegeven. Elke run heeft een volgnummer en de details van de run worden vermeld in de tabel onder in het scherm.

   ![Dashboard van run](media/tutorial-classifying-iris/run_dashboard.png)

1. Filter de tabel en selecteer vervolgens een of meer grafieken om de status, duur, nauwkeurigheid en regularisatiefrequentie van elke run te bekijken. 

1. Schakel de selectievakjes in naast twee of meer uitvoeringen in de tabel **Runs**. Selecteer de knop **Compare** om een gedetailleerd vergelijkingsdeelvenster te openen. U kunt de gegevens van de verschillende runs nu naast elkaar bekijken. 

1. Selecteer in de linkerbovenhoek van het **vergelijkingsdeelvenster** de knop **Run List** om terug te keren naar het **Run Dashboard**.

   ![Ga terug naar Run list](media/tutorial-classifying-iris/2-compare-back.png)

1. Selecteer een afzonderlijke run om de detailweergave van de run te zien. Opmerking: de statistieken voor de geselecteerde run worden vermeld in de sectie **Run Properties**. De bestanden die worden weggeschreven naar de uitvoermap, worden vermeld in de sectie **Outputs** en kunnen vanuit deze sectie worden gedownload.

   ![Details van run](media/tutorial-classifying-iris/run_details.png)

   De twee grafieken, de verwarringsmatrix en de ROC-curve met meerdere klassen worden weergegeven in de sectie **Visualizations**. Alle logboekbestanden zijn beschikbaar in de sectie **Logs**.


## <a name="run-scripts-in-local-docker-environments"></a>Scripts uitvoeren in een lokale Docker-omgeving

U kunt ook experimenteren met het uitvoeren van scripts voor een lokale Docker-container. U kunt gemakkelijk extra uitvoeringsomgevingen configureren, zoals Docker, en hierin een script uitvoeren. 

>[!NOTE]
>Volg de [instructies voor het maken van een virtuele Azure Data Science-machine of een Azure HDInsight Spark-cluster](how-to-create-dsvm-hdi.md) als u wilt experimenteren met het verzenden van scripts die moeten worden uitgevoerd in een Docker-container in een externe virtuele Azure-machine of een HDInsight Spark-cluster.

1. Als u dit nog niet hebt gedaan, installeert en start u Docker lokaal op uw Windows- of MacOS-machine. Zie de installatie-instructies voor Docker op https://docs.docker.com/install/ voor meer informatie. De Community Edition is voldoende.

1. Selecteer in het linkerdeelvenster het pictogram **Map** om de lijst **Files** te openen voor uw project. Vouw de map `aml_config` uit. 

2. Er zijn vooraf verschillende omgevingen geconfigureerd: **docker-python**, **docker-spark** en **local**. 

   Elke omgeving heeft twee bestanden, bijvoorbeeld `docker.compute` (voor zowel **docker-python** als **docker-spark**) en `docker-python.runconfig`. Open elk bestand om te zien dat er bepaalde opties zijn geconfigureerd in de teksteditor.  

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
   
   De Docker-engine voert de volgende taken uit:

    - Downloaden van de basisinstallatiekopie uit Azure.
    - Installeren van de Python-pakketten die zijn opgegeven in het bestand `conda_dependencies.yml`.
    - Starten van een Docker-container.
    - Kopiëren van of verwijzen naar (afhankelijk van de configuratie van de run) de lokale kopie van de projectmap.      
    - Uitvoeren van de het script `iris_sklearn.py`.

   Uiteindelijk moeten de resultaten er precies hetzelfde uitzien als voor de omgeving **local**.

5. Laten we nu Spark eens proberen. De Docker-basisinstallatiekopie bevat een vooraf geïnstalleerde en geconfigureerde Spark-instantie die u kunt gebruiken om een PySpak-script uit te voeren. Dit is een eenvoudige manier om een Apache Spark-programma te ontwikkelen en te testen zonder zelf tijd te hoeven besteden aan de installatie en configuratie van Apache Spark. 

   Open het `iris_spark.py`-bestand. Met dit script wordt het gegevensbestand `iris.csv` geladen, waarna het algoritme voor logistieke regressie uit de Spark Machine Learning-bibliotheek wordt gebruikt voor het classificeren van de Iris-gegevensset. Wijzig nu de uitvoeringsomgeving in **docker-spark**, het script in **iris_spark.py** en voer het script opnieuw uit. Dit proces duurt iets langer omdat er een Spark-sessie moet worden gemaakt en gestart binnen de Docker-container. U ziet ook dat de stdout anders is dan de stdout van `iris_spark.py`.

6. Begin nog een paar runs en experimenteer met verschillende argumenten. 

7. Open het bestand `iris_spark.py` om het logistieke regressiemodel te zien dat is gebouwd met behulp van de Spark Machine Learning-bibliotheek. 

8. Probeer wat dingen uit in het deelvenster **Jobs**, bekijk de uitvoeringsgeschiedenis en open een detailweergave van de runs die zijn uitgevoerd in verschillende omgevingen.

## <a name="run-scripts-in-the-cli-window"></a>Scripts uitvoeren in het CLI-venster

1. Start de Azure Machine Learning-opdrachtregelinterface (CLI):
   1. Start Azure Machine Learning Workbench.

   1. Selecteer in het menu Workbench **File** > **Open Command Prompt**. 
   
   De CLI-prompt start in de projectmap `C:\Temp\myIris\>` in Windows. Dit is het project dat u in Deel 1 van de zelfstudie hebt gemaakt.

   >[!IMPORTANT]
   >Om de volgende stappen uit te voeren, moet u dit CLI-venster gebruiken.

1. In het CLI-venster moet u zich aanmelden bij Azure. [Meer informatie over az-inloggen](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   U bent mogelijk al aangemeld. In dat geval kunt u deze stap overslaan.

   1. Voer in de opdrachtregel in:
      ```azurecli
      az login
      ```

      Deze opdracht retourneert een code om te gebruiken in uw browser bij https://aka.ms/devicelogin.

   1. Ga naar https://aka.ms/devicelogin in uw browser.

   1. Wanneer u daarom wordt gevraagd, typt u de code die u hebt ontvangen in de CLI in uw browser in.

   De Workbench-app en de CLI gebruiken onafhankelijke referentiecaches voor verificatie bij Azure-resources. Nadat u zich hebt aangemeld, hoeft u niet opnieuw te worden geverifieerd, totdat de token in cache verloopt. 

1. Als uw organisatie meerdere Azure-abonnementen (bedrijfsomgeving) heeft, moet u het abonnement instellen dat moet worden gebruikt. Zoek uw abonnement, stel het in met behulp van de abonnements-ID en test het vervolgens.

   1. Overzicht van elk Azure-abonnement waarvoor u toegang tot het gebruik van deze opdracht hebt:
   
      ```azurecli
      az account list -o table
      ```

      Gebruik de opdracht **az account list** om een lijst met abonnementen op te vragen die u kunt gebruiken voor aanmelding. 
      Als er meer dan één is, identificeert u de waarde van de abonnement-ID voor het abonnement dat u wilt gebruiken.

   1. Stel het Azure-abonnement dat u wilt gebruiken als het standaardaccount in:
   
      ```azurecli
      az account set -s <your-subscription-id>
      ```
      waar \<your subscription-id\> de ID-waarde is voor het abonnement dat u wilt gebruiken. Gebruik de haakjes niet.

   1. Bevestig de nieuwe abonnementsinstelling door de details voor het huidige abonnement aan te vragen. 

      ```azurecli
      az account show
      ```    

1. In het CLI-venster installeert u de Python-plottingbibliotheek **matplotlib**, als u deze bibliotheek nog niet hebt.

   ```azurecli
   pip install matplotlib
   ```

1. Verzend in het CLI-venster het script **iris_sklearn.py** als experiment.

   De uitvoering van iris_sklearn.py wordt uitgevoerd op de lokale compute-context.

   + In Windows:
     ```azurecli
     az ml experiment submit -c local .\iris_sklearn.py
     ```

   + In Mac OS:
     ```azurecli
     az ml experiment submit -c local iris_sklearn.py
     ```
   
   De uitvoer moet er ongeveer als volgt uitzien:
    ```text
    RunId: myIris_1521077190506
    
    Executing user inputs .....
    ===========================
    
    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]
    
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
    
    Execution Details
    =================
    RunId: myIris_1521077190506
    ```

1. Controleer de uitvoer. U krijgt dezelfde uitvoer en resultaten als toen u Workbench gebruikte voor het uitvoeren van het script. 

1. Voer in het CLI-venster het Python-script **iris_sklearn.py** uit, opnieuw met gebruik van een Docker-uitvoeringsomgeving (als u Docker op uw machine hebt geïnstalleerd).

   + Als uw container in Windows is: 
     |Uitvoering<br/>omgeving|Opdracht in Windows|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python .\iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark .\iris_spark.py 0.1`|

   + Als uw container in MacOS is: 
     |Uitvoering<br/>omgeving|Opdracht in Windows|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark iris_spark.py 0.1`|

1. Ga terug naar de Workbench en:
   1. Selecteer het pictogram van de map in het linkerdeelvenster voor een lijst met de projectbestanden.
   
   1. Open het Python-script met de naam **run.py**. Dit script is nuttig om verschillende regularisatiefrequenties te herhalen. 

   ![Ga terug naar Run list](media/tutorial-classifying-iris/2-runpy.png)

1. Voer het experiment meerdere keren met uit deze frequenties. 

   Met dit script wordt een taak `iris_sklearn.py` gestart met een regularisatiefrequentie van `10.0` (een heel groot aantal). Vervolgens wordt de frequentie in de volgende run gehalveerd, enzovoort, totdat de frequentie `0.005` is. 

   Het script bevat de volgende code:

   ![Ga terug naar Run list](media/tutorial-classifying-iris/2-runpy-code.png)

1. U kunt het script **run.py** vanaf de opdrachtregel als volgt uitvoeren:

   ```cmd
   python run.py
   ```

   Deze opdracht verzendt iris_sklearn.py meerdere keren met verschillende regularisatiefrequenties

   Wanneer `run.py` is voltooid, kunt u verschillende grafieken met metrische gegevens zien in de uitvoeringsgeschiedenis in Workbench.

## <a name="run-scripts-in-a-remote-docker-container"></a>Scripts uitvoeren in een externe Docker-container
Als u het script wilt uitvoeren in een Docker-container op een externe Linux-computer, moet u SSH-toegang (gebruikersnaam en wachtwoord) hebben tot die externe computer. Bovendien moet op de computer een Docker-engine zijn geïnstalleerd en worden uitgevoerd. De eenvoudigste manier om een dergelijke Linux-machine in te richten, is door een op Ubuntu gebaseerde DSVM (Data Science Virtual Machine) te maken in Azure. Leer [hoe u een Ubuntu-DSVM maakt die u in Azure ML Workbench kunt gebruiken](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal).

>[!NOTE] 
>De op CentOS gebaseerde DSVM wordt *niet* ondersteund.

1. Nadat de VM is gemaakt, kunt u deze als uitvoeringsomgeving koppelen door een set `.runconfig`- en `.compute`-bestanden te genereren. Gebruik de volgende opdracht om de bestanden te genereren. 

 Laten we het nieuwe rekendoel `myvm` noemen.
 
   ```azurecli
   az ml computetarget attach remotedocker --name myvm --address <your-IP> --username <your-username> --password <your-password>
   ```
   
   >[!NOTE]
   >Het IP-adres kan ook een vrij adresseerbare Fully Qualified Domain Name (FQDN) zijn, zoals `vm-name.southcentralus.cloudapp.azure.com`. We raden u aan om aan de DSVM een FQDN toe te voegen en deze te gebruiken in plaats van een IP-adres. Dit is nuttig omdat u de VM mogelijk op een later tijdstip wilt uitschakelen om kosten te besparen. Bovendien is het zo dat het IP-adres mogelijk is gewijzigd als u de VM de volgende keer start.

   >[!NOTE]
   >Naast verificatie met een gebruikersnaam en een wachtwoord kunt u ook een persoonlijke sleutel en een bijbehorende wachtwoordzin (indien van toepassing) opgeven. Gebruik hiervoor de opties `--private-key-file` en (optioneel) `--private-key-passphrase`.

   Bereid vervolgens het rekendoel **myvm** voor door deze opdracht uit te voeren.
   
   ```azurecli
   az ml experiment prepare -c myvm
   ```
   
   Voer vervolgens de volgende opdracht uit om de Docker-installatiekopie voor te bereiden in de VM zodat deze klaar is voor het uitvoeren van de scripts.
   
   >[!NOTE]
   >U kunt de waarde van `PrepareEnvironment` in `myvm.runconfig` ook wijzigen van de standaardwaarde `false` in `true`. Door deze wijziging wordt de Docker-container automatisch voorbereid als onderdeel van de eerste run.

2. Bewerk het gegenereerde bestand `myvm.runconfig` onder `aml_config` en wijzig het framework van de standaardwaarde `PySpark` in `Python`:

   ```yaml
   Framework: Python
   ```
   >[!NOTE]
   >PySpark werkt hier in principe ook, maar het is efficiënter om Python te gebruiken als u niet per se een Spark-sessie nodig hebt om uw Pyhton-script uit te voeren.

3. Gebruik dezelfde opdracht als eerder in het CLI-venster. Gebruik deze keer het doel _myvm_ om iris_sklearn.py in een externe Docker-container uit te voeren:
   ```azurecli
   az ml experiment submit -c myvm iris_sklearn.py
   ```
   De opdracht wordt uitgevoerd alsof u een `docker-python`-omgeving gebruikt, behalve dat de uitvoering wordt uitgevoerd op de externe Linux-VM. Het CLI-venster bevat dezelfde uitvoergegevens.

4. We gaan nu Spark gebruiken in de container. Open Verkenner. Maak een kopie van het bestand `myvm.runconfig` en wijzig de naam in `myvm-spark.runconfig`. Bewerk het nieuwe bestand om de instelling van `Framework` te wijzigen van `Python` in `PySpark`:
   ```yaml
   Framework: PySpark
   ```
   Laat het bestand `myvm.compute` ongewijzigd. Voor de uitvoering van Spark wordt dezelfde Docker-installatiekopie gebruikt op dezelfde VM. In het nieuwe bestand `myvm-spark.runconfig` verwijst het veld `Target` naar hetzelfde bestand `myvm.compute`, via de naam `myvm`.

5. Typ de volgende opdracht om het script **iris_spark.py** uit te voeren in de Spark-instantie die in de externe Docker-container wordt uitgevoerd:
   ```azureli
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="run-scripts-in-hdinsight-clusters"></a>Scripts uitvoeren in HDInsight-clusters
U kunt dit script ook uitvoeren in een HDInsight Spark-cluster. Leer [hoe u een HDInsight Spark-cluster maakt dat u kunt gebruiken in Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal).

>[!NOTE] 
>Het HDInsight-cluster moet Azure Blob als primaire opslag gebruiken. Het gebruik van Azure Data Lake-opslag wordt nog niet ondersteund.

1. Als u toegang hebt tot een Spark-cluster voor Azure HDInsight, genereert u een opdracht voor het uitvoeren van een HDInsight-configuratie zoals hieronder wordt weergegeven. Geef de naam op van het HDInsight-cluster, en uw HDInsight-gebruikersnaam en wachtwoord op als parameters. 

   Gebruik de volgende opdracht om een rekendoel te maken dat naar een HDInsight-cluster verwijst:

   ```azurecli
   az ml computetarget attach cluster --name myhdi --address <cluster head node FQDN> --username <your-username> --password <your-password>
   ```

   Om het HDInsight-cluster voor te bereiden, moet u deze opdracht uitvoeren:

   ```
   az ml experiment prepare -c myhdi
   ```

   De FQDN van het clusterhoofdknooppunt is meestal `<your_cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >De `username` is de SSH-gebruikersnaam van het cluster, gedefinieerd tijdens de installatie van HDInsight. De waarde is standaard `sshuser`. De waarde is niet `admin`, wat de andere gebruiker is die tijdens de installatie wordt gemaakt voor toegang tot de beheerwebsite van het cluster. 

2. Voer de volgende opdracht uit om het script **iris_spark.py** uit te voeren in het HDInsight-cluster:

   ```azurecli
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Wanneer u het script uitvoert in een extern HDInsight-cluster, kunt u ook de uitvoeringsdetails van de YARN-taak (Yet Another Resource Negotiator) bekijken op `https://<your_cluster_name>.azurehdinsight.net/yarnui`. Gebruik hiervoor het gebruikersaccount `admin`.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen
In dit tweede deel van de driedelige zelfstudiereeks hebt u het volgende geleerd:
> [!div class="checklist"]
> * Scripts openen en de code in de Workbench controleren
> * Scripts uitvoeren in een lokale omgeving
> * De uitvoeringsgeschiedenis controleren
> * Scripts uitvoeren in een lokale Docker-omgeving

U kunt nu het derde deel van deze zelfstudiereeks uitproberen, waarin u het logistieke regressiemodel kunt implementeren dat u als realtime webservice hebt gemaakt.

> [!div class="nextstepaction"]
> [Zelfstudie 3 - modellen implementeren](tutorial-classifying-iris-part-3.md)
