---
title: Via Python verbinding maken met Azure Database voor MySQL | Microsoft Docs
description: Deze snelstartgids bevat enkele voorbeelden van Python-code die u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit Azure Database voor MySQL.
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: python
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 40108e0c60a60da412ab521d467a07b8f0540140
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-python-to-connect-and-query-data"></a>Azure Database voor MySQL: Python gebruiken om verbinding te maken en gegevens op te vragen
In deze snelstartgids ziet u hoe u met behulp van [Python](https://python.org) verbinding maakt met Azure Database voor MySQL. Er worden SQL-instructies gebruikt om gegevens op te vragen, in te voegen, bij te werken en te verwijderen in de database vanaf Mac OS-, Ubuntu Linux- en Windows-platforms. In dit onderwerp wordt ervan uitgegaan dat u bekend bent met ontwikkelen met behulp van Python en dat u niet bekend bent met werken met Azure-Database voor MySQL.

## <a name="prerequisites"></a>Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-the-mysql-connector"></a>Python en de MySQL-connector installeren
Installeer [Python](https://www.python.org/downloads/) en de [MySQL-connector voor Python](https://dev.mysql.com/downloads/connector/python/) op uw computer. Volg de stappen in het desbetreffende gedeelte afhankelijk van uw platform:

### <a name="windows"></a>Windows
1. Download en installeer Python 2.7 van [python.org](https://www.python.org/downloads/windows/). 
2. Controleer de Python-installatie door de opdrachtprompt te starten. Voer de opdracht `C:\python27\python.exe -V` uit met als enige argument een hoofdletter V om het versienummer op te vragen.
3. Installeer de Python-connector voor MySQL op [mysql.com](https://dev.mysql.com/downloads/connector/python/) die overeenkomt met uw versie van Python.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. In Linux (Ubuntu) maakt Python meestal deel uit van de standaardinstallatie.
2. Controleer de Python-installatie door de bash-shell te starten. Voer de opdracht `python -V` uit met als enige argument een hoofdletter V om het versienummer op te vragen.
3. Controleer de PIP-installatie door met de opdracht `pip show pip -V` het versienummer op te vragen. 
4. PIP kan zijn opgenomen in sommige versies van Python. Als PIP niet is geïnstalleerd, kunt u het [PIP]-pakket installeren met de opdracht (https://pip.pypa.io/en/stable/installing/) `sudo apt-get install python-pip`.
5. Werk PIP bij naar de nieuwste versie door de opdracht `pip install -U pip` uit te voeren.
6. Gebruik deze PIP-opdracht om de MySQL-connector voor Python en de bijbehorende afhankelijkheden te installeren:

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a>MacOS
1. In Mac OS maakt Python meestal deel uit van de standaardinstallatie van het besturingssysteem.
2. Controleer de Python-installatie door de bash-shell te starten. Voer de opdracht `python -V` uit met als enige argument een hoofdletter V om het versienummer op te vragen.
3. Controleer de PIP-installatie door met de opdracht `pip show pip -V` het versienummer op te vragen.
4. PIP kan zijn opgenomen in sommige versies van Python. Als PIP niet is geïnstalleerd, kunt u het [PIP](https://pip.pypa.io/en/stable/installing/)-pakket installeren.
5. Werk PIP bij naar de nieuwste versie door de opdracht `pip install -U pip` uit te voeren.
6. Gebruik deze PIP-opdracht om de MySQL-connector voor Python en de bijbehorende afhankelijkheden te installeren:

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor MySQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links in Azure Portal op **Alle resources** en zoek de server die u hebt gemaakt (bijvoorbeeld **myserver4demo**).
3. Klik op de servernaam **myserver4demo**.
4. Selecteer de pagina **Eigenschappen** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**.
 ![Azure Database voor MySQL - Aanmeldgegevens van de serverbeheerder](./media/connect-python/1_server-properties-name-login.png)
5. Als u uw aanmeldingsgegevens voor de server bent vergeten, gaat u naar de pagina **Overzicht** om de aanmeldingsnaam van de serverbeheerder weer te geven en indien nodig het wachtwoord opnieuw in te stellen.
   

## <a name="run-python-code"></a>Python-code uitvoeren
- Plak de code in een tekstbestand en sla het bestand in een projectmap met bestand extensie .py (zoals C:\pythonmysql\createtable.py of /home/username/pythonmysql/createtable.py).
- Als u wilt de code uitvoeren, starten vanaf de opdrachtprompt of Bash-shell. Ga naar de projectmap `cd pythonmysql`. Typ vervolgens de python-opdracht, gevolgd door de bestandsnaam `python createtable.py` om de toepassing uit te voeren. Op het Windows-besturingssysteem als python.exe niet wordt gevonden, u mogelijk moet het volledige pad naar het uitvoerbare bestand opgeven of het Python-pad toevoegen aan de omgevingsvariabele path. `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a>Verbinden, tabel maken en gegevens invoegen
De volgende code gebruiken om te verbinden met de server, een tabel maken en de gegevens laden met behulp van een **invoegen** SQL-instructie. 

Met deze code wordt de bibliotheek mysql.connector geïmporteerd. De functie [connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wordt gebruikt om verbinding te maken met Azure Database voor MySQL, met behulp van de [connection-argumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in de config-verzameling. De code maakt gebruik van een cursor op de verbinding en de methode [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) voert de SQL-query op MySQL-database. 

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Drop previous table of same name if one exists
  cursor.execute("DROP TABLE IF EXISTS inventory;")
  print("Finished dropping table (if existed).")

  # Create table
  cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
  print("Finished creating table.")

  # Insert some data into table
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
  print("Inserted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="read-data"></a>Gegevens lezen
De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **Selecteer** SQL-instructie. 

Met deze code wordt de bibliotheek mysql.connector geïmporteerd. De functie [connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wordt gebruikt om verbinding te maken met Azure Database voor MySQL, met behulp van de [connection-argumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in de config-verzameling. De code maakt gebruik van een cursor op de verbinding en de methode [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) voert de SQL-instructie op MySQL-database. De rijen met gegevens worden gelezen met methode [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html). De resultatenset wordt bewaard in een verzamelrij en via een for-iterator worden de rijen steeds doorlopen.

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Read data
  cursor.execute("SELECT * FROM inventory;")
  rows = cursor.fetchall()
  print("Read",cursor.rowcount,"row(s) of data.")

  # Print all rows
  for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="update-data"></a>Gegevens bijwerken
De volgende code gebruiken om verbinding te en bijwerken van de gegevens met behulp van een **bijwerken** SQL-instructie. 

Met deze code wordt de bibliotheek mysql.connector geïmporteerd.  De functie [connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wordt gebruikt om verbinding te maken met Azure Database voor MySQL, met behulp van de [connection-argumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in de config-verzameling. De code maakt gebruik van een cursor op de verbinding en de methode [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) voert de SQL-instructie op MySQL-database. 

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in the table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a>Gegevens verwijderen
De volgende code gebruiken om verbinding te en verwijderen van gegevens met behulp van een **verwijderen** SQL-instructie. 

Met deze code wordt de bibliotheek mysql.connector geïmporteerd.  De functie [connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) wordt gebruikt om verbinding te maken met Azure Database voor MySQL, met behulp van de [connection-argumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in de config-verzameling. De code maakt gebruik van een cursor op de verbinding en de methode [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) voert de SQL-query op MySQL-database. 

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established.")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in the table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./concepts-migrate-import-export.md)
