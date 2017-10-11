---
title: MongoChef gebruiken voor Azure Cosmos DB | Microsoft Docs
description: 'Informatie over het gebruik van MongoChef met een Cosmos Azure DB: API voor MongoDB-account'
keywords: mongochef
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
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 54c9799bd646b827f602e2ea2f9a15a4fc853f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>MongoChef gebruiken met een Cosmos Azure DB: API voor MongoDB-account

Verbinding maken met een Cosmos Azure DB: de API voor MongoDB-account, moet u:

* Download en installeer [MongoChef](http://3t.io/mongochef)
* Uw Azure-Cosmos-DB hebben:-API voor MongoDB account [verbindingsreeks](connect-mongodb-account.md) informatie

## <a name="create-the-connection-in-mongochef"></a>De verbinding in MongoChef maken
Toevoegen van uw Azure-Cosmos-DB: API voor MongoDB-account zodanig in het Verbindingsbeheer MongoChef de volgende stappen uitvoeren.

1. Ophalen van de Cosmos Azure DB:-API voor MongoDB-verbindingsgegevens van de instructies [hier](connect-mongodb-account.md).

    ![Schermopname van de connection string-blade](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klik op **Connect** om te openen in het Verbindingsbeheer, klikt u vervolgens op **nieuwe verbinding**

    ![Schermopname van het MongoChef Verbindingsbeheer](./media/mongodb-mongochef/ConnectionManager.png)
3. In de **nieuwe verbinding** venster op de **Server** tabblad, voert u de HOST (FQDN) van de Cosmos Azure DB: API voor MongoDB-account en de poort.

    ![Schermopname van het tabblad MongoChef connection manager-server](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. In de **nieuwe verbinding** venster op de **verificatie** Kies verificatiemodus **standaard (MONGODB CR of SCARM-SHA-1)** en voer de gebruikersnaam en wachtwoord.  Accepteer de standaard verificatie-db (admin) of geef uw eigen waarde.

    ![Schermopname van het tabblad MongoChef connection manager verificatie](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. In de **nieuwe verbinding** venster op de **SSL** tabblad, Controleer de **Gebruik SSL-protocol om verbinding te** selectievakje en de **accepteren van zelfondertekende SSL-certificaten van server**  keuzerondje.

    ![Schermopname van het tabblad MongoChef verbinding manager SSL](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klik op de **testverbinding** knop valideren van de verbindingsinformatie, klikt u op **OK** terug te keren naar de nieuwe verbinding en klik vervolgens op **opslaan**.

    ![Schermopname van het venster MongoChef test verbinding](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>MongoChef gebruiken voor het maken van een database, de verzameling en de documenten
Voor het maken van een database, verzameling en -documenten met MongoChef, moet u de volgende stappen uitvoeren.

1. In **Verbindingsbeheer**, markeer de verbinding en klikt u op **Connect**.

    ![Schermopname van het MongoChef Verbindingsbeheer](./media/mongodb-mongochef/ConnectToAccount.png)
2. Klik met de rechtermuisknop op de host en kies **Database toevoegen**.  Geef de naam van een database en klik op **OK**.

    ![Schermopname van de databaseoptie MongoChef toevoegen](./media/mongodb-mongochef/AddDatabase1.png)
3. Klik met de rechtermuisknop op de database en kies **verzameling toevoegen**.  Geef de naam van een verzameling en klik op **maken**.

    ![Schermopname van de optie verzameling MongoChef toevoegen](./media/mongodb-mongochef/AddCollection.png)
4. Klik op de **verzameling** menu item, klikt u vervolgens op **toevoegen Document**.

    ![Schermopname van de menuopdracht Document MongoChef toevoegen](./media/mongodb-mongochef/AddDocument1.png)
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
6. Deze tijd de volgende inhoud toevoegen aan een ander document.

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
* Verkennen Azure Cosmos DB: API voor MongoDB [voorbeelden](mongodb-samples.md).
