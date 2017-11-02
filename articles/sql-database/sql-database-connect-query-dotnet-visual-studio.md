---
title: Visual Studio en .NET gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Visual Studio gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: devcenter
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 5fc50c78af7c88cc2d0c537456cb8323002f8696
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="use-net-c-with-visual-studio-to-connect-and-query-an-azure-sql-database"></a>.NET (C#) met Visual Studio gebruiken om verbinding te maken en query's uit te voeren voor een Azure SQL-database

In deze beknopte zelfstudie wordt gedemonstreerd hoe u het [.NET Framework](https://www.microsoft.com/net/) gebruikt om een C#-programma te maken met Visual Studio dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze beknopte zelfstudie te voltooien:

- Een Azure SQL-database. In deze zelfstudie worden de resources gebruikt die u hebt gemaakt in een van deze Quick Starts: 

   - [Database maken - Portal](sql-database-get-started-portal.md)
   - [Database maken - CLI](sql-database-get-started-cli.md)
   - [Database maken - PowerShell](sql-database-get-started-powershell.md)

- Een [firewallregel op serverniveau](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) voor het openbare IP-adres van de computer die u gebruikt voor deze beknopte zelfstudie.
- Een installatie van [Visual Studio Community 2017, Visual Studio Professional 2017 of Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam, databasenaam en aanmeldingsgegevens in de volgende procedures nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. Op de pagina **Overzicht** voor de database controleert u de volledig gekwalificeerde servernaam zoals in de volgende afbeelding wordt weergegeven. U kunt de cursor boven de servernaam houden om de optie **Klik om te kopiëren** naar boven te halen. 

   ![servernaam](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Als u de aanmeldingsgegevens voor uw Azure SQL Database-server bent vergeten, gaat u naar de SQL Database-serverpagina om de beheerdersnaam voor de server weer te geven. U kunt, indien nodig, het wachtwoord opnieuw instellen.

5. Klik op **Databaseverbindingsreeksen tonen**.

6. Bekijk de volledige **ADO.NET**-verbindingsreeks.

    ![ADO.NET-verbindingsreeks](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> U moet een firewallregel hebben ingesteld voor het openbare IP-adres van de computer waarop u deze zelfstudie uitvoert. Als u een andere computer gebruikt of een ander openbaar IP-adres hebt, maakt u een [firewallregel op serverniveau met behulp van Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-visual-studio-project"></a>Een nieuw Visual Studio-project maken

1. Kies in Visual Studio **File**, **New**, **Project**. 
2. Ga naar het dialoogvenster **New Project** en vouw **Visual C#** uit.
3. Selecteer **Console App** en voer *sqltest* voor de projectnaam in.
4. Klik op **OK** om het nieuwe project in Visual Studio te maken en te openen.
4. Klik in Solution Explorer met de rechtermuisknop op **sqltest** en klik vervolgens op **NuGet-pakketten beheren**. 
5. Zoek in **Bladeren** naar ```System.Data.SqlClient``` en selecteer deze.
6. Klik op de pagina **System.Data.SqlClient** op **Installeren**.
7. Wanneer de installatie is voltooid, controleert u de wijzigingen en klikt u vervolgens op **OK** om het venster **Preview** te sluiten. 
8. Als een venster voor **akkoord gaan met de licentie** wordt weergegeven, klikt u op **Ik ga akkoord**.

## <a name="insert-code-to-query-sql-database"></a>Code invoegen om een query uit te voeren voor een SQL-database
1. Schakel over naar (of open indien nodig) **Program.cs**.

2. Vervang de inhoud van **Program.cs** door de volgende code en voeg de juiste waarden toe voor de server, de database, de gebruiker en het wachtwoord.

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
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
            Console.ReadLine();
        }
    }
}
```

## <a name="run-the-code"></a>De code uitvoeren

1. Druk op **F5** om de toepassing uit te voeren.
2. Controleer of de bovenste 20 rijen worden geretourneerd, en sluit vervolgens het toepassingsvenster.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [verbinding maken met en een query uitvoeren voor een Azure SQL-database met behulp van .NET Core](sql-database-connect-query-dotnet-core.md) in Windows/Linux/macOS.  
- Meer informatie over [Aan de slag met .NET Core in Windows/Linux/macOS met behulp van de opdrachtregel](/dotnet/core/tutorials/using-with-xplat-cli).
- Meer informatie over [Uw eerste Azure SQL-database ontwerpen met behulp van SSMS](sql-database-design-first-database.md) of [Uw eerste Azure SQL-database ontwerpen met behulp van .NET](sql-database-design-first-database-csharp.md).
- Raadpleeg de [.NET-documentatie](https://docs.microsoft.com/dotnet/) voor meer informatie over .NET.
