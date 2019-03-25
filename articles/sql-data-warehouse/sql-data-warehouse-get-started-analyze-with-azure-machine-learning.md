---
title: Gegevens analyseren met Azure Machine Learning | Microsoft Docs
description: Gebruik Azure Machine Learning om een voorspellend Machine Learning-model te maken dat is gebaseerd op gegevens die zijn opgeslagen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: consume
ms.date: 03/22/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.openlocfilehash: 7f9500adc6871c4c9f81c32bf456bc36cf91db4b
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402555"
---
# <a name="analyze-data-with-azure-machine-learning"></a>Gegevens analyseren met Azure Machine Learning
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

In deze zelfstudie wordt gebruikgemaakt van Azure Machine Learning om een voorspellend Machine Learning-model te maken dat is gebaseerd op gegevens die zijn opgeslagen in Azure SQL Data Warehouse. U maakt een gerichte marketingcampagne voor Adventure Works, de fietsenwinkel, door te voorspellen of een klant een fiets waarschijnlijk wel of niet zal kopen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een SQL Data Warehouse waarin AdventureWorksDW-voorbeeldgegevens zijn geladen. Zie voor het inrichten hiervan [Een SQL Data Warehouse maken][Create a SQL Data Warehouse] en kies ervoor om de voorbeeldgegevens te laden. Als u wel een datawarehouse hebt maar nog geen voorbeeldgegevens, kunt u [voorbeeldgegevens handmatig laden][load sample data manually].

## <a name="1-get-the-data"></a>1. De gegevens ophalen
De gegevens bevinden zich in de weergave dbo.vTargetMail in de AdventureWorksDW-database. Deze gegevens lezen:

1. Meld u aan bij [Azure Machine Learning Studio][Azure Machine Learning studio] en klik op My experiments (Mijn experimenten).
2. Klik op **+ nieuw** in de linkerbenedenhoek van het scherm en selecteer **leeg Experiment**.
3. Voer een naam in voor uw experiment: Doelgerichte Marketing.
4. Sleep de **gegevens importeren** module onder **gegevensinvoer en uitvoer** vanuit het deelvenster met modules naar het canvas.
5. Geef de details van uw SQL Data Warehouse-database op in het deelvenster Properties.
6. Geef de database**query** op om de gewenste gegevens te lezen.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Voer het experiment uit door onder experimentencanvas op **RUN** (UITVOEREN) te klikken.
![Voer het experiment uit.][1]

Wanneer het experiment is uitgevoerd, klikt u op de uitvoerpoort onder in de module Reader en selecteert u **Visualize** (Visualiseren) om de geïmporteerde gegevens te zien.
![Geïmporteerde gegevens weergeven][3]

## <a name="2-clean-the-data"></a>2. De gegevens opschonen
Als u de gegevens wilt opschonen, verwijdert u enkele kolommen die niet relevant zijn voor het model. Om dit te doen:

1. Sleep de **Select Columns in Dataset** module onder **gegevenstransformatie < manipulatie** naar het canvas. Verbinding maken met deze module aan de **importgegevens** module.
2. Klik in het deelvenster Properties (Eigenschappen) op **Launch column selector** (Kolomselectie starten) om de kolommen op te geven die u wilt verwijderen.
   ![Projectkolommen][4]
3. Sluit twee kolommen: CustomerAlternateKey en GeographyKey.
   ![Overbodige kolommen verwijderen][5]

## <a name="3-build-the-model"></a>3. Het model maken
We zullen de gegevens 80-20 splitsen: 80% naar een machine learning-model te trainen en 20% voor het testen van het model. Voor dit binair klassificatieprobleem gaat u de algoritme Two-Class gebruiken.

1. Sleep de module **Split** (Splitsen) naar het canvas.
2. Voer in het eigenschappendeelvenster met 0,8 voor fractie van rijen in de eerste uitvoergegevensset.
   ![Gegevens splitsen in trainings- en testset][6]
3. Sleep de module **Two-Class Boosted Decision Tree** (Beslissingsstructuur op basis van twee klassen) naar het canvas.
4. Sleep de **Train Model** module naar het canvas en invoer opgeven door deze te verbinden met de **Two-Class Boosted beslissingsstructuur** (ML-algoritme) en **splitsen** (gegevens met het trainen van de de algoritme op)-modules. 
     ![Verbinding maken met de module Train Model (Model trainen)][7]
5. Klik vervolgens in het deelvenster Properties (Eigenschappen) op **Launch column selector** (Kolomselectie starten). Selecteer de kolom **BikeBuyer** als de kolom die u wilt voorspellen.
   ![Te voorspellen kolom selecteren][8]

## <a name="4-score-the-model"></a>4. Het model scoren
Nu gaat u testen hoe het model functioneert met testgegevens. U gaat het gekozen algoritme vergelijken met een ander algoritme om te zien welk algoritme de beste prestaties levert.

1. Sleep **Score Model** module naar het canvas en verbindt deze met **Train Model** en **Split Data** modules.
   ![Het model beoordelen][9]
2. Sleep de **Two-Class Bayes Point Machine** naar het experimentencanvas. U gaat dit algoritme vergelijken met de Two-Class Boosted Decision Tree (Beslissingsstructuur met twee klassen).
3. Kopieer en plak de modules Train Model en Score Model naar het canvas.
4. Sleep het model **Evaluate Model** (Model evalueren) naar het canvas om de twee algoritmen te vergelijken.
5. **Voer het experiment uit**.
   ![Het experiment uitvoeren.][10]
6. Klik op de uitvoerpoort onder in de module Evaluate Model (Model evalueren) en klik op Visualize (Visualiseren).
   ![Evaluatieresultaten visualiseren][11]

De verstrekte metrische gegevens zijn de ROC-curve, het precisie-/oproepdiagram en de liftcurve. Als u deze metrische gegevens bekijkt, ziet u dat het eerste model beter presteert dan het tweede. Als u de voorspellingen van het eerste model wilt zien, klikt u op de uitvoerpoort van de module Score Model en op Visualize (Visualiseren).
![Scoreresultaten visualiseren][12]

U ziet dat er twee of meer kolommen aan de testgegevensset zijn toegevoegd.

* Scored Probabilities (Berekende kansen): de waarschijnlijkheid dat een klant een fiets koopt.
* Scored Labels (Berekende Labels): de classificatie die door het model is uitgevoerd: fietskoper (1) of niet (0). Deze drempelwaarde voor waarschijnlijkheid voor labeling is ingesteld op 50% en kan worden aangepast.

Door de kolom BikeBuyer (werkelijk) te vergelijken met de kolom Scored Labels (voorspelling), ziet u hoe goed het model heeft gepresteerd. In de volgende stappen kunt u dit model gebruiken om voorspellingen te doen voor nieuwe klanten, en dit model publiceren als een webservice of resultaten opslaan in SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg [Inleiding tot Machine Learning in Azure][Introduction to Machine Learning on Azure] voor meer informatie over het bouwen van voorspellende Machine Learning-modellen.

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1-reader-new.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2-visualize-new.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3-readerdata-new.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4-projectcolumns-new.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5-columnselector-new.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6-split-new.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7-train-new.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8-traincolumnselector-new.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9-score-new.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10-evaluate-new.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11-evalresults-new.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12-scoreresults-new.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction to Machine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
