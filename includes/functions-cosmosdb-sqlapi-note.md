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
ms.openlocfilehash: c54aa861a47b11756f05e003e9b944df6c5b0e28
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175821"
---
Azure Cosmos DB-bindingen worden alleen ondersteund voor gebruik met de SQL-API. Voor alle andere Azure Cosmos DB-API's, moet u de database opent vanuit uw functie met behulp van de statische-client voor uw API, met inbegrip van [Azure Cosmos DB-API voor MongoDB](../articles/cosmos-db/mongodb-introduction.md), [Cassandra-API](../articles/cosmos-db/cassandra-introduction.md), [ Gremlin-API](../articles/cosmos-db/graph-introduction.md), en [Table-API](../articles/cosmos-db/table-introduction.md).
