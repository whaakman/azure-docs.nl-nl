---
title: Gegevens voorbereiden voor de zelfstudie 'Classifying Iris' (Iris classificeren) in Machine Learning-services (preview) | Microsoft Docs
description: Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel 1 over het voorbereiden van gegevens.
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e88b6bf74b7492353e5d5d004bde12fa4787e405
ms.contentlocale: nl-nl
ms.lasthandoff: 09/25/2017

---

# <a name="classifying-iris-part-1-prepare-data"></a>Classifying Iris deel 1: gegevens voorbereiden
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is deel één van een serie van drie. In deze zelfstudie doorlopen we de basisprincipes van Machine Learning-services (preview). In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een project maken in de Azure Machine Learning Workbench
> * Een gegevensvoorbereidingspakket maken
> * Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/Iris_flower_data_set) gebruikt om dingen overzichtelijk te houden. De schermafbeeldingen zijn specifiek voor Windows, maar de ervaring voor macOS is bijna identiek.

## <a name="launch-azure-machine-learning-workbench"></a>Azure Machine Learning Workbench starten
Volg de [Snelstartgids Installeren en maken](quickstart-installation.md) om de Azure Machine Learning Workbench-toepassing te installeren, die ook de opdrachtregelinterface (CLI: command-line interface) bevat. Start de app Azure Machine Learning Workbench en meld u indien nodig aan.

## <a name="create-a-new-project"></a>Een nieuw project maken
1. Klik in het deelvenster **PROJECTS** (PROJECTEN) op het pictogram **+** om een **New Project** (Nieuw project) te maken.

   ![nieuwe werkruimte](media/tutorial-classifying-iris/new_ws.png)

2. Vul de gegevens in bij **Create New Project** (Nieuw project maken). 

   ![Nieuw project](media/tutorial-classifying-iris/new_project.png)

   - Vul in het veld **Project name** (Projectnaam) een naam voor het project in. Gebruik bijvoorbeeld de waarde **myIris**.
   - Kies de **Project directory** (Projectmap) waarin het project moet worden gemaakt. Bijvoorbeeld **C:\Temp**. 
   - Voer de **Project description** (Projectbeschrijving) in. 
   - Het veld voor de **Git repository** veld is optioneel en kan leeg worden gelaten. U kunt een bestaande lege Git-repo (een repo zonder master branch) op VSTS opgeven. Hierdoor kunt u later Roaming and Sharing-scenario's inschakelen. Raadpleeg voor meer informatie het artikel [Using Git repo](using-git-ml-project.md) (Git-repo gebruiken). 
   - Kies een **Workspace** (Werkruimte). In deze zelfstudie wordt bijvoorbeeld **IrisGarden** gebruikt. 
   - Selecteer de sjabloon **Classifying Iris** (Iris classificeren) in de lijst met projectsjablonen. 

3. Klik op de knop **Create** (Maken) om het project te maken. Het project wordt voor u gemaakt en geopend.

## <a name="create-a-data-preparation-package"></a>Een gegevensvoorbereidingspakket maken
1. Open het bestand `iris.csv` vanuit de **bestandsweergave**. Het bestand is een eenvoudige tabel met 5 kolommen en 150 rijen. De tabel bevat vier numerieke parameterkolommen en een tekenreeks-doelkolom. Er zijn geen kolomkoppen.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   >Opmerking: het wordt afgeraden gegevensbestanden op te nemen in de projectmap, vooral wanneer het bestand groot is. We nemen `iris.csv` in deze sjabloon op voor demonstratiedoeleinden, omdat dit bestand zeer klein is. Raadpleeg voor meer informatie dit Engelstalige artikel over [het lezen en schrijven van grote gegevensbestanden](how-to-read-write-files.md).

2. Klik in de **gegevensweergave** op het pictogram **+** om een nieuwe gegevensbron toe te voegen. De wizard **Add Data Source** (Gegevensbron toevoegen) wordt gestart. 

   ![gegevensweergave](media/tutorial-classifying-iris/data_view.png)

3. Selecteer de optie **File(s)/Directory** (Bestand(en)/Map) en kies het lokale bestand `iris.csv`. Accepteer de standaardinstellingen voor elk scherm en klik tot slot op **Finish** (Voltooien). 

   ![iris selecteren](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Zorg ervoor dat u voor deze oefening het bestand `iris.csv` vanuit de huidige projectmap selecteert, anders kunnen latere stappen mislukken. 

4. Er wordt een nieuw bestand gemaakt, met de naam `iris-1.dsource`. Het bestand krijgt een unieke naam met '-1', omdat het voorbeeldproject al een bestand met de naam `iris.dsource` zonder nummer bevat.  Het bestand wordt geopend en de gegevens worden weergegeven. Er wordt automatisch een reeks kolomkoppen van `Column1` tot `Column5` toegevoegd aan de gegevensset. Schuif naar de onderkant. U zult zien dat de laatste rij van de gegevensset leeg is. Dat komt door een extra regeleinde in het CSV-bestand.

   ![gegevensweergave iris](media/tutorial-classifying-iris/iris_data_view.png)

5. Klik nu op de knop **Metrics** (Metrische gegevens). Bekijk de histogrammen en een volledige set statistieken die voor elke kolom worden berekend. U kunt ook op de knop **Data** (Gegevens) klikken om de gegevens opnieuw weer te geven. 

   ![gegevensweergave iris](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Klik op de knop **Prepare** (Voorbereiden) naast de knop **Metrics** (of de knop **Data** als u zich in de weergave met metrische gegevens bevindt). Het dialoogvenster **Prepare** (Voorbereiden) wordt weergegeven. Het voorbeeldproject bevat al het bestand `iris.dprep`, dus wordt u standaard gevraagd een nieuwe gegevensstroom te maken in dat gegevensvoorbereidingspakket **iris.dprep**. Wijzig de waarde van de vervolgkeuzelijst in **+New Data Preparation Package** en voer de nieuwe waarde "iris-1" in. Klik op **OK**.

   ![gegevensweergave iris](media/tutorial-classifying-iris/new_dprep.png)

Er wordt een nieuw gegevensvoorbereidingspakket met de naam `iris-1.dprep` gemaakt en geopend in de gevensvoorbereidingseditor.

Nu gaan we wat eenvoudige gegevensvoorbereiding doen. Wijzig de kolomnamen door op elke kolomkop te klikken en de koptekst bewerkbaar te maken. Voer `Sepal Length` (Lengte kelkblad), `Sepal Width` (Breedte kelkblad), `Petal Length` (Lengte kroonblad), `Petal Width` (Breedte kroonblad) en `Species` (Soort) in als kop voor de vijf kolommen.

![namen van kolommen wijzigen](media/tutorial-classifying-iris/rename_column.png)

Selecteer de kolom `Species` en klik erop met de rechtermuisknop. Kies **Value Counts** (aantallen waarden). 

![aantal waarden](media/tutorial-classifying-iris/value_count.png)

Door deze actie wordt een histogram met vier balken gemaakt. Zoals u ziet heeft onze doelkolom drie afzonderlijke waarden: `Iris_virginica`, `Iris_versicolor` en `Iris-setosa`. Er is ook één rij met een `(null)`-waarde. We gaan deze rij verwijderen door de balk te selecteren die de null-waarde vertegenwoordigt en op de filterknop **-** te klikken. 

![aantal waarden](media/tutorial-classifying-iris/filter_out.png)

Terwijl u bezig bent met het hernoemen van kolommen het wegfilteren van de rij met de null-waarde, wordt elke actie die u uitvoert geregistreerd als een gegevensvoorbereidingsstap in het deelvenster **STEPS** (STAPPEN). U kunt deze bewerken (om de instellingen aan te passen), de volgorde ervan wijzigen of ze zelfs verwijderen.

![stappen](media/tutorial-classifying-iris/steps.png)

## <a name="generate-pythonpyspark-code-to-invoke-data-prep-package"></a>Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

Sluit nu de DataPrep-editor. (Geen zorgen, er wordt automatisch opgeslagen.) Klik met de rechtermuisknop op het bestand **iris 1.dprep** om het contextmenu weer te geven, en kies **Generate Data Access Code File** (Toegangscode voor gegevenstoegang genereren). 

![code genereren](media/tutorial-classifying-iris/generate_code.png)

Er wordt een bestand met de naam **iris 1.py** gemaakt, waarin de volgende twee regels code vooraf zijn ingevuld (plus enkele opmerkingen):

```python
# This code snippet will load the referenced package and return a DataFrame.
# If the code is run in a PySpark environment, the code will return a
# Spark DataFrame. If not, the code will return a Pandas DataFrame.

from azureml.dataprep.package import run
df = run('iris.dprep', dataflow_idx=0)
```
Dit codefragment roept de logica aan die u hebt gemaakt als gegevensvoorbereidingspakket. Afhankelijk van de context waarin deze code wordt uitgevoerd, kan `df` een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) zijn als de code wordt uitgevoerd in Python-runtime, of een [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) als de code wordt uitgevoerd in een Spark-context. Voor meer informatie over het voorbereiden van gegevens in Azure Machine Learning Workbench verwijzen we naar de handleiding [Getting Started with Data Preparation](data-prep-getting-started.md) (Aan de slag met gegevensvoorbereiding).

## <a name="next-steps"></a>Volgende stappen
In dit eerste deel van deze zelfstudie hebt u geleerd hoe u Azure Machine Learning Workbench kunt gebruiken voor deze bewerkingen:
> [!div class="checklist"]
> * Een nieuw project maken 
> * Een gegevensvoorbereidingspakket maken
> * Python/PySpark-code genereren voor het aanroepen van het gegevensvoorbereidingspakket

U bent klaar om door te gaan met het volgende deel in de reeks, over het bouwen van een machine learning-model.
> [!div class="nextstepaction"]
> [Een model bouwen](tutorial-classifying-iris-part-2.md)

