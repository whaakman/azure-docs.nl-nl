---
title: Robo 3T gebruiken voor verbinding met Azure Cosmos DB
description: Meer informatie over het verbinding maken met Azure Cosmos DB met behulp van Robo 3T en Azure Cosmos DB-API voor MongoDB
keywords: robomongo
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/26/2018
ms.author: rimman
ms.openlocfilehash: af601075b8cc774654a9f2dbf83193bf9701166b
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789446"
---
# <a name="use-robo-3t-with-azure-cosmos-dbs-api-for-mongodb"></a>Robo 3T gebruiken met Azure Cosmos DB-API voor MongoDB

Voor verbinding met de Cosmos-account met behulp van Robo 3T, moet u:

* Download en installeer [Robo 3T](https://robomongo.org/)
* Uw Cosmos DB hebt [verbindingsreeks](connect-mongodb-account.md) informatie

## <a name="connect-using-robo-3t"></a>Verbinding maken met behulp van Robo 3T
Als u wilt uw Cosmos-account toevoegen aan de Robo 3T connection manager, moet u de volgende stappen uitvoeren:

1. Ophalen van de verbindingsgegevens voor uw Cosmos-account dat is geconfigureerd met behulp van de instructies van Azure Cosmos DB API MongoDB [hier](connect-mongodb-account.md).

    ![Schermopname van de blade verbindingsreeks](./media/mongodb-robomongo/connectionstringblade.png)
2. Run *Robomongo.exe*

3. Klik op de verbindingsknop onder **bestand** voor het beheren van uw verbindingen. Klik vervolgens op **maken** in de **MongoDB clientverbindingen** venster, die wordt geopend de **verbindingsinstellingen** venster.

4. In de **verbindingsinstellingen** venster, kies een naam. Zoek vervolgens de **Host** en **poort** uit de verbindingsinformatie in stap 1 en voert u deze in **adres** en **poort**, respectievelijk.

    ![Schermopname van de verbindingen voor het beheren van Robomongo](./media/mongodb-robomongo/manageconnections.png)
5. Op de **verificatie** tabblad **verificatie uitvoeren**. Voer vervolgens de Database (de standaardwaarde is *Admin*), **gebruikersnaam** en **wachtwoord**.
Beide **gebruikersnaam** en **wachtwoord** kunt u vinden in de verbindingsinformatie in stap 1.

    ![Schermopname van het tabblad Robomongo verificatie](./media/mongodb-robomongo/authentication.png)
6. Op de **SSL** tabblad controle **Gebruik SSL-protocol**, wijzig de **verificatiemethode** naar **een zelfondertekend certificaat**.

    ![Schermopname van het SSL-tabblad Robomongo](./media/mongodb-robomongo/SSL.png)
7. Klik tot slot, **Test** om te controleren of u geen verbinding maken, klikt u vervolgens **opslaan**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [Studio 3T gebruiken](mongodb-mongochef.md) met Azure Cosmos DB-API voor MongoDB.
- Verken MongoDB [voorbeelden](mongodb-samples.md) met Azure Cosmos DB-API voor MongoDB.
