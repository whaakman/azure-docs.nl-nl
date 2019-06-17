---
title: Connectiviteitsarchitectuur in Azure Database voor MariaDB
description: Beschrijft de architectuur van de verbinding voor uw Azure Database voor MariaDB-server.
author: kummanish
ms.author: manishku
ms.service: mariaDB
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: 126e7ce4d9784a2f6a59f39562dff9a0b9d60ea0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735389"
---
# <a name="connectivity-architecture-in-azure-database-for-mariadb"></a>Connectiviteitsarchitectuur in Azure Database voor MariaDB
In dit artikel wordt de Azure Database voor MariaDB connectiviteitsarchitectuur ook uitgelegd hoe het verkeer wordt omgeleid naar uw Azure Database voor MariaDB-exemplaar van clients binnen en buiten Azure.

## <a name="connectivity-architecture"></a>Connectiviteitsarchitectuur

Verbinding met uw Azure Database voor MariaDB tot stand is gebracht via een gateway die verantwoordelijk is voor routering binnenkomende verbindingen met de fysieke locatie van uw server in onze clusters. Het volgende diagram illustreert de werkstroom verkeer.

![Overzicht van de connectiviteitsarchitectuur](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

Als de client verbinding maakt met de database, krijgen ze een verbindingsreeks waarmee verbinding met de gateway. Deze gateway is een openbaar IP-adres dat op poort 3306 luistert. In het databasecluster verkeer doorgestuurd naar de juiste Azure-Database voor MariaDB. Als u wilt verbinding maken met uw server, zoals vanaf bedrijfsnetwerken, is het daarom noodzakelijk om de firewall van de client side waarmee uitgaand verkeer naar het bereiken van onze gateways te openen. Hieronder vindt u een volledige lijst van de IP-adressen die worden gebruikt door onze gateways per regio.

## <a name="azure-database-for-mariadb-gateway-ip-addresses"></a>Azure Database voor MariaDB-gateway-IP-adressen

De volgende tabel geeft een lijst van de primaire en secundaire IP-adressen van de Azure Database voor MariaDB-gateway voor alle regio's van gegevens. Het primaire IP-adres is het huidige IP-adres van de gateway en het tweede IP-adres is een IP-adres van de failover in geval van storing van de primaire. Zoals gezegd, moeten klanten uitgaand naar beide IP-adressen toestaan. Het tweede IP-adres niet luisteren op alle services totdat deze is geactiveerd door Azure Database voor MariaDB om verbindingen te accepteren.

| **Regionaam** | **Primaire IP-adres** | **Secundaire IP-adres** |
|:----------------|:-------------|:------------------------|
| Australië - oost | 13.75.149.87 | 40.79.161.1 |
| Australië - zuidoost | 191.239.192.109 | 13.73.109.251 |
| Brazilië - zuid | 104.41.11.5 | |
| Canada - midden | 40.85.224.249 | |
| Canada - oost | 40.86.226.166 | |
| US - centraal | 23.99.160.139 | 13.67.215.62 |
| China Oost 1 | 139.219.130.35 | |
| China - oost 2 | 40.73.82.1 | |
| China North 1 | 139.219.15.17 | |
| China - noord 2 | 40.73.50.0 | |
| Azië - oost | 191.234.2.139 | 52.175.33.150 |
| VS-Oost 1 | 191.238.6.43 | 40.121.158.30 |
| US - oost 2 | 191.239.224.107 | 40.79.84.180 * |
| Frankrijk - centraal | 40.79.137.0 | 40.79.129.1 |
| Duitsland - centraal | 51.4.144.100 | |
| India - centraal | 104.211.96.159 | |
| India - zuid | 104.211.224.146 | |
| India - west | 104.211.160.80 | |
| Japan - oost | 191.237.240.43 | 13.78.61.196 |
| Japan - west | 191.238.68.11 | 104.214.148.156 |
| Korea - centraal | 52.231.32.42 | |
| Korea - zuid | 52.231.200.86 |  |
| US - noord-centraal | 23.98.55.75 | 23.96.178.199 |
| Europa - noord | 191.235.193.75 | 40.113.93.91 |
| US - zuid-centraal | 23.98.162.75 | 13.66.62.124 |
| Azië - zuidoost | 23.100.117.95 | 104.43.15.0 |
| Verenigd Koninkrijk Zuid | 51.140.184.11 | |
| Verenigd Koninkrijk West | 51.141.8.11| |
| Europa -west | 191.237.232.75 | 40.68.37.158 |
| VS-West 1 | 23.99.34.75 | 104.42.238.205 |
| US - west 2 | 13.66.226.202 | |
||||

> [!NOTE]
> *VS-Oost 2* heeft ook een tertiaire IP-adres van `52.167.104.0`.

## <a name="next-steps"></a>Volgende stappen

* [Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van de Azure portal](./howto-manage-firewall-portal.md)
* [Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van Azure CLI](./howto-manage-firewall-cli.md)
