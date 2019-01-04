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
ms.date: 10/28/2018
ms.author: glenga
ms.openlocfilehash: 4ec2e9b931e6405aca5b4237bc044647af3b8bb3
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608576"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Azure Functions gebruiken voor het verbinding maken met een Azure SQL Database

In dit artikel wordt beschreven hoe u Azure Functions gebruikt om een geplande taak die verbinding met een Azure SQL Database-exemplaar maakt te maken. De functiecode opschonen van rijen in een tabel in de database. De nieuwe C# functie is gemaakt op basis van een vooraf gedefinieerde timer trigger-sjabloon in Visual Studio 2017. Ter ondersteuning van dit scenario, moet u ook een databaseverbindingsreeks instellen als een app-instelling in de functie-app. In dit scenario maakt gebruik van een bulksgewijze bewerking op de database. 

Als dit de eerste ervaring met C# functies, lees de [Azure Functions C# referentie voor ontwikkelaars](functions-dotnet-class-library.md).

## <a name="prerequisites"></a>Vereisten

+ Voer de stappen in het artikel [uw eerste functie maken met Visual Studio](functions-create-your-first-function-visual-studio.md) te maken van een lokale functie-app die gericht is op de runtime versie 2.x. U moet ook zijn gepubliceerd uw project aan een functie-app in Azure.

+ In dit artikel ziet u een Transact-SQL-opdracht die wordt uitgevoerd een bulkbewerking voor opschoning in de **SalesOrderHeader** tabel in de AdventureWorksLT-voorbeelddatabase. Voor het maken van de AdventureWorksLT-voorbeelddatabase, voer de stappen in het artikel [maken van een Azure SQL database in Azure portal](../sql-database/sql-database-get-started-portal.md).

+ U moet toevoegen een [firewallregel op serverniveau](../sql-database/sql-database-get-started-portal-firewall.md) voor het openbare IP-adres van de computer die u voor deze Quick Start gebruikt. Deze regel is vereist voor toegang tot de SQL database-instantie van de lokale computer zijn.  

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

U moet de verbindingsreeks ophalen voor de database die u hebt gemaakt toen u voltooid [maken van een Azure SQL database in Azure portal](../sql-database/sql-database-get-started-portal.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer **SQL-Databases** in het menu links en selecteer uw database op de **SQL-databases** pagina.

1. Selecteer **verbindingsreeksen** onder **instellingen** en kopieer de volledige **ADO.NET** verbindingsreeks.

    ![Kopieer de ADO.NET-verbindingsreeks.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>De verbindingsreeks instellen

Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Opslaan als een best practice voor beveiliging, verbindingsreeksen en andere geheimen in de instellingen van uw functie-app. Met de toepassingsinstellingen wordt voorkomen dat onbedoeld vrijgeven van de verbindingsreeks met uw code. U kunt toegang krijgen tot de app-instellingen voor uw functie-app rechtstreeks vanuit Visual Studio.

U moet eerder de app hebt gepubliceerd naar Azure. Als u dit nog niet hebt gedaan, [uw functie-app publiceren naar Azure](functions-develop-vs.md#publish-to-azure).

1. Klik in Solution Explorer met de rechtermuisknop op de functie-app-project en kies **publiceren** > **Toepassingsinstellingen beheren...** . Selecteer **-instelling toevoegen**in **nieuwe naam van de app-instelling**, type `sqldb_connection`, en selecteer **OK**.

    ![Toepassingsinstellingen voor de functie-app.](./media/functions-scenario-database-table-cleanup/functions-app-service-add-setting.png)

1. In de nieuwe **sqldb_connection** instelt, plak de verbindingsreeks die u hebt gekopieerd in de vorige sectie in de **lokale** veld en vervang `{your_username}` en `{your_password}` tijdelijke aanduidingen door echte de waarden. Selecteer **waarde invoeren tussen lokale** om te kopiëren van de bijgewerkte waarde in de **externe** veld en selecteer vervolgens **OK**.

    ![Instelling voor de verbindingsreeks SQL toevoegen.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-string.png)

    De verbindingsreeksen worden versleuteld opgeslagen in Azure (**externe**). Om te voorkomen dat lekkende geheimen, het bestand local.settings.json project (**lokale**) moet worden uitgesloten van broncodebeheer, zoals met behulp van een bestand .gitignore.

## <a name="add-the-sqlclient-package-to-the-project"></a>De SqlClient-pakket toevoegen aan het project

U moet het NuGet-pakket met de SqlClient-bibliotheek toevoegen. Deze bibliotheek van de toegang tot gegevens nodig is om te verbinden met een SQL-database.

1. Open uw lokale functie-app-project in Visual Studio 2017.

1. Klik in Solution Explorer met de rechtermuisknop op de functie-app-project en kies **NuGet-pakketten beheren**.

1. Zoek op het tabblad **Bladeren** naar ```System.Data.SqlClient``` en selecteer deze.

1. In de **System.Data.SqlClient** pagina, selecteer versie `4.5.1` en klik vervolgens op **installeren**.

1. Wanneer de installatie is voltooid, controleert u de wijzigingen en klikt u vervolgens op **OK** om het venster **Preview** te sluiten.

1. Als een venster voor **akkoord gaan met de licentie** wordt weergegeven, klikt u op **Ik ga akkoord**.

U kunt nu de C#-functiecode die is verbonden met uw SQL-Database toevoegen.

## <a name="add-a-timer-triggered-function"></a>Een door een timer geactiveerde functie toevoegen

1. Klik in Solution Explorer met de rechtermuisknop op de functie-app-project en kies **toevoegen** > **nieuwe Azure-functie**.

1. Met de **Azure Functions** hebt geselecteerd, Noem het nieuwe item er ongeveer als `DatabaseCleanup.cs` en selecteer **toevoegen**.

1. In de **nieuwe Azure-functie** dialoogvenster vak, kiest u **timertrigger** en vervolgens **OK**. Dit dialoogvenster wordt een codebestand voor de timer geactiveerde functie gemaakt.

1. Open het nieuwe codebestand en voeg de volgende using-instructies toe aan de bovenkant van het bestand:

    ```cs
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

1. Vervang de bestaande `Run` functie met de volgende code:

    ```cs
    [FunctionName("DatabaseCleanup")]
    public static async Task Run([TimerTrigger("*/15 * * * * *")]TimerInfo myTimer, ILogger log)
    {
        // Get the connection string from app settings and use it to create a connection.
        var str = Environment.GetEnvironmentVariable("sqldb_connection");
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " +
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.LogInformation($"{rows} rows were updated");
            }
        }
    }
    ```

    Deze functie wordt uitgevoerd elke 15 seconden om bij te werken de `Status` kolom op basis van de verzenddatum. Zie voor meer informatie over de Timer-trigger, [Timer-trigger voor Azure Functions](functions-bindings-timer.md).

1. Druk op **F5** om de functie-app te starten. De [Azure Functions Core Tools](functions-develop-local.md) tot uitvoering van het venster wordt geopend achter Visual Studio.

1. Op 15 seconden na het opstarten, is de functie wordt uitgevoerd. Bekijk de uitvoer en noteer het aantal rijen bijgewerkt in de **SalesOrderHeader** tabel.

    ![De functielogboeken weergeven.](./media/functions-scenario-database-table-cleanup/function-execution-results-log.png)

    Op de eerste uitvoering, dient u 32 rijen met gegevens werken. Volgende uitvoeringen geen gegevensrijen bijwerken, tenzij u wijzigingen in de tabelgegevens SalesOrderHeader aanbrengen zodat meer rijen worden geselecteerd door de `UPDATE` instructie.

Als u van plan om te bent [publiceren van deze functie](functions-develop-vs.md#publish-to-azure), moet u de `TimerTrigger` kenmerk voor een meer redelijke [cron-planning](functions-bindings-timer.md#cron-expressions) dan elke 15 seconden.

## <a name="next-steps"></a>Volgende stappen

Hierna leert u hoe u kunt gebruiken. Functions met Logic Apps om te integreren met andere services.

> [!div class="nextstepaction"]
> [Een functie maken die kan worden geïntegreerd met Logic Apps](functions-twitter-email.md)

Zie de volgende artikelen voor meer informatie over functies:

+ [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Naslaginformatie voor programmeurs over het coderen van functies en het definiëren van triggers en bindingen.
+ [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.  
