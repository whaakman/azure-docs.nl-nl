---
title: Concepten van de server in Azure-Database voor MySQL | Microsoft Docs
description: In dit onderwerp worden overwegingen en richtlijnen gegeven voor het werken met Azure-Database voor de MySQL-servers.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: d3de3fdf28997b63321bf23443472db43ebb5c52
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Concepten van de server in Azure voor MySQL-Database
Dit artikel vindt overwegingen en richtlijnen voor het werken met Azure-Database voor de MySQL-servers.

## <a name="what-is-an-azure-database-for-mysql-server"></a>Wat is een Azure-Database voor de MySQL-server?

Een Azure-Database voor de MySQL-server is een centrale beheerdersrechten voor meerdere databases. Het is dezelfde MySQL server constructie die u mogelijk nagegaan wat in de lokale wereld. In het bijzonder de Azure-Database voor de MySQL-service wordt beheerd, prestaties garanties biedt en beschrijft de toegangs- en -functies op serverniveau.

Een Azure-Database voor de MySQL-server:

- In een Azure-abonnement is gemaakt.
- Is de bovenliggende resource voor databases.
- Biedt een naamruimte voor databases.
- Is een container met sterke levensduur semantiek - een server verwijderen en de ingesloten databases worden verwijderd.
- Collocates resources in een regio.
- Biedt een eindpunt voor de verbinding voor server en toegang tot de database.
- Voorziet in het bereik voor management-beleidsregels die van toepassing op de databases: aanmelding, firewall, gebruikers, rollen, configuraties, enzovoort.
- Is beschikbaar in meerdere versies. Zie voor meer informatie [Azure-Database wordt ondersteund voor versies van MySQL-database](./concepts-supported-versions.md).

Op een Azure Database voor MySQL-server kunt u een of meerdere databases maken. U kunt ervoor kiezen om te maken van een individuele database per server om de bronnen te gebruiken of maken van meerdere databases om de resources te delen. De prijzen is gestructureerde per server, op basis van de configuratie van prijscategorie, Compute-eenheden, en opslag (GB). Zie voor meer informatie [Prijscategorieën](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>Hoe ik verbinding maken en verifiëren met een Azure-Database voor de MySQL-server?

De volgende elementen zorgt het veilige toegang tot uw database.
|||
| :-- | :-- |
| **Verificatie en autorisatie** | Azure-Database voor de MySQL-server ondersteunt systeemeigen MySQL-verificatie. U kunt verbinding maken en verifiëren naar een server met de server admin aanmelden. |
| **Protocol** | De service ondersteunt een protocol op basis van een bericht dat wordt gebruikt door MySQL. |
| **TCP/IP** | Het protocol wordt ondersteund via TCP/IP- en Unix-domain-sockets. |
| **Firewall** | Ter bescherming van uw gegevens een firewallregel voorkomt u dat alle toegang tot uw database-server, totdat u opgeven welke computers over de machtiging beschikken. Zie [Azure Database voor firewallregels voor MySQL Server](./concepts-firewall-rules.md). |
| **SSL** | De service ondersteunt afdwingen SSL-verbindingen tussen uw toepassingen en uw database-server.  Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie. |

## <a name="how-do-i-manage-a-server"></a>Hoe kan ik een server beheren?
U kunt Azure-Database voor de MySQL-servers beheren met behulp van de Azure-portal of Azure CLI.

## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service [Azure-Database voor MySQL-overzicht](./overview.md)
- Voor informatie over specifieke resource quota's en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-service-tiers.md)
- Zie voor meer informatie over verbinding maken met de service [verbindingsbibliotheken voor Azure-Database voor MySQL](./concepts-connection-libraries.md).
