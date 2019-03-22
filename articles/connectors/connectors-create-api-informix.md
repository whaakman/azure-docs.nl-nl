---
title: Verbinding maken met IBM Informix-database - Azure Logic Apps | Microsoft Docs
description: Resources beheren met IBM Informix REST-API's en Azure Logic Apps
author: gplarsen
manager: jeconnoc
ms.author: plarsen
ms.date: 09/26/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 6004c02f190bbfcf374b3b5d2a5c478f0e52c961
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58165897"
---
# <a name="get-started-with-the-informix-connector"></a>Aan de slag met de Informix-connector
Logic Apps verbindt connector van Microsoft voor Informix met bronnen die zijn opgeslagen in een IBM Informix-database. De Informix-connector bevat een client voor Microsoft om te communiceren met externe computers voor Informix-server via een TCP/IP-netwerk. Dit omvat clouddatabases zoals IBM Informix voor Windows die wordt uitgevoerd in Azure-virtualisatie, en het on-premises databases met behulp van de on-premises gegevensgateway. Zie de [lijst met ondersteunde](connectors-create-api-informix.md#supported-informix-platforms-and-versions) van IBM Informix-platforms en versies (in dit onderwerp).

De connector ondersteunt de volgende databasebewerkingen uit:

* Lijst met database-tabellen
* Lezen van één rij met behulp van selecteren
* Lezen van alle rijen met behulp van selecteren
* Een rij met behulp van INSERT toevoegen
* Wijzigen van één rij met UPDATE
* Verwijderen van één rij verwijderen gebruiken

Dit onderwerp ziet u hoe u de connector in een logische app databasebewerkingen proces gebruiken.

Zie voor meer informatie over Logic Apps, [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="available-actions"></a>Beschikbare acties
Deze connector ondersteunt de volgende acties van logische Apps:

* Getables
* GetRow
* GetRows
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Lijst met tabellen weergeven
Het maken van een logische app voor elke bewerking die bestaat uit veel stappen die worden uitgevoerd via de Microsoft Azure-portal.

In de logische app, kunt u een actie toevoegen aan lijst met tabellen in een Informix-database. Hiermee geeft u de connector voor het verwerken van een Informix-schema-instructie, zoals `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer **+** (plusteken), **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixgetTables`, **abonnement**, **resourcegroep**, **locatie**, en **App Service Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1. In de **ontwerper van logische Apps**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** in de lijst met **terugkeerpatroon**. 
3. In de **terugkeerpatroon** trigger, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst te selecteren **dag**, en selecteer vervolgens  **Interval** naar het type **7**.  
4. Selecteer de **+ nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** weergeven, typt u **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - tabellen ophalen (Preview)**.
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. In de **Informix - Get-tabellen** configuratie venster **selectievakje** om in te schakelen **verbinding maken via een on-premises gegevensgateway**. U ziet dat de instellingen voor het wijzigen van cloud naar on-premises.
   
   * Typ de waarde voor **Server**, in de vorm van-adres of alias poortnummer voor dubbele punt. Typ bijvoorbeeld `ibmserver01:9089`.
   * Typ de waarde voor **Database**. Typ bijvoorbeeld `nwind`.
   * Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **Basic**.
   * Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
   * Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
   * Selecteer de waarde voor **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
7. Selecteer **maken**, en selecteer vervolgens **opslaan**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. In de **InformixgetTables** blade in de **All runs** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoering).
9. In de **logische app** Selecteer **Details uitvoering van**. Binnen de **actie** in de lijst met **Get_tables**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoerkoppeling** om weer te geven van de invoer. Selecteer de **uitvoerkoppeling**, en de uitvoer bekijken, waaronder is een lijst met tabellen.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>De verbindingen maken
Deze connector biedt ondersteuning voor verbindingen met database on-premises en in de cloud met behulp van de volgende verbindingseigenschappen. 

| Eigenschap | Description |
| --- | --- |
| server |Vereist. Accepteert een string-waarde vertegenwoordigt een TCP/IP-adres of de alias in IPv4 of IPv6-indeling, gevolgd (puntkomma's gescheiden) door een TCP/IP-poortnummer. |
| database |Vereist. Accepteert een string-waarde vertegenwoordigt een DRDA relationele Database de naam (RDBNAM). Informix accepteert een 128-byte-tekenreeks (de database staat bekend als de naam van een IBM Informix-database (dbname)). |
| verificatie |Optioneel. Accepteert een item lijstwaarde, Basic of Windows (kerberos). |
| gebruikersnaam |Vereist. Een string-waarde accepteert. |
| wachtwoord |Vereist. Een string-waarde accepteert. |
| gateway |Vereist. Accepteert een lijst met item-waarde voor de on-premises gegevensgateway naar Logic Apps is gedefinieerd in de opslaggroep. |

## <a name="create-the-on-premises-gateway-connection"></a>De on-premises gatewayverbinding maken
Deze connector kan toegang tot een on-premises Informix-database met behulp van de on-premises gegevensgateway. Zie gateway-onderwerpen voor meer informatie. 

1. In de **Gateways** configuratie venster **selectievakje** om in te schakelen **verbinding maken via een gateway**. Zie de instellingen wijzigen van cloud naar on-premises.
2. Typ de waarde voor **Server**, in de vorm van-adres of alias poortnummer voor dubbele punt. Typ bijvoorbeeld `ibmserver01:9089`.
3. Typ de waarde voor **Database**. Typ bijvoorbeeld `nwind`.
4. Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **Basic**.
5. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
6. Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer de waarde voor **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
8. Selecteer **maken** om door te gaan. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>De cloudverbinding maken
Deze connector hebben toegang tot een cloud Informix-database. 

1. In de **Gateways** deelvenster configuratie, laat de **selectievakje** uitgeschakeld (gekleurd) **verbinding maken via een gateway**. 
2. Typ de waarde voor **verbindingsnaam**. Typ bijvoorbeeld `hisdemo2`.
3. Typ de waarde voor **Informix-servernaam**, in de vorm van-adres of alias poortnummer voor dubbele punt. Typ bijvoorbeeld `hisdemo2.cloudapp.net:9089`.
4. Typ de waarde voor **Informix-databasenaam**. Typ bijvoorbeeld `nwind`.
5. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
6. Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer **maken** om door te gaan. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Ophalen van alle rijen met behulp van selecteren
U kunt een actie voor logische app om op te halen van alle rijen in de Informix-tabel maken. Hiermee geeft u de connector voor het verwerken van een Informix SELECT-instructie, zoals `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer **+** (plusteken), **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam** (bijvoorbeeld) "**InformixgetRows**"), **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1. In de **ontwerper van logische Apps**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** in de lijst met **terugkeerpatroon**. 
3. In de **terugkeerpatroon** trigger, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst te selecteren **dag**, en selecteer vervolgens  **Interval** naar het type **7**. 
4. Selecteer de **+ nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** weergeven, typt u **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - rijen ophalen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**.
7. In de **verbindingen** configuratie venster **nieuw**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. In de **Gateways** deelvenster configuratie, laat de **selectievakje** uitgeschakeld (gekleurd) **verbinding maken via een gateway**.
   
   * Typ de waarde voor **verbindingsnaam**. Typ bijvoorbeeld `HISDEMO2`.
   * Typ de waarde voor **Informix-servernaam**, in de vorm van-adres of alias poortnummer voor dubbele punt. Typ bijvoorbeeld `HISDEMO2.cloudapp.net:9089`.
   * Typ de waarde voor **Informix-databasenaam**. Typ bijvoorbeeld `NWIND`.
   * Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
   * Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
9. Selecteer **maken** om door te gaan.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. In de **tabelnaam** in de lijst met de **pijl-omlaag**, en selecteer vervolgens **gebied**.
11. Selecteer desgewenst **geavanceerde opties weergeven** om op te geven van de opties voor query's.
12. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. In de **InformixgetRows** blade in de **All runs** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoering).
14. In de **logische app** Selecteer **Details uitvoering van**. Binnen de **actie** in de lijst met **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoerkoppeling** om weer te geven van de invoer. Selecteer de **uitvoerkoppeling**, en de uitvoer bekijken, waaronder is een lijst met rijen.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Een rij met behulp van INSERT toevoegen
U kunt een actie voor logische app als u wilt toevoegen van een rij in een Informix-tabel maken. Deze actie geeft de connector voor het verwerken van een Informix INSERT-instructie zoals `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer **+** (plusteken), **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixinsertRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1. In de **ontwerper van logische Apps**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** in de lijst met **terugkeerpatroon**. 
3. In de **terugkeerpatroon** trigger, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst te selecteren **dag**, en selecteer vervolgens  **Interval** naar het type **7**. 
4. Selecteer de **+ nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** weergeven, typt u **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - rij invoegen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuratie in het deelvenster selecteren om een verbinding te kiezen. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** in de lijst met de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode asterisk). Typ bijvoorbeeld `99999` voor **gebieds-id**, type `Area 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. In de **InformixinsertRow** blade in de **All runs** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoering).
12. In de **logische app** Selecteer **Details uitvoering van**. Binnen de **actie** in de lijst met **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoerkoppeling** om weer te geven van de invoer. Selecteer de **uitvoerkoppeling**, en de uitvoer bekijken, waaronder is de nieuwe rij.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Ophalen van één rij met behulp van selecteren
U kunt een actie voor logische app om op te halen van een rij in een Informix-tabel maken. Deze actie geeft de connector voor het verwerken van een instructie Informix selecteren waar zoals `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer **+** (plusteken), **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixgetRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1. In de **ontwerper van logische Apps**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** in de lijst met **terugkeerpatroon**. 
3. In de **terugkeerpatroon** trigger, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst te selecteren **dag**, en selecteer vervolgens  **Interval** naar het type **7**. 
4. Selecteer de **+ nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** weergeven, typt u **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - rijen ophalen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** deelvenster configuraties, selecteer een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** in de lijst met de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode asterisk). Typ bijvoorbeeld `99999` voor **gebieds-id**. 
10. Selecteer desgewenst **geavanceerde opties weergeven** om op te geven van de opties voor query's.
11. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. In de **InformixgetRow** blade in de **All runs** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoering).
13. In de **logische app** Selecteer **Details uitvoering van**. Binnen de **actie** in de lijst met **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoerkoppeling** om weer te geven van de invoer. Selecteer de **uitvoerkoppeling**, en de uitvoer weergeven; dat moet rij bevatten.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Wijzigen van één rij met UPDATE
U kunt een actie voor logische app als u wilt wijzigen van een rij in een Informix-tabel maken. Hiermee geeft u de connector voor het verwerken van een Informix-UPDATE-instructie, zoals `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer **+** (plusteken), **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixupdateRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1. In de **ontwerper van logische Apps**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** in de lijst met **terugkeerpatroon**. 
3. In de **terugkeerpatroon** trigger, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst te selecteren **dag**, en selecteer vervolgens  **Interval** naar het type **7**. 
4. Selecteer de **+ nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** weergeven, typt u **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - rij bijwerken (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** deelvenster configuraties, selecteer een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** in de lijst met de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode asterisk). Typ bijvoorbeeld `99999` voor **gebieds-id**, type `Updated 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. In de **InformixupdateRow** blade in de **All runs** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoering).
12. In de **logische app** Selecteer **Details uitvoering van**. Binnen de **actie** in de lijst met **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoerkoppeling** om weer te geven van de invoer. Selecteer de **uitvoerkoppeling**, en de uitvoer bekijken, waaronder is de nieuwe rij.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Verwijderen van één rij verwijderen gebruiken
U kunt een actie voor logische app als u wilt verwijderen van een rij in een Informix-tabel maken. Hiermee geeft u de connector voor het verwerken van een instructie Informix verwijderen, zoals `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer **+** (plusteken), **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixdeleteRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1. In de **ontwerper van logische Apps**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** in de lijst met **terugkeerpatroon**. 
3. In de **terugkeerpatroon** trigger, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst te selecteren **dag**, en selecteer vervolgens  **Interval** naar het type **7**. 
4. Selecteer de **+ nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** weergeven, typt u **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - rij verwijderen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuraties in het deelvenster een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** in de lijst met de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode asterisk). Typ bijvoorbeeld `99999` voor **gebieds-id**. 
10. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. In de **InformixdeleteRow** blade in de **All runs** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoering).
12. In de **logische app** Selecteer **Details uitvoering van**. Binnen de **actie** in de lijst met **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoerkoppeling** om weer te geven van de invoer. Selecteer de **uitvoerkoppeling**, en de uitvoer bekijken, die de verwijderde rij moet bevatten.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Ondersteunde Informix-platforms en versies
Deze connector ondersteunt de volgende versies van de IBM Informix, wanneer geconfigureerd ter ondersteuning van clientverbindingen gedistribueerd relationele Database architectuur (DRDA).

* IBM Informix 12.1
* IBM Informix korting van 11,7

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/informix/). 

## <a name="next-steps"></a>Volgende stappen
[Maak een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Verken de andere beschikbare connectors in Logic Apps op onze [lijst van API's](apis-list.md).

