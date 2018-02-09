---
title: Uw eerste Azure Database voor MySQL-database ontwerpen | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u een Azure Database voor MySQL-server en -database maakt en beheert via Azure Portal.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: 76cccf9e2ce0a1e59b43646c43ac165d46dade4a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Uw eerste Azure-database voor MySQL-database ontwerpen
Azure Database voor MySQL is een beheerde service waarmee u MySQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. Met behulp van Azure Portal kunt u eenvoudig uw server beheren en een database ontwerpen.

In deze zelfstudie leert u hoe u Azure Portal kunt gebruiken voor deze bewerkingen:

> [!div class="checklist"]
> * Een Azure Database voor MySQL maken
> * De serverfirewall configureren
> * Het opdrachtregelprogramma mysql gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Open uw favoriete webbrowser en ga naar [Microsoft Azure Portal](https://portal.azure.com/). Voer uw referenties in om u aan te melden bij de portal. De standaardweergave is uw service-dashboard.

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Een Azure Database voor MySQL-server wordt gemaakt met een gedefinieerde set [reken- en opslagresources](./concepts-compute-unit-and-storage.md). De server wordt gemaakt in een [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1. Ga naar **Databases** > **Azure Database voor MySQL**. Als u deze optie niet ziet in de categorie **Databases**, klikt u op **Alles weergeven** om alle beschikbare databaseservices weer te geven. U kunt ook **Azure Database voor MySQL** in het zoekvak typen om de service snel te vinden.
![2-1 Navigeren naar MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Klik op de tegel **Azure Database voor MySQL** en klik vervolgens op **Maken**.

In dit voorbeeld vult u het formulier Azure Database voor MySQL in met de volgende gegevens:

| **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld** |
|---|---|---|
| *Servernaam* | myserver4demo  | Servernaam moet globaal uniek zijn. |
| *Abonnement* | mysubscription | Selecteer uw abonnement in de vervolgkeuzelijst. |
| *Resourcegroep* | myResourceGroup | Maak een nieuwe resourcegroep of selecteer een bestaande. |
| *Aanmeldgegevens van serverbeheerder* | myadmin | Geef de accountnaam van de beheerder op. |
| *Wachtwoord* |  | Stel een wachtwoord voor het beheerdersaccount in. |
| *Wachtwoord bevestigen* |  | Bevestig het wachtwoord voor het beheerdersaccount. |
| *Locatie* |  | Selecteer een beschikbare regio. |
| *Versie* | 5.7 | Kies de nieuwste versie. |
| *Prestaties configureren* | Basis, 50 rekeneenheden, 50 GB  | Kies **Prijscategorie**, **Rekeneenheden**, **Opslag (GB)** en klik vervolgens op **OK**. |
| *Vastmaken aan dashboard* | Selecteren | Het is raadzaam dit selectievakje in te schakelen zodat u de server later gemakkelijk kunt terugvinden |
Klik vervolgens op **Maken**. Na een minuut of twee wordt een nieuwe Azure-database voor de MySQL-server uitgevoerd in de cloud. Klik op de knop **Meldingen** op de werkbalk om het implementatieproces te volgen.

## <a name="configure-firewall"></a>Firewall configureren
Databases van Azure Database voor MySQL worden beveiligd door een firewall. Standaard worden alle verbindingen met de server en de databases op de server geweigerd. Voordat u voor het eerst verbinding maakt met Azure Database voor MySQL moet u de firewall configureren en het IP-adres (of IP-adresbereik) van het openbare netwerk van de clientcomputer toevoegen.

1. Klik op uw zojuist gemaakte server en klik vervolgens op **Verbindingsbeveiliging**.
   ![3-1 Verbindingsbeveiliging](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. U kunt **Mijn IP toevoegen** kiezen of hier firewallregels configureren. Vergeet niet op **Opslaan** te klikken nadat u de regels hebt gemaakt.
U kunt nu verbinding maken met de server met behulp van het opdrachtregelprogramma mysql of met het GUI-hulpprogramma MySQL Workbench.

> [!TIP]
> Azure Database voor MySQL-servers communiceren via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure MySQL-server, tenzij de IT-afdeling poort 3306 openstelt.

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Ga naar Azure Portal om de volledig gekwalificeerde **servernaam** en **aanmeldingsnaam van de serverbeheerder** op te halen voor uw Azure-Database voor MySQL-server. U gebuikt de volledig gekwalificeerde servernaam om verbinding met uw server te maken via het opdrachtregelprogramma mysql. 

1. Klik in [Azure Portal](https://portal.azure.com/) op **Alle resources** in het menu aan de linkerkant, typ de naam en zoek naar uw Azure Database voor MySQL-server. Selecteer de naam van de server om de details te bekijken.

2. Klik onder Instellingen op **Eigenschappen**. Noteer de waarden voor **SERVERNAAM** en **AANMELDINGSGEGEVENS VAN DE SERVERBEHEERDER**. U kunt op de knop Kopiëren naast elk veld klikken om de gegevens naar het klembord te kopiëren.
   ![4-2 Servereigenschappen](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

In dit voorbeeld is de servernaam *myserver4demo.mysql.database.azure.com* en de aanmeldgegevens van de serverbeheerder zijn *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysql"></a>Verbinding maken met de server met behulp van mysql
Gebruik het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) om een verbinding tot stand te brengen met uw Azure-database voor MySQL-server. U kunt het opdrachtregelprogramma mysql uitvoeren in de browser met behulp van Azure Cloud Shell of het opdrachtregelprogramma starten vanaf uw eigen computer met behulp van de lokaal geïnstalleerde mysql-hulpprogramma's. Als u de Azure Cloud Shell wilt starten, klikt u op de knop `Try It` in een codeblok in dit artikel of gaat u naar Azure Portal en klikt u op het pictogram `>_` in de bovenste werkbalk rechts. 

Typ de opdracht voor verbinding:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Een lege database maken
Zodra u met de server bent verbonden, maakt u een lege database om mee te werken.
```sql
CREATE DATABASE mysampledb;
```

Voer bij de prompt de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database
U weet nu hoe u verbinding kunt maken met de Azure Database voor MySQL-database en dus is het tijd om enkele eenvoudige taken uit te voeren:

We gaan eerst een tabel maken en hierin enkele gegevens laden. Laten we een tabel maken waarin voorraadgegevens worden opgeslagen.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Gegevens laden in de tabellen
De volgende stap bestaat uit het toevoegen van gegevens aan de tabel. Voer bij de opdrachtprompt de volgende query uit om enkele rijen met gegevens in te voegen.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

We hebben nu twee rijen met voorbeeldgegevens in de tabel die u eerder hebt gemaakt.

## <a name="query-and-update-the-data-in-the-tables"></a>De gegevens in de tabellen opvragen en bijwerken
Voer de volgende query uit om gegevens op te halen uit de databasetabel.
```sql
SELECT * FROM inventory;
```

U kunt ook de gegevens in de tabellen bijwerken.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

De rij wordt dan meteen bijgewerkt wanneer u gegevens ophaalt.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip
Stel dat u per ongeluk een belangrijke databasetabel hebt verwijderd en dat de gegevens niet gemakkelijk kunnen worden hersteld. Azure Database voor MySQL biedt gelukkig de mogelijkheid om de server te herstellen naar een eerder tijdstip, door het maken van een kopie van de databases op de nieuwe server. U kunt deze nieuwe server dan gebruiken om de verwijderde gegevens te herstellen. Met de volgende stappen wordt de voorbeeldserver hersteld naar een punt voordat de tabel is toegevoegd.

1. Zoek uw Azure Database voor MySQL-server in Azure Portal. Klik op de pagina **Overzicht** op **Herstellen** op de werkbalk. De gelijknamige pagina wordt geopend.

   ![10-1 Een database herstellen](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Vul in het formulier **Herstellen** de vereiste gegevens in.
   
   ![10-2 Formulier Herstellen](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Herstelpunt**: selecteer een tijdstip in het verleden waarnaar u wilt herstellen, binnen de vermelde periode. Zorg ervoor dat u de lokale tijdzone converteert naar UTC.
   - **Herstellen naar nieuwe server**: geef de naam op van de nieuwe server waarnaar u wilt herstellen.
   - **Locatie**: de regio is hetzelfde als van de bronserver en kan niet worden gewijzigd.
   - **Prijscategorie**: de prijscategorie is hetzelfde als van de bronserver en kan niet worden gewijzigd.
   
3. Klik op **OK** om de server te [herstellen naar een eerder tijdstip](./howto-restore-server-portal.md), voordat de tabel werd toegevoegd. Als u een server herstelt, wordt er een nieuwe kopie van de server gemaakt, vanaf het tijdstip dat u opgeeft. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u Azure Portal gebruikt om het volgende te leren:

> [!div class="checklist"]
> * Een Azure Database voor MySQL maken
> * De serverfirewall configureren
> * Het opdrachtregelprogramma mysql gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

> [!div class="nextstepaction"]
> [Toepassingen koppelen met Azure Database voor MySQL](./howto-connection-string.md)
