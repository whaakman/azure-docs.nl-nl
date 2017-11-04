---
title: Gebruik van Azure Machine Learning met SQL datawarehouse | Microsoft Docs
description: Zelfstudie voor het gebruik van Azure Machine Learning met Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: c19860c6b5b1c15d1e29ddc67f9cf9ad4618725b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Gebruik Azure Machine Learning met SQL datawarehouse
Azure Machine Learning is een volledig beheerde predictive analytics-service die u kunt gebruiken voor het maken van voorspellende modellen op basis van uw gegevens in SQL Data Warehouse en vervolgens publiceren als webservices gereed om te gebruiken. U kunt de basiskennis van predictive analytics en machine learning door te lezen [Inleiding tot Machine Learning in Azure][Introduction to Machine Learning on Azure].  U kunt vervolgens informatie over het maken, trainen, beoordelen en testen van een machine learning-model met behulp van de [maken experiment zelfstudie][Create experiment tutorial].

In dit artikel leert u hoe u de volgende via de [Azure Machine Learning Studio][Azure Machine Learning Studio]:

* Gegevens lezen uit de database te maken, te trainen en een Voorspellend model beoordelen
* Schrijven van gegevens naar de database

## <a name="read-data-from-sql-data-warehouse"></a>Lezen van gegevens uit SQL Data Warehouse
Leest gegevens uit de tabel Product in de database AdventureWorksDW.

### <a name="step-1"></a>Stap 1
Start een nieuw experiment door te klikken op + Nieuw onderaan in het venster Machine Learning Studio EXPERIMENT selecteren en selecteer vervolgens leeg Experiment. Selecteer de naam van de standaard-experiment aan de bovenkant van het canvas en wijzig deze in een beschrijvende bijvoorbeeld fiets prijs voorspelling.

### <a name="step-2"></a>Stap 2
Zoek naar de module Reader in het palet met gegevenssets en modules aan de linkerkant van het experimentcanvas. Sleep de module naar het experimentcanvas.
![][drag_reader]

### <a name="step-3"></a>Stap 3
Selecteer de module Reader en invullen in het eigenschappendeelvenster.

1. Selecteer Azure SQL Database als de gegevensbron.
2. Database-servernaam: Typ de naam van de server. U kunt de [Azure-portal] [ Azure portal] dit vinden.

![][server_name]

1. Databasenaam: Typ de naam van een database op de server die u zojuist hebt opgegeven.
2. Server gebruikersaccountnaam: Typ de gebruikersnaam van een account met toegangsmachtigingen voor de database.
3. Het wachtwoord voor gebruikersaccount server: Geef het wachtwoord op voor het opgegeven gebruikersaccount.
4. Een servercertificaat accepteren: Gebruik deze optie (minder veilig) als u doorgaan met het certificaat van de site controleren wilt voordat u uw gegevens leest.
5. Databasequery: Voer een SQL-instructie die beschrijft de gegevens die u wilt lezen. In dit geval leest gegevens uit de volgende query Product-tabel.

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Stap 4
1. Voer het experiment uit door te klikken op uitvoeren onder het experimentcanvas.
2. Wanneer het experiment is voltooid, hebben de module Reader een groen vinkje om aan te geven met succes is voltooid. U ziet ook de status uitgevoerd in de rechterbovenhoek voltooid.

![][run]

1. De geïmporteerde gegevens zien, klikt u op de uitvoerpoort onder aan de gegevensset automobile en visualiseren selecteren.

## <a name="create-train-and-score-a-model"></a>Maken, te trainen en een model beoordelen
Nu kunt u deze gegevensset te gebruiken:

* Een Model maken: gegevens verwerken en functies definiëren
* Het model trainen: kiezen en toepassen van een leeralgoritme
* Het model beoordelen en testen: nieuwe fiets prijs voorspellen

![][model]

Voor meer informatie over het maken, trainen, beoordelen en testen van een machine learning-model gebruiken de [maken experiment zelfstudie][Create experiment tutorial].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Schrijven van gegevens naar Azure SQL Data Warehouse
We wordt de resultaatset ProductPriceForecast tabel in de database AdventureWorksDW geschreven.

### <a name="step-1"></a>Stap 1
Zoek naar de module van de schrijver in het palet met gegevenssets en modules aan de linkerkant van het experimentcanvas. Sleep de module naar het experimentcanvas.

![][drag_writer]

### <a name="step-2"></a>Stap 2
Selecteer de module Writer en het eigenschappendeelvenster invullen.

1. Selecteer Azure SQL Database als het doel-gegevens.
2. Database-servernaam: Typ de naam van de server. U kunt de [Azure-portal] [ Azure portal] dit vinden.
3. Databasenaam: Typ de naam van een database op de server die u zojuist hebt opgegeven.
4. Server gebruikersaccountnaam: Typ de gebruikersnaam van een account met schrijfmachtigingen voor de database.
5. Het wachtwoord voor gebruikersaccount server: Geef het wachtwoord op voor het opgegeven gebruikersaccount.
6. Een servercertificaat (onbeveiligde) accepteren: Selecteer deze optie als u niet wilt weergeven van het certificaat.
7. Door komma's gescheiden lijst met kolommen op te slaan: Geef een lijst van de gegevensset of het resultaat van de kolommen die u wilt uitvoeren.
8. Data-tabelnaam: Geef de naam van de tabel.
9. Door komma's gescheiden lijst met datatable kolommen: Geef de kolomnamen in de nieuwe tabel gebruiken. De kolomnamen kunnen afwijken van de namen in de gegevensset bron, maar u moet hetzelfde aantal kolommen hierheen die u voor de uitvoertabel definieert weergeven.
10. Aantal rijen per SQL Azure-bewerking geschreven: U kunt het aantal rijen die zijn geschreven met een SQL-database in één bewerking configureren.

![][writer_properties]

### <a name="step-3"></a>Stap 3
1. Voer het experiment uit door te klikken op uitvoeren onder het experimentcanvas.
2. Wanneer het experiment is voltooid, hebben alle modules een groen vinkje om aan te geven dat deze is voltooid.

## <a name="next-steps"></a>Volgende stappen
Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars][SQL Data Warehouse development overview] voor meer tips voor ontwikkelaars.

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction to machine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
