---
title: R gebruiken op Azure SQL Database-query
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In dit artikel wordt beschreven hoe u een R-script gebruiken om te verbinden met een Azure SQL database en query's uitvoeren met behulp van Transact-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 05/29/2019
ms.openlocfilehash: 796e9c17a457bfb1a79b600b178d2c733e10f91e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66382414"
---
# <a name="quickstart-use-r-to-query-an-azure-sql-database-preview"></a>Quickstart: R gebruiken om te vragen van een Azure SQL-database (preview)

 In deze Quick Start ziet u hoe u [R](https://www.r-project.org/) met Machine Learning Services verbinding maken met een Azure SQL database en Transact-SQL-instructies gebruikt om gegevens te doorzoeken. Machine Learning-Services is een functie van Azure SQL Database, die wordt gebruikt voor het uitvoeren van R-scripts in de database. Zie voor meer informatie, [Azure SQL Database Machine Learning-Services met R (preview)](sql-database-machine-learning-services-overview.md).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Een Azure SQL-database. U kunt een van deze quickstarts gebruiken om een database te maken en vervolgens te configureren in Azure SQL Database:

<!-- Managed instance is not supported during the preview
  || Single database | Managed instance |
  |:--- |:--- |:---|
  | Create| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Configure | [Server-level IP firewall rule](sql-database-server-level-firewall-rule.md) | [Connectivity from a VM](sql-database-managed-instance-configure-vm.md) |
  ||| [Connectivity from on-site](sql-database-managed-instance-configure-p2s.md) |
  | Load data | Adventure Works loaded per quickstart | [Restore Wide World Importers](sql-database-managed-instance-get-started-restore.md) |
  ||| Restore or import Adventure Works from [BACPAC](sql-database-import.md) file from [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) |
  |||
-->

  || Individuele database |
  |:--- |:--- |
  | Maken| [Portal](sql-database-single-database-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) |
  | Configureren | [IP-firewallregel op serverniveau](sql-database-server-level-firewall-rule.md) |
  | Gegevens laden | Adventure Works geladen volgens de quickstart |
  |||

  > [!NOTE]
  > Tijdens de Preview-versie van Azure SQL Database Machine Learning-Services met R, wordt de Implementatieoptie voor het beheerde exemplaar niet ondersteund.

<!-- Managed instance is not supported during the preview
  > [!IMPORTANT]
  > The scripts in this article are written to use the Adventure Works database. With a managed instance, you must either import the Adventure Works database into an instance database or modify the scripts in this article to use the Wide World Importers database.
-->

- Machine Learning-Services (met R) ingeschakeld. Na de onboarding voor de openbare preview wordt Machine Learning voor u ingeschakeld voor uw bestaande of nieuwe database. Volg de stappen in [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

- De meest recente [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). U kunt R-scripts met behulp van het beheer van andere databases of hulpmiddelen voor query's uitvoeren, maar in deze snelstartgids gebruikt u SSMS.

## <a name="get-sql-server-connection-information"></a>SQL Server-verbindingsgegevens ophalen

Haal de verbindingsgegevens op die u nodig hebt om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam of hostnaam, databasenaam en aanmeldingsgegevens nodig voor de volgende procedures.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Navigeer naar de pagina **SQL-database** of **Met SQL beheerde exemplaren**.

3. Bekijk op de pagina **Overzicht** de volledig gekwalificeerde servernaam naast **Servernaam** voor een individuele database, of de volledig gekwalificeerde servernaam naast **Host** voor een beheerd exemplaar. Als u de servernaam of hostnaam wilt kopiëren, plaatst u de muisaanwijzer erboven en selecteert u het pictogram **Kopiëren**.

## <a name="create-code-to-query-your-sql-database"></a>Code maken om query's uit te voeren op uw SQL-database

1. Open **SQL Server Management Studio** en maak verbinding met de SQL-database.

   Als u hulp bij het maken van een verbinding nodig hebt, raadpleegt u [Quick Start: SQL Server Management Studio gebruiken om verbinding te maken en query's uit te voeren op een Azure SQL database](sql-database-connect-query-ssms.md).

1. Het volledige R-script uit te geven de [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) opgeslagen procedure.

   Het script wordt doorgegeven via de `@script` argument. Alles in de `@script` argument moet een geldige R-code.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Als er fouten optreden, kan dit zijn omdat de openbare preview van Machine Learning Services (met R) niet is ingeschakeld voor uw SQL-database. Zie [vereisten](#prerequisites) hierboven.

## <a name="run-the-code"></a>De code uitvoeren

1. Voer de [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) opgeslagen procedure.

1. Controleer of dat de categorie/Product bovenste 20 rijen worden geretourneerd in de **berichten** venster.

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Azure SQL Database Machine Learning Services (met R)](sql-database-machine-learning-services-overview.md)
- [Maken en eenvoudige R-scripts uitvoeren in Azure SQL Database Machine Learning-Services (preview)](sql-database-quickstart-r-create-script.md)
- [Schrijven van geavanceerde R-functies in Azure SQL Database met behulp van Machine Learning-Services (preview)](sql-database-machine-learning-services-functions.md)
