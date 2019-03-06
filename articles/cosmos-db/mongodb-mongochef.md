---
title: Verbinding maken met Azure Cosmos DB-API voor MongoDB Studio 3T gebruiken
titleSuffix: Azure Cosmos DB
description: Leer hoe u verbinding maken met Cosmos DB met behulp van Studio 3T en Azure Cosmos DB-API voor MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.custom: seodec18
ms.openlocfilehash: 9b0a6393d2372c831fdc964dee18acbccfd39a77
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57449373"
---
# <a name="connect-to-cosmos-account-using-studio-3t"></a>Verbinding maken met Cosmos-account Studio 3T gebruiken

Voor verbinding met een Cosmos-account met behulp van Azure Cosmos DB-API voor MongoDB, moet u:

* Download en installeer [Studio 3T](https://studio3t.com/)
* Uw Cosmos DB hebt [verbindingsreeks](connect-mongodb-account.md) informatie

## <a name="create-the-connection-in-studio-3t"></a>De verbinding in Studio 3T maken
Als u wilt uw Cosmos-account toevoegen aan de Studio 3T connection manager, moet u de volgende stappen uitvoeren:

1. Ophalen van de verbindingsgegevens voor uw Cosmos-account dat is geconfigureerd met Azure Cosmos DB-API voor MongoDB met behulp van de instructies in de [verbinding maken met een MongoDB-toepassing met Azure Cosmos DB](connect-mongodb-account.md) artikel.

    ![Schermafbeelding van de pagina verbindingsreeks](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klik op **Connect** om te openen in de Connection Manager, klikt u vervolgens op **nieuwe verbinding**

    ![Schermafbeelding van de Studio 3T connection manager](./media/mongodb-mongochef/ConnectionManager.png)
3. In de **nieuwe verbinding** venster op de **Server** tabblad, voert u de HOST (FQDN) van het Azure Cosmos DB-account en de poort.

    ![Schermopname van het tabblad Studio 3T connection manager-server](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. In de **nieuwe verbinding** venster op de **verificatie** Kies verificatiemodus **Basic (MONGODB-CR of SCARM-SHA-1)** en voer de gebruikersnaam en wachtwoord.  Accepteer de standaard verificatie db (admin) of geef uw eigen waarde.

    ![Schermopname van het tabblad Studio 3T connection manager verificatie](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. In de **nieuwe verbinding** venster op de **SSL** tabblad en controleer de **Gebruik SSL-protocol om verbinding te** selectievakje en de **server zelf-ondertekend SSL-certificaten accepteren**  keuzerondje.

    ![Schermopname van het tabblad SSL van Studio 3T connection manager](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klik op de **testverbinding** knop valideren van de verbindingsgegevens, klikt u op **OK** terug te keren naar de nieuwe verbinding en klik vervolgens op **opslaan**.

    ![Schermafbeelding van de Studio 3T verbindingsvenster testen](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Studio 3T gebruiken voor het maken van een database, verzameling en documenten
Voor het maken van een database, verzameling en documenten Studio 3T gebruiken, moet u de volgende stappen uitvoeren:

1. In **Connection Manager**, markeert u de verbinding en klikt u op **Connect**.

    ![Schermafbeelding van de Studio 3T connection manager](./media/mongodb-mongochef/ConnectToAccount.png)
2. Met de rechtermuisknop op de host en kies **Database toevoegen**.  Geef een databasenaam op en klik op **OK**.

    ![Schermafbeelding van de optie Studio 3T Database toevoegen](./media/mongodb-mongochef/AddDatabase1.png)
3. Met de rechtermuisknop op de database en kies **verzameling toevoegen**.  Geef de naam van een verzameling en klik op **maken**.

    ![Schermafbeelding van de optie Studio 3T verzameling toevoegen](./media/mongodb-mongochef/AddCollection.png)
4. Klik op de **verzameling** menu, klikt u vervolgens op **Document toevoegen**.

    ![Schermafbeelding van de menuopdracht Studio 3T Document toevoegen](./media/mongodb-mongochef/AddDocument1.png)
5. Plak het volgende in het dialoogvenster Document toevoegen en klik vervolgens op **Document toevoegen**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Voeg een ander document, ditmaal met de volgende inhoud:

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Een voorbeeld-query uitvoeren. Bijvoorbeeld zoeken naar gezinnen zijn met de achternaam 'Andersen' en de ouders en statusvelden retourneren.

    ![Schermafbeelding van Mongo Chef-queryresultaten](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Robo 3T](mongodb-robomongo.md) met de API voor MongoDB van Azure Cosmos DB.
- Verken [voorbeelden](mongodb-samples.md) van MongoDB met de API voor MongoDB van Azure Cosmos DB.
