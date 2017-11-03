---
title: Verbinding maken met een Azure-database voor MySQL met behulp van Go | Microsoft Docs
description: Deze snelstartgids bevat enkele voorbeelden van Go-code die u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit een Azure-database voor MySQL.
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: go
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 1f18a35a3c22ecdc379bdffa1ecacb931c62a59d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-go-language-to-connect-and-query-data"></a>Azure-database voor MySQL: de taal Go gebruiken om verbinding te maken en gegevens op te vragen
Deze snelstartgids demonstreert hoe u verbinding maken met een Azure-Database voor MySQL van Windows-, Ubuntu Linux- en Apple Mac OS-platformen met code geschreven in de [gaat](https://golang.org/) taal. U ziet hier hoe u SQL-instructies gebruikt om gegevens in de database op te vragen, in te voegen, bij te werken en te verwijderen. In dit onderwerp wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met behulp van Ga en dat u niet bekend bent met werken met Azure-Database voor MySQL.

## <a name="prerequisites"></a>Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a>Go en de MySQL-connector installeren
Installeer [gaat](https://golang.org/doc/install) en de [gaat u-sql-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) op uw eigen computer. Volg de stappen in het desbetreffende gedeelte afhankelijk van uw platform:

### <a name="windows"></a>Windows
1. [Download](https://golang.org/dl/) en installeer Go voor Microsoft Windows volgens de [installatie-instructies](https://golang.org/doc/install).
2. Open de opdrachtprompt vanuit het Startmenu.
3. Maak een map voor uw project, bijvoorbeeld: `mkdir  %USERPROFILE%\go\src\mysqlgo`.
4. Wijzig de map in de projectmap, bijvoorbeeld `cd %USERPROFILE%\go\src\mysqlgo`.
5. Stel de omgevingsvariabele voor GOPATH zo in dat deze verwijst naar de broncodemap. `set GOPATH=%USERPROFILE%\go`.
6. Installeer het [Go-SQL-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) door de opdracht `go get github.com/go-sql-driver/mysql` uit te voeren.

   Kortom: installeer Go en voer vervolgens deze opdrachten uit in de opdrachtprompt:
   ```cmd
   mkdir  %USERPROFILE%\go\src\mysqlgo
   cd %USERPROFILE%\go\src\mysqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/go-sql-driver/mysql
   ```

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Open de Bash-shell. 
2. Installeer Go door `sudo apt-get install golang-go` uit te voeren.
3. Maak in de basismap een map voor uw project, bijvoorbeeld `mkdir -p ~/go/src/mysqlgo/`.
4. Wijzig de map in de map, bijvoorbeeld `cd ~/go/src/mysqlgo/`.
5. Stel de omgevingsvariabele GOPATH zo in dat deze verwijst naar een geldige bronmap, zoals de Go-map in uw huidige basismap. Uitvoeren op de Bash-shell `export GOPATH=~/go` naar de Ga-map toevoegen als de GOPATH voor de huidige sessie van de shell.
6. Installeer het [Go-SQL-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) door de opdracht `go get github.com/go-sql-driver/mysql` uit te voeren.

   Kortom: voer deze Bash-opdrachten uit:
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a>Apple macOS
1. Download en installeer Ga volgens de [installatie-instructies](https://golang.org/doc/install) die overeenkomt met uw platform. 
2. Open de Bash-shell.
3. Maak in de basismap een map voor uw project, bijvoorbeeld `mkdir -p ~/go/src/mysqlgo/`.
4. Wijzig de map in de map, bijvoorbeeld `cd ~/go/src/mysqlgo/`.
5. Stel de omgevingsvariabele GOPATH zo in dat deze verwijst naar een geldige bronmap, zoals de Go-map in uw huidige basismap. Uitvoeren op de Bash-shell `export GOPATH=~/go` naar de Ga-map toevoegen als de GOPATH voor de huidige sessie van de shell.
6. Installeer het [Go-SQL-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) door de opdracht `go get github.com/go-sql-driver/mysql` uit te voeren.

   Kortom: installeer Go en voer deze Bash-opdrachten uit:
   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor MySQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links in de Azure portal op **alle resources** en zoek vervolgens naar de server die u hebt ook (zoals **myserver4demo**).
3. Klik op de servernaam **myserver4demo**.
4. Selecteer de server **eigenschappen** pagina en maak een notitie van de **servernaam** en **aanmeldingsnaam van Server-beheerder**.
 ![Azure Database voor MySQL - Aanmeldgegevens van de serverbeheerder](./media/connect-go/1_server-properties-name-login.png)
5. Als u uw aanmeldingsgegevens server bent vergeten, gaat u naar de **overzicht** pagina om de aanmeldingsnaam voor Server-beheerder weer te geven en zo nodig het wachtwoord opnieuw instellen.
   

## <a name="build-and-run-go-code"></a>Go-code schrijven en uitvoeren 
1. Als u Golang-code wilt schrijven, gebruikt u een eenvoudige teksteditor zoals Kladblok in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) of [Nano](https://www.nano-editor.org/) in Ubuntu en TextEdit in macOS. Als u liever een uitgebreidere interactieve Development Environment (IDE), probeert [Gogland](https://www.jetbrains.com/go/) door Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) door Microsoft, of [Atom](https://atom.io/).
2. Plak de code Ga in de secties hieronder in tekstbestanden, en deze vervolgens opslaan in de projectmap met bestandsextensie \*.gaat u (zoals Windows-pad `%USERPROFILE%\go\src\mysqlgo\createtable.go` of Linux-pad `~/go/src/mysqlgo/createtable.go`).
3. Zoek de `HOST`, `DATABASE`, `USER`, en `PASSWORD` constanten in de code en vervolgens vervangen in het voorbeeld waarden door uw eigen waarden. 
4. Start de opdrachtprompt of Bash-shell. Wijzig de map in de projectmap. Voorbeeld voor Windows: `cd %USERPROFILE%\go\src\mysqlgo\`. Voorbeeld voor Linux: `cd ~/go/src/mysqlgo/`.  Sommige van de vermelde IDE-editors bieden mogelijkheden voor foutopsporing en runtime zonder dat daarvoor shell-opdrachten zijn vereist.
5. Voer de code uit door de opdracht `go run createtable.go` te typen. De toepassing wordt nu gecompileerd en uitgevoerd. 
6. Als u de code wilt bouwen in een systeemeigen toepassing, kunt u ook `go build createtable.go` gebruiken en vervolgens `createtable.exe` starten om de toepassing uit te voeren.

## <a name="connect-create-table-and-insert-data"></a>Verbinden, tabel maken en gegevens invoegen
De volgende code gebruiken om te verbinden met de server, een tabel maken en de gegevens laden met behulp van een **invoegen** SQL-instructie. 

Met de code worden drie pakketten geïmporteerd: het [SQL-pakket](https://golang.org/pkg/database/sql/), het [GO-SQL-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) (als stuurprogramma voor de communicatie met de Azure-database voor MySQL) en het [fmt-pakket](https://golang.org/pkg/fmt/) (voor de weergave van invoer en uitvoer op de opdrachtregel).

De code roept de methode [sql. Open()](http://go-database-sql.org/accessing.html) verbinding maken met Azure-Database voor MySQL en controleert u de verbinding met behulp van methode [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Er wordt telkens gebruikgemaakt van een [database-ingang](https://golang.org/pkg/database/sql/#DB), die de verbindingsgroep voor de databaseserver bevat. In de code wordt de methode [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) meerdere keren aangeroepen om diverse DDL-opdrachten uit te voeren. De code wordt ook gebruikgemaakt van [Prepare()](http://go-database-sql.org/prepared.html) en Exec() voorbereide instructies uitvoeren met andere parameters drie rijen invoegen. Telkens wanneer wordt een aangepaste checkError()-methode gebruikt om te controleren als er een fout is opgetreden en zorgen om af te sluiten.

Vervang de constanten `host`, `database`, `user` en `password` door uw eigen waarden. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed).")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table.")

    // Insert some data into table.
    sqlStatement, err := db.Prepare("INSERT INTO inventory (name, quantity) VALUES (?, ?);")
    res, err := sqlStatement.Exec("banana", 150)
    checkError(err)
    rowCount, err := res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("orange", 154)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("apple", 100)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}

```

## <a name="read-data"></a>Gegevens lezen
De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **Selecteer** SQL-instructie. 

Met de code worden drie pakketten geïmporteerd: het [SQL-pakket](https://golang.org/pkg/database/sql/), het [GO-SQL-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) (als stuurprogramma voor de communicatie met de Azure-database voor MySQL) en het [fmt-pakket](https://golang.org/pkg/fmt/) (voor de weergave van invoer en uitvoer op de opdrachtregel).

In de code wordt de methode [sql.Open()](http://go-database-sql.org/accessing.html) aangeroepen om verbinding te maken met de Azure-database voor MySQL. Vervolgens wordt de verbinding gecontroleerd met de methode [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Er wordt telkens gebruikgemaakt van een [database-ingang](https://golang.org/pkg/database/sql/#DB), die de verbindingsgroep voor de databaseserver bevat. De code roept de [Query()](https://golang.org/pkg/database/sql/#DB.Query)-methode aan om de geselecteerde opdracht uit te voeren. Vervolgens wordt [Next()](https://golang.org/pkg/database/sql/#Rows.Next) uitgevoerd om de resultatenset te doorlopen en [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) om de kolomwaarden te parseren, waarbij deze worden opgeslagen in de vorm van variabelen. Telkens wordt er een aangepaste checkError()-methode gebruikt om te controleren of er fouten zijn opgetreden en af te sluiten als dit het geval is.

Vervang de constanten `host`, `database`, `user` en `password` door uw eigen waarden. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Variables for printing column data when scanned.
    var (
        id       int
        name     string
        quantity int
    )

    // Read some data from the table.
    rows, err := db.Query("SELECT id, name, quantity from inventory;")
    checkError(err)
    defer rows.Close()
    fmt.Println("Reading data:")
    for rows.Next() {
        err := rows.Scan(&id, &name, &quantity)
        checkError(err)
        fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
    }
    err = rows.Err()
    checkError(err)
    fmt.Println("Done.")
}
```

## <a name="update-data"></a>Gegevens bijwerken
Gebruik de volgende code om verbinding te maken en de gegevens bij te werken met de SQL-instructie **UPDATE**. 

Met de code worden drie pakketten geïmporteerd: het [SQL-pakket](https://golang.org/pkg/database/sql/), het [GO-SQL-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) (als stuurprogramma voor de communicatie met de Azure-database voor MySQL) en het [fmt-pakket](https://golang.org/pkg/fmt/) (voor de weergave van invoer en uitvoer op de opdrachtregel).

In de code wordt de methode [sql.Open()](http://go-database-sql.org/accessing.html) aangeroepen om verbinding te maken met de Azure-database voor MySQL. Vervolgens wordt de verbinding gecontroleerd met de methode [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Er wordt telkens gebruikgemaakt van een [database-ingang](https://golang.org/pkg/database/sql/#DB), die de verbindingsgroep voor de databaseserver bevat. De code roept de [Exec()](https://golang.org/pkg/database/sql/#DB.Exec)-methode aan om de opdracht Bijwerken uit te voeren. Telkens wordt er een aangepaste checkError()-methode gebruikt om te controleren of er fouten zijn opgetreden en af te sluiten als dit het geval is.

Vervang de constanten `host`, `database`, `user` en `password` door uw eigen waarden. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Modify some data in table.
    rows, err := db.Exec("UPDATE inventory SET quantity = ? WHERE name = ?", 200, "banana")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="delete-data"></a>Gegevens verwijderen
Gebruik de volgende code om verbinding te maken en de gegevens te verwijderen met de SQL-instructie **DELETE**. 

Met de code worden drie pakketten geïmporteerd: het [SQL-pakket](https://golang.org/pkg/database/sql/), het [GO-SQL-stuurprogramma voor MySQL](https://github.com/go-sql-driver/mysql#installation) (als stuurprogramma voor de communicatie met de Azure-database voor MySQL) en het [fmt-pakket](https://golang.org/pkg/fmt/) (voor de weergave van invoer en uitvoer op de opdrachtregel).

In de code wordt de methode [sql.Open()](http://go-database-sql.org/accessing.html) aangeroepen om verbinding te maken met de Azure-database voor MySQL. Vervolgens wordt de verbinding gecontroleerd met de methode [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping). Er wordt telkens gebruikgemaakt van een [database-ingang](https://golang.org/pkg/database/sql/#DB), die de verbindingsgroep voor de databaseserver bevat. De code roept de [Exec()](https://golang.org/pkg/database/sql/#DB.Exec)-methode aan om de opdracht Verwijderen uit te voeren. Telkens wordt er een aangepaste checkError()-methode gebruikt om te controleren of er fouten zijn opgetreden en af te sluiten als dit het geval is.

Vervang de constanten `host`, `database`, `user` en `password` door uw eigen waarden. 

```Go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database.")

    // Modify some data in table.
    rows, err := db.Exec("DELETE FROM inventory WHERE name = ?", "orange")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./concepts-migrate-import-export.md)
