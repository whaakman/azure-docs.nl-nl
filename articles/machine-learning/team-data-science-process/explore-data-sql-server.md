---
title: Verken de gegevens in virtuele Machine van SQL Server op Azure | Microsoft Docs
description: Klik hier voor meer informatie over het verkennen van gegevens die zijn opgeslagen in een virtuele SQL Server-machine in Azure.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: d5079fc9d9a0ccd89664407fcdba9a7958e1407d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Gegevens in virtuele SQL Server-machine verkennen in Azure
Dit document wordt beschreven hoe gegevens die zijn opgeslagen in een virtuele SQL Server-machine in Azure. Dit kan worden gedaan door gegevens worsteling met SQL of via een programmeertaal zoals Python.

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u gegevens uit verschillende omgevingen met opslag verkennen met behulp van hulpprogramma's. Deze taak is een stap in de Cortana Analytics proces (CAP).

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> De voorbeeld-SQL-instructies in dit document wordt ervan uitgegaan dat gegevens in SQL Server worden. Als dat niet, raadpleegt u het procesoverzicht in cloud wetenschappelijke voor informatie over het verplaatsen van uw gegevens naar SQL Server.
> 
> 

## <a name="sql-dataexploration"></a>SQL-gegevens met SQL-scripts verkennen
Hier volgen enkele voorbeelden SQL-scripts die kunnen worden gebruikt om te verkennen gegevensarchieven in SQL Server.

1. Het aantal opmerkingen per dag
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. De niveaus in een categorische kolom ophalen
   
    `select  distinct <column_name> from <databasename>`
3. Het aantal niveaus in de combinatie van twee categorische kolommen ophalen 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. De verdeling voor numerieke kolommen ophalen
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Voor een voorbeeld, kunt u de [NYC Taxi gegevensset](http://www.andresmh.com/nyctaxitrips/) en verwijzen naar de met de titel IPNB [IPython laptop- en SQL Server met de gegevens van de NYC worsteling](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een end-to-end-procedure.
> 
> 

## <a name="python"></a>SQL-gegevens met behulp van Python verkennen
Met behulp van Python gegevens verkennen en functies genereren wanneer de gegevens in SQL Server is vergelijkbaar met het verwerken van gegevens in Azure blob met behulp van Python, zoals beschreven in [proces Azure Blob-gegevens in uw omgeving van de wetenschappelijke gegevens](data-blob.md). De gegevens moeten worden geladen uit de database in een pandas DataFrame en vervolgens verder kunnen worden verwerkt. We Documenteer het proces van het verbinden met de database en de gegevens in de DataFrame in deze sectie te laden.

Indeling van de volgende verbindingsreeks kan worden gebruikt voor het verbinden met een SQL Server-database met Python pyodbc (vervang servername, dbname, gebruikersnaam en wachtwoord op uw specifieke waarden) met:

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [Pandas bibliotheek](http://pandas.pydata.org/) in Python voorziet in een uitgebreide set van gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie voor het programmeren van Python. De volgende code leest de resultaten van een SQL Server-database in een kader Pandas-gegevens geretourneerd:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu u met de DataFrame Pandas werken kunt zoals besproken in het onderwerp [proces Azure Blob-gegevens in uw omgeving van de wetenschappelijke gegevens](data-blob.md).

## <a name="the-team-data-science-process-in-action-example"></a>De procedure van wetenschappelijke gegevens Team in actie voorbeeld
Zie voor een voorbeeld van de end-to-end-overzicht van het Cortana-Analytics-proces met een openbare gegevensset, [het Team gegevens wetenschappelijke processen in actie: met behulp van SQL Server](sql-walkthrough.md).

