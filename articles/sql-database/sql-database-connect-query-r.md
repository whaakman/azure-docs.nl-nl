---
title: Snelstart voor het gebruik van Machine Learning Services (met R) in Azure SQL Database (preview) | Microsoft Docs
description: In dit onderwerp ziet u hoe u Machine Learning Services kunt gebruiken in Azure SQL Database, en hoe u R-scripts uitvoert om geavanceerde analyses op schaal te bieden. U krijgt de mogelijkheid om berekeningen en verwerking uit te voeren op de plek waar gegevens zich bevinden, waardoor u gegevens niet meer over het hele netwerk hoeft te verplaatsen.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.reviewer: ''
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: e15cf93514f921223fea37aa480730bba46dd195
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57864946"
---
# <a name="quickstart-use-machine-learning-services-with-r-in-azure-sql-database-preview"></a>Quickstart: Machine Learning Services (met R) gebruiken in Azure SQL Database (preview)

In dit artikel wordt uitgelegd hoe u de openbare preview van [Machine Learning Services (met R) kunt gebruiken in Azure SQL Database](sql-database-machine-learning-services-overview.md). U wordt stapsgewijs begeleid door de basisbeginselen van het verplaatsen van gegevens tussen een SQL-database en R. Ook wordt uitgelegd hoe u juist opgemaakte R-code inpakt in de opgeslagen procedure [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) om Machine Learning-modellen in een SQL-database te bouwen, te trainen en te gebruiken.

Gebruik de kracht van de taal R om geavanceerde analyses en in-database machine learning te bieden. Deze mogelijkheid zorgt voor berekeningen en verwerking op de plaats waar de gegevens zich bevinden, u hoeft de gegevens niet op te halen via het netwerk. Maak ook gebruik van de kracht van zakelijke R-pakketten om geavanceerde analyses op schaal te bieden.

Machine Learning-services bevatten een basisdistributie van R, overlapt met zakelijke R-pakketten van Microsoft. R-functies en -algoritmen van Microsoft zijn ontworpen voor zowel schaal als bruikbaarheid en bieden voorspellende analyses, statistische modellen, gegevensvisualisaties en toonaangevende algoritmen voor machine learning.

Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/) voordat u begint.

> [!IMPORTANT]
> Azure SQL Database Machine Learning-Services is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

## <a name="prerequisites"></a>Vereisten

Als u de voorbeeldcode in deze oefeningen wilt uitvoeren, moet u eerst beschikken over een SQL-database met Machine Learning Services (met R). Na de onboarding voor de openbare preview wordt Machine Learning voor u ingeschakeld voor uw bestaande of nieuwe database. Volg de stappen in [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

U kunt verbinding maken met de SQL-database en de R-scripts uitvoeren met elk willekeurig hulpprogramma voor databasebeheer of het uitvoeren van query's, zo lang het programma verbinding kan maken met een SQL-database en een T-SQL-query of opgeslagen procedure kan uitvoeren. In deze snelstart wordt [SQL Server Management Studio](sql-database-connect-query-ssms.md) gebruikt.

Voor de oefening [een pakket toevoegen](#add-package) moet u ook [R](https://www.r-project.org/) en [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) installeren op de lokale computer.

Voor deze snelstart is ook vereist dat u een firewallregel op serverniveau configureert. Bekijk [Een firewallregel op serverniveau maken](sql-database-server-level-firewall-rule.md) voor een snelstart die laat zien hoe u dat doet.

## <a name="verify-r-exists"></a>Verifiëren of R bestaat

U kunt bevestigen dat Machine Learning Services (met R) is ingeschakeld voor uw SQL-database. Volg de onderstaande stappen.

1. Open SQL Server Management Studio en maak verbinding met de SQL-database. Zie voor meer informatie over hoe u verbinding maakt, [Quickstart: SQL Server Management Studio gebruiken om verbinding te maken en query's uit te voeren op een Azure SQL database](sql-database-connect-query-ssms.md).

1. Voer de onderstaande code uit. 

    ```sql
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(31 + 11)';
    GO
    ```
    Als alles goed is gegaan, ziet u een resultaatbericht zoals deze.

    ```text
    STDOUT message(s) from external script: 
    42
    ```

1. Als er fouten optreden, kan dit zijn omdat de openbare preview van Machine Learning Services (met R) niet is ingeschakeld voor uw SQL-database. Kijk hierboven hoe u zich kunt registreren voor de openbare preview.

## <a name="grant-permissions"></a>Machtigingen verlenen

Als u een beheerder bent, kunt u automatisch externe code uitvoeren. Alle andere gebruikers moet worden gemachtigd.

Vervang `<username>` door een geldige databasegebruikersaanmelding voordat u de opdracht uitvoert.

```sql
GRANT EXECUTE ANY EXTERNAL SCRIPT TO <username>
```

## <a name="basic-r-interaction"></a>Basisbewerkingen met R

Er zijn twee manieren waarop u R-code kunt uitvoeren in SQL Database:

+ Voeg een R-script toe als argument van de opgeslagen systeemprocedure, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).
+ Maak vanuit een [externe R-client](https://docs.microsoft.com/sql/advanced-analytics/r/set-up-a-data-science-client) verbinding met uw SQL-database, en voer de code uit met behulp van de SQL-database als de compute-context.

De volgende oefening is gericht op het eerste interactiemodel: R-code doorgeven aan een opgeslagen procedure.

1. Voer een eenvoudig script uit om te zien hoe een R-script wordt uitgevoerd in uw SQL-database.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R',
    @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c, d)'
    ```

2. Ervan uitgaande dat u alles juist hebt ingesteld, wordt nu het juiste resultaat berekend, en wordt via de R-functie `print` het resultaat geretourneerd in het **Berichtvenster**.

    **Results**

    ```text
    STDOUT message(s) from external script: 
    0.5 2
    ```

    Het ophalen van **stdout**-berichten is nuttig tijdens het testen van de code, maar het komt vaker voor dat u de resultaten moet retourneren in een tabelindeling, zodat u ze kunt gebruiken in een toepassing of naar een tabel kunt schrijven. Bekijk de secties voor invoer en uitvoer hieronder voor meer informatie.

Let op: alles binnen het `@script`-argument moet geldige R-code zijn.

## <a name="inputs-and-outputs"></a>Invoer en uitvoer

[sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) accepteert standaard één invoergegevensset. Deze levert u meestal in de vorm van een geldige SQL-query. Andere typen invoer kunnen worden doorgegeven als SQL-variabelen.

Met de opgeslagen procedure wordt één R-gegevensframe geretourneerd als uitvoer, maar u kunt ook scalaire waarden en modellen uitvoeren als variabelen. U kunt bijvoorbeeld een getraind model uitvoeren als een binaire variabele en deze doorgeven aan een T-SQL INSERT-instructie om dit model naar een tabel te schrijven. U kunt ook plots genereren (in binaire indeling) of scalaire waarden (afzonderlijke waarden, zoals de datum en tijd, de tijd die is verstreken tijdens het trainen van het model, enzovoort).

Op dit moment kijken we alleen naar de standaardinvoer- en uitvoervariabelen van sp_execute_external_script: `InputDataSet` en `OutputDataSet`.

1. Maak een kleine tabel met testgegevens door de volgende T-SQL-instructie uit te voeren:

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    INSERT INTO RTestData VALUES (1);
    INSERT INTO RTestData VALUES (10);
    INSERT INTO RTestData VALUES (100);
    GO
    ```

    Als de tabel is gemaakt, gebruikt u de volgende instructie om een query uit te voeren voor de tabel:
  
    ```sql
    SELECT * FROM RTestData
    ```

    **Results**

    ![Inhoud van de RTestData-tabel](./media/sql-database-connect-query-r/select-rtestdata.png)

2. U kunt de gegevens uit de tabel ophalen als invoer voor uw R-script. Voer de onderstaande instructie uit. Hiermee worden de gegevens opgehaald uit de tabel, een retour gemaakt via de R-runtime, en de waarden geretourneerd met de kolomnaam *NewColName*.

    De gegevens die worden geretourneerd met de query, worden doorgegeven aan de R-runtime. Hierna worden de gegevens als een gegevensframe geretourneerd naar SQL Database. Met het WITH RESULT SETS-component wordt het schema van de geretourneerde gegevenstabel gedefinieerd voor SQL Database.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    **Results**

    ![Uitvoer van R-script waarmee gegevens uit een tabel worden geretourneerd](./media/sql-database-connect-query-r/r-output-rtestdata.png)

3. We gaan nu de naam van de invoer- en uitvoervariabelen wijzigen. Voor het bovenstaande script zijn de standaardnamen voor invoer- en uitvoervariabelen gebruikt: _InputDataSet_ en _OutputDataSet_. Voor het definiëren van de ingevoerde gegevens die zijn gekoppeld aan _InputDatSet_, gebruikt u de  *\@input_data_1* variabele.

    In dit script zijn de namen van de invoer- en uitvoervariabelen voor de opgeslagen procedure gewijzigd in *SQL_out* en *SQL_in*:

    ```sql
    EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N' SQL_out <- SQL_in;'
      , @input_data_1 = N' SELECT 12 as Col;'
      , @input_data_1_name  = N'SQL_in'
      , @output_data_1_name =  N'SQL_out'
      WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    Let op: R is hoofdlettergevoelig. De hoofdletters en kleine letters van de invoer en uitvoervariabelen in `@input_data_1_name` en `@output_data_1_name` moeten daarom overeenkomen met die van de R-code in `@script`. 

    De volgorde van de parameters is ook belangrijk. U moet de vereiste parameters opgeven  *\@input_data_1* en  *\@output_data_1* eerste, als u wilt gebruiken de volgende optionele parameters  *\@ input_data_1_name* en  *\@output_data_1_name*.

    Er kan maar één invoergegevensset worden doorgegeven als parameter, en u kunt slechts één gegevensset retourneren. U kunt vanuit de R-code wel andere gegevenssets aanroepen en u kunt, naast de gegevensset, ook andere typen uitvoer retourneren. U kunt ook het uitvoertrefwoord toevoegen aan elke willekeurige parameter om deze te laten retourneren met de resultaten. 

    De `WITH RESULT SETS`-instructie definieert het schema voor de gegevens die worden gebruikt in SQL Database. U moet voor elke kolom die u retourneert uit R, gegevenstypen opgeven die compatibel zijn met SQL. U kunt de schemadefinitie gebruiken om ook nieuwe kolomnamen op te geven. U hoeft niet per se de kolomnamen uit het R-gegevensframe te gebruiken.

4. U kunt met behulp van het R-script ook waarden genereren en de invoerquerytekenreeks in _@input_data_1_ leeg laten.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N' mytextvariable <- c("hello", " ", "world");
            OutputDataSet <- as.data.frame(mytextvariable);'
        , @input_data_1 = N''
    WITH RESULT SETS (([Col1] CHAR(20) NOT NULL));
    ```

    **Results**

    ![Resultaten doorzoeken met @script als invoer](./media/sql-database-connect-query-r/r-data-generated-output.png)

## <a name="check-r-version"></a>R-versie controleren

Als u wilt zien welke versie van R is geïnstalleerd in uw SQL database, doet u het volgende:

1. Voer het onderstaande script uit in de SQL-database.

    ```SQL
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(version)';
    GO
    ```

2. Met de R-functie `print` wordt de versie geretourneerd in het **Berichtvenster**. In de voorbeelduitvoer hieronder ziet u dat R-versie 3.4.4 is geïnstalleerd in de SQL-database.

    **Results**

    ```text
    STDOUT message(s) from external script:
                   _
    platform       x86_64-w64-mingw32
    arch           x86_64
    os             mingw32
    system         x86_64, mingw32
    status
    major          3
    minor          4.4
    year           2018
    month          03
    day            15
    svn rev        74408
    language       R
    version.string R version 3.4.4 (2018-03-15)
    nickname       Someone to Lean On
    ```

## <a name="list-r-packages"></a>R-pakketten vermelden

Microsoft biedt een aantal R-pakketten waarin Machine Learning Services vooraf zijn geïnstalleerd in de SQL-database. Volg de onderstaande stappen om een lijst met de geïnstalleerde R-pakketten te zien, inclusief informatie over de versie, de afhankelijkheden, de licentie en het bibliotheekpad. Zie de sectie [een pakket toevoegen](#add-package) om extra pakketten toe te voegen.

1. Voer het onderstaande script uit in de SQL-database.

    ```SQL
    EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
    OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
    WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
        , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
    ```

2. De uitvoer komt uit `installed.packages()` in R en wordt geretourneerd als een resultatenset.

    **Results**

    ![Geïnstalleerde pakketten in R](./media/sql-database-connect-query-r/r-installed-packages.png)

## <a name="create-a-predictive-model"></a>Een voorspellend model maken

U kunt een model trainen met R en dit model opslaan in een tabel in de SQL-database. In deze oefening traint u een eenvoudig regressiemodel waarmee de remafstand van een auto wordt voorspeld op basis van de snelheid. U gebruikt de gegevensset `cars` die is opgenomen in R, omdat deze klein is en eenvoudig te begrijpen.

1. Maak eerst een tabel om de trainingsgegevens in op te slaan.

    ```sql
    CREATE TABLE dbo.CarSpeed (speed INT NOT NULL, distance INT NOT NULL)
    GO
    INSERT INTO dbo.CarSpeed (speed, distance)
    EXEC sp_execute_external_script
    @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

    Veel gegevenssets, grote en kleine, zijn opgenomen in de R-runtime. Typ `library(help="datasets")` in een R-opdrachtprompt om een lijst met gegevenssets op te halen die zijn geïnstalleerd met R.

2. Maak een regressiemodel. De snelheid van de auto bevat twee kolommen, beide numeriek, `dist` en `speed`. Sommige snelheden hebben meerdere waarnemingen. Van deze gegevens maakt u een lineair regressiemodel waarmee de relatie wordt beschreven tussen de snelheid van een auto en de afstand die is vereist om de auto te laten stoppen.

    De vereisten van een lineair model zijn eenvoudig:

   - Definieer een formule die de relatie beschrijft tussen de afhankelijke variabele `speed` en de onafhankelijke variabele `distance`.

   - Bied de invoergegevens die moeten worden gebruikt om het model te trainen.

     > [!TIP]
     > Als u uw kennis over lineaire modellen wilt opfrissen, raden we u aan deze zelfstudie te volgen, waarin het proces voor het aanpassen van een model met rxLinMod wordt beschreven: [Lineaire modellen aanpassen](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model)

     Als u het model wilt bouwen, definieert u de formule binnen de R-code en geeft u de gegevens door als invoerparameter.

     ```sql
     DROP PROCEDURE IF EXISTS generate_linear_model;
     GO
     CREATE PROCEDURE generate_linear_model
     AS
     BEGIN
       EXEC sp_execute_external_script
       @language = N'R'
       , @script = N'lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
           trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));'
       , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
       , @input_data_1_name = N'CarsData'
       , @output_data_1_name = N'trained_model'
       WITH RESULT SETS ((model VARBINARY(max)));
     END;
     GO
     ```

     Het eerste argument voor rxLinMod is de *formuleparameter*, waarmee afstand wordt gedefinieerd als afhankelijk van snelheid. De invoergegevens worden opgeslagen in de variabele `CarsData`, die wordt gevuld met de SQL-query. Als u geen specifieke naam toewijst aan de invoergegevens, is de standaardnaam voor de variabele _InputDataSet_.

2. Vervolgens maakt u een tabel waarin u het model opslaat, zodat u het opnieuw kunt trainen of kunt gebruiken voor voorspellingen. De uitvoer van een R-pakket waarmee een model wordt gebruikt, is meestal een **binair object**. De tabel moet daarom een kolom bieden van het type **VARBINARY(max)**.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
    );
    ```
3. Als u het model wilt opslaan, voert u de volgende Transact-SQL-instructie uit om de opgeslagen procedure aan te roepen, het model te genereren en op te slaan in een tabel.

    ```sql
    INSERT INTO dbo.stopping_distance_models (model)
    EXEC generate_linear_model;
    ```

    Opmerking: als u deze code een tweede keer uitvoert, krijgt u deze foutmelding:

    ```text
    Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object dbo.stopping_distance_models
    ```

    Eén manier om deze fout te voorkomen is om de naam bij te werken voor elk nieuw model. U kunt de naam bijvoorbeeld wijzigen in een meer beschrijvende naam, en extra info opnemen, zoals het modeltype, de dag waarop het model is gemaakt, enzovoort.

    ```sql
    UPDATE dbo.stopping_distance_models
    SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
    WHERE model_name = 'default model'
    ```

4. Over het algemeen is de uitvoer van R van de opgeslagen procedure [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) beperkt tot één gegevensframe.

    U kunt echter naast het gegevensframe ook uitvoer van andere typen retourneren, zoals scalaire waarden.

    Stel bijvoorbeeld, u wilt een model trainen, maar onmiddellijk een tabel met coëfficiënten van het model bekijken. U kunt de tabel met coëfficiënten maken als de hoofdresultatenset en het getrainde model uitvoeren in een SQL-variabele. U kunt het model onmiddellijk opnieuw gebruiken door de variabele aan te roepen, of u kunt het model opslaan zoals hier wordt weergegeven.

    ```sql
    DECLARE @model VARBINARY(max), @modelname VARCHAR(30)
    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
            speedmodel <- rxLinMod(distance ~ speed, CarsData)
            modelbin <- serialize(speedmodel, NULL)
            OutputDataSet <- data.frame(coefficients(speedmodel));'
        , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
        , @input_data_1_name = N'CarsData'
        , @params = N'@modelbin varbinary(max) OUTPUT'
        , @modelbin = @model OUTPUT
        WITH RESULT SETS (([Coefficient] FLOAT NOT NULL));

    -- Save the generated model
    INSERT INTO dbo.stopping_distance_models(model_name, model)
    VALUES ('latest model', @model)
    ```

    **Results**

    ![Getraind model met extra uitvoer](./media/sql-database-connect-query-r/r-train-model-with-additional-output.png)

## <a name="predict"></a>Voorspellen

Gebruik het model dat u in de vorige sectie hebt gemaakt, om voorspellingen te scoren met nieuwe gegevens. Als u wilt _scoren_ met behulp van nieuwe gegevens, haalt u een van de getrainde modellen op uit de tabel en roept u vervolgens een nieuwe set gegevens aan waarop u voorspellingen baseert. Scoren is een term die in de informatiewetenschap soms wordt gebruikt voor het genereren van voorspellingen, waarschijnlijkheden of andere waarden, op basis van nieuwe gegevens die worden ingevoerd in een getraind model.

1. Maak eerst een tabel met nieuwe snelheidsgegevens. Hebt u opgemerkt dat de oorspronkelijke trainingsgegevens stoppen bij een snelheid van 25 mijl per uur? Dit komt omdat de oorspronkelijke gegevens zijn gebaseerd op een experiment uit 1920. U vraagt zich misschien af hoe lang het duurt om een auto uit de jaren '20 te laten stoppen, ervan uitgaande dat deze 60 mph, of zelfs 100 mph, kan halen. Als u deze vraag wilt beantwoorden, moet u nieuwe waarden opgeven voor de snelheid.

    ```sql
    CREATE TABLE dbo.NewCarSpeed(speed INT NOT NULL,
        distance INT NULL
    )
    GO
    INSERT dbo.NewCarSpeed(speed)
    VALUES (40), (50), (60), (70), (80), (90), (100)
    ```

    In dit voorbeeld roept u de functie [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) aan, in plaats van de generieke R `predict`-functie. Dit doet u omdat het model is gebaseerd op het algoritme **rxLinMod** dat is opgegeven als onderdeel van het **RevoScaleR**-pakket.

    ```sql
    DECLARE @speedmodel varbinary(max) = 
        (SELECT model FROM dbo.stopping_distance_models WHERE model_name = 'latest model');

    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
                current_model <- unserialize(as.raw(speedmodel));
                new <- data.frame(NewCarData);
                predicted.distance <- rxPredict(current_model, new);
                str(predicted.distance);
                OutputDataSet <- cbind(new, ceiling(predicted.distance));
                '
        , @input_data_1 = N'SELECT speed FROM [dbo].[NewCarSpeed]'
        , @input_data_1_name = N'NewCarData'
        , @params = N'@speedmodel varbinary(max)'
        , @speedmodel = @speedmodel
    WITH RESULT SETS ((new_speed INT, predicted_distance INT));
    ```

    Met het bovenstaande script worden de volgende stappen uitgevoerd:

   + Gebruik de SELECT-instructie om een enkel model op te halen uit de tabel, en geef dit model door als invoerparameter.

   + Nadat u het model hebt opgehaald uit de tabel, roept u de `unserialize`-functie van het model aan.

       > [!TIP] 
       > Bekijk ook de nieuwe [serialisatiefuncties](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) die worden geleverd met RevoScaleR. Deze bieden ondersteuning voor scoren in realtime.
   + Pas de `rxPredict`-functie met de juiste argumenten toe op het model en geef de nieuwe invoergegevens op.

   + In het voorbeeld wordt de `str`-functie toegevoegd in de testfase om het schema te controleren van de gegevens die worden geretourneerd met R. U kunt deze instructie later verwijderen.

   + De kolomnamen die worden gebruikt in het R-script, worden niet per se doorgegeven aan de uitvoer van de opgeslagen procedure. Hier hebben we het WITH RESULTS-component gebruikt om een aantal nieuwe kolomnamen te definiëren.

     **Results**

     ![Resultatenset voor het voorspellen van de remafstand](./media/sql-database-connect-query-r/r-predict-stopping-distance-resultset.png)

     U kunt ook [PREDICT in Transact-SQL](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) gebruiken om een voorspelde waarde of score te genereren op basis van een opgeslagen model.

<a name="add-package"></a>

## <a name="add-a-package"></a>Een pakket toevoegen

Als u een pakket wilt gebruiken dat nog niet is geïnstalleerd in de SQL-database, kunt u dit installeren met behulp van [sqlmlutils](https://github.com/Microsoft/sqlmlutils). Volg de onderstaande stappen om het pakket te installeren.

1. Download het meest recente ZIP-bestand **sqlmlutils** uit [github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist) naar de lokale computer. U hoeft het bestand niet uit te pakken.

1. Als R nog niet is geïnstalleerd, downloadt u R op [www.r-project.org](https://www.r-project.org/) en installeert u het op de lokale computer. R is beschikbaar voor Windows, MacOS en Linux. In dit voorbeeld gebruiken we Windows.

1. Installeer eerst het pakket **RODBCext**. Dit pakket is vereist voor **sqlmlutils**. Met **RODBCext** wordt ook het afhankelijke pakket **RODBC** geïnstalleerd. Open een **opdrachtprompt** en voer de volgende opdracht uit:

    ```
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    ```

    Als u de foutmelding ''R' wordt niet herkend als een interne of externe opdracht, programma of batchbestand.', betekent dit waarschijnlijk dat het pad naar R.exe niet is opgenomen in de omgevingsvariabele **PATH** in Windows. U kunt de map toevoegen aan de omgevingsvariabele of u kunt naar de map gaan in de opdrachtprompt (bijvoorbeeld `cd C:\Program Files\R\R-3.5.1\bin`) voordat u de opdracht uitvoert.

1. Gebruik de opdracht **R CMD INSTALL** om **sqlmlutils** te installeren. Geef het pad op naar de map waarin u het ZIP-bestand hebt gedownload, en geef de naam van het ZIP-bestand op. Bijvoorbeeld:

    ```
    R CMD INSTALL C:\Users\youruser\Downloads\sqlmlutils_0.5.0.zip
    ```

    De uitvoer die u krijgt, moet er ongeveer als volgt uitzien:

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/youruser/Documents/R/win-library/3.5'
    package 'sqlmlutils' successfully unpacked and MD5 sums checked
    ```

1. In dit voorbeeld gebruikt u RStudio Desktop als de IDE. Als u liever een andere IDE gebruikt, kan dat ook. Als u RStudio nog niet hebt geïnstalleerd, kunt u RStudio Desktop downloaden en installeren op [www.rstudio.com/products/rstudio/download/](https://www.rstudio.com/products/rstudio/download/).

1. Open **RStudio** en maak een nieuw **R Script**-bestand. Gebruik de volgende R-code om een pakket te installeren met behulp van sqlmlutils. In het onderstaande voorbeeld installeert u het pakket [glue](https://cran.r-project.org/web/packages/glue/). Hiermee kunt u een tekenreeks opmaken en interpoleren.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!NOTE]
    > Het **bereik** kan **PUBLIC** zijn of **PRIVATE**. Een openbaar bereik is handig voor de databasebeheerder om pakketten te installeren die alle gebruikers kunnen gebruiken. Bij een privébereik is het pakket alleen beschikbaar voor de gebruiker die het installeert. Als u het bereik niet opgeeft, is het standaardbereik **PRIVATE**.

1. Controleer nu of het pakket **glue** is geïnstalleerd.

    ```R
    r<-sql_installed.packages(connectionString = connection, fields=c("Package", "LibPath", "Attributes", "Scope"))
    View(r)
    ```

    **Results**

    ![Inhoud van de RTestData-tabel](./media/sql-database-connect-query-r/r-verify-package-install.png)


1. Zodra het pakket is geïnstalleerd, kunt u het gebruiken in uw R-script via **sp_execute_external_script**. Open **SQL Server Management Studio** en maak verbinding met de SQL-database. Voer het volgende script uit:

    ```sql
    EXEC sp_execute_external_script @language = N'R'
    , @script = N'
    library(glue)

    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
        ''my age next year is {age + 1}, '',
        ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    print(text)
    ';
    ```

    U ziet het volgende resultaat op het tabblad Berichten.

    **Results**

    ```text
    STDOUT message(s) from external script:
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

1. Als u het pakket wilt verwijderen, voert u het volgende R-script uit in **RStudio** op de lokale computer.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC") 
    ```

> [!NOTE]
> Een andere manier om R-pakketten te installeren in de SQL-database is om het R-pakket te uploaden vanuit de bytestream met [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).

## <a name="next-steps"></a>Volgende stappen

Zie de onderstaande artikelen over Machine Learning Services voor meer informatie. Sommige van deze artikelen zijn bestemd voor SQL Server, maar de meeste informatie is ook van toepassing op Machine Learning Services (met R) in Azure SQL Database.

- [Azure SQL Database Machine Learning Services (met R)](sql-database-machine-learning-services-overview.md)
- [SQL Server Machine Learning-services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Zelfstudie: Informatie over in-database analyse met behulp van R in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)
- [End-to-end data science walkthrough for R and SQL Server (Overzicht van end-to-end-informatiewetenschap voor R en SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough)
- [Zelfstudie: RevoScaleR R-functies gebruiken met SQL Server-gegevens](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages)
