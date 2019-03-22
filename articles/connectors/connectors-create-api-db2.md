---
title: Verbinding maken met IBM DB2 - Azure Logic Apps
description: Resources beheren met IBM DB2 REST-API's en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: plarsen, LADocs
ms.topic: article
ms.date: 08/23/2018
tags: connectors
ms.openlocfilehash: c46fd1791751ebf912670875f9c8e18e8b0c5c40
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311302"
---
# <a name="manage-ibm-db2-resources-with-azure-logic-apps"></a>IBM DB2-resources beheren met Azure Logic Apps

Met Azure Logic Apps en de IBM DB2-connector, kunt u geautomatiseerde taken en werkstromen op basis van de resources die zijn opgeslagen in de DB2-database maken. Uw werkstromen kunnen verbinding maken met de resources in uw database, lezen en lijst met uw databasetabellen, rijen toevoegen, wijzigen van de rijen en rijen verwijderen. U kunt acties opnemen in uw logische apps die te antwoorden krijgen van uw database en de uitvoer beschikbaar voor andere acties.

In dit artikel wordt beschreven hoe u een logische app waarmee u verschillende databasebewerkingen kunt maken. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="supported-platforms-and-versions"></a>Ondersteunde platforms en versies

De DB2-connector bevat een Microsoft-client die met externe servers voor DB2 via een TCP/IP-netwerk communiceert. U kunt deze connector gebruiken voor toegang tot clouddatabases zoals IBM Bluemix dashDB of IBM DB2 voor Windows die wordt uitgevoerd in Azure virtualisatie. U kunt ook toegang tot on-premises DB2-database nadat u [installeren en instellen van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-connection.md).

De IBM DB2-connector ondersteunt deze IBM DB2-platforms en versies samen met IBM DB2 compatibele producten, zoals IBM Bluemix dashDB, die ondersteuning bieden voor gedistribueerde relationele Database architectuur (DRDA) SQL Access Manager (SQLAM) versie 10 en 11:

| Platform | Versie | 
|----------|---------|
| IBM DB2 voor z/OS | 11.1, 10.1 |
| IBM DB2 voor ik | 7.3, 7.2, 7.1 |
| IBM DB2 voor LUW | 11, 10.5 |
|||

## <a name="supported-database-operations"></a>Ondersteunde databasebewerkingen

De IBM DB2-connector biedt ondersteuning voor deze databasebewerkingen, die zijn toegewezen aan de bijbehorende acties die in de connector:

| Database-bewerking | Connector-actie |
|--------------------|------------------|
| Lijst met database-tabellen | Tabellen ophalen |
| Lezen van één rij met behulp van selecteren | Rij ophalen |
| Lezen van alle rijen met behulp van selecteren | Rijen ophalen |
| Een rij met behulp van INSERT toevoegen | Rij invoegen |
| Een rij met UPDATE bewerken | Rij bijwerken |
| Verwijderen van één rij verwijderen gebruiken | Rij verwijderen |
|||

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Een IBM DB2-database, ofwel op basis van een cloud of on-premises

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot de DB2-database. Deze connector beschikt u alleen acties, om de logische App, selecteert u een afzonderlijke trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.
De voorbeelden in dit artikel gebruiken de **terugkeerpatroon** trigger.

<a name="add-db2-action"></a>

## <a name="add-db2-action---get-tables"></a>DB2-actie - Get-tabellen toevoegen

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in de ontwerper van logische App, als dit niet al geopend.

1. Kies onder de trigger **nieuwe stap**.

1. Typ 'db2' als filter in het zoekvak. In dit voorbeeld onder de lijst met acties, selecteert u deze actie: **Ophalen van tabellen (Preview)**

   ![Actie selecteren](./media/connectors-create-api-db2/select-db2-action.png)

   U wordt gevraagd nu voor de verbindingsgegevens van de DB2-database.

1. Volg de stappen voor het maken van verbindingen voor [databases in de cloud](#cloud-connection) of [van on-premises databases](#on-premises-connection).

<a name="cloud-connection"></a>

## <a name="connect-to-cloud-db2"></a>Verbinding maken met de cloud DB2

Als u uw verbinding instelt, geef de details voor deze verbinding wanneer hierom wordt gevraagd, kiest u **maken**, en sla vervolgens uw logische app:

| Eigenschap | Vereist | Description |
|----------|----------|-------------|
| **Verbinding maken via on-premises gateway** | Nee | Geldt alleen voor on-premises verbindingen. |
| **Verbindingsnaam** | Ja | De naam voor de verbinding, bijvoorbeeld 'MyLogicApp-DB2-verbinding' |
| **Server** | Ja | Het adres of de alias dubbele-poortnummer voor de DB2-server, bijvoorbeeld "myDB2server.cloudapp.net:50000" <p><p>**Opmerking**: Deze waarde is een tekenreeks met een TCP/IP-adres of alias, hetzij in IPv4 of IPv6-indeling, gevolgd door een dubbele punt en een TCP/IP-poortnummer. |
| **Database** | Ja | De naam voor uw database <p><p>**Opmerking**: Deze waarde is een tekenreeks met een DRDA relationele Database de naam (RDBNAM): <p>-DB2 voor z/OS accepteert een 16-byte-tekenreeks waar de database op een locatie 'IBM DB2 voor z/OS' wordt genoemd. <br>-DB2 voor ik een 18-byte-tekenreeks waarin de database staat bekend accepteert als een ' IBM DB2 voor ik ' relationele database. <br>-DB2 voor LUW accepteert een 8-byte-tekenreeks. |
| **Gebruikersnaam** | Ja | De naam van de gebruiker voor de database <p><p>**Opmerking**: Deze waarde is een tekenreeks waarvan de lengte is gebaseerd op de specifieke database: <p><p>-DB2 voor z/OS accepteert een 8-byte-tekenreeks. <br>-DB2 voor ik een 10-byte-tekenreeks accepteert. <br>-DB2 voor Linux of UNIX accepteert een 8-byte-tekenreeks. <br>-DB2 voor Windows accepteert een 30-byte-tekenreeks. |
| **Wachtwoord** | Ja | Uw wachtwoord voor de database |
||||

Bijvoorbeeld:

![Verbindingsgegevens voor cloud-gebaseerde databases](./media/connectors-create-api-db2/create-db2-cloud-connection.png)

<a name="on-premises-connection"></a>

## <a name="connect-to-on-premises-db2"></a>Verbinding maken met on-premises DB2

Voordat u de verbinding maakt, hebt u al uw on-premises gegevensgateway is geïnstalleerd. Anders wordt u niet voltooien om uw verbinding. Als u de gatewayinstallatie hebt, gaat u verder met het leveren van details voor deze verbinding en kies vervolgens **maken**.

| Eigenschap | Vereist | Description |
|----------|----------|-------------|
| **Verbinding maken via on-premises gateway** | Ja | Is van toepassing wanneer u wilt dat een on-premises-verbinding en ziet u de on-premises verbindingseigenschappen. |
| **Verbindingsnaam** | Ja | De naam voor de verbinding, bijvoorbeeld 'MyLogicApp-DB2-verbinding' | 
| **Server** | Ja | Het adres of de alias dubbele-poortnummer voor de DB2-server, bijvoorbeeld "myDB2server:50000" <p><p>**Opmerking**: Deze waarde is een tekenreeks met een TCP/IP-adres of alias, hetzij in IPv4 of IPv6-indeling, gevolgd door een dubbele punt en een TCP/IP-poortnummer. |
| **Database** | Ja | De naam voor uw database <p><p>**Opmerking**: Deze waarde is een tekenreeks met een DRDA relationele Database de naam (RDBNAM): <p>-DB2 voor z/OS accepteert een 16-byte-tekenreeks waar de database op een locatie 'IBM DB2 voor z/OS' wordt genoemd. <br>-DB2 voor ik een 18-byte-tekenreeks waarin de database staat bekend accepteert als een ' IBM DB2 voor ik ' relationele database. <br>-DB2 voor LUW accepteert een 8-byte-tekenreeks. |
| **Verificatie** | Ja | Het verificatietype voor de verbinding, bijvoorbeeld 'Basic' <p><p>**Opmerking**: Selecteer deze waarde in de lijst, waaronder het Basic- of Windows (Kerberos). |
| **Gebruikersnaam** | Ja | De naam van de gebruiker voor de database <p><p>**Opmerking**: Deze waarde is een tekenreeks waarvan de lengte is gebaseerd op de specifieke database: <p><p>-DB2 voor z/OS accepteert een 8-byte-tekenreeks. <br>-DB2 voor ik een 10-byte-tekenreeks accepteert. <br>-DB2 voor Linux of UNIX accepteert een 8-byte-tekenreeks. <br>-DB2 voor Windows accepteert een 30-byte-tekenreeks. |
| **Wachtwoord** | Ja | Uw wachtwoord voor de database |
| **Gateway** | Ja | De naam van uw geïnstalleerde on-premises gegevensgateway <p><p>**Opmerking**: Selecteer deze waarde in de lijst met alle geïnstalleerde gegevensgateways binnen uw Azure-abonnement en resourcegroep. |
||||

Bijvoorbeeld:

![Verbindingsgegevens voor on-premises databases](./media/connectors-create-api-db2/create-db2-on-premises-connection.png)

### <a name="view-output-tables"></a>Weergave uitvoertabellen

Kies voor het uitvoeren van uw logische app handmatig, op de werkbalk van de ontwerper **uitvoeren**. Nadat uw logische app is voltooid, kunt u de uitvoer van de uitvoering weergeven.

1. Selecteer op het menu van uw logische app, **overzicht**.

1. Onder **samenvatting**, in de **geschiedenis van uitvoeringen** sectie, selecteert u de meest recente uitvoering, wat het eerste item in de lijst.

   ![Uitvoeringsgeschiedenis weergeven](./media/connectors-create-api-db2/run-history.png)

1. Onder **logische app**, u kunt nu de status van de invoer, bekijken en uitvoer voor elke stap in uw logische app.
Vouw de **tabellen ophalen** actie.

   ![Vouw de actie](./media/connectors-create-api-db2/expand-action-step.png)

1. U kunt bekijken van de invoer **onbewerkte invoer weergeven**.

1. U kunt de uitvoer bekijken **onbewerkte uitvoer weergeven**.

   De uitvoer bevatten een lijst met tabellen.

   ![Weergave uitvoertabellen](./media/connectors-create-api-db2/db2-connector-get-tables-outputs.png)

## <a name="get-row"></a>Rij ophalen

Als u wilt één record in een DB2-database-tabel ophalen, gebruikt u de **rij ophalen** actie in uw logische app. Deze bewerking wordt een DB2 `SELECT WHERE` instructie, bijvoorbeeld `SELECT FROM AREA WHERE AREAID = '99999'`.

1. Als u in uw logische app nooit DB2 acties voordat u hebt gebruikt, raadpleegt u de stappen in de [toevoegen DB2 actie - Get-tabellen](#add-db2-action) sectie, maar toevoegen de **rij ophalen** actie in plaats daarvan en keer vervolgens hier terug om door te gaan.

   Nadat u hebt toegevoegd de **rij ophalen** actie, dit is hoe uw voorbeeld van logische app wordt weergegeven:

   ![Actie van de rij ophalen](./media/connectors-create-api-db2/db2-get-row-action.png)

1. Geef waarden op voor de vereiste eigenschappen (*). Nadat u een tabel hebt geselecteerd, ziet u de actie de relevante eigenschappen die specifiek voor de records in die tabel zijn.

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Tabelnaam** | Ja | De tabel die de record heeft u wilt, zoals 'Gebied' in dit voorbeeld |
   | **Gebied-ID** | Ja | De ID voor de record die u wilt, zoals '99999"in dit voorbeeld |
   ||||

   ![Tabel selecteren](./media/connectors-create-api-db2/db2-get-row-action-select-table.png)

1. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

### <a name="view-output-row"></a>Rij van de uitvoer weergeven

Kies voor het uitvoeren van uw logische app handmatig, op de werkbalk van de ontwerper **uitvoeren**. Nadat uw logische app is voltooid, kunt u de uitvoer van de uitvoering weergeven.

1. Selecteer op het menu van uw logische app, **overzicht**.

1. Onder **samenvatting**, in de **geschiedenis van uitvoeringen** sectie, selecteert u de meest recente uitvoering, wat het eerste item in de lijst.

1. Onder **logische app**, u kunt nu de status van de invoer, bekijken en uitvoer voor elke stap in uw logische app.
Vouw de **rij ophalen** actie.

1. U kunt bekijken van de invoer **onbewerkte invoer weergeven**.

1. U kunt de uitvoer bekijken **onbewerkte uitvoer weergeven**.

   De uitvoer zijn de opgegeven rij.

   ![Rij van de uitvoer weergeven](./media/connectors-create-api-db2/db2-connector-get-row-outputs.png)

## <a name="get-rows"></a>Rijen ophalen

Om op te halen van alle records in een DB2-database-tabel, gebruikt u de **rijen ophalen** actie in uw logische app. Deze bewerking wordt een DB2 `SELECT` instructie, bijvoorbeeld `SELECT * FROM AREA`.

1. Als u in uw logische app nooit DB2 acties voordat u hebt gebruikt, raadpleegt u de stappen in de [toevoegen DB2 actie - Get-tabellen](#add-db2-action) sectie, maar toevoegen de **rijen ophalen** actie in plaats daarvan en keer vervolgens hier terug om door te gaan.

   Nadat u hebt toegevoegd de **rijen ophalen** actie, dit is hoe uw voorbeeld van logische app wordt weergegeven:

   ![Actie rijen ophalen](./media/connectors-create-api-db2/db2-get-rows-action.png)

1. Open de **tabelnaam** lijst en selecteer vervolgens de gewenste tabel die 'Gebied' in dit voorbeeld is:

   ![Tabel selecteren](./media/connectors-create-api-db2/db2-get-rows-action-select-table.png)

1. Als u wilt opgeven van een filter of een query voor de resultaten, kies **geavanceerde opties weergeven**.

1. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

### <a name="view-output-rows"></a>Weergave uitvoer rijen

Kies voor het uitvoeren van uw logische app handmatig, op de werkbalk van de ontwerper **uitvoeren**. Nadat uw logische app is voltooid, kunt u de uitvoer van de uitvoering weergeven.

1. Selecteer op het menu van uw logische app, **overzicht**.

1. Onder **samenvatting**, in de **geschiedenis van uitvoeringen** sectie, selecteert u de meest recente uitvoering, wat het eerste item in de lijst.

1. Onder **logische app**, u kunt nu de status van de invoer, bekijken en uitvoer voor elke stap in uw logische app.
Vouw de **rijen ophalen** actie.

1. U kunt bekijken van de invoer **onbewerkte invoer weergeven**.

1. U kunt de uitvoer bekijken **onbewerkte uitvoer weergeven**.

   De uitvoer opnemen alle records in de opgegeven tabel.

   ![Weergave uitvoer rijen](./media/connectors-create-api-db2/db2-connector-get-rows-outputs.png)

## <a name="insert-row"></a>Rij invoegen

Als u wilt één record toevoegen aan een DB2-database-tabel, gebruikt u de **rij invoegen** actie in uw logische app. Deze bewerking wordt een DB2 `INSERT` instructie, bijvoorbeeld `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

1. Als u in uw logische app nooit DB2 acties voordat u hebt gebruikt, raadpleegt u de stappen in de [toevoegen DB2 actie - Get-tabellen](#add-db2-action) sectie, maar toevoegen de **rij invoegen** actie in plaats daarvan en keer vervolgens hier terug om door te gaan.

   Nadat u hebt toegevoegd de **rij invoegen** actie, dit is hoe uw voorbeeld van logische app wordt weergegeven:

   ![Actie van de rij invoegen](./media/connectors-create-api-db2/db2-insert-row-action.png)

1. Geef waarden op voor de vereiste eigenschappen (*). Nadat u een tabel hebt geselecteerd, ziet u de actie de relevante eigenschappen die specifiek voor de records in die tabel zijn.

   Dit zijn de eigenschappen voor dit voorbeeld:

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Tabelnaam** | Ja | De tabel waar de record, zoals 'Gebied' toe te voegen |
   | **Gebied-ID** | Ja | De ID voor het gebied toe te voegen, zoals "99999" |
   | **Beschrijving van gebied** | Ja | De beschrijving van het gebied toe te voegen, zoals "Gebied 99999" |
   | **Regio-ID** | Ja | De ID voor de regio om toe te voegen, zoals "102" |
   |||| 

   Bijvoorbeeld:

   ![Tabel selecteren](./media/connectors-create-api-db2/db2-insert-row-action-select-table.png)

1. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

### <a name="view-insert-row-outputs"></a>Weergave uitvoer van de rij invoegen

Kies voor het uitvoeren van uw logische app handmatig, op de werkbalk van de ontwerper **uitvoeren**. Nadat uw logische app is voltooid, kunt u de uitvoer van de uitvoering weergeven.

1. Selecteer op het menu van uw logische app, **overzicht**.

1. Onder **samenvatting**, in de **geschiedenis van uitvoeringen** sectie, selecteert u de meest recente uitvoering, wat het eerste item in de lijst.

1. Onder **logische app**, u kunt nu de status van de invoer, bekijken en uitvoer voor elke stap in uw logische app.
Vouw de **rij invoegen** actie.

1. U kunt bekijken van de invoer **onbewerkte invoer weergeven**.

1. U kunt de uitvoer bekijken **onbewerkte uitvoer weergeven**.

   De uitvoer zijn de record die u hebt toegevoegd aan de opgegeven tabel.

   ![Weergave uitvoer met de ingevoegde rij](./media/connectors-create-api-db2/db2-connector-insert-row-outputs.png)

## <a name="update-row"></a>Rij bijwerken

Als u wilt één record in een DB2-database-tabel bijwerken, gebruikt u de **rij bijwerken** actie in uw logische app. Deze bewerking wordt een DB2 `UPDATE` instructie, bijvoorbeeld `UPDATE AREA SET AREAID = '99999', AREADESC = 'Updated 99999', REGIONID = 102)`.

1. Als u in uw logische app nooit DB2 acties voordat u hebt gebruikt, raadpleegt u de stappen in de [toevoegen DB2 actie - Get-tabellen](#add-db2-action) sectie, maar toevoegen de **rij bijwerken** actie in plaats daarvan en keer vervolgens hier terug om door te gaan.

   Nadat u hebt toegevoegd de **rij bijwerken** actie, dit is hoe uw voorbeeld van logische app wordt weergegeven:

   ![Actie van de rij bijwerken](./media/connectors-create-api-db2/db2-update-row-action.png)

1. Geef waarden op voor de vereiste eigenschappen (*). Nadat u een tabel hebt geselecteerd, ziet u de actie de relevante eigenschappen die specifiek voor de records in die tabel zijn.

   Dit zijn de eigenschappen voor dit voorbeeld:

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Tabelnaam** | Ja | De tabel waar de record, zoals "Gebied" bij te werken |
   | **Rij-ID** | Ja | De ID voor de record bij te werken, zoals "99999" |
   | **Gebied-ID** | Ja | De nieuwe gebied-ID, zoals "99999" |
   | **Beschrijving van gebied** | Ja | De nieuwe beschrijving gebied, zoals "Bijgewerkt 99999" |
   | **Regio-ID** | Ja | De nieuwe regio-ID, zoals "102" |
   ||||

   Bijvoorbeeld:

   ![Tabel selecteren](./media/connectors-create-api-db2/db2-update-row-action-select-table.png)

1. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

### <a name="view-update-row-outputs"></a>Uitvoer van de rij bijwerken weergeven

Kies voor het uitvoeren van uw logische app handmatig, op de werkbalk van de ontwerper **uitvoeren**. Nadat uw logische app is voltooid, kunt u de uitvoer van de uitvoering weergeven.

1. Selecteer op het menu van uw logische app, **overzicht**.

1. Onder **samenvatting**, in de **geschiedenis van uitvoeringen** sectie, selecteert u de meest recente uitvoering, wat het eerste item in de lijst.

1. Onder **logische app**, u kunt nu de status van de invoer, bekijken en uitvoer voor elke stap in uw logische app.
Vouw de **rij bijwerken** actie.

1. U kunt bekijken van de invoer **onbewerkte invoer weergeven**.

1. U kunt de uitvoer bekijken **onbewerkte uitvoer weergeven**.

   De uitvoer zijn de record die u in de opgegeven tabel bijgewerkt.

   ![Weergave uitvoer met de bijgewerkte rij](./media/connectors-create-api-db2/db2-connector-update-row-outputs.png)

## <a name="delete-row"></a>Rij verwijderen

Als u wilt één record verwijderen uit een DB2-database-tabel, gebruikt u de **rij verwijderen** actie in uw logische app. Deze bewerking wordt een DB2 `DELETE` instructie, bijvoorbeeld `DELETE FROM AREA WHERE AREAID = '99999'`.

1. Als u in uw logische app nooit DB2 acties voordat u hebt gebruikt, raadpleegt u de stappen in de [toevoegen DB2 actie - Get-tabellen](#add-db2-action) sectie, maar toevoegen de **rij verwijderen** actie in plaats daarvan en keer vervolgens hier terug om door te gaan.

   Nadat u hebt toegevoegd de **rij verwijderen** actie, dit is hoe uw voorbeeld van logische app wordt weergegeven:

   ![Rij actie verwijderen](./media/connectors-create-api-db2/db2-delete-row-action.png)

1. Geef waarden op voor de vereiste eigenschappen (*). Nadat u een tabel hebt geselecteerd, ziet u de actie de relevante eigenschappen die specifiek voor de records in die tabel zijn.

   Dit zijn de eigenschappen voor dit voorbeeld:

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Tabelnaam** | Ja | De tabel waar de record, zoals 'Gebied' verwijderen |
   | **Rij-ID** | Ja | De ID voor de record te verwijderen, zoals "99999" |
   ||||

   Bijvoorbeeld:

   ![Tabel selecteren](./media/connectors-create-api-db2/db2-delete-row-action-select-table.png)

1. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

### <a name="view-delete-row-outputs"></a>Weergave uitvoer van de rij verwijderen

Kies voor het uitvoeren van uw logische app handmatig, op de werkbalk van de ontwerper **uitvoeren**. Nadat uw logische app is voltooid, kunt u de uitvoer van de uitvoering weergeven.

1. Selecteer op het menu van uw logische app, **overzicht**.

1. Onder **samenvatting**, in de **geschiedenis van uitvoeringen** sectie, selecteert u de meest recente uitvoering, wat het eerste item in de lijst.

1. Onder **logische app**, u kunt nu de status van de invoer, bekijken en uitvoer voor elke stap in uw logische app.
Vouw de **rij verwijderen** actie.

1. U kunt bekijken van de invoer **onbewerkte invoer weergeven**.

1. U kunt de uitvoer bekijken **onbewerkte uitvoer weergeven**.

   De uitvoer niet langer deel de record die u hebt verwijderd uit de opgegeven tabel.

   ![Weergave uitvoer zonder verwijderde rij](./media/connectors-create-api-db2/db2-connector-delete-row-outputs.png)

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie, zoals triggers en acties limieten, zoals is beschreven in van de connector OpenAPI (voorheen Swagger)-bestand, raadpleegt u de [van de connector-verwijzingspagina](/connectors/db2/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
