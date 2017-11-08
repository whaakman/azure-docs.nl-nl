---
title: Overzicht van Azure-Database voor relationele databaseservice voor PostgreSQL | Microsoft Docs
description: Biedt een overzicht van Azure-Database voor relationele databaseservice voor PostgreSQL.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 10/20/2017
ms.openlocfilehash: 9aa24dd10ef29c716c05cafeb84e0beb23d50628
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Wat is Azure Database voor PostgreSQL?

Azure PostgreSQL-Database is een relationele database-service in de Microsoft cloud gebouwd voor ontwikkelaars op basis van de community-versie van de open-source [PostgreSQL](https://www.postgresql.org/) database-engine. Deze service is in de openbare preview. Azure PostgreSQL-Database biedt:

- Ingebouwde hoge beschikbaarheid met zonder extra kosten.
- Voorspelbare prestaties, met behulp van betalen naar gebruik prijzen liggen.
- Schalen op elk gewenst moment binnen enkele seconden.
- Ter bescherming van gevoelige gegevens in rust en in beweging beveiligd.
- Automatische back-ups en punt-in-time-herstel tot maximaal 35 dagen.
- Bedrijfsniveau beveiliging en naleving.

Alle deze mogelijkheden nodig bijna geen beheer en alle vindt u zonder extra kosten. Deze mogelijkheden kunnen u zich richten op snelle ontwikkeling van toepassingen en de tijd op de markt versnellen, plaats u kostbare tijd en hulpmiddelen voor het beheren van virtuele machines en infrastructuur. U kunt bovendien blijven te ontwikkelen van uw toepassing met de open-source hulpprogramma's en het platform van uw keuze en leveren met de snelheid en uw bedrijf nodig is zonder te leren van nieuwe vaardigheden efficiëntie. 

Dit artikel bevat een inleiding tot Azure-Database voor PostgreSQL belangrijkste concepten en functies met betrekking tot prestaties, schaalbaarheid en beheerbaarheid. Deze snelstartgidsen om aan de slag te zien:

- [Een Azure-Database maken voor PostgreSQL met Azure portal](quickstart-create-server-database-portal.md)
- [Een Azure-Database maken voor PostgreSQL met de Azure CLI](quickstart-create-server-database-azure-cli.md)

Zie voor een set van Azure CLI-voorbeelden:

- [Azure CLI-voorbeelden voor Azure-Database voor PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Prestaties en schaal aanpassen binnen enkele seconden
In voorbeeld van de Azure-Database voor PostgreSQL-service biedt twee Servicelagen: Basic en Standard. Elke laag biedt verschillende prestaties en mogelijkheden voor lichte tot zware workloads van databases. U kunt uw eerste app ontwikkelen op een kleine database voor enkele euro's per maand en past u de schaal aan de behoeften van uw oplossing. Dynamische schaalbaarheid kunt transparant reageren op snel veranderende resourcevereisten van uw database. U betaalt alleen voor de resources die u nodig en alleen wanneer u deze nodig hebt. Zie [Prijscategorieën](concepts-service-tiers.md) voor meer informatie.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
Hoe bepaal u wanneer omhoog of omlaag schalen? U de ingebouwde prestaties bewaken en waarschuwen functies, gecombineerd met de prestatieclassificaties op basis van de Compute-eenheden gebruiken. Met behulp van deze hulpprogramma's, u kunt snel inzicht in de impact van Compute eenheden omhoog schalen of naar beneden op basis van de prestatiebehoeften van uw huidige of de verwachte. Zie [waarschuwingen](howto-alert-on-metric.md) voor meer informatie.

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf
Azure toonaangevende-99,99% beschikbaarheid (niet beschikbaar in preview) serviceovereenkomst (SLA) mogelijk gemaakt door een wereldwijd netwerk van door Microsoft beheerde datacenters, wordt voorkomen dat uw app met 24/7. Met elke Azure-Database voor PostgreSQL-server is u profiteren van de ingebouwde beveiliging, fouttolerantie en gegevensbeveiliging die u anders moet kopen of ontwerpen, bouwen en beheren. Met Azure-Database voor PostgreSQL biedt elke servicelaag een uitgebreide set functies voor zakelijke continuïteit en opties die u gebruiken kunt om te laten en wordt uitgevoerd en blijf op deze manier. Met behulp van [herstelpunten](howto-restore-server-portal.md) kunt u een database terugzetten naar een eerdere toestand, tot 35 dagen geleden. Als er een storing optreedt in het datacenter die als host fungeert voor uw databases, kunt u bovendien databases herstellen met geografisch redundante exemplaren van recente back-ups.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
Azure-database-services hebben een traditie van gegevensbeveiliging dat Azure-Database voor PostgreSQL, met functies die de toegang wordt beperkt, gegevens in rust en in beweging beveiligen en activiteitsbewaking. Ga naar de [Azure Vertrouwenscentrum](https://www.microsoft.com/TrustCenter/Security/default.aspx) voor informatie over de beveiliging van de Azure platform.

De Azure-Database voor PostgreSQL-service gebruikt de versleuteling van opslag voor gegevens in rust. Gegevens met inbegrip van back-ups worden op schijf (met uitzondering van tijdelijke bestanden gemaakt door de engine tijdens het uitvoeren van query's) versleuteld. De service gebruikt AES 256-bits codering die is opgenomen in Azure storage-versleuteling en de sleutels zijn beheerd door het systeem. Versleuteling van opslag is altijd ingeschakeld en kan niet worden uitgeschakeld.

De Azure-Database voor PostgreSQL-service is standaard geconfigureerd om te vereisen [SSL verbindingsbeveiliging](./concepts-ssl-connection-security.md) voor gegevens in beweging via het netwerk. Afdwingen van SSL-verbindingen tussen uw database-server en client-toepassingen beschermt tegen 'man-in het midden'-aanvallen door het versleutelen van de gegevensstroom tussen de server en uw toepassing.  Desgewenst kunt u uitschakelen dat SSL vereist voor het verbinden met uw database-service als de clienttoepassing biedt geen ondersteuning voor SSL-verbindingen.

## <a name="next-steps"></a>Volgende stappen
- Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/postgresql/) kosten vergelijkingen en rekenmachines.
- Aan de slag door [maken van uw eerste Azure-Database voor PostgreSQL](./quickstart-create-server-database-portal.md).
- Bouw uw eerste app in Python, PHP, Ruby, C\#, Java, Node.js: [verbindingsbibliotheken](./concepts-connection-libraries.md)
