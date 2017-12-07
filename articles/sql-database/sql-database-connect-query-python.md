---
title: Python gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Python gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: On Demand
ms.tgt_pltfrm: n
ms.devlang: python
ms.topic: quickstart
ms.date: 08/09/2017
ms.author: carlrab
ms.openlocfilehash: f7496c7b78449d5ceae861dc0daac607acdb1f84
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-python-to-query-an-azure-sql-database"></a>Python gebruiken om een query uit te voeren voor een Azure SQL-database

 In deze Quick Start wordt gedemonstreerd hoe u [Python](https://python.org) gebruikt om verbinding te maken met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor de gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze beknopte zelfstudie te voltooien:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Een [firewallregel op serverniveau](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) voor het openbare IP-adres van de computer die u gebruikt voor deze beknopte zelfstudie.

- U hebt Python en verwante software voor het besturingssysteem geïnstalleerd:

    - **MacOS**: installeer Homebrew en Python, installeer het ODBC-stuurprogramma en SQLCMD, en installeer vervolgens het Python-stuurprogramma voor SQL Server. Zie [Stap 1.2, 1.3 en 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu**: installeer Python en andere vereiste pakketten, en installeer vervolgens het Python-stuurprogramma voor SQL Server. Zie [Stap 1.2, 1.3 en 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
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
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
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

