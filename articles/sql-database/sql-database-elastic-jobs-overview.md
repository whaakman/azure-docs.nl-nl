---
title: Uitgeschaalde clouddatabases beheren | Microsoft Docs
description: De service van de taak elastische database gebruiken voor het uitvoeren van een script voor een groep databases.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 10/22/2018
ms.openlocfilehash: 9647522f4b3990d065f292f05934b8d19c691454
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865519"
---
# <a name="managing-scaled-out-cloud-databases"></a>Uitgeschaalde clouddatabases beheren

[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]

**Taken voor elastic Database** is een klant gehost Azure Cloud Service waarmee de uitvoering van de ad-hoc en geplande administratieve taken die worden aangeroepen **taken**. Met taken, u kunt eenvoudig en betrouwbaar grote groepen van Azure SQL-databases beheren door het uitvoeren van Transact-SQL-scripts beheerbewerkingen wilt uitvoeren.

Voor het beheren van uitgeschaalde shard-databases, de **taken voor Elastic Database** functie (preview) kunt u op betrouwbare wijze een Transact-SQL (T-SQL)-script uitvoeren voor een groep databases, met inbegrip van:

- een aangepaste verzameling van databases (Zie hieronder)
- alle databases in een [elastische pool](sql-database-elastic-pool.md)
- een shard-verzameling (gemaakt met behulp van [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md)).

Zie voor meer informatie over het concept van een shard-database, [Sharding een SQL Server-Database](https://blog.pythian.com/sharding-sql-server-database/).

## <a name="documentation"></a>Documentatie

- [Installeer de onderdelen van de taak Elastic Database](sql-database-elastic-jobs-service-installation.md).
- [Aan de slag met taken voor Elastic Database](sql-database-elastic-jobs-getting-started.md).
- [Maken en beheren van taken met behulp van PowerShell](sql-database-elastic-jobs-powershell.md).
- [Maken en beheren van schaal van Azure SQL-databases](sql-database-elastic-jobs-getting-started.md)

![Elastische taak service][1]

## <a name="why-use-jobs"></a>Waarom-taken gebruiken

### <a name="manage"></a>Beheren

Eenvoudig doen schemawijzigingen, beheren van referenties, bijwerken van verwijzingsgegevens, verzamelen van prestatiegegevens of verzamelen van telemetriegegevens van tenants (klanten).

### <a name="reports"></a>Rapporten

Verzamel gegevens uit een verzameling Azure SQL-databases in één doeltabel.

### <a name="reduce-overhead"></a>Overhead verminderen

Normaal gesproken moet u elke database apart verbinden om Transact-SQL-instructies of andere beheertaken te kunnen uitvoeren. Een taak handelt het aanmelden bij elke database in de doelgroep af. U definieert, onderhoudt en handhaaft Transact-SQL-scripts die in een groep van Azure SQL-databases moeten worden uitgevoerd.

### <a name="accounting"></a>Boekhouding

Taken uitvoeren van het script en de status van de uitvoering voor elke database wordt vastgelegd. U krijgt automatische nieuwe pogingen in geval van mislukte pogingen.

### <a name="flexibility"></a>Flexibiliteit

Definieer aangepaste groepen van Azure SQL-databases en definieer schema's voor het uitvoeren van een taak.

> [!NOTE]
> Alleen een beperkte set functies voor elastische pools voor SQL Azure beperkt is in de Azure-portal beschikbaar. Gebruik de PowerShell-APIs voor toegang tot de volledige set van huidige functionaliteit.

## <a name="applications"></a>Toepassingen

- Uitvoeren van beheertaken, zoals het implementeren van een nieuw schema.
- Naslaginformatie over data-productinformatie algemene bijwerken voor alle databases. Of planningen automatische updates elke weekdag, uur.
- Bouw indexen opnieuw om queryprestaties te verbeteren. Het opnieuw opbouwen van kan worden geconfigureerd om uit te voeren in een verzameling van databases op periodieke basis, zoals tijdens de daluren.
- Verzamel op continue basis queryresultaten uit een reeks databases in een centrale tabel. Prestatiequery's kunnen voortdurend worden uitgevoerd en geconfigureerd om de uitvoering van aanvullende taken te activeren.
- Voer langer durende gegevensverwerkingsquery's uit voor een groot aantal databases, zoals het verzamelen van klanttelemetrie. Resultaten worden in één doeltabel verzameld voor verdere analyse.

## <a name="elastic-database-jobs-end-to-end"></a>Taken voor elastic Database: end-to-end

1. Installeer de **taken voor Elastic Database** onderdelen. Zie voor meer informatie, [taken voor Elastic Database installeren](sql-database-elastic-jobs-service-installation.md). Als de installatie mislukt, raadpleegt u [verwijderen](sql-database-elastic-jobs-uninstall.md).
2. Gebruik de PowerShell-APIs voor toegang tot meer functionaliteit, zoals het maken van aangepaste database verzamelingen, schema's toevoegen en/of resultatensets verzamelen. De portal te gebruiken voor eenvoudige installatie en het maken/beheren van taken die zijn beperkt tot uitvoering op basis van een **elastische pool**.
3. Versleutelde referenties voor het uitvoeren van taak maken en [de gebruiker (of de rol) toevoegen aan elke database in de groep](sql-database-security-overview.md).
4. Maak een idempotent T-SQL-script dat kan worden uitgevoerd op elke database in de groep.
5. Volg deze stappen voor het maken van taken met behulp van de Azure-portal: [maken en beheren van taken voor Elastic Database](sql-database-elastic-jobs-create-and-manage.md).
6. Of gebruik PowerShell-scripts: [maken en beheren van een SQL Database elastische databasetaken met behulp van PowerShell (preview)](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Idempotente scripts

De scripts moeten [idempotent](https://en.wikipedia.org/wiki/Idempotence). Simpel gezegd betekent 'idempotent' als het script kan worden uitgevoerd, en deze opnieuw wordt uitgevoerd, wordt hetzelfde resultaat plaatsvindt. Een script kan mislukken door tijdelijke netwerkproblemen. In dat geval wordt automatisch een vooraf ingesteld aantal keer opnieuw geprobeerd om de taak uit te voeren alvorens het wordt afgebroken. Een script idempotent heeft hetzelfde resultaat, zelfs als is twee keer met succes is uitgevoerd.

Een eenvoudige tactiek is te testen of een object bestaat voordat u het maakt.  

```sql
    IF NOT EXIST (some_object)
    -- Create the object
```

Op dezelfde manier moet een script met succes kunnen worden uitgevoerd door logisch te testen op problemen en hierop te reageren.

## <a name="failures-and-logs"></a>Fouten en Logboeken

Als een script dat na meerdere pogingen is mislukt, wordt de taak de fout wordt vastgelegd en blijft. Nadat een taak is beëindigd kunt (wat betekent dat een uitvoering op basis van alle databases in de groep), u controleren de lijst met mislukte pogingen. De logboeken vindt u informatie fouten opsporen in scripts defect.

## <a name="group-types-and-creation"></a>Groep typen en maken

Er zijn twee soorten groepen:

1. Shard-sets
2. Aangepaste groepen

Shard set groepen zijn gemaakt met behulp van de [hulpmiddelen voor Elastic Database](sql-database-elastic-scale-introduction.md). Wanneer u een shard set-groep maakt, worden de databases toegevoegd of verwijderd uit de groep automatisch. Bijvoorbeeld, een nieuwe shard worden automatisch in de groep wanneer u deze aan de shard-toewijzing toevoegen. Een taak kan vervolgens worden uitgevoerd op basis van de groep.

Aangepaste groepen, worden aan de andere kant streng gedefinieerd. U moet expliciet toevoegen of verwijderen van databases uit aangepaste groepen. Als een database in de groep wordt verwijderd, wordt de taak proberen het script uitvoeren op de database een uiteindelijke fout leidt. Groepen die zijn gemaakt met behulp van de Azure-portal op dit moment zijn aangepaste groepen.

## <a name="components-and-pricing"></a>Onderdelen en prijzen

De volgende onderdelen werken samen om u te maken van een Azure-Cloud-service waarmee ad-hoc uitvoeren van administratieve taken. De onderdelen zijn geïnstalleerd en wordt automatisch geconfigureerd tijdens de installatie in uw abonnement. Als ze allemaal dezelfde automatisch gegenereerde naam hebben, kunt u de services identificeren. De naam uniek is en bestaat uit het voorvoegsel "edj" gevolgd door 21 willekeurige tekens.

- Azure-Cloudservice

  Taken voor elastic database (preview) wordt geleverd als een Azure-Cloud klant gehoste service uit te voeren van de aangevraagde taken. De service is geïmplementeerd en die worden gehost in uw Microsoft Azure-abonnement vanuit de portal. De standaardwaarde geïmplementeerd met het minimum van twee werkrollen voor hoge beschikbaarheid van de service wordt uitgevoerd. De standaardgrootte van elke worker-rol (ElasticDatabaseJobWorker) wordt uitgevoerd op een A0-exemplaar. Zie voor informatie over prijzen [prijsinformatie voor cloudservices](https://azure.microsoft.com/pricing/details/cloud-services/).

- Azure SQL Database

  De service maakt gebruik van een Azure SQL Database ook wel de **control database** voor het opslaan van alle van de metagegevens van de taak. De standaardlaag van de service is een S0. Zie voor informatie over prijzen [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

- Azure Service Bus

  Een Azure Service Bus is voor de coördinatie van het werk binnen de Azure-Cloudservice. Zie [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

- Azure Storage

  Een Azure Storage-account wordt gebruikt voor het opslaan van logboekregistratie van diagnostische uitvoer in het geval dat een probleem vereist aanvullende foutopsporing (Zie [diagnostische gegevens inschakelen in Azure Cloud Services en Virtual Machines](../cloud-services/cloud-services-dotnet-diagnostics.md)). Zie voor informatie over prijzen [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>De werking van taken voor Elastic Database

1. Een Azure SQL Database is aangewezen een **control database** waarin alle gegevens in de metagegevens en status wordt opgeslagen.
2. De database wordt gebruikt door de **taak service** om te starten en bijhouden van taken om uit te voeren.
3. Twee verschillende rollen communiceren met de database:
   - Domeincontroller: Hiermee bepaalt u welke taken uit te voeren van de aangevraagde taak en mislukte taken voor nieuwe pogingen door het maken van nieuwe taken taken vereisen.
   - Taak uitvoeren van taak: Hiermee voert de taken van jobs.

### <a name="job-task-types"></a>Taak taaktypen

Er zijn meerdere typen taken die de uitvoering van taken uitvoeren:

- ShardMapRefresh

  Query's de shard-toewijzing om te bepalen van alle databases die worden gebruikt als shards
- ScriptSplit

  Hiermee wordt het script in 'Ga'-instructie in batches
- ExpandJob

  Hiermee maakt u onderliggende taken voor elke database uit een taak die gericht is op een groep databases
- ScriptExecution

  Een script op basis van een bepaalde database met behulp van gedefinieerde referenties wordt uitgevoerd
- Dacpac

  Een DACPAC is van toepassing op een bepaalde database met behulp van bepaalde referenties

## <a name="end-to-end-job-execution-work-flow"></a>End-to-end-taak uitvoeren-werkstroom

1. Met behulp van de Portal of de PowerShell-API, een taak wordt ingevoegd in de **control database**. De taak vraagt om de uitvoering van een Transact-SQL-script op basis van een groep databases met specifieke referenties.
2. De controller identificeert de nieuwe taak. Taken worden gemaakt en uitgevoerd om te splitsen van het script en vernieuwen van de groep databases. Ten slotte wordt een nieuwe taak gemaakt en uitgevoerd om de taak uitvouwen en nieuwe onderliggende taken waarbij elke onderliggende taak is opgegeven voor het uitvoeren van de Transact-SQL-script op basis van een individuele database in de groep te maken.
3. De controller identificeert de onderliggende taken. Voor elke taak, de controller maakt en activeert een taak voor het uitvoeren van het script voor een database.
4. Nadat alle taken hebt voltooid, wordt in de controller de taken bijgewerkt naar een onvoltooide status.
   Op elk gewenst moment tijdens het uitvoeren van taak, kan de PowerShell-API worden gebruikt om de huidige status van het uitvoeren van taak weer te geven. Alle tijden die zijn geretourneerd door de PowerShell-APIs worden weergegeven in UTC. Indien gewenst, kan een aanvraag voor annulering kan worden gestart als u wilt een taak te stoppen.

## <a name="next-steps"></a>Volgende stappen

[Installeer de onderdelen van](sql-database-elastic-jobs-service-installation.md), klikt u vervolgens [maken en een logboek in toevoegen aan elke database in de groep met databases](sql-database-manage-logins.md). Zie voor meer informatie over taak maken en beheren, [maken en beheren van taken voor elastic database](sql-database-elastic-jobs-create-and-manage.md). Zie ook [aan de slag met taken voor Elastic Database](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->
