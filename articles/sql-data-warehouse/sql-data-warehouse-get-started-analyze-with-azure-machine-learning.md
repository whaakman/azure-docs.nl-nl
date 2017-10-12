---
title: Gegevens analyseren met Azure Machine Learning | Microsoft Docs
description: Gebruik Azure Machine Learning om een voorspellend Machine Learning-model te maken dat is gebaseerd op gegevens die zijn opgeslagen in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3197948e32fe5c95b111fe5495a0e5f85966a24b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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
2. Klik op **+NEW** (Nieuw) en selecteer **Blank Experiment** (Leeg experiment).
3. Voer een naam in voor uw experiment: Targeted Marketing.
4. Sleep de module **Reader** van het deelvenster met modules naar het canvas.
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

1. Sleep de module **Project Columns** (Projectkolommen) naar het canvas.
2. Klik in het deelvenster Properties (Eigenschappen) op **Launch column selector** (Kolomselectie starten) om de kolommen op te geven die u wilt verwijderen.
   ![Projectkolommen][4]
3. Sluit twee kolommen uit: CustomerAlternateKey en GeographyKey.
   ![Overbodige kolommen verwijderen][5]

## <a name="3-build-the-model"></a>3. Het model maken
U gaat de gegevens 80-20 splitsen: 80% om een Machine Learning-model te trainen en 20% om het model te testen. Voor dit binair klassificatieprobleem gaat u de algoritme Two-Class gebruiken.

1. Sleep de module **Split** (Splitsen) naar het canvas.
2. Typ 0,8 in Fraction of rows in the first output dataset (Fractie van rijen in de eerste gegevensset) in het deelvenster Properties (Eigenschappen).
   ![Gegevens splitsen in trainings- en testset][6]
3. Sleep de module **Two-Class Boosted Decision Tree** (Beslissingsstructuur op basis van twee klassen) naar het canvas.
4. Sleep de module **Train Model** (Model trainen) naar het canvas en geef de invoer op. Klik vervolgens in het deelvenster Properties (Eigenschappen) op **Launch column selector** (Kolomselectie starten).
   * Eerste invoer: ML-algoritme.
   * Tweede invoer: de gegevens waarmee u het algoritme wilt trainen.
     ![Verbinding maken met de module Train Model (Model trainen)][7]
5. Selecteer de kolom **BikeBuyer** als de kolom die u wilt voorspellen.
   ![Te voorspellen kolom selecteren][8]

## <a name="4-score-the-model"></a>4. Het model scoren
Nu gaat u testen hoe het model functioneert met testgegevens. U gaat het gekozen algoritme vergelijken met een ander algoritme om te zien welk algoritme de beste prestaties levert.

1. Sleep de module **Score Model** (Model beoordelen) naar het canvas.
    Eerste invoer: getraind model Tweede invoer: testgegevens ![Het model beoordelen][9]
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
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction to Machine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
