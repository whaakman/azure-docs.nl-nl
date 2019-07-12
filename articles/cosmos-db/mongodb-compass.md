---
title: Verbinding maken met Azure Cosmos DB met behulp van kompas
description: Informatie over het gebruik van MongoDB kompas opslaan en beheren van gegevens in Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 06/24/2019
author: roaror
ms.author: roaror
ms.openlocfilehash: 102d3fdc2e36f812e9a86286383a06f9930a1947
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67665917"
---
# <a name="use-mongodb-compass-to-connect-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB kompas verbinding maken met Azure Cosmos DB-API voor MongoDB gebruiken 

In deze zelfstudie ziet u hoe u [MongoDB kompas](https://www.mongodb.com/products/compass) bij het opslaan van en/of beheren van gegevens in Cosmos DB. We gebruiken de Azure Cosmos DB-API voor MongoDB voor deze procedure. Voor wie niet bekend is kompas een GUI voor MongoDB. Het wordt vaak gebruikt om uw gegevens visualiseren, ad-hoc-query's, samen met het beheren van uw gegevens uitvoeren. 

Cosmos DB is wereldwijd gedistribueerde multi-modeldatabase-service van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query's op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Cosmos DB.


## <a name="pre-requisites"></a>Vereisten 
Voor verbinding met uw Cosmos DB-account met behulp van Robo 3T, moet u:

* Download en installeer [kompas](https://www.mongodb.com/download-center/compass?jmp=hero)
* Uw Cosmos DB hebt [verbindingsreeks](connect-mongodb-account.md) informatie

## <a name="connect-to-cosmos-dbs-api-for-mongodb"></a>Verbinding maken met Cosmos DB-API voor MongoDB 
Voor uw Cosmos DB-account verbinding met kompas, voert u de onderstaande stappen te volgen:

1. Ophalen van de verbindingsgegevens voor uw Cosmos-account dat is geconfigureerd met behulp van de instructies van Azure Cosmos DB API MongoDB [hier](connect-mongodb-account.md).

    ![Schermafbeelding van de blade verbindingsreeks](./media/mongodb-compass/mongodb-compass-connection.png)

2. Klik op de knop met de melding dat **naar Klembord kopiëren** naast uw **primaire/secundaire verbindingsreeks** in Cosmos DB. Op deze knop klikt, wordt de volledige verbindingsreeks naar het Klembord te kopiëren. 

    ![Schermopname van het kopiëren naar Klembord-knop](./media/mongodb-compass/mongodb-connection-copy.png)

3. Kompasrichting Open op uw bureaublad/computer en klik op **Connect** en vervolgens **verbinden...** . 

4. Een verbinding wordt automatisch gedetecteerd door kompas tekenreeks in het Klembord en wordt gevraagd om te vragen of u wilt gebruiken om verbinding te maken. Klik op **Ja** zoals wordt weergegeven in de onderstaande schermafbeelding.

    ![Schermafbeelding van de kompasrichting prompt om verbinding te maken](./media/mongodb-compass/mongodb-compass-detect.png)

5. Wanneer u op klikt **Ja** in de vorige stap, de details van de verbindingsreeks worden automatisch ingevuld. Verwijder de waarde automatisch wordt ingevuld de **replicanaam** veld om ervoor te zorgen dat dit veld leeg blijft. 

    ![Schermafbeelding van de kompasrichting prompt om verbinding te maken](./media/mongodb-compass/mongodb-compass-replica.png)

6. Klik op **Connect** aan de onderkant van de pagina. Uw Cosmos DB-account en de databases moeten nu zijn zichtbaar in MongoDB kompas.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Studio 3T](mongodb-mongochef.md) met de API voor MongoDB van Azure Cosmos DB.
- Verken [voorbeelden](mongodb-samples.md) van MongoDB met de API voor MongoDB van Azure Cosmos DB.