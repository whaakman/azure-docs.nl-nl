---
title: Vanuit Python verbinding maken met Azure Database voor PostgreSQL | Microsoft Docs
description: Biedt een voorbeeld van Python-code dat u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit een Azure Database voor PostgreSQL.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: python
ms.topic: hero-article
ms.date: 07/07/2017
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 3cd090b02887857a68271f021e3580e05660d1dc
ms.contentlocale: nl-nl
ms.lasthandoff: 07/13/2017

---
# <a name="azure-database-for-postgresql-use-python-to-connect-and-query-data"></a>Azure Database voor PostgreSQL: Python gebruiken om verbinding te maken en gegevens op te vragen
In deze snelstartgids ziet u hoe u [Python](https://python.org) gebruikt om verbinding te maken met een Azure Database voor PostgresSQL en vervolgens SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen vanaf het Mac OS-, Ubuntu Linux- en Windows-platform. In de stappen van dit artikel wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met behulp van Python en geen ervaring hebt met het werken met Azure Database voor PostgreSQL.

## <a name="prerequisites"></a>Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Database maken - Portal](quickstart-create-server-database-portal.md)
- [Database maken - CLI](quickstart-create-server-database-azure-cli.md)

U hebt ook het volgende nodig:
- [Python](https://www.python.org/downloads/) geïnstalleerd
- [pip](https://pip.pypa.io/en/stable/installing/)-pakket geïnstalleerd (pip is al geïnstalleerd als u binaire bestanden van Python 2 > =2.7.9 of Python 3 >=3.4 gebruikt die zijn gedownload van [python.org](https://python.org), maar u moet wel pip upgraden.)

## <a name="install-the-python-connection-libraries-for-postgresql"></a>De Python-verbindingsbibliotheken voor PostgreSQL installeren
Installeer het [psycopg2](http://initd.org/psycopg/docs/install.html)-pakket waarmee u verbinding kon maken met de database en er query's op kon uitvoeren. psycopg2 is [beschikbaar op PyPI](https://pypi.python.org/pypi/psycopg2/) in de vorm van [wheel](http://pythonwheels.com/)-pakketten voor de meest voorkomende platformen (Linux, OSX, Windows), zodat u de pip-installatie kunt installeren om de binaire versie van de module te downloaden, inclusief alle afhankelijkheden:

```cmd
pip install psycopg2
```
Zorg ervoor dat u een actuele versie van pip gebruikt (u kunt upgraden met behulp van bijvoorbeeld `pip install -U pip`)

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor PostgreSQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. In het menu links in Azure Portal klikt u op **Alle resources** en zoekt u naar de server die u zojuist hebt gemaakt: **mypgserver-20170401**.
3. Klik op de servernaam **mypgserver-20170401**.
4. Selecteer de pagina **Overzicht** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**.
 ![Azure Database voor PostgreSQL - Aanmeldgegevens van de serverbeheerder](./media/connect-python/1-connection-string.png)
5. Als u uw aanmeldingsgegevens voor de server bent vergeten, gaat u naar de pagina **Overzicht** om de aanmeldingsnaam van de serverbeheerder weer te geven en indien nodig het wachtwoord opnieuw in te stellen.

## <a name="how-to-run-python-code"></a>Python-code uitvoeren
- Maak in uw favoriete teksteditor een nieuw bestand met de naam postgres.py en sla het bestand op in een projectmap. Kopieer hieronder een fragment met voorbeeldcode, plak dit in het tekstbestand en sla het bestand op. Zorg ervoor dat u UTF-8-codering selecteert als u het bestand opslaat in Windows. 
- Voor het uitvoeren van de code opent u het opdrachtprompt of de bash-shell. Ga naar de projectmap, zoals `cd postgresql`. Typ vervolgens de python-opdracht, gevolgd door de bestandsnaam, zoals `python postgresql.py`.

> [!NOTE]
> Vanaf versie 3 van Python ziet u mogelijk de fout `SyntaxError: Missing parentheses in call to 'print'` wanneer u de onderstaande codeblokken uitvoert. Als dat gebeurt, vervangt u elke aanroep van de opdracht `print "string"` door een functieaanroep met haakjes, zoals `print("string")`.

## <a name="connect-create-table-and-insert-data"></a>Verbinden, tabel maken en gegevens invoegen
Gebruik de volgende code om verbinding te maken en de gegevens te laden met de functie [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) met een SQL-instructie **INSERT**. De functie [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) wordt gebruikt om de SQL-query uit te voeren op de PostgreSQL-database. Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="read-data"></a>Gegevens lezen
Gebruik de volgende code om de gegevens te lezen die zijn ingevoegd met behulp van de functie [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) met de SQL-instructie **SELECT**. Deze functie accepteert een query en retourneert een resultatenset die met behulp van [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall) kan worden herhaald. Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Gegevens bijwerken
Gebruik de volgende code om met de SQL-instructie **UPDATE** de inventarisrij bij te werken die u eerder hebt ingevoegd met behulp van de functie [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute). Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Gegevens verwijderen
Gebruik de volgende code om met de SQL-instructie **DELETE** de inventarisrij te verwijderen die u eerder hebt ingevoegd met behulp van de functie [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute). Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./howto-migrate-using-export-and-import.md)

