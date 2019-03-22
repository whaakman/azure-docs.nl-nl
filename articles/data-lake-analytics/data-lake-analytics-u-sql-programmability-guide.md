---
title: Handleiding voor programmeren voor Azure Data Lake U-SQL
description: Meer informatie over de set met services in Azure Data Lake Analytics waarmee u kunt maken van een cloud-gebaseerde big data-platform.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.topic: conceptual
ms.date: 06/30/2017
ms.openlocfilehash: f0f5a4ee5206201cca20e705011126e6cf472a1a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57835449"
---
# <a name="u-sql-programmability-guide"></a>Handleiding voor het programmeren van U-SQL

U-SQL is een querytaal die ontworpen voor big data-type van werkbelastingen. Een van de unieke eigenschappen van U-SQL is de combinatie van de SQL-achtige declaratieve taal met de uitbreidbaarheid en programmeerbaarheid die wordt geleverd door C#. In deze handleiding richten we over de uitbreidbaarheid en programmeermogelijkheden van de U-SQL-taal die wordt ingeschakeld door de C#.

## <a name="requirements"></a>Vereisten

Download en installeer [Azure Data Lake Tools voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="get-started-with-u-sql"></a>Aan de slag met U-SQL  

Bekijk het volgende U-SQL-script:

```
@a  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   1500.0, "2017-03-39"),
       ("Woodgrove", 2700.0, "2017-04-10")
    ) AS D( customer, amount, date );

@results =
  SELECT
    customer,
    amount,
    date
  FROM @a;    
```

Dit script definieert twee rijensets: `@a` en `@results`. De rijenset `@results` is gedefinieerd in `@a`.

## <a name="c-types-and-expressions-in-u-sql-script"></a>C#-typen en expressies in U-SQL-script

Een U-SQL-expressie wordt een C#-expressie in combinatie met logische U-SQL-bewerkingen zoals `AND`, `OR`, en `NOT`. U-SQL-expressies kunnen worden gebruikt met selecteren, uitpakken, waarbij, ONDERVINDT, GEGROEPEERD en DECLAREREN. Het volgende script wordt een tekenreeks geparseerd als een datum / tijdwaarde.

```
@results =
  SELECT
    customer,
    amount,
    DateTime.Parse(date) AS date
  FROM @a;    
```

Een tekenreeks door het volgende codefragment wordt geparseerd als datum/tijd-waarde in een instructie DECLARE.

```
DECLARE @d = DateTime.Parse("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a>C#-expressies gebruiken voor de conversie van het gegevenstype

Het volgende voorbeeld ziet u hoe u de conversie van een datum/tijd-gegevens kunt doen met behulp van C#-expressies. In dit specifieke scenario wordt tekenreeksgegevens datum/tijd geconverteerd naar standard datum/tijd met de notatie van de tijd 00:00:00 middernacht.

```
DECLARE @dt = "2016-07-06 10:23:15";

@rs1 =
  SELECT 
    Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
    dt AS olddt
  FROM @rs0;

OUTPUT @rs1 
  TO @output_file 
  USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a>C#-expressies gebruiken voor de datum van vandaag

Als u wilt ophalen van de datum van vandaag, gebruiken we de volgende C#-expressie: `DateTime.Now.ToString("M/d/yyyy")`

Hier volgt een voorbeeld van hoe u deze expressie in een script gebruiken:

```
@rs1 =
  SELECT
    MAX(guid) AS start_id,
    MIN(dt) AS start_time,
    MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
    MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
    DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
    user,
    des
  FROM @rs0
  GROUP BY user, des;
```
## <a name="using-net-assemblies"></a>Met behulp van .NET-assembly 's

U-SQL-uitbreidingsmodel is sterk afhankelijk van de mogelijkheid om toe te voegen van aangepaste code van .NET-assembly's. 

### <a name="register-a-net-assembly"></a>Registreren van een .NET-assembly

Gebruik de `CREATE ASSEMBLY` instructie voor het plaatsen van een .NET-assembly in een U-SQL-Database. Daarna wordt U-SQL-scripts gebruiken die assembly's met behulp van de `REFERENCE ASSEMBLY` instructie. 

De volgende code toont informatie over het registreren van een assembly:

```
CREATE ASSEMBLY MyDB.[MyAssembly]
   FROM "/myassembly.dll";
```

De volgende code laat zien hoe om te verwijzen naar een assembly:

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

Raadpleeg de [assembly-registratie-instructies](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) die bevat informatie over dit onderwerp in meer detail.


### <a name="use-assembly-versioning"></a>Assembly-versiebeheer gebruiken
Op dit moment gebruikt U-SQL .NET Framework versie 4.5. Dus zorg ervoor dat uw eigen assembly's compatibel met deze versie van de runtime zijn.

Zoals eerder aangegeven, code van U-SQL-wordt uitgevoerd in een 64-bits (x 64)-indeling. Dus zorg ervoor dat uw code wordt gecompileerd om uit te voeren op x64. Anders krijgt u de onjuiste indelingsfout eerder weergegeven.

Elke assembly DLL-bestand geüpload en resource-bestand, zoals een andere runtime, een systeemeigen assembly of een config-bestand mag maximaal 400 MB. De totale grootte van geïmplementeerde resources, via de RESOURCE implementeren of via de verwijzingen naar assembly's en de bijbehorende extra bestanden niet langer zijn dan 3 GB.

Ten slotte Opmerking elke U-SQL-database mag slechts één versie van een bepaalde assembly. Als u zowel versie 7 en 8-versie van de bibliotheek NewtonSoft Json.NET nodig hebt, moet u bijvoorbeeld deze registreren in twee verschillende databases. Bovendien kan elk script alleen verwijzen naar een versie van een bepaalde assembly DLL-bestand. In dit opzicht volgt U-SQL de C#-assembly beheer- en versiebeheer semantiek.

## <a name="use-user-defined-functions-udf"></a>De gebruiker gedefinieerde functies gebruiken: UDF
U-SQL-gebruiker gedefinieerde functies of UDF, programmeert routines die parameters accepteren, het uitvoeren van een actie (zoals een complexe berekening) en het resultaat van de actie als een waarde. De geretourneerde waarde van UDF mag alleen één scalair. U-SQL-UDF kan worden aangeroepen in U-SQL-Standaardscript net als elke andere C# scalaire functie.

Het is raadzaam dat u de gebruiker gedefinieerde U-SQL-functies als initialiseren **openbare** en **statische**.

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

Eerste we kijken naar het eenvoudige voorbeeld van het maken van een UDF.

In deze gebruiksscenario's moeten we de fiscale periode, met inbegrip van het kwartaal en fiscale maand van de eerste aanmelding voor de specifieke gebruiker te bepalen. De eerste fiscale maand van het jaar in ons scenario is juni.

Voor het berekenen van periode, stellen we de volgende C#-functie:

```
public static string GetFiscalPeriod(DateTime dt)
{
    int FiscalMonth=0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter=0;
    if (FiscalMonth >=1 && FiscalMonth<=3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
}
```

Het gewoon berekent fiscale maand en het kwartaal en retourneert een string-waarde. Voor juni, de eerste maand van het eerste kwartaal, gebruiken we 'Q1:P1'. Voor juli, we gebruiken 'Q1:P2', enzovoort.

Dit is een reguliere C#-functie die we gaan gebruiken in de U-SQL-project.

Hier ziet u hoe de code-behind-sectie eruit ziet in dit scenario:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }
    }
}
```

Nu gaan we deze functie aanroepen vanuit het standaardscript van U-SQL. Om dit te doen, hebben we een volledig gekwalificeerde naam voor de functie, met inbegrip van de naamruimte die in dit geval NameSpace.Class.Function(parameter) is op te geven.

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Hieronder vindt u de werkelijke basis U-SQL-script:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt DateTime,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

Hieronder vindt u het bestand voor uitvoer van de uitvoering van het script:

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

In dit voorbeeld ziet u een eenvoudige gebruik van inline UDF in U-SQL.

### <a name="keep-state-between-udf-invocations"></a>Status tussen UDF aanroepen bijhouden
U-SQL C# programmeerbaarheid objecten kunnen worden meer geavanceerde, met behulp van interactiviteit via de code-behind globale variabelen. Bekijk de volgende zakelijke gebruiksscenario's.

In grote organisaties kunnen gebruikers schakelen tussen varianten van interne toepassingen. Hierbij kan Microsoft Dynamics CRM, Power BI, enzovoort. Klanten zou willen toepassen van een analyse van de telemetrie van hoe gebruikers tussen verschillende toepassingen schakelen, wat de trends in gebruik zijn, enzovoort. Het doel van het bedrijf is het Optimaliseer gebruik van de toepassing. Ze mogelijk willen ook verschillende toepassingen of specifieke aanmelding routines te combineren.

Als u wilt dit doel te bereiken, hebben we om te bepalen van de sessie-id's en vertraging tussen de laatste sessie die is opgetreden.

Er moet een vorige aanmelding bij zoeken en wijs deze aanmelding voor alle sessies die worden gegenereerd voor dezelfde toepassing. De eerste uitdaging is dat U-SQL-Standaardscript niet kan we berekeningen via al berekende kolommen met de functie LAG van toepassing. De tweede uitdaging is dat we beschikken over de specifieke sessie voor alle sessies binnen de dezelfde periode houden.

U lost dit probleem, gebruiken we een globale variabele in een sectie code-behind: `static public string globalSession;`.

Deze globale variabele wordt toegepast op de hele rijenset tijdens de uitvoering van het script.

Dit is de code-behind-sectie van het U-SQL-programma:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60) {return Session;}
                else {return Guid.NewGuid().ToString();}
            }
            else {return Guid.NewGuid().ToString();}

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session)) { globalSession = Session; }
            return globalSession;
        }

    }
}
```

In dit voorbeeld ziet u de globale variabele `static public string globalSession;` gebruikt binnen de `getStampUserSession` functie en het ophalen van opnieuw initialiseren telkens wanneer de sessie-parameter is gewijzigd.

De U-SQL-Standaardscript is als volgt:

```
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT 
        EventDateTime,
        UserName,
    LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
        string.IsNullOrEmpty(LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
        USQLApplication21.UserSession.StampUserSession
           (
            EventDateTime,
            LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
            LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           ) AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT 
        EventDateTime,
        UserName,
        LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
        string.IsNullOrEmpty( LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
        USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
    TO @out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

Functie `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` hier tijdens de berekening van de tweede geheugen rijenset wordt genoemd. Dit wordt doorgegeven de `UserSessionTimestamp` kolom en retourneert de waarde pas `UserSessionTimestamp` is gewijzigd.

Het uitvoerbestand is als volgt:

```
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

In dit voorbeeld ziet u een meer complexe gebruiksscenario's waarop we een globale variabele gebruiken in een code-behind-sectie die wordt toegepast op de hele geheugen-rijenset.

## <a name="use-user-defined-types-udt"></a>Gebruik de gebruiker gedefinieerde typen: UDT
Gebruiker gedefinieerde typen of UDT, is een andere programmeerbaarheidsfunctie van U-SQL. U-SQL-UDT fungeert als een reguliere C# gebruiker gedefinieerd type. C# is een sterk getypeerde taal waarin het gebruik van ingebouwde en aangepaste gebruiker gedefinieerde typen.

U-SQL kan niet impliciet serialiseren of deserialiseren willekeurige UDT's wanneer de UDT tussen hoekpunten in rijensets wordt doorgegeven. Dit betekent dat de gebruiker heeft te bieden van een expliciete indelingsfunctie met behulp van de IFormatter-interface. Dit biedt U-SQL met het serialiseren en deserialiseren methoden voor het UDT.

> [!NOTE]
> Serialiseren U-SQL ingebouwde extractors en outputters op dit moment kunnen niet of deserialiseren UDT gegevens naar of van bestanden, zelfs met de set IFormatter. Wanneer u UDT gegevens schrijven naar een bestand met de uitvoer-instructie, of met een extractor lezen, hebt u dus geven deze als een tekenreeks of matrix van bytes. Vervolgens aanroepen van de serialisatie en deserialisatie van code (dat wil zeggen, van de UDT ToString() methode) expliciet. Aangepaste extractors en outputters, aan de andere kant kunnen lezen en schrijven UDT's.

Als we gebruiken UDT in EXTRACTOR of OUTPUTTER (van vorige selecteren), willen zoals hier wordt weergegeven:

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

De volgende fout ontvangen:

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how to serialize this type, or call a serialization method on the type in
the preceding SELECT.   C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

Als u wilt werken met UDT in outputter, hebben we het serialiseren van deze tekenreeks met de methode ToString() of maak een aangepaste outputter.

UDT's worden momenteel niet gebruikt in GROUP BY. Als UDT wordt gebruikt in GROUP BY, wordt de volgende fout gegenereerd:

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want to use with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

Voor het definiëren van een UDT, hebben we naar:

* Voeg de volgende naamruimten:

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* Voeg `Microsoft.Analytics.Interfaces`, die is vereist voor de UDT-interfaces. Bovendien `System.IO` nodig zijn voor het definiëren van de IFormatter-interface.

* Een type gebruikt gedefinieerde met SqlUserDefinedType kenmerk definiëren.

**SqlUserDefinedType** wordt gebruikt voor het markeren van de definitie van een type in een assembly als een door gebruiker gedefinieerde typen (UDT) in U-SQL. De eigenschappen van het kenmerk weerspiegelen de fysieke kenmerken van de UDT. Deze klasse kan niet worden overgenomen.

SqlUserDefinedType is een vereist kenmerk voor de definitie van UDT.

De constructor van de klasse:  

* SqlUserDefinedTypeAttribute (type-indeling)

* Type-indeling: Vereiste parameter voor het definiëren van een indelingsfunctie UDT--name, het type van de `IFormatter` interface hier moet worden doorgegeven.

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* Typische UDT vereist ook definitie van de interface IFormatter, zoals wordt weergegeven in het volgende voorbeeld:

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

De `IFormatter` interface serialiseert en gedeserialiseerd een objectgrafiek met het type van de hoofdmap van \<typeparamref name = "T" >.

\<typeparam name = "T" > het root-type voor de objectgrafiek voor het serialiseren en deserialiseren.

* **Deserialiseren**: Gedeserialiseerd op de gegevens op de opgegeven stroom en reconstitutes van de graph-objecten.

* **Serialiseren**: Serialiseert een object, of een grafiek van objecten met de opgegeven hoofdmap van de opgegeven stroom.

`MyType` Exemplaar: Het exemplaar van het type.  
`IColumnWriter` schrijver / `IColumnReader` lezer: De stroom van de onderliggende kolommen.  
`ISerializationContext` Context: Enum die definieert een set van vlaggen waarmee de bron- of -context voor de stroom tijdens de serialisatie.

* **Tussenliggende**: Hiermee geeft u de bron- of -context is een persistente archief.

* **Persistentie**: Hiermee geeft u op dat de bron- of -context een persistente archief is.

Als een reguliere C#-type, de definitie van een U-SQL-UDT kunt opnemen onderdrukkingen voor operators zoals +/ == /! =. Het kan ook betekenen statische methoden. Bijvoorbeeld, als we gaan deze UDT gebruiken als een parameter voor een statistische functie MIN U-SQL, we hebben voor het definiëren van < operator overschrijven.

Eerder in deze handleiding wordt een voorbeeld van fiscale periode-id van de specifieke datum in de notatie aangetoond `Qn:Pn (Q1:P10)`. Het volgende voorbeeld ziet hoe u een aangepast type voor fiscale waarden definieert.

Hieronder volgt een voorbeeld van een code-behind-sectie met aangepaste UDT en IFormatter interface:

```
[SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
public struct FiscalPeriod
{
    public int Quarter { get; private set; }

    public int Month { get; private set; }

    public FiscalPeriod(int quarter, int month):this()
    {
    this.Quarter = quarter;
    this.Month = month;
    }

    public override bool Equals(object obj)
    {
    if (ReferenceEquals(null, obj))
    {
        return false;
    }

    return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
    }

    public bool Equals(FiscalPeriod other)
    {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
    }

    public bool GreaterThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
    }

    public bool LessThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
    }

    public override int GetHashCode()
    {
    unchecked
    {
        return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
    }
    }

    public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
    {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
    }

    public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.Equals(c2);
    }

    public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
    {
    return !c1.Equals(c2);
    }
    public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.GreaterThan(c2);
    }
    public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.LessThan(c2);
    }
    public override string ToString()
    {
    return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
    }

}

public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
{
    public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
    {
    using (var binaryWriter = new BinaryWriter(writer.BaseStream))
    {
        binaryWriter.Write(instance.Quarter);
        binaryWriter.Write(instance.Month);
        binaryWriter.Flush();
    }
    }

    public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
    {
    using (var binaryReader = new BinaryReader(reader.BaseStream))
    {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
        return result;
    }
    }
}
```

Het gedefinieerde type bevat twee getallen: kwartaal en maand. Operators `==/!=/>/<` en statische methode `ToString()` hier zijn gedefinieerd.

Zoals eerder vermeld, UDT kan worden gebruikt in expressies voor selecteert, maar kan niet worden gebruikt in OUTPUTTER/EXTRACTOR zonder aangepaste serialisatie. Het bevat worden geserialiseerd als een tekenreeks met `ToString()` of gebruikt in combinatie met een aangepaste OUTPUTTER/EXTRACTOR.

Nu gaan we bespreken gebruik van UDT. In een sectie code-behind we onze GetFiscalPeriod functie gewijzigd in het volgende:

```
public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
{
    int FiscalMonth = 0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter = 0;
    if (FiscalMonth >= 1 && FiscalMonth <= 3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return new FiscalPeriod(FiscalQuarter, FiscalMonth);
}
```

Zoals u ziet, wordt de waarde van onze FiscalPeriod type geretourneerd.

We bieden hier een voorbeeld van hoe u verder in U-SQL-Standaardscript. In dit voorbeeld ziet u verschillende vormen van UDT aanroep van U-SQL-script.

```
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT 
        guid AS start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
        user,
        des
    FROM @rs0;

@rs2 =
    SELECT 
           start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           fiscalquarter,
           fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,

       // This user-defined type was created in the prior SELECT.  Passing the UDT to this subsequent SELECT would have failed if the UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

Hier volgt een voorbeeld van een volledige code-behind-sectie:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }



        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
    }
}
```

## <a name="use-user-defined-aggregates-udagg"></a>Gebruik de gebruiker gedefinieerde aggregaties: UDAGG
De gebruiker gedefinieerde verzamelingen zijn een aggregatie-functies die zijn verzonden niet out-of-the-box met U-SQL. Het voorbeeld is een statistische functie uit te voeren aangepaste wiskundige berekeningen, samenvoegingen van tekenreeksen, bewerkingen met tekenreeksen, enzovoort.

De definitie van de gebruiker gedefinieerde cumulatieve basisklasse is als volgt:

```csharp
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate** geeft aan dat het type moet worden geregistreerd als een gebruiker gedefinieerde aggregatie. Deze klasse kan niet worden overgenomen.

SqlUserDefinedType kenmerk is **optionele** voor UDAGG definitie.


De basisklasse kunt u drie abstracte parameters doorgeven: twee invoerparameters en als het resultaat. De gegevenstypen zijn variabel en moeten worden gedefinieerd tijdens de klassenovername van de.

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    { … }

    public override void Accumulate(string guid, string user)
    { … }

    public override string Terminate()
    { … }
}
```

* **Init** roept één keer voor elke groep tijdens de berekening. Het biedt een initialisatieroutine voor elke aggregatie-groep.  
* **Verzamelt** één keer voor elke waarde wordt uitgevoerd. Het biedt de belangrijkste functionaliteit voor het algoritme voor aggregatie. Het kan worden gebruikt voor samengevoegde waarden met verschillende soorten gegevens die tijdens de klassenovername van zijn gedefinieerd. Twee parameters van de variabele gegevenstypen kunnen worden geaccepteerd.
* **Beëindigen** eenmaal per groep van de aggregatie aan het einde van de verwerking van voor de uitvoer van het resultaat voor elke groep wordt uitgevoerd.

Om te declareren juiste invoer en uitvoer gegevenstypen, gebruikt u de klassendefinitie van de als volgt:

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: De eerste parameter voor het verzamelen
* T2: Tweede parameter verzamelen
* TResult: Retourtype van beëindigen

Bijvoorbeeld:

```
public class GuidAggregate : IAggregate<string, int, int>
```

of

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>UDAGG in U-SQL gebruiken
Voor het gebruik van UDAGG eerst definiëren in de code-behind of verwijzen naar van de bestaande programmeerbaarheid DLL zoals eerder is besproken.

Gebruik vervolgens de volgende syntaxis:

```
AGG<UDAGG_functionname>(param1,param2)
```

Hier volgt een voorbeeld van UDAGG:

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    {
        guid_agg = "";
    }

    public override void Accumulate(string guid, string user)
    {
        if (user.ToUpper()== "USER1")
        {
        guid_agg += "{" + guid + "}";
        }
    }

    public override string Terminate()
    {
        return guid_agg;
    }

}
```

En U-SQL-script:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

In dit scenario use-casescenario samenvoegen we klasse GUID's voor de specifieke gebruikers.

## <a name="use-user-defined-objects-udo"></a>Gebruik de gebruiker gedefinieerde objecten: UDO
U-SQL kunt u aangepaste programmeerbaarheid objecten, die worden aangeroepen gebruiker gedefinieerde objecten of UDO definiëren.

Hier volgt een lijst met UDO in U-SQL:

* Aangepaste extractors
    * Extract per rij
    * Gebruikt voor het ophalen van gegevens uit bestanden met aangepaste gestructureerde implementeren

* Gebruiker gedefinieerde outputters
    * Uitvoer per rij
    * Gebruikt voor het uitvoertypen voor aangepaste gegevens of aangepaste indelingen

* Gebruiker gedefinieerde-processors
    * Één rij en één rij produceert
    * Gebruikt voor het verminderen van het aantal kolommen of produceren van nieuwe kolommen met waarden die zijn afgeleid van een bestaande set kolommen

* Gebruiker gedefinieerde appliers
    * Één rij en produceert van 0 tot en met n rijen
    * Gebruikt met buitenste/CROSS toepassen

* Gebruiker gedefinieerde combiners
    * Combineert rijensets--gebruiker gedefinieerde JOINs

* Gebruiker gedefinieerde reducers tegelijkertijd
    * N rijen en produceert één rij
    * Gebruikt voor het verminderen van het aantal rijen

UDO wordt doorgaans expliciet genoemd in U-SQL-script als onderdeel van de volgende U-SQL-instructies:

* EXTRACT
* UITVOER
* PROCES
* COMBINEREN
* BEPERKEN

> [!NOTE]  
> De UDO zijn beperkt tot 0,5 Gb geheugen in beslag nemen.  Deze beperking geheugen is niet van toepassing op lokale uitvoeringen.

## <a name="use-user-defined-extractors"></a>Gebruik van de gebruiker gedefinieerde extractors
U-SQL kunt u externe gegevens importeren met behulp van een instructie EXTRAHEREN. Een instructie EXTRACT kunt ingebouwde UDO extractors gebruiken:  

* *Extractors.Text()*: Extractie van tekstbestanden met scheidingstekens van verschillende coderingen biedt.

* *Extractors.Csv()*: Extractie uit met door komma's gescheiden waarden biedt (CSV)-bestanden van verschillende coderingen.

* *Extractors.Tsv()*: Extractie van tabblad gescheiden waarden biedt (TSV)-bestanden van verschillende coderingen.

Kan het nuttig zijn voor het ontwikkelen van een aangepaste extractor. Dit kan nuttig zijn tijdens het importeren van gegevens als we willen een van de volgende taken:

* Invoergegevens wijzigen door het splitsen van kolommen en afzonderlijke waarden te wijzigen. De PROCESSOR-functionaliteit is het beter om kolommen te combineren.
* Niet-gestructureerde gegevens, zoals webpagina's en e-mailberichten of niet semi-gestructureerde gegevens, zoals XML/JSON parseren.
* Gegevens in niet-ondersteunde codering parseren.

Voor het definiëren van een gebruiker gedefinieerde extractor of opnemen, moeten we maken een `IExtractor` interface. Alle parameters voor de extractor, zoals rij/scheidingstekens, invoer- en codering, moeten worden gedefinieerd in de constructor van de klasse. De `IExtractor` interface mag ook een definitie voor de `IEnumerable<IRow>` overschrijven als volgt:

```
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

De **SqlUserDefinedExtractor** kenmerk geeft aan dat het type moet worden geregistreerd als een gebruiker gedefinieerde extractor. Deze klasse kan niet worden overgenomen.

SqlUserDefinedExtractor is een optioneel kenmerk voor de definitie van opnemen. Het wordt gebruikt voor het definiëren van AtomicFileProcessing-eigenschap voor het object uit.

* bool     AtomicFileProcessing   

* **de waarde True** = geeft aan dat voor deze extractor atomic invoerbestanden (JSON, XML,...) is vereist
* **de waarde False** = geeft aan dat deze extractor kunt omgaan met splitsen / gedistribueerde bestanden (CSV, SEQ,...)

De belangrijkste USIEF programmeerbaarheid objecten zijn **invoer** en **uitvoer**. Het invoerobject wordt gebruikt om op te sommen invoergegevens als `IUnstructuredReader`. De uitvoerobject wordt gebruikt om in te stellen uitvoergegevens als gevolg van de activiteit extractor.

De ingevoerde gegevens toegankelijk is via `System.IO.Stream` en `System.IO.StreamReader`.

Voor invoerkolommen opsomming splitsen we eerst de invoerstroom met behulp van een rijscheidingsteken.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Vervolgens verder invoer rij in kolom delen splitsen.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

Om in te stellen uitvoergegevens, gebruiken we de `output.Set` methode.

Het is belangrijk om te begrijpen dat de aangepaste extractor alleen levert kolommen en waarden die zijn gedefinieerd met de uitvoer. Methodeaanroep instellen.

```
output.Set<string>(count, part);
```

De uitvoer van de werkelijke extractor wordt geactiveerd door het aanroepen van `yield return output.AsReadOnly();`.

Hieronder volgt de extractor-voorbeeld:

```
[SqlUserDefinedExtractor(AtomicFileProcessing = true)]
public class FullDescriptionExtractor : IExtractor
{
     private Encoding _encoding;
     private byte[] _row_delim;
     private char _col_delim;

    public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
    {
         this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
         this._row_delim = this._encoding.GetBytes(row_delim);
         this._col_delim = col_delim;

    }

    public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
    {
         string line;
         //Read the input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split the input by the column delimiter
             string[] parts = line.Split(this._col_delim);
             int count = 0; // start with first column
             foreach (string part in parts)
             {
    if (count == 0)
             {  // for column “guid”, re-generated guid
                 Guid new_guid = Guid.NewGuid();
                 output.Set<Guid>(count, new_guid);
             }
             else if (count == 2)
             {
                 // for column “user”, convert to UPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep the rest of the columns as-is
                 output.Set<string>(count, part);
             }
             count += 1;
             }

         }
         yield return output.AsReadOnly();
         }
         yield break;
     }
}
```

In dit scenario use-casescenario de extractor worden opnieuw gegenereerd door de GUID voor kolom 'guid' en 'gebruiker' kolomwaarden omgezet in hoofdletters. Aangepaste extractors kunnen meer gecompliceerde resultaten opleveren met invoergegevens parseren en manipuleren van deze.

Hieronder vindt u base U-SQL-script dat gebruikmaakt van een aangepaste extractor:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file
        USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-outputters"></a>Gebruik van de gebruiker gedefinieerde outputters
Gebruiker gedefinieerde outputter is nog een U-SQL-UDO waarmee u de ingebouwde U-SQL-functionaliteit uitbreiden. Net als bij de extractor, er zijn verschillende ingebouwde outputters.

* *Outputters.Text()*: Schrijft gegevens naar tekstbestanden met scheidingstekens van verschillende coderingen.
* *Outputters.Csv()*: Schrijft gegevens naar een door komma's gescheiden waarden (CSV)-bestanden van verschillende coderingen.
* *Outputters.Tsv()*: Schrijft gegevens naar het tabblad gescheiden waarden (TSV)-bestanden van verschillende coderingen.

Aangepaste outputter kunt u gegevens in een aangepaste gedefinieerde indeling wilt schrijven. Dit kan nuttig zijn voor de volgende taken zijn:

* Gegevens schrijven naar een semi-gestructureerde of ongestructureerde-bestanden.
* Coderingen schrijven van gegevens niet worden ondersteund.
* Wijzigen van uitvoergegevens of aangepaste kenmerken toe te voegen.

Voor het definiëren van de gebruiker gedefinieerde outputter moeten we maken de `IOutputter` interface.

Hieronder volgt de base `IOutputter` implementatie van klasse:

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

Alle invoerparameters die zijn opgegeven voor de outputter, zoals rij/scheidingstekens, codering, enzovoort, moeten worden gedefinieerd in de constructor van de klasse. De `IOutputter` interface moet ook een definitie voor bevatten `void Output` overschrijven. Het kenmerk `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` kan eventueel worden ingesteld voor atomic bestandsverwerking. Zie de volgende details voor meer informatie.

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class MyOutputter : IOutputter
{

    public MyOutputter(myparam1, myparam2)
    {
      …
    }

    public override void Close()
    {
      …
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
      …
    }
}
```

* `Output` voor elke rij met invoer wordt genoemd. Retourneert de `IUnstructuredWriter output` rijenset.
* De Constructor-klasse wordt gebruikt om de parameters doorgeven aan de gebruiker gedefinieerde outputter.
* `Close` wordt gebruikt om eventueel negeren als u wilt vrijgeven duur status of te bepalen wanneer de laatste rij is geschreven.

**SqlUserDefinedOutputter** kenmerk geeft aan dat het type moet worden geregistreerd als een gebruiker gedefinieerde outputter. Deze klasse kan niet worden overgenomen.

SqlUserDefinedOutputter is een optioneel kenmerk voor de definitie van een gebruiker gedefinieerde outputter. Wordt gebruikt om de eigenschap AtomicFileProcessing te definiëren.

* bool     AtomicFileProcessing   

* **de waarde True** = geeft aan dat voor deze outputter atomic uitvoerbestanden (JSON, XML,...) is vereist
* **de waarde False** = geeft aan dat deze outputter kunt omgaan met splitsen / gedistribueerde bestanden (CSV, SEQ,...)

De belangrijkste programmeerbaarheid objecten zijn **rij** en **uitvoer**. De **rij** object wordt gebruikt om inventariseren uitvoergegevens als `IRow` interface. **Uitvoer** wordt gebruikt voor het instellen van uitvoergegevens naar het doelbestand.

De uitvoergegevens toegankelijk is via de `IRow` interface. Uitvoergegevens wordt een rij op een tijdstip doorgegeven.

De afzonderlijke waarden worden geïnventariseerd door het aanroepen van de Get-methode van de interface IRow:

```
row.Get<string>("column_name")
```

Afzonderlijke kolomnamen kunnen worden bepaald door het aanroepen van `row.Schema`:

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Deze aanpak kunt u een flexibele outputter voor elke metagegevensschema bouwen.

De uitvoergegevens worden geschreven naar het bestand met behulp van `System.IO.StreamWriter`. De stream-parameter is ingesteld op `output.BaseStream` als onderdeel van `IUnstructuredWriter output`.

Houd er rekening mee dat het is belangrijk om de gegevensbuffer voor het bestand leegmaken na elke iteratie rij. Bovendien de `StreamWriter` object moet worden gebruikt met de beschikbare kenmerk ingeschakeld (standaard) en de **met behulp van** sleutelwoord:

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

Anders Flush() methode aanroepen expliciet na elke iteratie. We zien in het volgende voorbeeld.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Kopteksten en voetteksten ingesteld voor de gebruiker gedefinieerde outputter
Om in te stellen een koptekst, één iteratie tot uitvoering van flow te gebruiken.

```
public override void Output(IRow row, IUnstructuredWriter output)
{
 …
if (isHeaderRow)
{
    …                
}

 …
if (isHeaderRow)
{
    isHeaderRow = false;
}
 …
}
}
```

De code in de eerste `if (isHeaderRow)` blok wordt slechts één keer uitgevoerd.

Voor de voettekst, gebruikt u de verwijzing naar het exemplaar van `System.IO.Stream` object (`output.BaseStream`). Schrijven van de voettekst in de methode Close() van de `IOutputter` interface.  (Zie het volgende voorbeeld voor meer informatie.)

Hieronder volgt een voorbeeld van een gebruiker gedefinieerde outputter:

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class HTMLOutputter : IOutputter
{
    // Local variables initialization
    private string row_delimiter;
    private char col_delimiter;
    private bool isHeaderRow;
    private Encoding encoding;
    private bool IsTableHeader = true;
    private Stream g_writer;

    // Parameters definition            
    public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    this.isHeaderRow = isHeader;
    this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
    }

    // The Close method is used to write the footer to the file. It's executed only once, after all rows
    public override void Close()
    {
    //Reference to IO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization to enumerate column names
    ISchema schema = row.Schema;

    // This is a data-independent header--HTML table definition
    if (IsTableHeader)
    {
        streamWriter.Write("<table border=1>");
        IsTableHeader = false;
    }

    // HTML table attributes
    string header_wrapper_on = "<th>";
    string header_wrapper_off = "</th>";
    string data_wrapper_on = "<td>";
    string data_wrapper_off = "</td>";

    // Header row output--runs only once
    if (isHeaderRow)
    {
        streamWriter.Write("<tr>");
        for (int i = 0; i < schema.Count(); i++)
        {
        var col = schema[i];
        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
        }
        streamWriter.Write("</tr>");
    }

    // Data row output
    streamWriter.Write("<tr>");                
    for (int i = 0; i < schema.Count(); i++)
    {
        var col = schema[i];
        string val = "";
        try
        {
        // Data type enumeration--required to match the distinct list of types from OUTPUT statement
        switch (col.Type.Name.ToString().ToLower())
        {
            case "string": val = row.Get<string>(col.Name).ToString(); break;
            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
            default: break;
        }
        }
        // Handling NULL values--keeping them empty
        catch (System.NullReferenceException)
        {
        }
        streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
    }
    streamWriter.Write("</tr>");

    if (isHeaderRow)
    {
        isHeaderRow = false;
    }
    // Reference to the instance of the IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define the factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

En U-SQL-Standaardscript:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
         FROM @input_file
         USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 
    TO @output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Dit is een HTML-outputter, die zorgt voor een HTML-bestand met gegevens in een tabel.

### <a name="call-outputter-from-u-sql-base-script"></a>Outputter aanroepen vanuit de Standaardscript U-SQL
Als u een aangepaste outputter vanuit het standaardscript van U-SQL, heeft het nieuwe exemplaar van het object outputter moet worden gemaakt.

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Om te voorkomen maakt u een instantie van het object in Standaardscript, kunnen we een functie-wrapper maken zoals wordt weergegeven in het eerdere voorbeeld:

```csharp
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

In dit geval de oorspronkelijke aanroep ziet er als volgt uit:

```
OUTPUT @rs0 
TO @output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a>Gebruiker gedefinieerde processors gebruiken
Gebruiker gedefinieerde processor- of UDP, is een U-SQL-UDO waarmee u de binnenkomende om rijen te verwerken door toe te passen programmeerfuncties. UDP kunt u kolommen samenvoegen, waarden wijzigen en nieuwe kolommen toevoegen, indien nodig. In principe kunt zo u het verwerken van een rijenset vereiste gegevenselementen produceren.

Voor het definiëren van een UDP, moeten we maken een `IProcessor` interface met de `SqlUserDefinedProcessor` kenmerk, die optioneel voor UDP is.

Deze interface mag de definitie voor de `IRow` interface rijenset overschrijven, zoals wordt weergegeven in het volgende voorbeeld:

```
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

**SqlUserDefinedProcessor** geeft aan dat het type moet worden geregistreerd als een gebruiker gedefinieerde processor. Deze klasse kan niet worden overgenomen.

Het kenmerk SqlUserDefinedProcessor **optionele** voor UDP-definitie.

De belangrijkste programmeerbaarheid objecten zijn **invoer** en **uitvoer**. Het invoerobject wordt gebruikt om te inventariseren kolommen voor invoer en uitvoer, en om in te stellen uitvoergegevens als gevolg van de processoractiviteit.

Voor invoerkolommen opsomming, gebruiken we de `input.Get` methode.

```
string column_name = input.Get<string>("column_name");
```

De parameter voor `input.Get` methode is een kolom die wordt doorgegeven als onderdeel van de `PRODUCE` -component van de `PROCESS` -instructie van de U-SQL-Standaardscript. We moeten het juiste gegevenstype hier gebruiken.

Gebruik voor de uitvoer van de `output.Set` methode.

Het is belangrijk te weten dat aangepaste producent voert alleen kolommen en waarden die zijn gedefinieerd met de `output.Set` aanroep van methode.

```
output.Set<string>("mycolumn", mycolumn);
```

De uitvoer van de werkelijke processor wordt geactiveerd door het aanroepen van `return output.AsReadOnly();`.

Hieronder volgt een voorbeeld van de processor:

```
[SqlUserDefinedProcessor]
public class FullDescriptionProcessor : IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
     string user = input.Get<string>("user");
     string des = input.Get<string>("des");
     string full_description = user.ToUpper() + "=>" + des;
     output.Set<string>("dt", input.Get<string>("dt"));
     output.Set<string>("full_description", full_description);
     output.Set<Guid>("new_guid", Guid.NewGuid());
     output.Set<Guid>("guid", input.Get<Guid>("guid"));
     return output.AsReadOnly();
 }
}
```

In dit scenario use case is de processor genereren van een nieuwe kolom met de naam 'full_description' door een combinatie van de bestaande kolommen--in dit geval 'gebruiker' in hoofdletters, en 'des'. Ook worden opnieuw gegenereerd door een GUID en de oorspronkelijke en nieuwe GUID-waarden worden geretourneerd.

Zoals u in het vorige voorbeeld zien kunt, kunt u C# methoden tijdens aanroepen `output.Set` aanroep van methode.

Hieronder volgt een voorbeeld van base U-SQL-script dat gebruikmaakt van een aangepaste processor:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-appliers"></a>Gebruik van de gebruiker gedefinieerde appliers
Een gebruiker gedefinieerde applier van U-SQL kunt u een aangepaste C#-functie voor elke rij die wordt geretourneerd door de buitenste tabelexpressie van een query aanroepen. De juiste invoer wordt geëvalueerd voor elke rij van de linkerinvoer en de rijen die worden geproduceerd worden gecombineerd voor de uiteindelijke uitvoer. De lijst met kolommen die worden geproduceerd door de operator APPLY is de combinatie van de set kolommen in de linker- en de juiste invoer.

Gebruiker gedefinieerde applier wordt aangeroepen als onderdeel van de expressie USQL selecteren.

De typische aanroep van de gebruiker gedefinieerde applier ziet er als volgt uit:

```
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Zie voor meer informatie over het gebruik van appliers in een SELECT-expressie [U-SQL Selecteer selecteren in de CROSS APPLY en OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx).

De gebruiker gedefinieerde applier basisklasse-definitie is als volgt:

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

Voor het definiëren van een gebruiker gedefinieerde applier, moeten we maken de `IApplier` interface met de [`SqlUserDefinedApplier`] kenmerk, die optioneel voor een gebruiker gedefinieerde applier definitie is.

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
    public ParserApplier()
    {
        …
    }

    public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
    {
        …
    }
}
```

* Van toepassing voor elke rij van de buitenste tabel wordt genoemd. Retourneert de `IUpdatableRow` uitvoer van de rijenset.
* De Constructor-klasse wordt gebruikt om de parameters doorgeven aan de gebruiker gedefinieerde applier.

**SqlUserDefinedApplier** geeft aan dat het type moet worden geregistreerd als een gebruiker gedefinieerde applier. Deze klasse kan niet worden overgenomen.

**SqlUserDefinedApplier** is **optionele** voor een gebruiker gedefinieerde applier definitie.


De belangrijkste programmeerbaarheid objecten zijn als volgt:

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Invoer rijensets worden doorgegeven als `IRow` invoer. De uitvoer-rijen worden gegenereerd als `IUpdatableRow` interface voor resultaatuitvoer.

Afzonderlijke kolomnamen kunnen worden bepaald door het aanroepen van de `IRow` Schema-methode.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

De werkelijke waarden ophalen uit de binnenkomende `IRow`, gebruiken we de methode voor Get() `IRow` interface.

```
mycolumn = row.Get<int>("mycolumn")
```

Of we de naam van de schema-kolom gebruiken:

```
row.Get<int>(row.Schema[0].Name)
```

De uitvoerwaarden moeten worden ingesteld met `IUpdatableRow` uitvoer:

```
output.Set<int>("mycolumn", mycolumn)
```

Het is belangrijk om te begrijpen dat aangepaste appliers alleen kolommen en waarden die zijn gedefinieerd met uitvoer `output.Set` aanroep van methode.

De werkelijke uitvoer wordt geactiveerd door het aanroepen van `yield return output.AsReadOnly();`.

De gebruiker gedefinieerde applier parameters kunnen worden doorgegeven aan de constructor. Applier kunt een variabele aantal kolommen die moeten worden gedefinieerd tijdens het aanroepen van applier in U-SQL-Standaardscript retourneren.

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Hier volgt de gebruiker gedefinieerde applier voorbeeld:

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
private string parsingPart;

public ParserApplier(string parsingPart)
{
    if (parsingPart.ToUpper().Contains("ALL")
    || parsingPart.ToUpper().Contains("MAKE")
    || parsingPart.ToUpper().Contains("MODEL")
    || parsingPart.ToUpper().Contains("YEAR")
    || parsingPart.ToUpper().Contains("TYPE")
    || parsingPart.ToUpper().Contains("MILLAGE")
    )
    {
    this.parsingPart = parsingPart;
    }
    else
    {
    throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
    }
}

public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
{

    string[] properties = input.Get<string>("properties").Split(',');

    //  only process with correct number of properties
    if (properties.Count() == 5)
    {

    string make = properties[0];
    string model = properties[1];
    string year = properties[2];
    string type = properties[3];
    int millage = -1;

    // Only return millage if it is number, otherwise, -1
    if (!int.TryParse(properties[4], out millage))
    {
        millage = -1;
    }

    if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
    if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
    if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
    if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
    if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
    }
    yield return output.AsReadOnly();            
}
}
```

Hieronder vindt u het standaardscript U-SQL voor deze gebruiker gedefinieerde applier:

```
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

In deze use-casescenario fungeert applier zelfgedefinieerde als een door komma's gescheiden waarden parser voor de vloot auto-eigenschappen. De rijen invoerbestand er uitzien als volgt uit:

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Chevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Het is een typische door tabs gescheiden TSV-bestand met een kolom met eigenschappen van die auto-eigenschappen zoals het merk en model bevat. Deze eigenschappen moeten worden geparseerd naar kolommen in de tabel. De applier die wordt geleverd kunt u ook voor het genereren van een dynamische aantal eigenschappen in de rijenset resultaat, op basis van de parameter die wordt doorgegeven. U kunt alle eigenschappen of een specifieke set alleen eigenschappen genereren.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

De gebruiker gedefinieerde applier kan worden aangeroepen als een nieuw exemplaar van applier object:

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

Of met het aanroepen van een wrapper factory methode:

```csharp
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a>Gebruik van de gebruiker gedefinieerde combiners
Gebruiker gedefinieerde combiner of UDC, kunt u rijen uit de linker- en rijensets, op basis van aangepaste logica combineren. Gebruiker gedefinieerde combiner wordt gebruikt met de expressie COMBINEREN.

Een combiner wordt aangeroepen met de expressie COMBINEREN, waarmee de nodige informatie over zowel de invoer rijensets, de groeperende kolommen, de verwachte resultaat schema en aanvullende informatie.

Om aan te roepen een combiner in een Standaardscript van U-SQL, we gebruiken de volgende syntaxis:

```
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Zie voor meer informatie, [expressie COMBINEREN (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).

Voor het definiëren van een gebruiker gedefinieerde combiner, moeten we maken de `ICombiner` interface met de [`SqlUserDefinedCombiner`] kenmerk, die optioneel voor de definitie van een gebruiker gedefinieerde Combiner is.

Base `ICombiner` klassedefinitie:

```
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

De aangepaste implementatie van een `ICombiner` interface mag de definitie voor een `IEnumerable<IRow>` onderdrukking combineren.

```
[SqlUserDefinedCombiner]
public class MyCombiner : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    …
}
}
```

De **SqlUserDefinedCombiner** kenmerk geeft aan dat het type moet worden geregistreerd als een gebruiker gedefinieerde combiner. Deze klasse kan niet worden overgenomen.

**SqlUserDefinedCombiner** wordt gebruikt voor het definiëren van de eigenschap Combiner mode. Het is een optioneel kenmerk voor de definitie van een gebruiker gedefinieerde combiner.

CombinerMode modus

CombinerMode enum kan duren voordat de volgende waarden:

* Volledig (0) elke rij van de uitvoer is mogelijk afhankelijk van alle invoer rijen van links en rechts met dezelfde sleutelwaarde is.

* Left (1) is die elke rij van de uitvoer is afhankelijk van de invoer één rij uit de links (en mogelijk alle rijen vanaf de rechterkant met dezelfde sleutelwaarde).

* Recht (2) voor die elke rij van de uitvoer is afhankelijk van één invoer rij van het recht (en mogelijk alle rijen vanaf de linkerkant met dezelfde sleutelwaarde).

* Inner (3) elke rij van de uitvoer is afhankelijk van één invoer rij van links en rechts met dezelfde waarde.

Voorbeeld: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


De belangrijkste programmeerbaarheid objecten zijn:

```csharp
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Invoer rijensets worden doorgegeven als **links** en **rechts** `IRowset` type interface. Beide rijensets moet worden geïnventariseerd voor verwerking. U kunt elke interface slechts één keer opsommen, zodat we hoeven te inventariseren en opslaan in cache deze indien nodig.

Voor cachedoeleinden, maken we een lijst met\<T\> soort geheugenstructuur als gevolg hiervan van een LINQ uitvoering van de query, specifiek lijst <`IRow`>. Het type anonieme gegevens kan worden gebruikt tijdens de opsomming ook.

Zie [Inleiding tot LINQ-query's (C#)](https://msdn.microsoft.com/library/bb397906.aspx) voor meer informatie over het LINQ-query's, en [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) voor meer informatie over IEnumerable\<T\> interface.

De werkelijke waarden ophalen uit de binnenkomende `IRowset`, gebruiken we de methode voor Get() `IRow` interface.

```
mycolumn = row.Get<int>("mycolumn")
```

Afzonderlijke kolomnamen kunnen worden bepaald door het aanroepen van de `IRow` Schema-methode.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Of met behulp van de naam van de schema-kolom:

```
c# row.Get<int>(row.Schema[0].Name)
```

De algemene opsomming met LINQ ziet er als volgt uit:

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Nadat de beide rijensets inventariseren, gaan we alle rijen doorlopen. Voor elke rij in de linker rijenset gaan we alle rijen die voldoen aan de voorwaarde van onze combiner zoeken.

De uitvoerwaarden moeten worden ingesteld met `IUpdatableRow` uitvoer.

```
output.Set<int>("mycolumn", mycolumn)
```

De werkelijke uitvoer wordt geactiveerd door aanroepen aan `yield return output.AsReadOnly();`.

Hieronder volgt een voorbeeld van een combiner:

```
[SqlUserDefinedCombiner]
public class CombineSales : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    var internetSales =
    (from row in left.Rows
          select new
          {
              ProductKey = row.Get<int>("ProductKey"),
              OrderDateKey = row.Get<int>("OrderDateKey"),
              SalesAmount = row.Get<decimal>("SalesAmount"),
              TaxAmt = row.Get<decimal>("TaxAmt")
          }).ToList();

    var resellerSales =
    (from row in right.Rows
     select new
     {
     ProductKey = row.Get<int>("ProductKey"),
     OrderDateKey = row.Get<int>("OrderDateKey"),
     SalesAmount = row.Get<decimal>("SalesAmount"),
     TaxAmt = row.Get<decimal>("TaxAmt")
     }).ToList();

    foreach (var row_i in internetSales)
    {
    foreach (var row_r in resellerSales)
    {

        if (
        row_i.OrderDateKey > 0
        && row_i.OrderDateKey < row_r.OrderDateKey
        && row_i.OrderDateKey == 20010701
        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
        {
        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
        output.Set<int>("ProductKey", row_i.ProductKey);
        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
        }

    }
    }
    yield return output.AsReadOnly();
}
}
```

In dit scenario use-casescenario bouwen we een analytics-rapport voor de leverancier. Het doel is om te zoeken naar alle producten die meer dan $20.000 kosten en die verkopen via de website sneller dan via de gewone detailhandelaar binnen een bepaald tijdsbestek.

Dit is de basis U-SQL-script. U kunt de logica tussen een OUTER-JOIN en een combiner vergelijken:

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 TO @output_file1 USING Outputters.Tsv();
OUTPUT @rs2 TO @output_file2 USING Outputters.Tsv();
```

Een door de gebruiker gedefinieerde combiner kan worden aangeroepen als een nieuw exemplaar van het applier object:

```
USING new MyNameSpace.MyCombiner();
```


Of met het aanroepen van een wrapper factory methode:

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a>Gebruik van de gebruiker gedefinieerde reducers tegelijkertijd

U-SQL kunt u aangepaste rijenset reducers tegelijkertijd schrijven in C# met behulp van het uitbreidingsframework van de gebruiker gedefinieerde operator en een IReducer-interface implementeert.

Gebruiker gedefinieerde reducer of UDR, kan worden gebruikt om te verwijderen van onnodige rijen tijdens het ophalen van gegevens (importeren). Deze kan ook worden gebruikt om te bewerken en evalueren van rijen en kolommen. Op basis van programmeerbaarheid logica, kan het ook definiëren welke rijen moeten worden geëxtraheerd.

Voor het definiëren van een klasse UDR, moeten we maken een `IReducer` interface met een optionele `SqlUserDefinedReducer` kenmerk.

Deze klasseninterface mogen geen definitie voor de `IEnumerable` interface rijenset overschrijven.

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

De **SqlUserDefinedReducer** kenmerk geeft aan dat het type moet worden geregistreerd als een gebruiker gedefinieerde reducer. Deze klasse kan niet worden overgenomen.
**SqlUserDefinedReducer** is een optioneel kenmerk voor de definitie van een gebruiker gedefinieerde reducer. Wordt gebruikt om de eigenschap IsRecursive te definiëren.

* BOOL IsRecursive    
* **de waarde True** = geeft aan of deze Reducer associatieve en commutatieve

De belangrijkste programmeerbaarheid objecten zijn **invoer** en **uitvoer**. Het invoerobject wordt gebruikt om inventariseren invoer rijen. Uitvoer wordt gebruikt om in te stellen uitvoer rijen als gevolg van activiteit beperken.

Voor invoerrijen opsomming, gebruiken we de `Row.Get` methode.

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

De parameter voor de `Row.Get` methode is een kolom die wordt doorgegeven als onderdeel van de `PRODUCE` klasse van de `REDUCE` -instructie van de U-SQL-Standaardscript. We moeten het juiste gegevenstype hier ook gebruiken.

Gebruik voor de uitvoer van de `output.Set` methode.

Het is belangrijk om te begrijpen die aangepaste reducer voert alleen de waarden uit die zijn gedefinieerd met de `output.Set` aanroep van methode.

```
output.Set<string>("mycolumn", guid);
```

De uitvoer van de werkelijke reducer wordt geactiveerd door het aanroepen van `yield return output.AsReadOnly();`.

Hieronder volgt een voorbeeld van een reducer:

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
    string guid;
    DateTime dt;
    string user;
    string des;

    foreach (IRow row in input.Rows)
    {
        guid = row.Get<string>("guid");
        dt = row.Get<DateTime>("dt");
        user = row.Get<string>("user");
        des = row.Get<string>("des");

        if (user.Length > 0)
        {
        output.Set<string>("guid", guid);
        output.Set<DateTime>("dt", dt);
        output.Set<string>("user", user);
        output.Set<string>("des", des);

        yield return output.AsReadOnly();
        }
    }
    }

}
```

In dit scenario use case is de reducer wordt overgeslagen rijen met een lege gebruikersnaam. Voor elke rij in de rijenset elke vereiste kolom leest, vervolgens wordt geëvalueerd als de lengte van de naam van de gebruiker. Het voert de daadwerkelijke rij alleen als de lengte van de gebruiker de naam van waarde is groter dan 0.

Hieronder vindt u base U-SQL-script dat gebruikmaakt van een aangepaste reducer:

```
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```
