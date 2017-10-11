---
title: De Azure SQL Database-connector in uw logische Apps toevoegen | Microsoft Docs
description: Overzicht van Azure SQL Database-connector met de parameters van de REST-API
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a3d5cb909dbfcb00f3fbfa0165bb6cd58eb18688
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-azure-sql-database-connector"></a>Aan de slag met de Azure SQL Database-connector
Met de Azure SQL Database-connector, werkstromen maken voor uw organisatie die gegevens in de tabellen te beheren. 

Met SQL Database u:

* Uw werkstroom door een nieuwe klant toe te voegen aan een database klanten of bijwerken van een order in een orderdatabase is opgebouwd.
* Acties voor een rij met gegevens ophalen, een nieuwe rij invoegen en verwijderen van zelfs gebruiken. Bijvoorbeeld, wanneer een record in Dynamics CRM Online (een trigger) wordt gemaakt, klikt u vervolgens een rij invoegen in een Azure SQL Database (een actie). 

In dit onderwerp leest u hoe voor het gebruik van de connector van de SQL-Database in een logische app, en vindt u ook de acties.

Zie voor meer informatie over Logic Apps, [wat zijn logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) en [een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-azure-sql-database"></a>Verbinding maken met Azure SQL Database
Om uw logische app toegang alle services tot, maakt u eerst een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld: voor verbinding met SQL-Database, u eerst een SQL-Database maken *verbinding*. Een verbinding wilt maken, moet u de referenties die u gebruikt om toegang tot de service die u verbinding met maakt invoeren. Dus in SQL-Database, Voer uw referenties voor SQL-Database om de verbinding te maken. 

#### <a name="create-the-connection"></a>De verbinding maken
> [!INCLUDE [Create the connection to SQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a>Een trigger te gebruiken
Deze connector heeft geen triggers bestaan niet. Met andere triggers kunt starten van de logische app, zoals een trigger terugkeerpatroon, een HTTP-Webhook-trigger, triggers beschikbaar met andere connectors en meer. [Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md) bevat een voorbeeld.

## <a name="use-an-action"></a>Gebruik een actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Selecteer het plusteken. Ziet u verschillende mogelijkheden: **een actie toevoegen**, **een voorwaarde toevoegen**, of een van de **meer** opties.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Kies **een actie toevoegen**.
3. Typ in het tekstvak 'sql' om een lijst met alle beschikbare acties.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. Kies in ons voorbeeld **SQL Server - Get-rij**. Als er al een verbinding bestaat, selecteert u de **tabelnaam** in de vervolgkeuzelijst, en voert u de **rij-ID** u wilt retourneren.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Als u wordt gevraagd om de verbindingsinformatie, voert u de details om de verbinding te maken. [De verbinding](connectors-create-api-sqlazure.md#create-the-connection) in dit onderwerp beschrijft deze eigenschappen. 
   
   > [!NOTE]
   > In dit voorbeeld wordt een rij uit een tabel retourneren. De gegevens in deze rij, toe te voegen nog een actie die u maakt een bestand met de velden uit de tabel. Bijvoorbeeld, een OneDrive-actie die de voornaam en achternaam velden gebruikt voor het maken van een nieuw bestand in de cloud storage-account toevoegen. 
   > 
   > 
5. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en automatisch kan worden ingeschakeld.

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/sql/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md). Bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).

