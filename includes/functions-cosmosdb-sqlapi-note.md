---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: c68b56463625a09f9c8341881284706ab8250f46
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608164"
---
Azure Cosmos DB-bindingen worden alleen ondersteund voor gebruik met de SQL-API. Voor alle andere Azure Cosmos DB-API's, moet u de database opent vanuit uw functie met behulp van de statische-client voor uw API, met inbegrip van [Azure Cosmos DB-API voor MongoDB](../articles/cosmos-db/mongodb-introduction.md), [Cassandra-API](../articles/cosmos-db/cassandra-introduction.md), [ Gremlin-API](../articles/cosmos-db/graph-introduction.md), en [Table-API](../articles/cosmos-db/table-introduction.md).
