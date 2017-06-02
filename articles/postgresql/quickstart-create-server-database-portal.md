---
title: 'Azure Portal: een Azure Database voor PostgreSQL-server maken | Microsoft Docs'
description: Quick Start-handleiding voor het beheren en maken van een Azure Database voor PostgreSQL-server via de Azure Portal-gebruikersinterface.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.custom: quick start create, mvc
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: c67ada15c11b81021ff5e6f6e5edc5cb530ece98
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Een Azure Database voor PostgreSQL-server maken in Azure Portal

Azure Database voor PostgreSQL is een beheerde service waarmee u PostgreSQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. In deze Quick Start ontdekt u hoe u een Azure Database voor PostgreSQL-server maakt in Azure Portal.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Een Azure Database voor PostgreSQL-server maken

Een Azure Database voor PostgreSQL-server wordt gemaakt met een gedefinieerde set [reken- en opslagresources](./concepts-compute-unit-and-storage.md). De server wordt gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md).

Volg deze stappen voor het maken van een Azure Database voor PostgreSQL-server:
1.    Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2.    Selecteer **Databases** op de pagina **Nieuw** en selecteer **Azure Database voor PostgreSQL** op de pagina **Databases**.
 ![Azure Database voor PostgreSQL - De database maken](./media/quickstart-create-database-portal/1-create-database.png)

3.    Vul het formulier voor gegevens van nieuwe server in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:
    - Servernaam: **mypgserver-20170401** (de servernaam verwijst naar een DNS-naam en moet daarom wereldwijd uniek zijn) 
    - Abonnement: als u meerdere abonnementen hebt, kiest u het abonnement waarin de resource bestaat of waarvoor wordt gefactureerd.
    - Resourcegroep: **myresourcegroup**
    - De aanmeldgegevens van de serverbeheerder (gebruikersnaam en wachtwoord)
    - Locatie
    - PostgreSQL-versie

  > [!IMPORTANT]
  > De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik.

4.    Klik op **Prijscategorie** om de servicelaag en het prestatieniveau voor de nieuwe database op te geven. Selecteer voor deze Quick Start de laag **Basic**, **50 rekeneenheden** en **50 GB** bijbehorende opslag.
 ![Azure Database voor PostgreSQL - De servicelaag kiezen](./media/quickstart-create-database-portal/2-service-tier.png)
5.    Klik op **OK**.
6.    Klik op **Maken** om de server in te richten. De inrichting duurt een paar minuten.

  > [!TIP]
  > Schakel de optie **Vastmaken aan dashboard** in om uw implementaties eenvoudig te kunnen volgen.

7.    Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.
 ![Azure Database voor PostgreSQL - Meldingen bekijken](./media/quickstart-create-database-portal/3-notifications.png)
   
  De database **postgres** wordt gemaakt op uw server. De database [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) is een standaarddatabase die kan worden gebruikt door gebruikers, hulpprogramma's en toepassingen van derden. 

## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

De Azure Database voor PostgreSQL-service maakt een firewall op serverniveau. De firewall voorkomt dat externe toepassingen en hulpprogramma's verbinding maken met de server of databases op de server, tenzij er een firewallregel wordt gemaakt om de firewall te openen voor specifieke IP-adressen. 

1.    Nadat de implementatie is voltooid, klikt u op **Alle resources** in het menu links en voert u de naam **mypgserver-20170401** in om te zoeken naar de zojuist gemaakte server. Klik op de servernaam in de zoekresultaten. De pagina **Overzicht** wordt geopend voor uw server en biedt opties voor verdere configuratie.
 
 ![Azure Database voor PostgreSQL - Zoeken naar de server ](./media/quickstart-create-database-portal/4-locate.png)

2.    Op de serverblade selecteert u **Verbindingsbeveiliging**. 
3.    Klik in het tekstvak onder **Regelnaam** en voeg een nieuwe firewallregel toe om het IP-bereik voor connectiviteit op de goedgekeurde lijst te plaatsen. In deze Quick Start gaat u alle IP-adressen toestaan door het volgende in te voeren: **Regelnaam = AllowAllIps**, **Begin-IP-adres = 0.0.0.0** en **Eind-IP-adres = 255.255.255.255**. Klik daarna op **Opslaan**. U kunt een firewallregel voor een IP-bereik zodat u vanaf uw netwerk verbinding kunt maken.

 ![Azure Database voor PostgreSQL - Een firewallregel maken](./media/quickstart-create-database-portal/5-firewall-2.png)

4.    Klik op **Opslaan** en klik vervolgens op **X** om de pagina **Verbindingsbeveiliging** te sluiten.

  > [!NOTE]
  > De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u alleen verbinding maken met uw Azure SQL Database-server als uw IT-afdeling poort 5432 openstelt.
  >

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Wanneer u de Azure Database voor PostgreSQL-server maakt, wordt de standaarddatabase **postgres** ook gemaakt. Als u verbinding met uw databaseserver wilt maken, moet u hostgegevens en toegangsreferenties opgeven.

1. In het menu links in Azure Portal klikt u op **Alle resources** en zoekt u naar de server die u zojuist hebt gemaakt: **mypgserver-20170401**.

  ![Azure Database voor PostgreSQL - Zoeken naar de server ](./media/quickstart-create-database-portal/4-locate.png)

2. Klik op de servernaam **mypgserver-20170401**.
3. Selecteer de pagina **Overzicht** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**.

 ![Azure Database voor PostgreSQL - Aanmeldgegevens van de serverbeheerder](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Verbinding maken met een PostgreSQL-database met behulp van psql in Cloud Shell

U gaat nu het opdrachtregelprogramma psql gebruiken om verbinding te maken met de Azure Database voor PostgreSQL-server. 
1. Open Azure Cloud Shell via het terminalpictogram in het navigatiedeelvenster bovenaan.

   ![Azure Database voor PostgreSQL - Azure Cloud Shell-terminalpictogram](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Azure Cloud Shell wordt geopend in uw browser zodat u bash-opdrachten kunt invoeren.

   ![Azure-Database voor PostgreSQL - Azure Shell-bash-prompt](./media/quickstart-create-database-portal/8-bash.png)

3. In het Cloud Shell-prompt maakt u verbinding met uw Azure Database voor PostgreSQL-server met de psql-opdrachten. De volgende indeling wordt gebruikt om verbinding te maken met een Azure Database voor PostgreSQL-server via het [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html)-hulpprogramma:
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   Met de volgende opdracht maakt u bijvoorbeeld verbinding met de standaarddatabase **postgres** op uw PostgreSQL-server **mypgserver-20170401.postgres.database.azure.com** met behulp van toegangsreferenties. Voer het wachtwoord van de serverbeheerder in wanneer dat wordt gevraagd.

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```
4.  Wanneer u met de server bent verbonden, maakt u in het prompt een lege database.
```bash
CREATE DATABASE mypgsqldb;
```

5.  In het prompt voert u de volgende opdracht uit om verbinding te maken met de zojuist gemaakte database **mypgsqldb**.
```bash
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Verbinding maken met een PostgreSQL-database met behulp van pgAdmin

Verbinding maken met een Azure PostgreSQL-server met behulp van het GUI-hulpprogramma _pgAdmin_
1.    Start de toepassing _pgAdmin_ op uw clientcomputer. U kunt _pgAdmin_ installeren via http://www.pgadmin.org/.
2.    Selecteer **Nieuwe Server toevoegen** via het menu **Snelkoppelingen**.
3.    In het dialoogvenster **Maken - Server** gaat u naar het tabblad **Algemeen** en voert u een unieke beschrijvende naam in voor de server, zoals **Azure PostgreSQL-server**.
![pgAdmin-hulpprogramma - Maken - Server](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.    In het dialoogvenster **Maken - Server** gaat u naar het tabblad **Verbinding**, gebruikt u de opgegeven instellingen en klikt u op **Opslaan**.
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
5.    Klik op **Opslaan**.
6.    In het linkerdeelvenster Browser vouwt u de optie **Servergroepen** uit. Kies uw **Azure PostgreSQL-server**.
7.  Kies de **server** waarmee u verbonden bent en selecteer daaronder **Databases**. 
8.    Klik met de rechtermuisknop op **Databases** om een database te maken.
9.    Kies de databasenaam **mypgsqldb** en voer de eigenaar in als serverbeheerder (**mylogin**).
10. Klik op **Opslaan** om een lege database te maken.
11. In **Browser** vouwt u **Server** uit. Vouw de server uit die u hebt gemaakt en bekijk daaronder de database **mypgsqldb**.
 ![pgAdmin - Maken - Database](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Resources opschonen
Verwijder alle resources die u in deze Quick Start hebt gemaakt door de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) te verwijderen.

> [!TIP]
> Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.

1.    Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myresourcegroup**.
2.    Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myresourcegroup** in het tekstvak en klik vervolgens op Verwijderen.

Als u de zojuist gemaakte server wilt verwijderen:
1.    In het menu links in Azure Portal klikt u op PostgreSQL-servers en zoekt u naar de server die u zojuist hebt gemaakt
2.    Op de pagina Overzicht klikt u op Verwijderen in het deelvenster bovenaan ![Azure Database voor PostgreSQL - Server verwijderen](./media/quickstart-create-database-portal/12-delete.png)
3.    Controleer de naam van de server die u wilt verwijderen en bekijk welke databases onder de server vallen. Voer **mypgserver-20170401** in in het tekstvak en klik vervolgens op Verwijderen.

## <a name="next-steps"></a>Volgende stappen
- Migreer uw database met behulp van [Exporteren en importeren](./howto-migrate-using-export-and-import.md) of [Dump maken en terugzetten](./howto-migrate-using-dump-and-restore.md).
- Zie [PostgreSQL-server maken - CLI](./quickstart-create-server-database-azure-cli.md) voor informatie over het maken van een Azure Database voor PostgreSQL-server met Azure CLI.
- Zie [Over de Azure Database voor PostgreSQL-service](./overview.md) voor een technisch overzicht.
