---
title: 'Azure Cosmos DB: een Flask web-app ontwikkelen met Python en de MongoDB-API van Azure Cosmos DB | Microsoft Docs'
description: Biedt een voorbeeld van Python Flask-code dat u kunt gebruiken om verbinding te maken met de MongoDB-API van Azure Cosmos DB en er query’s op uit te voeren
services: cosmos-db
documentationcenter: ''
author: heatherbshapiro
manager: kfile
ms.assetid: ''
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 10/2/2017
ms.author: hshapiro
ms.openlocfilehash: 095cc724beb9f35896bd02e299523839a9f43f4b
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
ms.locfileid: "33885099"
---
# <a name="azure-cosmos-db-build-a-flask-app-with-the-mongodb-api"></a>Azure Cosmos DB: een Flask-app maken met de MongoDB-API

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB.

In deze snelstartgids wordt gebruikgemaakt van het volgende [Flask voorbeeld](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample) en wordt gedemonstreerd hoe u een eenvoudige Flask-app kunt maken met de [Azure Cosmos DB-emulator](local-emulator.md) en de [MongoDB-API](mongodb-introduction.md) van Azure Cosmos DB in plaats van met MongoDB.

## <a name="prerequisites"></a>Vereisten

- Download de [Azure Cosmos DB-emulator](local-emulator.md). De emulator wordt momenteel alleen ondersteund op Windows. In het voorbeeld wordt getoond hoe het Flask-voorbeeld kan worden gebruikt met een productie-sleutel van Azure. Dit kan op elk platform worden uitgevoerd.

- Als u Visual Studio Code nog niet hebt geïnstalleerd, kunt u snel [VS Code](https://code.visualstudio.com/Download) installeren voor uw platform (Windows, Mac, Linux).

- Zorg ervoor dat u ondersteuning voor de taal Python toevoegt door een van de populaire Python-extensies te installeren.
    1. Selecteer een extensie.
    2. Installeer de extensie door `ext install` in het opdrachtenpalet `Ctrl+Shift+P` te typen.

    Voor de voorbeelden in dit document wordt gebruikgemaakt van de populaire en volledig functionele [Python-extensie](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python) van Don Jayamanne.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Flask MongoDB API-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken.

1. Open een opdrachtprompt, maak een nieuwe map met de naam git-samples en sluit vervolgens de opdrachtprompt.

    ```bash
    md "C:\git-samples"
    ```

2. Open een git-terminalvenster, bijvoorbeeld git bash, en gebruik de `cd`-opdracht om naar de nieuwe map te gaan voor het installeren van de voorbeeld-app.

    ```bash
    cd "C:\git-samples"
    ```

3. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git
    ```
3. Voer de volgende opdracht uit om de Python-modules te installeren.

    ```bash 
    pip install -r .\requirements.txt
    ```
4. Open de map in Visual Studio Code.

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources in de code worden gemaakt, kunt u de volgende codefragmenten bekijken. Sla dit anders over en ga naar [De web-app uitvoeren](#run-the-web-app). 

De volgende codefragmenten zijn allemaal afkomstig uit het bestand app.py en gebruiken de verbindingsreeks voor de lokale Azure Cosmos DB-emulator. Het wachtwoord moet worden opgesplitst, zoals hieronder wordt weergegeven, omdat de slashes anders niet kunnen worden geparseerd.

* Initialiseer de MongoDB-client, haal de database op en doorloop de verificatie.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Haal de verzameling op of maak er een als er nog geen is.

    ```python
    todos = db.todo #Select the collection
    ```

* De app maken

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## <a name="run-the-web-app"></a>De web-app uitvoeren

1. Controleer of de Azure Cosmos DB-emulator wordt uitgevoerd.

2. Open een terminalvenster en ga met `cd` naar de map waarin de app wordt opgeslagen.

3. Stel vervolgens de omgevingsvariabele voor de Flask-app in met `set FLASK_APP=app.py` of `export FLASK_APP=app.py` als u een Mac gebruikt.

4. Voer de app uit met `flask run` en blader naar [http://127.0.0.1:5000/](http://127.0.0.1:5000/).

5. Voeg taken toe en verwijder ze, en kijk hoe ze zijn toegevoegd en gewijzigd in de verzameling.

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Als u wilt testen hoe de code met een actief Azure Cosmos DB-account werkt, gaat u naar Azure Portal om een account te maken en haalt u uw verbindingsreeksgegevens op. Kopieer deze vervolgens in de app.

1. Klik in [Azure Portal](http://portal.azure.com/), in uw Azure Cosmos DB-account, in het linker navigatiegedeelte op **Verbindingsreeks** en klik vervolgens op **Sleutels voor lezen/schrijven**. U gebruikt de kopieerknoppen aan de rechterkant van het scherm om de Gebruikersnaam, het Wachtwoord, en de Host naar het bestand Dal.cs te kopiëren. Dit doet u in de volgende stap.

2. Open het bestand **app.py** in de hoofdmap.

3. Kopieer de waarde van **gebruikersnaam** van de portal (gebruik hiervoor de kopieerknop) en maak hiervan de waarde van **gebruikersnaam** in het bestand **app.py**.

4. Kopieer de waarde van **verbindingsreeks** van de portal en maak hiervan de waarde van de MongoClient in het bestand **app.py**.

5. Kopieer ten slotte de waarde van **wachtwoord** van de portal en maak hiervan de waarde van **wachtwoord** in het bestand **app.py**.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. U kunt deze nu net als voorheen uitvoeren.

## <a name="deploy-to-azure"></a>Implementeren in Azure

Als u deze app wilt implementeren, kunt u een nieuwe web-app maken in Azure en continue implementatie inschakelen met een fork van deze Github-opslagplaats. Volg deze [zelfstudie](https://docs.microsoft.com/azure/app-service-web/app-service-continuous-deployment) voor het instellen van continue implementatie met Github in Azure.

Wanneer u Azure implementeert, moet u de sleutels van uw toepassing verwijderen en controleren of de onderstaande sectie als opmerking is opgenomen:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

Vervolgens moet u uw MONGOURL, MONGO_PASSWORD en MONGO_USERNAME toevoegen aan de toepassingsinstellingen. In deze [zelfstudie](https://docs.microsoft.com/azure/app-service-web/web-sites-configure#application-settings) vindt u meer informatie over toepassingsinstellingen in Azure Web Apps.

Als u van deze opslagplaats geen fork wilt maken, kunt u ook op de knop Implementeren naar Azure hieronder klikken. Vervolgens gaat u naar Azure en stelt u daar de toepassingsinstellingen in op basis van de gegevens van uw Cosmos DB-account.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="http://azuredeploy.net/deploybutton.png"/>
</a>

> [!NOTE]
> Als u van plan bent uw code in Github of andere opties voor bronbeheer op te slaan, moet u uw verbindingsreeksen uit de code verwijderen. Ze kunnen ook worden ingesteld met toepassingsinstellingen voor de web-app.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u geleerd hoe een Azure Cosmos DB-account kunt maken en een Flask-app kunt uitvoeren met de API voor MongoDB. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren.

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB voor de MongoDB-API](mongodb-migrate.md)
