---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: bb915c18965a55c2b1fc7fe3cf1fa923d7f43fd6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43052516"
---
Azure Cosmos DB-bindingen worden alleen ondersteund voor gebruik met de SQL-API. Voor alle andere Azure Cosmos DB-API's, dient u de database vanuit uw functie te benaderen met behulp van de statische client voor uw API, waaronder voor de [MongoDB-API](../articles/cosmos-db/mongodb-introduction.md), de [Cassandra-API](../articles/cosmos-db/cassandra-introduction.md), de [Gremlin-API](../articles/cosmos-db/graph-introduction.md) en de [Table-API](../articles/cosmos-db/table-introduction.md).
