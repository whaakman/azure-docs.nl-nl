---
title: Concepten van hoge beschikbaarheid in Azure-Database voor PostgreSQL | Microsoft Docs
description: Dit onderwerp bevat informatie van hoge beschikbaarheid bij gebruik van Azure-Database voor PostgreSQL
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 315eb32a293eec89b274ccd017dba506241533e6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>Concepten van hoge beschikbaarheid in Azure voor PostgreSQL-Database
De Azure-Database voor PostgreSQL-service biedt een gegarandeerde hoge mate van beschikbaarheid. Back-financieel service level agreement (SLA) is 99,99% bij algemene beschikbaarheid. Er is bijna geen toepassing uitvaltijd wanneer u deze service.

## <a name="high-availability"></a>Hoge beschikbaarheid
Het model met hoge beschikbaarheid (HA) is gebaseerd op de ingebouwde failover-mechanismen wanneer een knooppunt niveau onderbreking optreedt. Een knooppunt niveau onderbreking kan zich voordoen vanwege een hardwarestoring of als reactie op een service-implementatie.

Te allen tijde worden wijzigingen aangebracht in een PostgreSQL optreden in de context van een transactie. Wijzigingen worden synchroon geregistreerd in Azure storage wanneer de transactie doorgevoerd is. Als een knooppunt niveau onderbreking optreedt, wordt de database-server automatisch maakt een nieuw knooppunt en opslag van gegevens is gekoppeld aan het nieuwe knooppunt. Actieve verbindingen worden verwijderd en inflight transacties zijn niet doorgevoerd.

## <a name="application-retry-logic-is-essential"></a>Pogingslogica voor de toepassing is het essentieel
Het is belangrijk dat PostgreSQL databasetoepassingen zijn ontworpen om te detecteren en probeer het opnieuw verbindingen verbroken en transacties is mislukt. Wanneer de toepassing nieuwe pogingen, van de toepassing verbinding deze wordt omgeleid naar de nieuwe maken instantie die voor de mislukte exemplaar overneemt.

Intern in Azure, een gateway gebruikt het omleiden van de verbindingen naar het nieuwe exemplaar. Na een onderbreking duurt het hele proces voor de failover-tien seconden doorgaans. De externe verbindingsreeks blijft hetzelfde zijn voor de clienttoepassingen.

## <a name="scaling-up-or-down"></a>Omhoog of omlaag schalen
Net als bij de HA-model, wanneer een Azure-Database voor PostgreSQL wordt geschaald omhoog of omlaag, een nieuwe server-exemplaar met de opgegeven grootte is gemaakt. De bestaande gegevensopslag is losgekoppeld van het oorspronkelijke exemplaar en gekoppeld aan het nieuwe exemplaar.

Tijdens de schaalaanpassing een onderbreking van de databaseverbindingen uitgevoerd. De clienttoepassingen zijn losgekoppeld en open niet-doorgevoerde transacties zijn geannuleerd. Zodra de clienttoepassing probeert opnieuw de verbinding of een nieuwe verbinding maakt, wordt de verbinding met de nieuwe formaat exemplaar verwezen door de gateway. 

## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service [Azure-Database voor PostgreSQL-overzicht](overview.md)
