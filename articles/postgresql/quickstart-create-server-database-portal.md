---
title: 'Azure-portal: Maak een Azure-Database voor PostgreSQL server | Microsoft Docs'
description: Quick Start guide te maken en beheren van een Azure-Database voor PostgreSQL-server met behulp van de Azure portal-gebruikersinterface.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/03/2017
ms.openlocfilehash: b78009a4b2683bb7ee881808ddbbc792d66dea6c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-in-the-azure-portal"></a>Maken van een Azure-Database voor PostgreSQL-server in de Azure portal

Azure PostgreSQL-Database is een beheerde service waarmee u kunt uitvoeren, beheren en schalen van maximaal beschikbare PostgreSQL-databases in de cloud. Deze snelstartgids wordt beschreven hoe u een Azure-Database voor PostgreSQL-server maken in ongeveer 5 minuten met behulp van de Azure-portal.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Open uw webbrowser en Ga naar de [portal](https://portal.azure.com/). Voer uw referenties in om u aan te melden bij de portal. De standaardweergave is uw service-dashboard.

## <a name="create-an-azure-database-for-postgresql-server"></a>Een Azure-database voor PostgreSQL-server maken

Een Azure Database voor PostgreSQL-server wordt gemaakt met een gedefinieerde set [reken- en opslagresources](./concepts-compute-unit-and-storage.md). De server wordt gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md).

Voor het maken van een Azure-Database voor PostgreSQL-server, moet u de volgende stappen uitvoeren:
1. Selecteer de knop **Nieuw** (+) in de linkerbovenhoek van de portal.

2. Selecteer **Databases** > **Azure PostgreSQL-Database**.

    ![De optie 'Azure-Database voor PostgreSQL'](./media/quickstart-create-database-portal/1-create-database.png)

3. Vul het formulier voor gegevens van de nieuwe server in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Servernaam |*mypgserver-20170401*|Een unieke naam waarmee uw Azure-Database voor PostgreSQL-server. De domeinnaam *postgres.database.azure.com* wordt toegevoegd aan de server die u opgeeft. De server kan alleen kleine letters, cijfers en het koppelteken (-) bevatten. Er moet ten minste 3 en maximaal 63 tekens bevatten.
    Abonnement|Uw abonnement|De Azure-abonnement dat u wilt gebruiken voor uw server. Als u meerdere abonnementen hebt, kiest u het abonnement waarin u wordt gefactureerd voor de resource.
    Resourcegroep|*myresourcegroup*| Een nieuwe Resourcegroepnaam of een bestaande uit uw abonnement.
    Aanmeldgegevens van serverbeheerder |*mylogin*| Uw eigen aanmeldingsaccount moet worden gebruikt wanneer u verbinding met de server maken. De aanmeldingsnaam voor de beheerder kan niet worden **azure_superuser,** **azure_pg_admin,** **admin,** **beheerder** **basis,** **Gast,** of **openbare.** Deze mag niet beginnen met **pg_**.
    Wachtwoord |Uw keuze | Een nieuw wachtwoord voor het beheerdersaccount voor de server. Het wachtwoord moet tussen 8 en 128 tekens lang zijn. Uw wachtwoord moet tekens bevatten uit drie van de volgende categorieën: Nederlandse hoofdletters letters, Nederlandse kleine letters, cijfers (0-9) en niet-alfanumerieke tekens (!, $, #, %, etc.).
    Locatie|De regio die het dichtst bij uw gebruikers| De locatie die zich het dichtst bij uw gebruikers.
    PostgreSQL-versie|De meest recente versie| De nieuwste versie, tenzij er specifieke vereisten.
    Prijscategorie | **Basic**, **50 rekeneenheden**, **50 GB** | De servicelaag en het prestatieniveau voor uw database wijzigen. Selecteer **prijscategorie**. Selecteer vervolgens de **Basic** tabblad. Selecteer vervolgens het linkereinde van het **eenheden Compute** schuifregelaar naar de waarde voor zo min mogelijk beschikbaar voor deze snelstartgids. Selecteer de prijscategorie laag als selectie wilt opslaan, **OK**. Zie de volgende schermafbeelding voor meer informatie. 
    Vastmaken aan dashboard | Selecteren | Hiermee kunt eenvoudig bijhouden van uw server op de front-dashboardpagina van uw portal.

    > [!IMPORTANT]
    > De aanmeldgegevens van serverbeheerder en het wachtwoord dat u hier opgeeft, hoeven aan te melden bij de server en de databases verderop in deze snelstartgids. Onthoud of noteer deze informatie voor later gebruik.

    ![Het deelvenster 'Prijscategorie'](./media/quickstart-create-database-portal/2-service-tier.png)

4. Selecteer **Maken** om de server in te richten. Dit kan maximaal 20 minuten duren.

5. Selecteer op de werkbalk de **meldingen** symbool voor het bewaken van het implementatieproces.

    ![Het deelvenster 'Meldingen'](./media/quickstart-create-database-portal/3-notifications.png)
   
  Standaard een **postgres** database wordt gemaakt onder uw server. De [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is een standaarddatabase die bedoeld voor gebruik door gebruikers, hulpprogramma's en toepassingen van derden. 

## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

Azure PostgreSQL-Database wordt gemaakt van een firewall op serverniveau. Dit voorkomt dat externe toepassingen en hulpprogramma's verbinding te maken met de server en alle databases op de server, tenzij u een regel voor het openen van de firewall voor specifieke IP-adressen maakt. 

1. Ga naar de server nadat de implementatie is voltooid. U kunt desgewenst naar de server zoeken. Selecteer bijvoorbeeld in het menu aan de linkerkant **alle resources**. Typ de naam van uw server, zoals in het voorbeeld **mypgserver 20170401**, om te zoeken naar de nieuwe virtuele server. Selecteer de servernaam van uw in de lijst met zoekresultaten. De pagina **Overzicht** wordt geopend voor uw server en biedt opties voor verdere configuratie.
 
    ![Zoekactie voor namen van server](./media/quickstart-create-database-portal/4-locate.png)

2. Selecteer **Verbindingsbeveiliging** op de serverpagina.

    ![De instelling ' verbinding '](./media/quickstart-create-database-portal/5-firewall-2.png)

3. Onder de **Firewall-regels** kop in de **regelnaam** kolom, selecteer het lege tekstvak om te beginnen met het maken van de firewallregel. 

    Voor deze snelstartgids laat u alle IP-adressen gaan we in de server. Vul in het tekstvak in elke kolom met de volgende waarden:

    Regelnaam | Start-IP | Eind-IP 
    ---|---|---
    AllowAllIps | 0.0.0.0 | 255.255.255.255

4. Selecteer **Opslaan** op de bovenste werkbalk van de pagina **Verbindingsbeveiliging**. Wacht totdat de melding wordt weergegeven waarin staat dat de beveiligingsupdate voor de verbinding is voltooid, voordat u doorgaat.

    > [!NOTE]
    > Verbindingen met uw Azure Database voor PostgreSQL-server communiceren via poort 5432. Als u probeert verbinding maken vanaf een bedrijfsnetwerk, wordt het uitgaande verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. Zo ja, u geen verbinding maken met uw server tenzij uw IT-afdeling poort 5432 wordt geopend.
    >

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Wanneer u uw Azure-Database voor PostgreSQL-server maakt, met een standaard-database de naam **postgres** wordt gemaakt. Voor verbinding met uw database-server, moet u uw volledige server-naam en admin-aanmeldingsreferenties. U hebt deze waarden mogelijk al eerder in dit artikel genoteerd. Als dat niet gemakkelijk informatie vindt u de server de naam en meld u aan op de server **overzicht** pagina in de portal.

Open de pagina **Overzicht** van de server. Noteer de **servernaam** en de **aanmeldingsnaam van Server-beheerder**. Beweeg de muisaanwijzer de cursor over elk veld en het symbool kopie wordt weergegeven aan de rechterkant van de tekst. Selecteer het symbool kopiëren indien nodig om de waarden te kopiëren.

 ![De pagina 'Overview'](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-the-postgresql-database-by-using-psql-in-cloud-shell"></a>Verbinding maken met de PostgreSQL-Database via psql in de Cloud-Shell

Er zijn een aantal toepassingen die kunt gebruiken om verbinding te maken met uw Azure Database voor PostgreSQL-server. We gaan eerst het opdrachtregelprogramma psql gebruiken om te laten zien hoe u verbinding maakt met de server. Kunt u een webbrowser en Azure Cloud Shell zoals hier wordt beschreven zonder te hoeven geen extra software te installeren. Als het hulpprogramma psql lokaal op uw computer is geïnstalleerd, kunt u dit ook gebruiken om verbinding te maken.

1. Selecteer de terminal symbool Cloud-Shell openen in het bovenste navigatiedeelvenster.

   ![Azure Cloud Shell terminal symbool](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Cloud-Shell wordt geopend in uw browser, u Bash-shell-opdrachten typt.

   ![Cloud Bash-Shell-prompt](./media/quickstart-create-database-portal/8-bash.png)

3. Verbinding met een database in uw Azure-Database voor PostgreSQL-server door de psql opdrachtregel te typen bij de Cloud Shell-prompt.

    Verbinding maken met een Azure-Database voor PostgreSQL-server met de [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) hulpprogramma, gebruik de volgende notatie:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    De volgende opdracht maakt bijvoorbeeld verbinding met een voorbeeldserver:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql parameter |Voorgestelde waarde|Beschrijving
    ---|---|---
    --host | Servernaam | De waarde van de server de naam die u hebt gebruikt toen u de Azure-Database voor PostgreSQL server eerder hebt gemaakt. De voorbeeld-server weergegeven **mypgserver 20170401.postgres.database.azure.com.** Gebruik de volledig gekwalificeerde domeinnaam (**\*. postgres.database.azure.com**) zoals weergegeven in het voorbeeld. Als u de servernaam niet meer weet, volgt u de stappen in de vorige sectie om de verbindingsgegevens op te halen. 
    --poort | 5432 | De poort moet worden gebruikt wanneer u verbinding met de Azure-Database voor PostgreSQL-server maken. 
    --gebruikersnaam | Aanmeldingsnaam van Server-beheerder |De server-aanmelding beheerdersgebruikersnaam die u hebt opgegeven toen u de Azure-Database voor PostgreSQL server eerder hebt gemaakt. Als u niet uw gebruikersnaam niet meer weet, volg de stappen in de vorige sectie ophalen van gegevens voor de verbinding. De indeling is *username@servername*.
    --dbnaam | *postgres* | De standaardwaarde systeem gegenereerde databasenaam die voor de eerste verbinding is gemaakt. Later kunt maken u uw eigen database.

    Nadat u de opdracht psql met uw eigen parameterwaarden uitvoert, wordt gevraagd of u een server van de Administrator-wachtwoord invoeren. Dit wachtwoord is dezelfde als die u hebt opgegeven tijdens het maken van de server. 

    psql parameter |Voorgestelde waarde|Beschrijving
    ---|---|---
    wachtwoord | Uw Administrator-wachtwoord | De getypte wachtwoord tekens zijn niet op de bash-prompt weergegeven. Nadat u alle tekens typt, selecteert u de **Enter** sleutel om te verifiëren en verbinding maken.

    Nadat u verbinding maakt, geeft het hulpprogramma psql postgres de opdrachtprompt u sql-opdrachten typt. In de uitvoer van de eerste verbinding is mogelijk een waarschuwing worden weergegeven omdat de psql in de Cloud-Shell mogelijk een verschillende versie dan de Azure-Database voor PostgreSQL server-versie. 
    
    Voorbeeld van psql-uitvoer:
    ```bash
    psql (9.5.7, server 9.6.2)
    WARNING: psql major version 9.5, server major version 9.6.
        Some psql features might not work.
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
    Type "help" for help.
   
    postgres=> 
    ```

    > [!TIP]
    > Als de firewall is niet geconfigureerd voor het toestaan van het IP-adres van Cloud-Shell, wordt het volgende foutbericht weergegeven:
    > 
    > ' psql: onherstelbare fout: geen pg_hba.conf-vermelding voor de host '138.91.195.82' gebruiker 'mylogin' database 'postgres', SSL op een onherstelbare fout: SSL-verbinding is vereist. SSL-opties opgeven en probeer het opnieuw.
    > 
    > Los de fout, Controleer of de serverconfiguratie komt overeen met de stappen in de sectie 'Een firewallregel op serverniveau configureren' van dit artikel.

4. Maak een lege database door bij de prompt de volgende opdracht te typen:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    De opdracht kan een paar minuten duren. 

5. Voer de volgende opdracht verbindingen overschakelen naar de zojuist gemaakte database bij de prompt **mypgsqldb**:
    ```bash
    \c mypgsqldb
    ```

6. Type `\q`, en selecteer vervolgens de **Enter** sleutel om psql af te sluiten. Nadat u klaar bent, kunt u Cloud-Shell sluiten.

Nu u verbonden bent met de Azure-Database voor PostgreSQL-server en u een lege database hebt gemaakt. De volgende sectie om verbinding met een andere algemene hulpprogramma pgAdmin te blijven.

## <a name="connect-to-the-postgresql-database-by-using-pgadmin"></a>Verbinding maken met de PostgreSQL-Database via pgAdmin

Verbinding maken met de Azure-PostgreSQL-server met behulp van de GUI-hulpprogramma pgAdmin:
1. Open de toepassing pgAdmin op de clientcomputer. U kunt installeren pgAdmin van de [pgAdmin website](http://www.pgadmin.org/).

2. Op de dashboardpagina onder de **snelkoppelingen** sectie, selecteer de **nieuwe Server toevoegen** symbool.

3. In de **maken - Server** in het dialoogvenster op de **algemene** tabblad, voer een unieke, beschrijvende naam voor de server, zoals **Azure PostgreSQL Server**.

    ![Het tabblad 'Algemeen'](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)

4. In de **maken - Server** in het dialoogvenster op de **verbinding** tabblad en selecteer vervolgens instellingen gebruikt als de opgegeven **opslaan**.

   ![Het tabblad 'Verbinding'](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    Parameter pgAdmin |Voorgestelde waarde|Beschrijving
    ---|---|---
    Host Name/Address | Servernaam | De waarde van de server de naam die u hebt gebruikt toen u de Azure-Database voor PostgreSQL server eerder hebt gemaakt. Onze server voorbeeld is **mypgserver 20170401.postgres.database.azure.com.** Gebruik de volledig gekwalificeerde domeinnaam (**\*. postgres.database.azure.com**) zoals weergegeven in het voorbeeld. Als u de servernaam niet meer weet, volgt u de stappen in de vorige sectie om de verbindingsgegevens op te halen. 
    Poort | 5432 | De poort moet worden gebruikt wanneer u verbinding met de Azure-Database voor PostgreSQL-server maken. 
    Onderhoud database | *postgres* | De naam van het systeem gegenereerde database standaard.
    Gebruikersnaam | Aanmeldingsnaam van Server-beheerder | De server-aanmelding beheerdersgebruikersnaam die u hebt opgegeven toen u de Azure-Database voor PostgreSQL server eerder hebt gemaakt. Als u de gebruikersnaam niet meer weet, volgt u de stappen in de vorige sectie om de verbindingsgegevens op te halen. De indeling is *username@servername*.
    Wachtwoord | Uw Administrator-wachtwoord | Het wachtwoord dat u hebt gekozen toen u de server eerder in deze snelstartgids hebt gemaakt.
    Rol | Leeg laten | Er is niet nodig om een rolnaam op dit moment. Laat het veld leeg.
    SSL-modus | Vereist | Standaard worden alle Azure PostgreSQL-servers gemaakt met SSL afdwingen ingeschakeld. Als u wilt uitschakelen SSL afdwingen, Zie [SSL afdwingen](./concepts-ssl-connection-security.md).
    
5. Selecteer **Opslaan**.

6. In de **Browser** in het linkerdeelvenster Vouw de **Servers** knooppunt. Selecteer uw server, bijvoorbeeld **Azure PostgreSQL Server**. Klik op verbinding kunnen maken.

7. Vouw het knooppunt Servers uit en vouw vervolgens **Databases** eronder uit. De lijst moet opnemen met uw bestaande *postgres* database en een nieuwe gebruiker-database, zoals **mypgsqldb**, die is gemaakt in de vorige sectie. U ziet u meerdere databases per server kunt maken met Azure-Database voor PostgreSQL.

8. Met de rechtermuisknop op **Databases**, kies de **maken** menu en selecteer vervolgens **Database**.

9. Typ de naam van een database van uw keuze in de **Database** veld zoals **mypgsqldb**, zoals wordt weergegeven in het voorbeeld.

10. Selecteer de **eigenaar** voor de database uit de lijst. Kies uw server admin aanmeldingsnaam, zoals in het voorbeeld **mylogin**.

11. Selecteer **opslaan** een nieuwe lege database maken.

12. In de **Browser** deelvenster ziet u de database die u hebt gemaakt in de lijst met databases onder de servernaam van uw.

    ![Het deelvenster "Browser"](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Resources opschonen
U kunt de resources die u hebt gemaakt in de Quick Start op twee manieren opschonen. U kunt de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) verwijderen, met alle resources uit de resourcegroep. Als u behouden van de andere resources wilt, verwijdert u alleen de bron voor één server.

> [!TIP]
> Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om ook deze Quick Starts te volgen, moet u geen resources opschonen die u in deze Quick Start hebt gemaakt. Als u niet van plan bent om door te gaan, volg deze stappen voor het verwijderen van de resources die zijn gemaakt door deze Quick Start in de portal.

Verwijderen van de hele resourcegroep, met inbegrip van de nieuwe server:
1. De resourcegroep niet vinden in de portal. Selecteer in het menu aan de linkerkant **resourcegroepen**. Selecteer vervolgens de naam van de resourcegroep, zoals in het voorbeeld **myresourcegroup**.

2. Selecteer **Verwijderen** op de pagina van de resourcegroep. Typ de naam van de resourcegroep, zoals in het voorbeeld **myresourcegroup**, in het tekstvak verwijderen te bevestigen. Selecteer **Verwijderen**.

Alleen de zojuist gemaakte server verwijderen:
1. Als u nog niet hebt geopend, moet u uw server vinden in de portal. Selecteer in het menu aan de linkerkant **alle resources**. Zoek vervolgens naar de server die u hebt gemaakt.

2. Selecteer **Verwijderen** op de pagina **Overzicht**.

    ![De knop 'Verwijderen'](./media/quickstart-create-database-portal/12-delete.png)

3. Bevestig de naam van de server die u wilt verwijderen en weergeven van de databases in deze die worden beïnvloed. Typ de naam van uw server in het tekstvak, zoals in het voorbeeld **mypgserver 20170401**. Selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./howto-migrate-using-export-and-import.md)
