---
title: Ontwerppatronen voor multitenant SaaS-toepassingen en Azure SQL Database | Microsoft Docs
description: Meer informatie over de vereisten en algemene gegevens architectuur patronen van meerdere tenants software als een dienst (SaaS)-database-toepassingen die worden uitgevoerd in een cloudomgeving.
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 02/01/2017
ms.author: srinia
ms.openlocfilehash: eef48cfcbc7d6c241b5ece863df0be6ecad78ca7
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="design-patterns-for-multi-tenant-saas-applications-and-azure-sql-database"></a>Ontwerppatronen voor multitenant SaaS-toepassingen en Azure SQL Database
In dit artikel kunt u lezen over de vereisten en algemene gegevens architectuur patronen van meerdere tenants software als een dienst (SaaS)-database-toepassingen die worden uitgevoerd in een cloudomgeving. Ook wordt de factoren die u moet rekening houden en de verschillen van verschillende ontwerppatronen uitgelegd. Elastische pools en elastische hulpprogramma's in Azure SQL Database kunt u uw specifieke behoeften zonder andere doelstellingen gevaar te brengen.

Ontwikkelaars maken soms keuzes die op basis van hun op lange termijn belang werken bij het ontwerpen van tenancymodus modellen voor de gegevenslagen van multitenant-toepassingen. In eerste instantie is ten minste een ontwikkelaar mogelijk over denken gebruiksgemak ontwikkelings- en cloud serviceprovider kosten te verlagen als belangrijker dan de isolatie van tenants of de schaalbaarheid van een toepassing. Deze keuze kan leiden tot tevredenheid vragen van klanten en een kostbare loop-correctie later.

Een multitenant-toepassing is een toepassing die wordt gehost in een cloudomgeving en biedt dezelfde set van services op honderden of duizenden tenants die niet delen of elkaars gegevens zien. Een voorbeeld is een SaaS-toepassing die services aan tenants in een cloud gehoste omgeving levert.

## <a name="multi-tenant-applications"></a>Multitenant-toepassingen
In toepassingen met meerdere tenants, kunnen gegevens en werkbelasting eenvoudig worden gepartitioneerd. U kunt partitioneren gegevens en de belasting, bijvoorbeeld langs de grenzen van de tenant, omdat de meeste aanvragen worden uitgevoerd binnen de grenzen van een tenant. Deze eigenschap is inherent aan de gegevens en de werkbelasting en deze hiermee de toepassing patronen besproken in dit artikel worden verbeterd.

Dit type toepassing ontwikkelaars gebruiken in het hele spectrum van cloud-gebaseerde toepassingen, waaronder:

* Partner databasetoepassingen die worden overgezet naar de cloud als SaaS-toepassingen
* SaaS-toepassingen die zijn opgebouwd voor de cloud bouwen
* Directe, klantgerichte toepassingen
* Werknemers gerichte bedrijfstoepassingen

SaaS-toepassingen die zijn ontworpen voor de cloud en die met hoofdmappen zijn databasetoepassingen doorgaans partner multitenant-toepassingen. Deze SaaS-toepassingen bieden een gespecialiseerde softwaretoepassing als een service aan hun tenants. Tenants kunnen toegang heeft tot de toepassingsservice en volledige eigendom van de bijbehorende gegevens die zijn opgeslagen als onderdeel van de toepassing. Maar als u wilt profiteren van de voordelen van SaaS, tenants enige controle over hun eigen gegevens moeten afstand. Vertrouwen ze de SaaS-provider om te voorkomen dat hun gegevens veilig en geïsoleerde gegevens van andere tenants. Voorbeelden van dit soort multitenant SaaS-toepassing zijn MYOB SnelStart en Salesforce.com. Elk van deze toepassingen kan worden gepartitioneerd langs de grenzen van de tenant en ondersteuning voor de toepassing ontwerppatronen besproken in dit artikel.

Toepassingen die een direct-service bieden voor klanten of werknemers binnen een organisatie (vaak aangeduid als gebruikers in plaats van tenants) zijn een andere categorie van het spectrum multitenant-toepassing. Klanten abonneren op de service en de gegevens die de serviceprovider verzamelt en slaat geen eigenaar. Serviceproviders hebben minder strenge vereisten voor het bewaren van gegevens van hun klanten van elkaar geïsoleerd buiten overheid voorgeschreven privacy voorschriften. Voorbeelden van dergelijke klantgerichte multitenant toepassing zijn inhoudsproviders van media zoals Netflix, Spotify en Xbox LIVE. Andere voorbeelden van toepassingen eenvoudig partitioneerbare klantgerichte, Internet-toepassingen of toepassingen van Internet der dingen (IoT) in elke klant of het apparaat kan fungeren als een partitie. Grenzen van partities kunnen afzonderlijke gebruikers en apparaten.

Niet alle toepassingen partitie eenvoudig langs één eigenschap zoals tenant, klant, gebruiker of apparaat. Een complexe ERP-toepassing (ERP), heeft bijvoorbeeld producten, orders en klanten. Deze heeft meestal een complexe schema met duizenden maximaal gekoppelde tabellen.

Een strategie voor geen enkele partitie kunt toepassen op alle tabellen en werkt via een toepassing werklast. Dit artikel is gericht op een multitenant-toepassingen waarvoor eenvoudig partitioneerbare gegevens en werkbelastingen.

## <a name="multi-tenant-application-design-trade-offs"></a>Ontwerp-en nadelen multitenant-toepassing
Het ontwerppatroon voor een multitenant-toepassingsontwikkelaar normaal gesproken kiest is gebaseerd op een afweging van de volgende factoren:

* **Tenantisolatie**. De ontwikkelaar moet ervoor zorgen dat tenants geen ongewenste toegang tot gegevens van andere tenants. De vereiste isolatie wordt uitgebreid naar andere eigenschappen, zoals beveiliging bieden van veel ruis veroorzaken neighbors, wordt een tenant gegevens kunnen herstellen en tenant-specifieke aanpassingen implementeren.
* **Kosten van de resource cloud**. Een SaaS-toepassing moet kostenbesparende cloudoplossing. Een ontwikkelaar van multitenant-toepassingen kunt kiezen om optimaliseren voor lagere kosten in het gebruik van cloudresources, zoals opslag en berekeningen kosten.
* **Gebruiksgemak DevOps**. Een ontwikkelaar van toepassingen met meerdere tenants moet gebruikmaken van isolatie beveiliging, onderhouden, en controleren van de status van de toepassing en het databaseschema en oplossen van problemen van de tenant. Complexiteit in de ontwikkeling van toepassingen en de bewerking wordt rechtstreeks omgezet in extra kosten en uitdagingen met tevredenheid tenant.
* **Schaalbaarheid**. De mogelijkheid om te stapsgewijs meer tenants en capaciteit toevoegen voor tenants die behoefte hebben aan deze is van cruciaal belang voor een geslaagde SaaS-bewerking.

Elk van deze factoren heeft verschillen vergeleken met een andere. De laagste kosten cloud aanbieden van de meest geschikte ontwikkeling biedt niet aanbieden. Het is belangrijk voor een ontwikkelaar op de hoogte keuzes maken over deze opties en hun verschillen tijdens het ontwerpproces voor de toepassing.

Een patroon populaire ontwikkeling is het inpakken van meerdere tenants naar een of meer databases. De voordelen van deze benadering zijn lagere kosten, omdat u voor enkele databases en de relatieve eenvoud betaalt van het werken met een beperkt aantal databases. Maar na verloop van tijd een ontwikkelaar van de SaaS-multitenant-toepassingen wordt Houd er rekening mee dat deze keuze aanzienlijke nadelen in isolatie van tenants en schaalbaarheid heeft. Als de isolatie van tenants is belangrijk, worden extra moeite is vereist voor tenant-gegevens in gedeelde opslag beveiligen tegen onbevoegde toegang of ruis neighbors. Deze extra moeite kan aanzienlijk verbeteren ontwikkelingsinspanningen en isolatie onderhoudskosten. Op dezelfde manier als voor het toevoegen van tenants vereist is, is dit ontwerppatroon doorgaans expertise opnieuw distribueren van gegevens van de tenant over databases voor de gegevenslaag van een toepassing goed worden geschaald.  

Isolatie van tenants vaak is een fundamenteel vereiste in SaaS-multitenant-toepassingen die geschikt zijn voor bedrijven en organisaties. Een ontwikkelaar kan verleiding komen door merkbare voordelen in eenvoud en kosten ten opzichte van de isolatie van tenants en schaalbaarheid. Deze wisselwerking kan bewijzen complexe en dure als de service groeit en de vereisten voor tenant-netwerkisolatie worden beheerd op de toepassingslaag en meer belangrijk. Isolatie van tenants kan echter in een multitenant-toepassingen die een directe, consumentgerichte service aan klanten bieden, zijn een lagere prioriteit dan optimaliseren voor de kosten van de resource cloud.

## <a name="multi-tenant-data-models"></a>Multitenant gegevensmodellen
Algemene ontwerp procedures voor het plaatsen van gegevens van de tenant volgen drie verschillende modellen, weergegeven in afbeelding 1.

![multitenant toepassingsgegevensmodellen](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

Afbeelding 1: Algemene ontwerp procedures voor meerdere tenants gegevensmodellen

* **Database per tenant**. Elke tenant heeft zijn eigen database. Alle gegevens van de tenant-specifieke is beperkt tot de database van de tenant en geïsoleerd van andere tenants en hun gegevens.
* **Gedeelde database shard**. Meerdere tenants delen één van meerdere databases. Een unieke set van tenants is toegewezen aan elke database met behulp van een strategie voor partitionering zoals hash, bereik of lijst partitioneren. Deze strategie verdeling van gegevens vaak aangeduid als sharding.
* **Database-enkel gedeeld**. Een enkelvoudige, soms grote-database bevat gegevens voor alle tenants, met de ondubbelzinnig kunnen worden gemaakt in een tenant-ID-kolom.

> [!NOTE]
> Een toepassingsontwikkelaar kunt kiezen om verschillende tenants plaats in de andere database, schema en gebruik vervolgens de naam van het schema te onderscheiden van de verschillende tenants. We raden deze benadering niet omdat hiervoor meestal het gebruik van dynamische SQL en kan niet effectief in het plan opslaan in cache. We richten op de gedeelde tabel benadering voor deze categorie van multitenant-toepassing in de rest van dit artikel.
> 
> 

## <a name="popular-multi-tenant-data-models"></a>Populaire multitenant gegevensmodellen
Het is belangrijk om te bepalen van de verschillende soorten multitenant gegevensmodellen in termen van de toepassing ontwerp-en nadelen die we al hebt geïdentificeerd. Deze factoren helpen kenmerkend zijn voor de drie meest voorkomende multitenant gegevensmodellen eerder beschreven en hun Databasegebruik zoals aangegeven in afbeelding 2.

* **Isolatie**. De mate van isolatie tussen de tenants mag een meting van hoeveel isolatie van tenants een gegevensmodel bereikt.
* **Kosten van de resource cloud**. De hoeveelheid resources delen tussen tenants kunt optimaliseren resourcekosten van de cloud. Een bron kan worden gedefinieerd als de kosten voor berekeningen en opslag.
* **DevOps kosten**. Het gemak van de ontwikkeling van toepassingen, implementatie en beheerbaarheid reduceert de totale kosten voor SaaS-bewerking.  

In afbeelding 2 toont de Y-as het niveau van de isolatie van tenants. De X-as geeft het niveau van het delen van bronnen. De grijze pijl in het midden geeft de richting van de DevOps-kosten, waarbij vergroten of verkleinen.

![Ontwerppatronen voor populaire multitenant-toepassing](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

Afbeelding 2: Populaire multitenant gegevensmodellen

De rechterbenedenhoek in afbeelding 2 ziet u een patroon van de toepassing die gebruikmaakt van een potentieel grote gedeelde één database, en de gedeelde tabel (of apart schema) benadering. Het is goed voor resource omdat alle huurders dezelfde database bronnen (CPU, geheugen, i/o) in één database gebruiken delen. Maar isolatie van tenants is beperkt. U moet mogelijk extra stappen ondernemen om te tenants beschermen tegen elkaar op de toepassingslaag. Deze aanvullende stappen kunnen aanzienlijk verbeteren de DevOps-kosten voor het ontwikkelen en beheren van de toepassing. Schaalbaarheid wordt beperkt door de schaal van de hardware die als host fungeert voor de database.

De Kwadrant linksonder in afbeelding 2 ziet u meerdere tenants shard tussen meerdere databases (doorgaans de andere hardware eenheden schalen). Elke database als host fungeert voor een subset van tenants, waarvan de betrekking schaalbaarheid van andere patronen. Als meer capaciteit vereist voor meer tenants is, kunt u eenvoudig de tenants op nieuwe databases die zijn toegewezen aan nieuwe hardware schaaleenheden plaatsen. Echter, de hoeveelheid resources delen wordt verminderd. Alleen tenants geplaatst op de dezelfde schaaleenheden die resources delen. Deze aanpak biedt weinig verbetering voor tenantisolatie, omdat er veel tenants nog steeds worden geplaatst zonder wordt automatisch beveiligd tegen elkaars acties. Complexiteit van de toepassing blijft hoog.

De linkerbovenhoek Kwadrant in afbeelding 2 is de derde benadering. Gegevens van elke tenant wordt in een eigen database. Deze aanpak goede isolatie van tenants eigenschappen heeft, maar beperkt resources delen wanneer elke database een eigen toegewijde bronnen heeft. Deze methode is geschikt als alle tenants voorspelbare werkbelastingen hebben. Als tenantwerkbelastingen minder voorspelbaar worden, de provider is niet geoptimaliseerd resources delen. Onvoorspelbaarheid is gebruikelijk voor SaaS-toepassingen. De provider moet ofwel te veel inrichten om te voldoen aan de eisen of lager resources. De actie resulteert in hogere kosten of lager tevredenheid van de tenant. Er wordt een hogere mate van resources te delen met tenants wenselijk om de oplossing kosteneffectiever zijn. Het aantal databases ook verhoogt, DevOps-kosten voor het implementeren en onderhouden van de toepassing. Ondanks deze problemen biedt deze methode de beste en gemakkelijkste isolatie voor tenants.

Deze factoren ook van invloed zijn op het ontwerppatroon voor dat een klant kiest:

* **Eigendom van gegevens van de tenant**. Het patroon van één database per tenant meer gericht op een toepassing waarin tenants eigendom van hun eigen gegevens behouden.
* **Schalen**. Een toepassing die gericht is op honderden of duizenden of miljoenen tenants Hiermee worden verbeterd benaderingen zoals sharding deling van databases. Vereisten voor netwerkisolatie kunnen nog steeds uitdagingen opleveren.
* **Waarde en zakelijke model**. Als een toepassing per-tenant omzet als zinvol klein (minder dan een dollar), de vereisten voor netwerkisolatie minder kritiek is geworden en een gedeelde database. Als omzet per tenant, enkele euro is, wordt een model database per tenant haalbaar is. Deze kan u helpen ontwikkelingskosten te verlagen.

Gezien de ontwerp-en nadelen wordt weergegeven in afbeelding 2, een ideaal multitenant model moet goed tenant isolatie eigenschappen opnemen met optimale resources te delen tussen de tenants. Dit model past in de categorie die wordt beschreven in de bovenste pas van afbeelding 2.

## <a name="multi-tenancy-support-in-azure-sql-database"></a>Ondersteuning voor multitenancy in Azure SQL Database
Azure SQL Database ondersteunt alle multitenant toepassing patronen die worden beschreven in afbeelding 2. Het ondersteunt ook het patroon van een toepassing die goede resources delen combineert met elastische pools en de voordelen van de isolatie van de database per tenant benadering (Zie de bovenste pas in afbeelding 3). Elastische database en -mogelijkheden in SQL-Database moet u helpen de kosten voor het ontwikkelen en beheren van een toepassing met veel databases (weergegeven in het gearceerde gedeelte in afbeelding 3) te verlagen. Deze hulpprogramma's kunt u maken en beheren van toepassingen die gebruikmaken van een van de patronen die meerdere databases.

![Patronen in Azure SQL Database](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

Afbeelding 3: Multitenant toepassing patronen in Azure SQL Database

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Database per tenant-model met elastische pools en hulpprogramma 's
Isolatie van tenants combineren elastische pools in SQL-Database met resources delen tussen databases van de tenant voor betere ondersteuning van de aanpak van de database per tenant. SQL-Database is een oplossing voor laag voor SaaS-providers die meerdere tenants toepassingen maken. De belasting van bronnen delen tussen de tenants wordt verplaatst van de toepassingslaag naar de database-service-laag. De complexiteit van het beheren en uitvoeren van query's op grote schaal tussen databases wordt vereenvoudigd met elastische taken, elastische query elastische transacties en de clientbibliotheek voor elastische database.

| Toepassingsvereisten | Mogelijkheden van de SQL-database |
| --- | --- |
| Isolatie van tenants en resources delen |[Elastische pools](sql-database-elastic-pool.md): toewijzen van een verzameling resources van de SQL-Database en de resources te delen tussen de verschillende databases. Ook tekenen afzonderlijke databases net zoveel bronnen uit de groep, indien nodig capaciteit vraag pieken vanwege wijzigingen in tenantwerkbelastingen voor. De elastische groep zelf kan worden geschaald omhoog of omlaag naar behoefte. Elastische pools bieden ook beheergemak en bewaking en probleemoplossing op het niveau van de groep van toepassingen. |
| Gebruiksgemak DevOps voor databases |[Elastische pools](sql-database-elastic-pool.md): als u eerder hebt genoteerd. |
| | [Elastische query](sql-database-elastic-query-horizontal-partitioning.md): Query voor databases voor rapportage of analyse cross-tenant. |
| | [Elastische taken](sql-database-elastic-jobs-overview.md): pakket en betrouwbaar onderhoudsbewerkingen database of wijzigingen in het schema database implementeren op meerdere databases. |
| | [Elastische transacties](sql-database-elastic-transactions-overview.md): proces tot verschillende databases wordt gewijzigd in een atomic- en geïsoleerde manier. Elastische transacties zijn nodig wanneer u toepassingen nodig 'Alles of niets' garanties over verschillende databasebewerkingen. |
| | [Clientbibliotheek voor elastische database](sql-database-elastic-database-client-library.md): gegevens distributies beheren en toewijzen van tenants naar databases. |

## <a name="shared-models"></a>Gedeelde modellen
Zoals eerder beschreven, voor de meeste SaaS-providers is een gedeeld model benadering oplevert problemen met tenant isolatie problemen en complexiteit met de ontwikkeling van toepassingen en onderhoud. Echter, voor multitenant-toepassingen die een service rechtstreeks aan de consument bieden, tenant-isolatievereisten mogelijk niet als hoge prioriteit als de kosten te minimaliseren. Ze mogelijk om in te pakken tenants in een of meer databases op een high-density om kosten te verlagen. Gedeelde databasemodellen met behulp van een individuele database of meerdere shard databases kunnen leiden tot extra efficiëntie in delen en de algehele kosten van de resource. Azure SQL Database biedt een aantal functies waarmee klanten bouwen isolatie voor betere beveiliging en beheer in de gegevenslaag.

| Toepassingsvereisten | Mogelijkheden van de SQL-database |
| --- | --- |
| Isolatie van beveiligingsfuncties |[Beveiliging](https://msdn.microsoft.com/library/dn765131.aspx) |
| [-Databaseschema](https://msdn.microsoft.com/library/dd207005.aspx) | |
| Gebruiksgemak DevOps voor databases |[Elastische query](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Elastische taken](sql-database-elastic-jobs-overview.md) |
| | [Elastische transacties](sql-database-elastic-transactions-overview.md) |
| | [Clientbibliotheek voor Elastic Database](sql-database-elastic-database-client-library.md) |
| | [Samenvoegen en splitsen elastische database](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Samenvatting
Vereisten voor tenant-netwerkisolatie zijn belangrijk voor de meeste multitenant SaaS-toepassingen. De beste optie om isolatie te bieden leans aanzienlijke richting van de aanpak van de database per tenant. De twee methoden vereist investeringen in complexe toepassingslagen waarvoor ervaren ontwikkeling personeel isolatie, waardoor aanzienlijk kosten en het risico te bieden. Als vereisten voor netwerkisolatie niet voor het begin van de serviceontwikkeling verwerkt zijn, zijn met terugwerkende kracht ze nog meer kostbare in de eerste twee modellen. De belangrijkste nadelen die zijn gekoppeld aan het model van de database per tenant hebben betrekking op de resourcekosten toegenomen cloud vanwege beperkte delen en te onderhouden en te veel databases beheren. Ontwikkelaars van SaaS-toepassingen is vaak het moeilijk wanneer ze deze verschillen.

Hoewel de verschillen mogelijk belangrijke barrières met de meeste cloudserviceproviders voor database, Azure SQL Database wordt voorkomen dat de barrières met de elastische groep en de mogelijkheden van de elastische database. SaaS-ontwikkelaars kunnen de isolatie-kenmerken van een model database per tenant combineren en delen van bronnen en de verbeteringen in beheerbaarheid van veel databases optimaliseren door elastische pools en bijbehorende hulpprogramma's.

Multitenant toepassing providers die geen vereisten voor tenant-netwerkisolatie hebben en die tenants kunt verpakken in een database op een high-density wellicht dat de gedeelde gegevens resultaat in extra efficiëntie bij het delen van bronnen modellen en verlagen de totale kosten. Azure SQL Database elastische database-hulpprogramma's, sharding-bibliotheken en beveiligingsfuncties helpen aanbieders van SaaS ontwikkelen en beheren van multitenant-toepassingen.

## <a name="next-steps"></a>Volgende stappen
[Aan de slag met hulpprogramma's voor elastische database](sql-database-elastic-scale-get-started.md) met een voorbeeld-app die u laat zien van de clientbibliotheek.

Maak een [aangepaste dashboard van de elastische groep voor SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) met een voorbeeld-app die gebruikmaakt van elastische pools voor een rendabele, schaalbare databaseoplossing.

Gebruik de Azure SQL Database-hulpmiddelen voor [migreren van bestaande databases uit te schalen](sql-database-elastic-convert-to-use-elastic-tools.md).

Zie voor een elastische pool maken met de Azure-portal, [maken van een elastische pool](sql-database-elastic-pool-manage-portal.md).  

Meer informatie over hoe [bewaken en beheren van een elastische pool](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Implementeren en een multitenant-toepassing die gebruikmaakt van Azure SQL Database - Wingtip SaaS verkennen](sql-database-saas-tutorial.md)
* [Wat is een Azure elastische groep?](sql-database-elastic-pool.md)
* [Uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [multitenant-toepassingen met elastische database-hulpprogramma's en beveiliging](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Verificatie in multitenant-apps met behulp van Azure Active Directory en OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [De toepassing Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Vragen en functieaanvragen

Voor vragen, zoeken we in de [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Toevoegen van een functie-aanvraag in de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

