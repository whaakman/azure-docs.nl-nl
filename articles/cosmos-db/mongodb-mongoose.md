---
title: Met behulp van het framework Mongoose met Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het een Node.js Mongoose-app verbinden met Azure Cosmos-DB
services: cosmos-db
documentationcenter: 
author: romitgirdhar
manager: jhubbard
editor: 
ms.assetid: de5eea58-ee7c-4609-b1c9-4af3e61a5883
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: rogirdh
ms.openlocfilehash: 9878936b5dd76730633dec16b1c3a3eaac78e95a
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure Cosmos DB: Met behulp van het framework Mongoose met Azure Cosmos-DB

Deze zelfstudie laat zien hoe u de [Mongoose Framework](http://mongoosejs.com/) bij het opslaan van gegevens in Azure Cosmos DB. We gebruiken de MongoDB-API voor Azure DB die Cosmos voor dit scenario. Voor die u niet bekend Mongoose is een framework object model voor MongoDB in Node.js en biedt een ongecompliceerde, schema's gebaseerde oplossing om te modelleren uw toepassingsgegevens.

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) versie v0.10.29 of hoger.

## <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

Begin met het maken van een Azure Cosmos DB-account. Als u al een account dat u wilt gebruiken, kunt u ook doorgaan met [uw Node.js-toepassing instellen](#SetupNode). Als u de Azure-Emulator Cosmos DB gebruikt, volg de stappen in [Azure Cosmos DB Emulator](local-emulator.md) instellen van de emulator en gaat u verder met [uw Node.js-toepassing instellen](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Uw Node.js-toepassing instellen

>[!Note]
> Als u alleen scenario de voorbeeldcode in plaats van installatie van de toepassing zelf wilt, kloon de [voorbeeld](https://github.com/Azure-Samples/Mongoose_CosmosDB) voor deze zelfstudie gebruikt en uw Node.js Mongoose-toepassing voor Azure Cosmos DB bouwen.

1. Voor het maken van een Node.js-toepassing in de map van uw keuze, voer de volgende opdracht in een opdrachtprompt knooppunt.

    ```npm init```

    De vragen te beantwoorden en uw project is gereed om te gaan.

1. Voeg een nieuw bestand naar de map en noem deze ```index.js```.
1. Installeer de vereiste pakketten met een van de ```npm install``` opties:
    * Mongoose:```npm install mongoose --save```
    * Dotenv (als u uw geheimen laden uit een bestand .env wilt):```npm install dotenv --save```
    
    >[!Note]
    > De ```--save``` vlag de afhankelijkheid toegevoegd aan het package.json-bestand.

1. De afhankelijkheden in uw index.js-bestand importeren.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Uw Cosmos-DB-verbindingsreeks en de naam van de Cosmos DB naar toevoegen de ```.env``` bestand.

    ```JavaScript
    COSMOSDB_CONNSTR={Your MongoDB Connection String Here}
    COSMOSDB_DBNAME={Your DB Name Here}
    ```

1. Verbinding maken met Azure Cosmos DB met behulp van het framework Mongoose door de volgende code toe te voegen aan het einde van index.js.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb"); //Creates a new DB, if it doesn't already exist

    var db = mongoose.connection;
    db.on('error', console.error.bind(console, 'connection error:'));
    db.once('open', function() {
    console.log("Connected to DB");
    });
    ```
    >[!Note]
    > Hier worden de omgevingsvariabelen geladen met process.env. {variableName} met de npm-pakket 'dotenv'.

    Zodra u met Azure Cosmos DB verbonden bent, kunt u nu instellen van de objectmodellen in Mongoose starten.
    
## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Aanvullende opmerkingen voor het gebruik van Mongoose met Azure Cosmos-DB

Mongoose maakt voor elk model die u maakt een nieuwe verzameling MongoDB onder de behandelt. Echter, gezien het facturering model per-verzameling van Azure Cosmos DB, niet worden de meest voordelige manier om door te gaan, als u hebt meerdere objectmodellen die zeer worden ingevuld.

Dit scenario omvat beide modellen. Aan bod eerst de procedure voor het opslaan van één type gegevens per verzameling. Dit is het gedrag defacto voor Mongoose.

Mongoose heeft ook een concept aangeroepen [Discriminators](http://mongoosejs.com/docs/discriminators.html). Discriminators zijn een mechanisme voor schema overname. Hiermee kunt u meerdere modellen met overlappende schema's boven op de dezelfde onderliggende MongoDB-verzameling.

U kunt de verschillende gegevensmodellen opslaan in dezelfde verzameling en vervolgens een filter-component gebruiken op het moment dat de query voor het ophalen van de benodigde gegevens.

### <a name="one-collection-per-object-model"></a>Een verzameling per objectmodel

Het standaardgedrag van Mongoose is het maken van een verzameling MongoDB telkens wanneer u een objectmodel maakt. Deze sectie behandelt hoe om dit te bereiken met MongoDB voor Azure Cosmos DB. Deze methode wordt aanbevolen met Azure Cosmos DB wanneer u de netwerkobjectmodellen met grote hoeveelheden gegevens hebt. Dit is de standaardinstelling operationele model voor Mongoose, dus u mogelijk bekend zijn met dit als u bekend met Mongoose bent.

1. Open uw ```index.js``` opnieuw.

1. De schemadefinitie voor 'Familie' maken.

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

1. Maak een object voor 'Familie'.

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

1. Tot slot gaan we slaat u het object bij Azure Cosmos DB. Hiermee maakt u een verzameling onder het behandelt.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Nu gaan we een ander schema en object maken. Deze tijd, maken we een voor 'vakantie bestemmingen, die de families mogelijk geïnteresseerd zijn in.
    1. Net als de laatste keer maken we het schema
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. Een voorbeeld-object (u kunt meerdere objecten toevoegen aan dit schema) maken en op te slaan.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Nu ziet u in de Azure-portal gaat, twee verzamelingen in Azure Cosmos-database gemaakt.

    ![Node.js-zelfstudie - schermopname van de Azure-portal een Azure DB die Cosmos-account wordt weergegeven met de naam van de verzameling gemarkeerd - knooppuntdatabase][alldata]

1. We lezen ten slotte de gegevens uit Azure Cosmos DB. Aangezien we maken gebruik van het standaard Mongoose operationele model, wordt gelezen gegevens zijn hetzelfde als alle andere leesbewerkingen met Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Met behulp van Mongoose discriminators voor het opslaan van gegevens in één enkele verzameling

In deze methode gebruiken we [Mongoose Discriminators](http://mongoosejs.com/docs/discriminators.html) voor het optimaliseren voor de kosten van elke Azure DB die Cosmos-verzameling. Discriminators kunt u voor het definiëren van een verschillende 'sleutel', zodat u kunt opslaan, onderscheiden en filter op een ander objectmodellen.

Hier maken we een basisobject model, definieert u een verschillende sleutel en 'Familie' en 'VacationDestinations' als een uitbreiding toevoegen aan het basismodel.

1. Laten we de base config instellen en definieer de discriminator-sleutel.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Vervolgens laten we het algemene objectmodel definiëren

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Nu definiëren we het model 'Familie'. U ziet hier dat we gebruiken ```commonModel.discriminator``` in plaats van ```mongoose.model```. We zijn bovendien ook de basis config toevoegen aan het schema mongoose. Dus hier de discriminatorKey is ```FamilyType```.

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

1. Op deze manier gaan we een ander schema deze tijd toevoegen voor de 'VacationDestinations'. Hier de DiscriminatorKey is ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Tot slot gaan we-objecten gemaakt voor het model en opslaan.
    1. Laten we objecten toevoegen aan het model 'Familie'.
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

    1. Vervolgens laten we objecten toevoegen aan het model 'VacationDestinations' en opslaan.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Nu, als u terug naar de Azure-portal gaat, ziet u dat er slechts één verzameling met de naam ```alldata``` met zowel 'Familie' en 'VacationDestinations'-gegevens.

    ![Node.js-zelfstudie - schermopname van de Azure-portal een Azure DB die Cosmos-account wordt weergegeven met de naam van de verzameling gemarkeerd - knooppuntdatabase][mutiple-coll]

1. U ziet ook dat elk object een ander kenmerk wel heeft ```__type```, welke u onderscheid tussen de twee verschillende objectmodellen maken help.

1. Tot slot gaan we de gegevens die zijn opgeslagen in Azure Cosmos DB lezen. Mongoose zorgt voor het filteren van gegevens op basis van het model. U moet dus niets verschillende bij het lezen van gegevens. Geef alleen uw model (in dit geval ```Family_common```) en Mongoose ingangen filteren op 'DiscriminatorKey'.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Zoals u ziet, is het eenvoudig te werken met Mongoose discriminators. Als u een app die gebruikmaakt van het framework Mongoose hebt, is deze zelfstudie een manier om u voor uw toepassing en wordt uitgevoerd op de MongoDB-API voor Azure Cosmos DB zonder te veel wijzigingen.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de MongoDB bewerkingen, operators, fasen, opdrachten en opties die worden ondersteund door de API van Azure Cosmos DB MongoDB in [MongoDB-API-ondersteuning voor MongoDB-functies en de syntaxis](mongodb-feature-support.md).

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png