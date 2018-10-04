---
title: SSL-connectiviteit voor Azure Database voor MariaDB
description: Informatie voor het configureren van Azure Database voor MariaDB en gekoppelde toepassingen gebruiken SSL-verbindingen
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1d0b27a8fd7e3882a73624fa1b668ac602a85e6b
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249499"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>SSL-verbindingen in Azure Database voor MariaDB
Azure Database voor MariaDB ondersteunt verbindingen van uw database-server voor clienttoepassingen met Secure Sockets Layer (SSL). Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld.

## <a name="default-settings"></a>Standaardinstellingen
Standaard moet de database-service worden geconfigureerd voor SSL-verbindingen vereisen bij het verbinden met MariaDB.  We raden aan om te voorkomen dat de SSL-optie zoveel mogelijk uit te schakelen.

Bij het inrichten van een nieuwe Azure-Database voor MariaDB-server via de Azure-portal en CLI kan afdwingen van SSL-verbindingen is standaard ingeschakeld.

Verbindingsreeksen voor verschillende programmeertalen worden weergegeven in de Azure-portal. Deze tekenreeksen voor databaseverbindingen omvatten de vereiste parameters SSL verbinding maakt met uw database. Selecteer uw server in de Azure-portal. Onder de **instellingen** kop, selecteer de **verbindingsreeksen**. De SSL-parameter is afhankelijk van de connector, bijvoorbeeld ' ssl = true "of" sslmode = vereisen "of" sslmode = vereist ' en andere verschillen.

Raadpleeg voor informatie over het in- of uitschakelen van SSL-verbinding bij het ontwikkelen van toepassingen, [over het configureren van SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [firewallregels voor server](concepts-firewall-rules.md)
- Meer informatie over het [SSL configureren](howto-configure-ssl.md).
