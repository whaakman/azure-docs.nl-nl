---
title: Azure Functions gebruiken voor het uitvoeren van een database opschoontaak | Microsoft Docs
description: Azure Functions gebruiken om een taak die is verbonden met Azure SQL Database periodiek opschonen van rijen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: 9d8261a22f5ea9ce61bcdc79d24a6c054597039b
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Azure Functions gebruiken voor verbinding met een Azure SQL Database
Dit onderwerp leest u het gebruik van Azure Functions voor het maken van een geplande taak opruimen van rijen in een tabel in een Azure SQL Database. De nieuwe C#-functie is gemaakt op basis van een vooraf gedefinieerde timer trigger-sjabloon in de Azure-portal. Ter ondersteuning van dit scenario, moet u ook een databaseverbindingsreeks instellen als een app-instelling in de functie-app. Dit scenario maakt gebruik van een bulksgewijze bewerking op de database. 

Functie proces afzonderlijke hebben maken, lezen, bijwerken en delete (CRUD)-bewerkingen in een tabel met Mobile Apps, moet u in plaats daarvan gebruiken [Mobile Apps-bindingen](functions-bindings-mobile-apps.md).

## <a name="prerequisites"></a>Vereisten

+ In dit onderwerp wordt de timerfunctie van een geactiveerd. Voer de stappen in het onderwerp [maken van een functie in Azure die wordt geactiveerd door een timer](functions-create-scheduled-function.md) om een C#-versie van deze functie te maken.   

+ Dit onderwerp wordt beschreven voor een Transact-SQL-opdracht die wordt uitgevoerd een bulkbewerking opschoning in de **SalesOrderHeader** tabel in de voorbeelddatabase van AdventureWorksLT. Voltooi de stappen in het onderwerp voor het maken van de voorbeelddatabase van AdventureWorksLT [maken van een Azure SQL database in de Azure portal](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

U moet de verbindingsreeks ophalen voor de database die u hebt gemaakt toen u voltooid [maken van een Azure SQL database in de Azure portal](../sql-database/sql-database-get-started-portal.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
 
3. Selecteer **SQL-Databases** uit in het menu links en selecteert u de database op de **SQL-databases** pagina.

4. Selecteer **databaseverbindingsreeksen tonen** en kopieer de volledige **ADO.NET** verbindingsreeks.

    ![Kopieer de ADO.NET-verbindingsreeks.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>De verbindingsreeks instellen 

Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Het is een best practice verbindingsreeksen en andere geheime informatie opgeslagen in de functie app-instellingen. Met de toepassingsinstellingen wordt voorkomen dat onbedoeld vrijgeven van de verbindingsreeks met uw code. 

1. Navigeer naar de functie-app die u hebt gemaakt [maken van een functie in Azure die wordt geactiveerd door een timer](functions-create-scheduled-function.md).

2. Selecteer **platformfuncties** > **toepassingsinstellingen**.
   
    ![Toepassingsinstellingen voor de functie-app.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Schuif omlaag naar **verbindingsreeksen** en een verbindingsreeks met de instellingen die zijn opgegeven in de tabel toevoegen.
   
    ![Een verbindingsreeks toevoegen aan de functie app-instellingen.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Instelling       | Voorgestelde waarde | Beschrijving             | 
    | ------------ | ------------------ | --------------------- | 
    | **Naam**  |  sqldb_connection  | Gebruikt voor toegang tot de opgeslagen verbindingsreeks in uw functiecode.    |
    | **Waarde** | Gekopieerde tekenreeks  | Plak de verbindingsreeks die u in de vorige sectie hebt gekopieerd en vervang `{your_username}` en `{your_password}` tijdelijke aanduidingen door echte waarden. |
    | **Type** | SQL Database | De standaard SQL Database-verbinding gebruiken. |   

3. Klik op **Opslaan**.

U kunt nu de C# functiecode die verbinding met uw SQL-Database maakt toevoegen.

## <a name="update-your-function-code"></a>Werk uw functiecode

1. In de functie-app, selecteert u de functie timer geactiveerd.
 
3. De volgende assemblyverwijzingen aan de bovenkant van de bestaande functiecode toevoegen:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. Voeg de volgende `using` instructies voor de functie:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Vervang de bestaande `Run` functie met de volgende code:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    Bij deze voorbeeldopdracht updates de `Status` kolom op basis van de verzending. Het moet 32 gegevensrijen bijwerken.

5. Klik op **opslaan**, bekijk de **logboeken** windows voor de volgende functie uitvoering en noteer het aantal rijen bijgewerkt in de **SalesOrderHeader** tabel.

    ![Bekijk de logboeken van de functie.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Volgende stappen

Vervolgens informatie over het gebruik van functies met Logic Apps om te integreren met andere services.

> [!div class="nextstepaction"] 
> [Maken van een functie die kan worden geïntegreerd met Logic Apps](functions-twitter-email.md)

Zie de volgende onderwerpen voor meer informatie over functies:

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Naslaginformatie voor programmeurs over het coderen van functies en het definiëren van triggers en bindingen.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.  
