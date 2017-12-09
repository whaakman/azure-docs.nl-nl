---
title: Ontwerp van uw eerste Azure-Database voor de MySQL-database. Azure-Portal | Microsoft Docs
description: Deze zelfstudie wordt uitgelegd hoe maken en beheren van Azure-Database voor MySQL-server en database via Azure Portal.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: 9e3b498aa28a1075c8ed65ea698df955824b9893
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Ontwerp van uw eerste Azure-Database voor de MySQL-database
Azure Database voor MySQL is een beheerde service waarmee u MySQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. Met de Azure portal, kunt u eenvoudig beheren van uw server en ontwerpen van een database.

In deze zelfstudie maakt u de Azure portal gebruiken voor meer informatie over hoe:

> [!div class="checklist"]
> * Een Azure-Database maken voor MySQL
> * De serverfirewall configureren
> * Mysql-opdrachtregelprogramma gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Open uw favoriete webbrowser en Ga naar de [Microsoft Azure-portal](https://portal.azure.com/). Voer uw referenties in om u aan te melden bij de portal. De standaardweergave is uw service-dashboard.

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Een Azure Database voor MySQL-server wordt gemaakt met een gedefinieerde set [reken- en opslagresources](./concepts-compute-unit-and-storage.md). De server wordt gemaakt in een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1. Navigeer naar **Databases** > **Azure MySQL-Database**. Als u niet kunt vinden MySQL-Server onder **Databases** categorie, klikt u op **alle** om alle beschikbare databaseservices weer te geven. U kunt ook typen **Azure Database voor MySQL** in het zoekvak om snel de service niet vinden.
![2-1, navigeer naar MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Klik op **Azure Database voor MySQL** tegel en klik vervolgens op **maken**.

Vul de Azure-Database MySQL formulier met de volgende informatie in dit voorbeeld:

| **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld** |
|---|---|---|
| *Servernaam* | myserver4demo  | Servernaam moet globaal uniek zijn. |
| *Abonnement* | mysubscription | Selecteer uw abonnement in de vervolgkeuzelijst. |
| *Resourcegroep* | myResourceGroup | Maak een nieuwe resourcegroep of selecteer een bestaande. |
| *Aanmeldgegevens van serverbeheerder* | myadmin | Accountnaam van de Setup-beheerder. |
| *Wachtwoord* |  | Stel een wachtwoord voor het beheerdersaccount in. |
| *Wachtwoord bevestigen* |  | Bevestig het wachtwoord voor het beheerdersaccount. |
| *Locatie* |  | Selecteer een beschikbare regio. |
| *Versie* | 5.7 | Kies de nieuwste versie. |
| *Prestaties configureren* | Basis, 50 compute-eenheden, 50 GB  | Kies **Prijscategorie**, **Rekeneenheden**, **Opslag (GB)** en klik vervolgens op **OK**. |
| *Vastmaken aan dashboard* | Selecteren | Het is raadzaam dit selectievakje in te schakelen zodat u de server later gemakkelijk kunt terugvinden |
Klik vervolgens op **Maken**. Na een minuut of twee wordt een nieuwe Azure-database voor de MySQL-server uitgevoerd in de cloud. U kunt klikken op **meldingen** op de werkbalk om te controleren van het implementatieproces.

## <a name="configure-firewall"></a>Firewall configureren
Azure voor MySQL-Databases worden beveiligd door een firewall. Standaard worden alle verbindingen met de server en de databases binnen de server geweigerd. Voordat u verbinding maakt met Azure-Database voor MySQL voor de eerste keer, de firewall configureren voor IP-adres van de client-computer openbaar netwerk (of IP-adresbereik) toevoegen.

1. Klik op de nieuwe virtuele server en klik vervolgens op **verbindingsbeveiliging**.
   ![Beveiliging 3-1-verbindingen](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. U kunt **Mijn IP toevoegen**, of hier firewallregels configureren. Vergeet niet op **Opslaan** te klikken nadat u de regels hebt gemaakt.
U kunt nu verbinding met de server met het opdrachtregelprogramma mysql of MySQL Workbench GUI-hulpprogramma.

> [!TIP]
> Azure-Database voor de MySQL-server communiceert via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan door de firewall van uw netwerk. Zo ja, u geen verbinding maken met Azure MySQL-server tenzij poort 3306 wordt geopend door uw IT-afdeling.

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Ophalen van de volledig gekwalificeerde **servernaam** en **aanmeldingsnaam van Server-beheerder** voor uw Azure-Database voor MySQL-server van de Azure-portal. De volledig gekwalificeerde servernaam kunt u verbinding met de server met het opdrachtregelprogramma mysql. 

1. In [Azure-portal](https://portal.azure.com/), klikt u op **alle resources** in het menu links, typ de naam en zoek naar uw Azure-Database voor de MySQL-server. Selecteer de naam van de server om de details weer te geven.

2. Onder de instellingen voor de kop, klikt u op **eigenschappen**. Noteer **servernaam** en **AANMELDINGSNAAM van SERVER-beheerder**. U mag klikt u op de knop kopiëren naast elk veld naar het Klembord kopiëren.
   ![Eigenschappen van de server 4-2](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

In dit voorbeeld is de servernaam *myserver4demo.mysql.database.azure.com* en de aanmeldgegevens van de serverbeheerder zijn *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysql"></a>Verbinding maken met de server met behulp van mysql
Gebruik het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) om een verbinding tot stand te brengen met uw Azure-database voor MySQL-server. U kunt het opdrachtregelhulpprogramma mysql uitvoeren vanuit de Azure-Cloud-Shell in de browser of vanuit uw eigen computer lokaal met behulp van mysql-hulpprogramma's geïnstalleerd. Start de Azure-Cloud-Shell, klikt u op de `Try It` op een codeblok in dit artikel of Ga naar de Azure-portal en klik op de `>_` pictogram in de bovenste werkbalk rechts. 

Typ de opdracht voor verbinding:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Een lege database maken
Als u met de server verbonden bent, een lege database maken voor gebruik met.
```sql
CREATE DATABASE mysampledb;
```

Voer de volgende opdracht verbinding overschakelen naar de nieuwe database op de opdrachtprompt:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database
Nu dat u hoe u verbinding maken met de Azure-Database voor de MySQL-database weet, kunt u enkele eenvoudige taken voltooien:

Eerst een tabel maken en deze met enkele gegevens te laden. We maken een tabel met inventarisatie-informatie.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Gegevens laden in de tabellen
Nu dat u een tabel hebt, moet u enkele gegevens invoegen in het. Voer de volgende query voor het invoegen van een aantal rijen van de gegevens in het venster opdrachtprompt openen.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

U hebt nu twee rijen van de voorbeeldgegevens in de tabel die u eerder hebt gemaakt.

## <a name="query-and-update-the-data-in-the-tables"></a>Vragen en de gegevens in de tabellen bijwerken
De volgende query om informatie te halen uit de databasetabel.
```sql
SELECT * FROM inventory;
```

U kunt ook de gegevens in de tabellen bijwerken.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

De rij wordt dienovereenkomstig bijgewerkt wanneer u gegevens ophaalt.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip
Stel dat u een belangrijke databasetabel per ongeluk hebt verwijderd en de gegevens niet gemakkelijk kan niet worden hersteld. Azure MySQL-Database kunt u de server te herstellen naar een punt in tijd, het maken van een kopie van de databases in de nieuwe server. U kunt deze nieuwe server gebruiken om uw verwijderde gegevens te herstellen. Voordat u de tabel is toegevoegd, de volgende stappen uit de voorbeeldserver herstellen naar een punt.

1. Zoek uw Azure-Database voor MySQL in de Azure-portal. Op de **overzicht** pagina, klikt u op **herstellen** op de werkbalk. De pagina voor herstel wordt geopend.

   ![10-1 herstel van een database](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Vul de **herstellen** formulier met de vereiste informatie.
   
   ![10-2-formulier voor herstel](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Herstelpunt**: Selecteer een point-in-time die u herstellen wilt, binnen de periode die wordt vermeld. Zorg ervoor dat uw lokale tijdzone converteren naar UTC.
   - **Herstellen naar de nieuwe server**: Geef een nieuwe servernaam die u wilt herstellen.
   - **Locatie**: de regio is hetzelfde als de bronserver en kan niet worden gewijzigd.
   - **Prijscategorie**: de prijscategorie is hetzelfde als de bronserver en kan niet worden gewijzigd.
   
3. Klik op **OK** de server te herstellen [herstellen naar een punt in tijd](./howto-restore-server-portal.md) voordat de tabel is verwijderd. Herstellen van een server, maakt een nieuwe kopie van de server, vanaf het punt in tijd die u opgeeft. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie gebruikt u de Azure portal naar geleerde how to:

> [!div class="checklist"]
> * Een Azure-Database maken voor MySQL
> * De serverfirewall configureren
> * Mysql-opdrachtregelprogramma gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

> [!div class="nextstepaction"]
> [Verbinding maken tussen toepassingen met Azure-Database voor MySQL](./howto-connection-string.md)
