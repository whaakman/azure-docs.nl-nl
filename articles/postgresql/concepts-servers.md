---
title: Concepten van de server in Azure Database for PostgreSQL
description: In dit artikel vindt u overwegingen en richtlijnen voor het configureren en beheren van Azure Database voor PostgreSQL-servers.
services: postgresql
author: rachel-msft
ms.author: raagyema
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/27/2018
ms.openlocfilehash: 8fcb5e8371d6c813eb7f0ab4d23a5aac5c41fb3b
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404636"
---
# <a name="azure-database-for-postgresql-servers"></a>Azure Database for PostgreSQL-servers
In dit artikel vindt u overwegingen en richtlijnen voor het werken met Azure Database for PostgreSQL-servers.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Wat is een Azure Database for PostgreSQL-server?
Een Azure Database for PostgreSQL-server is een centraal beheerpunt voor meerdere databases. Dit is dezelfde PostgreSQL-server om voor te bereiden die u al bekend mee in de on-premises wereld wellicht. Met name de PostgreSQL-service wordt beheerd, biedt prestaties, toegang en-functies op het niveau van de server.

Een Azure Database for PostgreSQL-server:

- Wordt gemaakt binnen een Azure-abonnement.
- Is de bovenliggende resource voor databases.
- Biedt een naamruimte voor databases.
- Is een container met sterke levensduursemantiek; d.w.z. - een server verwijderen en de ingesloten databases worden verwijderd.
- Groepeert resources in een regio.
- Biedt een verbindingseindpunt voor server en database-toegang (. postgresql.database.azure.com).
- Vindt u het bereik voor management-beleid die van toepassing op de betreffende databases zijn: aanmelding, firewall, gebruikers, rollen, configuraties, enzovoort.
- Is beschikbaar in meerdere versies. Zie voor meer informatie, [ondersteunde versies van PostgreSQL-database](concepts-supported-versions.md).
- Kan worden uitgebreid door gebruikers. Zie voor meer informatie, [PostgreSQL-extensies](concepts-extensions.md).

Binnen een Azure Database for PostgreSQL-server, kunt u een of meerdere databases maken. U kunt kiezen voor het maken van één database per server om gebruik te maken van alle resources of voor meerdere databases om de resources te delen. De prijzen zijn gestructureerd per server, op basis van de configuratie van prijzen laag vCores en opslag (GB). Zie voor meer informatie, [Prijscategorieën](./concepts-pricing-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Hoe kan ik verbinding maken en verifiëren met een Azure Database for PostgreSQL-server?
De volgende elementen ervoor te zorgen veilige toegang tot uw database:

|||
|:--|:--|
| **Verificatie en autorisatie** | Azure Database for PostgreSQL-server biedt ondersteuning voor systeemeigen PostgreSQL-verificatie. U kunt verbinding maken en verifiëren bij de server met de serverbeheerder. |
| **Protocol** | De service ondersteunt een protocol op basis van een bericht dat wordt gebruikt door PostgreSQL. |
| **TCP/IP** | Het protocol wordt ondersteund via TCP/IP, en sockets van Unix-domein. |
| **Firewall** | Ter bescherming van uw gegevens, een firewallregel voorkomt dat alle toegang tot uw server en op de betreffende databases, totdat u opgeeft welke computers zijn gemachtigd. Zie [Azure Database for PostgreSQL-Server firewall-regels](concepts-firewall-rules.md). |

## <a name="managing-your-server"></a>Uw server beheren
U kunt Azure Database for PostgreSQL-servers beheren met behulp van de [Azure-portal](https://portal.azure.com) of de [Azure CLI](/cli/azure/postgres).

Tijdens het maken van een server, stelt u up maken van de referenties voor de gebruiker met beheerdersrechten. De gebruiker met beheerdersrechten is de hoogste bevoegdheden-gebruiker die u op de server hebt. Deze behoort tot de rol azure_pg_admin. Deze rol is niet gemachtigd voor volledige beheerder. 

Het kenmerk van de beheerder PostgreSQL is toegewezen aan de azure_superuser, die deel uitmaakt van de beheerde service. U hebt geen toegang aan deze rol.

Een Azure Database for PostgreSQL-server heeft twee standaard-databases: 
- **postgres** -een standaarddatabase die u verbinding met één keer op de server maken kunt wordt gemaakt.
- **azure_maintenance** -deze database wordt gebruikt voor het scheiden van de processen die de beheerde service van gebruikersacties leveren. U hebt geen toegang tot deze database.
- **azure_sys** -een-database voor de Query Store. Deze database worden gegevens niet wanneer de Query Store is uitgeschakeld; Dit is de standaardinstelling. Zie voor meer informatie de [Query Store overzicht](concepts-query-store.md).


## <a name="server-parameters"></a>Serverparameters
De PostgreSQL-serverparameters bepalen de configuratie van de server. In Azure Database for PostgreSQL, wordt de lijst met parameters kan worden bekeken en bewerkt met behulp van de Azure-portal of Azure CLI. 

Als een beheerde service voor Postgres, de configureerbare parameters die in Azure Database for PostgreSQL vormen een subset van de parameters in een lokaal exemplaar van de Postgres (Zie voor meer informatie over de Postgres-parameters voor de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.6/static/runtime-config.html)). Uw Azure Database for PostgreSQL-server is ingeschakeld met standaardwaarden voor alle parameters gemaakt. Sommige parameters waarvoor een server opnieuw opstarten of beheerder toegang tot de wijzigingen worden doorgevoerd door de gebruiker kan niet worden geconfigureerd.


## <a name="next-steps"></a>Volgende stappen
- Zie voor een overzicht van de service, [Azure Database for PostgreSQL-overzicht](overview.md).
- Voor informatie over specifieke resource quota en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](concepts-pricing-tiers.md).
- Zie voor meer informatie over verbinding maken met de service [verbindingsbibliotheken voor Azure Database for PostgreSQL](concepts-connection-libraries.md).
- Weergeven en bewerken van parameters van de server via [Azure-portal](howto-configure-server-parameters-using-portal.md) of [Azure CLI](howto-configure-server-parameters-using-cli.md).
