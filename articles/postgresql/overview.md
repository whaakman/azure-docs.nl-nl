---
title: Overzicht van de relationele databaseservice Azure Database for PostgreSQL
description: Dit biedt een overzicht van de relationele databaseservice Azure Database for PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 05/06/2019
ms.openlocfilehash: f4023fa84215a0319669de0d812d8306b62278e3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073273"
---
# <a name="what-is-azure-database-for-postgresql"></a>Wat is Azure Database for PostgreSQL?
Azure Database voor PostgreSQL is een relationele databaseservice in de Microsoft cloud die is gebouwd voor ontwikkelaars. Deze is gebaseerd op de communityversie van open-source [PostgreSQL](https://www.postgresql.org/) database-engine en is beschikbaar in twee implementatieopties: Single-Server en grootschalige (Citus) (preview).

## <a name="azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL - één Server
De Implementatieoptie voor één Server biedt:

- Ingebouwde hoge beschikbaarheid zonder extra kosten (SLA van 99,99%)
- Voorspelbare prestaties, tegen all-inclusive prijzen op basis van betalen per gebruik
- Verticale schaal naar wens binnen enkele seconden
- Bewaking en waarschuwingen voor het evalueren van snel de gevolgen van omhoog schalen
- Beveiliging ter bescherming van gevoelige niet-actieve en actieve gegevens
- Automatische back-ups en herstel naar een tijdstip tot 35 dagen geleden
- Beveiliging en naleving van enterprise-kwaliteit

Alle deze mogelijkheden vereisen nauwelijks beheer, en alles wordt zonder extra kosten geleverd. Hiermee kunt u zich kunt richten op het snel ontwikkelen van toepassingen en aan het verkorten van de tijd op de markt brengen, in plaats van kostbare tijd en resources voor het beheren van virtuele machines en infrastructuur. U kunt blijven ontwikkelen van uw toepassing met de open-source hulpprogramma's en het platform van uw keuze, zonder nieuwe vaardigheden moet aanleren.

De Implementatieoptie voor één Server biedt drie categorieën: Basic, Algemeen gebruik en Geoptimaliseerd voor geheugen. Elke categorie biedt verschillende resources ter ondersteuning van de workloads van uw databases. U kunt uw eerste app op een kleine database bouwen voor een paar euro met maand en vervolgens de schaal ervan aanpassen om aan de vereisten van uw oplossing te voldoen. Doordat de schaalbaarheid dynamisch is, kan uw database op een transparante manier reageren op snel veranderende resourcevereisten. U betaalt alleen voor de resources die u nodig hebt op het moment dat u ze nodig hebt. Zie  [Prijscategorieën](concepts-pricing-tiers.md) voor meer informatie.

## <a name="azure-database-for-postgresql---hyperscale-citus-preview"></a>Azure Database voor PostgreSQL - grootschalige (Citus) (preview)
Query's schalen de optie grootschalige (Citus) horizontaal over meerdere machines met behulp van sharding. De query-engine parallelizes binnenkomende SQL-query's op deze servers voor sneller antwoorden op grote gegevenssets. Deze fungeert toepassingen waarvoor een grotere schaal en prestaties, in het algemeen werkbelastingen die nadert-- of al meer dan--100 GB aan gegevens.

De Implementatieoptie grootschalige (Citus) biedt:

- Horizontaal schalen over meerdere machines met behulp van sharding
- Query-parallellisatie over deze servers voor sneller antwoorden op grote gegevenssets
- Uitstekende ondersteuning voor toepassingen met meerdere tenants, realtime operationele analyses en hoge doorvoer transactionele werkbelastingen

Toepassingen die zijn ontwikkeld voor PostgreSQL gedistribueerde query's op grote schaal (Citus uitvoeren kunt) met standaard [verbindingsbibliotheken](./concepts-connection-libraries.md) en minimale wijzigingen worden aangebracht.

Houd er rekening mee dat zeer grootschalige (Citus) in openbare preview en als zodanig is biedt nog geen SLA.

## <a name="data-security"></a>Gegevensbeveiliging
Azure Database voor PostgreSQL beschermt de services Azure database traditiegetrouw gegevensbeveiliging. Het bevat functies voor toegangsbeperking, inactieve gegevens en in-motion beveiligen en activiteitsbewaking. Bezoek het [Vertrouwenscentrum van Azure](https://azure.microsoft.com/overview/trusted-cloud/) voor informatie over de beveiliging van het Azure-platform.

De Azure Database for PostgreSQL-service gebruikt opslagversleuteling voor data-at-rest en voldoet aan FIPS 140-2. Gegevens, met inbegrip van back-ups worden op schijf versleuteld. De service gebruikt de AES-256-bits codering opgenomen in Azure storage-versleuteling, en de sleutels zijn beheerd door het systeem. Opslagversleuteling is altijd actief en kan niet worden uitgeschakeld. Standaard vereist de Azure Database for PostgreSQL-service beveiligde verbindingen voor gegevens in beweging zowel via het netwerk en tussen de database en client-toepassing.

## <a name="contacts"></a>Contactpersonen
Voor vragen of suggesties over het werken met Azure Database for PostgreSQL, stuurt u een e-mailbericht met de Azure Database voor PostgreSQL-Team ([ @Ask Azure DB voor PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Dit adres is voor algemene vragen in plaats van ondersteuningstickets.

Bovendien, houd rekening met deze punten van de contactpersoon die waar nodig:
- Neem contact op met ondersteuning van Azure of een probleem met uw account [bestand een ticket van de Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Als u feedback wilt geven of een nieuwe functie wilt aanvragen, maakt u een vermelding via [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

## <a name="next-steps"></a>Volgende stappen
- Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/postgresql/) voor kostenvergelijkingen en calculators.
- Aan de slag met het maken van uw eerste Azure Database for PostgreSQL [één Server](./quickstart-create-server-database-portal.md) of [grootschalige (Citus) (preview)](./quickstart-create-hyperscale-portal.md)
- Bouw uw eerste app in Python, PHP, Ruby, C\#, Java, Node.js: [verbindingsbibliotheken](./concepts-connection-libraries.md)
