---
title: Distributiekolommen kiezen in Azure Database voor PostgreSQL – grootschalige (Citus) (preview)
description: Goede keuze voor van distributiekolommen in veelvoorkomende scenario's voor grootschalig
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e9fba14b8979f739fd29bc277e32fb544221d08a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65078983"
---
# <a name="choose-distribution-columns-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Distributiekolommen kiezen in Azure Database voor PostgreSQL – grootschalige (Citus) (preview)

Het kiezen van de distributie van elke tabelkolom is **een van de belangrijkste** beslissingen modelleren. Zeer grootschalige slaat rijen in shards op basis van de waarde van de rijen in de distributie-kolom.

De juiste keuzegroepen gerelateerde gegevens bij elkaar op dezelfde fysieke knooppunten, waardoor snelle en toe te voegen ondersteuning voor alle SQL-functies vraagt. Een onjuiste keuze maakt het systeem langzaam wordt uitgevoerd, en alle SQL-functies op knooppunten niet wordt ondersteund.

In deze sectie geeft distributie kolom tips voor de twee meest voorkomende grootschalige scenario's.

### <a name="multi-tenant-apps"></a>Apps met meerdere Tenants

Een vorm van hiërarchische database modelleren voor het distribueren van query's op knooppunten in de servergroep maakt gebruik van de architectuur met meerdere tenants.  De bovenkant van de hiërarchie van gegevens wordt ook wel de *tenant-ID*, en moeten worden opgeslagen in een kolom in elke tabel.

Zeer grootschalige inspecteert query's om te zien welke tenant-ID ze betrekking hebben op, en de overeenkomende tabel shard worden gevonden. Deze stuurt de query naar een enkele worker-knooppunt met de shard. Een query uitvoeren met alle relevante gegevens op hetzelfde knooppunt worden geplaatst, wordt de plaatsing genoemd.

Het volgende diagram illustreert CO-locatie in het gegevensmodel van meerdere tenants. Deze twee tabellen, Accounts en campagnes bevat, elk gedistribueerd door `account_id`. De grijze vakken vertegenwoordigen shards, elk waarvan de kleur vertegenwoordigt welke worker-knooppunt bevat. Groen shards worden samen opgeslagen op een worker-knooppunt en blauw op een andere. U ziet hoe een joinquery tussen Accounts en campagnes zouden hebben de benodigde gegevens samen op één knooppunt wanneer beide tabellen beperken tot hetzelfde account\_id.

![multitenant CO-locatie](media/concepts-hyperscale-choosing-distribution-column/multi-tenant-colocation.png)

Om toe te passen dit ontwerp in uw eigen schema, Identificeer wat wordt verstaan onder een tenant in uw toepassing. Algemene instanties omvatten een bedrijf, account, organisatie of de klant. Naam van de kolom worden er ongeveer als `company_id` of `customer_id`. Een toelichting van elk van uw query's en u zich afvragen: zou dit werkt als er extra WHERE-componenten te beperken van alle tabellen die zijn betrokken tot rijen met dezelfde tenant-ID?
Query's in het model met meerdere tenants zijn gericht op een tenant, bijvoorbeeld query's op de verkoop- of -inventaris zou worden binnen het bereik van binnen een bepaalde store.

#### <a name="best-practices"></a>Beste praktijken

-   **Partitie gedistribueerde tabellen door een algemene tenant\_id-kolom.** Bijvoorbeeld in een SaaS-toepassing waarbij tenants bedrijven, de tenant zijn\_-id waarschijnlijk bedrijf\_id.
-   **Kleine cross-tenant-tabellen omzetten in referentietabellen.** Wanneer u meerdere tenants delen een kleine tabel met gegevens, kunt u deze als een tabel distribueren.
-   **Filter toepassing op alle query's door de tenant beperken\_id.** Elke query moet gegevens voor één tenant per keer aanvragen.

Lees de [multitenant zelfstudie](./tutorial-design-database-hyperscale-multi-tenant.md) voor een voorbeeld van het bouwen van dit type toepassing.

### <a name="real-time-apps"></a>Realtime-Apps

De architectuur met meerdere tenants introduceert een hiërarchische structuur en maakt gebruik van collocatie gegevens op route-query's per tenant. Realtime-architecturen is daarentegen, afhankelijk van specifieke distributie-eigenschappen van hun gegevens voor uiterst parallelle verwerking.

In dat geval gebruiken we 'entiteit-ID' in als een term voor van distributiekolommen in het realtime model. Typische entiteiten zijn gebruikers, hosts of apparaten.

Realtime query's doorgaans om vragen numerieke statistische functies die zijn gegroepeerd op datum of categorie. Zeer grootschalige verzendt deze query's naar elke shard voor gedeeltelijke resultaten en het definitieve antwoord op het coördinatorknooppunt ophaalprotocol. Query's uitgevoerd snelste wanneer als veel knooppunten mogelijk bijdragen, en dat geen één knooppunt moet een onevenredige hoeveelheid werk doen.

#### <a name="best-practices"></a>Beste praktijken

-   **Kies een kolom met hoge kardinaliteit, als de distributiekolom.** Ter vergelijking: een \"status\" veld in een tabel met de waarden "new", "betaalde" en "verzonden" is een slechte keuze van een distributiekolom. Wordt ervan uitgegaan dat alleen de enkele waarden, die beperkt het aantal shards die de gegevens kan bevatten, en het aantal knooppunten die kunnen worden verwerkt. Tussen de kolommen met hoge kardinaliteit is het handig ook kiezen die vaak worden gebruikt in group by-componenten of als lid worden van sleutels.
-   **Kies een kolom met gelijke verdeling.** Als u een tabel in een kolom die aan bepaalde algemene waarden ook ongelijkmatig distribueert, vaak gegevens in de tabel om te worden verzameld in bepaalde shards. De knooppunten die deze shards uiteindelijk meer werk dan andere knooppunten.
-   **Distribueren van feiten- en dimensietabellen tabellen op hun gemeenschappelijke kolommen.**
    De feitentabel kan slechts één distributiesleutel hebben. Tabellen die deelnemen aan op een andere sleutel zijn zich niet op dezelfde locatie bevindt als de feitentabel. Kies één dimensie plaatsen op basis van hoe vaak deze is gekoppeld en de grootte van de gekoppelde rijen.
-   **Wijzig de sommige dimensietabellen in referentietabellen.** Als een dimensietabel niet kan geplaatst met de feitentabel worden, kunt u de prestaties van query's verbeteren door het distribueren van kopieën van de dimensietabel op alle knooppunten in de vorm van een tabel.

Lees de [realtime dashboard zelfstudie](./tutorial-design-database-hyperscale-realtime.md) voor een voorbeeld van het bouwen van dit type toepassing.

### <a name="timeseries-data"></a>Tijdseriegegevens

In een time series-workload query toepassingen recente informatie bij het archiveren van oude gegevens.

De meest voorkomende fout in het modelleren van timeseries-gegevens in grootschalige maakt gebruik van de tijdstempel zelf als een kolom van het distributiepunt. Een hash-distributiepunt op basis van tijd wilt distribueren tijden schijnbaar willekeurig in verschillende shards in plaats van bereik van de tijd in shards samen blijven. Query's met betrekking tot tijd algemeen verwijzen naar de adresbereiken van tijd (bijvoorbeeld de meest recente gegevens), zodat deze een hash-distributiepunt zou leiden tot het netwerk overhead.

#### <a name="best-practices"></a>Beste praktijken

-   **Kies een tijdstempel wordt als niet als de distributiekolom.** Kies een ander distributiepunt-kolom. Gebruik de tenant-ID in een multitenant-app of in een realtime-app gebruikt de entiteit-ID.
-   **Gebruik PostgreSQL tabel voor het partitioneren van tijd in plaats daarvan.** Gebruik tabellen partitioneren om een grote tabel van de tijd geordende opeenvolgende gegevens in meerdere overgenomen tabellen met elk met verschillende tijdsbereik.  Distribueren van een Postgres-gepartitioneerde tabel in grootschalige maakt shards voor de overgenomen tabellen.

Lees de [timeseries zelfstudie](https://aka.ms/hyperscale-tutorial-timeseries) voor een voorbeeld van het bouwen van dit type toepassing.

## <a name="next-steps"></a>Volgende stappen
- Informatie over hoe [CO](concepts-hyperscale-colocation.md) tussen gedistribueerde gegevens kunnen query's snel worden uitgevoerd
