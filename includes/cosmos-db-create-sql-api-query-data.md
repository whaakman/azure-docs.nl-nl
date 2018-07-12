---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: e6e70da1f939547cdb589ae7bcb6ed1f6148f22e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733762"
---
U kunt nu query's in Data Explorer gebruiken om uw gegevens te halen en te filteren.

1. U ziet dat de query standaard is ingesteld op `SELECT * FROM c`. Deze standaardquery haalt alle documenten in de verzameling op en geeft ze weer. 

    ![De standaardquery in Data Explorer is 'SELECT * FROM c'](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)

2. Blijf op het tabblad **Documenten** en wijzig de query door op de knop **Filter bewerken** te klikken, `ORDER BY c._ts DESC` aan het vak Querypredicaat toe te voegen en vervolgens op **Filter toepassen** te klikken.

    ![Wijzig de standaardquery door ORDER BY c._ts DESC toe te voegen en te klikken op Filter toepassen](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

Deze gewijzigde query sorteert documenten in aflopende volgorde op basis van hun tijdstempel. Uw tweede document wordt nu dus als eerste weergegeven. Als u bekend bent met SQL-syntaxis, kunt u een van de ondersteunde [SQL-query's](../articles/cosmos-db/sql-api-sql-query.md) in dit vak invoeren. 

Ons werk in Data Explorer is voltooid. Voordat we verdergaan met het werken met code, willen we u erop wijzen dat u Data Explorer ook kunt gebruiken voor het maken van opgeslagen procedures, UDF's en triggers om bedrijfslogica aan de serverzijde uit te voeren en doorvoer te schalen. In Data Explorer wordt alle ingebouwde programmatische gegevenstoegang zichtbaar die beschikbaar is in de API's, maar biedt eenvoudige toegang tot uw gegevens in Azure Portal.