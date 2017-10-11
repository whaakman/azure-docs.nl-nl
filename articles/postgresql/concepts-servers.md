---
title: Concepten van de server in Azure-Database voor PostgreSQL | Microsoft Docs
description: In dit onderwerp worden overwegingen en richtlijnen gegeven voor het werken met Azure-Database voor PostgreSQL-servers.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: c79603a11fa23ad775783157675c07ab67102057
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-servers"></a>Azure-Database voor PostgreSQL-Servers
In dit onderwerp worden overwegingen en richtlijnen gegeven voor het werken met Azure-Database voor PostgreSQL-servers.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Wat is een Azure-Database voor PostgreSQL-server?
Een Azure-Database voor PostgreSQL-server is een centrale beheerdersrechten voor meerdere databases. Het is dezelfde PostgreSQL server constructie die u mogelijk nagegaan wat in de lokale wereld. In het bijzonder de PostgreSQL-service wordt beheerd, prestaties garanties biedt, beschrijft de toegangs- en -functies op serverniveau.

Een Azure-Database voor PostgreSQL-server:

- In een Azure-abonnement is gemaakt.
- Is de bovenliggende resource voor databases.
- Biedt een naamruimte voor databases.
- Is een container met sterke levensduur semantiek - een server verwijderen en de ingesloten databases worden verwijderd.
- Collocates resources in een regio.
- Biedt een eindpunt voor de verbinding voor server en database-toegang (. postgresql.database.azure.com).
- Voorziet in het bereik voor management-beleidsregels die van toepassing op de databases: aanmelding, firewall, gebruikers, rollen, configuraties, enzovoort.
- Is beschikbaar in meerdere versies. Zie voor meer informatie [ondersteund PostgreSQL-databaseversies](concepts-supported-versions.md).
- Kan worden uitgebreid door gebruikers. Zie voor meer informatie [PostgreSQL extensies](concepts-extensions.md).

U kunt een of meerdere databases maken binnen een Azure-Database voor PostgreSQL-server. U kunt kiezen voor het maken van één database per server om gebruik te maken van alle resources of voor meerdere databases om de resources te delen. De prijzen gestructureerde per server, op basis van de configuratie van prijscategorie, compute-eenheden, opslag (GB). Zie voor meer informatie [Prijscategorieën](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Hoe ik verbinding maken en verifiëren met een Azure-Database voor PostgreSQL-server?
De volgende elementen zorgt het veilige toegang tot uw database.

|||
| :-- | :-- |
| **Verificatie en autorisatie** | Azure-Database voor PostgreSQL-server ondersteunt systeemeigen PostgreSQL-verificatie. U kunt verbinding maken en verifiëren bij de server met de server admin aanmelden. |
| **Protocol** | De service ondersteunt een protocol op basis van een bericht dat wordt gebruikt door PostgreSQL. |
| **TCP/IP** | Het protocol wordt ondersteund via TCP/IP en via Unix-domain-sockets. |
| **Firewall** | Ter bescherming van uw gegevens, een firewallregel voorkomt u dat alle toegang tot uw database-server of de databases totdat u opgeven welke computers over machtigingen beschikken. Zie [Azure Database voor firewallregels voor PostgreSQL Server](concepts-firewall-rules.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Hoe kan ik een server beheren?
U kunt Azure-Database voor PostgreSQL-servers beheren met behulp van de Azure-portal of de [Azure CLI](/cli/azure/postgres).

## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service [Azure-Database voor PostgreSQL-overzicht](overview.md)
- Voor informatie over specifieke resource quota's en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](concepts-service-tiers.md)
- Zie voor informatie over verbinding maken met de service, [verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md).
