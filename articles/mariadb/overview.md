---
title: Overzicht van de relationele databaseservice Azure Database for MariaDB
description: Overzicht van de relationele databaseservice Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: overview
ms.custom: mvc
ms.date: 03/20/2019
ms.openlocfilehash: a5d00c24531099e66afcb6ccf07cfdf99abd28d1
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "60846239"
---
# <a name="what-is-azure-database-for-mariadb"></a>Wat is Azure Database for MariaDB?

Azure Database for MariaDB is een relationele databaseservice in de Microsoft-cloud. Azure Database voor MariaDB is gebaseerd op de [MariaDB community edition](https://mariadb.org/download/) (beschikbaar onder de licentie GPLv2)-database-engine, versie 10.2.

Azure Database for MariaDB levert:

- Ingebouwde hoge beschikbaarheid zonder extra kosten.
- Voorspelbare prestaties, tegen all-inclusive prijzen op basis van betalen per gebruik.
- Schalen naar behoefte, binnen enkele seconden.
- Beveiliging ter bescherming van gevoelige data-at-rest en in-motion.
- Automatische back-ups en herstel naar een tijdstip tot 35 dagen geleden.
- Beveiliging en naleving van bedrijfskwaliteit.

Deze mogelijkheden vereisen nauwelijks beheer. Ze worden zonder extra kosten aangeboden. Met Azure Database for MariaDB kunt u uw app snel ontwikkelen en sneller op de markt uitbrengen. U hoeft geen kostbare tijd en resources te besteden aan het beheren van virtuele machines en infrastructuur. U kunt uw toepassing ook verder blijven ontwikkelen met behulp van de open source-programma's en het platform van uw keuze. Lever met de snelheid en efficiëntie die uw bedrijf vereist, en dit alles zonder nieuwe vaardigheden te hoeven leren.

Zie voor meer informatie over de belangrijkste concepten en functies in Azure Database for MariaDB, inclusief prestaties, schaalbaarheid en beheerbaarheid, deze snelstartgidsen:

- [Een Azure Database for MariaDB-server maken met behulp van de Azure-portal](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Een Azure Database for MariaDB-server maken met behulp van de Azure CLI](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Binnen een paar seconden prestaties en schaal aanpassen

De Azure Database for MariaDB-service biedt verschillende servicelagen: Basic, Algemeen gebruik en Geoptimaliseerd voor geheugen. Elke laag biedt verschillende prestaties en mogelijkheden voor lichte tot zware workloads van databases. U kunt uw eerste app op een kleine database bouwen voor een paar euro met maand en vervolgens de schaal ervan aanpassen om aan de vereisten van uw oplossing te voldoen. Doordat de schaalbaarheid dynamisch is, kan uw database op een transparante manier reageren op snel veranderende resourcevereisten. U betaalt alleen voor de resources die u nodig hebt op het moment dat u ze nodig hebt. Zie  [Prijscategorieën](concepts-pricing-tiers.md) voor meer informatie.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen

Hoe bepaalt u wanneer u omhoog of omlaag moet schalen? U maakt gebruik van de ingebouwde functies voor prestatiebewaking en waarschuwingen van Azure Database for MariaDB, in combinatie met de prestatiebeoordelingen op basis van vCores. Met behulp van deze tools kunt u snel beoordelen wat de impact is van het aanpassen van de schaal van vCores op basis van uw huidige of verwachte prestatiebehoeften. Zie [Waarschuwingen](howto-alert-metric.md) voor meer details.

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf

De toonaangevende serviceovereenkomst (SLA) van Azure met 99,99% beschikbaarheid wordt mogelijk gemaakt door een wereldwijd netwerk van door Microsoft beheerde datacenters. Het netwerk zorgt ervoor dat uw app 24/7 wordt uitgevoerd. U profiteert van de ingebouwde beveiliging, fouttolerantie en gegevensbeveiliging in Azure Database for MariaDB. Met Azure Database for MariaDB kunt u met behulp van herstelpunten een server terugzetten naar een eerdere toestand, tot 35 dagen geleden.

## <a name="secure-your-data"></a>Uw gegevens beveiligen

Azure-databaseservices hebben traditiegetrouw een uitstekende gegevensbeveiliging die Azure Database for MariaDB voortzet. Azure Database for MariaDB biedt functies voor toegangsbeperking, beveiliging van data-at-rest en in-motion en activiteitsbewaking. Bezoek het [Vertrouwenscentrum van Azure](https://www.microsoft.com/en-us/trustcenter/security) voor informatie over de beveiliging van het Azure-platform.

De Azure Database for MariaDB-service gebruikt opslagversleuteling voor data-at-rest en voldoet aan FIPS 140-2. Gegevens, inclusief back-upgegevens, worden versleuteld op de schijf. (Tijdelijke bestanden die zijn gemaakt door de engine tijdens het uitvoeren van query's zijn niet versleuteld op de schijf.) De service maakt gebruikt van AES 256-bits versleuteling die deel uitmaakt van Azure Storage-versleuteling. De sleutels worden door het systeem beheerd. Opslagversleuteling is altijd actief en kan niet worden uitgeschakeld.

Standaard is Azure Database for MariaDB-service zo geconfigureerd dat [SSL-verbindingsbeveiliging](./concepts-ssl-connection-security.md) vereist is voor gegevens die zich door het netwerk verplaatsen. Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld. Desgewenst kunt u uitschakelen dat SSL is vereist voor het maken van verbinding met uw databaseservice als uw clienttoepassing geen ondersteuning biedt voor SSL-connectiviteit.

## <a name="contacts"></a>Contactpersonen

U kunt eventuele vragen of suggesties over het werken met Azure Database for MariaDB sturen naar het [Azure Database for MariaDB-team](mailto:AskAzureDBforMariaDB@service.microsoft.com) (geen alias voor technische ondersteuning).

U kunt ook contact opnemen met de volgende aanspreekpunten:
- Als u contact wilt opnemen met de ondersteuning voor Azure [opent u een ondersteuningsaanvraag](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) in de Azure-portal.
- Als u een probleem met uw account wilt oplossen, [opent u een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) in de Azure-portal.
- Als u feedback wilt geven of een nieuwe functie wilt aanvragen, maakt u een vermelding in de [Azure-feedbackforums](https://feedback.azure.com/forums/915439-azure-database-for-mariadb).

## <a name="next-steps"></a>Volgende stappen

Nu u een inleiding tot Azure Database for MariaDB hebt gelezen, bent u klaar voor het volgende:
- Dee pagina met [prijzen](https://azure.microsoft.com/pricing/details/mariadb/) bekijken voor kostenvergelijkingen en calculators. 
- Aan de slag gaan met het [maken van uw eerste server](quickstart-create-mariadb-server-database-using-azure-portal.md).

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
