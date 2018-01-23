---
title: Gebruik Studio 3T (MongoChef) met Azure Cosmos DB | Microsoft Docs
description: Informatie over het gebruik van Studio 3T met een account voor Azure Cosmos DB MongoDB-API
keywords: mongochef, studio 3T
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: anhoh
ms.openlocfilehash: 0341fbf668bbbc8f02e78bc1f6c7a00ecc939cc2
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-cosmos-db-use-studio-3t-with-a-mongodb-api-account"></a>Azure Cosmos DB: Gebruik Studio 3T met een account met MongoDB-API

Voor verbinding met een Azure Cosmos DB MongoDB-API-account, moet u het volgende doen:

* Download en installeer [Studio 3T](https://studio3t.com/) (voorheen bekend als MongoChef)
* Hebt u uw Azure-Cosmos-DB [verbindingsreeks](connect-mongodb-account.md) informatie voor uw account MongoDB

## <a name="create-the-connection-in-studio-3t"></a>De verbinding in Studio 3T maken
Als u wilt uw Azure DB die Cosmos-account toevoegen aan de Studio 3T connection manager, moet u de volgende stappen uitvoeren:

1. Ophalen van de Azure Cosmos DB-verbindingsgegevens voor de MongoDB-API-account van de instructies in de [verbinding maken met een toepassing MongoDB bij Azure Cosmos DB](connect-mongodb-account.md) artikel.

    ![Schermopname van de pagina van de tekenreeks verbinding](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klik op **Connect** om te openen in het Verbindingsbeheer, klikt u vervolgens op **nieuwe verbinding**

    ![Schermopname van de Studio 3T connection manager](./media/mongodb-mongochef/ConnectionManager.png)
3. In de **nieuwe verbinding** venster op de **Server** tabblad, voert u de HOST (FQDN) van de Azure DB die Cosmos-account en de poort.

    ![Schermopname van het tabblad Studio 3T connection manager-server](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. In de **nieuwe verbinding** venster op de **verificatie** Kies verificatiemodus **Basic (MONGODB CR of SCARM-SHA-1)** en voer de gebruikersnaam en wachtwoord.  Accepteer de standaard verificatie-db (admin) of geef uw eigen waarde.

    ![Schermopname van het tabblad Studio 3T connection manager verificatie](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. In de **nieuwe verbinding** venster op de **SSL** tabblad, Controleer de **Gebruik SSL-protocol om verbinding te** selectievakje en de **accepteren van zelfondertekende SSL-certificaten van server**  keuzerondje.

    ![Schermopname van het tabblad Studio 3T verbinding manager SSL](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klik op de **testverbinding** knop valideren van de verbindingsinformatie, klikt u op **OK** terug te keren naar de nieuwe verbinding en klik vervolgens op **opslaan**.

    ![Schermopname van het venster Studio 3T test verbinding](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Studio-3T gebruiken voor het maken van een database, de verzameling en de documenten
Voor het maken van een database, verzameling en -documenten met Studio 3T, moet u de volgende stappen uitvoeren:

1. In **Verbindingsbeheer**, markeer de verbinding en klikt u op **Connect**.

    ![Schermopname van de Studio 3T connection manager](./media/mongodb-mongochef/ConnectToAccount.png)
2. Met de rechtermuisknop op de host en kies **Database toevoegen**.  Geef de naam van een database en klik op **OK**.

    ![Schermopname van de optie Studio 3T Database toevoegen](./media/mongodb-mongochef/AddDatabase1.png)
3. Met de rechtermuisknop op de database en kies **verzameling toevoegen**.  Geef de naam van een verzameling en klik op **maken**.

    ![Schermopname van de optie Studio 3T verzameling toevoegen](./media/mongodb-mongochef/AddCollection.png)
4. Klik op de **verzameling** menu item, klikt u vervolgens op **toevoegen Document**.

    ![Schermopname van de menuopdracht Studio 3T Document toevoegen](./media/mongodb-mongochef/AddDocument1.png)
5. Plak het volgende in het dialoogvenster Document toevoegen en klik vervolgens op **toevoegen Document**.

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
6. Toevoegen van een ander document, ditmaal met de volgende inhoud:

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
7. Een voorbeeldquery uitvoeren. Bijvoorbeeld zoeken families met de naam van de laatste 'Andersen' en de bovenliggende groepen en velden voor status geretourneerd.

    ![Schermopname van het Mongo Chef queryresultaten](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Volgende stappen
* MongoDB-API van Azure Cosmos DB verkennen [voorbeelden](mongodb-samples.md).
