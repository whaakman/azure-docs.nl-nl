---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 11/09/2018
ms.author: genemi
ms.openlocfilehash: c4329b9efef3cdb2911466e64ac6c9f07a1e9b31
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269689"
---
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a>Voorbeeld van C#-programma

In de volgende secties van dit artikel wordt een C#-programma gepresenteerd dat gebruikmaakt van ADO.NET voor het verzenden van Transact-SQL-instructies naar de SQL-database. Met het C#-programma worden de volgende acties uitgevoerd:

1. [Maakt verbinding met de SQL-database met behulp van ADO.NET](#cs_1_connect).
2. [Maakt tabellen](#cs_2_createtables).
3. [Vult de tabellen met gegevens, door T-SQL INSERT-instructies uit te geven](#cs_3_insert).
4. [Werkt gegevens bij met behulp van een join](#cs_4_updatejoin).
5. [Verwijdert gegevens met behulp van een join](#cs_5_deletejoin).
6. [Selecteert gegevensrijen met behulp van een join](#cs_6_selectrows).
7. Sluit de verbinding (waardoor alle tijdelijke tabellen worden gestopt vanuit tempdb).

Het C#-programma bevat:

- C#-code om verbinding te maken met de database.
- Methoden waarmee de T-SQL-broncode wordt geretourneerd.
- Twee methoden waarmee de T-SQL wordt verzonden naar de database.

#### <a name="to-compile-and-run"></a>Compileren en uitvoeren

Dit C#-programma is logischerwijs één .cs-bestand. Maar hier is het programma fysiek onderverdeeld in verschillende codeblokken, waardoor elk blok gemakkelijker te zien en te begrijpen is. Ga als volgt te werk om dit programma te compileren en uit te voeren:

1. Maak een C#-project in Visual Studio.
    - Het projecttype moet een *consoletoepassing* zijn met een hiërarchie die lijkt op de volgende: **Sjablonen** > **Visual C#** > **klassieke Windows-bureaublad** > **Consoletoepassing (.NET Framework)**.
3. Wis in het bestand **Program.cs** de kleine coderegels aan het begin.
3. Kopieer en plak elk van de volgende blokken in Program.cs, in dezelfde volgorde die hier wordt weergegeven.
4. Werk in Program.cs de volgende waarden bij in de methode **Main**:

   - **cb.DataSource**
   - **cd.UserID**
   - **cb.Password**
   - **InitialCatalog**

5. Controleer of wordt verwezen naar de assembly **System.Data.dll**. Vouw in het deelvenster **Solution Explorer** het knooppunt **Verwijzingen** uit om dit te controleren.
6. Klik op het menu **Bouwen** om het programma te bouwen in Visual Studio.
7. Klik op de knop **Start** om het programma uit te voeren in Visual Studio. De rapportuitvoer wordt weergegeven in een cmd.exe-venster.

> [!NOTE]
> U kunt de T-SQL bewerken om een **#** toe te voegen vóór de tabelnamen. Hierdoor worden ze tijdelijke tabellen in **tempdb**. Dit kan nuttig zijn voor demonstratiedoeleinden, wanneer er geen testdatabase beschikbaar is. Tijdelijke tabellen worden automatisch verwijderd als de verbinding wordt verbroken. Alle VERWIJZINGEN naar refererende sleutels worden niet afgedwongen voor tijdelijke tabellen.
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a>C#-blok 1: Verbinding maken met behulp van ADO.NET

- [Volgende](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View the report output here, then press any key to end the program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-to-create-tables"></a>C#-blok 2: T-SQL voor het maken van tabellen

- [Vorige](#cs_1_connect) &nbsp; / &nbsp; [Volgende](#cs_3_insert)

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

#### <a name="entity-relationship-diagram-erd"></a>ERD (diagram Relatie tussen eenheden)

Voor de voorgaande CREATE TABLE-instructies is het trefwoord **VERWIJZINGEN** nodig voor het maken van een *FK*-relatie (refererende sleutel) relatie tussen twee tabellen.  Als u tempdb gebruikt, geeft u commentaar bij het trefwoord `--REFERENCES` met behulp van twee streepjes aan het begin.

Hierna volgt een ERD die de relatie tussen de twee tabellen weergeeft. De waarden in de #tabEmployee.DepartmentCode *onderliggende* kolom zijn beperkt tot de waarden in de #tabDepartment.Department *bovenliggende* kolom.

![ERD met refererende sleutel](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-to-insert-data"></a>C#-blok 3: T-SQL voor het invoegen van tabellen

- [Vorige](#cs_2_createtables) &nbsp; / &nbsp; [Volgende](#cs_4_updatejoin)


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- The company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- The company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
### <a name="c-block-4-t-sql-to-update-join"></a>C#-blok 4: T-SQL voor update-join

- [Vorige](#cs_3_insert) &nbsp; / &nbsp; [Volgende](#cs_5_deletejoin)


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
### <a name="c-block-5-t-sql-to-delete-join"></a>C#-blok 5: T-SQL voor delete-join

- [Vorige](#cs_4_updatejoin) &nbsp; / &nbsp; [Volgende](#cs_6_selectrows)


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size the Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband the Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-to-select-rows"></a>C#-blok 6: T-SQL voor het selecteren van rijen

- [Vorige](#cs_5_deletejoin) &nbsp; / &nbsp; [Volgende](#cs_6b_datareader)


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all the final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
### <a name="c-block-6b-executereader"></a>C#-blok 6b: ExecuteReader

- [Vorige](#cs_6_selectrows) &nbsp; / &nbsp; [Volgende](#cs_7_executenonquery)

Deze methode is ontworpen voor het uitvoeren van de T-SQL SELECT-instructie die wordt gebouwd met de methode **Build_6_Tsql_SelectEmployees**.


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
### <a name="c-block-7-executenonquery"></a>C#-blok 7: ExecuteNonQuery

- [Vorige](#cs_6b_datareader) &nbsp; / &nbsp; [Volgende](#cs_8_output)

Deze methode wordt aangeroepen voor bewerkingen die de gegevensinhoud van tabellen wijzigen zonder gegevensrijen te retourneren.


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
### <a name="c-block-8-actual-test-output-to-the-console"></a>C#-blok 8: Werkelijke testuitvoer naar de console

- [Vorige](#cs_7_executenonquery)

In deze sectie wordt de uitvoer vastgelegd die via het programma is verzonden naar de console. Alleen de GUID-waarden variëren tussen testuitvoeringen.


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
