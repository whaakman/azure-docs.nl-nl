---
title: Azure Functions gebruiken voor het uitvoeren van een database opschoontaak | Microsoft Docs
description: Azure Functions gebruiken om een taak die verbinding met Azure SQL Database maakt voor het periodiek opschonen van rijen.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: 024958d8a548313b53fc24ade5805de036a89afb
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49351912"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Azure Functions gebruiken voor het verbinding maken met een Azure SQL Database
Dit onderwerp ziet u hoe u Azure Functions gebruiken voor het maken van een geplande taak opschonen van rijen in een tabel in een Azure SQL Database. De nieuwe C#-scriptfunctie is gemaakt op basis van een vooraf gedefinieerde timer trigger-sjabloon in Azure portal. Ter ondersteuning van dit scenario, moet u ook een databaseverbindingsreeks instellen als een app-instelling in de functie-app. In dit scenario maakt gebruik van een bulksgewijze bewerking op de database. 

Om een functie proces individueel maken, lezen, bijwerken en (CRUD)-verwijderbewerkingen in een tabel met Mobile Apps, moet u in plaats daarvan gebruikt [Mobile Apps-bindingen](functions-bindings-mobile-apps.md).

> [!IMPORTANT]
> De voorbeelden in dit document zijn van toepassing op de runtime 1.x. Meer informatie over het maken van een 1.x-functie-app [vindt u hier](./functions-versions.md#creating-1x-apps).

## <a name="prerequisites"></a>Vereisten

+ In dit onderwerp wordt gebruikgemaakt van een timer geactiveerde functie. Voer de stappen in het onderwerp [een functie maken in Azure die wordt geactiveerd door een timer](functions-create-scheduled-function.md) om een C#-versie van deze functie te maken.   

+ In dit onderwerp ziet u een Transact-SQL-opdracht die wordt uitgevoerd een bulkbewerking voor opschoning in de **SalesOrderHeader** tabel in de AdventureWorksLT-voorbeelddatabase. Voor het maken van de AdventureWorksLT-voorbeelddatabase, voer de stappen in het onderwerp [maken van een Azure SQL database in Azure portal](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

U moet de verbindingsreeks ophalen voor de database die u hebt gemaakt toen u voltooid [maken van een Azure SQL database in Azure portal](../sql-database/sql-database-get-started-portal.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
 
3. Selecteer **SQL-Databases** in het menu links en selecteer uw database op de **SQL-databases** pagina.

4. Selecteer **databaseverbindingsreeksen tonen** en kopieer de volledige **ADO.NET** verbindingsreeks. 

    ![Kopieer de ADO.NET-verbindingsreeks.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>De verbindingsreeks instellen 

Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Het is een aanbevolen procedure voor het opslaan van verbindingsreeksen en andere geheimen in de instellingen van uw functie-app. Met de toepassingsinstellingen wordt voorkomen dat onbedoeld vrijgeven van de verbindingsreeks met uw code. 

1. Ga naar de functie-app die u hebt gemaakt [een functie maken in Azure die wordt geactiveerd door een timer](functions-create-scheduled-function.md).

2. Selecteer **platformfuncties** > **toepassingsinstellingen**.
   
    ![Toepassingsinstellingen voor de functie-app.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Schuif omlaag naar **verbindingsreeksen** en een verbindingsreeks met de instellingen zoals opgegeven in de tabel toevoegen.
   
    ![Een verbindingsreeks toevoegen aan de instellingen voor de functie-app.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Instelling       | Voorgestelde waarde | Beschrijving             | 
    | ------------ | ------------------ | --------------------- | 
    | **Naam**  |  sqldb_connection  | Gebruikt voor toegang tot de opgeslagen connection string in uw functiecode aan te geven.    |
    | **Waarde** | Gekopieerde tekenreeks  | Plak de verbindingsreeks die u in de vorige sectie hebt gekopieerd en vervang `{your_username}` en `{your_password}` tijdelijke aanduidingen door echte waarden. |
    | **Type** | SQL Database | De standaard SQL-Database-verbinding gebruiken. |   

3. Klik op **Opslaan**.

U kunt nu de C#-functiecode die is verbonden met uw SQL-Database toevoegen.

## <a name="update-your-function-code"></a>Uw functiecode bijwerken

1. Selecteer de timer geactiveerde functie in uw functie-app in de portal.
 
3. Voeg de volgende assemblyverwijzingen toe aan de bovenkant van de bestaande C#-script functiecode:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```
    >[!NOTE]
    >De code in deze voorbeelden zijn C#-script uit de portal. Wanneer u een vooraf gecompileerde C#-functie lokaal ontwikkelt, moet u in plaats daarvan verwijzingen naar deze ophaalprotocol in uw lokale project toevoegen.  

3. Voeg de volgende `using` instructies toe aan de functie:
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

    In dit voorbeeldopdracht werkt de `Status` kolom op basis van de verzenddatum. Deze moet 32 rijen met gegevens bijwerken.

5. Klik op **opslaan**, bekijk de **logboeken** windows voor de volgende functie worden uitgevoerd in en noteer het aantal rijen bijgewerkt in de **SalesOrderHeader** tabel.

    ![De functielogboeken weergeven.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Volgende stappen

Hierna leert u hoe u Functions met Logic Apps gebruikt om te integreren met andere services.

> [!div class="nextstepaction"] 
> [Een functie maken die kan worden geïntegreerd met Logic Apps](functions-twitter-email.md)

Zie de volgende onderwerpen voor meer informatie over functies:

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Naslaginformatie voor programmeurs over het coderen van functies en het definiëren van triggers en bindingen.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.  
