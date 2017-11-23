---
title: Maken van de functies voor gegevens in SQL Server met behulp van SQL en Python | Microsoft Docs
description: Gegevens over het installatieproces van SQL Azure
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: 
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: bradsev;fashah;garye
ms.openlocfilehash: dd919e7f87080b8c4ad1f8d3de26d6f71470a264
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Met SQL en Python functies maken voor gegevens in SQL Server
Dit document wordt beschreven hoe functies voor opgeslagen gegevens in een SQL Server VM op Azure waarmee algoritmen efficiënter meer van de gegevens genereren. U kunt SQL- of een programmeertaal zoals Python gebruiken deze taak uit te voeren. Beide benaderingen zijn hier gedemonstreerd.

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Dit **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u functies voor gegevens in verschillende omgevingen maken. Deze taak is een stap in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [!NOTE]
> Voor een voorbeeld, kunt u raadplegen de [NYC Taxi gegevensset](http://www.andresmh.com/nyctaxitrips/) en verwijzen naar de met de titel IPNB [IPython laptop- en SQL Server met de gegevens van de NYC worsteling](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een end-to-end-procedure.
> 
> 

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [een Azure Storage-account maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Uw gegevens opgeslagen in SQL Server. Als u niet hebt, raadpleegt u [gegevens verplaatsen naar een Azure SQL Database voor Azure Machine Learning](move-sql-azure.md) voor instructies over het verplaatsen van de gegevens bevat.

## <a name="sql-featuregen"></a>Functie genereren met behulp van SQL
In deze sectie beschrijven we manieren voor het genereren van functies met behulp van SQL:  

1. [Aantal op basis van functie generatie](#sql-countfeature)
2. [Met binning functie generatie](#sql-binningfeature)
3. [Implementeren van de functies van één kolom](#sql-featurerollout)

> [!NOTE]
> Als u extra functies genereert, kunt u als kolommen toevoegen aan de bestaande tabel of een nieuwe tabel maken met de aanvullende functies en de primaire sleutel, die kan worden samengevoegd met de oorspronkelijke tabel.
> 
> 

### <a name="sql-countfeature"></a>Aantal op basis van functie generatie
Dit document wordt gedemonstreerd op twee manieren voor het genereren van het aantal functies. De eerste methode Voorwaardelijke som en de tweede methode gebruikt de component 'where'. Deze kunnen vervolgens worden samengevoegd met de oorspronkelijke tabel (met behulp van de primaire-sleutelkolommen) om het aantal functies samen met de oorspronkelijke gegevens hebben.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Met binning functie generatie
Het volgende voorbeeld ziet u hoe het genereren van binned functies met binning (met behulp van 5 opslaglocaties) een numerieke kolom die kan worden gebruikt als een functie in plaats daarvan:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Implementeren van de functies van één kolom
In deze sectie ziet u hoe u één kolom in een tabel voor het genereren van aanvullende functies implementeren. In het voorbeeld wordt ervan uitgegaan dat er een breedtegraad of lengtegraad kolom in de tabel waaruit u wilt genereren van functies.

Hier volgt een korte primer op breedtegraad/lengtegraad locatiegegevens (resources voorzien van stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Hier volgen enkele nuttige overwegingen bij het gebruik van locatiegegevens voordat u onderdelen uit het veld maakt:

* Het teken geeft aan of we noorden of Zuid, Oost of west op de hele wereld.
* Een andere waarde dan nul honderden cijfer lengtegraad aangeeft, niet breedtegraad wordt gebruikt.
* De tientallen cijfer biedt een positie naar ongeveer 1000 kilometers. Dit biedt nuttige informatie over welke continent of Oceaan we worden op.
* Het cijfer eenheden (één decimale graad) biedt een positie tot 111 kilometers (60 zeemijl, ongeveer 69 mijl). Dit geeft aan ongeveer, voor welke grote rechtsgebied we zijn in.
* De eerste decimaal verdient aanbeveling tot 11.1 km: deze de positie van een grote plaats van een aangrenzende grote stad kunt onderscheiden.
* De tweede decimaal verdient aanbeveling tot 1.1 km: het één village kunt scheiden van de volgende.
* De derde decimaal is waard tot 110 m: dat deze grote de veld of institutionele bereik kunt identificeren.
* De vierde decimaal is waard maximaal 11 m: dat deze een pakket van de grond kunt identificeren. Dit is vergelijkbaar met de gebruikelijke nauwkeurigheid van een GPS-eenheid niet corrigeert zonder storing.
* De vijfde decimaal is waard maximaal 1.1 m: dat deze structuren onderscheidt van elkaar. Nauwkeurigheid tot dit niveau met commerciële GPS-eenheden kan alleen worden verkregen met differentiële gecorrigeerd.
* De zesde decimaal is waard maximaal 0.11 m: die u kunt deze gebruiken voor het indelen van structuren met details voor het ontwerpen van landschappen, wegen bouwen. Moet meer dan goed genoeg voor het bijhouden van verkeer van glaciers en rivieren zijn. Dit kan worden bereikt door middel van hele maatregelen met GPS zoals differentially gecorrigeerde GPS.

Informatie over de locatie kan featurized worden door het scheiden van de regio, locatie en plaats. Let op: één keer kunt ook aanroepen een REST-eindpunt zoals Bing kaarten-API beschikbaar op `https://msdn.microsoft.com/library/ff701710.aspx` voor de regio/regionale informatie.

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

Deze functies op basis van locatie kunnen verder worden gebruikt voor het genereren van aantal aanvullende functies, zoals eerder beschreven.

> [!TIP]
> U kunt de records met behulp van de taal van keuze programmatisch invoegen. U wilt opnemen in segmenten schrijven efficiëntie te verbeteren. [Hier volgt een voorbeeld van hoe u dit doen met pyodbc](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
> Een ander alternatief is gegevens invoegen in de database met behulp [hulpprogramma BCP](https://msdn.microsoft.com/library/ms162802.aspx)
> 
> 

### <a name="sql-aml"></a>Verbinding maken met Azure Machine Learning
De nieuwe functie worden toegevoegd als een kolom aan een bestaande tabel of opgeslagen in een nieuwe tabel en samengevoegd met de oorspronkelijke tabel voor machine learning. Functies kunnen worden gegenereerd of als u al hebt gemaakt, toegankelijk via de [importgegevens](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) -module in de Azure ML zoals hieronder wordt weergegeven:

![Azure ML-lezers](./media/sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Gebruik een programmeertaal zoals Python
Met behulp van Python functies genereren wanneer de gegevens in SQL Server is vergelijkbaar met het verwerken van gegevens in Azure blob met behulp van Python. Zie voor vergelijking [proces Azure Blob-gegevens in uw omgeving van de wetenschappelijke gegevens](data-blob.md). De gegevens uit de database in een frame pandas gegevens verdere verwerking laden. Het proces van het verbinden met de database en de gegevens in het kader van de gegevens te laden wordt in deze sectie beschreven.

Indeling van de volgende verbindingsreeks kan worden gebruikt voor het verbinden met een SQL Server-database met Python pyodbc (vervang servername, dbname, gebruikersnaam en wachtwoord op uw specifieke waarden) met:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [Pandas bibliotheek](http://pandas.pydata.org/) in Python voorziet in een uitgebreide set van gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie voor het programmeren van Python. De volgende code leest de resultaten van een SQL Server-database in een kader Pandas-gegevens geretourneerd:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu u met het kader van de gegevens Pandas werken kunt zoals beschreven in de onderwerpen [maken van de functies voor Azure blob storage-gegevens met behulp van Panda](create-features-blob.md).

