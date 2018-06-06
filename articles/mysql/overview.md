---
title: Overzicht van Azure-Database voor relationele databaseservice voor MySQL
description: Overzicht van de Azure-Database voor relationele databaseservice voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/02/2018
ms.custom: mvc
ms.openlocfilehash: 0ae9c0168d8f4ff7dbea4c21c91fc70a3d355a5f
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736470"
---
# <a name="what-is-azure-database-for-mysql"></a>Wat is Azure voor MySQL-Database?
Azure MySQL-Database is een relationele database-service in de Microsoft-cloud op basis van de [MySQL Community Edition](https://www.mysql.com/products/community/) database-engine. Azure MySQL-Database biedt:

- Ingebouwde hoge beschikbaarheid zonder extra kosten.
- Voorspelbare prestaties, tegen all-inclusive prijzen op basis van betalen per gebruik.
- Schaal naar behoefte binnen enkele seconden.
- Ter bescherming van gevoelige niet-actieve en actieve gegevens.
- Automatische back-ups en herstel naar een tijdstip tot 35 dagen geleden.
- Beveiliging en naleving van bedrijfskwaliteit.

Deze mogelijkheden nodig bijna geen beheer en alle vindt u zonder extra kosten. Hiermee kunt u zich richten op sneller ontwikkelen en de tijd op de markt implementatietijd plaats toewijzen kostbare tijd en hulpmiddelen voor het beheren van virtuele machines en infrastructuur. U kunt bovendien blijven voor het ontwikkelen van uw toepassing met de open-source hulpprogramma's en het platform van uw keuze leveren snel en efficiënt uw bedrijfsbehoeften over te leren van nieuwe vaardigheden.

![Azure conceptueel diagram van MySQL-Database](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Dit artikel bevat een inleiding tot Azure-Database voor MySQL belangrijkste concepten en functies met betrekking tot prestaties, schaalbaarheid en beheerbaarheid, met koppelingen naar meer gedetailleerde informatie. Zie deze Quick Starts om snel aan de slag te gaan:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

Zie de volgende artikelen voor een reeks Azure CLI-voorbeelden:
- [Azure CLI-voorbeelden voor Azure-Database voor MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Binnen een paar seconden prestaties en schaal aanpassen
De Azure-Database voor de MySQL-service biedt verschillende Servicelagen: Basic, algemeen gebruik en geoptimaliseerd voor geheugen. Elke laag biedt verschillende prestaties en mogelijkheden voor lichte tot zware workloads van databases. U kunt uw eerste app op een kleine database bouwen voor een paar euro met maand en vervolgens de schaal ervan aanpassen om aan de vereisten van uw oplossing te voldoen. Doordat de schaalbaarheid dynamisch is, kan uw database op een transparante manier reageren op snel veranderende resourcevereisten. U betaalt alleen voor de resources die u nodig hebt op het moment dat u ze nodig hebt. Zie [Prijscategorieën](concepts-service-tiers.md) voor meer details.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
Hoe bepaalt u wanneer u omhoog of omlaag moet schalen? U de ingebouwde prestaties bewaken en waarschuwen functies, gecombineerd met de prestatieclassificaties op basis van vCores gebruiken. Deze hulpprogramma's gebruikt, kunt u snel beoordelen de impact van vergroten/verkleinen vCores omhoog of omlaag op basis van de prestatiebehoeften van uw huidige of de verwachte. Zie [Waarschuwingen](howto-alert-on-metric.md) voor meer details.

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf
De toonaangevende serviceovereenkomst (SLA) van Azure met 99,99% beschikbaarheid dankzij een wereldwijd netwerk van door Microsoft beheerde datacenters, zorgt ervoor dat uw app continu (24 uur per dag, 7 dagen per week) beschikbaar blijft. Met elke Azure-Database voor de MySQL-server is u profiteren van de ingebouwde beveiliging, fouttolerantie en gegevensbeveiliging die u anders moet kopen of ontwerpen, bouwen en beheren. Met Azure-Database voor MySQL, kunt u herstellen punt in tijd voor het herstellen van een server naar een eerdere toestand, tot 35 dagen geleden.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
Azure-database-services hebben een traditie van gegevensbeveiliging die Azure-Database voor MySQL beschermt, met functies die de toegang beperken, gegevens in rust en in beweging beveiligen en helpt u bij activiteit controleren. Bezoek het [Vertrouwenscentrum van Azure](https://www.microsoft.com/en-us/trustcenter/security) voor informatie over de beveiliging van het Azure-platform.

De Azure-Database voor de MySQL-service gebruikt de versleuteling van opslag voor gegevens in rust. Gegevens, inclusief back-ups worden op schijf (met uitzondering van tijdelijke bestanden die zijn gemaakt door de engine tijdens het uitvoeren van query's) versleuteld. De service maakt gebruikt van AES 256-bits versleuteling die deel uitmaakt van Azure Storage-versleuteling. De sleutels worden door het systeem beheerd. Opslagversleuteling is altijd actief en kan niet worden uitgeschakeld.

De Azure-Database voor de MySQL-service is standaard geconfigureerd om te vereisen [SSL verbindingsbeveiliging](./concepts-ssl-connection-security.md) voor gegevens in beweging via het netwerk. Afdwingen van SSL-verbindingen tussen uw database-server en de clienttoepassingen helpt te beschermen tegen 'man-in het midden'-aanvallen door het versleutelen van de gegevensstroom tussen de server en uw toepassing. Desgewenst kunt u uitschakelen dat SSL is vereist voor het maken van verbinding met uw databaseservice als uw clienttoepassing geen ondersteuning biedt voor SSL-connectiviteit.

## <a name="contacts"></a>Contactpersonen
Voor vragen of suggesties over het werken met Azure-Database voor MySQL, kan er een e-mail sturen naar de Azure-Database voor MySQL-Team ([ @Ask Azure DB voor MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com)). Houd er rekening mee dat dit niet een alias voor technische ondersteuning is.

Overweeg daarnaast de volgende punten van contactpersoon naar gelang van toepassing:
- Contact opnemen met ondersteuning van Azure [bestand een ticket vanuit de Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- U herstelt een probleem met uw account door het bestand een [ondersteuningsaanvraag](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) in de Azure portal.
- Om feedback te geven of om aan te vragen van nieuwe functies, maakt u een vermelding via [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

## <a name="next-steps"></a>Volgende stappen
Nu u een inleiding tot Azure-Database voor MySQL lezen en de vraag 'Wat is Azure-Database voor MySQL?' hebt beantwoord, bent u klaar om te:
- Zie de pagina met prijzen kosten vergelijkingen en rekenmachines. [Prijzen](https://azure.microsoft.com/pricing/details/mysql/)
- Aan de slag door het maken van uw eerste server. [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Bouw uw eerste app met behulp van uw voorkeurstaal: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md)  |  [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [gaan](./connect-go.md)
