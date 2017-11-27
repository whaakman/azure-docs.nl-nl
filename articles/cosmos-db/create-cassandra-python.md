---
title: 'Snelstartgids: Cassandra API met behulp van Python - Azure Cosmos DB | Microsoft Docs'
description: Deze snelstartgids laat zien hoe de Azure Cosmos-DB Apache Cassandra API gebruiken om u te maken van de toepassing van een profiel met behulp van Python
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4ebc883e-c512-4e34-bd10-19f048661159
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: 4a2347fe9578b35c95d240c5c4dd2bf062077ece
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-cassandra-app-with-python-and-azure-cosmos-db"></a>Snelstartgids: Een app Cassandra met Python en Azure Cosmos DB bouwen

Deze snelstartgids laat zien hoe u Python en de Azure DB die Cosmos [Cassandra API](cassandra-introduction.md) voor het bouwen van een app profiel door het klonen van een voorbeeld van GitHub. Deze snelstartgids leidt u door het maken van een Azure DB die Cosmos-account via het web gebaseerde Azure-portal.

Azure Cosmos-database is de service van Microsoft wereldwijd gedistribueerde database voor meerdere model. U kunt snel maken en query document, tabel, sleutel / waarde- en grafiek databases, die allemaal van de algemene distributie en de mogelijkheden van de horizontale schaal de kern van Azure Cosmos DB profiteren.   

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]U kunt ook [gratis Azure Cosmos DB probeer](https://azure.microsoft.com/try/cosmosdb/) gratis verbintenissen en zonder een Azure-abonnement.

Toegang tot de API van Azure Cosmos DB Cassandra preview-programma. Als u nog niet voor toegang tot toegepast [nu aanmelden](cassandra-introduction.md#sign-up-now).

Daarnaast doet u het volgende:
* [Python](https://www.python.org/downloads/) versie v2.7.14
* [Git](http://git-scm.com/)
* [Python-stuurprogramma voor Apache Cassandra](https://github.com/datastax/python-driver)

## <a name="create-a-database-account"></a>Een databaseaccount maken

Voordat u een documentdatabase maken kunt, moet u een Cassandra-account maken met Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we kloon een Cassandra-API-app vanuit github, de verbindingsreeks instellen en uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een git-terminalvenster zoals git bash, en gebruik de `cd` opdracht om te wijzigen naar een map voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt u een kopie van de voorbeeld-app op uw computer. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources worden gemaakt in de code bent, kunt u de volgende codefragmenten bekijken. De codefragmenten zijn afkomstig uit de `pyquickstart.py` bestand. Anders kunt u verder gaan naar [bijwerken van de verbindingsreeks](#update-your-connection-string). 

* Gebruikersnaam en wachtwoord is ingesteld met behulp van de pagina van de tekenreeks verbinding in de Azure portal. U de path\to\cert vervangen door het pad naar uw X509 certificaat.

   ```python
    ssl_opts = {
            'ca_certs': 'path\to\cert',
            'ssl_version': ssl.PROTOCOL_TLSv1_2
            }
    auth_provider = PlainTextAuthProvider( username=cfg.config['username'], password=cfg.config['password'])
    cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider, ssl_options=ssl_opts)
    session = cluster.connect()
   
   ```

* De `cluster` is geïnitialiseerd met contactPoint informatie. De contactPoint wordt opgehaald uit de Azure-portal.

    ```python
   cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider)
    ```

* De `cluster` maakt verbinding met de Azure-API voor Cassandra Cosmos DB.

    ```python
    session = cluster.connect()
    ```

* Een nieuwe keyspace wordt gemaakt.

    ```python
   session.execute('CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }')
    ```

* Een nieuwe tabel is gemaakt.

   ```
   session.execute('CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)');
   ```

* Sleutel/waarde-entiteiten worden ingevoegd.

    ```Python
    insert_data = session.prepare("INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)")
    batch = BatchStatement()
    batch.add(insert_data, (1, 'LyubovK', 'Dubai'))
    batch.add(insert_data, (2, 'JiriK', 'Toronto'))
    batch.add(insert_data, (3, 'IvanH', 'Mumbai'))
    batch.add(insert_data, (4, 'YuliaT', 'Seattle'))
    ....
    session.execute(batch)
    ```

* Query voor het ophalen van alle sleutelwaarden ophalen.

    ```Python
    rows = session.execute('SELECT * FROM uprofile.user')
    ```  
    
* De query voor het ophalen van een sleutel-waarde.

    ```Python
    
    rows = session.execute('SELECT * FROM uprofile.user where user_id=1')
    ```  

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan uw app kan communiceren met uw gehoste-database.

1. In de [Azure-portal](http://portal.azure.com/), klikt u op **verbindingsreeks**. 

    Gebruik de ![Knop Kopiëren](./media/create-cassandra-python/copy.png) knop aan de rechterkant van het scherm voor het kopiëren van de bovenste waarde, het CONTACT-punt.

    ![Bekijken en kopiëren van een gebruiker via een toegangspunt, wachtwoord en neem contact op met de Azure portal, verbinding tekenreeks blade](./media/create-cassandra-python/keys.png)

2. Open het `config.py`-bestand. 

3. De waarde van het punt van CONTACT vanuit de portal via plakken `<FILLME>` op regel 10.

    Regel 10 nu zijn vergelijkbaar met 

    `'contactPoint': 'cosmos-db-quickstarts.documents.azure.com:10350'`

4. De waarde van de gebruikersnaam van de portal kopiëren en plakken via `<FILLME>` op regel 6.

    Regel 6 nu zijn vergelijkbaar met 

    `'username': 'cosmos-db-quickstart',`
    
5. De waarde van het wachtwoord van de portal kopiëren en plakken via `<FILLME>` op regel 8.

    Regel 8 nu zijn vergelijkbaar met

    `'password' = '2Ggkr662ifxz2Mg==`';`

6. Sla het bestand config.py.
    
## <a name="use-the-x509-certificate"></a>Gebruik de X509-certificaat

1. Als u het Baltimore CyberTrust Root toevoegen moet, heeft het serienummer 02:00:00:b9 en SHA1 vingerafdruk d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Het kan worden gedownload vanaf https://cacert.omniroot.com/bc2025.crt, opgeslagen op een lokaal bestand met extensie .cer

2. Open pyquickstart.py en path\to\cert om te verwijzen naar het nieuwe certificaat te wijzigen.

3. Pyquickstart.py opslaan.

## <a name="run-the-app"></a>De app uitvoeren

1. Gebruik de opdracht cd in de terminal git naar de map azure-cosmos-db-cassandra-python-getting-started. 

2. Voer de volgende opdrachten voor het installeren van de vereiste modules:

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Voer de volgende opdracht om uw knooppunttoepassing te starten:

    ```
    python pyquickstart.py
    ```

3. Controleer of de resultaten naar verwachting vanaf de opdrachtregel.

    Druk op CTRL + c drukken om te stoppen exection van het programma en sluit het consolevenster. 

    ![Bekijken en controleren van de uitvoer](./media/create-cassandra-python/output.png)
    
    U kunt nu Data Explorer openen in de Azure portal om te zien van de query, wijzigen en werken met deze nieuwe gegevens. 

    ![De gegevens in Data Explorer weergeven](./media/create-cassandra-python/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hoe u een verzameling kunt maken met Data Explorer en hebt u een app uitgevoerd. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren. 

> [!div class="nextstepaction"]
> [Cassandra gegevens importeren in Azure Cosmos-DB](cassandra-import-data.md)

