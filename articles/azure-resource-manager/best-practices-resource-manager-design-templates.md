---
title: Azure-sjablonen voor complexe oplossingen ontwerpen | Microsoft Docs
description: Bevat aanbevolen procedures voor het ontwerpen van Azure Resource Manager-sjablonen voor complexe scenario 's
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ce1141d6-ece7-4976-acea-1db1f775409e
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: dcc31f7a8c85a8f7fbd554371a66fb1e348bca17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="design-patterns-for-azure-resource-manager-templates-when-deploying-complex-solutions"></a>Ontwerppatronen voor Azure Resource Manager-sjablonen bij het implementeren van complexe oplossingen
Flexibele wijze op basis van Azure Resource Manager-sjablonen gebruikt, kunt u complexe topologieën implementeren snel en consistent. U kunt deze implementaties eenvoudig aanpassen als de offerings core evolueren of om varianten voor uitschieter scenario's of klanten mogelijk te maken.

In dit onderwerp maakt deel uit van een grotere technisch document. Om te lezen van het volledige papier, downloaden [World klasse Azure Resource Manager-sjablonen overwegingen en procedures bewezen](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

De voordelen van de onderliggende Azure Resource Manager combineren sjablonen met het aanpassingsvermogen en de leesbaarheid van Notation JSON (JavaScript Object). Met sjablonen, kunt u het volgende doen:

* Topologieën en hun werkbelasting consistent te implementeren.
* Alle resources in een toepassing die samen met resourcegroepen beheren.
* Op rollen gebaseerde toegangsbeheer (RBAC) juiste om toegang te verlenen aan gebruikers, groepen en services van toepassing.
* Gebruik deze koppelingen voor het stroomlijnen van taken zoals facturering samentellingen labels.

In dit artikel biedt details over het verbruik scenario's, architectuur en implementatie-patronen die zijn geïdentificeerd tijdens onze ontwerp sessies en de werkelijke sjabloon implementaties van klanten Azure Customer Advisory Team (AzureCAT). Verre academic, deze benaderingen procedures op de hoogte gebracht door de ontwikkeling van sjablonen voor 12 van de bovenkant op basis van Linux OSS-technologieën, waaronder zijn beproefde: Apache Kafka, Apache Spark, Cloudera, Couchbase, Hortonworks HDP, DataStax Enterprise aangedreven door Apache Cassandra, Elasticsearch Jenkins, MongoDB, PostgreSQL, Redis en Nagios. 

In dit artikel deelt deze bewezen procedures waarmee u kunt bouwen world klasse Azure Resource Manager-sjablonen.  

In onze werk met klanten hebben we verschillende Resource Manager-sjabloon verbruik ervaringen geïdentificeerd voor ondernemingen, systeem SI's (SI) s en CSV's. De volgende secties bevatten een overzicht van veelvoorkomende scenario's en patronen voor verschillende soorten klanten.

## <a name="enterprises-and-system-integrators"></a>Ondernemingen en systeemintegrators
In grote organisaties vaak ziet u twee consumenten van Resource Manager-sjablonen: ontwikkelteams interne software en zakelijke IT. We hebt gevonden die de scenario's voor de kaart SIs voor de scenario's voor ondernemingen, zodat dezelfde overwegingen van toepassing.

### <a name="internal-software-development-teams"></a>Interne software ontwikkelteams
Als uw team software ontwikkelt ter ondersteuning van uw bedrijf, bieden sjablonen een eenvoudige manier om snel implementeren technologieën voor gebruik in bedrijfsspecifieke-oplossingen. U kunt ook sjablonen snel training om omgevingen te maken waarmee teamleden krijgen vaardigheden die nodig zijn.

U kunt sjablonen als-is of uitbreiden of opstellen ze aan uw behoeften. Met labels binnen sjablonen, kunt u een facturering samenvatting met verschillende weergaven zoals team, project, afzonderlijke en education bieden.

Bedrijven willen vaak ontwikkelteams software om een sjabloon voor consistente implementatie van een oplossing te maken. De sjabloon vergemakkelijkt beperkingen zodat bepaalde items in deze omgeving vast blijven en kunnen niet worden overschreven. Een bank moet mogelijk een sjabloon wilt opnemen RBAC zodat programmeurs niet een oplossing voor bankieren om gegevens te verzenden naar een persoonlijke opslagaccount herzien.

### <a name="corporate-it"></a>Zakelijke IT
Zakelijke IT-organisaties doorgaans sjablonen gebruiken voor het leveren van cloud-capaciteit en cloud-gebaseerde mogelijkheden.

#### <a name="cloud-capacity"></a>Cloudcapaciteit
Er is een veelgebruikte manier voor zakelijke IT-groepen voor cloudcapaciteit voor teams met 't-shirt grootten', die standaard aanbieding grootten zoals kleine, middelgrote en grote zijn. De offerings t-shirt grootte kunnen verschillende brontypen meng en hoeveelheden en tegelijkertijd een niveau van standaardisering die het mogelijk maakt gebruik van sjablonen. De sjablonen bieden capaciteit in een consistente manier die wordt afgedwongen bedrijfsbeleid en labels wordt gebruikt voor terugstorting aan organisaties verbruikt.

U moet mogelijk bieden ontwikkeling, testen of productie-omgevingen waarbinnen de software-ontwikkelteams hun oplossingen kunnen implementeren. De omgeving heeft een vooraf gedefinieerde netwerktopologie en elementen die de software-ontwikkelteams niet wijzigen, zoals regels voor toegang tot het openbare internet en pakket-inspectie. Mogelijk hebt u ook de organisatie-specifieke rollen voor deze omgevingen met verschillende toegangsrechten voor de omgeving.

#### <a name="cloud-hosted-capabilities"></a>Cloud-gebaseerde mogelijkheden
U kunt sjablonen gebruiken ter ondersteuning van cloud-gebaseerde mogelijkheden, zoals afzonderlijke softwarepakketten of samengestelde aanbiedingen die worden aangeboden aan interne zakelijke regels. Een voorbeeld van een samengestelde aanbieding is analytics as a service-analyse, visualisatie en andere technologieën, geleverd in de configuratie van een geoptimaliseerde, met elkaar verbonden op een vooraf gedefinieerde netwerktopologie.

Cloud-gebaseerde mogelijkheden worden beïnvloed door de overwegingen voor beveiliging en rol tot stand gebracht door de cloudcapaciteit aanbiedt op waarop ze worden gebouwd. Deze mogelijkheden worden aangeboden ongewijzigd of als een beheerde service. Voor de laatste, toegang tot beperkte rollen zijn vereist om toegang te krijgen in de omgeving voor beheerdoeleinden.

## <a name="cloud-service-vendors"></a>Leveranciers van cloud-service
Na het praten met veel CSV's, geïdentificeerd we meerdere manieren om services voor uw klanten en de bijbehorende vereiste te implementeren.

### <a name="csv-hosted-offering"></a>Aanbieding CSV gehost
Als u uw aanbod in uw eigen Azure-abonnement host, twee hosting benaderingen gelden: een unieke implementatie implementeren voor elke klant of implementeren van schaaleenheden die onderbouwing van een gedeelde infrastructuur, die wordt gebruikt voor alle klanten.

* **Verschillende implementaties voor elke klant.** Verschillende implementaties per klant vereisen vaste topologieën van verschillende bekende configuraties. Deze implementaties mogelijk andere virtuele machine (VM) grootten, verschillend aantal knooppunten en andere hoeveelheden bijbehorende opslag. Labels van implementaties wordt gebruikt voor totaliseren facturering van elke klant. RBAC kan worden ingeschakeld zodat klanten toegang hebben tot aspecten van hun cloudomgeving.
* **Schaaleenheden in een gedeelde omgeving met meerdere tenants.** Een schaaleenheid voor omgevingen met meerdere tenants kan bestaan uit een sjabloon. In dit geval wordt dezelfde infrastructuur gebruikt ter ondersteuning van alle klanten. De implementaties vertegenwoordigen een groep resources die een niveau van de capaciteit voor de gehoste oplossing, zoals het aantal gebruikers en het aantal transacties leveren. Deze schaaleenheden worden verhoogd of verlaagd omdat vraag vereist.

### <a name="csv-offering-injected-into-customer-subscription"></a>CSV-aanbieding is opgenomen in het klantabonnement
U kunt uw om software te implementeren in abonnementen die eigendom zijn van end-klanten. U kunt sjablonen gebruiken voor het implementeren van verschillende implementaties in Azure-account van de klant.

Deze implementaties gebruikmaken van RBAC zodat u kunt bijwerken en beheren van de implementatie binnen het account van de klant.

### <a name="azure-marketplace"></a>Azure Marketplace
Als u wilt adverteren en de offerings via een marketplace, zoals Azure Marketplace verkopen kunt u sjablonen voor het leveren van verschillende soorten implementaties die worden uitgevoerd in Azure-account van een klant ontwikkelen. Deze unieke implementaties kunnen doorgaans worden beschreven als een t-shirt grootte (klein, normaal, groot), product/doelgroep type (community, developer, enterprise) of onderdeeltype (basic, hoge beschikbaarheid).  In sommige gevallen kunnen deze typen u bepaalde kenmerken van de implementatie, zoals VM-type of aantal schijven opgeven.

## <a name="oss-projects"></a>OSS-projecten
Resource Manager-sjablonen inschakelen in open-source projecten, een community voor het implementeren van een oplossing snel met bewezen. U kunt sjablonen opslaan in een GitHub-opslagplaats, zodat de community gedurende een bepaalde periode herzien kunt. Gebruikers kunnen deze sjablonen in hun eigen Azure-abonnementen implementeren.

De volgende secties beschrijven de zaken die u moet overwegen voordat het ontwerpen van uw oplossing.

## <a name="identifying-what-is-outside-and-inside-a-vm"></a>Wat is buiten en in een virtuele machine identificeren
Bij het ontwerpen van uw sjabloon is het handig om te kijken naar de vereisten in termen van wat buiten en in de virtuele machines (VM's is):

* Buiten betekent dat de virtuele machines en andere bronnen van uw implementatie, zoals de netwerktopologie tagging, verwijzingen naar de certificaten/geheimen en toegangsbeheer op basis van rollen. Al deze resources maken deel uit van uw sjabloon.
* Binnen betekent dat de geïnstalleerde software en de algehele status van de gewenste configuratie. Andere methoden, zoals VM-extensies of scripts worden gebruikt in een geheel of gedeeltelijk. Deze mechanismen kunnen worden geïdentificeerd en uitgevoerd door de sjabloon, maar niet in deze.

Algemene voorbeelden van activiteiten die u 'in het vak doen zou' zijn-  

* Installeren of verwijderen van serverfuncties en onderdelen
* Software installeren en configureren op het niveau van knooppunt of het cluster
* Websites op een webserver implementeren
* Database-schema's implementeren
* Register of andere typen van configuratie-instellingen beheren
* Bestanden en mappen beheren
* Starten, stoppen en beheren van processen en services
* Lokale groepen en gebruikersaccounts beheren
* Installeren en beheren van pakketten (.msi, .exe, yum, enz.)
* Omgevingsvariabelen beheren
* Uitvoeren van systeemeigen scripts (Windows PowerShell, bash, enz.)

### <a name="desired-state-configuration-dsc"></a>Configuratie van de gewenste status (DSC)
Gegevensclassificatie over de interne status van uw VM's na de implementatie, die u wilt controleren of deze implementatie niet 'nemen' uit de configuratie die u hebt gedefinieerd en ingeschakeld in broncodebeheer. Deze aanpak zorgt ervoor dat uw ontwikkelaars of beheerders geen ad-hoc wijzigingen aanbrengen in een omgeving die niet goedgekeurd, getest, of vastgelegd in broncodebeheer. Dit besturingselement is belangrijk, omdat de handmatige wijzigingen niet in verbinding met broncodebeheer zijn. Ze zijn ook niet deel uit van de standaardimplementatie en is van invloed op toekomstige geautomatiseerde implementaties van de software.

Configuratie van de gewenste status is ook belangrijk vanuit het beveiligingsoogpunt van buiten uw interne werknemers. Hackers probeert regelmatig te plegen op en misbruik van softwaresystemen. Als dat lukt, is het gebruikelijk zijn voor het installeren van bestanden en de status van een geïnfecteerd systeem anders te wijzigen. Configuratie van de gewenste status wordt gebruikt, kunt u zien delta's tussen de status van de gewenste en de daadwerkelijke en herstellen van een bekende configuratie.

Er zijn resource-uitbreidingen voor de meest populaire mechanismen voor DSC - PowerShell DSC, Chef en Puppet. Elk van deze extensies de beginstatus van de virtuele machine kunt implementeren en ook worden gebruikt om ervoor te zorgen dat de gewenste status wordt bijgehouden.

## <a name="common-template-scopes"></a>Algemene sjabloon scopes
In onze ervaring hebt we drie key-oplossing sjablonen scopes ontstaan gezien. Deze drie scopes, capaciteit, functionaliteit en end-to-end-oplossing, worden in de volgende secties beschreven.

### <a name="capacity-scope"></a>Capaciteit bereik
Een scope capaciteit biedt een aantal resources in een standaard topologie die is vooraf geconfigureerd moet voldoen aan de voorschriften en het beleid. De meest voorkomende voorbeeld is een standaard ontwikkelingsomgeving in een Enterprise-IT of SI-scenario implementeren.

### <a name="capability-scope"></a>Mogelijkheid bereik
Een scope mogelijkheid is gericht op het implementeren en configureren van een topologie voor een bepaalde technologie. Algemene scenario's met inbegrip van technologieën zoals SQL Server, Cassandra, Hadoop.

### <a name="end-to-end-solution-scope"></a>End-to-end oplossing bereik
Een End-to-End oplossing bereik is gericht dan een enkele functie en in plaats daarvan zijn gericht op een end-to-end-oplossing die bestaat uit meerdere mogelijkheden.  

Een sjabloon gericht op de oplossing scope doet zich voor als een set van een of meer sjablonen voor gericht op de mogelijkheid met oplossings-specifieke bronnen, de logica en de gewenste status. Een voorbeeld van een sjabloon gericht op de oplossing is een complete pijplijn oplossing gegevenssjabloon. De sjabloon kan oplossings-specifieke topologie en status meng met meerdere gericht op de mogelijkheid oplossingssjablonen zoals Kafka, Storm en Hadoop.

## <a name="choosing-free-form-vs-known-configurations"></a>Gratis formulier versus bekende configuraties te kiezen
U kunt in eerste instantie een sjabloon moet verschaffen consument de grootste flexibiliteit, maar de keuze van of u wilt gebruiken, vrije vorm configuraties versus bekende configuraties van invloed op een aantal zaken overwegen. Deze sectie worden de belangrijkste klantvereisten en technische overwegingen die in de vorm van de aanpak gedeeld in dit document.

### <a name="free-form-configurations"></a>Vrije-configuraties
Op het aardoppervlak geluid vrije vorm configuraties ideaal. Ze kunt u een VM-type selecteren en een willekeurig aantal knooppunten en de gekoppelde schijven voor knooppunten, en als parameters om een sjabloon te doen. Deze aanpak is echter niet ideaal voor een aantal scenario's.

In [grootten voor virtuele machines](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), worden geïdentificeerd door de verschillende typen van de virtuele machine en de beschikbare grootten en elk van het aantal duurzame schijven (2, 4, 8, 16 of 32) die kan worden gekoppeld. Elke gekoppelde schijf biedt 500 IOPS en veelvouden van deze schijven kunnen worden gegroepeerd voor vermenigvuldigingsfactor dat aantal IOPS. 16 schijven kunnen bijvoorbeeld worden gegroepeerd om 8000 IOP's bieden. Groeperen wordt met configuratie in het besturingssysteem, met behulp van Microsoft Windows Storage Spaces of redundante matrix van goedkope schijven (RAID) in Linux uitgevoerd.

De selectie van de configuratie van een vrije vorm kan enkele VM-instanties verschillende VM typen en groottes voor ALE exemplaren, verschillende schijven voor het VM-type en een of meer scripts voor het configureren van de VM-inhoud.

Het is gebruikelijk dat een implementatie mogelijk meerdere typen knooppunten, zoals de master- en gegevensknooppunten, zodat deze flexibiliteit vaak voor elk knooppunttype is opgegeven.

Als u begint met het implementeren van clusters een significante, wordt u begint te werken met deze complexe scenario's. Als u een Hadoop-cluster zijn implementeert, bijvoorbeeld zou met 8 hoofdknooppunten 200 gegevensknooppunten en gegroepeerde 4 gekoppelde schijven op elk knooppunt master en gegroepeerde 16 gekoppelde schijven per gegevensknooppunt, er 208 VM's en 3,232 schijven te beheren.

Een opslagaccount, wordt de geïdentificeerde 20.000 transacties per seconde beperken, zodat u moet partitioneren van de storage-account kijken en berekeningen gebruiken om te bepalen van het juiste aantal opslagaccounts voor deze topologie bovenstaande aanvragen vertraging. Gezien het grote aantal combinaties die worden ondersteund door de vrije vorm benadering, zijn dynamische berekeningen nodig om te bepalen van de juiste partitionering. De taal van de Azure Resource Manager-sjabloon biedt momenteel geen wiskundige functies, dus u moet deze berekeningen uitvoeren in code genereren van een unieke, vastgelegde sjabloon met de juiste informatie.

In de enterprise IT en SI-scenario's, moet iemand onderhouden van de sjablonen en ondersteuning voor de geïmplementeerde topologieën voor een of meer organisaties. Deze extra overhead: verschillende configuraties en sjablonen voor elke klant: sterk wenselijk.

U kunt deze sjablonen gebruiken voor het implementeren van omgevingen in Azure-abonnement van uw klant, maar zakelijke IT-teams en CSV's doorgaans deze implementeren in hun eigen abonnementen, met behulp van een functie terugstorting aan hun klanten factureren. In deze scenario's, het doel is om te capaciteit voor meerdere klanten in een pool van abonnementen implementeren en implementaties veel ingevuld in de abonnementen op abonnement kabels minimaliseren, dat wil zeggen, meer abonnementen beheren. Met echt dynamische implementatiegrootte vereist bereiken van dit type dichtheid zorgvuldige planning en extra ontwikkeling voor steigers werk voor de hele organisatie.

Bovendien geen abonnementen via een API-aanroep kunt maken maar handmatig moet doen via de portal. Als het aantal abonnementen toeneemt, alle resulterende abonnement kabels menselijke tussenkomst vereist, kan niet worden geautomatiseerd. U moet een aantal abonnementen handmatig om ervoor te zorgen abonnementen beschikbaar zijn in te richten met zoveel variabiliteit in de grootte van de implementaties.

Deze factoren in overweging moet aan een werkelijk vrije vorm-configuratie is minder dan ten eerste blush interessante.

### <a name="known-configurations--the-t-shirt-sizing-approach"></a>Bekende configuraties, de aanpak van t-shirt schaling
In plaats daarvan dan een sjabloon die voorziet in totaal flexibiliteit en talloze variaties bieden, in onze ervaring een algemene patroon is om aan te bieden de mogelijkheid om te selecteren van bekende configuraties — in feite standaard t-shirt groottes zoals sandbox, kleine, middelgrote en grote. Andere voorbeelden van t-shirt grootten zijn de producten, zoals community edition of enterprise edition.  In andere gevallen mogelijk werklastspecifiek configuraties van een technologie – zoals kaart verminderen of er zijn geen sql.

Veel bedrijven organisaties, OSS-leveranciers en SIs Maak de offerings beschikbaar zijn op deze manier in on-premises gevirtualiseerde omgevingen (ondernemingen) of als software as a service (SaaS)-aanbiedingen (CSV's en OSVs).

Deze aanpak geeft goede, bekende configuraties van verschillende grootten die vooraf zijn geconfigureerd voor klanten. Zonder bekende configuraties moeten end klanten cluster formaat op hun eigen bepalen, rekening te houden in resourcebeperkingen platform en rekenen om te identificeren van de resulterende partitionering van storage-accounts en andere bronnen (vanwege beperkingen van cluster grootte en resource). Bekende configuraties kunnen klanten eenvoudig selecteert u de juiste t-shirt grootte — dat wil zeggen, een bepaalde implementatie. Naast het maken van een betere ervaring voor de klant, een klein aantal bekende configuraties is gemakkelijker te ondersteunen en kunt u een hoger niveau van de dichtheid leveren.

Een benadering bekende configuratie is gericht op t-shirt grootten kan ook verschillend aantal knooppunten binnen een grootte hebben. Bijvoorbeeld, een klein t-shirt mogelijk tussen 3 en 10 knooppunten.  De grootte van de t-shirt zou worden ontworpen om geschikt voor maximaal 10 knooppunten en de consument bieden de mogelijkheid om selecties tot de maximale grootte geïdentificeerd vrije vorm te maken.  

De grootte van een t-shirt op basis van het type werkbelasting, mogelijk meer vrije vorm van aard in termen van het aantal knooppunten dat kan worden geïmplementeerd, maar heeft verschillende knooppunt werkbelastingsgrootte en configuratie van de software op het knooppunt.

T-shirt grootten op basis van producten, zoals community of Enterprise, mogelijk verschillende resourcetypen en maximum aantal knooppunten dat kan worden geïmplementeerd, worden doorgaans gekoppeld aan de licenties of beschikbaarheid van functies tussen de verschillende aanbiedingen.

U kunt ook geschikt voor klanten met unieke varianten met behulp van de sjablonen JSON-indeling. Uitschieters betreft, kunt u de juiste planning en overwegingen voor ontwikkeling, ondersteuning en de kosten kunt opnemen.

Op basis van de klant sjabloon verbruik scenario's en vereisten geïdentificeerd aan het begin van dit document, wordt een patroon voor de sjabloon ontleding geïdentificeerd.

## <a name="capacity-and-capability-scoped-solution-templates"></a>Capaciteit en de mogelijkheid bereik oplossingssjablonen
Afbreken biedt een modulaire benadering voor de ontwikkeling van de sjabloon die ondersteunt opnieuw gebruiken, uitbreidbaarheid, testen en tooling. Deze sectie bevat details over hoe een benadering afbreken met sjablonen met een capaciteit of mogelijkheden bereik kan worden toegepast.

Deze aanpak een belangrijkste sjabloon parameterwaarden ontvangt van een consumer sjabloon vervolgens koppelingen naar diverse typen sjablonen en scripts downstream zoals hieronder wordt weergegeven. Parameters en variabelen met statische gegenereerde variabelen worden gebruikt om waarden in en uit de gekoppelde sjablonen te leveren.

![Sjabloonparameters](./media/best-practices-resource-manager-design-templates/template-parameters.png)

**Parameters worden doorgegeven aan een belangrijkste sjabloon vervolgens naar de gekoppelde sjablonen**

De volgende secties zich richten op de typen sjablonen en scripts die één sjabloon is onderverdeeld. De secties vindt u methoden voor het doorgeven van informatie over de status van de sjablonen. Elke sjabloon en de scripttypen in de installatiekopie worden samen met voorbeelden beschreven. Zie voor een voorbeeld contextuele ' samen te stellen: een voorbeeldimplementatie ' verderop in dit document.

### <a name="template-metadata"></a>Metagegevens van de sjabloon
Metagegevens van de sjabloon (het metadata.json-bestand) bevat een sleutel-waardeparen die een sjabloon in de JSON die kan worden gelezen door mensen en softwaresystemen te beschrijven.

![Metagegevens van de sjabloon](./media/best-practices-resource-manager-design-templates/template-metadata.png)

**Metagegevens van de sjabloon wordt beschreven in het bestand metadata.json**

Softwareagents kunnen ophalen van het bestand metadata.json en publiceren van de informatie en een koppeling naar de sjabloon in een webpagina of directory. Elementen zijn *itemDisplayName*, *beschrijving*, *samenvatting*, *githubUsername*, en *dateUpdated*.

Een voorbeeld van een bestand wordt hieronder weergegeven in zijn geheel.

    {
        "itemDisplayName": "PostgreSQL 9.3 on Ubuntu VMs",
        "description": "This template creates a PostgreSQL streaming-replication between a master and one or more slave servers each with 2 striped data disks. The database servers are deployed into a private-only subnet with one publicly accessible jumpbox VM in a DMZ subnet with public IP.",
        "summary": "PostgreSQL stream-replication with multiple slave servers and a publicly accessible jumpbox VM",
        "githubUsername": "arsenvlad",
        "dateUpdated": "2015-04-24"
    }

### <a name="main-template"></a>Belangrijkste sjabloon
De belangrijkste sjabloon parameters van een gebruiker ontvangt, die gegevens worden gebruikt voor het vullen van complexe objectvariabelen en de gekoppelde sjablonen wordt uitgevoerd.

![Belangrijkste sjabloon](./media/best-practices-resource-manager-design-templates/main-template.png)

**De belangrijkste sjabloon ontvangt parameters van een gebruiker**

Een parameter die is opgegeven is een bekende configuratietype ook wel bekend als de parameter voor de t-shirt vanwege de gestandaardiseerde waarden zoals klein, normaal of groot. In de praktijk, kunt u deze parameter op verschillende manieren. Zie 'Bekende configuratie resources sjabloon' verderop in dit document voor meer informatie.

Sommige resources worden ongeacht de bekende configuratie wordt opgegeven door de parameter van een gebruiker geïmplementeerd. Deze resources zijn ingericht met een sjabloon met één gedeelde resource en worden gedeeld door andere sjablonen, dus de sjabloon voor de gedeelde bron eerste wordt uitgevoerd.

Sommige resources zijn optioneel ongeacht de opgegeven configuratie voor bekende geïmplementeerd.

### <a name="shared-resources-template"></a>Gedeelde bronnen sjabloon
Deze sjabloon biedt resources die door alle bekende configuraties gedeeld worden. Het bevat het virtuele netwerk, beschikbaarheidssets en andere bronnen die zijn vereist, ongeacht de bekende configuratie-sjabloon die wordt geïmplementeerd.

![Sjabloonresources](./media/best-practices-resource-manager-design-templates/template-resources.png)

**Gedeelde bronnen sjabloon**

Resourcenamen, zoals de naam van het virtuele netwerk zijn gebaseerd op de belangrijkste sjabloon. U kunt ze als een variabele in die sjabloon opgeven of ze ontvangen als een parameter van de gebruiker, zoals vereist door uw organisatie.

### <a name="optional-resources-template"></a>Optionele resources sjabloon
De sjabloon optionele resources bevat resources die programmatisch zijn geïmplementeerd op basis van de waarde van een parameter of variabele.

![Optionele resources](./media/best-practices-resource-manager-design-templates/optional-resources.png)

**Optionele resources sjabloon**

U kunt bijvoorbeeld een sjabloon optionele resources voor het configureren van een jumpbox waarmee indirecte toegang aan een gedistribueerde omgeving van het openbare Internet. U zou een parameter of variabele gebruiken om te bepalen of de jumpbox moet worden ingeschakeld en de *concat* functie voor het bouwen van de naam van het doel voor de sjabloon, zoals *jumpbox_enabled.json*. Sjabloon koppelen, zou de resulterende variabele voor het installeren van de jumpbox gebruiken.

U kunt een koppeling van de sjabloon optionele resources vanaf meerdere locaties:

* Wanneer voor elke implementatie, maakt u een koppeling basis van parameters van de sjabloon gedeelde bronnen.
* Indien van toepassing om te selecteren van bekende configuraties — bijvoorbeeld alleen installeren op grote implementaties: Maak een koppeling aangestuurde parameter of variabele op basis van beschikbare resources van de sjabloon bekende configuratie.

Hiermee wordt aangegeven of een bepaalde bron is optioneel mag niet worden aangedreven door de consument sjabloon maar door de sjabloonprovider. Bijvoorbeeld, moet u mogelijk om te voldoen aan een bepaald Productvereiste of product-invoegtoepassing (algemeen voor CSV's) of beleidsregels afdwingen (common voor SIs- en enterprise IT groepen). In dergelijke gevallen kunt u een variabele gebruiken om te bepalen of de resource moet worden geïmplementeerd.

### <a name="known-configuration-resources-template"></a>Configuratie voor bekende bronnen sjabloon
In de belangrijkste sjabloon kan een parameter worden blootgesteld zodat de consument sjabloon een gewenste bekende configuratie te implementeren op te geven. Deze bekende configuratie wordt vaak een benadering van de grootte t-shirt gebruikt met een set van configuratie van vaste grootte zoals sandbox, klein, normaal en groot.

![Configuratie voor bekende bronnen](./media/best-practices-resource-manager-design-templates/known-config.png)

**Configuratie voor bekende bronnen sjabloon**

De grootte t-shirt benadering wordt meestal gebruikt, maar de parameters kunnen bestaan uit een reeks bekende configuraties. U kunt bijvoorbeeld een reeks omgevingen voor een zakelijke toepassing zoals ontwikkelen, testen en Product opgeven. Of u deze voor een cloudservice kunt gebruiken om aan te duiden schaaleenheden voor verschillende versies van het product of configuraties van het product zoals Community-, Developer- of Enterprise.

Net als bij de gedeelde bron-sjabloon, worden variabelen doorgegeven aan de sjabloon bekende configuraties uit:

* Een eindgebruiker — dat wil zeggen, de parameters die worden verzonden naar de belangrijkste sjabloon.
* Een organisatie, dat wil zeggen, de variabelen in de belangrijkste sjabloon die staan voor interne vereisten of beleidsregels.

### <a name="member-resources-template"></a>Lid resources sjabloon
Een of meer knooppunt lidtypen zijn vaak opgenomen binnen een bekende configuratie. Bijvoorbeeld, met Hadoop hebt u hoofdknooppunten- en gegevensknooppunten. Als u MongoDB installeert, hebt u gegevensknooppunten en een instellingen. Als u DataStax implementeert, hebt u gegevensknooppunten en een virtuele machine met OpsCenter geïnstalleerd.

![Leden resources](./media/best-practices-resource-manager-design-templates/member-resources.png)

**Lid resources sjabloon**

Elk type knooppunten kan verschillende grootten van virtuele machines, het aantal gekoppelde schijven, scripts voor het installeren en instellen van de knooppunten, de poortconfiguraties voor de virtuele machines, het aantal exemplaren en andere gegevens hebben. Zodat elk knooppunttype opgehaald eigen lid resource sjabloon de details bevat voor het implementeren en configureren van een infrastructuur evenals scripts te implementeren en configureren van software binnen de virtuele machine wordt uitgevoerd.

Voor virtuele machines zijn twee soorten scripts meestal gebruikt, algemeen herbruikbare en aangepaste scripts.

### <a name="widely-reusable-scripts"></a>Algemeen herbruikbare scripts
Algemeen herbruikbare scripts kunnen worden gebruikt over meerdere typen sjablonen. Een van de betere voorbeelden van deze algemeen herbruikbare scripts RAID op Linux groep schijven en een groter aantal IOPS zo ingesteld. Ongeacht de software wordt geïnstalleerd in de virtuele machine, biedt dit script hergebruik van bewezen voor algemene scenario's.

![Herbruikbare scripts](./media/best-practices-resource-manager-design-templates/reusable-scripts.png)

**Lid resources sjablonen kunnen algemeen herbruikbare scripts aanroepen**

### <a name="custom-scripts"></a>Aangepaste scripts
Sjablonen roept vaak een of meer scripts die installeren en configureren van software binnen de virtuele machines. Een algemene patroon wordt weergegeven met grote topologieën waarin meerdere exemplaren van een of meer lidtypen zijn geïmplementeerd. Een script voor installatie wordt gestart voor elke virtuele machine die kan worden uitgevoerd parallel, gevolgd door een setup-script dat wordt aangeroepen nadat alle virtuele machines (of alle virtuele machines van het type van een opgegeven lid) zijn geïmplementeerd.

![Aangepaste scripts](./media/best-practices-resource-manager-design-templates/custom-scripts.png)

**Lid resources sjablonen kunnen scripts aanroepen voor een specifiek doel zoals VM-configuratie**

## <a name="capability-scoped-solution-template-example---redis"></a>Gericht op de mogelijkheid oplossing sjabloon voorbeeld - Redis
Om weer te geven hoe een implementatie mogelijk werken, kunt u een voorbeeld van het bouwen van een sjabloon voor de implementatie en configuratie van Redis in standaard t-shirt grootten gaan we kijken.  

Er zijn een set gedeelde bronnen (virtueel netwerk, storage-account, beschikbaarheidssets) en een optionele bron (jumpbox) voor de implementatie. Er zijn meerdere bekende configuraties die worden weergegeven als t-shirt grootten (klein, normaal, groot), maar met één knooppunt elk type. Er zijn ook twee specifiek doel-scripts (installatie, configuratie).

### <a name="creating-the-template-files"></a>De sjabloonbestanden maken
Maakt u een Main-sjabloon met de naam azuredeploy.json.

Maken van gedeelde Resources sjabloon met de naam gedeeld resources.json

Maken van een optionele Resource-sjabloon voor de implementatie van een jumpbox, met de naam jumpbox_enabled.json inschakelen

Redis maakt gebruik van slechts één knooppunt-type, zodat u één lid Resource sjabloon met de naam knooppunt resources.json maken.

Met Redis, die u wilt installeren van elke afzonderlijke knooppunten en het cluster vervolgens ingesteld.  U hebt mogelijk de installatie en het instellen van scripts, redis-cluster-install.sh en redis-cluster-setup.sh.

### <a name="linking-the-templates"></a>De sjablonen koppelen
Met behulp van sjabloon koppelt, de belangrijkste sjabloon koppelingen uit aan de sjabloon gedeelde bronnen, waaruit blijkt dat het virtuele netwerk.

Logica wordt toegevoegd in de belangrijkste sjabloon waarmee consumenten van de sjabloon op te geven of een jumpbox moet worden geïmplementeerd. Een *ingeschakeld* waarde voor de *EnableJumpbox* parameter geeft aan dat de klant wil een jumpbox implementeren. Wanneer deze waarde is opgegeven, de sjabloon worden aaneengeschakeld *_enabled* als achtervoegsel aan de naam van een basissjabloon voor de mogelijkheid jumpbox.

De belangrijkste sjabloon geldt de *grote* parameter waarde als achtervoegsel aan de naam van een basissjabloon voor t-shirt grootten en gebruikt vervolgens die waarde in een sjabloonkoppeling uit naar *technology_on_os_large.json*.

Deze afbeelding zou zijn vergelijkbaar met de topologie.

![Sjabloon redis](./media/best-practices-resource-manager-design-templates/redis-template.png)

**Structuur van de sjabloon voor een Redis-sjabloon**

### <a name="configuring-state"></a>Status configureren
Er zijn twee stappen voor de configuratie van de status voor de knooppunten in het cluster, allebei vertegenwoordigd door specifieke doel-Scripts.  'redis-cluster-install.sh' Redis installeert en 'redis-cluster-setup.sh' ingesteld in het cluster.

### <a name="supporting-different-size-deployments"></a>Ondersteuning van verschillende grootte implementaties
Binnen variabelen, de sjabloon van de grootte t-shirt geeft het aantal knooppunten van elk type te implementeren voor de opgegeven grootte (*grote*). Het aantal VM-exemplaren die gebruikmaken van de resource lussen, unieke namen voor de resources die door de naam van een knooppunt met een numerieke reeksnummer van toe te voegen implementeert *copyIndex()*. Wordt deze stappen voor hot en warme zone VM's, zoals gedefinieerd in de sjabloon t-shirt-naam

## <a name="decomposition-and-end-to-end-solution-scoped-templates"></a>End-to-end-oplossing en afbreken binnen het bereik van sjablonen
Een oplossingssjabloon met een bereik van de end-to-end-oplossing is gericht op een end-to-end-oplossing levert.  Deze aanpak is meestal een samenstelling van meerdere gericht op de mogelijkheid sjablonen met aanvullende bronnen, logische en status.

Als gemarkeerd in de onderstaande afbeelding, wordt gebruikt voor sjablonen van de mogelijkheid binnen het bereik van hetzelfde model uitgebreid voor sjablonen met een bereik van de oplossing voor End-to-End.

Een gedeelde Resources sjabloon en een optionele Resources sjablonen dienen dezelfde functie uit als in de capaciteit en mogelijkheden binnen het bereik van sjabloon benaderingen, maar zijn binnen het bereik van de end-to-end-oplossing.

Zoals binnen het bereik van end-to-end oplossing sjablonen kunnen doorgaans hebben t-shirt grootten, de sjabloon bekend configuratie Resources weerspiegelt wat is vereist voor een bepaalde bekende configuratie van de oplossing.

De configuratie Resources sjabloon bekend koppelingen naar een of meer functionaliteit binnen het bereik oplossingssjablonen die relevant zijn voor de end-to-end-oplossing, evenals de lid Resource-sjablonen die vereist voor de end-to-end-oplossing zijn.

Als de t-shirt grootte van de oplossing is mogelijk anders dan de afzonderlijke gericht op de mogelijkheid sjabloon, worden variabelen in de sjabloon bekend configuratie bronnen gebruikt voor de juiste waarden voor downstream mogelijkheid binnen het bereik van oplossingssjablonen voor het implementeren van de juiste t-shirt grootte.

![End-to-end](./media/best-practices-resource-manager-design-templates/end-to-end.png)

**Het model dat wordt gebruikt voor de capaciteit of mogelijkheden bereik oplossingssjablonen gemakkelijk kunnen worden uitgebreid voor end-to-end-oplossing sjabloon scopes**

## <a name="preparing-templates-for-the-marketplace"></a>Sjablonen voorbereiden voor de Marketplace
De voorgaande aanpak gemakkelijk is geschikt voor scenario's waarbij ondernemingen SIs en CSV's aan de sjablonen zelf implementeren of hun klanten te implementeren op hun eigen inschakelen.

Een andere gewenste scenario implementeert een sjabloon via de marketplace.  Deze aanpak afbreken werkt voor de marketplace, evenals met enkele kleine wijzigingen.

Zoals eerder vermeld, kunnen sjablonen worden gebruikt op het aanbod van verschillende typen voor verkoop in de marketplace. Afzonderlijke implementatietypen mogelijk t-shirt grootten (klein, normaal, groot), het product/doelgroep type (community, developer, enterprise) of onderdeeltype (basic, hoge beschikbaarheid).

Zoals hieronder wordt weergegeven, kunnen de mogelijkheid binnen het bereik van sjablonen of bestaande oplossing voor end-to-end worden direct gebruikt voor de verschillende bekende configuraties in de marketplace lijst.

De parameters voor de belangrijkste sjabloon worden eerst gewijzigd als het binnenkomende parameter met de naam tshirtSize wilt verwijderen.

Terwijl de afzonderlijke implementatietypen aan de sjabloon bekend configuratie Resources toewijzen, moeten deze ook de algemene bronnen en configuratie gevonden in de sjabloon met gedeelde bronnen en mogelijk die in optionele Resource sjablonen.

Als u wilt uw sjabloon publiceren naar de marketplace, u een afzonderlijk exemplaar van de Main-sjabloon die wordt vervangen door de eerder beschikbaar inkomende parameter van tshirtSize aan een variabele die is ingesloten in de sjabloon tot stand brengen.

![Marketplace](./media/best-practices-resource-manager-design-templates/marketplace.png)

**Aanpassing van een oplossing binnen het bereik van de sjabloon voor de marketplace**

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het delen van de status van en naar sjablonen, [delen status in Azure Resource Manager-sjablonen](best-practices-resource-manager-state.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

