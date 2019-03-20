---
title: Concepten van de server in Azure Database voor MariaDB
description: In dit onderwerp vindt u overwegingen en richtlijnen voor het werken met Azure Database voor MariaDB-servers.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f61f8740c9514f6276afb2ee84bcdccdc54c0710
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57997891"
---
# <a name="server-concepts-in-azure-database-for-mariadb"></a>Concepten van de server in Azure Database voor MariaDB
In dit artikel vindt u overwegingen en richtlijnen voor het werken met Azure Database voor MariaDB-servers.

## <a name="what-is-an-azure-database-for-mariadb-server"></a>Wat is een Azure Database voor MariaDB-server?

Een Azure Database voor MariaDB-server is een centraal beheerpunt voor meerdere databases. Dit is dezelfde MariaDB-server om voor te bereiden die u al bekend mee in de on-premises wereld wellicht. Met name de Azure Database voor MariaDB-service wordt beheerd, prestaties biedt en wordt aangegeven dat toegang en -functies op serverniveau.

Een Azure Database voor MariaDB-server:

- Wordt gemaakt binnen een Azure-abonnement.
- Is de bovenliggende resource voor databases.
- Biedt een naamruimte voor databases.
- Is een container met sterke levensduursemantiek; d.w.z. - een server verwijderen en de ingesloten databases worden verwijderd.
- Groepeert resources in een regio.
- Biedt een verbindingseindpunt voor server en database-toegang.
- Vindt u het bereik voor management-beleid die van toepassing op de betreffende databases zijn: aanmelding, firewall, gebruikers, rollen, configuraties, enzovoort.
- Is beschikbaar in de versie van de engine MariaDB 10.2. Zie voor meer informatie, [ondersteund Azure Database voor MariaDB databaseversies](./concepts-supported-versions.md).

Op een Azure Database for MariaDB-server kunt u een of meerdere databases maken. U kunt kiezen voor het maken van een individuele database per server om alle resources te gebruiken of om meerdere databases voor het delen van de resources te maken. De prijzen zijn gestructureerd per server, op basis van de configuratie van prijzen laag vCores en opslag (GB). Zie voor meer informatie, [Prijscategorieën](./concepts-pricing-tiers.md).

## <a name="how-do-i-secure-an-azure-database-for-mariadb-server"></a>Hoe beveilig ik een Azure Database voor MariaDB-server?

De volgende elementen zorgen veilige toegang tot uw database.

|||
| :--| :--|
| **Verificatie en autorisatie** | Azure Database voor MariaDB-server biedt ondersteuning voor systeemeigen MySQL-verificatie. U kunt verbinding maken en verifiëren met een server met de serverbeheerder. |
| **Protocol** | De service ondersteunt een protocol op basis van een bericht dat wordt gebruikt door MySQL. |
| **TCP/IP** | Het protocol wordt ondersteund via TCP/IP en sockets van Unix-domein. |
| **Firewall** | Ter bescherming van uw gegevens, voorkomt een firewallregel alle toegang tot uw databaseserver totdat u opgeeft welke computers zijn gemachtigd. Zie [Azure Database voor MariaDB Server firewall-regels](./concepts-firewall-rules.md). |
| **SSL** | De service biedt ondersteuning voor afdwingen van SSL-verbindingen tussen uw toepassingen en uw database-server. Zie [configureren van SSL-connectiviteit in uw toepassing veilig verbinding maken met Azure Database voor MariaDB](./howto-configure-ssl.md). |

## <a name="how-do-i-manage-a-server"></a>Hoe kan ik een server beheren?
U kunt Azure Database voor MariaDB-servers beheren met behulp van de Azure-portal of de Azure CLI.

## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service, [Azure Database voor MariaDB-overzicht](./overview.md)
- Voor informatie over specifieke resource quota en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-pricing-tiers.md)

<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->
