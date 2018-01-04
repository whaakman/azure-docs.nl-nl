---
title: Gegevens voorbereiden voor de zelfstudie 'Classifying Iris' (Iris classificeren) in Machine Learning-services (preview) | Microsoft Docs
description: "Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel één en hier wordt het voorbereiden van gegevens besproken."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 09/28/2017
ms.openlocfilehash: f417154c2c2a27b356cefb94739838bd2136e756
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="classify-iris-part-1-prepare-the-data"></a>Classifying Iris deel 1: gegevens voorbereiden
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is deel één van een serie van drie. In deze zelfstudie doorlopen we de basisprincipes van Machine Learning-services (preview). Procedures voor:
> [!div class="checklist"]
> * Een project maken in Azure Machine Learning Workbench.
> * Een gegevensvoorbereidingspakket maken.
> * Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket.

In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/Iris_flower_data_set) gebruikt. De schermopnamen zijn specifiek voor Windows, maar de Mac OS-versie is bijna identiek.

## <a name="prerequisites"></a>Vereisten
- Een Azure Machine Learning Experimenten-account maken.
- Azure Machine Learning Workbench installeren.

Volg de instructies in [dit artikel](quickstart-installation.md) om de toepassing Azure Machine Learning Workbench te installeren. Deze installatie omvat ook het platformoverschrijdende opdrachtregelprogramma van Azure, te weten Azure CLI.

## <a name="create-a-new-project-in-azure-machine-learning-workbench"></a>Een nieuw project maken in Azure Machine Learning Workbench
1. Open de app Azure Machine Learning Workbench en meld u indien nodig aan. Klik in het deelvenster **PROJECTS** (PROJECTEN) op het plusteken (**+**) om **New Project** (Nieuw project) te kiezen.

   ![Nieuwe werkruimte](media/tutorial-classifying-iris/new_ws.png)

2. Vul de gegevens in bij **Create New Project** (Nieuw project maken): 

   ![Nieuw project](media/tutorial-classifying-iris/new_project.png)

   - Vul in het veld **Project name** (Projectnaam) een naam in voor het project. Gebruik bijvoorbeeld de waarde **myIris**.
   - Selecteer bij **Project directory** (Projectmap) de map waarin het project moet worden gemaakt. Gebruik bijvoorbeeld de waarde `C:\Temp\`. 
   - Typ eventueel beschrijvende tekst bij **Projectbeschrijving**. 
   - Het veld voor de **Git repository** is ook optioneel en kan leeg worden gelaten. U kunt een bestaande, lege Git-repo (een repo zonder master branch) in Visual Studio Team Services opgeven. Als u een Git-repository gebruikt die al bestaat, kunt u later de scenario's voor roaming en delen inschakelen. Zie [Use Git repo](using-git-ml-project.md) (Git-repo gebruiken) voor meer informatie. 
   - Selecteer bij **Workspace** een werkruimte. In deze zelfstudie wordt bijvoorbeeld **IrisGarden** gebruikt. 
   - Selecteer de sjabloon **Classifying Iris** (Iris classificeren) in de lijst met projectsjablonen. 

3. Selecteer de knop **Create** (Maken). Het project wordt voor u gemaakt en geopend.

## <a name="create-a-data-preparation-package"></a>Een gegevensvoorbereidingspakket maken
1. Open het bestand **iris.csv** vanuit de **bestandsweergave**. Het bestand is een tabel met vijf kolommen en 150 rijen. De tabel bevat vier numerieke parameterkolommen en een tekenreeks-doelkolom. Er zijn geen kolomkoppen.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Het wordt afgeraden gegevensbestanden op te nemen in de projectmap, met name wanneer het bestand groot is. We nemen **iris.csv** in deze sjabloon op voor demonstratiedoeleinden, omdat dit bestand zeer klein is. Raadpleeg voor meer informatie dit automatisch vertaalde artikel over [het lezen en schrijven van grote gegevensbestanden](how-to-read-write-files.md).

2. Klik in de **gegevensweergave** op het plusteken (**+**) om een nieuwe gegevensbron toe te voegen. De pagina **Add Data Source** (Gegevensbron toevoegen) wordt geopend. 

   ![Gegevensweergave](media/tutorial-classifying-iris/data_view.png)

3. Laat de standaardwaarden staan en selecteer de knop **Next** (Volgende).  
 
   ![Iris selecteren](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Zorg ervoor dat u voor deze oefening het bestand **iris.csv** vanuit de huidige projectmap selecteert, anders kunnen latere stappen mislukken.
   
4. Nadat u het bestand hebt geselecteerd, selecteert u de knop **Voltooien**.

4. Er wordt een nieuw bestand gemaakt met de naam **iris-1.dsource**. Het bestand krijgt een unieke naam door het toevoegen van '-1', omdat het voorbeeldproject al een bestand met de naam **iris.dsource** zonder nummer bevat.  

   Het bestand wordt geopend en de gegevens worden weergegeven. Er wordt automatisch een reeks kolomkoppen, van **Column1** tot **Column5**, toegevoegd aan deze gegevensset. Schuif naar de onderkant. U zult zien dat de laatste rij van de gegevensset leeg is. De rij is leeg omdat er een extra regeleinde in het CSV-bestand staat.

   ![Gegevensweergave iris](media/tutorial-classifying-iris/iris_data_view.png)

5. Selecteer de knop **Metrics** (Metrische gegevens). Bekijk de histogrammen. Er is voor elke kolom een volledige set statistieken berekend. U kunt ook de knop **Data** (Gegevens) selecteren om de gegevens opnieuw weer te geven. 

   ![Gegevensweergave iris](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Selecteer de knop **Prepare** (Voorbereiden). Het **gelijknamige** dialoogvenster wordt geopend. 

   Het voorbeeldproject bevat een bestand **iris.dprep**. Standaard wordt u gevraagd om een nieuwe gegevensstroom te maken in het gegevensvoorbereidingspakket **iris.dprep** dat al bestaat. 

   Selecteer **+ New Data Preparation Package** (+ Nieuw gegevensvoorbereidingspakket) in de vervolgkeuzelijst, geef een nieuwe naam op voor het pakket, gebruik **iris 1**, en selecteer vervolgens **OK**.

   ![Gegevensweergave iris](media/tutorial-classifying-iris/new_dprep.png)

   Er wordt een nieuw pakket voor gegevensvoorbereiding gemaakt met de naam **iris-1.dprep** en geopend in de editor voor gegevensvoorbereiding.

7. Nu gaan we enkele eenvoudige stappen uitvoeren om de gegevens voor te bereiden. Wijzig de kolomnamen. Selecteer elke kolomkop om de koptekst te bewerken. 

   Geef voor de vijf kolommen respectievelijk **Petal Length**, **Petal Width**, **Petal Length**, **Petal Width** en **Species** op.

   ![Kolomnamen wijzigen](media/tutorial-classifying-iris/rename_column.png)

8. Als u afzonderlijke waarden wilt tellen, selecteert u de kolom **Species** en klikt u er met de rechtermuisknop op. Selecteer **Value Counts** (Aantallen waarden) in de vervolgkeuzelijst. 

   ![Selecteer Value Counts](media/tutorial-classifying-iris/value_count.png)

   Hiermee opent u het deelvenster **Inspectors** en ziet u een histogram met vier balken. U ziet dat de doelkolom drie afzonderlijke waarden heeft: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** en een **(null)**-waarde.

9. Null-waarden kunt u wegfilteren door de balk in de grafiek te selecteren die de waarde null vertegenwoordigt. Er is één rij met een **(null)**-waarde. Als u deze rij wilt verwijderen, selecteert u het minteken (**-**).

   ![Histogram met aantal waarden](media/tutorial-classifying-iris/filter_out.png)

10. Let op de afzonderlijke stappen die worden beschreven in het deelvenster **STEPS**. De acties voor het wijzigen van de kolomnamen en het wegfilteren van de rijen met null-waarden zijn vastgelegd als stappen voor het voorbereiden van gegevens. U kunt afzonderlijke stappen aanpassen om de instellingen te wijzigen, de volgorde van de stappen veranderen en zelfs stappen verwijderen.

   ![Stappen](media/tutorial-classifying-iris/steps.png)

11. Sluit de editor voor gegevensvoorbereiding. Selecteer **Close** (x) op het tabblad **iris 1** met het grafiekpictogram. Uw werk wordt automatisch opgeslagen in het bestand **iris 1.dprep** dat wordt weergegeven onder de kop **Data Preparations** (Gegevensvoorbereiding).

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

1. Klik met de rechtermuisknop op het bestand **iris 1.dprep** om het contextmenu weer te geven en selecteer **Generate Data Access Code File** (Toegangscode voor gegevenstoegang genereren). 

   ![Code genereren](media/tutorial-classifying-iris/generate_code.png)

2. Er wordt een nieuw bestand met de naam **iris-1.py** geopend met daarin de volgende regels code:

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

   Dit codefragment roept de logica aan die u hebt gemaakt als gegevensvoorbereidingspakket. Afhankelijk van de context waarin deze code wordt uitgevoerd, vertegenwoordigt `df` verschillende soorten gegevensframes. Een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) wordt gebruikt bij uitvoering in Python-runtime, of een [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) bij uitvoering in een Spark-context. 

   Voor meer informatie over het voorbereiden van gegevens in Azure Machine Learning Workbench verwijzen we naar de handleiding [Getting Started with Data Preparation](data-prep-getting-started.md) (Aan de slag met gegevensvoorbereiding).

## <a name="next-steps"></a>Volgende stappen
In dit eerste deel van deze zelfstudie hebt u geleerd hoe u Azure Machine Learning Workbench kunt gebruiken voor deze bewerkingen:
> [!div class="checklist"]
> * Een nieuw project maken. 
> * Een gegevensvoorbereidingspakket maken.
> * Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket.

U bent klaar om door te gaan met het volgende deel in de reeks, waar u leert hoe u een Azure Machine Learning-model bouwt.
> [!div class="nextstepaction"]
> [Een model bouwen](tutorial-classifying-iris-part-2.md)
