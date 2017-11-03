---
title: Concepten van hoge beschikbaarheid in Azure-Database voor MySQL | Microsoft Docs
description: Dit onderwerp bevat informatie van hoge beschikbaarheid bij gebruik van Azure-Database voor MySQL
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/31/2017
ms.openlocfilehash: 5b63a1ac666a14354b5b93f22722b624244a7aa2
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="high-availability-concepts-in-azure-database-for-mysql"></a>Concepten van hoge beschikbaarheid in Azure voor MySQL-Database
De Azure-Database voor de MySQL-service biedt een gegarandeerde hoge mate van beschikbaarheid. Back-financieel service level agreement (SLA) is 99,99% bij algemene beschikbaarheid. Er is bijna geen toepassing uitvaltijd wanneer u deze service.

## <a name="high-availability"></a>Hoge beschikbaarheid
Het model met hoge beschikbaarheid (HA) is gebaseerd op de ingebouwde failover-mechanismen wanneer een knooppunt niveau onderbreking optreedt. Een knooppunt niveau onderbreking kan zich voordoen vanwege een hardwarestoring of als reactie op een service-implementatie.

Te allen tijde worden wijzigingen aangebracht in een Azure-Database voor MySQL-database-server optreden in de context van een transactie. Wijzigingen worden synchroon geregistreerd in Azure storage wanneer de transactie doorgevoerd is. Als een knooppunt niveau onderbreking optreedt, wordt de database-server automatisch maakt een nieuw knooppunt en opslag van gegevens is gekoppeld aan het nieuwe knooppunt. Actieve verbindingen worden verwijderd en inflight transacties zijn niet doorgevoerd.

## <a name="application-retry-logic-is-essential"></a>Pogingslogica voor de toepassing is het essentieel
Het is belangrijk dat MySQL databasetoepassingen zijn ontworpen om te detecteren en probeer het opnieuw verbindingen verbroken en transacties is mislukt. Wanneer de toepassing opnieuw probeert, wordt de toepassing verbinding transparant omgeleid naar de zojuist gemaakte instantie die voor de mislukte exemplaar overneemt.

Intern in Azure, een gateway gebruikt het omleiden van de verbindingen naar het nieuwe exemplaar. Na een onderbreking vindt het hele proces voor de failover-meestal tien seconden. Aangezien de omleiding intern wordt verwerkt door de gateway, blijft de externe verbindingsreeks voor de clienttoepassingen hetzelfde.

## <a name="scaling-up-or-down"></a>Omhoog of omlaag schalen
Net als bij de HA-model, wanneer een Azure-Database voor MySQL wordt geschaald omhoog of omlaag, een nieuwe server-exemplaar met de opgegeven grootte is gemaakt. De bestaande gegevensopslag is losgekoppeld van het oorspronkelijke exemplaar en gekoppeld aan het nieuwe exemplaar.

Tijdens de schaalaanpassing een onderbreking van de databaseverbindingen uitgevoerd. De clienttoepassingen zijn losgekoppeld en open niet-doorgevoerde transacties zijn geannuleerd. Zodra de clienttoepassing probeert opnieuw de verbinding of een nieuwe verbinding maakt, wordt de verbinding met de nieuwe formaat exemplaar verwezen door de gateway. 

## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service [Azure-Database voor MySQL-overzicht](overview.md)
