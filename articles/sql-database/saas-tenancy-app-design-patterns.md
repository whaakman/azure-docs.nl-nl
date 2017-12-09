---
title: Multitenant SaaS patronen - Azure SQL Database | Microsoft Docs
description: Meer informatie over de vereisten en algemene gegevens architectuur patronen van meerdere tenants software als een dienst (SaaS)-database-toepassingen die worden uitgevoerd in de Azure-cloud-omgeving.
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: billgib
manager: craigg
editor: MightyPen,srinia
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: billgib
ms.openlocfilehash: 1b6c780000d8c5e31a78f7f83ae74c002e8f8349
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="multi-tenant-saas-database-tenancy-patterns"></a>Multitenant SaaS-database tenancymodus patronen

Bij het ontwerpen van een multitenant SaaS-toepassing, moet u zorgvuldig de tenancymodus model die het beste past bij de behoeften van uw toepassing.  Een model tenancymodus bepaalt hoe gegevens van elke tenant wordt toegewezen aan opslag.  Uw keuze van tenancymodus model heeft gevolgen voor toepassingen ontwerpen en beheren.  Later overschakelen naar een ander model is het soms kostbaar.

Hier volgt een bespreking van de alternatieve tenancymodus modellen.

## <a name="a-how-to-choose-the-appropriate-tenancy-model"></a>A. Het kiezen van het juiste tenancymodus-model

In het algemeen de tenancymodus model heeft geen gevolgen voor de functie van een toepassing, maar deze waarschijnlijk heeft gevolgen voor andere aspecten van de hele oplossing.  De volgende criteria worden gebruikt om elk van de modellen vast te stellen:

- **Schaalbaarheid:**
    - Het aantal tenants.
    - Opslag per-tenant.
    - Opslag in geaggregeerde vorm.
    - Werkbelasting.

- **Tenantisolatie:** &nbsp; gegevensisolatie en prestaties (of de werkbelasting van een tenant heeft gevolgen voor andere).

- **Kosten per tenant:** &nbsp; kosten van de Database.

- **Complexiteit ontwikkeling:**
    - Wijzigingen in het schema.
    - Wijzigingen in query's (vereist door het patroon).

- **Operationele complexiteit:**
    - Bewaken en beheren van prestaties.
    - Schemabeheer van het.
    - Herstellen van een tenant.
    - Herstel na noodgevallen.

- **Aanpassingsmogelijkheden:** &nbsp; eenvoudig te ondersteunen schema aanpassingen die zijn tenantspecifieke of tenant-klasse-specifieke.

De bespreking van de tenancymodus is gericht op de *gegevens* laag.  Maar kijk eens naar de *toepassing* laag.  De toepassingslaag wordt beschouwd als een monolithisch entiteit.  Als u de toepassing in veel kleine onderdelen verdelen, wordt uw keuze van tenancymodus model mogelijk gewijzigd.  U kan een aantal onderdelen anders dan andere behandelen met betrekking tot zowel tenancymodus en het opslagtechnologie of platform gebruikt.

## <a name="b-standalone-single-tenant-app-with-single-tenant-database"></a>B. Zelfstandige één tenant app met één tenant-database

#### <a name="application-level-isolation"></a>Het niveau isoleren van webtoepassingen

In dit model, wordt de gehele toepassing herhaaldelijk, één keer geïnstalleerd voor elke tenant.  Elk exemplaar van de app is een zelfstandig exemplaar, zodat deze nooit met elk ander exemplaar van de zelfstandige samenwerkt.  Elk exemplaar van de app heeft slechts één tenant en daarom moet slechts één database.  De tenant heeft de database naar zichzelf.

![Ontwerp van zelfstandige app met exact één database voor één tenant.][image-standalone-app-st-db-111a]

Elke app-exemplaar is geïnstalleerd in een afzonderlijke Azure-resourcegroep.  De resourcegroep kan deel uitmaken van een abonnement dat eigendom is van de leverancier of de tenant.  In beide gevallen kunt de leverancier van de software voor de tenant beheren.  Elk toepassingsexemplaar is geconfigureerd om verbinding maken met de bijbehorende database.

Elke tenant-database wordt geïmplementeerd als een zelfstandige database.  Dit model biedt de grootste isolatie van de database.  Maar de isolatie vereist dat er voldoende bronnen worden toegewezen aan elke database voor het afhandelen van de piekbelastingen.  Hier is het belang dat elastische pools kunnen niet worden gebruikt voor databases die zijn geïmplementeerd in verschillende resourcegroepen bevinden of in verschillende abonnementen behoren.  Deze beperking kunt u deze zelfstandige één tenant app de meest dure oplossing van een algemene database-kostenoogpunt model.

#### <a name="vendor-management"></a>De leverancier van management

De leverancier van de toegang tot alle databases in alle zelfstandige app-exemplaren, zelfs als de app-exemplaren in andere tenant abonnementen zijn geïnstalleerd.  De toegang is verkregen via SQL-verbindingen.  Deze toegang cross-exemplaar kan de leverancier te centraliseren Schemabeheer en cross-databasequery voor rapportage of analytics doeleinden kunt inschakelen.  Als u dit soort gecentraliseerd beheer, moet een catalogus worden geïmplementeerd die tenant-id wordt toegewezen aan database URI's.  Azure SQL Database biedt een sharding-bibliotheek die wordt gebruikt samen met een SQL-database voor een catalogus.  De bibliotheek sharding heet formeel de [clientbibliotheek voor elastische Database][docu-elastic-db-client-library-536r].

## <a name="c-multi-tenant-app-with-database-per-tenant"></a>C. Multitenant-app met database per tenant

Dit patroon van de volgende maakt gebruik van een toepassing met meerdere tenants met veel databases, worden alle databases voor één tenant.  Een nieuwe database wordt ingericht voor elke nieuwe tenant.  De toepassingslaag wordt geschaald *up* verticaal door meer bronnen per knooppunt toe te voegen.  Of de app wordt geschaald *uit* horizontaal door meer knooppunten toe te voegen.  De schaal is gebaseerd op werkbelasting en niet afhankelijk is van het nummer of de schaal van de afzonderlijke databases.

![Ontwerp van multitenant-app met database per tenant.][image-mt-app-db-per-tenant-132d]

#### <a name="customize-for-a-tenant"></a>Aanpassen voor een tenant

Als het patroon van zelfstandige app biedt het gebruik van één tenant databases sterk tenantisolatie.  In elke app waarvan model alleen databases van één tenant geeft, worden het schema voor een bepaalde één database aangepast en geoptimaliseerd voor de tenant.  Deze aanpassing heeft geen invloed op andere tenants in de app. Een tenant wellicht mogelijk gegevens buiten de basisgegevensvelden die alle tenants moeten.  Het veld extra gegevens mogelijk verder kan een index.

Met de database per tenant is het eenvoudig voor aanpassen van het schema voor een of meer afzonderlijke tenants.  Leverancier van de toepassing moet de procedures voor het beheren van schema aanpassingen op grote schaal zorgvuldig ontwerpen.

#### <a name="elastic-pools"></a>Pools voor Elastic Database

Wanneer databases zijn geïmplementeerd in dezelfde resourcegroep bevinden, kunnen ze worden gegroepeerd in pools voor elastische databases.  De pools bieden een rendabele manier om resources te delen tussen meerdere databases.  Deze optie van toepassingen is goedkoper dan het vereisen van elke database moet groot genoeg voor de gebruikspieken plant die deze optreedt.  Hoewel gegroepeerde databases toegang tot resources delen kunnen ze nog steeds een hoge mate van isolatie van de prestaties bereiken.

![Ontwerp van multitenant-app met de database-per-tenant, met behulp van de elastische groep.][image-mt-app-db-per-tenant-pool-153p]

Azure SQL Database biedt de hulpprogramma's die nodig zijn om te configureren, bewaken en beheren van het delen.  Beide toepassingen niveau en database prestatiewaarden zijn beschikbaar in de Azure-portal en via logboekanalyse.  De metrische gegevens kunt geweldige inzicht in prestaties cumulatieve en tenantspecifieke geven.  Afzonderlijke databases kunnen worden verplaatst tussen mediagroepen gereserveerde bronnen bieden aan een specifieke tenant.  Deze hulpprogramma's kunnen u goede prestaties op een rendabele manier te garanderen.

#### <a name="operations-scale-for-database-per-tenant"></a>Bewerkingen voor de database per tenant schalen

De Azure SQL Database-platform heeft veel beheerfuncties zijn ontworpen voor het beheer van grote aantallen van databases op grote schaal, zoals ook meer dan 100.000 databases.  Deze voorzieningen maken het patroon database per tenant aannemelijke.

Stel bijvoorbeeld dat een systeem heeft een database 1000-tenant als slechts één database.  De database wellicht 20 indexen.  Als de worden geconverteerd naar de databases van 1000 single-tenant, wordt de quatity van indexen komt tot 20.000.  In SQL-Database als onderdeel van [automatische afstemming][docu-sql-db-automatic-tuning-771a], de automatische indexering functies zijn standaard ingeschakeld.  Automatische indexering beheert voor u alle 20.000 indexen en hun lopende maken en -neerzetten-optimalisatie.  Deze geautomatiseerde acties worden uitgevoerd binnen een individuele database, en ze niet zijn gecoördineerd of beperkt door soortgelijke acties in andere databases.  Automatisch indexeren wordt behandeld indexen anders in een bezet database dan in een minder bezet database.  Dit type index management aanpassing zou zijn op de schaal van de database per tenant als deze grote beheertaak zou moeten handmatig worden uitgevoerd.

Andere functies die goed schaalbaar omvatten het volgende:

- Ingebouwde back-ups.
- Hoge beschikbaarheid.
- Versleuteling op schijf.
- Prestatietelemetrie.

#### <a name="automation"></a>Automatisering

De beheerbewerkingen die kunnen worden vastgelegd in een script en die worden aangeboden via een [devops] [ http-visual-studio-devops-485m] model.  De bewerkingen kunnen zelfs worden geautomatiseerd en worden weergegeven in de toepassing.

Bijvoorbeeld, kan u het herstel van een enkele tenant naar een eerder punt in tijd automatiseren.  Het herstel hoeft slechts één enkel-tenant-database die de tenant opslaat herstellen.  Deze restore heeft geen invloed op een andere tenants, waarin wordt bevestigd dat beheerbewerkingen op het niveau van contexttoken gedetailleerde van elke afzonderlijke tenant zijn.

## <a name="d-multi-tenant-app-with-multi-tenant-databases"></a>D. Multitenant-app met meerdere tenants databases

Patroon voor een ander beschikbaar is voor het opslaan van tal van tenants in een multitenant-database.  Het toepassingsexemplaar kan een willekeurig aantal databases van meerdere tenants hebben.  Het schema van een multitenant-database moet een of meer tenant-id kolommen hebben, zodat de gegevens van een bepaalde tenant selectief kunnen worden opgehaald.  Bovendien wordt het schema mogelijk enkele tabellen of kolommen die worden gebruikt door alleen een subset van tenants.  Statische code- en referentiegegevens samenvoegt wordt echter slechts één keer wordt opgeslagen en wordt gedeeld door alle tenants.

#### <a name="tenant-isolation-is-sacrificed"></a>Isolatie van tenants is gedood

*Gegevens:* &nbsp; noodzakelijkerwijs offert isolatie van tenants een multitenant-database.  De gegevens van meerdere tenants worden samen in één database worden opgeslagen.  Tijdens de ontwikkeling, zorg ervoor dat de query's gegevens uit meer dan één tenant nooit zichtbaar.  SQL Database ondersteunt [rijniveau beveiliging][docu-sql-svr-db-row-level-security-947w], die kan worden afgedwongen dat gegevens die zijn geretourneerd door een query moet binnen het bereik van een enkele tenant.

*Verwerking:* &nbsp; een multitenant-database deelt berekenings-en opslagbronnen over alle de tenants.  De database in zijn geheel worden gecontroleerd om te controleren of dat deze presteren presteert.  Het Azure systeem heeft echter geen ingebouwde manier om te controleren of het gebruik van deze bronnen door een afzonderlijke tenant beheren.  Daarom zijn de multitenant-database voor een verhoogd risico op problemen ruis neighbors, waarbij de werkbelasting van één overactive tenant heeft impact op de ervaring van de prestaties van andere tenants in dezelfde database.  Aanvullende controle op toepassingsniveau kan op tenantniveau prestaties controleren.

#### <a name="lower-cost"></a>Lagere kosten

Multitenant-databases hebben in het algemeen de laagste per-tenant kosten.  Resourcekosten voor een zelfstandige database zijn lager is dan voor een oftewel grootte elastische pool.  Bovendien voor scenario's waarbij tenants alleen beperkt opslag moeten kunnen mogelijk miljoenen tenants worden opgeslagen in een individuele database.  Er is geen elastische pool kan miljoenen databases bevatten.  Een oplossing met 1000 databases per groep met toepassingen die 1000, kan echter de schaal van miljoenen indien het lastig voor het beheren van steeds bereiken.

Twee varianten van een multitenant-databasemodel worden besproken in het volgende met het shard multitenant-model wordt de meest flexibele en schaalbare.

## <a name="e-multi-tenant-app-with-a-single-multi-tenant-database"></a>E. Multitenant-app met een enkele multitenant-database

De eenvoudigste multitenant database patroon gebruikt een zelfstandige database als host voor gegevens voor alle tenants.  Als meer tenants worden toegevoegd, wordt de database uitgebreid met meer opslagruimte en rekencapaciteit bronnen.  Deze schaal omhoog mogelijk alle die is vereist, hoewel er altijd een limiet ultimate schaal.  Lange voordat deze limiet wordt bereikt de database wordt echter lastig zijn om te beheren.

Beheerbewerkingen die zijn gericht op individuele tenants zijn moeilijker te implementeren in een multitenant-database.  En kan op grote schaal deze bewerkingen worden onaanvaardbaar langzaam.  Een voorbeeld hiervan is een punt in tijd terugzetten van de gegevens voor één tenant.

## <a name="f-multi-tenant-app-with-sharded-multi-tenant-databases"></a>F. Multitenant-app met shard multitenant-databases

De meeste SaaS-toepassingen toegang tot de gegevens van slechts één tenant tegelijk.  Dit toegangspatroon maakt het mogelijk gegevens van de tenant verdeeld over meerdere databases of shards, waarbij alle gegevens voor een tenant is opgenomen in één shard.  In combinatie met een patroon multitenant-database, kunt een shard model vrijwel onbeperkte schaal.

![Ontwerp van multitenant-app met shard multitenant-databases.][image-mt-app-sharded-mt-db-174s]

#### <a name="manage-shards"></a>Shards beheren

Sharding voegt complexiteit bij het ontwerp en de operationele beheer.  Een catalogus is vereist voor het onderhouden van de toewijzing tussen tenants en databases.  Bovendien zijn management procedures vereist voor het beheren van de shards en de tenant-populatie.  Bijvoorbeeld, moeten procedures zijn ontworpen toevoegen en verwijderen van shards en tenant om gegevens te verplaatsen tussen shards.  Een manier om te schalen is door een nieuwe shard toevoegen en deze vullen met nieuwe tenants.  Op andere tijden kunt u een dichtbevolkte shard splitsen in twee minder veel ingevuld shards.  Nadat u hebt verschillende tenants verplaatst of stopgezet, kunt u zeer ingevuld shards samenvoegen.  De samenvoeging zou leiden tot meer voordelige bronnen beter worden benut.  Tenants kunnen ook worden verplaatst tussen shards saldo werkbelastingen.

SQL-Database biedt een gesplitste/merge-hulpprogramma dat in combinatie met de sharding-bibliotheek en de catalogusdatabase werkt.  De opgegeven app kan worden gesplitst en merge shards en het tenant-gegevens kunt verplaatsen tussen shards.  De app onderhoudt ook de catalogus tijdens deze bewerkingen wordt de markering van invloed op een tenants als offline is voordat ze worden verplaatst.  Na het verplaatsen, de app-updates de catalogus opnieuw met de nieuwe toewijzing en het markeren van de tenant als weer online is.

#### <a name="smaller-databases-more-easily-managed"></a>Kleinere databases meer eenvoudig worden beheerd

Door tenants over meerdere databases worden verdeeld, wordt de shard multitenantoplossing resulteert in kleinere databases die eenvoudiger worden beheerd.  Bijvoorbeeld, omvat een specifieke tenant herstellen naar een eerdere punt in tijd nu één kleinere database is teruggezet vanuit een back-up, in plaats van een grotere database waarin alle tenants. De grootte van de database en het aantal tenants per database kunnen worden gekozen om de werkbelasting en de risicobeheerinspanningen in balans.

#### <a name="tenant-identifier-in-the-schema"></a>Tenant-id in het schema

Afhankelijk van de sharding-methode gebruikt, kunnen extra beperkingen worden opgelegd voor het databaseschema.  De toepassing van de gesplitste/merge SQL-Database is vereist dat het schema de sharding-sleutel, die meestal de tenant-id is bevat.  De tenant-id is het toonaangevende element in de primaire sleutel van alle shard tabellen.  De tenant-id kan de toepassing gesplitste/merge snel zoeken en verplaatsen van gegevens die zijn gekoppeld aan een specifieke tenant.

#### <a name="elastic-pool-for-shards"></a>Elastische pool voor shards

Shard multitenant-databases kunnen worden geplaatst in elastische pools.  Met veel databases voor één tenant in een pool is over het algemeen als kosten efficiënter veel tenants dat in enkele multitenant-databases.  Multitenant databases zijn nuttig wanneer er een groot aantal relatief inactieve tenants.

## <a name="g-hybrid-sharded-multi-tenant-database-model"></a>G. Hybride shard multitenant databasemodel

In het hybride-model hebben alle databases die de tenant-id in hun schema.  De databases zijn alle geschikt is voor het opslaan van meer dan één tenant en de databases kunnen shard.  Zodat ze alle databases van meerdere tenants in de zin schema zijn.  In de praktijk bevatten sommige van deze databases nog slechts één tenant.  Ongeacht, heeft het aantal tenants die zijn opgeslagen in een bepaalde database geen invloed op het databaseschema.

#### <a name="move-tenants-around"></a>Tenants verplaatsen

U kunt op elk gewenst moment een bepaalde tenant verplaatsen naar een eigen multitenant-database.  En op elk gewenst moment kunt u van gedachten verandert en de tenant verplaatst naar een database met meerdere tenants.  U kunt ook een huurder toewijzen aan nieuwe database voor één tenant wanneer u de nieuwe database inricht.

Het model hybride dat wat als er grote verschillen tussen de resourcebehoeften van persoonsgegevens groepen van tenants.  Stel bijvoorbeeld dat tenants die deel uitmaken van een gratis proefversie zijn niet gegarandeerd hetzelfde hoog niveau van de prestaties die geabonneerde tenants.  Het beleid mogelijk voor tenants in de gratis proefperiode moeten worden opgeslagen in een multitenant-database die wordt gedeeld door de gratis proefversie tenants.  Wanneer u een gratis proefversie van uw tenant is lid van het algemene serviceniveau, kan de tenant worden verplaatst naar een andere database voor meerdere tenants die mogelijk minder tenants.  Een abonnee voor het serviceniveau premium betaalt kan worden verplaatst naar een eigen nieuwe database voor één tenant.

#### <a name="pools"></a>Pools

In dit model hybride kunnen de databases voor één tenant voor abonnee tenants worden geplaatst in resourcegroepen databasekosten per tenant te verlagen.  Dit geldt in het model van de database per tenant.

## <a name="h-tenancy-models-compared"></a>H. Tenancymodus modellen vergeleken

De volgende tabel geeft een overzicht van de verschillen tussen de belangrijkste tenancymodus-modellen.

| Meting | Zelfstandige app | Database per tenant | Shard multitenant |
| :---------- | :------------- | :------------------ | :------------------- |
| Schalen | Middelgroot<br />1 100s | Zeer hoog<br />1 100.000 | Onbeperkt<br />1 1.000.000 |
| Isolatie van tenants | Zeer hoog | Hoog | Laag; met uitzondering van een singleton-tenant (dat wil zeggen alleen in een db MT). |
| Databasekosten per tenant | Hoog; voor pieken wordt aangepast. | Laag; Pools gebruikt. | Laagste, voor kleine tenants in MT-databases. |
| Bewaking van toepassingsprestaties en beheer | Per-tenant alleen | Aggregatie + per tenant | Aggregatie; Hoewel per tenant alleen voor singletons is. |
| Ontwikkeling complexiteit | Laag | Laag | Gemiddeld; Als gevolg van sharding. |
| Operationele complexiteit | Laag-hoog. Afzonderlijk eenvoudige en complexe op grote schaal. | Laag-Medium. Patronen complexiteit op grote schaal. | Laag-hoog. Beheer van een afzonderlijke tenant is complex. |
| &nbsp; ||||

## <a name="next-steps"></a>Volgende stappen

- [Implementeren en een multitenant Wingtip-toepassing die gebruikmaakt van het model van de SaaS-database per tenant - Azure SQL Database verkennen][docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]

- [Welkom bij de Wingtip Tickets SaaS Azure SQL Database tenancymodus voorbeeldapp][docu-saas-tenancy-welcome-wingtip-tickets-app-384w]


<!--  Article link references.  -->

[http-visual-studio-devops-485m]: https://www.visualstudio.com/devops/

[docu-sql-svr-db-row-level-security-947w]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security

[docu-elastic-db-client-library-536r]: sql-database-elastic-database-client-library.md
[docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]: sql-database-saas-tutorial.md
[docu-sql-db-automatic-tuning-771a]: sql-database-automatic-tuning.md
[docu-saas-tenancy-welcome-wingtip-tickets-app-384w]: saas-tenancy-welcome-wingtip-tickets-app.md


<!--  Image references.  -->

[image-standalone-app-st-db-111a]: media/saas-tenancy-app-design-patterns/saas-standalone-app-single-tenant-database-11.png "Ontwerp van zelfstandige app met exact één database voor één tenant."

[image-mt-app-db-per-tenant-132d]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-13.png "Ontwerp van multitenant-app met database per tenant."

[image-mt-app-db-per-tenant-pool-153p]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-pool-15.png "Ontwerp van multitenant-app met de database-per-tenant, met behulp van de elastische groep."

[image-mt-app-sharded-mt-db-174s]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-sharded-multi-tenant-databases-17.png "Ontwerp van multitenant-app met shard multitenant-databases."

