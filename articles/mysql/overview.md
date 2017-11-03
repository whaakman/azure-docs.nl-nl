---
title: Overzicht van Azure-Database voor relationele databaseservice voor MySQL | Microsoft Docs
description: Overzicht van de Azure-Database voor relationele databaseservice voor MySQL.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/20/2017
ms.custom: mvc
ms.openlocfilehash: 2a9efdd9285dfa5fca450ede64e5f6ee54cbc72b
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2017
---
# <a name="what-is-azure-database-for-mysql"></a>Wat is Azure voor MySQL-Database?
Azure MySQL-Database is een relationele database-service in de Microsoft-cloud op basis van de [MySQL Community Edition](https://www.mysql.com/products/community/) database-engine. Deze service is in de openbare preview. Azure MySQL-Database biedt:

- Ingebouwde hoge beschikbaarheid met zonder extra kosten.
- Voorspelbare prestaties, met behulp van betalen naar gebruik prijzen liggen.
- Schalen op elk gewenst moment binnen enkele seconden.
- Ter bescherming van gevoelige gegevens in rust en in beweging beveiligd.
- Automatische back-ups en punt-in-time-herstel tot maximaal 35 dagen.
- Bedrijfsniveau beveiliging en naleving.

Deze mogelijkheden nodig bijna geen beheer en alle vindt u zonder extra kosten. Hiermee kunt u zich richten op sneller ontwikkelen en de tijd op de markt implementatietijd plaats toewijzen kostbare tijd en hulpmiddelen voor het beheren van virtuele machines en infrastructuur. U kunt bovendien blijven voor het ontwikkelen van uw toepassing met de open-source hulpprogramma's en het platform van uw keuze leveren snel en efficiënt uw bedrijfsbehoeften over te leren van nieuwe vaardigheden.

![Azure conceptueel diagram van MySQL-Database](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Dit artikel bevat een inleiding tot Azure-Database voor MySQL belangrijkste concepten en functies met betrekking tot prestaties, schaalbaarheid en beheerbaarheid, met koppelingen naar meer gedetailleerde informatie. Deze snelstartgidsen om aan de slag te zien:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

Zie voor een set van Azure CLI-voorbeelden:
- [Azure CLI-voorbeelden voor Azure-Database voor MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Prestaties en schaal aanpassen binnen enkele seconden
In voorbeeld van de Azure-Database voor de MySQL-service biedt twee Servicelagen: Basic en Standard. Elke laag biedt verschillende prestaties en mogelijkheden voor lichte tot zware workloads van databases. U kunt uw eerste app ontwikkelen op een kleine database voor enkele euro's per maand en past u de schaal aan de behoeften van uw oplossing. Dynamische schaalbaarheid kunt transparant reageren op snel veranderende resourcevereisten van uw database. U betaalt alleen voor de resources die u nodig en alleen wanneer u deze nodig hebt. Zie [Prijscategorieën](concepts-service-tiers.md) voor meer informatie.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
Hoe bepaal u wanneer omhoog of omlaag schalen? U de ingebouwde prestaties bewaken en waarschuwen functies, gecombineerd met de prestatieclassificaties op basis van de Compute-eenheden gebruiken. Met behulp van deze hulpprogramma's, u kunt snel inzicht in de impact van Compute eenheden omhoog schalen of naar beneden op basis van de prestatiebehoeften van uw huidige of de verwachte. Zie [waarschuwingen](howto-alert-on-metric.md) voor meer informatie.

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf
Azure toonaangevende-99,99% beschikbaarheid serviceovereenkomst (SLA) mogelijk gemaakt door een wereldwijd netwerk van door Microsoft beheerde datacenters, wordt voorkomen dat uw app met 24/7. Met elke Azure-Database voor de MySQL-server is u profiteren van de ingebouwde beveiliging, fouttolerantie en gegevensbeveiliging die u anders moet kopen of ontwerpen, bouwen en beheren. Met Azure-Database voor MySQL, kunt u herstellen punt in tijd voor het herstellen van een server naar een eerdere toestand, tot 35 dagen geleden.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
Azure-database-services hebben een traditie van gegevensbeveiliging die Azure-Database voor MySQL beschermt, met functies die de toegang beperken, gegevens in rust en in beweging beveiligen en helpt u bij activiteit controleren. Ga naar de [Azure Vertrouwenscentrum](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) voor informatie over de beveiliging van de Azure platform.

De Azure-Database voor de MySQL-service gebruikt de versleuteling van opslag voor gegevens in rust. Gegevens, inclusief back-ups worden op schijf (met uitzondering van tijdelijke bestanden die zijn gemaakt door de engine tijdens het uitvoeren van query's) versleuteld. De service gebruikt AES 256-bits codering die is opgenomen in Azure storage-versleuteling en de sleutels zijn beheerd door het systeem. Versleuteling van opslag is altijd ingeschakeld en kan niet worden uitgeschakeld.

De Azure-Database voor de MySQL-service is standaard geconfigureerd om te vereisen [SSL verbindingsbeveiliging](./concepts-ssl-connection-security.md) voor gegevens in beweging via het netwerk. Afdwingen van SSL-verbindingen tussen uw database-server en de clienttoepassingen helpt te beschermen tegen 'man-in het midden'-aanvallen door het versleutelen van de gegevensstroom tussen de server en uw toepassing.  Desgewenst kunt u uitschakelen dat SSL vereist voor het verbinden met uw database-service als de clienttoepassing biedt geen ondersteuning voor SSL-verbindingen.

## <a name="next-steps"></a>Volgende stappen
Nu u een inleiding tot Azure-Database voor MySQL lezen en de vraag 'Wat is Azure-Database voor MySQL?' hebt beantwoord, bent u klaar om te:
- Zie de pagina met prijzen kosten vergelijkingen en rekenmachines. [Prijzen](https://azure.microsoft.com/pricing/details/mysql/)
- Aan de slag door het maken van uw eerste server. [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Bouw uw eerste app met behulp van uw voorkeurstaal: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md)  |  [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [gaan](./connect-go.md)
