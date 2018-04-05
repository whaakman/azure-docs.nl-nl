---
title: Overzicht van de relationele databaseservice Azure Database for PostgreSQL
description: Dit biedt een overzicht van de relationele databaseservice Azure Database for PostgreSQL.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 02/28/2018
ms.openlocfilehash: 766373f4b9390e576285db73d9e9e9942eb6624f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="what-is-azure-database-for-postgresql"></a>Wat is Azure Database for PostgreSQL?

Azure Database for PostgreSQL is een relationele databaseservice in de Microsoft Cloud die is gebouwd voor ontwikkelaars op basis van de community-versie van de open-source [PostgreSQL](https://www.postgresql.org/)-database-engine. Azure Database for PostgreSQL heeft het volgende te bieden:

- Ingebouwde hoge beschikbaarheid zonder extra kosten
- Voorspelbare prestaties, tegen all-inclusive prijzen op basis van betalen per gebruik
- Schalen op elk gewenst moment binnen enkele seconden
- Beveiliging ter bescherming van gevoelige niet-actieve en actieve gegevens
- Automatische back-ups en herstel naar een tijdstip tot 35 dagen geleden
- Beveiliging en naleving van enterprise-kwaliteit

Alle deze mogelijkheden vereisen nauwelijks beheer, en alles wordt zonder extra kosten geleverd. Dankzij deze mogelijkheden kunt u zich richten op het sneller ontwikkelen en op de markt brengen van toepassingen, in plaats van kostbare tijd en middelen in te zetten voor het beheer van virtuele machines en infrastructuur. U kunt bovendien de open-source hulpprogramma's en het platform van uw keuze blijven gebruiken om uw toepassing verder te ontwikkelen, en deze te leveren met de snelheid en efficiëntie die uw bedrijf vereist zonder dat u nieuwe vaardigheden hoeft te leren. 

Dit artikel bevat een inleiding tot de belangrijkste concepten en functies van Azure Database for PostgreSQL met betrekking tot prestaties, schaalbaarheid en beheerbaarheid. Zie deze snelstartgidsen om snel aan de slag te gaan:

- [Een Azure Database for PostgreSQL maken met Azure Portal](quickstart-create-server-database-portal.md)
- [Een Azure Database for PostgreSQL maken met Azure CLI](quickstart-create-server-database-azure-cli.md)

Zie de volgende artikelen voor een reeks Azure CLI-voorbeelden:

- [Azure CLI-voorbeelden voor Azure Database for PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Binnen een paar seconden prestaties en schaal aanpassen
Er zijn drie prijscategorieën voor de Azure Database for PostgreSQL-service: Basic, Algemeen gebruik en Geoptimaliseerd voor geheugen. Elke categorie biedt verschillende resources ter ondersteuning van de workloads van uw databases. U kunt uw eerste app op een kleine database bouwen voor een paar euro met maand en vervolgens de schaal ervan aanpassen om aan de vereisten van uw oplossing te voldoen. Doordat de schaalbaarheid dynamisch is, kan uw database op een transparante manier reageren op snel veranderende resourcevereisten. U betaalt alleen voor de resources die u nodig hebt op het moment dat u ze nodig hebt. Zie [Prijscategorieën](concepts-pricing-tiers.md) voor meer details.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
Hoe bepaalt u wanneer u omhoog of omlaag moet schalen? U gebruikt de ingebouwde Azure-functies voor bewaking en waarschuwingen. Met behulp van deze tools kunt u snel beoordelen wat de impact is van het aanpassen van de schaal op basis van uw huidige of verwachte prestatie- of opslagbehoeften. Zie [Waarschuwingen](howto-alert-on-metric.md) voor meer details.

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf
De toonaangevende serviceovereenkomst (SLA) van Azure met 99,99% beschikbaarheid dankzij een wereldwijd netwerk van door Microsoft beheerde datacenters, zorgt ervoor dat uw app continu (24 uur per dag, 7 dagen per week) beschikbaar blijft. Elke Azure Database for PostgreSQL-server is voorzien van ingebouwde beveiliging, fouttolerantie en gegevensbeveiliging die u anders zelf zou moeten kopen of ontwerpen, ontwikkelen en beheren. Elke prijscategorie van Azure Database for PostgreSQL bevat een uitgebreide reeks functies en opties voor bedrijfscontinuïteit, zodat u snel aan de slag kunt gaan en efficiënt kunt werken. Met behulp van [herstelpunten](howto-restore-server-portal.md) kunt u een database terugzetten naar een eerdere toestand, tot 35 dagen geleden. Als er een storing optreedt in het datacenter dat uw databases host, kunt u bovendien databases terugzetten met geografisch redundante back-upkopieën.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
Azure databaseservices hebben traditiegetrouw een uitstekende gegevensbeveiliging. Deze traditie wordt voortgezet in Azure Database for PostgreSQL, met functies voor toegangsbeperking, het beveiligen van niet-actieve en actieve gegevens en activiteitsbewaking. Bezoek het [Vertrouwenscentrum van Azure](https://www.microsoft.com/en-us/trustcenter/security) voor informatie over de beveiliging van het Azure-platform.

De Azure Database for PostgreSQL-service gebruikt opslagversleuteling voor niet-actieve gegevens. Gegevens, met inbegrip van back-ups, worden op schijf versleuteld (met uitzondering van tijdelijke bestanden die door de engine tijdens het uitvoeren van query's worden gemaakt). De service maakt gebruikt van AES 256-bits versleuteling die deel uitmaakt van Azure Storage-versleuteling. De sleutels worden door het systeem beheerd. Opslagversleuteling is altijd actief en kan niet worden uitgeschakeld.

Standaard is de Azure Database for PostgreSQL-service zo geconfigureerd dat [SSL-verbindingsbeveiliging](./concepts-ssl-connection-security.md) is vereist voor gegevens die zich door het netwerk verplaatsen. Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld.  Desgewenst kunt u uitschakelen dat SSL is vereist voor het maken van verbinding met uw databaseservice als uw clienttoepassing geen ondersteuning biedt voor SSL-connectiviteit.

## <a name="next-steps"></a>Volgende stappen
- Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/postgresql/) voor kostenvergelijkingen en calculators.
- Ga aan de slag met het [maken van uw eerste Azure Database for PostgreSQL](./quickstart-create-server-database-portal.md).
- Bouw uw eerste app in Python, PHP, Ruby, C\#, Java, Node.js: [verbindingsbibliotheken](./concepts-connection-libraries.md)
