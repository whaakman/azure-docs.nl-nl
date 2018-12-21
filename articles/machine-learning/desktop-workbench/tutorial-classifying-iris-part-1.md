---
title: Gegevens voorbereiden voor de zelfstudie 'Classifying Iris' (Iris classificeren) in Machine Learning-service (preview) | Microsoft Docs
description: Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-service (preview) end-to-end gebruikt. Dit is deel één en hier wordt het voorbereiden van gegevens besproken.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs, gcampanella
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 03/07/2018
ROBOTS: NOINDEX
ms.openlocfilehash: dd10581888da64114debec40cba8564023033864
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53278505"
---
# <a name="tutorial-1-classify-iris---preparing-the-data"></a>Zelfstudie 1: Iris classificeren - gegevens voorbereiden

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is **deel één van een serie van drie**. In deze zelfstudie doorloopt u de basisprincipes van Azure Machine Learning-service (preview) en leert u het volgende:

> [!div class="checklist"]
> * Een project maken in Azure Machine Learning Workbench
> * Een gegevensvoorbereidingspakket maken
> * Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/Iris_flower_data_set) gebruikt. 

[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

U hebt het volgende nodig om deze zelfstudie te voltooien:
- Een Azure Machine Learning Experimenten-account
- Azure Machine Learning Workbench moet zijn geïnstalleerd

Als u deze vereisten nog niet hebt, volgt u de stappen in het artikel [Quick Start: Installeren en starten](quickstart-installation.md) om uw accounts in te stellen en de toepassing Azure Machine Learning Workbench te installeren. 

## <a name="create-a-new-project-in-workbench"></a>Een nieuw project maken in Workbench

Als u de stappen in het artikel [Quick Start: Installeren en starten](quickstart-installation.md) hebt gevolgd, hebt u dit project al gemaakt en kunt u doorgaan met de volgende sectie.

1. Open de app Azure Machine Learning Workbench en meld u indien nodig aan. 
   
   + In Windows kunt u de Workbench starten met de snelkoppeling **Machine Learning Workbench** op het bureaublad. 
   + Selecteer in macOS **Azure ML Workbench** in Launchpad.

1. Klik in het deelvenster **PROJECTS** op het plusteken (+) om **New Project** te kiezen.  

   ![Nieuwe werkruimte](./media/tutorial-classifying-iris/new_ws.png)

1. Vul de formuliervelden in en selecteer de knop **Maken** om een nieuw project te maken in Workbench.

   Veld|Aanbevolen waarde voor zelfstudie|Beschrijving
   ---|---|---
   Projectnaam | myIris |Voer een unieke naam in die uw account aanduidt. U kunt uw eigen naam gebruiken, maar ook de naam van een afdeling of project waarmee het experiment is verbonden. De naam moet minimaal 2 en maximaal 32 tekens lang zijn. De naam mag alleen alfanumerieke tekens en streepjes (-) bevatten. 
   Projectmap | c:\Temp\ | Selecteer de map waarin het project moet worden gemaakt.
   Projectbeschrijving | _leeg laten_ | Optioneel veld, nuttig voor het beschrijven van de projecten.
   URL van GIT-opslagplaats Visualstudio.com |_leeg laten_ | Optioneel veld. U kunt een project koppelen aan een Git-opslagplaats in Azure DevOps voor broncodebeheer en samenwerking. [Informatie over het instellen daarvan](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo). 
   Geselecteerde werkruimte | IrisGarden (indien aanwezig) | Kies een werkruimte die u hebt gemaakt voor uw Experimenten-account in Azure Portal. <br/>Als u de snelstart hebt gevolgd, hebt u een werkruimte met de naam IrisGarden. Selecteer anders de werkruimte die u hebt gemaakt bij het maken van uw Experimenten-account, of een andere werkruimte die u wilt gebruiken.
   Projectsjabloon | Iris classificeren | Sjablonen bevatten scripts en gegevens die u kunt gebruiken om het product te verkennen. Deze sjabloon bevat de scripts en gegevens die u nodig hebt voor deze snelstart en andere zelfstudies op deze documentatiesite. 

   ![Nieuw project](media/tutorial-classifying-iris/new_project.png)
 
 Er wordt een nieuw project gemaakt en het projectdashboard wordt geopend met dat project. U kunt nu de introductiepagina, gegevensbronnen, notitieblokken en broncodebestanden van het project verkennen. 

   ![Project openen](media/tutorial-classifying-iris/project-open.png)
 

## <a name="create-a-data-preparation-package"></a>Een gegevensvoorbereidingspakket maken

U kunt vervolgens de gegevens verkennen en beginnen met het voorbereiden van de gegevens in Azure Machine Learning Workbench. Elke transformatie die u uitvoert in Workbench wordt opgeslagen in een JSON-indeling in een lokaal gegevensvoorbereidingspakket (*.dprep-bestand). Dit gegevensvoorbereidingspakket is de primaire container voor de gegevensvoorbereidingswerkzaamheden in Workbench.

Dit pakket kan later worden overgedragen aan een runtime, zoals local-C#/CoreCLR, Scala/Spark of Scala/HDI. 

1. Selecteer het mappictogram om de bestandsweergave te openen en selecteer vervolgens **iris.csv** om dit bestand te openen.

   Dit bestand bevat een tabel met vijf kolommen en 50 rijen. Vier kolommen zijn kolommen voor numerieke functies. De vijfde kolom is een kolom voor een tekenreeksdoel. Geen van de kolommen heeft een kopnaam.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Het wordt afgeraden gegevensbestanden op te nemen in de projectmap, met name wanneer het bestand groot is. Omdat het gegevensbestand **iris.csv** klein is, is het in deze sjabloon opgenomen voor demonstratiedoeleinden. Raadpleeg voor meer informatie dit automatisch vertaalde artikel over [het lezen en schrijven van grote gegevensbestanden](how-to-read-write-files.md).

2. Klik in de **gegevensweergave** op het plusteken (**+**) om een nieuwe gegevensbron toe te voegen. De pagina **Add Data Source** (Gegevensbron toevoegen) wordt geopend. 

   ![Gegevensweergave in Azure Machine Learning Workbench](media/tutorial-classifying-iris/data_view.png)

3. Selecteer **Tekstbestanden(\*.csv, \*.json, \*.txt., ...)** en klik op **Volgende**.
   ![Gegevensbron in Azure Machine Learning Workbench](media/tutorial-classifying-iris/data-source.png)

4. Blader naar het bestand **iris.csv**, en klik op **Voltooien**. Hiermee worden standaardwaarden gebruikt voor parameters zoals het scheidingsteken en gegevenstypen.

   >[!IMPORTANT]
   >Zorg ervoor dat u voor deze oefening het bestand **iris.csv** vanuit de huidige projectmap selecteert, anders kunnen latere stappen mislukken.
 
   ![Iris selecteren](media/tutorial-classifying-iris/select_iris_csv.png)
   
5. Er wordt een nieuw bestand gemaakt met de naam **iris-1.dsource**. Het bestand krijgt een unieke naam eindigend op -1, omdat het voorbeeldproject al een bestand met de naam **iris.dsource** zonder nummer bevat.  

   Het bestand wordt geopend en de gegevens worden weergegeven. Er wordt automatisch een reeks kolomkoppen, van **Column1** tot **Column5**, toegevoegd aan deze gegevensset. Schuif naar de onderkant. U zult zien dat de laatste rij van de gegevensset leeg is. De rij is leeg omdat er een extra regeleinde in het CSV-bestand staat.

   ![Gegevensweergave iris](media/tutorial-classifying-iris/iris_data_view.png)

1. Selecteer de knop **Metrics** (Metrische gegevens). Er worden histogrammen gegenereerd en weergegeven.

   U kunt terugschakelen naar de gegevensweergave door de knop **Gegevens** te selecteren.
   
   ![Gegevensweergave iris](media/tutorial-classifying-iris/iris_data_view_metrics.png)

1. Bekijk de histogrammen. Er is voor elke kolom een volledige set statistieken berekend. 

   ![Gegevensweergave iris](media/tutorial-classifying-iris/iris_metrics_view.png)

8. Maak eerst een gegevensvoorbereidingspakket door de knop **Prepare** te selecteren. Het **gelijknamige** dialoogvenster wordt geopend. 

   Het voorbeeldproject bevat het gegevensvoorbereidingsbestand **iris.dprep** dat standaard is geselecteerd. 

   ![Gegevensweergave iris](media/tutorial-classifying-iris/prepare.png)

1. Maak een nieuw gegevensvoorbereidingspakket maken door **Nieuw gegevensvoorbereidingspakket** te selecteren in de vervolgkeuzelijst.

   ![Gegevensweergave iris](media/tutorial-classifying-iris/prepare_new.png)

1. Voer een nieuwe waarde in voor de naam van het pakket (gebruik daarvoor **iris-1**) en selecteer **OK**.

   Er wordt een nieuw pakket voor gegevensvoorbereiding gemaakt met de naam **iris-1.dprep** en geopend in de editor voor gegevensvoorbereiding.

   ![Gegevensweergave iris](media/tutorial-classifying-iris/prepare_iris-1.png)

   Nu gaan we enkele eenvoudige stappen uitvoeren om de gegevens voor te bereiden. 

1. Selecteer elke kolomkop om de koptekst te bewerken. Wijzig de naam van elke kolom als volgt: 

   Geef voor de vijf kolommen respectievelijk **Sepal Length**, **Sepal Width**, **Petal Length**, **Petal Width** en **Species** (in deze volgorde) op.

   ![Kolomnamen wijzigen](media/tutorial-classifying-iris/rename_column.png)

1. Unieke waarden tellen:
   1. Selecteer de kolom **Species**
   1. Klik met de rechtermuisknop om deze te selecteren. 
   1. Selecteer **Value Counts** (Aantallen waarden) in de vervolgkeuzelijst. 

   Het deelvenster **Inspectors** wordt geopend onder de gegevens. Een histogram met vier balken wordt weergegeven. De doelkolom bevat vier afzonderlijke waarden: **Iris-virginica**, **Iris-versicolor**, **Iris-setosa** en een **(null)**-waarde.

   ![Selecteer Value Counts](media/tutorial-classifying-iris/value_count.png)

   ![Histogram met aantal waarden](media/tutorial-classifying-iris/filter_out.png)

1. Als u de null-waarden wilt filteren, selecteert u de balk "(null)" en selecteert u het minteken (**-**). 

   De rij (null) wordt vervolgens grijs weergegeven om aan te geven dat deze is gefilterd. 

   ![Null-waarden filteren](media/tutorial-classifying-iris/filter_out2.png)

1. Bekijk de afzonderlijke gegevensvoorbereidingsstappen die worden beschreven in het deelvenster **STEPS**. De acties voor het wijzigen van de kolomnamen en het wegfilteren van de rijen met null-waarden zijn vastgelegd als stappen voor het voorbereiden van gegevens. U kunt afzonderlijke stappen aanpassen om de instellingen ervan te wijzigen, de volgorde van de stappen veranderen en zelfs stappen verwijderen.

   ![Stappen](media/tutorial-classifying-iris/steps.png)

1. Sluit de editor voor gegevensvoorbereiding. Selecteer het pictogram **x** op het tabblad **iris-1** bij het grafiekpictogram om het tabblad te sluiten. Uw werk wordt automatisch opgeslagen in het bestand **iris 1.dprep** dat wordt weergegeven onder de kop **Data Preparations** (Gegevensvoorbereiding).

   ![Sluiten](media/tutorial-classifying-iris/close.png)

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

 De uitvoer van een gegevensvoorbereidingspakket kan rechtstreeks in Python of in een Jupyter Notebook worden gecontroleerd. De pakketten kunnen worden uitgevoerd op meerdere runtimes waaronder lokale Python, Spark (inclusief in Docker) en HDInsight. 

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

   Afhankelijk van de context waarin deze code wordt uitgevoerd, vertegenwoordigt `df` een ander soort gegevensframe:
   + Bij het uitvoeren in een Python-runtime wordt een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) gebruikt.
   + Bij het uitvoeren in een Spark-context wordt een [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) gebruikt. 
   
   Voor meer informatie over het voorbereiden van gegevens in Azure Machine Learning Workbench verwijzen we naar de handleiding [Aan de slag met gegevensvoorbereiding](data-prep-getting-started.md).

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
