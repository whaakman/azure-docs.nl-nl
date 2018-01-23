---
title: MongoDB-verbindingsreeks voor een account voor Azure Cosmos DB | Microsoft Docs
description: Ontdek hoe u uw MongoDB-app verbinden met een Azure DB die Cosmos-account met behulp van een verbindingsreeks voor MongoDB.
keywords: mongodb-verbindingsreeks
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: e36f7375-9329-403b-afd1-4ab49894f75e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: anhoh
ms.openlocfilehash: 41c0b360b54f9a9dcd2458ce5bdc05911c4f73a5
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Verbinding maken met een toepassing MongoDB bij Azure Cosmos-DB
Ontdek hoe u uw MongoDB-app verbinden met een Azure DB die Cosmos-account met behulp van een verbindingsreeks voor MongoDB. U kunt vervolgens een Azure DB die Cosmos-database als het archief voor uw app MongoDB. 

Deze zelfstudie biedt twee manieren verbindingsreeksgegevens ophalen:

- [De snel starten methode](#QuickstartConnection), voor gebruik met .NET, Node.js, MongoDB-Shell, Java en Python-stuurprogramma's
- [De aangepaste tekenreeks verbindingsmethode](#GetCustomConnection), voor gebruik met andere stuurprogramma's

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Als u geen Azure-account hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/) nu. 
- Een Azure Cosmos DB-account. Zie voor instructies [bouwen van een web-app van de MongoDB-API met .NET- en de Azure-portal](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>De MongoDB-verbindingsreeks ophalen met behulp van de snel starten
1. In een internetbrowser, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).
2. In de **Azure Cosmos DB** blade, selecteert u de API voor MongoDB-account. 
3. Klik in het linkerdeelvenster van de accountblade **snel starten**. 
4. Kies uw platform (**.NET**, **Node.js**, **MongoDB-Shell**, **Java**, **Python**). Als er geen het hulpprogramma wordt weergegeven, of je--Documenteer we continu meer verbinding codefragmenten. Stuur een reactie hieronder op wat u zou willen zien. Lees voor meer informatie over hoe u uw eigen verbinding opgesteld, [ophalen van het account verbindingsinformatie](#GetCustomConnection).
5. Kopieer en plak het volgende codefragment in uw app MongoDB.

    ![De blade snel starten](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a>De verbindingsreeks MongoDB om aan te passen ophalen
1. In een internetbrowser, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).
2. In de **Azure Cosmos DB** blade, selecteert u de API voor MongoDB-account. 
3. Klik in het linkerdeelvenster van de accountblade **verbindingsreeks**. 
4. De **verbindingsreeks** blade wordt geopend. Alle informatie die nodig zijn voor verbinding met het account met behulp van een stuurprogramma voor MongoDB, met inbegrip van een vooraf gedefinieerde verbindingsreeks heeft.

    ![Connection String-blade](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Vereisten voor verbinding tekenreeks
> [!Important]
> Azure Cosmos DB heeft strenge beveiligingsvereisten en standaarden. Azure DB Cosmos-accounts vereist verificatie en veilige communicatie via *SSL*. 
>
>

Azure Cosmos DB ondersteunt standaard MongoDB URI indeling van de verbindingsreeks, met een aantal specifieke vereisten: Azure Cosmos DB accounts verificatie en veilige communicatie via SSL vereisen. Indeling van de verbindingsreeks is dus:

    mongodb://username:password@host:port/[database]?ssl=true

De waarden van deze tekenreeks zijn beschikbaar in de **verbindingsreeks** blade eerder weergegeven:

* Gebruikersnaam (vereist): Azure DB die Cosmos-accountnaam.
* Wachtwoord (vereist): Azure Cosmos DB accountwachtwoord.
* Host (vereist): FQDN-naam van de Azure DB die Cosmos-account.
* Poort (vereist): 10255.
* Database (optioneel): de database die de verbinding wordt gebruikt. Als er geen database wordt opgegeven, wordt de standaarddatabase is 'test'.
* SSL = true (vereist)

Neem bijvoorbeeld het account dat wordt weergegeven in de **verbindingsreeks** blade. Er is een geldige verbindingsreeks:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [Studio 3T gebruiken (MongoChef)](mongodb-mongochef.md) met een Azure Cosmos DB-API voor MongoDB-account.
* De API van Azure Cosmos DB verkennen voor MongoDB hand [voorbeelden](mongodb-samples.md).
