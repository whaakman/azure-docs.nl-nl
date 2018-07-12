---
title: Verbinding maken met SQL Server of Azure SQL Database - Azure Logic Apps | Microsoft Docs
description: Hoe u toegang tot en beheer van SQL-databases on-premises of in de cloud door het automatiseren van werkstromen met Azure Logic Apps
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38544020"
---
# <a name="connect-to-sql-server-or-azure-sql-database-from-azure-logic-apps"></a>Verbinding maken met SQL Server of Azure SQL Database van Azure Logic Apps

Dit artikel wordt beschreven hoe u toegang tot gegevens in uw SQL-database in een logische app met de SQL Server-connector. Op die manier kunt u taken, processen en werkstromen die uw SQL-gegevens en resources beheren met het maken van logische apps automatiseren. De connector werkt voor beide [SQL Server on-premises](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) en voor [Azure SQL Database in de cloud](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview). 

Logische apps die worden uitgevoerd wanneer geactiveerd door gebeurtenissen in uw SQL-database of in andere systemen, zoals Dynamics CRM Online, kunt u maken. Uw logische apps kunnen ook ophalen, invoegen en verwijderen van gegevens, samen met de SQL-query's en opgeslagen procedures uitvoeren. U kunt bijvoorbeeld een logische app die automatisch wordt gecontroleerd op nieuwe records in Dynamics CRM Online, voegt items toe aan uw SQL-database voor alle nieuwe records en verzendt vervolgens e-mailwaarschuwingen bouwen.

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). Connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/sql/" target="blank">documentatie voor SQL Server-connector</a>.

## <a name="prerequisites"></a>Vereisten

* De logische app waar u toegang wilt tot uw SQL-database hebben. Als uw logische app met een SQL-trigger wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Een [Azure SQL-database](../sql-database/sql-database-get-started-portal.md) of een [SQL Server-database](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) 

  De tabellen moeten gegevens hebben, zodat uw logische app kunt resultaten geretourneerd bij het aanroepen van bewerkingen. Als u een Azure SQL Database maakt, kunt u voorbeelddatabases die opgenomen zijn. 

* De naam van uw SQL-server, databasenaam, uw gebruikersnaam en wachtwoord. U moet deze referenties, zodat u uw logica voor toegang tot uw SQL-server kunt autoriseren. 

  * Voor Azure SQL Database, kunt u deze gegevens vinden in de verbindingsreeks of in Azure portal onder de SQL Database-eigenschappen:

    "Server = tcp: <*servernaam*>. database.windows.net,1433;Initial catalogus = <*yourDatabaseName*>; Beveiliginsinfo = False; Gebruikers-ID = <*uwgebruikersnaam*>; Wachtwoord = <*yourPassword*>; Voor MultipleActiveResultSets = False; Versleutelen = True; TrustServerCertificate = False; Time-out voor verbindingen = 30; "

  * Voor SQL Server, kunt u deze informatie vinden in de verbindingsreeks: 

    "Server = <*yourServerAddress*>; Database = <*yourDatabaseName*>; Gebruikers-Id = <*uwgebruikersnaam*>; Wachtwoord = <*yourPassword*>; "

* Voordat u logische apps met on-premises systemen, zoals SQL Server verbinden kunt, moet u [instellen van een on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md). Op die manier kunt u de gateway wanneer u de SQL-verbinding voor uw logische app maakt.

<a name="add-sql-trigger"></a>

## <a name="add-sql-trigger"></a>SQL-trigger toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

1. In de Azure portal of Visual Studio, maak een lege logische app, die wordt geopend de ontwerper van logische Apps. Dit voorbeeld wordt de Azure-portal.

2. Typ 'sql server' als filter in het zoekvak. Selecteer de SQL-trigger die u wilt dat in de lijst met triggers. 

   Selecteer deze trigger voor dit voorbeeld: **SQL Server - wanneer een item wordt gemaakt**

   ![Selecteer 'SQL Server - wanneer een item wordt gemaakt' trigger](./media/connectors-create-api-sqlazure/sql-server-trigger.png)

3. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de SQL-verbinding nu](#create-connection). 
   Of, als de verbinding al bestaat, selecteert u de **tabelnaam** die u wilt dat in de lijst.

   ![Tabel selecteren](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

4. Stel de **Interval** en **frequentie** eigenschappen opgeven hoe vaak uw logische app controleert op de tabel.

   In dit voorbeeld controleert alleen de geselecteerde tabel niets anders. 
   Toevoegen als u wilt doen iets interessanter, acties die de taken die u wilt uitvoeren. 
   
   Bijvoorbeeld, om het nieuwe item weergeven in de tabel, mogelijk andere acties toevoegen, zoals het maken van een bestand met velden uit de tabel en verzend e-mailwaarschuwingen. 
   Zie voor meer informatie over andere acties voor deze connector of andere connectors [Logic Apps-connectors](../connectors/apis-list.md).

5. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**. 

   Deze stap automatisch kunt en uw logische app live in Azure publiceert. 

<a name="add-sql-action"></a>

## <a name="add-sql-action"></a>SQL-actie toevoegen

In Azure Logic Apps, een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in uw werkstroom die volgt op een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met de [terugkeertrigger](../connectors/connectors-native-recurrence.md), en een actie die een rij uit een SQL-database opgehaald.

1. Open uw logische app in de ontwerper van logische Apps in de Azure portal of Visual Studio. Dit voorbeeld wordt de Azure-portal.

2. Kies in de Logic App Designer onder de trigger of actie, **nieuwe stap** > **een actie toevoegen**.

   ![Kies de optie 'Nieuwe stap', 'Een actie toevoegen'](./media/connectors-create-api-sqlazure/add-action.png)
   
   Als u wilt toevoegen een actie tussen bestaande stappen, Beweeg de muis boven de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

2. Typ 'sql server' als filter in het zoekvak. Selecteer een SQL-actie op die u wilt dat in de lijst met acties. 

   Selecteer deze actie, Hiermee haalt u een enkele record in dit voorbeeld: **SQL Server - rij ophalen**

   ![Voer 'sql server', 'SQL Server - rij ophalen' selecteren](./media/connectors-create-api-sqlazure/select-sql-get-row.png) 

3. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de SQL-verbinding nu](#create-connection). 
   Of, als de verbinding bestaat, selecteert u een **tabelnaam**, en voer de **rij-ID** voor de record die u wilt.

   ![Voer de naam van de tabel en rij-ID](./media/connectors-create-api-sqlazure/table-row-id.png)
   
   In het volgende voorbeeld wordt slechts één rij uit de geselecteerde tabel niets anders. 
   Als u wilt weergeven van de gegevens in deze rij, kan u andere acties die een bestand te met velden uit de rij voor later kunnen worden gecontroleerd maken toevoegen en sla dat bestand in een cloudopslagaccount. Zie voor meer informatie over andere acties in deze connector of andere connectors [Logic Apps-connectors](../connectors/apis-list.md).

4. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**. 

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Verbinding maken met uw database

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="process-data-in-bulk"></a>Gegevens bulksgewijs verwerken

Wanneer u met resultatensets zo groot is werkt dat de connector niet alle resultaten op hetzelfde moment retourneren of u betere controle over de grootte en de structuur voor uw resultatensets wilt, kunt u *paginering*, waarmee u deze beheren resultaten als kleiner wordt. 

[!INCLUDE [Set up pagination for results exceeding default page size](../../includes/connectors-pagination-bulk-data-transfer.md)]

### <a name="create-a-stored-procedure"></a>Een opgeslagen procedure maken

Bij het ophalen of meerdere rijen invoegen, uw logische app deze items kunt doorlopen met behulp van een [ *totdat-lus* ](../logic-apps/logic-apps-control-flow-loops.md#until-loop) binnen deze [limieten](../logic-apps/logic-apps-limits-and-config.md). Maar soms de logische app is om te werken met recordsets zo groot is, zoals duizenden of miljoenen rijen, die u wilt minimaliseren, de kosten voor het aanroepen van de database. 

In plaats daarvan kunt u een <a href="https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine" target="blank"> *opgeslagen procedure* </a> die wordt uitgevoerd in uw SQL-exemplaar en maakt gebruik van de **selecteren - ORDER BY** instructie voor het organiseren van de resultaten zoals u dat wilt. Deze oplossing hebt u meer controle over de grootte en structuur van de resultaten. Uw logische app roept de opgeslagen procedure met behulp van de SQL Server-connector **opgeslagen procedure uitvoeren** actie. 

Zie de volgende artikelen voor informatie over de details van deze oplossing:

* <a href="https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx" target="_blank">SQL-paginering voor de overdracht van grote hoeveelheden met Logic Apps</a>

* <a href="https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql" target="_blank">Selecteer - ORDER BY-component</a>

## <a name="connector-specific-details"></a>Connector-specifieke details

Zie voor technische informatie over triggers, acties en limieten van de connector de [details van de verwijzing van de connector](/connectors/sql/). 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)

