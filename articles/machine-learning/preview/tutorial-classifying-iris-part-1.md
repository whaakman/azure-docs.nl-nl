---
title: Gegevens voorbereiden voor de zelfstudie 'Classifying Iris' (Iris classificeren) in Machine Learning-services (preview) | Microsoft Docs
description: "Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel één en hier wordt het voorbereiden van gegevens besproken."
services: machine-learning
author: hning86
ms.author: haining, j-martens
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 02/28/2018
ms.openlocfilehash: 0bef557ee1394e3c786fd2c54e821b5dea28fabf
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-classify-iris-part-1---preparing-the-data"></a>Zelfstudie: classificeren van Iris deel 1: gegevens voorbereiden

Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is deel één van een serie van drie. In deze zelfstudie doorlopen we de basisprincipes van Machine Learning-services (preview). Procedures voor:

> [!div class="checklist"]
> * Een project maken in Azure Machine Learning Workbench
> * Een gegevensvoorbereidingspakket maken
> * Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/Iris_flower_data_set) gebruikt. De schermafbeeldingen zijn specifiek voor Windows, maar de ervaring voor macOS is bijna identiek.

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

U hebt het volgende nodig om deze zelfstudie te voltooien:
- Een Azure Machine Learning Experimenten-account
- Azure Machine Learning Workbench moet zijn geïnstalleerd

Als u nog geen account hebt en Workbench nog niet is geïnstalleerd, volgt u de stappen in het artikel [Quickstart: Install and start](quickstart-installation.md) (Snelstart: installeren en starten) om dit account in te stellen en de toepassing Azure Machine Learning Workbench te installeren. 

## <a name="create-a-new-project-in-workbench"></a>Een nieuw project maken in Workbench

Als u de stappen in het artikel [Quickstart: Install and start](quickstart-installation.md) (Snelstart: installeren en starten) hebt gevolgd, hebt u dit project al gemaakt en kunt u doorgaan met de volgende sectie.

1. Open de app Azure Machine Learning Workbench en meld u indien nodig aan. 
   
   + In Windows kunt u de Workbench starten met de snelkoppeling **Machine Learning Workbench** op het bureaublad. 
   + Selecteer in macOS **Azure ML Workbench** in Launchpad.

1. Klik in het deelvenster **PROJECTS** op het plusteken (+) om **New Project** te kiezen.  

   ![Nieuwe werkruimte](media/tutorial-classifying-iris/new_ws.png)

1. Vul de formuliervelden in en selecteer de knop **Maken** om een nieuw project te maken in Workbench.

   Veld|Aanbevolen waarde voor zelfstudie|Beschrijving
   ---|---|---
   Projectnaam | myIris |Voer een unieke naam in die uw account aanduidt. U kunt uw eigen naam gebruiken, maar ook de naam van een afdeling of project waarmee het experiment is verbonden. De naam moet minimaal 2 en maximaal 32 tekens lang zijn. De naam mag alleen alfanumerieke tekens en streepjes (-) bevatten. 
   Projectmap | c:\Temp\ | Selecteer de map waarin het project moet worden gemaakt.
   Projectbeschrijving | _leeg laten_ | Optioneel veld, nuttig voor het beschrijven van de projecten.
   Visualstudio.com |_leeg laten_ | Optioneel veld. Een project kan eventueel worden gekoppeld aan een Git-opslagplaats in Visual Studio Team Services voor broncodebeheer en samenwerking. [Informatie over het instellen daarvan](https://docs.microsoft.com/en-us/azure/machine-learning/preview/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo). 
   Werkruimte | IrisGarden (indien aanwezig) | Kies een werkruimte die u hebt gemaakt voor uw Experimenten-account in Azure Portal. <br/>Als u de snelstart hebt gevolgd, hebt u een werkruimte met de naam IrisGarden. Selecteer anders de werkruimte die u hebt gemaakt bij het maken van uw Experimenten-account, of een andere werkruimte die u wilt gebruiken.
   Projectsjabloon | Iris classificeren | Sjablonen bevatten scripts en gegevens die u kunt gebruiken om het product te verkennen. Deze sjabloon bevat de scripts en gegevens die u nodig hebt voor deze snelstart en andere zelfstudies op deze documentatiesite. 

   ![Nieuw project](media/tutorial-classifying-iris/new_project.png)
 
 Er wordt een nieuw project gemaakt en het projectdashboard wordt geopend met dat project. U kunt nu de introductiepagina, gegevensbronnen, notitieblokken en broncodebestanden van het project verkennen. 

## <a name="create-a-data-preparation-package"></a>Een gegevensvoorbereidingspakket maken

In dit gedeelte van de zelfstudie verkent u de gegevens en start u het gegevensvoorbereidingsproces. Wanneer u de gegevens in Azure Machine Learning Workbench voorbereidt, wordt een JSON-weergave van de transformaties die u in Workbench uitvoert, opgeslagen in een lokaal gegevensvoorbereidingspakket (*.dprep-bestand). Dit gegevensvoorbereidingspakket is de primaire container voor de gegevensvoorbereidingswerkzaamheden in Workbench.

Dit pakket kan voor uitvoering worden overgedragen aan een runtime, zoals local-C#/CoreCLR, Scala/Spark of Scala/HDI, waar code wordt gegenereerd voor de juiste runtime voor uitvoering. 

1. Selecteer het mappictogram om de bestandsweergave te openen en selecteer vervolgens **iris.csv** om dit bestand te openen.  

   Het bestand is een tabel met vijf kolommen en 150 rijen. De tabel bevat vier numerieke parameterkolommen en een tekenreeks-doelkolom. Er zijn geen kolomkoppen.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Het wordt afgeraden gegevensbestanden op te nemen in de projectmap, met name wanneer het bestand groot is. We nemen **iris.csv** in deze sjabloon op voor demonstratiedoeleinden, omdat dit bestand zeer klein is. Raadpleeg voor meer informatie dit automatisch vertaalde artikel over [het lezen en schrijven van grote gegevensbestanden](how-to-read-write-files.md).

2. Klik in de **gegevensweergave** op het plusteken (**+**) om een nieuwe gegevensbron toe te voegen. De pagina **Add Data Source** (Gegevensbron toevoegen) wordt geopend. 

   ![Gegevensweergave](media/tutorial-classifying-iris/data_view.png)

3. Selecteer **Tekstbestanden (*.csv, .json, .txt.,...)**  en klik op **Volgende**.
   ![Gegevensbron](media/tutorial-classifying-iris/data-source.png)
   

4. Blader naar het bestand **iris.csv**, en klik op **Volgende**.  
 
   ![Iris selecteren](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Zorg ervoor dat u voor deze oefening het bestand **iris.csv** vanuit de huidige projectmap selecteert, anders kunnen latere stappen mislukken.
   
5. Laat de standaardwaarden ongewijzigd en klik op **Voltooien**.

6. Er wordt een nieuw bestand gemaakt met de naam **iris-1.dsource**. Het bestand krijgt een unieke naam eindigend op -1, omdat het voorbeeldproject al een bestand met de naam **iris.dsource** zonder nummer bevat.  

   Het bestand wordt geopend en de gegevens worden weergegeven. Er wordt automatisch een reeks kolomkoppen, van **Column1** tot **Column5**, toegevoegd aan deze gegevensset. Schuif naar de onderkant. U zult zien dat de laatste rij van de gegevensset leeg is. De rij is leeg omdat er een extra regeleinde in het CSV-bestand staat.

   ![Gegevensweergave iris](media/tutorial-classifying-iris/iris_data_view.png)

7. Selecteer de knop **Metrics** (Metrische gegevens). Bekijk de histogrammen. Er is voor elke kolom een volledige set statistieken berekend. U kunt ook de knop **Data** (Gegevens) selecteren om de gegevens opnieuw weer te geven. 

   ![Gegevensweergave iris](media/tutorial-classifying-iris/iris_metrics_view.png)

8. Selecteer de knop **Prepare** (Voorbereiden). Het **gelijknamige** dialoogvenster wordt geopend. 

   Het voorbeeldproject bevat een bestand **iris.dprep**. Standaard wordt u gevraagd om een nieuwe gegevensstroom te maken in het gegevensvoorbereidingspakket **iris.dprep** dat al bestaat. 

   Selecteer **+ New Data Preparation Package** (+ Nieuw gegevensvoorbereidingspakket) in de vervolgkeuzelijst, geef een nieuwe naam op voor het pakket, gebruik **iris 1**, en selecteer vervolgens **OK**.

   ![Gegevensweergave iris](media/tutorial-classifying-iris/new_dprep.png)

   Er wordt een nieuw pakket voor gegevensvoorbereiding gemaakt met de naam **iris-1.dprep** en geopend in de editor voor gegevensvoorbereiding.

9. Nu gaan we enkele eenvoudige stappen uitvoeren om de gegevens voor te bereiden. Selecteer elke kolomkop om de koptekst te bewerken. Wijzig de naam van elke naam als volgt: 

   Geef voor de vijf kolommen respectievelijk **Sepal Length**, **Sepal Width**, **Petal Length**, **Petal Width** en **Species** (in deze volgorde) op.

   ![Kolomnamen wijzigen](media/tutorial-classifying-iris/rename_column.png)

10. Als u afzonderlijke waarden wilt tellen, selecteert u de kolom **Species** en klikt u er met de rechtermuisknop op. Selecteer **Value Counts** (Aantallen waarden) in de vervolgkeuzelijst. 

   Met deze actie wordt het deelvenster **Inspectors** onder de gegevens geopend. Een histogram met vier balken wordt weergegeven. U ziet dat de doelkolom drie afzonderlijke waarden heeft: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** en een **(null)**-waarde.

   ![Selecteer Value Counts](media/tutorial-classifying-iris/value_count.png)

11. Als u de null-waarden wilt filteren, selecteert u het label Null en selecteert u het minteken (**-**). De rij Null wordt vervolgens grijs weergegeven om aan te geven dat deze is gefilterd. 

   ![Histogram met aantal waarden](media/tutorial-classifying-iris/filter_out.png)

12. Let op de afzonderlijke stappen die worden beschreven in het deelvenster **STEPS**. De acties voor het wijzigen van de kolomnamen en het wegfilteren van de rijen met null-waarden zijn vastgelegd als stappen voor het voorbereiden van gegevens. U kunt afzonderlijke stappen aanpassen om de instellingen te wijzigen, de volgorde van de stappen veranderen en zelfs stappen verwijderen.

   ![Stappen](media/tutorial-classifying-iris/steps.png)

13. Sluit de editor voor gegevensvoorbereiding. Selecteer **Close** (x) op het tabblad **iris 1** met het grafiekpictogram. Uw werk wordt automatisch opgeslagen in het bestand **iris 1.dprep** dat wordt weergegeven onder de kop **Data Preparations** (Gegevensvoorbereiding).

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

<!-- The output/results of a Package can be explored in Python or via a Jupyter Notebook. A Package can be executed across multiple runtimes including local Python, Spark (including in Docker), and HDInsight. A Package contains one or more Dataflows that are the steps and transforms applied to the data. A Package may use another Package as a Data Source (referred to as a Reference Data Flow). -->

1. Zoek het bestand **iris 1.dprep** op het tabblad Data Preparations.

1. Klik met de rechtermuisknop op het bestand **iris 1.dprep** en selecteer **Generate Data Access Code File** in het contextmenu. 

   ![Code genereren](media/tutorial-classifying-iris/generate_code.png)

   Een nieuw bestand met de naam **iris 1.py** wordt geopend met de volgende regels met code om de logica op te roepen die u als een gegevensvoorbereidingspakket hebt gemaakt:

   ```python
   # Use the Azure Machine Learning data preparation package
   from azureml.dataprep import package

   # Use the Azure Machine Learning data collector to log various metrics
   from azureml.logging import get_azureml_logger
   logger = get_azureml_logger()

   # This call will load the referenced package and return a DataFrame.
   # If run in a PySpark environment, this call returns a
   # Spark DataFrame. If not, it will return a Pandas DataFrame.
   df = package.run('iris-1.dprep', dataflow_idx=0)

   # Remove this line and add code that uses the DataFrame
   df.head(10)
   ```

   Afhankelijk van de context waarin deze code wordt uitgevoerd, vertegenwoordigt `df` verschillende soorten gegevensframes. Een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) wordt gebruikt bij uitvoering in Python-runtime, of een [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) bij uitvoering in een Spark-context. 
   
   Voor informatie over het voorbereiden van gegevens in Azure Machine Learning Workbench verwijzen we naar de handleiding [Getting Started with Data Preparation](data-prep-getting-started.md) (Aan de slag met gegevensvoorbereiding).

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure Machine Learning Workbench gebruikt om de volgende bewerkingen uit te voeren:
> [!div class="checklist"]
> * Een nieuw project maken
> * Een gegevensvoorbereidingspakket maken
> * Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

U bent klaar om door te gaan met het volgende deel in de zelfstudiereeks, waar u leert hoe u een Azure Machine Learning-model bouwt.
> [!div class="nextstepaction"]
> [Zelfstudie 2: modellen bouwen](tutorial-classifying-iris-part-2.md)
