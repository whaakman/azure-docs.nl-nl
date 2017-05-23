---
title: 'Quick Start: een Azure-database voor MySQL-server maken - Azure Portal | Microsoft-documenten'
description: Dit artikel leidt u stapsgewijs door de Azure Portal zodat u snel, in ongeveer vijf minuten, een voorbeeld van een Azure-database voor MySQL-server kunt maken.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 25bfd2c6c25ddb8747dec58fdc68f904f81127fa
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Een Azure-database voor MySQL-server maken met behulp van Azure Portal

Dit artikel leidt u stapsgewijs door de Azure Portal zodat u in ongeveer vijf minuten een voorbeeld van een Azure-database voor MySQL-server kunt maken. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Open uw webbrowser en ga naar de [Microsoft Azure Portal](https://portal.azure.com/). Voer uw referenties in om u aan te melden bij de portal. De standaardweergave is uw service-dashboard.

![Azure Portal - aanmelden en dashboard](./media/quickstart-create-mysql-server-database-using-azure-portal/1_portal-login.png)

## <a name="create-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken

1. Ga naar **Databases** > **MySQL**. Als u Azure Database voor MySQL Server niet kunt vinden in de categorie **Databases**, klikt u op **Alles zien** om alle beschikbare database-services weer te geven. U kunt ook **MySQL** in het zoekvak typen om de service snel te vinden.
![Azure Portal - nieuw - database - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

2. Klik op het pictogram **MySQL** en klik vervolgens op **Maken**.
In ons voorbeeld vult u de volgende informatie in op de pagina Azure-database voor MySQL:

| **Formulierveld** | **Beschrijving van veld** |
|----------------|-----------------------|
| *Servernaam* | mysqlserver4demo (servernaam is globaal uniek) |
| *Abonnement* | MySQLaaS (selecteer in vervolgkeuzelijst) |
| *Resourcegroep* | myresource (maak een resourcegroep of gebruik een bestaande resourcegroep) |
| *Aanmeldgegevens van serverbeheerder* | myadmin (stel accountnaam van beheerder in) |
| *Wachtwoord* | stel wachtwoord voor beheerdersaccount in |
| *Wachtwoord bevestigen* | bevestig wachtwoord voor beheerdersaccount |
| *Locatie* | Noord-Europa (kies tussen **Noord-Europa** en **West-V.S.**) |
| *Versie* | 5.6 (kies versie van MySQL server) |
| *Prestaties configureren* | Basic (kies **Prestatielaag**, **Rekeneenheden**, **Opslag** en klik vervolgens op **OK**) |

![Azure Portal - maak MySQL aan door de vereiste invoer voor het formulier te verstrekken](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

Na enkele minuten is uw Azure-database voor MySQL-server ingericht en gebruiksklaar. U kunt op de knop **Meldingen** (belpictogram) op de werkbalk klikken om het implementatieproces te volgen.

> [!TIP]
> Aangeraden wordt de Azure-services in dezelfde regio te plaatsen en de dichtstbijzijnde locatie te kiezen. Daarnaast kunt de optie **Vastmaken aan dashboard** selecteren om uw implementaties gemakkelijk te kunnen volgen.

## <a name="configure-the-firewall"></a>De firewall configureren
Voordat u vanaf uw client voor het eerst verbinding maakt met Azure Database voor MySQL, moet u de firewall configureren en het IP-adres van het openbare netwerk van de client aan de whitelist toevoegen.

1. Klik op uw zojuist gemaakte server en klik vervolgens op **Instellingen**.
  ![Azure Portal - MySQL - knop Instellingen](./media/quickstart-create-mysql-server-database-using-azure-portal/4_server-settings.png)

2. Klik in de sectie **ALGEMEEN** op **Firewallinstellingen**. U kunt op **Mijn IP toevoegen** klikken om het IP-adres van uw lokale computer toe te voegen of u kunt een reeks IP-adressen configureren. Vergeet niet op **Opslaan** te klikken nadat u de regels hebt gemaakt.
  ![Azure Portal - firewallregel toevoegen en opslaan](./media/quickstart-create-mysql-server-database-using-azure-portal/5_firewall-settings.png)

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de FQDN (fully qualified domain name) op voor uw Azure MySQL-server in de Azure Portal. U gebruikt de FQDN om verbinding met uw server te maken met behulp van het opdrachtregelprogramma **mysql.exe**.

1.    In [Azure Portal](https://portal.azure.com/) klikt u op **Alle resources** in het menu links en vervolgens op uw Azure-database voor MySQL-server.

2.    Klik op **Eigenschappen**. Noteer de **SERVERNAAM** en **AANMELDGEGEVENS VAN DE SERVERBEHEERDER**.
In dit voorbeeld is de servernaam *mysql4doc.database.windows.net* en de aanmeldgegevens van de serverbeheerder zijn *mysqladmin@mysql4doc*.

## <a name="connect-to-the-server-using-mysqlexe-command-line-tool"></a>Maak verbinding met de server met behulp van het opdrachtregelprogramma mysqlexe
U kunt meerdere databases maken binnen een MySQL-server. Er is geen limiet aan het aantal databases dat kan worden gemaakt, maar meerdere databases delen wel dezelfde serverresources.  Maak verbinding met uw server met het opdrachtregelprogramma **mysql.exe**. Open de **Azure Cloud Shell** in de portal en voer het volgende in:

1. Maak verbinding met de server met behulp van het opdrachtregelprogramma **mysql**:
```dos
 mysql -h mysqlserver4demo.database.windows.net -u myadmin@mysqlserver4demo -p
```

2. Bekijk de status van de server:
```dos
 mysql> status
```
  ![Opdrachtprompt - mysql opdrachtregelvoorbeeld](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

> [!TIP]
> Zie [MySQL 5.6 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) (MySQL 5.6-handleiding, hoofdstuk 4.5.1) voor aanvullende opdrachten.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Maak verbinding met de server met het MySQL Workbench GUI-hulpprogramma
1.    Start de toepassing MySQL Workbench op uw clientcomputer. U kunt MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) downloaden en installeren.

2.    In het dialoogvenster **Nieuwe verbinding instellen** voert u de volgende gegevens in op het tabblad **Parameters**:

| **Parameters** | **Beschrijving** |
|----------------|-----------------|
|    *Verbindingsnaam* | geef een naam op voor deze verbinding (dit kan van alles zijn) |
| *Verbindingsmethode* | kies Standard (TCP/IP) |
| *Hostnaam* | mycliserver.database.windows.net (SERVERNAAM die u eerder hebt genoteerd) |
| *Poort* | 3306 |
| *Gebruikersnaam* | myadmin@mycliserver (AANMELDGEGEVENS VAN SERVERBEHEERDER die u eerder hebt genoteerd) |
| *Wachtwoord* | u kunt het wachtwoord van het beheerdersaccount in de kluis opslaan |

![nieuwe verbinding instellen](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

3.    Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd.

4.    U kunt nu op de verbinding klikken die zojuist is gemaakt om verbinding te maken met de server.

> SSL wordt standaard afgedwongen op uw server. Om verbinding te kunnen maken, is hiervoor aanvullende configuratie nodig . Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.  Als SSL wilt uitschakelen voor deze Quick Start, kunt u naar Verbindingsbeveiliging in de portal gaan om het afdwingen van SSL uit te schakelen.

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet voor een andere Quick Start/zelfstudie nodig hebt, kunt u ze verwijderen door het volgende te doen:

1. Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myresource**. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myresource** in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw eerste Azure-database voor MySQL-database ontwerpen](./tutorial-design-database-using-portal.md)


