---
title: 'Zelfstudie: Een Azure Database for PostgreSQL - één Server met behulp van Azure portal ontwerpen'
description: In deze zelfstudie laat zien hoe uw eerste Azure Database for PostgreSQL - één Server met behulp van de Azure-portal te ontwerpen.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 06/25/2019
ms.openlocfilehash: 421d5cde46b466c0c13a52755abdf137e52f2f6b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443105"
---
# <a name="tutorial-design-an-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Zelfstudie: Een Azure Database for PostgreSQL - één Server met behulp van de Azure-portal ontwerpen

Azure Database voor PostgreSQL is een beheerde service waarmee u PostgreSQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. Met behulp van Azure Portal kunt u eenvoudig uw server beheren en een database ontwerpen.

In deze zelfstudie leert u hoe u Azure Portal kunt gebruiken voor deze bewerkingen:
> [!div class="checklist"]
> * Een Azure-database voor PostgreSQL-server maken
> * De serverfirewall configureren
> * Het hulpprogramma [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

## <a name="prerequisites"></a>Vereisten
Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-an-azure-database-for-postgresql"></a>Een Azure Database voor PostgreSQL-server maken

Een Azure Database voor PostgreSQL-server wordt gemaakt met een gedefinieerde set [reken- en opslagresources](./concepts-compute-unit-and-storage.md). De server wordt gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md).

Volg deze stappen voor het maken van een Azure Database voor PostgreSQL-server:
1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Azure Database voor PostgreSQL** op de pagina **Databases**.
   ![Azure Database voor PostgreSQL - De database maken](./media/tutorial-design-database-using-azure-portal/1-create-database.png)

3. Selecteer de **één server** Implementatieoptie.

   ![Azure Database voor PostgreSQL - optie voor implementatie van één server selecteren](./media/tutorial-design-database-using-azure-portal/select-deployment-option.png)

4. Vul de **basisbeginselen** formulier met de volgende informatie:

    ![Een server maken](./media/tutorial-design-database-using-azure-portal/create-basics.png)

    Instelling|Voorgestelde waarde|Description
    ---|---|---
    Abonnement|De naam van uw abonnement|Het Azure-abonnement dat u wilt gebruiken voor uw server. Als u meerdere abonnementen hebt, kiest u het abonnement waarin u wordt gefactureerd voor de resource.
    Resourcegroep|*myresourcegroup*| Een nieuwe resourcegroepnaam of een bestaande naam uit uw abonnement.
    Servernaam |*mydemoserver*|Een unieke naam ter identificatie van uw Azure Database for PostgreSQL-server. De domeinnaam *postgres.database.azure.com* wordt toegevoegd aan de naam van de server die u opgeeft. De server mag alleen kleine letters, cijfers en het koppelteken (-) bevatten. Hij moet ten minste 3 en mag maximaal 63 tekens bevatten.
    Gegevensbron | *Geen* | Selecteer *geen* te maken van een nieuwe server maken. (Selecteer *Back-up* als u een server maakt op basis van een geo-back-up van een bestaande Azure Database for PostgreSQL-server).
    Gebruikersnaam van beheerder |*myadmin*| Uw eigen aanmeldingsaccount dat moet worden gebruikt om verbinding te maken met de server. De aanmeldingsnaam voor de beheerder mag niet **azure_superuser,** **azure_pg_admin,** **admin,** **administrator,** **root,** **guest** of **public** zijn. De naam mag niet met **pg_** beginnen.
    Wachtwoord |Uw wachtwoord| Een nieuw wachtwoord voor het beheerdersaccount voor de server. Dit wachtwoord moet tussen 8 en 128 tekens bevatten. Uw wachtwoord moet tekens bevatten uit drie van de volgende categorieën: Nederlandse hoofdletters, Nederlandse kleine letters, cijfers (0 t/m 9) en niet-alfanumerieke tekens (!, $, #, % enzovoort).
    Location|De regio het dichtst bij uw gebruikers| De locatie die zich het dichtst bij uw gebruikers bevindt.
    Version|De meest recente primaire versie| De nieuwste primaire versie van PostgreSQL, tenzij u specifieke andere vereisten hebt.
    COMPUTE en opslag | **Algemeen**, **Gen 5**, **2 vCores**, **5 GB**, **7 dagen**, **Geografisch redundant** | De reken-, opslag- en back-upconfiguraties voor de nieuwe server. Selecteer **-server configureren**. Selecteer vervolgens het tabblad **Algemeen**. *Gen 5*, *4 vCores*, *100 GB*, en *7 dagen* zijn de standaardwaarden voor **bewerking voor Compute**,  **vCore**, **opslag**, en **bewaarperiode voor back-up**. U kunt deze schuifregelaars laten zoals is of pas ze aan. Als u serverback-ups in geografisch redundante opslag wilt inschakelen, selecteert u **Geografisch redundant** in het gedeelte **Redundantieopties voor back-up**. Selecteer **OK** om deze geselecteerde prijscategorie op te slaan. Deze selecties worden afgebeeld in de volgende schermopname.

   > [!NOTE]
   > Overweeg het gebruik van de prijscategorie Basic als lichte reken- en I/O-capaciteit voldoende is voor uw workload. Servers die zijn gemaakt in de prijscategorie Basic kunnen later niet meer worden geschaald voor Algemeen gebruik of Geoptimaliseerd voor geheugen. Zie de pagina met [prijzen](https://azure.microsoft.com/pricing/details/postgresql/) voor meer informatie.
   > 

    ![Het deelvenster Prijscategorie](./media/quickstart-create-database-portal/2-pricing-tier.png)

    > [!TIP]
    > Met **automatische groei** ingeschakeld op uw server toeneemt opslag wanneer u de toegewezen limiet nadert zonder gevolgen voor uw workload.

5. Selecteer **bekijken + maken** naar Controleer uw selecties. Selecteer **Maken** om de server in te richten. Deze bewerking kan enkele minuten duren.

6. Selecteer het pictogram **Meldingen** (een klok) op de werkbalk om het implementatieproces te bewaken. Na de implementatie kunt u **Vastmaken aan dashboard** selecteren. Hiermee maakt u een tegel voor deze server op uw dashboard in Azure Portal als snelkoppeling naar de **overzichtspagina** van de server. Als u **Naar de resource gaan** selecteert, wordt de **overzichtspagina** van de server weergegeven.

    ![Het deelvenster Meldingen](./media/quickstart-create-database-portal/3-notifications.png)
   
   Standaard word een **postgres**-database op uw server gemaakt. De database [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) is een standaarddatabase die kan worden gebruikt door gebruikers, hulpprogramma's en toepassingen van derden. (De andere standaarddatabase is **azure_maintenance**. De functie van de database is het scheiden van beheerde serviceprocessen van acties van de gebruiker. U hebt geen toegang tot deze database.)


## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

De Azure Database for PostgreSQL-service gebruikt een firewall op serverniveau. Standaard voorkomt deze firewall dat externe toepassingen en hulpprogramma's verbinding maken met de server of databases op de server, tenzij er een firewallregel wordt gemaakt om de firewall te openen voor een specifiek IP-adresbereik. 

1. Nadat de implementatie is voltooid, klikt u op **Alle resources** in het menu links en voert u de naam **mydemoserver** in om te zoeken naar de zojuist gemaakte server. Klik op de servernaam in de zoekresultaten. De pagina **Overzicht** wordt geopend voor uw server en biedt opties voor verdere configuratie.

   ![Azure Database voor PostgreSQL - Zoeken naar de server](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2. Selecteer **Verbindingsbeveiliging** op de serverpagina. 

3. Klik in het tekstvak onder **regelnaam** en voeg een nieuwe firewallregel om op te geven van het IP-bereik voor connectiviteit. Voer uw IP-bereik in. Klik op **Opslaan**.

   ![Azure Database voor PostgreSQL - Een firewallregel maken](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4. Klik op **Opslaan** en klik vervolgens op **X** om de pagina **Verbindingsbeveiliging** te sluiten.

   > [!NOTE]
   > De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 5432 openstelt.
   >

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Toen u de Azure Database for PostgreSQL-server maakte, is de standaarddatabase **postgres** ook gemaakt. Als u verbinding met uw databaseserver wilt maken, moet u hostgegevens en toegangsreferenties opgeven.

1. Klik in het menu links in Azure Portal op **Alle resources** en zoek de server die u zojuist hebt gemaakt.

   ![Azure Database voor PostgreSQL - Zoeken naar de server](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2. Klik op de servernaam **mydemoserver**.

3. Selecteer de pagina **Overzicht** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**.

   ![Azure Database voor PostgreSQL - Aanmeldgegevens van de serverbeheerder](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-to-postgresql-database-using-psql"></a>Verbinding maken met een PostgreSQL-database met behulp van psql
Als op uw clientcomputer PostgreSQL is geïnstalleerd, kunt u een lokale instantie van [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) of de Azure Cloud Console gebruiken om verbinding te maken met een Azure PostgreSQL-server. U gaat nu het opdrachtregelprogramma psql gebruiken om verbinding te maken met de Azure Database voor PostgreSQL-server.

1. Voer de volgende psql-opdracht uit om verbinding te maken met een Azure Database for PostgreSQL-database:
   ```
   psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
   ```

   Met de volgende opdracht maakt u bijvoorbeeld verbinding met de standaarddatabase **postgres** op uw PostgreSQL-server **mydemoserver.postgres.database.azure.com** met behulp van toegangsreferenties. Voer het `<server_admin_password>` in dat u koos toen u werd gevraagd om een wachtwoord.
  
   ```
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

   > [!TIP]
   > Als u liever een URL-pad gebruiken voor verbinding met Postgres, URL-codering het @-teken in de gebruikersnaam met `%40`. Bijvoorbeeld zou de verbindingsreeks voor de psql zijn,
   > ```
   > psql postgresql://myadmin%40mydemoserver@mydemoserver.postgres.database.azure.com:5432/postgres
   > ```

2. Wanneer u met de server bent verbonden, maakt u bij de prompt een lege database:
   ```sql
   CREATE DATABASE mypgsqldb;
   ```

3. Voer na de prompt de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database **mypgsqldb**:
   ```sql
   \c mypgsqldb
   ```

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database
U weet nu hoe u verbinding kunt maken met de Azure Database for PostgreSQL en dus is het tijd om enkele eenvoudige taken uit te voeren:

We gaan eerst een tabel maken en hierin enkele gegevens laden. We gaan een tabel maken die inventarisgegevens bijhoudt met behulp van deze SQL-code:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

U kunt de zojuist gemaakte tabel nu in de lijst met tabellen zien door het volgende te typen:
```sql
\dt
```

## <a name="load-data-into-the-tables"></a>Gegevens laden in de tabellen
De volgende stap bestaat uit het toevoegen van gegevens aan de tabel. Voer bij de opdrachtprompt de volgende query uit om enkele rijen met gegevens in te voegen.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

U hebt nu twee rijen met voorbeeldgegevens in de inventaristabel die u eerder hebt gemaakt.

## <a name="query-and-update-the-data-in-the-tables"></a>De gegevens in de tabellen opvragen en bijwerken
Voer de volgende query uit om gegevens op te halen uit de databasetabel met inventarisgegevens. 
```sql
SELECT * FROM inventory;
```

U kunt ook de gegevens in de tabel bijwerken.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

U kunt de bijgewerkte waarden zien wanneer u de gegevens ophaalt.
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-to-a-previous-point-in-time"></a>Gegevens herstellen naar een eerder tijdstip
Stel dat u deze tabel per ongeluk hebt verwijderd. Dit is iets wat u niet eenvoudig kunt herstellen. Met Azure Database for PostgreSQL kunt u gegevens herstellen van elk tijdstip waarop een back-up van uw server is gemaakt (welke tijdstippen dat zijn, is afhankelijk van de geconfigureerde bewaarperiode voor back-ups) en dit tijdstip herstellen naar een nieuwe server. U kunt deze nieuwe server dan gebruiken om de verwijderde gegevens te herstellen. Met de volgende stappen wordt de server **mydemoserver** hersteld naar een tijdstip voordat de inventaristabel is toegevoegd.

1. Klik op de pagina **Overzicht** van Azure Database for PostgreSQL voor uw server op de werkbalk op **Herstellen**. De**gelijknamige**pagina wordt geopend.

   ![Azure Portal: opties voor het herstellen van formulieren](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)

2. Vul in het formulier **Restore** de vereiste gegevens in:

   ![Azure Portal: opties voor het herstellen van formulieren](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)

   - **Herstelpunt**: selecteer een tijdstip voorafgaand aan het moment waarop de server is gewijzigd
   - **Doelserver**: geef de naam op van de nieuwe server waarnaar u wilt herstellen
   - **Locatie**: u kunt de regio niet selecteren, standaard is deze hetzelfde als die van de bronserver
   - **Prijscategorie**: u kunt deze waarde niet wijzigen als u een server gaat herstellen. Deze is hetzelfde als de bronserver. 
3. Klik op **OK** om [de server te herstellen naar een tijdstip](./howto-restore-server-portal.md) voordat de tabel is toegevoegd. Als u een server herstelt naar een eerder tijdstip, wordt er een nieuw exemplaar gemaakt van de oorspronkelijke server met de inhoud zoals die aanwezig was op het tijdstip dat u opgeeft. Dit tijdstip moet wel binnen de bewaarperiode liggen die wordt gehanteerd voor uw [prijscategorie](./concepts-pricing-tiers.md).

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u Azure Portal en andere hulpprogramma's kunt gebruiken voor het volgende:
> [!div class="checklist"]
> * Een Azure-database voor PostgreSQL-server maken
> * De serverfirewall configureren
> * Het hulpprogramma [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) gebruiken om een database te maken
> * Voorbeeldgegevens laden
> * Querygegevens
> * Gegevens bijwerken
> * Gegevens terugzetten

Ga naar deze zelfstudie als u wilt leren hoe u de Azure CLI gebruikt om soortgelijke taken uit te voeren: [Uw eerste Azure Database for PostgreSQL ontwerpen met Azure CLI](tutorial-design-database-using-azure-cli.md)
