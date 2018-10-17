---
title: Overzicht van de relationele databaseservice Azure Database for MariaDB
description: Overzicht van de relationele databaseservice Azure Database for MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: overview
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 3649a173d5707179ca8547a8169b7d308c4f7f1c
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249159"
---
# <a name="what-is-azure-database-for-mariadb"></a>Wat is Azure Database for MariaDB?
Azure Database for MariaDB is een relationele databaseservice in de Microsoft Cloud die is gebaseerd op de [MariaDB Community Edition](https://mariadb.org/download/)-database engine. Deze service is beschikbaar in de openbare-previewversie. Azure Database for MariaDB levert:

- Ingebouwde hoge beschikbaarheid zonder extra kosten.
- Voorspelbare prestaties, tegen all-inclusive prijzen op basis van betalen per gebruik.
- Schalen naar behoefte, binnen enkele seconden.
- Ter bescherming van gevoelige niet-actieve en actieve gegevens.
- Automatische back-ups en herstel naar een tijdstip tot 35 dagen geleden.
- Beveiliging en naleving van bedrijfskwaliteit.

Deze mogelijkheden vereisen nauwelijks beheer, en alles wordt zonder extra kosten geleverd. Dankzij deze mogelijkheden kunt u zich richten op het sneller ontwikkelen en op de markt brengen van apps, in plaats van kostbare tijd en middelen in te zetten voor het beheer van virtuele machines en infrastructuur. U kunt bovendien de open-source hulpprogramma's en het platform van uw keuze blijven gebruiken om uw toepassing verder te ontwikkelen, en deze te leveren met de snelheid en efficiëntie die uw bedrijf vereist zonder dat u nieuwe vaardigheden hoeft te leren.

Dit artikel bevat een inleiding tot de belangrijkste concepten en functies van Azure Database for MariaDB met betrekking tot prestaties, schaalbaarheid en beheerbaarheid, met koppelingen naar meer gedetailleerde informatie. Zie deze Quick Starts om snel aan de slag te gaan:
- [Een Azure Database for MariaDB-server maken met behulp van de Azure-portal](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Een Azure Database for MariaDB-server maken met behulp van Azure CLI](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Binnen een paar seconden prestaties en schaal aanpassen
In de preview biedt de Azure Database for MariaDB-service verschillende prijscategorieën: Basic, Algemeen gebruik en Geoptimaliseerd voor geheugen. Elke laag biedt verschillende prestaties en mogelijkheden voor lichte tot zware workloads van databases. U kunt uw eerste app op een kleine database bouwen voor een paar euro met maand en vervolgens de schaal ervan aanpassen om aan de vereisten van uw oplossing te voldoen. Doordat de schaalbaarheid dynamisch is, kan uw database op een transparante manier reageren op snel veranderende resourcevereisten. U betaalt alleen voor de resources die u nodig hebt op het moment dat u ze nodig hebt. Zie [Prijscategorieën](concepts-pricing-tiers.md) voor meer details.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
Hoe bepaalt u wanneer u omhoog of omlaag moet schalen? U maakt gebruik van de ingebouwde functies voor prestatiebewaking en waarschuwingen, in combinatie met de prestatiebeoordelingen op basis van vCores. Met behulp van deze tools kunt u snel beoordelen wat de impact is van het aanpassen van de schaal van vCores op basis van uw huidige of verwachte prestatiebehoeften. <!--See [Alerts](howto-alert-on-metric.md) for details.-->

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf
De toonaangevende serviceovereenkomst (SLA) van Azure met 99,99% beschikbaarheid (niet aangeboden tijdens openbare preview) dankzij een wereldwijd netwerk van door Microsoft beheerde datacenters, zorgt ervoor dat uw app continu (24 uur per dag, 7 dagen per week) beschikbaar blijft. Elke Azure Database for MariaDB-server is voorzien van ingebouwde beveiliging, fouttolerantie en gegevensbeveiliging die u anders zelf zou moeten kopen of ontwerpen, ontwikkelen en beheren. Met Azure Database for MariaDB kunt u met behulp van herstelpunten een server terugzetten naar een eerdere toestand, tot 35 dagen geleden.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
Azure databaseservices hebben traditiegetrouw een uitstekende gegevensbeveiliging. Deze traditie wordt voortgezet in Azure Database for MariaDB, met functies voor toegangsbeperking, het beveiligen van niet-actieve en actieve gegevens en activiteitsbewaking. Bezoek het [Vertrouwenscentrum van Azure](https://www.microsoft.com/en-us/trustcenter/security) voor informatie over de beveiliging van het Azure-platform.

De Azure Database for MariaDB-service gebruikt opslagversleuteling voor niet-actieve gegevens. Gegevens met inbegrip van back-ups worden op schijf versleuteld (met uitzondering van tijdelijke bestanden die door de engine tijdens het uitvoeren van query's worden gemaakt). De service maakt gebruikt van AES 256-bits versleuteling die deel uitmaakt van Azure Storage-versleuteling. De sleutels worden door het systeem beheerd. Opslagversleuteling is altijd actief en kan niet worden uitgeschakeld.

Standaard is de Azure Database for MariaDB-service zo geconfigureerd dat [SSL-verbindingsbeveiliging](./concepts-ssl-connection-security.md) vereist is voor gegevens die zich door het netwerk verplaatsen. Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld. Desgewenst kunt u uitschakelen dat SSL is vereist voor het maken van verbinding met uw databaseservice als uw clienttoepassing geen ondersteuning biedt voor SSL-connectiviteit.

## <a name="contacts"></a>Contactpersonen
Voor eventuele vragen of suggesties over het werken met Azure Database for MariaDB stuurt u een e-mailbericht naar het Azure Database for MariaD-team ([@AskAzure DB for MariaD](mailto:AskAzureDBforMariaDB@service.microsoft.com)). Let op: dit is geen alias voor technische ondersteuning.

Overweeg ook de volgende aanspreekpunten, indien van toepassing:
- Als u contact wilt opnemen met Azure-ondersteuning, kunt u een [ticket indienen vanuit Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Als u een probleem met uw account wilt oplossen, kunt u een [ondersteuningsaanvraag](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) indienen in Azure Portal.
- Als u feedback wilt geven of een nieuwe functie wilt aanvragen, maakt u een vermelding via [UserVoice](https://feedback.azure.com/forums/915439-azure-database-for-mariadb).

## <a name="next-steps"></a>Volgende stappen
Nu u de inleiding tot Azure Database for MariaDB hebt gelezen en weet wat Azure Database for MariaDB is, bent u klaar voor de volgende stappen:
- Zie de pagina met prijzen voor kostenvergelijkingen en calculators. [Prijzen](https://azure.microsoft.com/pricing/details/mariadb/)
- Ga aan de slag met het maken van uw eerste server. [Een Azure Database for MariaDB-server maken met behulp van de Azure-portal](quickstart-create-mariadb-server-database-using-azure-portal.md)

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
