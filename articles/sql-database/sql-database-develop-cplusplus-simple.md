---
title: Verbinding maken met SQL Database met behulp van C en C++ | Microsoft Docs
description: Gebruik de voorbeeldcode in deze quick start om een moderne toepassing met C++ en ondersteund door een krachtige relationele database in de cloud met Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: cpp
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/12/2018
ms.openlocfilehash: 6ebef74cee6f919fe6b8cf666db06e0ab22cec73
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55566921"
---
# <a name="connect-to-sql-database-using-c-and-c"></a>Verbinding maken met SQL Database met behulp van C en C++

Dit bericht is gericht op C en C++-ontwikkelaars die verbinding maken met Azure SQL-database. Het is onderverdeeld in secties, zodat u kunt naar de sectie die het beste uw interesse legt gaan.

## <a name="prerequisites-for-the-cc-tutorial"></a>Vereisten voor de C/C++-zelfstudie

Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen abonnement hebt, kunt u zich registreren voor een [gratis Azure-proefversie](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/). U kunt de C++-taalonderdelen om te bouwen en uitvoeren van dit voorbeeld moet installeren.
* [Visual Studio Linux Development](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Als u op Linux ontwikkelt, moet u ook de Visual Studio-Linux-extensie installeren.

## <a id="AzureSQL"></a>Azure SQL Database en SQL Server op virtuele machines
SQL Azure is gebouwd op Microsoft SQL Server en is ontworpen voor een hoge beschikbaarheid, goed presterende en schaalbare service. Er zijn veel voordelen voor het gebruik van SQL Azure via uw eigen database dat on-premises. Met SQL Azure-hebt u geen te installeren, instellen, onderhouden of beheren van uw database, maar alleen de inhoud en de structuur van uw database. Dingen die we een zorgen maken over met databases zoals zijn alle fouttolerantie en redundantie ingebouwd in.

Azure heeft momenteel twee opties voor het hosten van SQL server-workloads: Azure SQL database,-database as a service en SQL server op virtuele Machines (VM). Je krijgt geen informatie over de verschillen tussen deze twee, behalve dat Azure SQL-database de beste keuze voor nieuwe cloudgebaseerde toepassingen is om te profiteren van de kostenbesparingen en optimalisatie van prestaties die cloudservices bieden. Als u van plan te bent migreren of uitbreiden van uw on-premises toepassingen naar de cloud, kan SQL server op virtuele machine van Azure beter voor u werkt. Dingen om eenvoudig te houden voor in dit artikel, gaan we een Azure SQL-database maken.

## <a id="ODBC"></a>Data access-technologieën: ODBC- en OLE DB
Verbinding maken met Azure SQL-database is niet anders en er zijn momenteel twee manieren verbinding maken met databases: ODBC (Open Database connectivity) en OLE DB (Object koppelen en Embedding-database). Microsoft heeft in de afgelopen jaren uitgelijnd met [ODBC voor toegang tot systeemeigen relationele gegevens](https://blogs.msdn.microsoft.com/sqlnativeclient/2011/08/29/microsoft-is-aligning-with-odbc-for-native-relational-data-access/). ODBC is relatief eenvoudig en ook veel sneller dan OLE DB. Hier het enige nadeel is dat een oude C-style-API maakt gebruik van ODBC.

## <a id="Create"></a>Stap 1:  Het maken van uw Azure SQL-Database
Zie de [aan de slag pagina](sql-database-get-started-portal.md) voor informatie over het maken van een voorbeelddatabase.  U kunt ook kunt u volgen dit [korte video van twee minuten](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) te maken van een Azure SQL-database met behulp van de Azure-portal.

## <a id="ConnectionString"></a>Stap 2:  Verbindingsreeks ophalen
Nadat u uw Azure SQL database is ingericht, moet u bij het uitvoeren van de volgende stappen uit om te bepalen van de verbindingsgegevens en uw client-IP-adres voor firewalltoegang toevoegen.

In [Azure-portal](https://portal.azure.com/), gaat u naar uw Azure SQL-database ODBC-verbindingsreeks met behulp van de **databaseverbindingsreeksen tonen** vermeld als een deel van de overzichtssectie voor uw database:

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

Kopieer de inhoud van de **ODBC (inclusief Node.js) [SQL-verificatie]** tekenreeks. We gebruiken deze tekenreeks later opnieuw verbinding maken vanaf de opdrachtregel C++ ODBC-interpreter. Deze tekenreeks bevat informatie zoals het stuurprogramma, server en andere verbindingsparameters database.

## <a id="Firewall"></a>Stap 3:  Uw IP-adres toevoegen aan de firewall
Ga naar de firewallsectie voor uw Database-server en voeg uw [client-IP-aan de firewall met de volgende stappen](sql-database-configure-firewall-settings.md) om ervoor te zorgen we geslaagde verbinding kan maken:

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

Op dit moment uw Azure SQL-database hebt geconfigureerd en klaar bent om te verbinden vanaf uw C++-code.

## <a id="Windows"></a>Stap 4: Verbinding maken vanaf een Windows-C/C++-toepassing
U kunt eenvoudig verbinding maken met uw [Azure SQL-database met behulp van ODBC op Windows met behulp van dit voorbeeld](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) die wordt gemaakt met Visual Studio. Het voorbeeld wordt een opdrachtregel ODBC-interpreter die kan worden gebruikt om verbinding maken met onze Azure SQL-database geïmplementeerd. In dit voorbeeld wordt een Database source name (DSN) bestand als een opdrachtregelargument ofwel de uitgebreide verbindingsreeks die we eerder hebt gekopieerd vanuit Azure portal. Open de eigenschappenpagina voor dit project en plak de verbindingsreeks als een opdrachtargument voor een zoals hier wordt weergegeven:

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Zorg ervoor dat u de juiste verificatiegegevens voor uw database opgeven als onderdeel van de verbindingsreeks voor deze database.

Start de toepassing te bouwen. U ziet het volgende venster valideren van de verbinding is geslaagd. U kunt zelfs een SQL-basisopdrachten, zoals uitvoeren **-tabel maken** voor het valideren van de verbinding met de database:

![SQL-opdrachten](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

U kunt ook een DSN-bestand met de wizard die wordt gestart wanneer er geen argumenten zijn opgegeven maken. Het is raadzaam dat u deze optie ook proberen. U kunt deze DSN-bestand gebruiken voor automation en beschermen van uw verificatie-instellingen:

![DSN-bestand maken](./media/sql-database-develop-cplusplus-simple/datasource.png)

Gefeliciteerd! U hebt nu verbinding gemaakt met Azure SQL met C++ en ODBC op Windows. U kunt lezen om te doen voor Linux-platform hetzelfde blijven.

## <a id="Linux"></a>Stap 5: Verbinding maken vanaf een Linux-C/C++-toepassing
Als u dit nog niet hebt op basis van het nieuws nog, kunt Visual Studio u nu ook Linux C++-toepassing te ontwikkelen. U kunt meer informatie over deze nieuwe scenario in de [Visual C++ voor ontwikkeling onder Linux](https://blogs.msdn.microsoft.com/vcblog/2016/03/30/visual-c-for-linux-development/) blog. Als u wilt maken voor Linux, moet u een externe computer waarop uw Linux-distributie wordt uitgevoerd. Als u geen een beschikbaar is, u kunt een instellen snel met behulp van [Linux Azure Virtual machines](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Voor deze zelfstudie laat het ons wordt ervan uitgegaan dat u een Ubuntu 16.04 Linux-distributie die u instelt. De stappen die hier moeten ook van toepassing op 15.10 Ubuntu, Red Hat 6 en 7 van Red Hat.

De volgende stappen uit installeren de bibliotheken die nodig zijn voor SQL en ODBC voor uw distributie:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Launch Visual Studio. Onder Tools -> Options -> Cross-Platform -> Connection Manager, een verbinding toevoegen aan uw box Linux:

![Extra opties](./media/sql-database-develop-cplusplus-simple/tools.png)

Nadat verbinding via SSH is gemaakt, maakt u een leeg project (Linux)-sjabloon:

![Nieuwe projectsjabloon, maken](./media/sql-database-develop-cplusplus-simple/template.png)

Vervolgens kunt u toevoegen een [nieuw C bronbestand en vervang deze door deze inhoud](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c). Met behulp van de ODBC-API's SQLAllocHandle, SQLSetConnectAttr en SQLDriverConnect, zou het mogelijk om te initialiseren en geen verbinding maken met uw database.
Net als met het Windows-ODBC-voorbeeld moet u de aanroep SQLDriverConnect vervangen door de details van uw database Verbindingsreeksparameters eerder hebt gekopieerd vanuit Azure portal.

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

Het laatste wat u moet doen voordat het compileren van is het toevoegen van **odbc** als een afhankelijkheid van de bibliotheek:

![ODBC toe te voegen als een invoer-bibliotheek](./media/sql-database-develop-cplusplus-simple/lib.png)

Voor het starten van uw toepassing, halen de Linux-Console van de **Debug** menu:

![Linux-Console](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Als de verbinding geslaagd is, ziet u nu de huidige databasenaam in de Linux-Console afgedrukt:

![Linux-Console-venster uitvoer](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Gefeliciteerd! U hebt de zelfstudie voltooid en kunt nu verbinding maken met uw Azure SQL DB via C++ op Windows en Linux-platforms.

## <a id="GetSolution"></a>De volledige C/C++-zelfstudieoplossing ophalen
U vindt de oplossing GetStarted alle voorbeelden in dit artikel op GitHub bevat:

* [Voorbeeld van ODBC-C++ Windows](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), downloadt het voorbeeld Windows C++ ODBC-verbinding maken met Azure SQL
* [Voorbeeld van ODBC-C++ Linux](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), het Linux-voorbeeld C++ ODBC-verbinding maken met Azure SQL downloaden

## <a name="next-steps"></a>Volgende stappen
* Controleer de [overzicht van de ontwikkeling van de SQL Database](sql-database-develop-overview.md)
* Meer informatie over de [ODBC-API-verwijzing](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/)

## <a name="additional-resources"></a>Aanvullende resources
* [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Bekijk alle [mogelijkheden van SQL Database](https://azure.microsoft.com/services/sql-database/)

