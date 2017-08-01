---
title: 'Azure Cosmos DB: een toepassing bouwen met Python en de DocumentDB API | Microsoft Docs'
description: "Biedt een voorbeeld van Python-code dat u kunt gebruiken om verbinding te maken met de DocumentDB API van Azure Cosmos DB en er query’s op uit te voeren"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/13/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 08d467ea27484e7d1d07d6c21b2e04b6525fbcd8
ms.contentlocale: nl-nl
ms.lasthandoff: 06/01/2017


---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-python-and-the-azure-portal"></a>Azure Cosmos DB: een DocumentDB API-app bouwen met Python en Azure Portal

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u een Azure Cosmos DB-account, een documentdatabase en een verzameling kunt maken met behulp van Azure Portal. Vervolgens ontwikkelt u een console-app die is gebouwd op de [DocumentDB Python-API](documentdb-sdk-python.md) en voert u deze uit.

## <a name="prerequisites"></a>Vereisten

* Voordat u met dit voorbeeld aan de slag gaat, moet u aan de volgende vereisten voldoen:
    * [Visual Studio 2015](http://www.visualstudio.com/) of hoger.
    * Python Tools for Visual Studio van [GitHub](http://microsoft.github.io/PTVS/). In deze zelfstudie wordt gebruikgemaakt van Python Tools for VS 2015.
    * Python 2.7 van [python.org](https://www.python.org/downloads/release/python-2712/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Een verzameling toevoegen

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een DocumentDB API-app klonen vanuit GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een venster in een git-terminal zoals git bash en `cd` naar een werkmap.  

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand DocumentDBGetStarted.py en u zult zien dat deze regels code de Azure Cosmos DB-resources maken. 


* De DocumentClient wordt geïnitialiseerd.

    ```python
    # Initialize the Python DocumentDB client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* Er wordt een nieuwe database gemaakt.

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })
    ```

* Er wordt een nieuwe verzameling gemaakt.

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)
    ```

* Er wordt een aantal documenten gemaakt.

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* Een query wordt uitgevoerd met behulp van SQL.

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Klik in [Azure Portal](http://portal.azure.com/), in uw Azure Cosmos DB-account, in het linker navigatiegedeelte op **Sleutels** en klik vervolgens op **Sleutels voor lezen/schrijven**. In de volgende stap gebruikt u de kopieerknoppen aan de rechterkant van het scherm om de URI en primaire sleutel in het bestand `DocumentDBGetStarted.py` te kopiëren.

    ![Een toegangssleutel bekijken en kopiëren in Azure Portal, blade Sleutels](./media/create-documentdb-dotnet/keys.png)

2. Open het bestand `DocumentDBGetStarted.py`. 

3. Kopieer uw URI-waarde vanaf de portal (met de kopieerknop) en geef deze als waarde aan de eindpuntsleutel in `DocumentDBGetStarted.py`. 

    `config.ENDPOINT : "https://FILLME.documents.azure.com"`

4. Kopieer vervolgens de waarde van uw PRIMAIRE SLEUTEL vanaf de portal en geef deze als waarde aan de `config.MASTERKEY` in `DocumentDBGetStarted.py`. U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

    `config.MASTERKEY : "FILLME"`
    
## <a name="run-the-app"></a>De app uitvoeren
1. Klik in Visual Studio met de rechtermuisknop op het project in **Solution Explorer**, selecteer de huidige Python-omgeving en klik er met de rechtermuisknop op.

2. Selecteer Python-pakket installeren en typ vervolgens **pydocumentdb**

3. Druk op F5 om de toepassing uit te voeren. Uw app wordt in uw browser weergegeven. 

U kunt nu teruggaan naar Data Explorer en deze nieuwe gegevens bekijken, wijzigen, een query erop uitvoeren of er iets anders mee doen. 

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal:

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hoe u een verzameling kunt maken met Data Explorer en hebt u een app uitgevoerd. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB voor de DocumentDB API](import-data.md)



