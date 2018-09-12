---
title: Python gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Python gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: python
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: carlrab
ms.openlocfilehash: 2f907ccd371a9b2c4601a5ea266326a32c4f665d
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44054930"
---
# <a name="use-python-to-query-an-azure-sql-database"></a>Python gebruiken om een query uit te voeren voor een Azure SQL-database

 In deze Quick Start wordt gedemonstreerd hoe u [Python](https://python.org) gebruikt om verbinding te maken met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor de gegevens. Voor meer informatie voor sdk, bekijk onze [verwijzings](https://docs.microsoft.com/python/api/overview/azure/sql)-documentatie, een pyodbc-[voorbeeld](https://github.com/mkleehammer/pyodbc/wiki/Getting-started), en de [pyodbc-](https://github.com/mkleehammer/pyodbc/wiki/) GitHub-opslagplaats.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Een [firewallregel op serverniveau](sql-database-get-started-portal-firewall.md) voor het openbare IP-adres van de computer die u gebruikt voor deze snelstart.

- U hebt Python en verwante software voor het besturingssysteem geïnstalleerd:

    - **MacOS**: installeer Homebrew en Python, installeer het ODBC-stuurprogramma en SQLCMD, en installeer vervolgens het Python-stuurprogramma voor SQL Server. Zie [Stap 1.2, 1.3 en 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu**: installeer Python en andere vereiste pakketten, en installeer vervolgens het Python-stuurprogramma voor SQL Server. Zie [stap 1.2, 1.3 en 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
    - **Windows**: installeer de nieuwste versie van Python (de omgevingsvariabele wordt nu voor u geconfigureerd), installeer het ODBC-stuurprogramma en SQLCMD, en installeer vervolgens het Python-stuurprogramma voor SQL Server. Zie [Stap 1.2, 1.3 en 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/). 

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Code invoegen om een query uit te voeren voor een SQL-database 

1. Maak een nieuw bestand in uw favoriete teksteditor **sqltest.py**.  

2. Vervang de inhoud door de volgende code en voeg de juiste waarden toe voor de server, de database, de gebruiker en het wachtwoord.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-the-code"></a>De code uitvoeren

1. Voer bij de opdrachtprompt de volgende opdrachten uit:

   ```Python
   python sqltest.py
   ```

2. Controleer of de bovenste 20 rijen worden geretourneerd, en sluit vervolgens het toepassingsvenster.

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft Python-stuurprogramma's voor SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Developer Center](https://azure.microsoft.com/develop/python/?v=17.23h)

