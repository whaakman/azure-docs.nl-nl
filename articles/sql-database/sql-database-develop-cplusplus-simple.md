---
title: Verbinding maken met SQL Database met C en C++ | Microsoft Docs
description: Gebruik de voorbeeldcode in dit snel starten voor het bouwen van een moderne een toepassing met C++ en back-ups door een krachtige relationele database in de cloud met Azure SQL Database.
services: sql-database
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: 
ms.assetid: 07d9e0b1-3234-4f17-a252-a7559160a9db
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 03/06/2017
ms.author: edmacauley
ms.openlocfilehash: 7c78f90c14c1915c760720948d6d7ae99ceb6f1d
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="connect-to-sql-database-using-c-and-c"></a>Verbinding maken met SQL Database met C en C++
Dit bericht is gericht op C en C++ ontwikkelaars die verbinding maken met Azure SQL-database. Het is onderverdeeld in secties, zodat u kunt springen naar de sectie waarmee uw interesse beste wordt vastgelegd. 

## <a name="prerequisites-for-the-cc-tutorial"></a>Vereisten voor de C/C++-zelfstudie
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen abonnement hebt, kunt u zich registreren voor een [gratis Azure-proefversie](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/). U kunt de onderdelen van de taal C++ voor het bouwen en uitvoeren van dit voorbeeld moet installeren.
* [Visual Studio Linux ontwikkeling](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Als u op Linux ontwikkelt, moet u ook de uitbreiding van de Visual Studio Linux installeren. 

## <a id="AzureSQL"></a>Azure SQL Database en SQL Server op virtuele machines
Azure SQL is gebaseerd op Microsoft SQL Server en is zodanig ontworpen dat een hoge beschikbaarheid, zodat en schaalbare service. Er zijn veel voordelen voor het gebruik van SQL Azure via uw eigen database met on-premises. Met SQL Azure kunt u hoeft te installeren, instellen, beheren of beheren van uw database, maar alleen de inhoud en de structuur van uw database. Typische dingen die we bang met databases zoals zijn alle fouttolerantie en redundantie ingebouwd in. 

Azure heeft momenteel twee opties voor het hosten van SQL server-werkbelastingen: Azure SQL database, -database als een service en de SQL server op virtuele Machines (VM). We krijgt geen informatie over de verschillen tussen deze twee, behalve dat Azure SQL database het beste nieuwe cloud-gebaseerde toepassingen om te profiteren van de kostenbesparingen en optimalisatie van de prestaties die cloudservices bieden. Als u plan te bent migreren of het uitbreiden van uw on-premises toepassingen naar de cloud, mogelijk SQL server op virtuele machine van Azure werken beter voor u. Om de zaken eenvoudige voor dit artikel, gaat u een Azure SQL database te maken. 

## <a id="ODBC"></a>Data access-technologieën: ODBC en OLE DB
Verbinding maken met Azure SQL DB gaat niet anders en er zijn momenteel twee manieren om verbinding te databases: ODBC (Open Database connectivity) en OLE DB (database-Object koppelen en insluiten). Microsoft heeft in de afgelopen jaren uitgelijnd met [ODBC voor toegang tot systeemeigen relationele gegevens](https://blogs.msdn.microsoft.com/sqlnativeclient/2011/08/29/microsoft-is-aligning-with-odbc-for-native-relational-data-access/). ODBC is relatief eenvoudig en ook veel sneller dan OLE DB. De enige belemmering hier is dat een oude C-stijl-API maakt gebruik van ODBC. 

## <a id="Create"></a>Stap 1: Uw Azure SQL-Database maken
Zie de [pagina aan de slag](sql-database-get-started-portal.md) voor informatie over het maken van een voorbeelddatabase.  U kunt ook kunt u dit [korte video van twee minuten](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) voor het maken van een Azure SQL database met de Azure portal.

## <a id="ConnectionString"></a>Stap 2: De verbindingsreeks ophalen
Nadat u uw Azure SQL database is ingericht, moet u bij het uitvoeren van de volgende stappen uit om te bepalen van de verbindingsgegevens en voeg uw client-IP voor firewalltoegang. 

In [Azure-portal](https://portal.azure.com/), gaat u naar uw Azure SQL database ODBC-verbindingsreeks met behulp van de **databaseverbindingsreeksen tonen** vermeld als een deel van de overzichtssectie voor uw database: 

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

Kopieer de inhoud van de **ODBC (omvat Node.js) [SQL-verificatie]** tekenreeks. We gebruiken deze tekenreeks later vanaf onze C++ ODBC-opdrachtregel-interpreter verbinding maken. Deze tekenreeks bevat informatie zoals het stuurprogramma, server en andere databaseparameters. 

## <a id="Firewall"></a>Stap 3: Uw IP-toevoegen aan de firewall
Ga naar de firewallsectie voor uw Database-server en voeg uw [client-IP-aan de hand van deze stappen firewall](sql-database-configure-firewall-settings.md) om ervoor te zorgen dat er een geslaagde verbinding kan worden gemaakt: 

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

Op dit moment uw Azure SQL-database hebt geconfigureerd en klaar bent om te verbinden vanuit uw code C++. 

## <a id="Windows"></a>Stap 4: Verbinding maken vanuit een Windows-C/C++-toepassing
U kunt eenvoudig verbinding maken met uw [Azure SQL DB met ODBC op Windows met behulp van dit voorbeeld](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) die wordt gemaakt met Visual Studio. Het voorbeeld wordt een ODBC-opdrachtregel-interpreter waarmee u verbinding maken met onze SQL Azure DB die wordt geïmplementeerd. Dit voorbeeld wordt een Database source name (DSN) bestand als een opdrachtregelargument ofwel de uitgebreide verbindingsreeks die we eerder hebt gekopieerd uit de Azure portal. Online zetten van het eigenschappenblad voor dit project en plak de verbindingsreeks als een argument van de opdracht als volgt te werk: 

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Zorg ervoor dat u de details van de juiste verificatie voor uw database opgeven als een deel van deze database-verbindingsreeks. 

Start de toepassing te bouwen. U ziet het volgende venster valideren van de verbinding is geslaagd. U kunt zelfs enkele eenvoudige SQL-opdrachten zoals uitvoeren **tabel maken** voor het valideren van de verbinding met de database:

![SQL-opdrachten](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

U kunt ook een DSN-bestand met de wizard die wordt gestart wanneer er zijn geen argumenten zijn opgegeven maken. Het is raadzaam dat u deze optie ook proberen. U kunt deze DSN-bestand gebruiken voor automatisering en beveiligen van uw verificatie-instellingen: 

![DSN-bestand maken](./media/sql-database-develop-cplusplus-simple/datasource.png)

Gefeliciteerd. U hebt nu verbinding gemaakt met C++ en ODBC in Windows Azure-SQL. U kunt blijven lezen om Doe hetzelfde voor Linux-platform. 

## <a id="Linux"></a>Stap 5: Verbinding maakt vanaf een Linux-C/C++-toepassing
Als u het nieuws nog niet hebt gehoord, kunt Visual Studio u nu ook C++ Linux-toepassing te ontwikkelen. U kunt meer informatie over deze nieuwe scenario in de [Visual C++ voor Linux-ontwikkeling](https://blogs.msdn.microsoft.com/vcblog/2016/03/30/visual-c-for-linux-development/) blog. Als u wilt maken voor Linux, moet u een externe computer waarop uw distro Linux wordt uitgevoerd. Als u niet hebt kunt u beschikbaar zijn, stel een snel met behulp van [Linux Azure Virtual machines](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Voor deze zelfstudie laat het ons wordt ervan uitgegaan dat u hebt een Ubuntu 16.04 Linux-distributie instellen. De stappen die hier moeten ook toepassen op Ubuntu 15.10, Red Hat 6 en 7 voor Red Hat. 

De volgende stappen uit installeren de bibliotheken die nodig zijn voor SQL en ODBC voor uw distro:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Visual Studio starten. Onder Tools -> Opties -> Cross-Platform -> Connection Manager, een verbinding toevoegen aan uw Linux-vak: 

![Extra opties](./media/sql-database-develop-cplusplus-simple/tools.png)

Nadat de verbinding via SSH is gemaakt, maakt u een leeg projectsjabloon (Linux): 

![Nieuw project-sjabloon](./media/sql-database-develop-cplusplus-simple/template.png)

Vervolgens kunt u toevoegen een [nieuwe C bronbestand en vervang deze door deze inhoud](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c). Met behulp van de ODBC-API's SQLAllocHandle, SQLSetConnectAttr en SQLDriverConnect, moet u kunnen initialiseren en geen verbinding maken met uw database. Zoals met het Windows-ODBC-voorbeeld moet u de aanroep SQLDriverConnect vervangen door de details van uw databaseparameters voor de verbindingsreeks eerder hebt gekopieerd uit de Azure-portal. 

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

Het laatste wat u moet doen voordat compileren is het toevoegen van **odbc** als een afhankelijkheid van de bibliotheek: 

![ODBC toe te voegen als een invoer-bibliotheek](./media/sql-database-develop-cplusplus-simple/lib.png)

Start uw toepassing, online zetten van de Linux-Console vanaf de **Debug** menu: 

![Linux-Console](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Als de verbinding geslaagd is, ziet u nu de huidige databasenaam afgedrukt in de Linux-Console: 

![Linux-Console-venster uitvoer](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Gefeliciteerd. U de zelfstudie hebt voltooid en u kunt nu verbinding maken met uw Azure SQL DB uit C++ op Windows- en Linux-platforms.

## <a id="GetSolution"></a>De volledige oplossing van de C/C++-zelfstudie
U vindt de GetStarted-oplossing waarin alle voorbeelden in dit artikel op github:

* [Voorbeeld van de ODBC-C++ Windows](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), het voorbeeld Windows C++ ODBC-verbinding maken met Azure SQL downloaden
* [Voorbeeld van de ODBC-C++ Linux](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), het voorbeeld Linux C++ ODBC-verbinding maken met Azure SQL downloaden

## <a name="next-steps"></a>Volgende stappen
* Controleer de [overzicht voor ontwikkelaars van SQL-Database](sql-database-develop-overview.md)
* Meer informatie over de [ODBC-API-referentiemateriaal](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/)

## <a name="additional-resources"></a>Aanvullende bronnen
* [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Bekijk alle [mogelijkheden van SQL Database](https://azure.microsoft.com/services/sql-database/)

