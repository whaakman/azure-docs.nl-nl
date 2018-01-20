---
title: De Informix-connector in uw logische Apps toevoegen | Microsoft Docs
description: Overzicht van de connector Informix met parameters van de REST-API
services: 
documentationcenter: 
author: gplarsen
manager: anneta
editor: 
tags: connectors
ms.assetid: ca2393f0-3073-4dc2-8438-747f5bc59689
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: b3b352b185b7dfeee12ac9bee1b72cb740add5b8
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-informix-connector"></a>Aan de slag met de Informix-connector
Microsoft-connector voor Informix verbindt Logic Apps met resources die zijn opgeslagen in een IBM Informix-database. De connector Informix bevat een Microsoft-client om te communiceren met externe computers voor Informix-server via een TCP/IP-netwerk. Dit omvat cloud databases, zoals IBM Informix voor Windows wordt uitgevoerd in Azure virtualisatie en het on-premises databases die gebruikmaken van de lokale data gateway. Zie de [ondersteund lijst](connectors-create-api-informix.md#supported-informix-platforms-and-versions) IBM Informix-platformen en-versies (in dit onderwerp).

De connector ondersteunt de volgende databasebewerkingen:

* Lijst databasetabellen
* Lezen van één rij met selecteren
* Lezen van alle rijen met selecteren
* Een rij invoegen met toevoegen
* Wijzigen van één rij met UPDATE
* Verwijderen van één rij met DELETE

Dit onderwerp leest u hoe u de connector in een logische app aan databasebewerkingen proces.

Zie voor meer informatie over Logic Apps, [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="available-actions"></a>Beschikbare acties
Deze connector ondersteunt de volgende logic app acties:

* Getables
* GetRow
* GetRows
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Lijst met tabellen
Maken van een logische app voor een bewerking bestaat uit een groot aantal stappen uitgevoerd via de Microsoft Azure portal.

In de logische app, kunt u een actie toevoegen aan de lijst met tabellen in een Informix-database. Deze actie Hiermee geeft u de connector voor het verwerken van een instructie Informix schema zoals `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixgetTables`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**.  
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - Get-tabellen (Preview)**.
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. In de **Informix - Get-tabellen** deelvenster configuratie, selecteer **selectievakje** om in te schakelen **verbinden via lokale gegevensgateway**. U ziet dat de instellingen vanuit de cloud naar on-premises wijzigen.
   
   * Typ de waarde voor **Server**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `ibmserver01:9089`.
   * Typ de waarde voor **Database**. Typ bijvoorbeeld `nwind`.
   * Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **Basic**.
   * Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
   * Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
   * Selecteer de waarde voor **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
7. Selecteer **maken**, en selecteer vervolgens **opslaan**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. In de **InformixgetTables** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
9. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_tables**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die een lijst met tabellen bevatten.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>De verbindingen maken
Deze connector ondersteunt verbindingen met database on-premises en in de cloud met behulp van de volgende verbindingseigenschappen. 

| Eigenschap | Beschrijving |
| --- | --- |
| server |Vereist. Accepteert een string-waarde die aangeeft een TCP/IP-adres of de alias in IPv4 of IPv6-indeling, gevolgd (puntkomma's gescheiden) door een TCP/IP-poortnummer. |
| database |Vereist. Accepteert een string-waarde die aangeeft een DRDA relationele Database naam (RDBNAM). Informix accepteert een 128-byte-tekenreeks (de database staat bekend als de naam van een IBM Informix-database (dbname)). |
| verificatie |Optioneel. Een item lijstwaarde, Basic of Windows (kerberos) accepteert. |
| gebruikersnaam |Vereist. Een string-waarde accepteert. |
| wachtwoord |Vereist. Een string-waarde accepteert. |
| Gateway |Vereist. Een lijst item-waarde die aangeeft van de lokale data gateway gedefinieerd voor Logic Apps in de opslaggroep accepteert. |

## <a name="create-the-on-premises-gateway-connection"></a>De on-premises gatewayverbinding maken
Deze connector hebben toegang tot een lokale Informix-database met behulp van de lokale data gateway. Zie gateway-onderwerpen voor meer informatie. 

1. In de **Gateways** deelvenster configuratie, selecteer **selectievakje** om in te schakelen **verbinden via de gateway**. Zie de instellingen wijzigen vanuit de cloud naar on-premises.
2. Typ de waarde voor **Server**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `ibmserver01:9089`.
3. Typ de waarde voor **Database**. Typ bijvoorbeeld `nwind`.
4. Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **Basic**.
5. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
6. Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer de waarde voor **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
8. Selecteer **maken** om door te gaan. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>De verbinding van de cloud maken
Deze connector hebben toegang tot een cloud Informix-database. 

1. In de **Gateways** deelvenster configuratie laat de **selectievakje** uitgeschakeld (waar) **verbinden via de gateway**. 
2. Typ de waarde voor **verbindingsnaam**. Typ bijvoorbeeld `hisdemo2`.
3. Typ de waarde voor **Informix-servernaam**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `hisdemo2.cloudapp.net:9089`.
4. Typ de waarde voor **Informix databasenaam**. Typ bijvoorbeeld `nwind`.
5. Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
6. Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer **maken** om door te gaan. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Ophalen van alle rijen met selecteren
U kunt een logische app-actie voor het ophalen van alle rijen in de tabel Informix maken. Deze actie Hiermee geeft u de connector voor het verwerken van een Informix SELECT-instructie, zoals `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam** (bijvoorbeeld) "**InformixgetRows**'), **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - Get-rijen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**.
7. In de **verbindingen** deelvenster configuratie, selecteer **nieuw**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. In de **Gateways** deelvenster configuratie laat de **selectievakje** uitgeschakeld (waar) **verbinden via de gateway**.
   
   * Typ de waarde voor **verbindingsnaam**. Typ bijvoorbeeld `HISDEMO2`.
   * Typ de waarde voor **Informix-servernaam**, in de vorm van een poortnummer of de alias dubbele punt. Typ bijvoorbeeld `HISDEMO2.cloudapp.net:9089`.
   * Typ de waarde voor **Informix databasenaam**. Typ bijvoorbeeld `NWIND`.
   * Typ de waarde voor **gebruikersnaam**. Typ bijvoorbeeld `informix`.
   * Typ de waarde voor **wachtwoord**. Typ bijvoorbeeld `Password1`.
9. Selecteer **maken** om door te gaan.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
11. Selecteer desgewenst **geavanceerde opties weergeven** queryopties opgeven.
12. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. In de **InformixgetRows** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
14. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die een lijst met rijen bevatten.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Een rij invoegen met toevoegen
U kunt een logische app-actie voor het toevoegen van een rij in een Informix-tabel maken. Deze actie Hiermee geeft u de connector voor het verwerken van een instructie INSERT Informix zoals `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixinsertRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - rij invoegen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** deelvenster configuratie, selecteren om een verbinding te kiezen. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**, type `Area 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. In de **InformixinsertRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
12. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die de nieuwe rij bevatten.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Ophalen van één rij met selecteren
U kunt een logische app-actie voor het ophalen van een rij in een Informix-tabel maken. Deze actie Hiermee geeft u de connector voor het verwerken van een instructie Informix selecteren waar zoals `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixgetRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - Get-rijen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuraties deelvenster, selecteer een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**. 
10. Selecteer desgewenst **geavanceerde opties weergeven** queryopties opgeven.
11. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. In de **InformixgetRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
13. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; behoren die rij.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Wijzigen van één rij met UPDATE
U kunt een logische app actie om te wijzigen van een rij in een Informix-tabel maken. Deze actie Hiermee geeft u de connector voor het verwerken van een instructie UPDATE Informix zoals `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixupdateRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - Update-rij (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuraties deelvenster, selecteer een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**, type `Updated 99999`, en het type `102` voor **REGIONID**. 
10. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. In de **InformixupdateRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
12. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; die de nieuwe rij bevatten.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Verwijderen van één rij met DELETE
U kunt een logische app-actie voor het verwijderen van een rij in een Informix-tabel maken. Deze actie Hiermee geeft u de connector voor het verwerken van een Informix-instructie DELETE, zoals `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logische app maken
1. In de **Azure start board**, selecteer  **+**  (plusteken) **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals `InformixdeleteRow`, **abonnement**, **resourcegroep**, **locatie**, en **App Service-Plan**. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**.

### <a name="add-a-trigger-and-action"></a>Toevoegen van een trigger en action
1. In de **Logic Apps Designer**, selecteer **leeg LogicApp** in de **sjablonen** lijst.
2. In de **triggers** selecteert **terugkeerpatroon**. 
3. In de **terugkeerpatroon** worden geactiveerd, selecteer **bewerken**, selecteer **frequentie** vervolgkeuzelijst selecteren **dag**, en selecteer vervolgens **Interval** naar het type **7**. 
4. Selecteer de **+ een nieuwe stap** vak en selecteer vervolgens **een actie toevoegen**.
5. In de **acties** typt **informix** in de **zoeken naar meer acties** invoervak en selecteer vervolgens **Informix - rij verwijderen (Preview)**.
6. In de **rijen (Preview) ophalen** actie, selecteer **verbinding wijzigen**. 
7. In de **verbindingen** configuraties deelvenster een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. In de **tabelnaam** lijst, selecteert de **pijl-omlaag**, en selecteer vervolgens **gebied**.
9. Geef waarden voor alle vereiste kolommen (Zie rode sterretjes). Typ bijvoorbeeld `99999` voor **gebieds-id**. 
10. Selecteer **Opslaan**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. In de **InformixdeleteRow** blade in de **alle sessies** lijst onder **samenvatting**, selecteert u het item eerst weergegeven (meest recente uitvoeren).
12. In de **logische app uitgevoerd** blade Selecteer **Details uitvoering van**. Binnen de **actie** selecteert **Get_rows**. Zie de waarde voor **Status**, die moet worden **geslaagd**. Selecteer de **invoer koppeling** om weer te geven van de invoer. Selecteer de **uitvoer koppeling**, en de uitvoer weergeven; dat de verwijderde rij bevatten.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Ondersteunde platforms Informix en versies
Deze connector ondersteunt de volgende versies van de IBM Informix wanneer geconfigureerd ter ondersteuning van clientverbindingen gedistribueerd relationele Database architectuur (DRDA).

* IBM Informix 12.1
* IBM Informix 11,7

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/informix/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).

