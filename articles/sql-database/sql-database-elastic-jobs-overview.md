---
title: Cloud uitgebreide databases beheren | Microsoft Docs
description: Ziet u de service van de taak elastische database
metakeywords: azure sql database elastic databases
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 6fa47cf2-1162-4534-a206-6e2d95b78580
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 8e84562115a866c0df5e0dee6c7f66c036a74737
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-scaled-out-cloud-databases"></a>Cloud uitgebreide databases beheren
Voor het beheren van uitgebreide shard-databases, de **elastische Database taken** functie (preview) kunt u op betrouwbare wijze een (T-SQL) Transact-SQL-script uitvoeren in een groep van databases, met inbegrip van:

* een aangepaste verzameling van databases (Zie hieronder)
* alle databases in een [elastische groep](sql-database-elastic-pool.md)
* een set shard (gemaakt met behulp van [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md)). 

## <a name="documentation"></a>Documentatie
* [Installeer de onderdelen van de taak elastische Database](sql-database-elastic-jobs-service-installation.md). 
* [Aan de slag met elastische Database taken](sql-database-elastic-jobs-getting-started.md).
* [Maken en beheren van taken met behulp van PowerShell](sql-database-elastic-jobs-powershell.md).
* [Maken en beheren van geschaalde uit Azure SQL-Databases](sql-database-elastic-jobs-getting-started.md)

**Elastische Database taken** is momenteel een klant gehost Azure Cloud Service waarmee de uitvoering van de ad-hoc en geplande administratieve taken die worden aangeroepen **taken**. Met taken, kan eenvoudig en betrouwbaar grote groepen van Azure SQL-Databases beheren door het uitvoeren van Transact-SQL-scripts administratieve bewerkingen uit te voeren. 

![Service voor elastische database-taak][1]

## <a name="why-use-jobs"></a>Waarom taken gebruiken?
**Beheren**

Schemawijzigingen eenvoudig doen, Referentiebeheer, verwijzing Gegevensupdates, prestatiegegevens verzamelen of telemetrie tenantverzameling (klant).

**Rapporten**

Cumulatieve gegevens uit een verzameling van Azure SQL-Databases in een doeltabel met één.

**Overhead verminderen**

Normaal gesproken moet u verbinding maken met elke database, onafhankelijk van elkaar om te kunnen uitvoeren van Transact-SQL-instructies of andere beheertaken. Een taak verwerkt de taak van het aanmelden bij elke database in de doelgroep. U ook definiëren, onderhouden en persistent maken van Transact-SQL-scripts moeten worden uitgevoerd in een groep van Azure SQL-Databases.

**Accounting**

Taken het script uitvoert en de status van de uitvoering voor elke database wordt vastgelegd. Automatisch opnieuw wordt ook krijgen als er fouten optreden.

**Flexibiliteit**

Aangepaste groepen van Azure SQL-Databases, en schema's voor het uitvoeren van een taak te definiëren.

> [!NOTE]
> In de Azure portal is slechts een beperkte set functies beperkt tot SQL Azure elastische pools beschikbaar. Gebruik de PowerShell APIs voor toegang tot de volledige set van de huidige functionaliteit.
> 
> 

## <a name="applications"></a>Toepassingen
* Administratieve taken uitvoeren, zoals het implementeren van een nieuw schema.
* Algemene referentie gegevens product-informatie voor alle databases bijwerken. Of schema's automatische updates elke weekdag, na uur.
* Samenstellen ter verbetering van de prestaties van query's. Het opnieuw opbouwen kan worden geconfigureerd om uit te voeren in een verzameling van databases op periodieke basis, zoals tijdens de daluren.
* Queryresultaten uit een set van databases in een centrale tabel op basis van de lopende verzamelen. Prestaties van query's worden voortdurend uitgevoerd en geconfigureerd voor de trigger aanvullende taken worden uitgevoerd.
* Langer actief gegevensverwerking-query's uitvoeren in een groot aantal databases, zoals het verzamelen van telemetrie van de klant. Resultaten worden verzameld in een doeltabel met één voor verdere analyse.

## <a name="elastic-database-jobs-end-to-end"></a>Elastische taken van de Database: end-to-end
1. Installeer de **elastische Database taken** onderdelen. Zie voor meer informatie [elastische Database installeren taken](sql-database-elastic-jobs-service-installation.md). Als de installatie mislukt, raadpleegt u [verwijderen](sql-database-elastic-jobs-uninstall.md).
2. Gebruik de PowerShell APIs voor toegang tot meer functionaliteit, bijvoorbeeld het maken van aangepaste database verzamelingen, schema's toe te voegen en/of resultatensets verzamelen. De portal gebruiken voor eenvoudige installatie en het maken/bewaking van taken die zijn beperkt tot uitvoering op basis van een **elastische pool**. 
3. Gecodeerde referenties voor het uitvoeren van taak maken en [de gebruiker (of de rol) toevoegen aan elke database in de groep](sql-database-security-overview.md).
4. Maak een idempotent T-SQL-script dat kan worden uitgevoerd op elke database in de groep. 
5. Volg deze stappen voor taken met de Azure portal maken: [maken en beheren van taken voor elastische Database](sql-database-elastic-jobs-create-and-manage.md). 
6. Of PowerShell-scripts gebruiken: [maken en beheren van een SQL-Database elastische database taken met behulp van PowerShell (preview)](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>De Idempotent-scripts
De scripts moet [idempotent](https://en.wikipedia.org/wiki/Idempotence). Eenvoudige voorwaarden 'idempotent' betekent dat als het script is voltooid en opnieuw wordt uitgevoerd, wordt hetzelfde resultaat optreedt. Een script kan mislukken door tijdelijke netwerkproblemen. De taak wordt in dat geval wordt automatisch opnieuw uitvoeren van het script een vooraf ingestelde aantal keren voordat desisting. Een idempotent-script geeft hetzelfde resultaat, zelfs als is twee keer met succes is uitgevoerd. 

Een eenvoudige e-mailbericht is om te testen op de aanwezigheid van een object voordat u deze maakt.  

    IF NOT EXIST (some_object)
    -- Create the object 
    -- If it exists, drop the object before recreating it.

Op dezelfde manier een script dat moet kunnen probleemloos worden uitgevoerd door logisch voor testen en tegen te gaan eventuele voorwaarden gevonden.

## <a name="failures-and-logs"></a>Logboeken en fouten
Als een script na meerdere pogingen mislukt, wordt de taak de fout wordt vastgelegd en voortgezet. Nadat een taak is beëindigd kunt (wat betekent een uitgevoerd tegen alle databases in de groep), u controleren de lijst met mislukte pogingen. De logboeken vindt u informatie voor foutopsporing defecte scripts. 

## <a name="group-types-and-creation"></a>Groep typen en maken
Er zijn twee soorten groepen: 

1. Shard sets
2. Aangepaste groepen

Shard set groepen zijn gemaakt met behulp van de [hulpmiddelen voor elastische databases](sql-database-elastic-scale-introduction.md). Wanneer u een set shard groep maakt, worden de databases toegevoegd of verwijderd uit de groep automatisch. Bijvoorbeeld, is een nieuwe shard automatisch in de groep wanneer u deze aan de shard-toewijzing toevoegen. Een taak kan vervolgens worden uitgevoerd op de groep.

Aangepaste groepen worden aan de andere kant streng gedefinieerd. U moet expliciet toevoegen of verwijderen van databases uit aangepaste groepen. Als een database in de groep wordt verwijderd, wordt het script uitvoeren op de database een problemen waardoor de taak proberen. Groepen die zijn gemaakt met behulp van de Azure-portal op dit moment zijn aangepaste groepen. 

## <a name="components-and-pricing"></a>Onderdelen en prijzen
De volgende onderdelen werken samen om het maken van een Azure-Cloud-service waarmee ad-hoc-uitvoering van administratieve taken. De onderdelen zijn geïnstalleerd en wordt automatisch geconfigureerd tijdens de installatie in uw abonnement. U kunt de services identificeren als ze allemaal dezelfde automatisch gegenereerde naam hebben. De naam uniek is en bestaat uit het voorvoegsel 'edj' gevolgd door 21 willekeurige tekens.

* **Azure-Cloudservice**: taken voor elastische database (preview) wordt geleverd als een Azure-Cloud klant gehoste service voor uitvoering van de aangevraagde taken uitvoeren. De service is geïmplementeerd en gehost in uw Microsoft Azure-abonnement vanuit de portal. De standaardwaarde geïmplementeerd met het minimum van twee werkrollen voor hoge beschikbaarheid van de service wordt uitgevoerd. De standaardgrootte van elke worker-rol (ElasticDatabaseJobWorker) wordt uitgevoerd op een exemplaar van A0. Zie voor prijzen [cloudservices-prijzen](https://azure.microsoft.com/pricing/details/cloud-services/). 
* **Azure SQL Database**: de service gebruikmaakt van een Azure SQL Database bekend als de **beheer database** voor het opslaan van alle metagegevens van de taak. De servicelaag standaard is een S0. Zie voor prijzen [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* **Azure Service Bus**: een Azure Service Bus is voor de coördinatie van de werkzaamheden binnen de Azure-Cloudservice. Zie [Service Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/).
* **Azure Storage**: een Azure Storage-account wordt gebruikt voor het opslaan van logboekregistratie van diagnostische uitvoer in het geval dat een probleem vereist meer foutopsporing (Zie [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md)). Zie voor prijzen [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>De werking van taken voor elastische Database
1. Een Azure SQL Database is aangewezen een **beheer database** waarin alle gegevens in de metagegevens en de status wordt opgeslagen.
2. De database worden geopend door de **service taak** starten en het bijhouden van taken uit te voeren.
3. Twee verschillende rollen communiceren met de database: 
   * Controller: Hiermee wordt bepaald welke taken vereisen taken voor het uitvoeren van de aangevraagde taak en mislukte taken voor nieuwe pogingen door het maken van nieuwe taken.
   * Taak taakuitvoering: Wordt uitgevoerd de taken.

### <a name="job-task-types"></a>Taak taaktypen
Er zijn meerdere typen taken die de uitvoering van taken uitvoeren:

* ShardMapRefresh: Een query de shard-kaart om te bepalen van alle databases die worden gebruikt als shards
* ScriptSplit: Splitst het script via de GO'-instructies in batches
* ExpandJob: Maakt onderliggende taken voor elke database uit een taak die gericht is op een groep met databases
* ScriptExecution: Een script op basis van een bepaalde database met gedefinieerde referenties wordt uitgevoerd
* Dacpac: Een DACPAC geldt voor een bepaalde database met behulp van bepaalde referenties

## <a name="end-to-end-job-execution-work-flow"></a>End-to-end-taak kan worden uitgevoerd-werkstroom
1. Met de Portal of de PowerShell-API's een taak wordt ingevoegd in de **beheer database**. De taak vraagt om de uitvoering van een Transact-SQL-script voor een groep van databases met behulp van specifieke referenties.
2. De controller identificeert de nieuwe taak. Taken worden gemaakt en wordt uitgevoerd voor het splitsen van het script en vernieuwen van de groep databases. Ten slotte wordt een nieuwe taak gemaakt en de taak en nieuwe onderliggende taken waarbij elke onderliggende taak is opgegeven voor het uitvoeren van de Transact-SQL-script op een individuele database in de groep maken om te worden uitgevoerd.
3. De controller identificeert de onderliggende taken. Voor elke taak de controller maakt en een taak voor het uitvoeren van het script op basis van een database wordt geactiveerd. 
4. Nadat alle taken hebt voltooid, werkt de domeincontroller de taken naar een voltooide status. 
   Op elk moment tijdens de taakuitvoering kan de PowerShell-API worden gebruikt om de huidige status van het uitvoeren van taak weer te geven. Alle tijden die zijn geretourneerd door de PowerShell APIs worden weergegeven in UTC. Indien gewenst, kunt een annuleringsverzoek worden gestart om een taak stoppen. 

## <a name="next-steps"></a>Volgende stappen
[De onderdelen installeren](sql-database-elastic-jobs-service-installation.md), klikt u vervolgens [maken en toevoegen van een logbestand in elke database in de groep van databases](sql-database-manage-logins.md). Zie voor meer inzicht in taak maken en beheren, [maken en beheren van taken voor elastische database](sql-database-elastic-jobs-create-and-manage.md). Zie ook [aan de slag met elastische Database taken](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->


