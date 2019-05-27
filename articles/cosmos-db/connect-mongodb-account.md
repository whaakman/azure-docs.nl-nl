---
title: Verbinding maken met een MongoDB-toepassing met Azure Cosmos DB
description: Leer hoe u uw MongoDB-app verbinden met Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: dabce19d60a380b47d3583dedb5c11303f416ce7
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978712"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Verbinding maken met een MongoDB-toepassing met Azure Cosmos DB
Leer hoe u uw MongoDB-app verbinden met een Azure Cosmos DB met behulp van een MongoDB-verbindingsreeks. U kunt vervolgens een Azure Cosmos-database gebruiken als de gegevens opslaan voor uw MongoDB-app. 

In deze zelfstudie biedt twee manieren om de verbindingsreeksinformatie ophalen:

- [De Quick Start-methode](#QuickstartConnection), voor gebruik met .NET, Node.js, MongoDB-Shell, Java en Python-stuurprogramma's
- [De aangepaste tekenreeks verbindingsmethode](#GetCustomConnection), voor gebruik met andere stuurprogramma's

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Als u een Azure-account niet hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/) nu. 
- Een Cosmos-account. Zie voor instructies [bouwen van een web-app met behulp van Azure Cosmos DB-API voor MongoDB en .NET SDK](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>De MongoDB-verbindingsreeks ophalen met behulp van Snel starten
1. In een internetbrowser, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).
2. In de **Azure Cosmos DB** blade, selecteer de API. 
3. Klik in het linkerdeelvenster van de accountblade op **Quick start**. 
4. Kies uw platform (**.NET**, **Node.js**, **MongoDB-Shell**, **Java**, **Python**). Als u niet uw stuurprogramma of hulpprogramma wordt vermeld ziet, geen zorgen, documenteren we voortdurend meer verbindingscodefragmenten. Stuur een reactie hieronder op wat u graag zou willen zien. Als u wilt weten hoe ze moesten uw eigen verbinding maken, lezen [van het account verbindingsreeksgegevens downloaden](#GetCustomConnection).
5. Kopieer en plak het codefragment in uw MongoDB-app.

    ![Quick start-blade](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a> Haal de MongoDB-verbindingsreeks om aan te passen
1. In een internetbrowser, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).
2. In de **Azure Cosmos DB** blade, selecteer de API. 
3. Klik in het linkerdeelvenster van de accountblade op **Connection String**. 
4. De **Connection String** blade wordt geopend. Alle informatie die verbinding maken met het account met behulp van een stuurprogramma voor MongoDB, met inbegrip van een vooraf gedefinieerde verbindingsreeks nodig heeft.

    ![De blade Verbindingsreeks](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Vereisten voor verbinding tekenreeks
> [!Important]
> Voor Azure Cosmos DB gelden strenge beveiligingsvereisten en -normen. Azure Cosmos DB-accounts vereist verificatie en beveiligde communicatie via *SSL*. 
>
>

Azure Cosmos DB ondersteunt standaard MongoDB URI indeling van de verbindingsreeks, met een aantal specifieke vereisten: Azure Cosmos DB-accounts is verificatie en beveiligde communicatie via SSL vereist. De indeling van de verbindingsreeks is dus:

    mongodb://username:password@host:port/[database]?ssl=true

De waarden van deze tekenreeks zijn beschikbaar in de **Connection String** blade eerder weergegeven:

* Gebruikersnaam (vereist): Cosmos-accountnaam.
* Wachtwoord (vereist): Cosmos-accountwachtwoord.
* Host (vereist): FQDN-naam van de Cosmos-account.
* Poort (vereist): 10255.
* Database (optioneel): De database die gebruikmaakt van de verbinding. Als er geen database is opgegeven, wordt de standaarddatabase is 'test'.
* SSL = true (vereist)

Neem bijvoorbeeld het account dat wordt weergegeven in de **Connection String** blade. Er is een geldige verbindingsreeks:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Studio 3T](mongodb-mongochef.md) met de API voor MongoDB van Azure Cosmos DB.
- Meer informatie over het [gebruik van Robo 3T](mongodb-robomongo.md) met de API voor MongoDB van Azure Cosmos DB.
- Verken [voorbeelden](mongodb-samples.md) van MongoDB met de API voor MongoDB van Azure Cosmos DB.
