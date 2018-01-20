---
title: De DB2-connector in uw logische Apps toevoegen | Microsoft Docs
description: Overzicht van DB2-connector met de parameters van de REST-API
services: 
documentationcenter: 
author: gplarsen
manager: erikre
editor: 
tags: connectors
ms.assetid: 1c6b010c-beee-496d-943a-a99e168c99aa
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: 7ad246ede7e891de42235443c1bc6a90b88ad5ac
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-db2-connector"></a>Aan de slag met de DB2-connector
Microsoft-connector voor DB2 verbindt Logic Apps met resources die zijn opgeslagen in een IBM DB2-database. Deze connector omvat een Microsoft-client om te communiceren met externe computers voor DB2-server via een TCP/IP-netwerk. Dit omvat cloud databases, zoals IBM Bluemix dashDB of IBM DB2 voor Windows wordt uitgevoerd in Azure virtualisatie en het on-premises databases die gebruikmaken van de lokale data gateway. Zie de [ondersteund lijst](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2-platformen en-versies (in dit onderwerp).

De DB2-connector ondersteunt de volgende databasebewerkingen:

* Lijst databasetabellen
* Lezen van één rij met selecteren
* Lezen van alle rijen met selecteren
* Een rij invoegen met toevoegen
* Wijzigen van één rij met UPDATE
* Verwijderen van één rij met DELETE

Dit onderwerp leest u hoe u de connector in een logische app aan databasebewerkingen proces.

Zie voor meer informatie over Logic Apps, [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="available-actions"></a>Beschikbare acties
De DB2-connector ondersteunt de volgende logic app acties:

* GetTables
* GetRow
* GetRows
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Lijst met tabellen
Maken van een logische app voor een bewerking bestaat uit een groot aantal stappen uitgevoerd via de Microsoft Azure portal.

In de logische app, kunt u een actie toevoegen aan de lijst met tabellen in een DB2-database. Hiermee geeft u de actie die de connector voor het verwerken van een instructie DB2 schema zoals `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `Db2getTables`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en stel vervolgens de **Interval** naar het type **7**.  
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt `db2` in de **zoeken naar meer acties** invoervak en selecteer vervolgens **DB2 - Get-tabellen (Preview)**.
   
   ![](./media/connectors-create-api-db2/Db2connectorActions.png)  
6. In de **DB2 - Get-tabellen** deelvenster configuratie, selecteer **selectievakje** om in te schakelen **verbinden via lokale gegevensgateway**. U ziet dat de instellingen vanuit de cloud naar on-premises wijzigen.
   
   * Typ de waarde voor **Server**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `ibmserver01:50000`.
   * Typ de waarde voor **Database**. Typ bijvoorbeeld `nwind`.
   * Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **Basic**.
   * Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
   * Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
   * Selecteer de waarde voor **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
7. Selecteer **maken**, en selecteer vervolgens **opslaan**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)
8. In de **Db2getTables** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
9. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_tables**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die een lijst met tabellen bevatten.
   
   ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>De verbindingen maken
Deze connector ondersteunt verbindingen met databases die worden gehost on-premises en in de cloud met behulp van de volgende verbindingseigenschappen. 

| Eigenschap | Beschrijving |
| --- | --- |
| server |Vereist. Een tekenreekswaarde die staat voor een TCP/IP-adres of de alias in IPv4 of IPv6-indeling, gevolgd (gescheiden door puntkomma) door een TCP/IP-poortnummer accepteert. |
| database |Vereist. Accepteert een string-waarde die een DRDA relationele Database naam (RDBNAM aangeeft). DB2 voor z/OS accepteert een 16-byte-tekenreeks (de database staat bekend als een IBM DB2 voor de locatie van de z-/ OS). DB2 voor i5/OS accepteert geen 18-byte-tekenreeks (database staat bekend als een IBM DB2 voor i relationele database). DB2 voor LUW accepteert een 8-byte-tekenreeks. |
| verificatie |Optioneel. Een item lijstwaarde, Basic of Windows (kerberos) accepteert. |
| gebruikersnaam |Vereist. Een string-waarde accepteert. DB2 voor z/OS accepteert een 8-byte-tekenreeks. DB2 voor i een 10-byte-tekenreeks accepteert. DB2 voor Linux of UNIX accepteert een 8-byte-tekenreeks. DB2 voor Windows accepteert een 30-byte-tekenreeks. |
| wachtwoord |Vereist. Een string-waarde accepteert. |
| Gateway |Vereist. Een lijst item-waarde die aangeeft van de lokale data gateway gedefinieerd voor Logic Apps in de opslaggroep accepteert. |

## <a name="create-the-on-premises-gateway-connection"></a>De on-premises gatewayverbinding maken
Deze connector hebben toegang tot een lokale DB2-database met behulp van de lokale gateway. Zie gateway-onderwerpen voor meer informatie. 

1. In de **Gateways** deelvenster configuratie, selecteer **selectievakje** om in te schakelen **verbinden via de gateway**. U ziet dat de instellingen vanuit de cloud naar on-premises wijzigen.
2. Typ de waarde voor **Server**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `ibmserver01:50000`.
3. Typ de waarde voor **Database**. Typ bijvoorbeeld `nwind`.
4. Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **Basic**.
5. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
6. Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer de waarde voor **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
8. Selecteer **maken** om door te gaan. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>De verbinding van de cloud maken
Deze connector hebben toegang tot een cloud DB2-database. 

1. In de **Gateways** deelvenster configuratie laat de **selectievakje** uitgeschakeld (waar) **verbinden via de gateway**. 
2. Typ de waarde voor **verbindingsnaam**. Typ bijvoorbeeld `hisdemo2`.
3. Typ de waarde voor **DB2-servernaam**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `hisdemo2.cloudapp.net:50000`.
4. Typ de waarde voor **DB2 databasenaam**. Typ bijvoorbeeld `nwind`.
5. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
6. Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer **maken** om door te gaan. 
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Ophalen van alle rijen met selecteren
U kunt een logische app-actie voor het ophalen van alle rijen in een tabel DB2 definiëren. Dit geeft de connector voor het verwerken van een DB2 SELECT-instructie, zoals de instructie `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `Db2getRows`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt `db2` in de **zoeken naar meer acties** invoervak en selecteer vervolgens **DB2 - Get-rijen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**.
7. In de **verbindingen** deelvenster configuratie, selecteer **nieuw**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
8. In de **Gateways** deelvenster configuratie laat de **selectievakje** uitgeschakeld (waar) **verbinden via de gateway**.
   
   * Typ de waarde voor **verbindingsnaam**. Typ bijvoorbeeld `HISDEMO2`.
   * Typ de waarde voor **DB2-servernaam**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `HISDEMO2.cloudapp.net:50000`.
   * Typ de waarde voor **DB2 databasenaam**. Typ bijvoorbeeld `NWIND`.
   * Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
   * Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
9. Selecteer **maken** om door te gaan.
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)
10. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
11. Selecteer desgewenst **geavanceerde opties weergeven** queryopties opgeven.
12. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)
13. In de **Db2getRows** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
14. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die een lijst met rijen bevatten.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Een rij invoegen met toevoegen
U kunt een logische app-actie voor het toevoegen van een rij in een tabel DB2 definiëren. Deze actie Hiermee geeft u de connector voor het verwerken van een instructie INSERT DB2 zoals `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `Db2insertRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **db2** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **DB2 - rij invoegen (Preview)**.
6. In de **DB2 - rij invoegen (Preview)** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** deelvenster configuratie, selecteert u een verbinding. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**, type `Area 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
11. In de **Db2insertRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
12. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die de nieuwe rij bevatten.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Ophalen van één rij met selecteren
U kunt een logische app-actie voor het ophalen van een rij in een tabel DB2 definiëren. Deze actie Hiermee geeft u de connector voor het verwerken van een instructie DB2 selecteren waar zoals `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam** (bijvoorbeeld) "**Db2getRow**'), **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst. 
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **db2** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **DB2 - Get-rijen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuraties deelvenster een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**. 
10. Selecteer desgewenst **geavanceerde opties weergeven** queryopties opgeven.
11. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)
12. In de **Db2getRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
13. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; behoren die rij.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Wijzigen van één rij met UPDATE
U kunt een logische app actie om te wijzigen van een rij in een tabel DB2 definiëren. Deze actie Hiermee geeft u de connector voor het verwerken van een instructie UPDATE DB2 zoals `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `Db2updateRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **db2** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **DB2 - Update-rij (Preview)**.
6. In de **DB2 - Update-rij (Preview)** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuraties deelvenster, selecteer een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**, type `Updated 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)
11. In de **Db2updateRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
12. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die de nieuwe rij bevatten.
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Verwijderen van één rij met DELETE
U kunt een logische app-actie voor het verwijderen van een rij in een tabel DB2 definiëren. Deze actie Hiermee geeft u de connector voor het verwerken van een DB2-instructie DELETE, zoals `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `Db2deleteRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst. 
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** selecteert **db2** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **DB2 - rij verwijderen (Preview)**.
6. In de **DB2 - rij verwijderen (Preview)** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuraties deelvenster een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**. 
10. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)
11. In de **Db2deleteRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
12. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; dat de verwijderde rij bevatten.
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="supported-db2-platforms-and-versions"></a>Ondersteunde platforms DB2 en versies
Deze connector ondersteunt de volgende IBM DB2-platforms en versies, evenals IBM DB2 compatibele producten (bijvoorbeeld IBM Bluemix dashDB) die ondersteuning bieden voor Distributed relationele Database architectuur (DRDA) SQL toegang Manager (SQLAM) versie 10 en 11:

* IBM DB2 voor z/OS 11.1
* IBM DB2 voor z/OS 10.1
* IBM DB2 voor i 7.3
* IBM DB2 voor i 7.2
* IBM DB2 voor i 7.1
* IBM DB2 voor LUW 11
* IBM DB2 voor LUW 10.5

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/db2/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).

