---
title: Voorbeeldgegevens in SQL Server op Azure | Microsoft Docs
description: Voorbeeldgegevens in SQL Server op Azure
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: fbd83ad59a9db1daca4ba16402031e2c1c5b7991
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="heading"></a>Voorbeeldgegevens in SQL Server op Azure
Dit document wordt beschreven hoe voorbeeldgegevens die zijn opgeslagen in SQL Server op Azure met SQL of de programmeertaal Python. Ook ziet u hoe steekproefgegevens verplaatsen naar Azure Machine Learning door op een bestand op te slaan, uploaden naar een Azure-blob en vervolgens te lezen in Azure Machine Learning Studio.

De steekproeven van Python gebruikt de [pyodbc](https://code.google.com/p/pyodbc/) ODBC-bibliotheek verbinding maken met SQL Server op Azure en de [Pandas](http://pandas.pydata.org/) bibliotheek te doen de steekproeven.

> [!NOTE]
> De SQL-voorbeeldcode in dit document wordt ervan uitgegaan dat de gegevens zich in een SQL-Server op Azure. Als dit niet het geval is, raadpleegt u [gegevens verplaatsen naar SQL Server op Azure](move-sql-server-virtual-machine.md) onderwerp voor instructies over het verplaatsen van uw gegevens naar SQL Server op Azure.
> 
> 

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u voorbeeldgegevens uit verschillende omgevingen voor opslag. 

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Waarom een steekproef nemen uit uw gegevens?**
Als de gegevensset die u wilt analyseren groot is, is het meestal een goed idee om de gegevens om deze aan de grootte van een kleinere maar representatief en gemakkelijker down-voorbeeld. Dit vereenvoudigt het begrip van de gegevens, exploratie en functie-engineering. De rol in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is het inschakelen van snel maken van een prototype van de functies voor het verwerken van gegevens en machine learning-modellen.

Deze taak steekproeven is een stap in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="SQL"></a>Met behulp van SQL
Deze sectie beschrijft de verschillende methoden met SQL voor het uitvoeren van eenvoudige willekeurige steekproef op basis van de gegevens in de database. Kies een methode op basis van de gegevensgrootte van uw en de distributie.

De volgende twee items laten zien hoe newid in SQL Server gebruikt de steekproeven uitvoeren. Welke methode u kiest is afhankelijk van hoe willekeurige u wilt dat de steekproef worden (pk_id in de volgende voorbeeldcode wordt ervan uitgegaan dat een automatisch gegenereerde primaire sleutel).

1. Minder strikte steekproef
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Meer steekproef 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Component TABLESAMPLE kan worden gebruikt voor steekproeven evenals gedemonstreerd hieronder. Dit kan een betere benadering zijn als de gegevensgrootte van uw groot is (ervan uitgaande dat de gegevens op verschillende pagina's worden niet gecorreleerd) en voor de query uit te voeren in een redelijke tijd.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> U kunt verkennen en functies van deze steekproefgegevens genereren door op te slaan in een nieuwe tabel
> 
> 

### <a name="sql-aml"></a>Verbinding maken met Azure Machine Learning
U kunt rechtstreeks de voorbeeldquery's boven in de Azure Machine Learning [importgegevens] [ import-data] module die u wilt de gegevens onderweg down-voorbeeld en brengt u het in een Azure Machine Learning-experiment. Een schermafbeelding van de module reader gebruikt om te lezen van de steekproefgegevens wordt hieronder weergegeven:

![lezer sql][1]

## <a name="python"></a>Met behulp van de programmeertaal Python
Deze sectie wordt gedemonstreerd met behulp van de [pyodbc bibliotheek](https://code.google.com/p/pyodbc/) tot stand brengen van een ODBC verbinding maken met een SQL server-database in Python. De tekenreeks voor databaseverbinding is als volgt: (vervangen servername, dbname, username en password door uw configuratie):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [Pandas](http://pandas.pydata.org/) in Python-bibliotheek biedt een uitgebreide set gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie voor het programmeren van Python. De onderstaande code leest een steekproef 0,1% van de gegevens uit een tabel in Azure SQL-database in een Pandas-gegevens:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Nu kunt u werken met de steekproefgegevens in het kader van de gegevens Pandas. 

### <a name="python-aml"></a>Verbinding maken met Azure Machine Learning
U kunt de volgende voorbeeldcode omlaag actieve gegevens naar een bestand opslaan en upload dit naar een Azure-blob. De gegevens in de blob rechtstreeks kunnen worden gelezen in een Azure Machine Learning-Experiment met behulp van de [importgegevens] [ import-data] module. De stappen zijn als volgt: 

1. Het frame pandas-gegevens schrijven naar een lokaal bestand
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Lokaal bestand uploaden naar Azure blob
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Gegevens lezen uit Azure blob met Azure Machine Learning [importgegevens] [ import-data] module, zoals wordt weergegeven in het scherm schermafbeelding hieronder:

![lezer blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>De procedure van wetenschappelijke gegevens Team in actie voorbeeld
Zie voor een overzicht van de end-to-end voorbeeld van het Team gegevens wetenschap proces een met een openbare gegevensset [Team gegevens wetenschappelijke processen in actie: SQL-Server](sql-walkthrough.md).

[1]: ./media/sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
