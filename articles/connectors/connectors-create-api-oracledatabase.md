---
title: De connector Oracle-Database in Azure Logic Apps toevoegen | Microsoft Docs
description: De connector Oracle-Database in een logische app gebruiken
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: mandia; ladocs
ms.openlocfilehash: 4119e72abd568c9b53245a872691f06329b040de
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-oracle-database-connector"></a>Aan de slag met de Oracle-Database-connector

De connector Oracle-Database maakt, u organisatie werkstromen die gebruikmaken van gegevens in uw bestaande database. Deze connector kunt verbinden met een lokale Oracle-Database of Azure een virtuele machine met Oracle-Database is geïnstalleerd. Met deze connector kunt u het volgende doen:

* Uw werkstroom door een nieuwe klant toe te voegen aan een database klanten of bijwerken van een order in een orderdatabase is opgebouwd.
* Acties voor een rij met gegevens ophalen, een nieuwe rij invoegen en verwijderen van zelfs gebruiken. Bijvoorbeeld, wanneer een record in Dynamics CRM Online (een trigger) wordt gemaakt, klikt u vervolgens een rij invoegen in een Oracle-Database (een actie). 

Dit onderwerp leest u hoe u de connector Oracle-Database in een logische app.

## <a name="prerequisites"></a>Vereisten

* Ondersteunde versies van Oracle: 
    * Oracle 9 en hoger
    * Oracle-clientsoftware 8.1.7 of hoger

* Installeer de lokale data gateway. [Verbinding maken met on-premises gegevens vanuit logic apps](../logic-apps/logic-apps-gateway-connection.md) vermeldt de stappen. De gateway is vereist voor het verbinding maken met een lokale Oracle-Database of een Azure-virtuele machine met Oracle DB geïnstalleerd. 

    > [!NOTE]
    > De lokale data gateway fungeert als een brug en biedt een veilige gegevensoverdracht tussen on-premises gegevens (gegevens die zich niet in de cloud) en uw logische apps. Dezelfde gateway kan worden gebruikt met meerdere services en meerdere gegevensbronnen. U wellicht dus slechts één keer de gateway te installeren.

* De Oracle-Client installeren op de computer waarop u de lokale data gateway hebt geïnstalleerd. Zorg ervoor dat de 64-bits Oracle-gegevensprovider voor .NET van Oracle installeren:  

  [64-bits ODAC 12c versie 4 (12.1.0.2.4) voor Windows x64](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Als de Oracle-client niet is geïnstalleerd, wordt er een fout optreedt wanneer u probeert te maken of de verbinding wilt gebruiken. Zie de veelvoorkomende fouten in dit onderwerp.


## <a name="add-the-connector"></a>De connector toevoegen

> [!IMPORTANT]
> Deze connector heeft geen triggers bestaan niet. Alleen acties heeft. Dus als u uw logische app maakt, een andere trigger voor het starten van uw logische app, zoals toevoegen **schema - terugkeerpatroon**, of **vraag / antwoord - antwoord**. 

1. In de [Azure-portal](https://portal.azure.com), een lege logische app maken.

2. Aan het begin van uw logische app, selecteer de **vraag / antwoord - aanvraag** trigger: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. Selecteer **Opslaan**. Wanneer u opslaat, wordt een aanvraag-URL wordt automatisch gegenereerd. 

4. Selecteer **nieuwe stap**, en selecteer **een actie toevoegen**. Typ in `oracle` om te zien van de beschikbare acties: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Dit is ook de snelste manier om te zien van de triggers en acties die beschikbaar zijn voor elke connector. Typ in het gedeelte van de connectornaam van de, zoals `oracle`. De ontwerpfunctie voor een lijst met alle triggers en acties. 

5. Selecteer een van de acties zoals **Oracle-Database - Get-rij**. Selecteer **verbinden via lokale gegevensgateway**. Voer de naam van de Oracle-server, de verificatiemethode, de gebruikersnaam, wachtwoord en selecteer de gateway:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. Eenmaal zijn verbonden, selecteer een tabel in de lijst en voer de rij-ID aan de tabel. U moet weten de in de tabel-id. Als u niet weet, contact op met uw beheerder Oracle DB, en de uitvoer van `select * from yourTableName`. Hiermee krijgt u identificeerbare informatie die u nodig hebt om door te gaan.

    In het volgende voorbeeld wordt de taakgegevens wordt geretourneerd uit een database Human Resources: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. In deze stap, kunt u een van de andere connectors gebruiken voor het bouwen van uw werkstroom. Als u ophalen van gegevens uit Oracle testen wilt, klikt u vervolgens verzend een e-mailbericht met de Oracle-gegevens met behulp van een van de verzonden e-connectors, zoals Office 365 of Gmail. De dynamische tokens van de Oracle-tabel gebruiken om de `Subject` en `Body` van uw e-mailadres:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. **Sla** uw logische app en selecteer vervolgens **uitvoeren**. Sluit de ontwerpfunctie en bekijk de geschiedenis wordt uitgevoerd voor de status. Als dit mislukt, selecteer de rij mislukt bericht. De ontwerpfunctie wordt geopend en ziet u dat de stap is mislukt en ziet ook de foutgegevens. Als dat lukt, ontvangt u een e-mailbericht met de informatie die u hebt toegevoegd.


### <a name="workflow-ideas"></a>Werkstroom ideeën

* U wilt bewaken van de hashtag #oracle en de tweets in een database te plaatsen, zodat ze kunnen worden opgevraagd, en in andere toepassingen gebruikt. In een logische app, voegt u de `Twitter - When a new tweet is posted` activeren en voer de **#oracle** hashtag. Voeg vervolgens de `Oracle Database - Insert row` actie, en selecteer de tabel:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* Berichten worden verzonden naar een Service Bus-wachtrij. U wilt deze berichten ophalen en plaatsen in een database. In een logische app, voegt u de `Service Bus - when a message is received in a queue` activeren en selecteert u de wachtrij. Voeg vervolgens de `Oracle Database - Insert row` actie, en selecteer de tabel:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Veelvoorkomende fouten

#### <a name="error-cannot-reach-the-gateway"></a>**Fout**: de Gateway kan niet worden bereikt.

**Oorzaak**: de lokale data gateway kan geen verbinding maken met de cloud. 

**Risicobeperking**: Zorg ervoor dat uw gateway wordt uitgevoerd op de on-premises machine waar u het hebt geïnstalleerd en dat deze verbinding kan maken met internet.  Het is raadzaam om de gateway niet is geïnstalleerd op een computer die kan worden uitgeschakeld of de slaapstand. U kunt ook de lokale data gateway-service (PBIEgwService) opnieuw.

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a>**Fout**: de gebruikte provider is verouderd: ' voor System.Data.OracleClient is vereist voor Oracle-clientsoftware version 8.1.7 of hoger.'. Ga naar [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) om de officiële provider te installeren.

**Oorzaak**: de Oracle-client SDK niet is geïnstalleerd op de computer waarop de lokale data gateway wordt uitgevoerd.  

**Resolutie**: downloaden en installeren van de Oracle-client-SDK op dezelfde computer als de lokale data gateway.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Fout**: tabel [tabelnaam] geen alle sleutelkolommen gedefinieerd

**Oorzaak**: de tabel heeft geen primaire sleutel.  

**Resolutie**: de Oracle-Database connector vereist dat een tabel met een primaire sleutelkolom worden gebruikt.

#### <a name="currently-not-supported"></a>Momenteel ondersteund niet

* Weergaven en opgeslagen procedures 
* Een tabel met samengestelde sleutels
* Geneste objecttypen in tabellen
 
## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/oracle/). 

## <a name="get-some-help"></a>Hulp krijgen

De [Azure Logic Apps-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is een goede plaats om te vragen, beantwoorden van vragen en zien wat anderen Logic Apps doen. 

U kunt logische Apps en connectors verbeteren door uw stem en verzenden van uw ideeën op [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish). 


## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md), en bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).
