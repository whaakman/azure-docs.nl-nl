---
title: Met Azure Cosmos DB met behulp van het Mongoose-framework
description: Leer hoe u een Node.js Mongoose-app kunt verbinden met Azure Cosmos DB
services: cosmos-db
author: slyons
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: sclyon
ms.openlocfilehash: 1230e8d4352254ef637419c77b4addb7f6f6bb05
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52875124"
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure Cosmos DB: het Mongoose-framework gebruiken met Azure Cosmos DB

Deze zelfstudie laat zien hoe u het [Mongoose-framework](http://mongoosejs.com/) gebruikt bij het opslaan van gegevens in Azure Cosmos DB. In deze zelfstudie gebruiken we de MongoDB-API voor Azure Cosmos DB. Voor wie niet bekend is met Mongoose: dit is een framework voor objectmodellering voor MongoDB in Node.js en biedt een ongecompliceerde, op schema's gebaseerde oplossing voor het modelleren van uw toepassingsgegevens.

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) versie v0.10.29 of hoger.

## <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, kunt u verder naar de stap [Uw Node.js-toepassing instellen](#SetupNode). Als u de Azure Cosmos DB-emulator gebruikt, volgt u de stappen in [Azure Cosmos DB-emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Node.js-toepassing instellen](#SetupNode) te gaan.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Uw Node.js-toepassing instellen

>[!Note]
> Als u alleen de voorbeeldcode wilt doorlopen in plaats van de toepassing zelf te installeren, kopieert u het [voorbeeld](https://github.com/Azure-Samples/Mongoose_CosmosDB) dat voor deze zelfstudie is gebruikt en bouwt u de Node.js Mongoose-toepassing voor Azure Cosmos DB.

1. Voer de volgende opdracht uit in een knooppuntopdrachtprompt om een Node.js-toepassing in de map van uw keuze te maken.

    ```npm init```

    Beantwoord de vragen en uw project is klaar voor gebruik.

1. Voeg een nieuw bestand toe aan de map en noem het ```index.js```.
1. Installeer de vereiste pakketten met een van de ```npm install```-opties:
    * Mongoose: ```npm install mongoose@5 --save```

    > [!Note]
    > De onderstaande Mongoose voorbeeld van de verbinding is gebaseerd op Mongoose 5 +, die is gewijzigd sinds de eerdere versies.
    
    * Dotenv (als u uw geheimen wilt laden uit een .env-bestand): ```npm install dotenv --save```

    >[!Note]
    > Met de ```--save```-markering wordt de afhankelijkheid toegevoegd aan het package.json-bestand.

1. Importeer de afhankelijkheden in uw index.js-bestand.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Voeg uw Cosmos-DB-verbindingsreeks en de naam van de Cosmos DB toe aan het ```.env```-bestand.

    ```JavaScript
    COSMOSDB_CONNSTR=mongodb://{cosmos-user}.documents.azure.com:10255/{dbname}
    COSMODDB_USER=cosmos-user
    COSMOSDB_PASSWORD=cosmos-secret
    ```

1. Maak verbinding met Azure Cosmos DB met behulp van het Mongoose-framework door de volgende code toe te voegen aan het einde van index.js.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      }
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > Hier worden de omgevingsvariabelen geladen met behulp van process.env. {variableName} met het npm-pakket 'dotenv'.

    Zodra u verbonden bent met Azure Cosmos DB, kunt u objectmodellen gaan instellen in Mongoose.

## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Aanvullende opmerkingen voor het gebruik van Mongoose met Azure Cosmos DB

Voor elk model dat u maakt, maakt Mongoose een nieuwe MongoDB-verzameling eronder. Echter, gezien voor Azure Cosmos DB een facturatiemodel per verzameling bestaat, is dit mogelijk niet de meest kostenefficiënte aanpak als u meerdere objectmodellen hebt met weinig gegevens.

In deze zelfstudie worden beide modellen besproken. Eerst bespreken we het opslaan van één soort gegevens per verzameling. Dit is het de facto gedrag voor Mongoose.

Mongoose heeft ook een concept genaamd [discriminators](http://mongoosejs.com/docs/discriminators.html). Discriminators zijn een mechanisme voor schema-overname. Hiermee kunt u meerdere modellen met overlappende schema's boven op dezelfde onderliggende MongoDB-verzameling hebben.

U kunt de verschillende gegevensmodellen opslaan in dezelfde verzameling en vervolgens een filtercomponent gebruiken op het moment dat de query voor het ophalen van de benodigde gegevens wordt uitgevoerd.

### <a name="one-collection-per-object-model"></a>Eén verzameling per objectmodel

Standaard maakt Mongoose telkens wanneer u een objectmodel maakt een MongoDB-verzameling. In deze sectie wordt besproken hoe u dit kunt doen met MongoDB voor Azure Cosmos DB. Deze methode wordt aanbevolen met Azure Cosmos DB wanneer u objectmodellen met grote hoeveelheden gegevens hebt. Dit is het standaardmodel voor Mongoose, dus mogelijk bent u hiermee bekend als u bekend bent met Mongoose.

1. Open uw ```index.js``` opnieuw.

1. Maak de schemadefinitie voor 'Family'.

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Maak een object voor 'Family'.

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Tot slot slaan we het object op in Azure Cosmos DB. Hiermee maakt u een onderliggende verzameling.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. We gaan nu een ander schema en object maken. Deze keer maken we er een voor 'Vacation Destinations' waarin de 'families' mogelijk geïnteresseerd in zijn.
    1. Net als de laatste keer maken we het schema
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. Maak een voorbeeldobject (u kunt meerdere objecten toevoegen aan dit schema) en sla het op.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Nu ziet u in Azure Portal twee verzamelingen die in Azure Cosmos DB zijn gemaakt.

    ![Node.js-zelfstudie - schermopname van Azure Portal met een Azure Cosmos DB-account met meerdere verzamelingsnamen gemarkeerd - knooppuntdatabase][mutiple-coll]

1. Tot slot lezen we de gegevens uit Azure Cosmos DB. Aangezien we gebruikmaken van het standaardmodel van Mongoose, zijn de leesbewerkingen hetzelfde als alle andere leesbewerkingen met Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Mongoose-discriminators gebruiken voor het opslaan van gegevens in één enkele verzameling

In deze methode gebruiken we [Mongoose-discriminators](http://mongoosejs.com/docs/discriminators.html) om de kosten van elke Azure Cosmos DB-verzameling zo laag mogelijk te houden. Met discriminators kunt u een onderscheidende 'sleutel' definiëren, zodat u kunt opslaan in, onderscheid maken tussen en filteren op verschillende objectmodellen.

Hier maken we een basisobjectmodel, definiëren we een onderscheidende sleutel en voegen we 'Family' en 'VacationDestinations' toe aan het basismodel.

1. We gaan de basisconfiguratie instellen en de discriminatorsleutel definiëren.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Vervolgens gaan we het algemene objectmodel definiëren

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. We definiëren nu het model 'Family'. U ziet hier dat we ```commonModel.discriminator``` gebruiken in plaats van ```mongoose.model```. We voegen de basisconfiguratie ook toe aan het mongoose-schema. Hier is de discriminatorKey dus ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Op dezelfde manier gaan we nu een ander schema toevoegen voor de 'VacationDestinations'. Hier is de discriminatorKey ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Tot slot maken we objecten voor het model en slaan het op.
    1. We gaan objecten toevoegen aan het model 'Family'.
    ```JavaScript
    const family_common = new Family_common({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });

    family_common.save((err, saveFamily) => {
        console.log("Saved: " + JSON.stringify(saveFamily));
    });
    ```

    1. Vervolgens voegen we objecten toe aan het model 'VacationDestinations' en slaan het op.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Als u nu terug naar Azure Portal gaat, ziet u dat er slechts één verzameling met de naam ```alldata``` met zowel 'Family'- als 'VacationDestinations'-gegevens heeft.

    ![Node.js-zelfstudie - schermopname van Azure Portal met een Azure Cosmos DB-account met de verzamelingsnaam gemarkeerd - knooppuntdatabase][alldata]

1. U ziet ook dat elk object een ander kenmerk heeft dat wordt aangeroepen als ```__type```, waarmee u onderscheid tussen de twee verschillende objectmodellen kunt maken.

1. Tot slot lezen we de gegevens die zijn opgeslagen in Azure Cosmos DB. Mongoose zorgt voor het filteren van gegevens op basis van het model. U hoeft dus niets anders te doen tijdens het lezen van gegevens. Geef alleen uw model op (in dit geval ```Family_common```) en Mongoose zorgt dat wordt gefilterd op 'DiscriminatorKey'.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Zoals u ziet, is het eenvoudig om met Mongoose-discriminators te werken. Als u dus een app hebt die gebruikmaakt van het Mongoose-framework, is deze zelfstudie een manier om uw toepassing zonder te veel wijzigingen uit te voeren in de MongoDB-API voor Azure Cosmos DB.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [MongoDB-API-ondersteuning voor MongoDB-functies en -syntaxis](mongodb-feature-support.md) voor meer informatie over de MongoDB-bewerkingen, -operators, -fasen, -opdrachten en -opties die worden ondersteund door de API van Azure Cosmos DB MongoDB.

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png
