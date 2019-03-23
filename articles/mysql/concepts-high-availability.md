---
title: Concepten van hoge beschikbaarheid in Azure Database for MySQL
description: Dit onderwerp vindt u informatie van hoge beschikbaarheid bij het gebruik van Azure Database voor MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 055727695bfa1ce8a6bb160a7e071c2a161afb3b
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351028"
---
# <a name="high-availability-concepts-in-azure-database-for-mysql"></a>Concepten van hoge beschikbaarheid in Azure Database for MySQL
De Azure Database for MySQL-service biedt een gegarandeerde hoge mate van beschikbaarheid. De met financiële garantie service level agreement (SLA) is 99,99% bij algemene beschikbaarheid. Er is bijna geen toepassing uitvaltijd wanneer u deze service.

## <a name="high-availability"></a>Hoge beschikbaarheid
Het model met hoge beschikbaarheid (HA) is gebaseerd op ingebouwde mechanismen voor failover als een knooppunt op serverniveau onderbreking optreedt. Een onderbreking knooppunt op serverniveau kan zich voordoen vanwege een hardwarestoring optreedt of als reactie op een service-implementatie.

Allen tijde optreden wijzigingen aangebracht in een Azure Database for MySQL-database-server in de context van een transactie. Wijzigingen worden synchroon geregistreerd in Azure storage als de transactie doorgevoerd wordt. Als een knooppunt op serverniveau onderbreking optreedt, wordt de database-server automatisch maakt u een nieuw knooppunt en de opslag van gegevens is gekoppeld aan het nieuwe knooppunt. Alle actieve verbindingen verbroken en eventuele actieve transacties zijn niet doorgevoerd.

## <a name="application-retry-logic-is-essential"></a>Logica voor opnieuw proberen van toepassing is van essentieel belang
Het is belangrijk dat MySQL database-toepassingen zijn gebouwd om te detecteren en probeer het opnieuw verbindingen verbroken en transacties is mislukt. Wanneer de toepassing opnieuw probeert, wordt de verbinding van de toepassing transparant omgeleid naar de zojuist gemaakte instantie ten opzichte van het mislukte exemplaar heeft.

In Azure, wordt een gateway intern gebruikt om te leiden van de verbindingen naar het nieuwe exemplaar. De volledige failover-proces duurt meestal tientallen seconden na een onderbreking. Omdat de omleiding intern door de gateway verwerkt wordt, wordt de tekenreeks voor externe verbinding blijft hetzelfde voor de clienttoepassingen.

## <a name="scaling-up-or-down"></a>Omhoog of omlaag schalen
Net als bij de HA-model, wanneer een Azure Database for MySQL omhoog of omlaag wordt geschaald, een nieuw exemplaar van de server met de opgegeven grootte is gemaakt. De bestaande opslag van gegevens is losgekoppeld van het oorspronkelijke exemplaar en gekoppeld aan het nieuwe exemplaar.

Tijdens de schaalbewerking is een onderbreking van de databaseverbindingen vindt plaats. De clienttoepassingen die niet zijn verbonden en open niet-doorgevoerde transacties worden geannuleerd. Nadat u de clienttoepassing probeert opnieuw verbinding of een nieuwe verbinding maakt, is de gateway zorgt ervoor dat de verbinding met de nieuwe grootte-exemplaar. 

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [afhandeling van tijdelijke connectiviteitsfouten](concepts-connectivity.md)
- Meer informatie over het [repliceert uw gegevens met meer replica's](howto-read-replicas-portal.md)
