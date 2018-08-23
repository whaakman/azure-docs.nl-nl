---
title: Studio 3T gebruiken (MongoChef) met Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het Studio 3T gebruiken met een MongoDB-API van Azure Cosmos DB-account
keywords: mongochef, studio 3T
services: cosmos-db
author: slyons
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
ms.date: 01/18/2018
ms.author: sclyon
ms.openlocfilehash: af66f413055c01d9a4c3e94c7d3b9c5f1d5917b0
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42061320"
---
# <a name="azure-cosmos-db-use-studio-3t-with-a-mongodb-api-account"></a>Azure Cosmos DB: Gebruik Studio 3T met een MongoDB-API-account

Voor verbinding met een MongoDB-API van Azure Cosmos DB-account, moet u:

* Download en installeer [Studio 3T](https://studio3t.com/) (voorheen bekend als MongoChef)
* Uw Azure Cosmos DB hebt [verbindingsreeks](connect-mongodb-account.md) informatie voor uw MongoDB-account

## <a name="create-the-connection-in-studio-3t"></a>De verbinding in Studio 3T maken
Uw Azure Cosmos DB-account toevoegen aan de Studio 3T connection manager, moet u de volgende stappen uitvoeren:

1. Ophalen van de Azure Cosmos DB-verbindingsgegevens voor uw MongoDB-API-account met behulp van de instructies in de [verbinding maken met een MongoDB-toepassing met Azure Cosmos DB](connect-mongodb-account.md) artikel.

    ![Schermopname van de pagina verbindingsreeks](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klik op **Connect** om te openen in de Connection Manager, klikt u vervolgens op **nieuwe verbinding**

    ![Schermopname van de Studio 3T connection manager](./media/mongodb-mongochef/ConnectionManager.png)
3. In de **nieuwe verbinding** venster op de **Server** tabblad, voert u de HOST (FQDN) van het Azure Cosmos DB-account en de poort.

    ![Schermopname van het tabblad Studio 3T connection manager-server](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. In de **nieuwe verbinding** venster op de **verificatie** Kies verificatiemodus **Basic (MONGODB-CR of SCARM-SHA-1)** en voer de gebruikersnaam en wachtwoord.  Accepteer de standaard verificatie db (admin) of geef uw eigen waarde.

    ![Schermopname van het tabblad Studio 3T connection manager verificatie](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. In de **nieuwe verbinding** venster op de **SSL** tabblad en controleer de **Gebruik SSL-protocol om verbinding te** selectievakje en de **server zelf-ondertekend SSL-certificaten accepteren**  keuzerondje.

    ![Schermopname van het tabblad SSL van Studio 3T connection manager](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klik op de **testverbinding** knop valideren van de verbindingsgegevens, klikt u op **OK** terug te keren naar de nieuwe verbinding en klik vervolgens op **opslaan**.

    ![Schermopname van het venster met Studio 3T test-verbinding](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Studio 3T gebruiken voor het maken van een database, verzameling en documenten
Voor het maken van een database, verzameling en documenten Studio 3T gebruiken, moet u de volgende stappen uitvoeren:

1. In **Connection Manager**, markeert u de verbinding en klikt u op **Connect**.

    ![Schermopname van de Studio 3T connection manager](./media/mongodb-mongochef/ConnectToAccount.png)
2. Met de rechtermuisknop op de host en kies **Database toevoegen**.  Geef een databasenaam op en klik op **OK**.

    ![Schermopname van de optie Studio 3T Database toevoegen](./media/mongodb-mongochef/AddDatabase1.png)
3. Met de rechtermuisknop op de database en kies **verzameling toevoegen**.  Geef de naam van een verzameling en klik op **maken**.

    ![Schermopname van de optie Studio 3T verzameling toevoegen](./media/mongodb-mongochef/AddCollection.png)
4. Klik op de **verzameling** menu, klikt u vervolgens op **Document toevoegen**.

    ![Schermopname van het menu-item van Studio 3T Document toevoegen](./media/mongodb-mongochef/AddDocument1.png)
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

    ![Schermopname van de queryresultaten Mongo Chef](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Volgende stappen
* Azure Cosmos DB MongoDB API verkennen [voorbeelden](mongodb-samples.md).
