---
title: Verbinding maken met Oracle Database - Azure Logic Apps | Microsoft Docs
description: Plaats en -records met Oracle Database REST-API's en Azure Logic Apps beheren
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/29/2017
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 9d606dd4faf324d68e4365eae0802de2384471ab
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295354"
---
# <a name="get-started-with-the-oracle-database-connector"></a>Aan de slag met de Oracle-Database-connector

Met de Oracle-Database-connector maken u organisatie werkstromen die gebruikmaken van gegevens in uw bestaande database. Deze connector kunt verbinding maken met een on-premises Oracle-Database of een Azure-machines met Oracle-Database is geïnstalleerd. Met deze connector kunt u het volgende doen:

* Bouw uw werkstroom door het toevoegen van een nieuwe klant met een database klanten of bijwerken van een order in een orderdatabase.
* Acties gebruiken om te ontvangen van een rij met gegevens, een nieuwe rij invoegen en zelfs verwijderen. Bijvoorbeeld, wanneer een record wordt gemaakt in Dynamics CRM Online (een trigger), klikt u vervolgens een rij invoegen in een Oracle-Database (een actie). 

Dit artikel leest u hoe het gebruik van de Oracle-Database-connector in een logische app.

## <a name="prerequisites"></a>Vereisten

* Ondersteunde Oracle-versies: 
    * Oracle 9 en hoger
    * Oracle-clientsoftware 8.1.7 en hoger

* De on-premises gegevensgateway installeren. [Verbinding maken met on-premises gegevens vanuit logische apps](../logic-apps/logic-apps-gateway-connection.md) vermeldt de stappen. De gateway is vereist voor het verbinding maken met een on-premises Oracle-Database of een Azure-VM met Oracle-database is geïnstalleerd. 

    > [!NOTE]
    > De on-premises gegevensgateway fungeert als een brug en biedt een veilige gegevensoverdracht tussen on-premises gegevens (gegevens die zich niet in de cloud) en uw logische apps. Dezelfde gateway kan worden gebruikt met meerdere services en meerdere gegevensbronnen. Dus, wellicht u alleen voor het installeren van de gateway eenmaal.

* Het Oracle-Client installeren op de computer waarop u de on-premises gegevensgateway hebt geïnstalleerd. Zorg ervoor dat de 64-bits Oracle-gegevensprovider voor .NET van Oracle te installeren:  

  [64-bits ODAC 12c Release 4 (12.1.0.2.4) voor Windows x64](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Als de Oracle-client niet is geïnstalleerd, wordt er een fout optreedt wanneer u probeert te maken of de verbinding gebruiken. Zie de veelvoorkomende fouten in dit artikel.


## <a name="add-the-connector"></a>De connector toevoegen

> [!IMPORTANT]
> Deze connector beschikt niet over geen triggers. Er worden alleen acties. Dus wanneer u uw logische app maakt, een andere trigger voor het starten van uw logische app, zoals toevoegen **planning - terugkeerpatroon**, of **aanvraag / antwoord - antwoord**. 

1. In de [Azure-portal](https://portal.azure.com), een lege logische app maken.

2. Aan het begin van de logische app, selecteer de **aanvraag / antwoord - aanvraag** trigger: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. Selecteer **Opslaan**. Wanneer u opslaat, wordt een aanvraag-URL wordt automatisch gegenereerd. 

4. Selecteer **nieuwe stap**, en selecteer **een actie toevoegen**. Typ in `oracle` om te zien van de beschikbare acties: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Dit is ook de snelste manier om te zien van de triggers en acties die beschikbaar zijn voor elke connector. Typ in het gedeelte van de connectornaam van de, zoals `oracle`. De ontwerpfunctie voor een lijst met alle triggers en acties. 

5. Selecteer een van de acties, zoals **Oracle Database - Get-rij**. Selecteer **verbinding maken via een on-premises gegevensgateway**. Voer de naam van de Oracle-server, verificatiemethode, gebruikersnaam, wachtwoord en selecteer de gateway:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. Eenmaal verbinding hebben, een tabel in de lijst selecteren en de rij-ID invoeren op de tabel. U moet weten de id voor de tabel. Als u niet weet, contact op met uw beheerder Oracle DB, en de uitvoer van `select * from yourTableName`. Hierdoor kunt u persoonlijke informatie die u nodig hebt om door te gaan.

    In het volgende voorbeeld taakgegevens uit een Human Resources-database geretourneerd: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. In deze stap kunt u een van de andere connectors gebruiken om u te maken van uw werkstroom. Als u ophalen van gegevens uit Oracle testen wilt, klikt u vervolgens Stuur uzelf een e-mailbericht met de Oracle-gegevens met behulp van een van de verzenden e-connectors, zoals Office 365- of Gmail. De dynamische tokens van de Oracle-tabel gebruiken om te bouwen de `Subject` en `Body` van uw e-mailadres:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. **Sla** uw logische app en selecteer vervolgens **uitvoeren**. Sluit de ontwerpfunctie en kijken naar de geschiedenis van uitvoeringen van de status. Als dit mislukt, selecteert u de mislukte bericht rij. De ontwerpfunctie wordt geopend en toont u die stap is mislukt en toont ook de foutgegevens. Als dat lukt, ontvangt u een e-mailbericht met de informatie die u hebt toegevoegd.


### <a name="workflow-ideas"></a>Werkstroom ideeën

* U wilt controleren van de hashtag #oracle en plaats u de tweets in een database, zodat ze kunnen worden opgevraagd en in andere toepassingen gebruikt. Een logische app, voeg de `Twitter - When a new tweet is posted` activeren en voer de **#oracle** hashtag. Voeg vervolgens de `Oracle Database - Insert row` actie, en selecteer uw tabel:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* Berichten worden verzonden naar een Service Bus-wachtrij. U wilt ophalen, deze berichten en plaats u ze in een database. Een logische app, voeg de `Service Bus - when a message is received in a queue` activeren en selecteert u de wachtrij. Voeg vervolgens de `Oracle Database - Insert row` actie, en selecteer uw tabel:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Algemene fouten

#### <a name="error-cannot-reach-the-gateway"></a>**Fout**: Kan de Gateway niet bereiken

**Oorzaak**: De on-premises gegevensgateway is niet alleen verbinding maken met de cloud. 

**Risicobeperking**: Zorg ervoor dat uw gateway wordt uitgevoerd op de on-premises computer waar u deze hebt geïnstalleerd, en dat deze verbinding kan maken met internet.  Het is raadzaam om de gateway niet installeren op een computer die kan worden uitgeschakeld of de slaapstand. U kunt ook de on-premises gegevensgatewayservice (PBIEgwService) opnieuw.

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-see-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a>**Fout**: De gebruikte provider is verouderd: ' System.Data.OracleClient is Oracle-clientsoftwareversie 8.1.7 of hoger.'. Zie [ https://go.microsoft.com/fwlink/p/?LinkID=272376 ](https://go.microsoft.com/fwlink/p/?LinkID=272376) om de officiële provider te installeren.

**Oorzaak**: De SDK van de Oracle-client is niet geïnstalleerd op de computer waarop de on-premises gegevensgateway is uitgevoerd.  

**Oplossing**: Download en installeer de Oracle-client-SDK op dezelfde computer als de on-premises gegevensgateway.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Fout**: Voor tabel [tabelnaam] geen sleutelkolommen gedefinieerd

**Oorzaak**: De tabel heeft geen primaire sleutel.  

**Oplossing**: Het Oracle Database-connector vereist dat een tabel met een primaire-sleutelkolom worden gebruikt.

#### <a name="currently-not-supported"></a>Momenteel ondersteund niet

* Weergaven 
* Een tabel met samengestelde sleutels
* Geneste objecttypen in tabellen
 
## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/oracle/). 

## <a name="get-some-help"></a>Hulp krijgen

De [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is een fantastische plek om te vragen stellen, vragen te beantwoorden en zien wat andere gebruikers van Logic Apps het doen zijn. 

U kunt helpen Logic Apps en connectors verbeteren door te stemmen en het verzenden van uw ideeën op [ https://aka.ms/logicapps-wish ](https://aka.ms/logicapps-wish). 


## <a name="next-steps"></a>Volgende stappen
[Maak een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md), en bekijk de beschikbare connectors in Logic Apps op [lijst van API's](apis-list.md).
