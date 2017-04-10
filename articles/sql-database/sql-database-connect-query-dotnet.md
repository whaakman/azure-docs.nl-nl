---
title: Verbinding maken met Azure SQL Database via .NET (C#) | Microsoft Docs
description: Gebruik de voorbeeldcode in deze Quick Start om met Azure SQL Database een moderne toepassing te bouwen met C# die wordt ondersteund door een krachtige relationele database in de cloud.
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 7faca033-24b4-4f64-9301-b4de41e73dfd
ms.service: sql-database
ms.custom: quick start
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/28/2017
ms.author: andrela;sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: c6c0c218b8d0456d37a4514238675fd8e75faf9d
ms.lasthandoff: 03/30/2017


---
# <a name="azure-sql-database-use-net-c-to-connect-and-query-data"></a>Azure SQL Database: .NET (C#) gebruiken om verbinding te maken en query's uit te voeren voor gegevens

Gebruik [C# en ADO.NET](https://msdn.microsoft.com/library/kb9s9ks0.aspx) om verbinding te maken met de Azure SQL-database en om er query's voor uit te voeren. In deze handleiding wordt informatie geboden over het gebruik van C# om verbinding te maken met een Azure SQL-database, over het uitvoeren van query's en over het invoegen, bijwerken en verwijderen van instructies.

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)

## <a name="configure-development-environment"></a>De ontwikkelomgeving configureren

In de volgende secties worden details gegeven over het configureren van uw bestaande ontwikkelomgevingen in Mac OS, Linux (Ubuntu) en Windows om te werken met Azure SQL Database.

### <a name="mac-os"></a>**Mac OS**
Open de terminal en navigeer naar een map waar u van plan bent een .NET Core-project te maken. Voer de volgende opdrachten in voor het installeren van **Brew**, **OpenSSL** en **.NET Core**. 

```C#
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

Installeer .NET Core op Mac OS. Download het [officiële installatieprogramma](https://go.microsoft.com/fwlink/?linkid=843444). Met dit installatieprogramma installeert u de hulpprogramma's en plaats u ze op het PAD, zodat u dotnet kunt uitvoeren vanuit de console

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Open de terminal en navigeer naar een map waar u van plan bent een .NET Core-project te maken. Voer de volgende opdrachten in om **.NET Core** te installeren.

```C#
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```

### <a name="windows"></a>**Windows**
Installeer de Visual Studio 2015 Community-editie en .NET Framework. Als Visual Studio al is geïnstalleerd op uw computer, kunt u de volgende stappen overslaan.

Visual Studio 2015 Community is een volledig functionele, uitbreidbare en gratis IDE voor het maken van moderne toepassingen voor Android, iOS en Windows, voor web- en databasetoepassingen, en voor cloudservices.

1. Download het [installatieprogramma](https://go.microsoft.com/fwlink/?LinkId=691978). 
2. Voer het installatieprogramma uit en volg de weergegeven opdrachten om de installatie te voltooien.

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

Haal de verbindingsreeks op in Azure Portal. U gebruikt de verbindingsreeks om verbinding te maken met de Azure SQL-database.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. Bekijk in het deelvenster **Essentials** voor uw database de volledig gekwalificeerde servernaam. 

    <img src="./media/sql-database-connect-query-dotnet/connection-strings.png" alt="connection strings" style="width: 780px;" />

4. Klik op **Databaseverbindingsreeksen tonen**.

5. Bekijk de volledige**ADO.NET**-verbindingsreeks.

    <img src="./media/sql-database-connect-query-dotnet/adonet-connection-string.png" alt="ADO.NET connection string" style="width: 780px;" />
    
## <a name="add-systemdatasqlclient"></a>System.Data.SqlClient toevoegen
Als u gebruikmaakt van .NET Core, voegt u System.Data.SqlClient als een afhankelijkheid toe aan het ***.csproj***-bestand van het project.

```xml
<ItemGroup>
    <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
</ItemGroup>
```

## <a name="select-data"></a>Gegevens selecteren

1. Open een leeg codebestand in uw ontwikkelingsomgeving.
2. Voeg ```using System.Data.SqlClient``` toe aan uw codebestand ([naamruimte System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)). 

3. Gebruik [SqlCommand.ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx) met de Transact-SQL-instructie [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) om in uw Azure SQL-database query's uit te voeren voor gegevens. De juiste waarden voor uw server toevoegen

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="insert-data"></a>Gegevens invoegen

Gebruik [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) met de Transact-SQL-instructie [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) om in uw Azure SQL-database gegevens in te voegen.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nInsert data example:");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("INSERT INTO [SalesLT].[Product] ([Name], [ProductNumber], [Color], [StandardCost], [ListPrice], [SellStartDate]) ");
                    sb.Append("VALUES (@Name, @ProductNumber, @Color, @StandardCost, @ListPrice, @SellStartDate);");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ProductNumber", "200989");
                        command.Parameters.AddWithValue("@Color", "Blue");
                        command.Parameters.AddWithValue("@StandardCost", 75);
                        command.Parameters.AddWithValue("@ListPrice", 80);
                        command.Parameters.AddWithValue("@SellStartDate", "7/1/2016");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) inserted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="update-data"></a>Gegevens bijwerken

Gebruik [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) met de Transact-SQL-instructie [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) om in uw Azure SQL-database gegevens bij te werken.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nUpdate data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("UPDATE [SalesLT].[Product] SET ListPrice = @ListPrice WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ListPrice", 500);
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) updated");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="delete-data"></a>Gegevens verwijderen

Gebruik [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) met de Transact-SQL-instructie [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) om in uw Azure SQL-database gegevens te verwijderen.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nDelete data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("DELETE FROM SalesLT.Product WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) deleted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

- zie [.NET-documentatie](https://docs.microsoft.com/dotnet/) voor .NET-documentatie.
- Zie [Visual Studio Code](https://code.visualstudio.com/docs) voor meer informatie over het uitvoeren van query's voor en het bewerken van gegevens met Visual Studio Code.

