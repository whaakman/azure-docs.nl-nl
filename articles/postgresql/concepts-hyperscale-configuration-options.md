---
title: Azure Database voor PostgreSQL – grootschalige (Citus) (preview) instellingen voor prestaties
description: Opties voor een servergroep grootschalige (Citus), met inbegrip van knooppunt rekentijd, opslag en regio's.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e8c1c2e51ca14ae9b17f0d7efb20552cdd55139b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077288"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-preview-performance-options"></a>Azure Database voor PostgreSQL – grootschalige (Citus) (preview) instellingen voor prestaties

## <a name="compute-and-storage"></a>Compute en opslag
 
U kunt de instellingen berekening en opslag onafhankelijk van elkaar voor worker-knooppunten en het coördinatorknooppunt selecteren in een grootschalige (Citus) server-groep.  COMPUTE-resources worden verstrekt als vCores, die staan voor de logische CPU van de onderliggende hardware. De maximale grootte voor het inrichten van verwijst naar de capaciteit die beschikbaar zijn voor de coördinator en worker-knooppunten in uw servergroep grootschalige (Citus). De opslag bevat databasebestanden, tijdelijke bestanden, transactielogboeken en de Postgres server-Logboeken. De totale hoeveelheid opslag die u ook inrichten definieert de i/o-capaciteit beschikbaar voor elk knooppunt werknemer en coordinator.
 
|                       | Worker-knooppunt           | Coördinatorknooppunt      |
|-----------------------|-----------------------|-----------------------|
| Compute, vCores       | 4, 8, 16, 32          | 4, 8, 16, 32          |
| Geheugen per vCore, GiB | 8                     | 4                     |
| Opslaggrootte, TiB     | 0.5, 1, 2             | 0.5, 1, 2             |
| Opslagtype          | Algemeen gebruik (SSD) | Algemeen gebruik (SSD) |
| IOPS                  | Maximaal 3 IOPS/GiB      | Maximaal 3 IOPS/GiB      |


## <a name="regions"></a>Regio's
Servergroepen grootschalige (Citus) zijn beschikbaar in de volgende Azure-regio's:
* US - oost 2
* Azië - zuidoost
* Europa -west
* US - west 2

## <a name="pricing"></a>Prijzen
Zie de service voor de meest recente prijsinformatie [pagina met prijzen](https://azure.microsoft.com/pricing/details/postgresql/).
Om te zien van de kosten voor de configuratie die u wilt, de [Azure-portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) ziet u de maandelijkse kosten op de **configureren** tabblad op basis van de opties die u selecteert. Als u geen Azure-abonnement hebt, kunt u de prijscalculator van Azure gebruiken om op te halen van een geschatte prijs. Op de [prijscalculator van Azure](https://azure.microsoft.com/pricing/calculator/) website, selecteer **items toevoegen**, vouw de **Databases** categorie, en kies **Azure Database voor PostgreSQL: Zeer grootschalige (Citus)** om aan te passen van de opties.
 
## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [een grootschalige (Citus) server-groep maken in de portal](quickstart-create-hyperscale-portal.md).
