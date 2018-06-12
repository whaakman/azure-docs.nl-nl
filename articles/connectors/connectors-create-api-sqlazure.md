---
title: Verbinding maken met SQL Server of Azure SQL Database - Azure Logic Apps | Microsoft Docs
description: Het openen en SQL-databases on-premises of in de cloud beheren met het automatiseren van werkstromen met Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/15/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: dccb91c782408a5fed5c3ef1b68f9918823ce402
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296285"
---
# <a name="connect-to-sql-server-or-azure-sql-database-from-azure-logic-apps"></a>Verbinding maken met SQL Server of Azure SQL-Database vanuit Azure Logic Apps

Dit artikel laat zien hoe u toegang hebt tot gegevens in uw SQL-database in een logische app met de SQL Server-connector. Op die manier kunt u taken, processen en werkstromen die de SQL-gegevens en bronnen beheren door te maken van logische apps automatiseren. De connector werkt voor zowel [SQL Server on-premises](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) en voor [Azure SQL Database in de cloud](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview). 

U kunt logische apps die worden uitgevoerd wanneer deze worden geactiveerd door gebeurtenissen in de SQL-database of in andere systemen, zoals Dynamics CRM Online maken. Uw logische apps kunnen ook ophalen, invoegen en verwijderen van gegevens samen met het uitvoeren van SQL-query's en opgeslagen procedures. U kunt bijvoorbeeld een logische app, die automatisch wordt gecontroleerd op nieuwe records in Dynamics CRM Online en stuurt vervolgens e-mailwaarschuwingen items toegevoegd aan de SQL-database voor alle nieuwe records maken.

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/sql/" target="blank">SQL Server-connector verwijzing</a>.

## <a name="prerequisites"></a>Vereisten

* De logische app waar u toegang wilt tot de SQL-database hebben. U kunt uw logische app starten met een SQL-trigger, moet u een [lege logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Een [Azure SQL-database](../sql-database/sql-database-get-started-portal.md) of een [SQL Server-database](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) 

  Uw tabellen moeten gegevens hebben, zodat uw logische app resultaten retourneren kunt bij het aanroepen van bewerkingen. Als u een Azure SQL Database maakt, kunt u voorbeelddatabases die opgenomen zijn. 

* De naam van uw SQL-server, databasenaam, uw gebruikersnaam en het wachtwoord. U moet deze referenties zodat u kunt uw logica voor toegang tot uw SQL-server machtigen. 

  * Voor Azure SQL Database, kunt u deze informatie vinden in de verbindingsreeks of in de Azure-portal onder de SQL-Database-eigenschappen:

    ' Server = tcp: <*servernaam*>. database.windows.net,1433;Initial catalogus = <*yourDatabaseName*>; Beveiliginsinfo = False; Gebruikers-ID = <*uwgebruikersnaam*>; Wachtwoord = <*yourPassword*>; Voor MultipleActiveResultSets = False; Versleutelen = True; TrustServerCertificate = False; Verbindingstime-out = 30; "

  * Voor SQL Server, kunt u deze informatie vinden in de verbindingsreeks: 

    ' Server = <*yourServerAddress*>; Database = <*yourDatabaseName*>; Gebruikers-Id = <*uwgebruikersnaam*>; Wachtwoord = <*yourPassword*>; "

* Voordat u logische apps met on-premises systemen, zoals SQL Server verbinden kunt, moet u [Stel een gegevensgateway lokale](../logic-apps/logic-apps-gateway-install.md). Op die manier kunt u de gateway bij het maken van de SQL-verbinding voor uw logische app.

<a name="add-sql-trigger"></a>

## <a name="add-sql-trigger"></a>SQL-trigger toevoegen

In Azure Logic Apps elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde is voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine maakt een logische app-exemplaar en de werkstroom van uw app wordt gestart.

1. In de Azure-portal of Visual Studio maakt een lege logische app, Logic Apps Designer te openen. In dit voorbeeld wordt de Azure-portal.

2. Voer in het zoekvak 'sql server' als filter. Selecteer de SQL-trigger die u wilt dat in de lijst triggers. 

   Selecteer deze trigger voor dit voorbeeld: **SQL Server - wanneer een item wordt gemaakt**

   ![Selecteer 'SQL Server - wanneer een item wordt gemaakt' trigger](./media/connectors-create-api-sqlazure/sql-server-trigger.png)

3. Als u wordt gevraagd om de details voor verbinding [nu uw SQL-verbinding maken](#create-connection). 
   Of, als uw verbinding al aanwezig is, selecteert u de **tabelnaam** die u wilt dat in de lijst.

   ![Tabel selecteren](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

4. Stel de **Interval** en **frequentie** eigenschappen opgeven hoe vaak uw logische app in de tabel gecontroleerd.

   In dit voorbeeld controleert alleen de geselecteerde tabel niets anders. 
   Hiertoe iets interessanters toevoegen acties die de taken die u wilt uitvoeren. 
   
   Bijvoorbeeld om het nieuwe item weergeven in de tabel, mogelijk andere acties toevoegen, zoals een bestand met velden uit de tabel te maken en verzend een e-mailwaarschuwingen. 
   Zie voor meer informatie over andere acties voor deze connector of andere connectors [Logic Apps connectors](../connectors/apis-list.md).

5. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**. 

   Deze stap schakelt automatisch en publiceert uw logische app live in Azure. 

<a name="add-sql-action"></a>

## <a name="add-sql-action"></a>SQL-actie toevoegen

In Azure Logic Apps een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in de werkstroom die na een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met de [terugkeerpatroon trigger](../connectors/connectors-native-recurrence.md), en roept een actie die een rij opgehaald uit een SQL-database.

1. Open uw logische app in Logic Apps Designer in de Azure-portal of Visual Studio. In dit voorbeeld wordt de Azure-portal.

2. Kies in de ontwerpfunctie Logic App, onder de trigger of actie **nieuwe stap** > **een actie toevoegen**.

   ![Kies 'Nieuwe stap', 'Een actie toevoegen'](./media/connectors-create-api-sqlazure/add-action.png)
   
   Als u een actie tussen bestaande stappen toevoegen, wilt u de muisaanwijzer over de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

2. Voer in het zoekvak 'sql server' als filter. Selecteer een SQL-actie op die u wilt dat in de lijst met acties. 

   Bijvoorbeeld, selecteert u deze actie, die één record opgehaald: **SQL Server - rij ophalen**

   ![Voer 'sql server', 'SQL Server - Get-rij' selecteren](./media/connectors-create-api-sqlazure/select-sql-get-row.png) 

3. Als u wordt gevraagd om de details voor verbinding [nu uw SQL-verbinding maken](#create-connection). 
   Of, als de verbinding bestaat, selecteert u een **tabelnaam**, en voer de **rij-ID** voor de gewenste record.

   ![Voer de tabelnaam en rij-ID](./media/connectors-create-api-sqlazure/table-row-id.png)
   
   In het volgende voorbeeld wordt slechts één rij uit de geselecteerde tabel niets anders. 
   Bekijk de gegevens in deze rij, kan u andere acties die het maken van een bestand met velden uit de rij voor latere revisie toevoegen en dat bestand opslaan in een cloud-opslagaccount. Zie voor meer informatie over andere acties in deze connector of andere connectors [Logic Apps connectors](../connectors/apis-list.md).

4. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**. 

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Verbinding maken met uw database

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="process-data-in-bulk"></a>Verwerkte gegevens bulksgewijs

Wanneer u met resultatensets zo groot is werkt dat de connector niet alle resultaten op hetzelfde moment retourneert of u betere controle over de grootte en de structuur voor uw resultatensets wilt, kunt u *paginering*, waarmee u de beheren resultaten als kleinere informatiesets. 

[!INCLUDE [Set up pagination for results exceeding default page size](../../includes/connectors-pagination-bulk-data-transfer.md)]

### <a name="create-a-stored-procedure"></a>Een opgeslagen procedure maken

Ophaalt of meerdere rijen invoegen, uw logische app deze artikelen kunt doorlopen met behulp van een [ *tot lus* ](../logic-apps/logic-apps-control-flow-loops.md#until-loop) binnen deze [limieten](../logic-apps/logic-apps-limits-and-config.md). Maar soms uw logische app werkt met recordsets zo groot is, zoals duizenden of miljoenen rijen die u wilt minimaliseren, de kosten voor het aanroepen van de database heeft. 

In plaats daarvan kunt u een <a href="https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine" target="blank"> *opgeslagen procedure* </a> die wordt uitgevoerd in uw SQL-exemplaar en maakt gebruik van de **selecteren - ORDER BY** instructie te organiseren de resultaten van de gewenste manier. Deze oplossing biedt u meer controle over de grootte en de structuur van uw resultaten. Uw logische app de opgeslagen procedure aanroept met behulp van de SQL Server-connector **opgeslagen procedure uitvoeren** in te grijpen. 

Zie voor meer informatie oplossing in deze artikelen:

* <a href="https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx" target="_blank">SQL-paginering voor overdracht van grote hoeveelheden gegevens met Logic Apps</a>

* <a href="https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql" target="_blank">Selecteer - ORDER BY-component</a>

## <a name="connector-specific-details"></a>Connector-specifieke details

Zie voor technische informatie over deze connector triggers, acties en limieten voor de [van connector paginaverwijzing](/connectors/sql/). 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)

