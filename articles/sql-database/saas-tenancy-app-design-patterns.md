---
title: Multitenant SaaS-patronen - Azure SQL Database | Microsoft Docs
description: Meer informatie over de vereisten en algemene gegevens architectuur patronen van multitenant software als een service (SaaS)-databasetoepassingen die in de Azure-cloud-omgeving worden uitgevoerd.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, sstein
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: eff6859dda771bfc2ca2e709578983b6113c6057
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227483"
---
# <a name="multi-tenant-saas-database-tenancy-patterns"></a>Multitenant SaaS-patronen voor databases-tenants

Bij het ontwerpen van een multitenant SaaS-toepassing, moet u zorgvuldig het model voor tenants die het beste past bij de behoeften van uw toepassing.  Een model tenants bepaalt hoe de gegevens van elke tenant wordt toegewezen aan opslag.  Uw eigen keuze aan tenants-model heeft gevolgen voor toepassing ontwerpen en beheren.  Soms is het kostbare om later overschakelen naar een ander model.

Dit artikel wordt beschreven modellen van andere tenants.

## <a name="a-saas-concepts-and-terminology"></a>A. SaaS-concepten en terminologie

In de Software als een Service (SaaS)-model van uw bedrijf niet wordt verkocht *licenties* met de software. In plaats daarvan elke klant wordt verhuren betalingen voor uw bedrijf, zodat elke klant een *tenant* van uw bedrijf.

Elke tenant krijgt toegang tot de onderdelen van uw SaaS-toepassing tegen betalen huur, en heeft de gegevens die zijn opgeslagen in de SaaS-systeem.

De term *tenants model* verwijst naar hoe tenants opgeslagen gegevens zijn onderverdeeld:

- *Single-tenants:* &nbsp; gegevens van slechts één tenant in elke database worden opgeslagen.
- *Multitenancy:* &nbsp; elke database slaat de gegevens van meerdere afzonderlijke tenants (met mechanismen voor het beveiligen van de privacy van gegevens).
- Hybride tenants modellen zijn ook beschikbaar.

## <a name="b-how-to-choose-the-appropriate-tenancy-model"></a>B. De juiste tenants-model kiezen

In het algemeen de tenants-model heeft geen invloed op de functie van een toepassing, maar dit waarschijnlijk invloed heeft op andere aspecten van de algehele oplossing.  De volgende criteria worden gebruikt om elk van de modellen vast te stellen:

- **Schaalbaarheid:**
    - Het aantal tenants.
    - Opslag per-tenant.
    - Opslag in geaggregeerde vorm.
    - Workload.

- **Tenantisolatie:** &nbsp; gegevens te isoleren en prestaties (of workload van één tenant heeft gevolgen voor anderen).

- **Kosten per tenant:** &nbsp; kosten van de Database.

- **Ontwikkeling van complexiteit:**
    - Wijzigingen in schema.
    - Wijzigingen in query's (vereist door het patroon).

- **Operationele complexiteit:**
    - Prestaties bewaken en beheren.
    - Schemabeheer.
    - Herstellen van een tenant.
    - Herstel na noodgevallen.

- **Aanpasbaarheid:** &nbsp; gebruiksgemak schema aanpassingen die zijn ondersteunende tenantspecifieke of klasse-specifieke tenant.

De discussie tenants is gericht op de *gegevens* laag.  Maar kijk eens naar de *toepassing* laag.  Niveau van de toepassing wordt beschouwd als een monolithische entiteit.  Als u de toepassing in veel kleine onderdelen deelt, wijzigen uw eigen keuze aan tenants model.  U kunt sommige onderdelen anders dan andere behandelen met betrekking tot zowel tenants en de technologie voor gegevensopslag of platform dat wordt gebruikt.

## <a name="c-standalone-single-tenant-app-with-single-tenant-database"></a>C. Zelfstandige app met één tenant met één tenant-database

#### <a name="application-level-isolation"></a>Het niveau isoleren van webtoepassingen

In dit model, wordt de hele toepassing herhaaldelijk, één keer geïnstalleerd voor elke tenant.  Elk exemplaar van de app is een zelfstandig exemplaar, zodat u nooit de interactie met een andere zelfstandige instantie.  Elk exemplaar van de app heeft slechts één tenant, en moet daarom slechts één database.  De tenant heeft de database naar zichzelf.

![Ontwerp van zelfstandige app met één database voor één tenant.][image-standalone-app-st-db-111a]

Elke app-exemplaar is geïnstalleerd in een afzonderlijke Azure-resourcegroep.  De resourcegroep kan deel uitmaken van een abonnement dat eigendom is van de softwareleverancier van de of de tenant.  In beide gevallen kunt de leverancier van de software voor de tenant beheren.  Elk exemplaar is geconfigureerd voor verbinding met de bijbehorende database.

Elke tenant-database wordt geïmplementeerd als een individuele database.  Dit model biedt de grootste isolatie van de database.  Maar de isolatie vereist dat er voldoende bronnen worden toegewezen aan elke database voor het afhandelen van de piekbelastingen.  Hier is het belangrijk dat elastische pools kunnen niet worden gebruikt voor databases die zijn geïmplementeerd in verschillende resourcegroepen bevinden of in verschillende abonnementen.  Deze beperking kunt u deze zelfstandige app met één tenant de duurste-oplossing van een algemene database kosten perspectief model.

#### <a name="vendor-management"></a>Leveranciersbeheer van de

De leverancier van de toegang tot alle databases in alle zelfstandige app-exemplaren, zelfs als de app-exemplaren in de andere tenant abonnementen zijn geïnstalleerd.  De toegang wordt verkregen via SQL-verbindingen.  Deze toegang cross-exemplaar kan de leverancier te centraliseren Schemabeheer en cross-database-query voor rapportage-of analytics kunt inschakelen.  Als dit soort gecentraliseerd beheer vereist is, moet een catalogus worden geïmplementeerd die tenant-id's wordt toegewezen aan database URI's.  Azure SQL Database biedt een sharding-bibliotheek die samen met een SQL-database wordt gebruikt voor een catalogus.  De sharding-bibliotheek formeel heet de [elastische Database-clientbibliotheek][docu-elastic-db-client-library-536r].

## <a name="d-multi-tenant-app-with-database-per-tenant"></a>D. App met meerdere tenants met database-per-tenant

Deze volgende patroon maakt gebruik van een toepassing met meerdere tenants met veel databases worden alle databases van één tenant.  Een nieuwe database wordt ingericht voor elke nieuwe tenant.  De application tier wordt geschaald *van* verticaal door meer resources per knooppunt toe te voegen.  Of de app wordt geschaald *uit* horizontaal door meer knooppunten toe te voegen.  De schaal is gebaseerd op werkbelasting en is onafhankelijk van het getal of de schaal van de afzonderlijke databases.

![Ontwerp van multitenant-app met database-per-tenant.][image-mt-app-db-per-tenant-132d]

#### <a name="customize-for-a-tenant"></a>Aanpassen voor een tenant

Als het patroon van zelfstandige app biedt het gebruik van één tenant-databases sterke tenantisolatie.  In elke app waarvan model slechts één tenant-databases bevat, kan het schema voor elke een bepaalde database worden aangepast en geoptimaliseerd voor de tenant.  Deze aanpassing heeft geen invloed op andere tenants in de app. Een tenant moet wellicht mogelijk gegevens buiten de basisgegevensvelden die alle tenants moeten.  Het veld extra gegevens mogelijk verder, een index.

Met de database-per-tenant is het eenvoudig om te realiseren aanpassen van het schema voor een of meer afzonderlijke tenants.  Leverancier van de toepassing moet de procedures voor het beheren van schema-aanpassingen op schaal zorgvuldig ontwerpen.

#### <a name="elastic-pools"></a>Pools voor Elastic Database

Wanneer databases zijn geïmplementeerd in dezelfde resourcegroep bevinden, kunnen ze worden gegroepeerd in pools voor elastische databases.  De pools bieden een kosteneffectieve manier van delen van resources voor verschillende databases.  Deze optie pool is goedkoper dan het vereisen van elke database te groot genoeg zijn om de gebruikspieken plant die er mogelijk te maken.  Hoewel de databases in pools delen de toegang tot resources kunnen ze nog steeds een hoge mate van isolatie van de prestaties te bereiken.

![Ontwerp van multitenant-app met database-per-tenant, met behulp van de elastische pool.][image-mt-app-db-per-tenant-pool-153p]

Azure SQL Database biedt de hulpprogramma's die nodig zijn om te configureren, bewaken en beheren van het delen.  De metrische gegevens voor beide toepassingen op serverniveau en databaseniveau prestaties zijn beschikbaar in Azure portal, en via Log Analytics.  De metrische gegevens krijgt fantastische inzichten in aggregeren en tenant-specifieke prestaties.  Afzonderlijke databases kunnen worden verplaatst tussen groepen voor gereserveerde bronnen voor een specifieke tenant.  Deze hulpprogramma's kunnen u om ervoor te zorgen goede prestaties op een voordelige manier.

#### <a name="operations-scale-for-database-per-tenant"></a>Schaal van de bewerkingen voor de database-per-tenant

De Azure SQL Database-platform heeft veel functies die zijn ontworpen voor het beheren van grote aantallen databases op schaal, zoals databases en meer dan 100.000.  Deze functies kunt u de database-per-tenant-patroon plausibele.

Stel bijvoorbeeld dat een systeem heeft een 1000 tenantdatabase als slechts één database.  De database mogelijk 20 indexen.  Als het systeem wordt geconverteerd naar 1000 één tenant-databases met, stijgt het aantal indexen 20.000.  In SQL-Database als onderdeel van [automatisch afstemmen][docu-sql-db-automatic-tuning-771a], de automatische indexering functies zijn standaard ingeschakeld.  Automatische indexering beheert voor u alle 20.000 indexen en hun actieve maken en -neerzetten-optimalisatie.  Deze geautomatiseerde acties worden uitgevoerd binnen een individuele database, en ze niet worden gecoördineerd of beperkt door soortgelijke handelingen uit in andere databases.  Automatische indexering behandelt indexen die zich anders in een bezette database dan in een minder bezette database bevinden.  Dit type index management aanpassing is niet praktisch op de schaal van de database-per-tenant als deze enorme beheertaak hebt moeten handmatig worden uitgevoerd.

Andere functies die schaalbaar zijn onder andere:

- Ingebouwde back-ups.
- Hoge beschikbaarheid.
- Versleuteling op de schijf.
- Prestatietelemetrie.

#### <a name="automation"></a>Automation

De beheerbewerkingen die kunnen worden in een script vastgelegd en die worden aangeboden via een [devops] [ http-visual-studio-devops-485m] model.  De bewerkingen kunnen zelfs worden geautomatiseerd en weergegeven in de toepassing.

U kan bijvoorbeeld het herstel van één tenant naar een eerder tijdstip automatiseren in-time.  Het herstel is alleen nodig heeft om terug te zetten van de ene één tenant-database waarin de tenant.  Deze terugzetten heeft geen invloed op andere tenants, waarin wordt bevestigd dat beheerbewerkingen op het fijn gedetailleerd niveau van elke afzonderlijke tenant zijn.

## <a name="e-multi-tenant-app-with-multi-tenant-databases"></a>E. Multitenant-app met meerdere tenants databases

Patroon voor een ander beschikbaar is voor het opslaan van veel tenants in een multitenant-database.  Exemplaar van de toepassing kan een willekeurig aantal databases voor meerdere tenants hebben.  Het schema van een multitenant-database moet een of meer tenant-id-kolommen hebben, zodat de gegevens van een bepaalde tenant selectief kunnen worden opgehaald.  Bovendien wordt het schema mogelijk een paar tabellen of kolommen die worden gebruikt door alleen een subset van tenants.  Statische code en referentie-gegevens wordt echter slechts één keer worden opgeslagen en wordt gedeeld door alle tenants.

#### <a name="tenant-isolation-is-sacrificed"></a>Isolatie van tenants is gedood

*Gegevens:* &nbsp; per se offert isolatie van tenants een multitenant-database.  De gegevens van meerdere tenants worden samen in één database opgeslagen.  Tijdens de ontwikkeling, moet u ervoor zorgen dat query's nooit gegevens van meer dan één tenant maken.  SQL Database ondersteunt [beveiliging op rijniveau][docu-sql-svr-db-row-level-security-947w], die kan worden afgedwongen dat gegevens die worden geretourneerd door een query worden afgestemd op één tenant.

*Verwerken:* &nbsp; een multitenant-database deelt reken- en opslagresources voor alle van de tenants.  De database als geheel kan worden gecontroleerd om te controleren of dat deze acceptabel presteert.  De Azure-systeem heeft echter geen ingebouwde manier om te controleren of het gebruik van deze resources door een afzonderlijke tenant beheren.  Daarom wordt de multitenant-database een verhoogd risico op luidruchtige buren, waarbij de workload van één overactive tenant heeft gevolgen voor de prestaties van andere tenants in dezelfde database worden aangetroffen.  Aanvullende controle op toepassingsniveau, kan op tenantniveau prestaties bewaken.

#### <a name="lower-cost"></a>Lagere kosten

Databases van meerdere tenants hebben in het algemeen is het laagste per tenant kosten.  Resourcekosten voor een individuele database zijn lager dan voor een oftewel grootte elastische pool.  Bovendien voor scenario's waarbij tenants alleen beperkt opslag moeten kunnen mogelijk miljoenen tenants worden opgeslagen in een individuele database.  Geen elastische pool kan miljoenen databases bevatten.  Een oplossing met 1000 databases per pool, met 1000 pools, kan echter de schaal van miljoenen indien steeds moeilijker voor het beheren van bereiken.

Twee variaties van een multitenant-database-model worden besproken in het volgende, met de shard multitenant-model wordt het meest flexibele en schaalbare.

## <a name="f-multi-tenant-app-with-a-single-multi-tenant-database"></a>F. App met meerdere tenants met een database met meerdere tenants

De eenvoudigste patroon voor multitenant-database maakt gebruik van een individuele database met hostgegevens voor alle tenants.  Als er meer tenants worden toegevoegd, wordt de database omhoog geschaald met meer opslagruimte en rekencapaciteit bronnen.  Deze omhoog schalen mogelijk alle die nodig is, hoewel er altijd een schaallimiet voor de ultieme.  Lange voordat deze limiet is bereikt de database wordt echter lastig zijn om te beheren.

Beheerbewerkingen die op de afzonderlijke tenants gericht zijn, hoe complexer om te implementeren in een multitenant-database.  En op schaal deze bewerkingen worden onaanvaardbaar langzaam.  Een voorbeeld hiervan is een point-in-time-terugzetten van de gegevens voor één tenant.

## <a name="g-multi-tenant-app-with-sharded-multi-tenant-databases"></a>G. App met meerdere tenants met shard multitenant-databases

De meeste SaaS-toepassingen toegang tot de gegevens van slechts één tenant per keer.  Dit toegangspatroon kunt gegevens van de tenant wordt verdeeld over meerdere databases of shards, waarbij alle gegevens voor een tenant is opgenomen in één shard.  In combinatie met een patroon voor multitenant-database, kunt een shard-model vrijwel onbeperkte schaal.

![Ontwerp van multitenant-app met shard multitenant-databases.][image-mt-app-sharded-mt-db-174s]

#### <a name="manage-shards"></a>Shards beheren

Sharding complexer zowel het ontwerp en operationeel beheer.  Een catalogus is vereist voor het onderhouden van de toewijzing tussen tenants en databases.  Bovendien zijn management procedures vereist voor het beheren van de shards en de bevolking van de tenant.  Bijvoorbeeld, moeten procedures worden ontworpen toevoegen en verwijderen van shards en om tenantgegevens te verplaatsen tussen shards.  Een manier om te schalen is door een nieuwe shard toevoegen en deze vullen met nieuwe tenants.  Op andere momenten kan u een dichtbevolkte shard splitsen in twee minder-dichtbevolkte shards.  Nadat u verschillende tenants hebt verplaatst of buiten gebruik gesteld, kunt u spaarzaam ingevuld shards samenvoegen.  De bewerking resulteert in meer betaalbare Resourcegebruik.  Tenants kunnen ook worden verplaatst tussen shards te verdelen workloads.

SQL Database biedt een hulpprogramma voor splitsen en samenvoegen/waarmee in combinatie met de sharding-bibliotheek en de catalogusdatabase werkt.  De opgegeven app kunt splitsen en samenvoegen shards, en het tenantgegevens kunt verplaatsen tussen shards.  De app onderhoudt ook de catalogus tijdens deze bewerkingen, markering beïnvloed tenants als offline is voordat ze worden verplaatst.  Na het verplaatsen wordt werkt de app de catalogus opnieuw met de nieuwe toewijzing en het markeren van de tenant als weer online.

#### <a name="smaller-databases-more-easily-managed"></a>Kleinere databases meer eenvoudig worden beheerd

Door het distribueren van tenants voor meerdere databases, de shard multitenantoplossing resulteert in kleinere databases die eenvoudiger worden beheerd.  Bijvoorbeeld, omvat een specifieke tenant naar een eerdere punt in tijd nu herstellen het herstellen van een enkele kleinere database vanuit een back-up, in plaats van een grotere database waarin alle tenants. De grootte van de database en het aantal tenants per database, kunnen worden gekozen om te verdelen van de werkbelasting en de risicobeheerinspanningen.

#### <a name="tenant-identifier-in-the-schema"></a>Tenant-id in het schema

Afhankelijk van de sharding-benadering die wordt gebruikt, kunnen extra beperkingen worden opgelegd voor het databaseschema.  De toepassing van de splitsen en samenvoegen/SQL-Database is vereist dat het schema bevat de sharding-sleutel, die normaal gesproken de tenant-id is.  De tenant-id is het belangrijkste element in de primaire sleutel van alle tabellen van de shard.  De tenant-id kan de toepassing/splitsen en samenvoegen om snel te vinden en verplaatsen van gegevens die zijn gekoppeld aan een specifieke tenant.

#### <a name="elastic-pool-for-shards"></a>Elastische pool voor shards

Shard multitenant-databases kunnen worden geplaatst in elastische pools.  Met veel één tenant-databases in een groep is over het algemeen als rendabel als bestaande uit tal van tenants in een paar databases voor meerdere tenants.  Databases van meerdere tenants zijn nuttig wanneer er een groot aantal relatief inactieve tenants.

## <a name="h-hybrid-sharded-multi-tenant-database-model"></a>H. Hybride shard multitenant-database-model

Alle databases hebben in het hybride-model, de tenant-id in hun schema.  De databases zijn geschikt voor alle opslag van meer dan één tenant kunnen, en de databases shard.  Zodat ze alle databases van meerdere tenants in de zin schema zijn.  Nog in de praktijk bevatten sommige van deze databases slechts één tenant.  Ongeacht heeft het aantal tenants die zijn opgeslagen in een bepaalde database geen invloed op het databaseschema.

#### <a name="move-tenants-around"></a>Tenants verplaatsen

U kunt op elk gewenst moment een bepaalde tenant verplaatsen naar een eigen multitenant-database.  En op elk gewenst moment kunt u van gedachten veranderen en de tenant terug verplaatsen naar een database met meerdere tenants.  U kunt ook een tenant toewijzen aan nieuwe één tenant-database wanneer u de nieuwe database inricht.

Het model hybride schijnt wanneer er grote verschillen tussen de resourcebehoeften van identificeerbare groepen van tenants.  Stel bijvoorbeeld dat tenants die deel uitmaken van een gratis proefversie de dezelfde hoge mate van prestaties die geabonneerde tenants zijn niet gegarandeerd.  Het beleid mogelijk voor tenants in de gratis proefperiode worden opgeslagen in een multitenant-database die wordt gedeeld door de gratis proefversie tenants.  Als een gratis proefversie van uw tenant zich op de basis-servicelaag abonneert, kan de tenant worden verplaatst naar een andere multitenant-database die mogelijk minder tenants.  Een abonnee voor de premiumlaag-service betaalt kan worden verplaatst naar een eigen nieuwe één tenant-database.

#### <a name="pools"></a>Pools

In dit model hybride, kunnen de één tenant-databases voor abonnee tenants worden geplaatst in resourcegroepen om databasekosten per tenant te verlagen.  Dit gebeurt ook in het model van de database-per-tenant.

## <a name="i-tenancy-models-compared"></a>I. Tenants modellen vergeleken

De volgende tabel geeft een overzicht van de verschillen tussen de belangrijkste tenants-modellen.

| Meting | Zelfstandige app | Database-per-tenant | Een shard met meerdere tenants |
| :---------- | :------------- | :------------------ | :------------------- |
| Schalen | Middelgroot<br />1-100s | Zeer hoog<br />1-100.000 | Onbeperkt<br />1-1.000.000 |
| Isolatie van tenants | Zeer hoog | Hoog | Laag; met uitzondering van een enkele tenant (dat wil zeggen alleen in een db MT). |
| Databasekosten per tenant | Hoge; wordt de grootte van pieken. | Laag; toepassingen die worden gebruikt. | Laagste, voor kleine tenants in MT-databases. |
| Prestaties controleren en beheren | Per-tenant alleen | Statistische functie + per tenant | Aggregaat. Hoewel per-tenant alleen voor fungeert. |
| Ontwikkeling van complexiteit | Laag | Laag | Gemiddeld; vanwege sharding. |
| Operationele complexiteit | Laag-hoog. Afzonderlijk eenvoudige, complexe op grote schaal. | Low-Medium. Patronen complexiteit op schaal. | Laag-hoog. Beheer van afzonderlijke tenants is ingewikkeld. |
| &nbsp; ||||

## <a name="next-steps"></a>Volgende stappen

- [Een multitenant Wingtip-toepassing die gebruikmaakt van de database-per-tenant SaaS-model - Azure SQL Database implementeren en verkennen][docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]

- [Welkom bij de Wingtip Tickets voorbeeld-app voor tenants van SaaS Azure SQL Database][docu-saas-tenancy-welcome-wingtip-tickets-app-384w]


<!--  Article link references.  -->

[http-visual-studio-devops-485m]: https://www.visualstudio.com/devops/

[docu-sql-svr-db-row-level-security-947w]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security

[docu-elastic-db-client-library-536r]: sql-database-elastic-database-client-library.md
[docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]: sql-database-saas-tutorial.md
[docu-sql-db-automatic-tuning-771a]: sql-database-automatic-tuning.md
[docu-saas-tenancy-welcome-wingtip-tickets-app-384w]: saas-tenancy-welcome-wingtip-tickets-app.md


<!--  Image references.  -->

[image-standalone-app-st-db-111a]: media/saas-tenancy-app-design-patterns/saas-standalone-app-single-tenant-database-11.png "Ontwerp van zelfstandige app met één database voor één tenant."

[image-mt-app-db-per-tenant-132d]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-13.png "Ontwerp van multitenant-app met database-per-tenant."

[image-mt-app-db-per-tenant-pool-153p]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-pool-15.png "Ontwerp van multitenant-app met database-per-tenant, met behulp van de elastische pool."

[image-mt-app-sharded-mt-db-174s]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-sharded-multi-tenant-databases-17.png "Ontwerp van multitenant-app met shard multitenant-databases."

