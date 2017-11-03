---
title: Via C++ verbinding maken met Azure Database voor MySQL | Microsoft Docs
description: Deze snelstartgids bevat een voorbeeld van C++-code dat u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit een Azure Database voor MySQL.
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: C++
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 92620c8081b1f0f5c96cc3ae09465b3526e74042
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a>Azure Database voor MySQL: Connector/C++ gebruiken om verbinding te maken en gegevens op te vragen
Deze snelstartgids demonstreert hoe u verbinding maken met een Azure-Database voor MySQL met behulp van een C++-toepassing. U ziet hier hoe u SQL-instructies gebruikt om gegevens in de database op te vragen, in te voegen, bij te werken en te verwijderen. In dit onderwerp wordt ervan uitgegaan dat u bekend bent met ontwikkelen met C++ en dat u niet bekend bent met werken met Azure-Database voor MySQL.

## <a name="prerequisites"></a>Vereisten
Deze snelstartgids maakt gebruik van de resources die zijn gemaakt in een van de volgende handleidingen als uitgangspunt nemen:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

U moet ook het volgende doen:
- [.NET Framework](https://www.microsoft.com/net/download) installeren
- [Visual Studio](https://www.visualstudio.com/downloads/) installeren
- [MySQL-connector/C++](https://dev.mysql.com/downloads/connector/cpp/) installeren 
- [Boost](http://www.boost.org/) installeren

## <a name="install-visual-studio-and-net"></a>Visual Studio en .NET installeren
Voor de stappen in dit gedeelte wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met .NET.

### <a name="windows"></a>**Windows**
- Installeer Visual Studio 2017 Community, dit een uitbreidbaar, gratis IDE is voor het maken van moderne toepassingen voor Android, iOS, Windows, evenals web en databasetoepassingen, aanbevolen, volledige en cloudservices. U kunt de volledige .NET Framework of alleen .NET Core installeren: de codefragmenten in de Quick Start werkt met beide. Als u Visual Studio is geïnstalleerd op uw computer al hebt, kunt u de volgende twee stappen overslaan.
   1. Download het [installatieprogramma voor Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15). 
   2. Voer het installatieprogramma uit en volg de weergegeven opdrachten om de installatie te voltooien.

### <a name="configure-visual-studio"></a>**Visual Studio configureren**
1. Ga in Visual Studio naar projecteigenschap > configuratie-eigenschappen > C/C++ > linker > algemeen > aanvullende bibliotheekmappen, en voeg de map lib\opt (bijvoorbeeld: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) van de c++-connector toe.
2. Eigenschap vanuit Visual Studio-project > configuratie-eigenschappen > C/C++ > Algemeen > aanvullende mappen bevatten:
   - Voeg opnemen / Active directory van c++-connector (dat wil zeggen: C:\Program Files (x86) \MySQL\MySQL Connector C++ 1.1.9\include\).
   - Hoofdmap versterking-bibliotheek toevoegen (dat wil zeggen: C:\boost_1_64_0\).
3. Eigenschap vanuit Visual Studio-project > configuratie-eigenschappen > C/C++ > linker > invoer > Extra afhankelijkheden mysqlcppconn.lib toevoegen in het tekstveld.
4. Kopieer mysqlcppconn.dll uit de bibliotheekmap C++-connector in stap 3 naar dezelfde map als het uitvoerbare bestand van de toepassing of toe te voegen aan de omgevingsvariabele zodat uw toepassing kan worden gedetecteerd.

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor MySQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links in Azure-portal op **alle resources**, en het zoeken naar de server die u hebt gemaakt (zoals **myserver4demo**).
3. Klik op de servernaam.
4. Selecteer de server **eigenschappen** pagina en maak een notitie van de **servernaam** en **aanmeldingsnaam van Server-beheerder**.
 ![Naam van Azure Database voor MySQL-server](./media/connect-cpp/1_server-properties-name-login.png)
5. Als u uw aanmeldingsgegevens server bent vergeten, gaat u naar de **overzicht** pagina om de aanmeldingsnaam voor Server-beheerder weer te geven en zo nodig het wachtwoord opnieuw instellen.

## <a name="connect-create-table-and-insert-data"></a>Verbinden, tabel maken en gegevens invoegen
De volgende code gebruiken om verbinding te en de gegevens laden met behulp van **CREATE TABLE** en **INSERT INTO** SQL-instructies. In de code wordt de klasse sql::Driver met de methode connect() gebruikt om een verbinding te maken met MySQL. Vervolgens worden de methoden createStatement() en execute()) gebruikt om de databaseopdrachten uit te voeren. 

Vervang de parameters Host, DBName, User en Password door de waarden die u hebt opgegeven tijdens het maken van de server en database. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a>Gegevens lezen

De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **Selecteer** SQL-instructie. In de code wordt de klasse sql::Driver met de methode connect() gebruikt om een verbinding te maken met MySQL. Vervolgens worden de methoden prepareStatement() en executeQuery() gebruikt om de SELECT-opdrachten uit te voeren. De code gebruikt vervolgens next() om naar de records in de resultaten te gaan. De code wordt tot slot getInt() en getString() parseren van de waarden in de record.

Vervang de parameters Host, DBName, User en Password door de waarden die u hebt opgegeven tijdens het maken van de server en database. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a>Gegevens bijwerken
De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **UPDATE** SQL-instructie. In de code wordt de klasse sql::Driver met de methode connect() gebruikt om een verbinding te maken met MySQL. Vervolgens worden de methoden prepareStatement() en executeQuery() gebruikt om de UPDATE-opdrachten uit te voeren. 

Vervang de parameters Host, DBName, User en Password door de waarden die u hebt opgegeven tijdens het maken van de server en database. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a>Gegevens verwijderen
De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **verwijderen** SQL-instructie. In de code wordt de klasse sql::Driver met de methode connect() gebruikt om een verbinding te maken met MySQL. Vervolgens worden de methoden prepareStatement() en executeQuery() gebruikt om de DELETE-opdrachten uit te voeren.

Vervang de parameters Host, DBName, User en Password door de waarden die u hebt opgegeven tijdens het maken van de server en database. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Uw MySQL-database migreren naar Azure Database voor MySQL met behulp van dumpen en terugzetten](concepts-migrate-dump-restore.md)
