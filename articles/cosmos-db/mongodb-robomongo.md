---
title: Robomongo gebruiken voor Azure Cosmos DB
description: 'Meer informatie over het Robomongo gebruiken met een Azure Cosmos DB: API voor MongoDB-account'
keywords: robomongo
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: 78f0158c9a80a60717b81b4788531c7efd979111
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863802"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Robomongo gebruiken met een Azure Cosmos DB: API voor MongoDB-account
Verbinding maken met een Azure Cosmos DB: API voor MongoDB-account robomongo gebruiken, moet u:

* Download en installeer [Robomongo](https://robomongo.org/)
* Hebt u uw Azure Cosmos DB: API voor MongoDB-account [verbindingsreeks](connect-mongodb-account.md) informatie

## <a name="connect-using-robomongo"></a>Verbinding maken via Robomongo
Toevoegen van uw Azure Cosmos DB: API voor MongoDB-account aan de Robomongo MongoDB-clientverbindingen, de volgende stappen uitvoeren.

1. Ophalen van uw Azure Cosmos DB: API voor MongoDB-account verbindingsinformatie met behulp van de instructies [hier](connect-mongodb-account.md).

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
* Azure Cosmos DB verkennen: API voor MongoDB [voorbeelden](mongodb-samples.md).
