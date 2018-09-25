---
title: Concepten van hoge beschikbaarheid in Azure Database voor MariaDB
description: In dit onderwerp vindt u informatie van hoge beschikbaarheid bij het gebruik van Azure Database voor MariaDB
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 2f06c9515c6ee79ea7475452b7000d1babdbecfe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956520"
---
# <a name="high-availability-concepts-in-azure-database-for-mariadb"></a>Concepten van hoge beschikbaarheid in Azure Database voor MariaDB
De Azure Database voor MariaDB-service biedt een gegarandeerde hoge mate van beschikbaarheid. De met financiële garantie service level agreement (SLA) is 99,99% bij algemene beschikbaarheid. De SLA is niet van toepassing op preview-versie. Er is bijna geen toepassing uitvaltijd wanneer u deze service.

## <a name="high-availability"></a>Hoge beschikbaarheid
Het model met hoge beschikbaarheid (HA) is gebaseerd op ingebouwde mechanismen voor failover als een knooppunt op serverniveau onderbreking optreedt. Een onderbreking knooppunt op serverniveau kan zich voordoen vanwege een hardwarestoring optreedt of als reactie op een service-implementatie.

Allen tijde optreden wijzigingen aangebracht in een Azure Database voor MariaDB database-server in de context van een transactie. Wijzigingen worden synchroon geregistreerd in Azure storage als de transactie doorgevoerd wordt. Als een knooppunt op serverniveau onderbreking optreedt, wordt de database-server automatisch maakt u een nieuw knooppunt en de opslag van gegevens is gekoppeld aan het nieuwe knooppunt. Alle actieve verbindingen verbroken en eventuele actieve transacties zijn niet doorgevoerd.

## <a name="application-retry-logic-is-essential"></a>Logica voor opnieuw proberen van toepassing is van essentieel belang
Het is belangrijk dat MariaDB database-toepassingen zijn gebouwd om te detecteren en probeer het opnieuw verbindingen verbroken en transacties is mislukt. Wanneer de toepassing opnieuw probeert, wordt de verbinding van de toepassing transparant omgeleid naar de zojuist gemaakte instantie ten opzichte van het mislukte exemplaar heeft.

In Azure, wordt een gateway intern gebruikt om te leiden van de verbindingen naar het nieuwe exemplaar. De volledige failover-proces duurt meestal tientallen seconden na een onderbreking. Omdat de omleiding intern door de gateway verwerkt wordt, wordt de tekenreeks voor externe verbinding blijft hetzelfde voor de clienttoepassingen.

## <a name="scaling-up-or-down"></a>Omhoog of omlaag schalen
Net als bij de HA-model, wanneer een Azure Database voor MariaDB omhoog of omlaag wordt geschaald, een nieuw exemplaar van de server met de opgegeven grootte is gemaakt. De bestaande opslag van gegevens is losgekoppeld van het oorspronkelijke exemplaar en gekoppeld aan het nieuwe exemplaar.

Tijdens de schaalbewerking is een onderbreking van de databaseverbindingen vindt plaats. De clienttoepassingen die niet zijn verbonden en open niet-doorgevoerde transacties worden geannuleerd. Nadat u de clienttoepassing probeert opnieuw verbinding of een nieuwe verbinding maakt, is de gateway zorgt ervoor dat de verbinding met de nieuwe grootte-exemplaar.

## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service, [Azure Database voor MariaDB-overzicht](overview.md)
