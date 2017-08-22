---
title: 'Quick Start: een Azure-database voor MySQL-server maken - Azure Portal | Microsoft-documenten'
description: Dit artikel leidt u stapsgewijs door de Azure Portal zodat u snel, in ongeveer vijf minuten, een voorbeeld van een Azure-database voor MySQL-server kunt maken.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 829c7e73cbf22d866bbd6fd54edc7a954ad7174c
ms.contentlocale: nl-nl
ms.lasthandoff: 08/16/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Een Azure-database voor MySQL-server maken met behulp van Azure Portal
Azure Database voor MySQL is een beheerde service waarmee u MySQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. In deze Quick Start ontdekt u hoe u in ongeveer vijf minuten een Azure Database voor MySQL-server maakt in Azure Portal. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Open uw webbrowser en ga naar de [Microsoft Azure Portal](https://portal.azure.com/). Voer uw referenties in om u aan te melden bij de portal. De standaardweergave is uw service-dashboard.

## <a name="create-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Een Azure Database voor MySQL-server wordt gemaakt met een gedefinieerde set [reken- en opslagresources](./concepts-compute-unit-and-storage.md). De server wordt gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md).

Volg deze stappen voor het maken van een Azure Database voor MySQL-server:

1. Klik op de knop **Nieuw** (+) in de linkerbovenhoek van Azure Portal.

2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Azure Database voor MySQL** op de pagina **Databases**. U kunt ook **MySQL** in het zoekvak van de pagina Nieuw typen om de service te vinden.
![Azure Portal - nieuw - database - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Vul het formulier voor gegevens van nieuwe server in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld** 
    ---|---|---
    Servernaam | myserver4demo | Kies een unieke naam ter identificatie van de Azure Database voor MySQL-server. De domeinnaam *mysql.database.azure.com* wordt toegevoegd aan de naam van de server die toepassingen moeten gebruiken om verbinding mee te maken. De servernaam mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 63 tekens lang zijn.
    Abonnement | Uw abonnement | Het Azure-abonnement dat u wilt gebruiken voor uw server. Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource zal worden gefactureerd.
    Resourcegroep | myResourceGroup | U kunt een nieuwe resourcegroepnaam maken of een bestaande naam uit uw abonnement gebruiken.
    Aanmeldgegevens van serverbeheerder | myadmin | Maak uw eigen aanmeldingsaccount die moet worden gebruikt om verbinding te maken met de server. De aanmeldingsnaam voor de beheerder kan niet 'azure_superuser', 'admin', 'administrator', 'root', 'guest' of 'public' zijn.
    Wachtwoord | *Uw keuze* | Maak een nieuw wachtwoord voor het beheerdersaccount voor de server. Het moet 8 tot 128 tekens bevatten. Uw wachtwoord moet tekens bevatten uit drie van de volgende categorieën: Nederlandse hoofdletters, Nederlandse kleine letters, cijfers (0-9) en niet-alfanumerieke tekens (!, $, #, %, etc.).
    Wachtwoord bevestigen | *Uw keuze*| Bevestig het wachtwoord voor het beheerdersaccount.
    Locatie | *De regio het dichtst bij uw gebruikers*| Kies de locatie die zich het dichtst bij uw gebruikers of andere Azure-toepassingen bevindt.
    Versie | *Kies de nieuwste versie*| Kies de meest recente versie, tenzij u specifieke vereisten hebt.
    Prijscategorie | **Basic**, **50 rekeneenheden**, **50 GB** | Klik op **Prijscategorie** om de servicelaag en het prestatieniveau voor de nieuwe database op te geven. Kies de prijscategorie **Basic** op het tabblad bovenaan. Klik aan de linkerkant van de schuifregelaar **Rekeneenheden** totdat de waarde zo laag mogelijk is voor deze Quick Start. Klik vervolgens op **OK** om de geselecteerde prijscategorie op te slaan. Zie de volgende schermafbeelding.
    Vastmaken aan dashboard | Selecteren | Selecteer de optie **Vastmaken aan dashboard** om het eenvoudig bijhouden van uw server op de eerste dashboardpagina van uw Azure Portal in te schakelen.

    > [!IMPORTANT]
    > De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik.
    > 

    ![Azure Portal - maak MySQL aan door de vereiste invoer voor het formulier te verstrekken](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Klik op **Maken** om de server in te richten. Het inrichten duurt enkele minuten, maximaal 20 minuten.
   
5.  Klik op de werkbalk op **Meldingen** (pictogram van een klok) om het implementatieproces te bewaken.

## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

De service Azure Database voor MySQL maakt een firewall op serverniveau. De firewall voorkomt dat externe toepassingen en hulpprogramma's verbinding maken met de server of databases op de server, tenzij er een firewallregel wordt gemaakt om de firewall te openen voor specifieke IP-adressen. 

1.  Ga naar de server nadat de implementatie is voltooid. U kunt desgewenst naar de server zoeken. Klik bijvoorbeeld op **Alle resources** in het menu links en voer de naam van de server in (bijvoorbeeld *myserver4demo*) om te zoeken naar de zojuist gemaakte server. Klik op de servernaam in de zoekresultaten. De pagina **Overzicht** wordt geopend voor uw server en biedt opties voor verdere configuratie.

2. Selecteer **Verbindingsbeveiliging** op de serverpagina.

3.  Klik onder de kop **Firewallregels** in het lege tekstvak in de kolom **Regelnaam** om te beginnen met het maken van de firewallregel. 

    In deze Quick Start staan we alle IP-adressen toe op de server door in het tekstvak in elke kolom de volgende waarden in te vullen:

    Regelnaam | Start-IP | Eind-IP 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Klik op de bovenste werkbalk van de pagina **Verbindingsbeveiliging** op **Opslaan**. Wacht even totdat er een melding wordt weergegeven dat het bijwerken van de beveiliging van de verbinding is voltooid.

    > [!NOTE]
    > Verbindingen met Azure Database voor MySQL communiceren via poort 3306. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 3306 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u alleen verbinding maken met uw server als uw IT-afdeling poort 3306 openstelt.
    > 

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen
Om verbinding te maken met uw databaseserver, moet u de volledige servernaam en aanmeldingsreferenties van de beheerder weten. U hebt deze waarden mogelijk al eerder in dit artikel genoteerd. Als u dat niet hebt gedaan, kunt u de servernaam en aanmeldingsgegevens eenvoudig terugvinden op de pagina **Overzicht** of  **Eigenschappen** van de server in Azure Portal.

1. Open de pagina **Overzicht** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**. 
    Beweeg de muisaanwijzer de cursor over elk veld. Het kopieerpictogram wordt rechts van de tekst weergegeven. Klik op het kopieerpictogram indien nodig om de waarden te kopiëren.

    In dit voorbeeld is de servernaam *myserver4demo.mysql.database.azure.com* en de aanmeldgegevens van de serverbeheerder zijn *myadmin@myserver4demo*.

## <a name="connect-to-mysql-using-mysql-command-line-tool"></a>Verbinding maken met MySQL met behulp van het opdrachtregelprogramma mysql
Er zijn verschillende toepassingen die u kunt gebruiken om verbinding te maken met uw Azure Database voor MySQL-server. We gaan eerst het opdrachtregelprogramma [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) gebruiken om te laten zien hoe u verbinding maakt met de server.  U kunt een webbrowser en de Azure Cloud Shell gebruiken zoals hier wordt beschreven, zonder dat u extra software hoeft te installeren. Als het hulpprogramma mysql lokaal op uw computer is geïnstalleerd, kunt u dit ook gebruiken om verbinding te maken.

1. Open Azure Cloud Shell via het terminalpictogram ( >_ ) in de rechterbovenhoek van Azure Portal.

2. Azure Cloud Shell wordt geopend in uw browser zodat u bash shell-opdrachten kunt invoeren.

    ![Opdrachtprompt - voorbeeld van opdrachtregel mysql](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. Maak vanaf de Cloud Shell-prompt verbinding met uw Azure Database voor MySQL-server door de mysql-opdrachtregel te typen bij de groene prompt.

    De volgende indeling wordt gebruikt om verbinding te maken met een Azure Database voor MySQL-server via het hulpprogramma mysql:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Met de volgende opdracht maakt u bijvoorbeeld verbinding met onze voorbeeldserver:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    mysql-parameter |Voorgestelde waarde|Beschrijving
    ---|---|---
    --host | *servernaam* | Geef de waarde van de servernaam op die u hebt gebruikt toen u de Azure Database voor MySQL-server eerder hebt gemaakt. De server in ons voorbeeld is myserver4demo.mysql.database.azure.com. Gebruik de FQDN (Fully Qualified Domain Name) (\*.mysql.database.azure.com) zoals weergegeven in het voorbeeld. Volg de stappen in de vorige sectie om de verbindingsgegevens op te halen als u de servernaam bent vergeten. 
    --user | *aanmeldnaam van serverbeheerder* |Typ de gebruikersnaam van de serverbeheerder die u hebt opgegeven toen u de Azure Database voor MySQL-server eerder hebt gemaakt. Volg de stappen in de vorige sectie om de verbindingsgegevens op te halen als u de gebruikersnaam bent vergeten.  De indeling is *username@servername*.
    --password | *wacht totdat u hierom wordt gevraagd* | U wordt gevraagd om uw wachtwoord op te geven nadat u de opdracht hebt ingevoerd. Voer het wachtwoord in dat u hebt opgegeven tijdens het maken van de server.  Houd er rekening mee dat de ingevoerde wachtwoordtekens niet worden weergegeven bij de bash-prompt terwijl u de tekens typt. Druk op enter nadat u de tekens hebt getypt om te verifiëren en verbinding te maken.

   Als de verbinding tot stand is gebracht, geeft het hulpprogramma mysql de prompt `mysql>` weer voor het typen van opdrachten. 

    Voorbeeld van mysql-uitvoer:
    ```bash
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Als de firewall niet is geconfigureerd voor het toestaan van het IP-adres van de Azure Cloud Shell, wordt het volgende foutbericht weergegeven:
    >
    > ERROR 2003 (28000): Client with IP address 123.456.789.0 is not allowed to access the server.
    >
    > U kunt deze fout oplossen door ervoor te zorgen dat de serverconfiguratie overeenkomt met de stappen in de sectie *Een serverfirewallregel configureren* van het artikel.

4. Geef de status van de server weer om de functionaliteit van de verbinding te controleren. Typ `status` op de prompt mysql> zodra de server is verbonden.
    ```sql
    status
    ```

   > [!TIP]
   > Zie [hoofdstuk 4.5.1 in de Engelstalige naslaghandleiding van MySQL 5.7](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) voor aanvullende opdrachten.

5.  Maak een lege database door bij de prompt mysql> de volgende opdracht te typen:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    Het uitvoeren van de opdracht kan even duren. 

    Op een Azure Database voor MySQL-server kunt u een of meerdere databases maken. U kunt kiezen voor het maken van één database per server om gebruik te maken van alle resources of voor meerdere databases om de resources te delen. Er is geen limiet aan het aantal databases dat kan worden gemaakt, maar meerdere databases delen wel dezelfde serverresources. 

6. Vraag de databases op bij de prompt mysql> door de volgende opdracht te typen:

    ```sql
    SHOW DATABASES;
    ```

7.  Typ `\q` en druk op Enter om het hulpprogramma mysql af te sluiten. U kunt de Azure Cloud Shell sluiten als u klaar bent.

U hebt nu verbinding gemaakt met de Azure Database voor MySQL-server en een lege gebruikersdatabase gemaakt. Ga door met de volgende sectie om een vergelijkbare oefening te herhalen om verbinding te maken met dezelfde server, maar nu met een veelgebruikt hulpprogramma, namelijk MySQL Workbench.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Maak verbinding met de server met het MySQL Workbench GUI-hulpprogramma
Verbinding maken met een Azure MySQL-server met behulp van het GUI-hulpprogramma MySQL Workbench:

1.  Start de toepassing MySQL Workbench op uw clientcomputer. U kunt MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) downloaden en installeren.

2.  In het dialoogvenster **Nieuwe verbinding instellen** voert u de volgende gegevens in op het tabblad **Parameters**:

    ![nieuwe verbinding instellen](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld** |
    |---|---|---|
    |   Verbindingsnaam | Demo-verbinding | Geef een label op voor deze verbinding. |
    | Verbindingsmethode | Standard (TCP/IP) | Standard (TCP/IP) is voldoende. |
    | Hostnaam | *servernaam* | Geef de waarde van de servernaam op die u hebt gebruikt toen u de Azure Database voor MySQL-server eerder hebt gemaakt. De server in ons voorbeeld is myserver4demo.mysql.database.azure.com. Gebruik de FQDN (Fully Qualified Domain Name) (\*.mysql.database.azure.com) zoals weergegeven in het voorbeeld. Volg de stappen in de vorige sectie om de verbindingsgegevens op te halen als u de servernaam bent vergeten.  |
    | Poort | 3306 | Gebruik altijd poort 3306 bij het verbinden met Azure Database voor MySQL. |
    | Gebruikersnaam |  *aanmeldnaam van serverbeheerder* | Typ de gebruikersnaam van de serverbeheerder die u hebt opgegeven toen u de Azure Database voor MySQL-server eerder hebt gemaakt. De gebruikersnaam in ons voorbeeld is myadmin@myserver4demo. Volg de stappen in de vorige sectie om de verbindingsgegevens op te halen als u de gebruikersnaam bent vergeten. De indeling is *username@servername*.
    | Wachtwoord | Uw wachtwoord | Klik op de knop Opslaan in de kluis... om het wachtwoord op te slaan. |

    Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd. Klik op OK om de verbinding op te slaan. 

    > [!NOTE]
    > SSL wordt standaard afgedwongen op uw server. Om verbinding te kunnen maken, is hiervoor aanvullende configuratie nodig. Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.  Als u SSL wilt uitschakelen voor deze Quick Start, gaat u naar Azure Portal en klikt u op de pagina Verbindingsbeveiliging om de wisselknop SSL-verbinding afdwingen op Uit te zetten.

## <a name="clean-up-resources"></a>Resources opschonen
De resources die u hebt gemaakt in de Quick Start kunt u opschonen door de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) te verwijderen, met daarin alle resources in de resourcegroep, of door alleen de serverresource te verwijderen als u de andere resources intact wilt houden.

> [!TIP]
> Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.
>

De hele resourcegroep verwijderen, met inbegrip van de nieuwe server:
1.  Zoek de resourcegroep in Azure Portal. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de naam van uw resourcegroep, zoals **myresourcegroup** in ons voorbeeld.
2.  Klik op de pagina van de resourcegroep op **Verwijderen**. Typ vervolgens de naam van de resourcegroep, zoals **myresourcegroup** uit het voorbeeld, in het tekstvak om het verwijderen te bevestigen en klik vervolgens op **Verwijderen**.

Alleen de zojuist gemaakte server verwijderen:
1.  Als de server niet is geopend, zoekt u de server in Azure Portal. Klik in het menu aan de linkerkant in Azure Portal op **Alle resources** en zoek naar de server die u hebt gemaakt.
2.  Ga op de pagina **Overzicht** naar het bovenste deelvenster en klik op de knop **Verwijderen**.
![Azure Database voor MySQL - server verwijderen](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Controleer de naam van de server die u gaat verwijderen en kijk welke databases eveneens worden verwijderd. Typ de naam van uw server in het tekstvak, zoals **myserver4demo** uit ons voorbeeld, en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw eerste Azure-database voor MySQL-database ontwerpen](./tutorial-design-database-using-portal.md)


