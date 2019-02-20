---
title: Visual Studio met .NET en C# gebruiken om een query uit te voeren op een Azure SQL-database | Microsoft Docs
description: Gebruik Visual Studio om een C#-toepassing te maken die is verbonden met een Azure SQL-database en query's uitvoert op deze database met behulp van Transact-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/12/2019
ms.openlocfilehash: c6b23038ad68492e1965e1ebf7ce5e7cf1d788f7
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236631"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-an-azure-sql-database"></a>Snelstartgids: Gebruik .NET (C#) en C# in Visual Studio om verbinding te maken met, en query's uit te voeren op, een Azure SQL-database

In deze snelstartgids ziet u hoe u het [.NET Framework](https://www.microsoft.com/net/) en C#-code gebruikt in Visual Studio om query's uit te voeren op een Azure SQL-database met Transact-SQL-instructies.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

- Een Azure SQL-database. U kunt een van deze quickstarts gebruiken om een database te maken en vervolgens te configureren in Azure SQL Database:

  || Individuele database | Beheerd exemplaar |
  |:--- |:--- |:---|
  | Maken| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) |
  | Configureren | [IP-firewallregel op serverniveau](sql-database-server-level-firewall-rule.md)| [Connectiviteit vanaf een VM](sql-database-managed-instance-configure-vm.md)|
  |||[Connectiviteit vanaf een locatie](sql-database-managed-instance-configure-p2s.md)
  |Gegevens laden|Adventure Works geladen per quickstart|[Wide World Importers herstellen](sql-database-managed-instance-get-started-restore.md)
  |||Adventure Works herstellen of importeren vanuit een [BACPAC](sql-database-import.md)-bestand uit [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > De scripts in dit artikel worden geschreven voor gebruik met de Adventure Works-database. Met een beheerd exemplaar moet u de Adventure Works-database importeren in een exemplaardatabase of de scripts in dit artikel wijzigen voor gebruik van de Wide World Importers-database.

- [Visual Studio 2017](https://www.visualstudio.com/downloads/) Community-, Professional- of Enterprise-editie.

## <a name="get-sql-server-connection-information"></a>SQL Server-verbindingsgegevens ophalen

Haal de verbindingsgegevens op die u nodig hebt om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam of hostnaam, databasenaam en aanmeldingsgegevens in de volgende procedures nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Navigeer naar de pagina **SQL-database** of **Met SQL beheerde exemplaren**.

3. Bekijk op de pagina **Overzicht** de volledig gekwalificeerde servernaam naast **Servernaam** voor een individuele database, of de volledig gekwalificeerde servernaam naast **Host** voor een beheerd exemplaar. Als u de servernaam of hostnaam wilt kopiëren, plaatst u de muisaanwijzer erop en selecteert u het pictogram **Kopiëren**. 

## <a name="create-code-to-query-the-sql-database"></a>Code maken om query's uit te voeren op de SQL-database

1. Selecteer in Visual Studio **Bestand** > **Nieuw** > **Project**. 
   
1. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Visual C#** en **Consoletoepassing (.NET Framework)**.
   
1. Voer *sqltest* in voor de projectnaam en selecteer vervolgens **OK**. Het nieuwe project wordt gemaakt. 
   
1. Selecteer **Project** > **NuGet-pakketten beheren**. 
   
1. Selecteer in **NuGet Package Manager** het tabblad **Bladeren** en zoek en selecteer **System.Data.SqlClient**.
   
1. Selecteer op de pagina **System.Data.SqlClient** de optie **Installeren**. 
   - Selecteer **OK** om door te gaan met de installatie. 
   - Als een venster voor **akkoord gaan met de licentie** wordt weergegeven, selecteert u **Ik ga akkoord**.
   
1. Wanneer de installatie is voltooid, kunt u **NuGet Package Manager** sluiten. 
   
1. Vervang in de code-editor de inhoud **Program.cs** door de volgende code. Gebruik uw waarden voor `<server>`, `<username>`, `<password>` en `<database>`.
   
   >[!IMPORTANT]
   >Voor de code in dit voorbeeld worden de voorbeeldgegevens gebruikt van AdventureWorksLT, die u als bron kunt kiezen bij het maken van uw database. Als in uw database andere gegevens staan, kunt u tabellen uit uw eigen database gebruiken in de SELECT-query. 
   
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
                   builder.DataSource = "<server>.database.windows.net"; 
                   builder.UserID = "<username>";            
                   builder.Password = "<password>";     
                   builder.InitialCatalog = "<database>";
   
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

1. Om de app uit te voeren, selecteert u **Fouten opsporen** > **Foutopsporing starten** of selecteert u **Start** op de werkbalk of drukt u op **F5**.
1. Controleer of de bovenste twintig rijen Categorie/Product in uw database worden geretourneerd en sluit vervolgens het appvenster.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [verbinding maken met en een query uitvoeren op een Azure SQL-database met behulp van .NET Core](sql-database-connect-query-dotnet-core.md) in Windows/Linux/macOS.  
- Meer informatie over [Aan de slag met .NET Core in Windows/Linux/macOS met behulp van de opdrachtregel](/dotnet/core/tutorials/using-with-xplat-cli).
- Meer informatie over [Uw eerste Azure SQL-database ontwerpen met behulp van SSMS](sql-database-design-first-database.md) of [Uw eerste Azure SQL-database ontwerpen met behulp van .NET](sql-database-design-first-database-csharp.md).
- Raadpleeg de [.NET-documentatie](https://docs.microsoft.com/dotnet/) voor meer informatie over .NET.
- Voorbeeld van pogingslogica: [Flexibel verbinding maken met SQL via ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n].


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

