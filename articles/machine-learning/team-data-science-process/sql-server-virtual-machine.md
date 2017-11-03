---
title: Verken de gegevens in een SQL Server-machine in Azure | Microsoft Docs
description: Gegevens verkennen en genereren van functies in een virtuele machine van SQL Server op Azure
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 063709a22540e22d1eb6f1c6a6ff777e95f6a29a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="heading"></a>Procesgegevens in de virtuele Machine van SQL Server op Azure
Dit document wordt beschreven hoe gegevens verkennen en functies voor gegevens die zijn opgeslagen in een virtuele SQL Server-machine in Azure te genereren. Dit kan worden gedaan door gegevens worsteling met SQL of via een programmeertaal zoals Python.

> [!NOTE]
> De voorbeeld-SQL-instructies in dit document wordt ervan uitgegaan dat gegevens in SQL Server worden. Als dat niet, raadpleegt u het procesoverzicht in cloud wetenschappelijke voor informatie over het verplaatsen van uw gegevens naar SQL Server.
> 
> 

## <a name="SQL"></a>Met behulp van SQL
We beschrijven de volgende gegevens wrangling taken in deze sectie met behulp van SQL:

1. [Gegevensverkenning](#sql-dataexploration)
2. [Functie generatie](#sql-featuregen)

### <a name="sql-dataexploration"></a>Gegevensverkenning
Hier volgen enkele voorbeelden SQL-scripts die kunnen worden gebruikt om te verkennen gegevensarchieven in SQL Server.

> [!NOTE]
> Voor een voorbeeld, kunt u de [NYC Taxi gegevensset](http://www.andresmh.com/nyctaxitrips/) en verwijzen naar de met de titel IPNB [IPython laptop- en SQL Server met de gegevens van de NYC worsteling](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een end-to-end-procedure.
> 
> 

1. Het aantal opmerkingen per dag
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. De niveaus in een categorische kolom ophalen
   
    `select  distinct <column_name> from <databasename>`
3. Het aantal niveaus in de combinatie van twee categorische kolommen ophalen 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. De verdeling voor numerieke kolommen ophalen
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="sql-featuregen"></a>Functie generatie
In deze sectie beschrijven we manieren voor het genereren van functies met behulp van SQL:  

1. [Aantal op basis van functie generatie](#sql-countfeature)
2. [Met binning functie generatie](#sql-binningfeature)
3. [Implementeren van de functies van één kolom](#sql-featurerollout)

> [!NOTE]
> Als u extra functies genereert, kunt u als kolommen toevoegen aan de bestaande tabel of een nieuwe tabel maken met de aanvullende functies en de primaire sleutel, die kan worden samengevoegd met de oorspronkelijke tabel. 
> 
> 

### <a name="sql-countfeature"></a>Aantal op basis van functie generatie
De volgende voorbeelden ziet twee manieren voor het genereren van het aantal functies. De eerste methode Voorwaardelijke som en de tweede methode gebruikt de component 'where'. Deze kunnen vervolgens worden samengevoegd met de oorspronkelijke tabel (met behulp van de primaire-sleutelkolommen) om het aantal functies samen met de oorspronkelijke gegevens hebben.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="sql-binningfeature"></a>Met binning functie generatie
Het volgende voorbeeld ziet u hoe het genereren van binned functies met binning (met behulp van vijf opslaglocaties) een numerieke kolom die kan worden gebruikt als een functie in plaats daarvan:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Implementeren van de functies van één kolom
In deze sectie ziet u hoe u één kolom in een tabel voor het genereren van aanvullende functies implementeren. In het voorbeeld wordt ervan uitgegaan dat er een breedtegraad of lengtegraad kolom in de tabel waaruit u wilt genereren van functies.

Hier volgt een korte primer op breedtegraad/lengtegraad locatiegegevens (resources voorzien van stackoverflow [meten; de nauwkeurigheid van de breedtegraad en lengtegraad?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Dit is handig om te begrijpen voordat featurizing het locatieveld:

* Het teken vertellen of we noorden of Zuid, Oost of west op de hele wereld.
* Een andere waarde dan nul honderden cijfer vertelt ons dat we WL, niet NB gebruiken!
* De tientallen cijfer biedt een positie naar ongeveer 1000 kilometers. Dit biedt ons nuttige informatie over welke continent of Oceaan we worden op.
* Het cijfer eenheden (één decimale graad) biedt een positie tot 111 kilometers (60 zeemijl, ongeveer 69 mijl). Deze kunt ons ongeveer welke grote rechtsgebied die we in zijn.
* De eerste decimaal verdient aanbeveling tot 11.1 km: deze de positie van een grote plaats van een aangrenzende grote stad kunt onderscheiden.
* De tweede decimaal verdient aanbeveling tot 1.1 km: het één village kunt scheiden van de volgende.
* De derde decimaal is waard tot 110 m: dat deze grote de veld of institutionele bereik kunt identificeren.
* De vierde decimaal is waard maximaal 11 m: dat deze een pakket van de grond kunt identificeren. Dit is vergelijkbaar met de gebruikelijke nauwkeurigheid van een GPS-eenheid niet corrigeert zonder storing.
* De vijfde decimaal is waard maximaal 1.1 m: dat deze structuren onderscheidt van elkaar. Nauwkeurigheid tot dit niveau met commerciële GPS-eenheden kan alleen worden verkregen met differentiële gecorrigeerd.
* De zesde decimaal is waard maximaal 0.11 m: die u kunt deze gebruiken voor het indelen van structuren met details voor het ontwerpen van landschappen, wegen bouwen. Moet meer dan goed genoeg voor het bijhouden van verkeer van glaciers en rivieren zijn. Dit kan worden bereikt door middel van hele maatregelen met GPS zoals differentially gecorrigeerde GPS.

Informatie over de locatie kan worden featurized als volgt scheiden van de regio, locatie en plaats. Houd er rekening mee dat kunt u ook een REST-eindpunt zoals Bing kaarten-API beschikbaar op aanroepen [zoeken naar een locatie met punt](https://msdn.microsoft.com/library/ff701710.aspx) voor de regio/regionale informatie.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Deze functies op basis van locatie kunnen verder worden gebruikt voor het genereren van aantal aanvullende functies zoals eerder beschreven. 

> [!TIP]
> U kunt de records met behulp van de taal van keuze programmatisch invoegen. Mogelijk moet u de gegevens invoegen in segmenten schrijven efficiëntie te verbeteren (Zie voor een voorbeeld van hoe u dit doen met pyodbc [A Hallo wereld-voorbeeld voor toegang tot SQL Server met behulp van python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Een ander alternatief is om gegevens te voegen aan de database met de [hulpprogramma BCP](https://msdn.microsoft.com/library/ms162802.aspx).
> 
> 

### <a name="sql-aml"></a>Verbinding maken met Azure Machine Learning
De nieuwe functie worden toegevoegd als een kolom aan een bestaande tabel of opgeslagen in een nieuwe tabel en samengevoegd met de oorspronkelijke tabel voor machine learning. Functies kunnen worden gegenereerd of als u al hebt gemaakt, toegankelijk via de [importgegevens] [ import-data] -module in Azure Machine Learning zoals hieronder wordt weergegeven:

![azureml lezers][1] 

## <a name="python"></a>Gebruik een programmeertaal zoals Python
Met behulp van Python gegevens verkennen en functies genereren wanneer de gegevens in SQL Server is vergelijkbaar met het verwerken van gegevens in Azure blob met behulp van Python, zoals beschreven in [proces Azure Blob-gegevens in uw omgeving van de wetenschappelijke gegevens](data-blob.md). De gegevens moeten worden geladen uit de database in een frame pandas-gegevens en vervolgens verder kunnen worden verwerkt. We Documenteer het proces van het verbinden met de database en de gegevens in het kader van de gegevens in deze sectie te laden.

Indeling van de volgende verbindingsreeks kan worden gebruikt voor het verbinden met een SQL Server-database met Python pyodbc (vervang servername, dbname, gebruikersnaam en wachtwoord op uw specifieke waarden) met:

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [Pandas bibliotheek](http://pandas.pydata.org/) in Python voorziet in een uitgebreide set van gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie voor het programmeren van Python. De onderstaande code leest de resultaten van een SQL Server-database in een kader Pandas-gegevens geretourneerd:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu u met het kader van de gegevens Pandas werken kunt zoals beschreven in het artikel [proces Azure Blob-gegevens in uw omgeving van de wetenschappelijke gegevens](data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Azure Gegevenswetenschap in actie voorbeeld
Zie voor een voorbeeld van de end-to-end-overzicht van het Azure Data wetenschap proces met behulp van een openbare gegevensset, [Azure gegevens wetenschappelijke processen in actie](sql-walkthrough.md).

[1]: ./media/sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

