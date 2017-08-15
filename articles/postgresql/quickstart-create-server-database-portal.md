---
title: 'Azure Portal: een Azure Database voor PostgreSQL-server maken | Microsoft Docs'
description: Quick Start-handleiding voor het beheren en maken van een Azure Database voor PostgreSQL-server via de Azure Portal-gebruikersinterface.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/04/2017
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: e89058a500aeb542ae4c7dc6bb4a9b9ae54db7f2
ms.contentlocale: nl-nl
ms.lasthandoff: 08/09/2017

---

# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Een Azure Database voor PostgreSQL-server maken in Azure Portal

Azure Database voor PostgreSQL is een beheerde service waarmee u PostgreSQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. In deze Quick Start ontdekt u hoe u een Azure Database voor PostgreSQL-server maakt in Azure Portal.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Een Azure Database voor PostgreSQL-server maken

Een Azure Database voor PostgreSQL-server wordt gemaakt met een gedefinieerde set [reken- en opslagresources](./concepts-compute-unit-and-storage.md). De server wordt gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md).

Volg deze stappen voor het maken van een Azure Database voor PostgreSQL-server:
1.  Klik op de knop **Nieuw** (+) in de linkerbovenhoek van Azure Portal.
2.  Selecteer **Databases** op de pagina **Nieuw** en selecteer **Azure Database voor PostgreSQL** op de pagina **Databases**.
 ![Azure Database voor PostgreSQL - De database maken](./media/quickstart-create-database-portal/1-create-database.png)

3.  Vul het formulier voor gegevens van nieuwe server in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Servernaam |*mypgserver-20170401*|Kies een unieke naam ter identificatie van uw Azure-database voor PostgreSQL-server. De domeinnaam *postgres.database.azure.com* wordt toegevoegd aan de naam van de server die u opgeeft voor de toepassingen waarmee verbinding moet worden gemaakt. De servernaam mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 63 tekens lang zijn.
    Abonnement|*Uw abonnement*|Het Azure-abonnement dat u wilt gebruiken voor uw server. Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource zal worden gefactureerd.
    Resourcegroep|*myresourcegroup*| U kunt een nieuwe resourcegroepnaam maken of een bestaande naam uit uw abonnement gebruiken.
    Aanmeldgegevens van serverbeheerder |*mylogin*| Maak uw eigen aanmeldingsaccount die moet worden gebruikt wanneer verbinding wordt gemaakt met de server. De aanmeldingsnaam voor de beheerder kan niet 'azure_superuser', 'azure_pg_admin', 'admin', ' administrator', 'root', 'guest' of 'public' zijn en mag niet beginnen met 'pg_'.
    Wachtwoord |*Uw keuze* | Maak een nieuw wachtwoord voor het beheerdersaccount voor de server. Het moet 8 tot 128 tekens bevatten. Uw wachtwoord moet tekens bevatten uit drie van de volgende categorieën: Nederlandse hoofdletters, Nederlandse kleine letters, cijfers (0-9) en niet-alfanumerieke tekens (!, $, #, %, etc.).
    Locatie|*De regio het dichtst bij uw gebruikers*| Kies de locatie die het dichtst bij uw gebruikers is.
    PostgreSQL-versie|*Kies de nieuwste versie*| Kies de meest recente versie, tenzij u specifieke vereisten hebt.
    Prijscategorie | **Basic**, **50 rekeneenheden**, **50 GB** | Klik op **Prijscategorie** om de servicelaag en het prestatieniveau voor de nieuwe database op te geven. Kies de prijscategorie Basic op het tabblad bovenaan. Klik aan de linkerkant van de schuifregelaar Rekeneenheden zodat de waarde zo laag mogelijk is voor deze snelstartgids. Klik vervolgens op **OK** om de geselecteerde prijscategorie op te slaan. Zie de volgende schermafbeelding.
    | Vastmaken aan dashboard | Selecteren | Selecteer de optie **Vastmaken aan dashboard** om het eenvoudig bijhouden van uw server op de eerste dashboardpagina van uw Azure Portal in te schakelen.

  > [!IMPORTANT]
  > De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik.

    ![Azure Database voor PostgreSQL - De prijscategorie kiezen](./media/quickstart-create-database-portal/2-service-tier.png)

4.  Klik op **Maken** om de server in te richten. Het inrichten duurt enkele minuten, maximaal 20 minuten.

5.  Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.
 ![Azure Database voor PostgreSQL - Meldingen bekijken](./media/quickstart-create-database-portal/3-notifications.png)
   
  De database **postgres** wordt gemaakt op uw server. De database [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) is een standaarddatabase die kan worden gebruikt door gebruikers, hulpprogramma's en toepassingen van derden. 

## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

De Azure Database voor PostgreSQL-service maakt een firewall op serverniveau. De firewall voorkomt dat externe toepassingen en hulpprogramma's verbinding maken met de server of databases op de server, tenzij er een firewallregel wordt gemaakt om de firewall te openen voor specifieke IP-adressen. 

1.  Ga naar de server nadat de implementatie is voltooid. Indien nodig u kunt zoeken. Klik bijvoorbeeld op **Alle resources** in het menu links en voer de naam van de server (bijvoorbeeld mypgserver-20170401) in om te zoeken naar de zojuist gemaakte server. Klik op de servernaam in de zoekresultaten. De pagina Overzicht wordt geopend voor uw server en biedt opties voor verdere configuratie.
 
    ![Azure Database voor PostgreSQL - Zoeken naar de server ](./media/quickstart-create-database-portal/4-locate.png)

2.  Op de serverpagina selecteert u **Verbindingsbeveiliging**. 
    ![Azure Database voor PostgreSQL - Een firewallregel maken](./media/quickstart-create-database-portal/5-firewall-2.png)

3.  Klik onder de kop **Firewallregels** in het lege tekstvak in de kolom **Regelnaam** om te beginnen met het maken van de firewallregel. 

    In deze snelstartgids staan we alle IP-adressen toe in de server door de volgende waarden in te vullen in het tekstvak in elke kolom:

    Regelnaam | Start-IP | Eind-IP 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Klik in de bovenste werkbalk van de pagina Verbindingsbeveiliging op **Opslaan**. Klik op **X** om de pagina Verbindingsbeveiliging te sluiten.

    > [!NOTE]
    > De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u alleen verbinding maken met uw Azure SQL Database-server als uw IT-afdeling poort 5432 openstelt.
  >

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Wanneer u de Azure Database voor PostgreSQL-server maakt, wordt de standaarddatabase met de naam **postgres** gemaakt. Voor verbinding met uw databaseserver, moet u de volledige servernaam en aanmeldingsreferenties van de beheerder intrekken. U hebt deze waarden mogelijk al eerder in dit snelstartartikel genoteerd. Als u niet hebt gedaan, kunt u de servernaam en aanmeldingsgegevens gemakkelijk terugvinden op de overzichtspagina voor servers in de Azure portal.

1. Open de pagina **Overzicht** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**.
    Beweeg de muisaanwijzer de cursor over elk veld. Het kopieerpictogram wordt rechts van de tekst weergegeven. Klik op het kopieerpictogram indien nodig om de waarden te kopiëren.

 ![Azure Database voor PostgreSQL - Aanmeldgegevens van de serverbeheerder](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Verbinding maken met een PostgreSQL-database met behulp van psql in Cloud Shell

Er zijn een aantal toepassingen die kunt gebruiken om verbinding te maken met uw Azure Database voor PostgreSQL-server. We gaan eerst het opdrachtregelprogramma psql gebruiken om te laten zien hoe u verbinding maakt met de server.  U kunt een webbrowser en de Azure Cloud Shell gebruiken zoals hier wordt beschreven, zonder dat u extra software hoeft te installeren. Als het hulpprogramma psql lokaal op uw computer is geïnstalleerd, kunt u dit ook gebruiken om verbinding te maken.

1. Open Azure Cloud Shell via het terminalpictogram in het navigatiedeelvenster bovenaan.

   ![Azure Database voor PostgreSQL - Azure Cloud Shell-terminalpictogram](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Azure Cloud Shell wordt geopend in uw browser zodat u bash shell-opdrachten kunt invoeren.

   ![Azure-Database voor PostgreSQL - Azure Shell-bash-prompt](./media/quickstart-create-database-portal/8-bash.png)

3. Maak bij de Cloud Shell-prompt verbinding met een database in uw Azure Database voor PostgreSQL-server door de psql-opdrachtregel te typen bij de groene prompt.

    De volgende indeling wordt gebruikt om verbinding te maken met een Azure Database voor PostgreSQL-server via het [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html)-hulpprogramma:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    De volgende opdracht maakt bijvoorbeeld verbinding met een voorbeeldserver:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql parameter |Voorgestelde waarde|Beschrijving
    ---|---|---
    --host | *servernaam* | Geef de waarde van de servernaam op die u hebt gebruikt toen u de Azure Database voor PostgreSQL hebt gemaakt. Onze weergegeven voorbeeldserver mypgserver-20170401.postgres.database.azure.com. Gebruik de FQDN (Fully Qualified Domain Name) (\*. postgres.database.azure.com) zoals weergegeven in het voorbeeld. Volg de vorige sectie om de verbindingsgegevens op te halen als u de servernaam bent vergeten. 
    --poort | **5432** | Gebruik altijd poort 5432 bij het verbinden met Azure Database voor PostgreSQL. 
    --gebruikersnaam | *aanmeldnaam van serverbeheerder* |Typ de gebruikersnaam van de serverbeheerder die u hebt opgegeven toen u de Azure Database voor PostgreSQL hebt gemaakt. Volg de vorige sectie om de verbindingsgegevens op te halen als u de gebruikersnaam bent vergeten.  De indeling is *username@servername*.
    --dbnaam | **postgres** | Gebruik de standaardnaam voor de database *postgres* die door het systeem is gegenereerd voor de eerste verbinding. Later kunt u uw eigen database maken.

    Na het uitvoeren van de psql-opdracht met uw eigen parameterwaarden, wordt u gevraagd het beheerderswachtwoord van de server in te voeren. Dit is het wachtwoord dat u hebt opgegeven tijdens het maken van de server. 

    psql parameter |Voorgestelde waarde|Beschrijving
    ---|---|---
    wachtwoord | *uw beheerderswachtwoord* | Houd er rekening mee dat de getypte wachtwoordtekens niet worden weergegeven op de bash-prompt. Druk op enter nadat u de tekens hebt getypt om te verifiëren en verbinding te maken.

4.  Wanneer u met de server bent verbonden, maakt u in het prompt een lege database door de volgende opdracht te typen:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```

5.  In het prompt voert u de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database **mypgsqldb**.
    ```bash
    \c mypgsqldb
    ```

6.  Typ \q en druk op ENTER om psql af te sluiten. U kunt de Azure Cloud Shell sluiten.

U hebt nu verbinding met de Azure Database voor PostgreSQL en hebt een lege gebruikersdatabase gemaakt. Ga verder met de volgende sectie om verbinding te maken via een ander algemeen hulpprogramma: pgAdmin.

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Verbinding maken met een PostgreSQL-database met behulp van pgAdmin

Verbinding maken met een Azure PostgreSQL-server met behulp van het GUI-hulpprogramma _pgAdmin_
1.  Start de toepassing _pgAdmin_ op uw clientcomputer. U kunt _pgAdmin_ installeren via http://www.pgadmin.org/.
2.  Selecteer **Nieuwe Server toevoegen** via het menu **Snelkoppelingen**.
3.  In het dialoogvenster **Maken - Server** gaat u naar het tabblad **Algemeen** en voert u een unieke beschrijvende naam in voor de server, zoals **Azure PostgreSQL-server**.
![pgAdmin-hulpprogramma - Maken - Server](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.  In het dialoogvenster **Maken - Server** gaat u naar het tabblad **Verbinding**, gebruikt u de opgegeven instellingen en klikt u op **Opslaan**.
   ![pgAdmin - Maken - Server](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)
    - **Hostnaam/-adres**: mypgserver-20170401.postgres.database.azure.com 
        - Volledig gekwalificeerde servernaam.
    - **Poort:** 5432
        - Het poortnummer dat door deze databaseserver wordt gebruikt, is 5432.
    - **Onderhoudsdatabase**: postgres 
        - De door het systeem gegenereerde standaarddatabasenaam.
    - **Gebruikersnaam:** mylogin@mypgserver-20170401 
        - De gebruikersnaam van de serverbeheerder (user@mypgserver) die eerder in deze Quick Start is verkregen.
    - **Wachtwoord**: het wachtwoord dat u hebt gekozen toen u eerder in deze Quick Start de server maakte.
    - **SSL-modus**: vereisen
        - Standaard worden alle Azure PostgreSQL-servers gemaakt waarbij SSL geforceerd wordt ingeschakeld. Zie [SSL afdwingen](./concepts-ssl-connection-security.md) als u het geforceerd inschakelen van SSL wilt uitschakelen.
5.  Klik op **Opslaan**.
6.  In het linkerdeelvenster Browser vouwt u de optie **Servergroepen** uit. Kies uw **Azure PostgreSQL-server**.
7.  Kies de **server** waarmee u verbonden bent en selecteer daaronder **Databases**. 
8.  Klik met de rechtermuisknop op **Databases** om een database te maken.
9.  Kies de databasenaam **mypgsqldb** en voer de eigenaar in als serverbeheerder (**mylogin**).
10. Klik op **Opslaan** om een lege database te maken.
11. In **Browser** vouwt u **Server** uit. Vouw de server uit die u hebt gemaakt en bekijk daaronder de database **mypgsqldb**.
 ![pgAdmin - Maken - Database](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Resources opschonen
Verwijder alle resources die u in deze Quick Start hebt gemaakt door de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) te verwijderen.

> [!TIP]
> Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.

1.  Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myresourcegroup**.
2.  Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myresourcegroup** in het tekstvak en klik vervolgens op Verwijderen.

Als u de zojuist gemaakte server wilt verwijderen:
1.  In het menu links in Azure Portal klikt u op PostgreSQL-servers en zoekt u naar de server die u zojuist hebt gemaakt
2.  Op de pagina Overzicht klikt u op Verwijderen in het deelvenster bovenaan ![Azure Database voor PostgreSQL - Server verwijderen](./media/quickstart-create-database-portal/12-delete.png)
3.  Controleer de naam van de server die u wilt verwijderen en bekijk welke databases onder de server vallen. Voer **mypgserver-20170401** in in het tekstvak en klik vervolgens op Verwijderen.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./howto-migrate-using-export-and-import.md)

