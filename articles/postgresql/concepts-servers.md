---
title: Concepten van de server in Azure-Database voor PostgreSQL | Microsoft Docs
description: In dit onderwerp worden overwegingen en richtlijnen gegeven voor het configureren en beheren van de Azure-Database voor PostgreSQL-servers.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 12/02/2017
ms.openlocfilehash: d7eec2735e48f57500eb2ea822f0949d2ec2e585
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-database-for-postgresql-servers"></a>Azure-Database voor PostgreSQL-Servers
Dit artikel vindt overwegingen en richtlijnen voor het werken met Azure-Database voor PostgreSQL-servers.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Wat is een Azure-Database voor PostgreSQL-server?
Een Azure-Database voor PostgreSQL-server is een centrale beheerdersrechten voor meerdere databases. Het is dezelfde PostgreSQL server constructie die u mogelijk nagegaan wat in de lokale wereld. In het bijzonder de PostgreSQL-service wordt beheerd, prestaties garanties biedt, beschrijft de toegangs- en -functies op het niveau van de server.

Een Azure-Database voor PostgreSQL-server:

- In een Azure-abonnement is gemaakt.
- Is de bovenliggende resource voor databases.
- Biedt een naamruimte voor databases.
- Is een container met sterke levensduur semantiek - een server verwijderen en de ingesloten databases worden verwijderd.
- Collocates resources in een regio.
- Biedt een eindpunt voor de verbinding voor server en database-toegang (. postgresql.database.azure.com).
- Voorziet in het bereik voor management-beleidsregels die van toepassing op de databases: aanmelding, firewall, gebruikers, rollen, configuraties, enzovoort.
- Is beschikbaar in meerdere versies. Zie voor meer informatie [ondersteunde versies van PostgreSQL-database](concepts-supported-versions.md).
- Kan worden uitgebreid door gebruikers. Zie voor meer informatie [PostgreSQL extensies](concepts-extensions.md).

U kunt een of meerdere databases maken binnen een Azure-Database voor PostgreSQL-server. U kunt kiezen voor het maken van één database per server om gebruik te maken van alle resources of voor meerdere databases om de resources te delen. De prijzen is gestructureerde per server, op basis van de configuratie van prijscategorie, compute-eenheden, en opslag (GB). Zie voor meer informatie [Prijscategorieën](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Hoe ik verbinding maken en verifiëren met een Azure-Database voor PostgreSQL-server?
De volgende elementen zorgt het veilige toegang tot uw database:

|||
|:--|:--|
| **Verificatie en autorisatie** | Azure-Database voor PostgreSQL-server ondersteunt systeemeigen PostgreSQL-verificatie. U kunt verbinding maken en verifiëren bij de server met de server admin aanmelden. |
| **Protocol** | De service ondersteunt een protocol op basis van een bericht dat wordt gebruikt door PostgreSQL. |
| **TCP/IP** | Het protocol wordt ondersteund via TCP/IP en via Unix-domain-sockets. |
| **Firewall** | Ter bescherming van uw gegevens, een firewallregel voorkomt u dat alle toegang tot uw server en de databases, totdat u opgeven welke computers over de machtiging beschikken. Zie [Azure Database voor firewallregels voor PostgreSQL Server](concepts-firewall-rules.md). |

## <a name="how-do-i-manage-a-server"></a>Hoe kan ik een server beheren?
U kunt Azure-Database voor PostgreSQL-servers beheren met behulp van de [Azure-portal](https://portal.azure.com) of de [Azure CLI](/cli/azure/postgres).

## <a name="server-parameters"></a>Serverparameters
De parameters van de server PostgreSQL bepalen de configuratie van de server. In de Azure-Database voor PostgreSQL, de lijst met parameters kan worden bekeken en bewerkt met behulp van de Azure-portal of Azure CLI. 

Als een beheerde service voor Postgres configureerbare parameters in Azure-Database voor PostgreSQL zijn een subset van de parameters in een lokaal exemplaar van de Postgres (Zie voor meer informatie over de Postgres parameters voor de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.6/static/runtime-config.html)). Uw Azure-Database voor PostgreSQL-server is ingeschakeld met de standaardwaarden voor elke parameter op maken. Parameters waarvoor een server opnieuw opstarten of beheerder toegang wijzigingen van kracht te laten door de gebruiker kan niet worden geconfigureerd.


## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service [Azure-Database voor PostgreSQL overzicht](overview.md).
- Voor informatie over specifieke resource quota's en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](concepts-service-tiers.md).
- Zie voor informatie over verbinding maken met de service, [verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md).
- Weergeven en bewerken van de parameters van de server via [Azure-portal](howto-configure-server-parameters-using-portal.md) of [Azure CLI](howto-configure-server-parameters-using-cli.md).
