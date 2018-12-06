---
title: Concepten van de server in Azure Database for MySQL
description: In dit onderwerp vindt u overwegingen en richtlijnen voor het werken met Azure Database voor MySQL-servers.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 0ddab6a982f54f0309e87d3b74a7f21c0bb67ced
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955510"
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Concepten van de server in Azure Database for MySQL

In dit artikel vindt u overwegingen en richtlijnen voor het werken met Azure Database voor MySQL-servers.

## <a name="what-is-an-azure-database-for-mysql-server"></a>Wat is een Azure Database for MySQL-server?

Een Azure Database for MySQL-server is een centraal beheerpunt voor meerdere databases. Dit is dezelfde MySQL-server om voor te bereiden die u al bekend mee in de on-premises wereld wellicht. Met name de Azure Database for MySQL-service wordt beheerd, prestaties biedt en wordt aangegeven dat toegang en -functies op serverniveau.

Een Azure Database voor MySQL-server:

- Wordt gemaakt binnen een Azure-abonnement.
- Is de bovenliggende resource voor databases.
- Biedt een naamruimte voor databases.
- Is een container met sterke levensduursemantiek; d.w.z. - een server verwijderen en de ingesloten databases worden verwijderd.
- Groepeert resources in een regio.
- Biedt een verbindingseindpunt voor server en database-toegang.
- Vindt u het bereik voor management-beleid die van toepassing op de betreffende databases zijn: aanmelding, firewall, gebruikers, rollen, configuraties, enzovoort.
- Is beschikbaar in meerdere versies. Zie voor meer informatie, [ondersteund Azure Database for MySQL-databaseversies](./concepts-supported-versions.md).

Op een Azure Database voor MySQL-server kunt u een of meerdere databases maken. U kunt kiezen voor het maken van een individuele database per server om alle resources te gebruiken of om meerdere databases voor het delen van de resources te maken. De prijzen zijn gestructureerd per server, op basis van de configuratie van prijzen laag vCores en opslag (GB). Zie voor meer informatie, [Prijscategorieën](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>Hoe kan ik verbinding maken en verifiëren met een Azure Database voor MySQL-server?

De volgende elementen zorgen veilige toegang tot uw database.
|     |     |
| :-- | :-- |
| **Verificatie en autorisatie** | Azure Database voor MySQL-server biedt ondersteuning voor systeemeigen MySQL-verificatie. U kunt verbinding maken en verifiëren met een server met de serverbeheerder. |
| **Protocol** | De service ondersteunt een protocol op basis van een bericht dat wordt gebruikt door MySQL. |
| **TCP/IP** | Het protocol wordt ondersteund via TCP/IP en sockets van Unix-domein. |
| **Firewall** | Ter bescherming van uw gegevens, voorkomt een firewallregel alle toegang tot uw databaseserver totdat u opgeeft welke computers zijn gemachtigd. Zie [Azure Database for MySQL Server firewall-regels](./concepts-firewall-rules.md). |
| **SSL** | De service biedt ondersteuning voor afdwingen van SSL-verbindingen tussen uw toepassingen en uw database-server.  Zie [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (SSL-connectiviteit in uw toepassing configureren om veilig verbinding te maken met Azure-database voor MySQL) voor meer informatie. |

## <a name="how-do-i-manage-a-server"></a>Hoe kan ik een server beheren?

U kunt Azure Database voor MySQL-servers beheren met behulp van de Azure-portal of de Azure CLI.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van de service, [Azure Database for MySQL-overzicht](./overview.md)
- Voor informatie over specifieke resource quota en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-service-tiers.md)
- Zie voor meer informatie over het verbinden met de service [verbindingsbibliotheken voor Azure Database for MySQL](./concepts-connection-libraries.md).
