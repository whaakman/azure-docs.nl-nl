---
title: Knooppunten in de Azure Database voor PostgreSQL – grootschalige (Citus) (preview)
description: De twee soorten knooppunten in een servergroep.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: c6b948ed63f43f1597103d123be5ed39f42bd276
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077273"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Knooppunten in de Azure Database voor PostgreSQL – grootschalige (Citus) (preview)

De grootschalige (Citus) (preview) die als host fungeert type kunt Azure Database voor PostgreSQL-servers (knooppunten genoemd) te coördineren met elkaar in een architectuur met 'niets wordt gedeeld'. De knooppunten in een servergroep gezamenlijk meer gegevens en meer CPU-kernen dan mogelijk op één server zijn zou gebruiken. De architectuur kan ook de database om te schalen door meer knooppunten toe te voegen aan de servergroep.

## <a name="coordinator-and-workers"></a>Coördinator en werknemers

Elke servergroep heeft een coördinatorknooppunt en meerdere werknemers. De query's verzenden toepassingen naar het coördinatorknooppunt, die deze doorstuurt naar de desbetreffende werknemers en stelt u de resultaten samen. Toepassingen kunnen niet rechtstreeks verbinding maken met werknemers.

Voor elke query de coördinator doorgestuurd naar een enkele worker-knooppunt, of deze voor verschillende afhankelijk van of de vereiste gegevens zich op één knooppunt of meerdere bevinden parallelizes. De coördinator beslist wat te doen door metagegevenstabellen raadplegen. Deze tabellen worden de DNS-namen en status van worker-knooppunten en de verdeling van gegevens bijhouden over knooppunten.

## <a name="next-steps"></a>Volgende stappen
- Informatie over hoe knooppunten opslaan [gedistribueerde gegevens](concepts-hyperscale-distributed-data.md)
