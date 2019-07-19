---
title: Replica's in Azure Database for MariaDB lezen
description: In dit artikel wordt het lezen van replica's voor Azure Database for MariaDB beschreven.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 07/12/2019
ms.openlocfilehash: e6bbe15727a6f989d8c16c67591d39d7870d5708
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67874899"
---
# <a name="read-replicas-in-azure-database-for-mariadb"></a>Replica's in Azure Database for MariaDB lezen

Met de functie replica lezen kunt u gegevens van een Azure Database for MariaDB server repliceren naar een alleen-lezen server. U kunt van de hoofd server naar Maxi maal vijf replica's repliceren. Replica's worden asynchroon bijgewerkt met behulp van de MariaDB-engine van het binaire logboek bestand (binlog) met de globale trans actie-ID (GTID). Zie het [overzicht van binlog-replicatie](https://mariadb.com/kb/en/library/replication-overview/)voor meer informatie over binlog-replicatie.

> [!IMPORTANT]
> U kunt een lees replica maken in dezelfde regio als uw hoofd server of in andere Azure-regio's van uw keuze. Het lezen van replica's (dezelfde regio en kruis regio) bevindt zich momenteel in de open bare preview.

Replica's zijn nieuwe servers die u op dezelfde manier beheert als gewone Azure Database for MariaDB servers. Voor elke Lees replica wordt u gefactureerd voor de ingerichte Compute in vCores en Storage in GB/maand.

Raadpleeg de [documentatie voor MariaDB-replicatie](https://mariadb.com/kb/en/library/gtid/)voor meer informatie over GTID-replicatie.

## <a name="when-to-use-a-read-replica"></a>Wanneer moet u een lees replica gebruiken?

De functie voor het lezen van replica's helpt bij het verbeteren van de prestaties en schaal baarheid van Lees bare werk belastingen. Lees werkbelastingen kunnen worden geïsoleerd voor de replica's, terwijl schrijf werkbelastingen kunnen worden omgeleid naar de Master.

Een veelvoorkomend scenario is om BI-en analytische werk belastingen de Lees replica te laten gebruiken als gegevens bron voor rapportage.

Omdat replica's alleen-lezen zijn, worden ze niet rechtstreeks op de Master gereduceerd. Deze functie is niet gericht op write-intensieve workloads.

De functie replica lezen maakt gebruik van asynchrone replicatie. De functie is niet bedoeld voor synchrone replicatie scenario's. Er is een meet bare vertraging tussen het hoofd en de replica. De gegevens op de replica worden uiteindelijk consistent met de gegevens op de Master. Gebruik deze functie voor werk belastingen die deze vertraging kunnen bevatten.

Bij het lezen van replica's kan uw plan voor herstel na nood gevallen worden verbeterd. Als er sprake is van een regionale nood geval en uw master server niet beschikbaar is, kunt u uw workload door sturen naar een replica in een andere regio. Als u dit wilt doen, laat u de replica schrijf bewerkingen accepteren met behulp van de functie voor het stoppen van de replicatie. U kunt uw toepassing vervolgens omleiden door de connection string bij te werken. Meer informatie vindt u in de sectie [stoppen van replicatie](#stop-replication) .

## <a name="create-a-replica"></a>Replica's maken

Als een master server geen bestaande replica servers heeft, wordt de Master eerst opnieuw opgestart om zichzelf voor te bereiden voor replicatie.

Wanneer u de werk stroom voor het maken van de replica start, wordt er een lege Azure Database for MariaDB-server gemaakt. De nieuwe server wordt gevuld met de gegevens die zich op de hoofd server bevonden. De aanmaak tijd is afhankelijk van de hoeveelheid gegevens op de Master en de tijd sinds de laatste wekelijkse volledige back-up. De tijd kan variëren van een paar minuten tot enkele uren.

> [!NOTE]
> Als u geen opslag waarschuwing hebt ingesteld op uw servers, kunt u dit het beste doen. De waarschuwing meldt u wanneer een server de opslag limiet nadert, die van invloed is op de replicatie.

Meer informatie over [het maken van een lees replica in de Azure Portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Verbinding maken met een replica

Wanneer u een replica maakt, neemt deze de firewall regels of het VNet-service-eind punt van de hoofd server niet over. Deze regels moeten onafhankelijk worden ingesteld voor de replica.

De replica neemt het beheerders account over van de hoofd server. Alle gebruikers accounts op de hoofd server worden gerepliceerd naar de replica's die worden gelezen. U kunt alleen verbinding maken met een lees replica met behulp van de gebruikers accounts die beschikbaar zijn op de master server.

U kunt verbinding maken met de replica door de hostnaam en een geldig gebruikers account te gebruiken, net zoals bij een gewone Azure Database for MariaDB-server. Voor een server met de naam **myreplica** met de gebruikers naam **myadmin**van de beheerder kunt u verbinding maken met de replica met behulp van de MySQL cli:

```bash
mysql -h myreplica.mariadb.database.azure.com -u myadmin@myreplica -p
```

Voer bij de prompt het wacht woord voor het gebruikers account in.

## <a name="monitor-replication"></a>Replicatie controleren

Azure Database for MariaDB levert de **replicatie vertraging in seconden** metric in azure monitor. Deze metriek is alleen beschikbaar voor replica's.

Deze metrische gegevens worden berekend met `seconds_behind_master` behulp van de beschik bare metrische gegevens in de opdracht van `SHOW SLAVE STATUS` MariaDB.

Stel een waarschuwing in om u te informeren wanneer de replicatie vertraging een waarde bereikt die niet geschikt is voor uw werk belasting.

## <a name="stop-replication"></a>Replicatie stoppen

U kunt de replicatie tussen een Master en een replica stoppen. Nadat de replicatie tussen een hoofd server en een lees replica is gestopt, wordt de replica een zelfstandige server. De gegevens op de zelfstandige server zijn de gegevens die beschikbaar zijn op de replica op het moment dat de opdracht stop-replicatie werd gestart. De zelfstandige server is niet actief bij de hoofd server.

Wanneer u ervoor kiest om de replicatie naar een replica te stoppen, gaan alle koppelingen naar het vorige hoofd object en andere replica's verloren. Er is geen automatische failover tussen een hoofd database en de replica.

> [!IMPORTANT]
> De zelfstandige server kan niet opnieuw in een replica worden gemaakt.
> Voordat u de replicatie op een lees replica stopt, moet u ervoor zorgen dat de replica over alle gegevens beschikt die u nodig hebt.

Meer informatie over het [stoppen van replicatie naar een replica](howto-read-replicas-portal.md).

## <a name="considerations-and-limitations"></a>Overwegingen en beperkingen

### <a name="pricing-tiers"></a>Prijscategorieën

Het lezen van replica's is momenteel alleen beschikbaar in de prijs Categorieën Algemeen en geoptimaliseerd voor geheugen.

### <a name="master-server-restart"></a>Hoofd server opnieuw opstarten

Wanneer u een replica maakt voor een model zonder bestaande replica's, wordt de Master eerst opnieuw opgestart om zichzelf voor te bereiden voor replicatie. Houd er rekening mee dat u deze bewerkingen uitvoert tijdens een rustige periode.

### <a name="new-replicas"></a>Nieuwe replica's

Er wordt een lees replica gemaakt als een nieuwe Azure Database for MariaDB-server. Een bestaande server kan niet worden gemaakt in een replica. Het is niet mogelijk om een replica van een andere Lees replica te maken.

### <a name="replica-configuration"></a>Replica configuratie

Een replica wordt gemaakt met behulp van dezelfde server configuratie als de Master. Nadat een replica is gemaakt, kunnen verschillende instellingen onafhankelijk van de hoofd server worden gewijzigd: generatie van compute, vCores, opslag, back-up van Bewaar periode en Engine versie van MariaDB. De prijs categorie kan ook onafhankelijk worden gewijzigd, met uitzonde ring van of van de Basic-laag.

> [!IMPORTANT]
> Voordat een configuratie van een hoofd server wordt bijgewerkt naar nieuwe waarden, moet u de replica configuratie bijwerken naar de waarden gelijk of hoger. Met deze actie zorgt u ervoor dat de replica alle wijzigingen kan aanbrengen die in de master zijn aangebracht.

### <a name="stopped-replicas"></a>Gestopte replica's

Als u de replicatie tussen een hoofd server en een lees replica stopt, wordt de gestopte replica een zelfstandige server die zowel lees-als schrijf bewerkingen accepteert. De zelfstandige server kan niet opnieuw in een replica worden gemaakt.

### <a name="deleted-master-and-standalone-servers"></a>Verwijderde Master-en zelfstandige servers

Wanneer een master server wordt verwijderd, wordt replicatie gestopt voor alle replica's. Deze replica's worden zelfstandige servers. De hoofd server zelf wordt verwijderd.

### <a name="user-accounts"></a>Gebruikersaccounts

Gebruikers op de hoofd server worden gerepliceerd naar de Lees replica's. U kunt alleen verbinding maken met een lees replica met behulp van de beschik bare gebruikers accounts op de master server.

### <a name="server-parameters"></a>Server parameters

Om te voor komen dat gegevens kunnen worden gesynchroniseerd en om mogelijke gegevens verlies of-beschadiging te voor komen, worden bepaalde server parameters vergrendeld bij het gebruik van replica's voor lezen.

De volgende server parameters zijn vergrendeld op de Master-en replica servers:
- [`innodb_file_per_table`](https://mariadb.com/kb/en/library/innodb-system-variables/#innodb_file_per_table) 
- [`log_bin_trust_function_creators`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#log_bin_trust_function_creators)

De [`event_scheduler`](https://mariadb.com/kb/en/library/server-system-variables/#event_scheduler) para meter is vergrendeld op de replica servers.

### <a name="other"></a>Overige

- Het maken van een replica van een replica wordt niet ondersteund.
- In-Memory tabellen kunnen ertoe leiden dat replica's niet meer synchroon zijn. Dit is een beperking van de MariaDB-replicatie technologie.
- Zorg ervoor dat de hoofd Server tabellen primaire sleutels hebben. Als er geen primaire sleutels zijn, kan dit leiden tot replicatie latentie tussen de hoofd-en replicas.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [het maken en beheren van Lees replica's met behulp van de Azure Portal](howto-read-replicas-portal.md)
