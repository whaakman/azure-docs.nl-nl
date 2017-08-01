---
title: 'Quick Start: een Azure-database voor MySQL-server maken - Azure Portal | Microsoft-documenten'
description: Dit artikel leidt u stapsgewijs door de Azure Portal zodat u snel, in ongeveer vijf minuten, een voorbeeld van een Azure-database voor MySQL-server kunt maken.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: hero-article
ms.date: 06/14/2017
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: dba50b369fb87d5f6d5118038c75392bd719cc10
ms.contentlocale: nl-nl
ms.lasthandoff: 06/28/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Een Azure-database voor MySQL-server maken met behulp van Azure Portal
Dit artikel leidt u stapsgewijs door de Azure-portal zodat u in ongeveer vijf minuten een Azure-database voor MySQL-server kunt maken. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Open uw webbrowser en ga naar de [Microsoft Azure Portal](https://portal.azure.com/). Voer uw referenties in om u aan te melden bij de portal. De standaardweergave is uw service-dashboard.

## <a name="create-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
1. Klik op de knop **Nieuw** in de linkerbovenhoek van de Azure-portal.

2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Azure Database voor MySQL** op de pagina **Databases**. U kunt ook **MySQL** in het zoekvak van de pagina Nieuw typen om de service te vinden.
![Azure Portal - nieuw - database - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Vul het formulier voor gegevens van nieuwe server in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:

| **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld** |
|---|---|---|
| *Servernaam* | myserver4demo  | Servernaam moet globaal uniek zijn. |
| *Abonnement* | mysubscription | Selecteer uw abonnement in de vervolgkeuzelijst. |
| *Resourcegroep* | myResourceGroup | Maak een nieuwe resourcegroep of selecteer een bestaande. |
| *Aanmeldgegevens van serverbeheerder* | myadmin | Geef de naam van een beheerdersaccount op in de MySQL-engine. |
| *Wachtwoord* |  | Stel een wachtwoord voor het beheerdersaccount in. |
| *Wachtwoord bevestigen* |  | Bevestig het wachtwoord voor het beheerdersaccount. |
| *Locatie* |  | Selecteer een beschikbare regio. |
| *Versie* | 5.7 | Kies de nieuwste versie. |
| *Prijscategorie* | Basic, 50 rekeneenheden, 50 opslag (GB)  | Kies **Prijscategorie**, **Rekeneenheden**, **Opslag (GB)** en klik vervolgens op **OK**. |
| *Vastmaken aan dashboard* | Selecteren | Het is raadzaam dit selectievakje in te schakelen zodat u de server later gemakkelijk kunt terugvinden |

   Klik op **Prijscategorie** om de prijscategorie en het prestatieniveau voor de nieuwe database op te geven. Selecteer voor deze Quick Start de laag Basic, 50 rekeneenheden en 50 GB bijbehorende opslag. Klik vervolgens op **OK** om de prijscategorie op te slaan.
   
   ![Azure Portal - maak MySQL aan door de vereiste invoer voor het formulier te verstrekken](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

   Klik vervolgens op **Maken**. Na een minuut of twee wordt een nieuwe Azure-database voor de MySQL-server uitgevoerd in de cloud. Klik op de knop **Meldingen** (belpictogram) op de werkbalk om het implementatieproces te volgen.

## <a name="configure-the-firewall"></a>De firewall configureren
Voordat u voor het eerst verbinding maakt met Azure Database voor MySQL, moet u de firewall configureren en het IP-adres (of adresbereik) van het openbare netwerk van de client aan de whitelist toevoegen.

1. Nadat de implementatie is voltooid, klikt u op **Alle resources** in het menu links en voert u de naam **myserver4demo** in om te zoeken naar de zojuist gemaakte server. Klik op de servernaam in de zoekresultaten. De pagina Overzicht wordt geopend voor uw server en biedt opties voor verdere configuratie.

2. Op de serverblade selecteert u **Verbindingsbeveiliging**.

3. Klik op **Mijn IP toevoegen** om het IP-adres van uw lokale computer toe te voegen of configureer een reeks IP-adressen. Vergeet niet op **Opslaan** te klikken nadat u de regels hebt gemaakt.
  ![Azure Portal - firewallregel toevoegen en opslaan](./media/quickstart-create-mysql-server-database-using-azure-portal/5_firewall-settings.png)

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de FQDN (fully qualified domain name) op voor uw Azure MySQL-server in de Azure Portal. U gebruikt de FQDN om verbinding met uw server te maken met behulp van het opdrachtregelprogramma **mysql.exe**.

1.  In [Azure Portal](https://portal.azure.com/) klikt u op **Alle resources** in het menu links en vervolgens op uw Azure-database voor MySQL-server.

2.  Klik op **Eigenschappen**. Noteer de **SERVERNAAM** en **AANMELDGEGEVENS VAN DE SERVERBEHEERDER**.
In dit voorbeeld is de servernaam *myserver4demo.mysql.database.azure.com* en de aanmeldgegevens van de serverbeheerder zijn *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysqlexe-command-line-tool"></a>Maak verbinding met de server met behulp van het opdrachtregelprogramma mysqlexe
Gebruik het [opdrachtregelprogramma mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) om een verbinding tot stand te brengen met uw Azure-database voor MySQL-server. U kunt het opdrachtregelprogramma mysql uitvoeren in de browser met behulp van de Azure Cloud Shell of het opdrachtregelprogramma starten vanaf uw eigen computer met behulp van de lokaal geïnstalleerde mysql-hulpprogramma's. Als u de Azure Cloud Shell wilt starten, klikt u op de knop `Try It` in een codeblok in dit artikel of gaat u naar de [Azure-portal](https://portal.azure.com) en klikt u op het pictogram `>_` in de bovenste werkbalk rechts. 

1. Typ de opdracht voor verbinding:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Geef de status van de server weer om de functionaliteit van de verbinding te controleren. Typ `status` op de prompt mysql> zodra de server is verbonden.
```sql
status
```

   ![Opdrachtprompt - voorbeeld van opdrachtregel mysql](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

   > [!TIP]
   > Zie [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) (MySQL 5.7-handleiding, hoofdstuk 4.5.1) voor aanvullende opdrachten.

3. Maak een lege database door de opdracht `CREATE DATABASE` te typen op de prompt mysql>.

   ```sql
   CREATE DATABASE quickstartdb;
   ```

   Een Azure-database voor MySQL-server kan een of meerdere databases bevatten. U kunt kiezen voor het maken van één database per server om gebruik te maken van alle resources of voor meerdere databases om de resources te delen. Er is geen limiet aan het aantal databases dat kan worden gemaakt, maar meerdere databases delen wel dezelfde serverresources.  

4. Geef een lijst met databases weer door de opdracht `SHOW DATABASES` te typen bij de prompt mysql>.

   ```sql
   SHOW DATABASES;
   ```

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Maak verbinding met de server met het MySQL Workbench GUI-hulpprogramma
1.  Start de toepassing MySQL Workbench op uw clientcomputer. U kunt MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) downloaden en installeren.

2.  In het dialoogvenster **Nieuwe verbinding instellen** voert u de volgende gegevens in op het tabblad **Parameters**:

   ![nieuwe verbinding instellen](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

| **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld** |
|---|---|---|
|   *Verbindingsnaam* | Demo-verbinding| Geef een label op voor deze verbinding. |
| *Verbindingsmethode* | Standard (TCP/IP) | Standard (TCP/IP) is voldoende. |
| *Hostnaam* | myserver4demo.mysql.database.azure.com | Gebruik de volledig gekwalificeerde servernaam voor uw server. |
| *Poort* | 3306 | Gebruik de standaardpoort 3306. |
| *Gebruikersnaam* | myadmin@myserver4demo  | Gebruik de aanmeldgegevens van de serverbeheerder u eerder hebt genoteerd met een @-teken en de naam van de server. |
| *Wachtwoord* | Uw wachtwoord | Klik op de knop Opslaan in de kluis... om het wachtwoord op te slaan. |

Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd. Klik op OK om de verbinding op te slaan. 

> [!NOTE]
> SSL wordt standaard afgedwongen op uw server. Om verbinding te kunnen maken, is hiervoor aanvullende configuratie nodig. Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie.  Als u SSL wilt uitschakelen voor deze zelfstudie, gaat u naar de Azure-portal en klikt u op de pagina Verbindingsbeveiliging om de wisselknop SSL-verbinding afdwingen uit te schakelen.

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder alle resources die u in deze Quick Start hebt gemaakt door de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) te verwijderen.

> [!TIP]
> Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.

1.  Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myresourcegroup**.
2.  Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myresourcegroup** in het tekstvak en klik vervolgens op Verwijderen.

Als u de zojuist gemaakte server wilt verwijderen:
1.  In het menu links in Azure Portal klikt u op PostgreSQL-servers en zoekt u naar de server die u zojuist hebt gemaakt
2.  Op de pagina Overzicht klikt u op de knop Verwijderen in het deelvenster bovenaan ![Azure Database voor MySQL - Server verwijderen](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Controleer de naam van de server die u wilt verwijderen en bekijk welke databases onder de server vallen. Geef **myserver4demo** op in het tekstvak en klik vervolgens op Verwijderen.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw eerste Azure-database voor MySQL-database ontwerpen](./tutorial-design-database-using-portal.md)


