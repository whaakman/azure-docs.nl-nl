---
title: Azure SQL Database Azure casestudy - Snelstart | Microsoft Docs
description: Meer informatie over hoe SnelStart SQL-Database gebruikt om snel uitgevouwen de zakelijke services met een frequentie van 1000 nieuwe Azure SQL-Databases per maand
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: fab506b2-439d-4f1a-bdc5-d1d25c80d267
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Inactive
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 4fa21cf9cbd1680ddd855189f50af50e1068ccd5
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Met Azure, is SnelStart snel de zakelijke services met een frequentie van 1000 nieuwe Azure SQL-Databases per maand uitgebreid
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

SnelStart maakt populaire financiële - en business-beheersoftware voor kleine en middelgrote bedrijven (SMB's) in de Nederlandse. 55,000 klanten worden afgehandeld door een personeel van 110 werknemers, met inbegrip van een IT-personeel van 35. Door het verplaatsen van bureaublad software naar een software-as-a-service (SaaS) bieden gebouwd op Azure, gedaan SnelStart de meeste van ingebouwde services automatiseren met behulp van de vertrouwde omgeving in C# en optimaliseren van prestaties en schaalbaarheid door geen van beide bedrijven boven of onder-inrichten met elastische pools-beheer. Het gebruik van Azure biedt SnelStart de soepele manier om met bewegende klanten tussen on-premises en de cloud.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-the-desktop-to-the-cloud"></a>Waarom SnelStart services uitgebreid vanaf het bureaublad naar de cloud
> "Werken met Azure betekent we kunt leveren software sneller, snel reageren op verzoeken van klanten en schalen van oplossingen wanneer aanvragen worden verhoogd."
> 
> — Henry is, Software Architect
> 
> 

SnelStart uitgevoerd een geslaagde software-bedrijven jaar, met een model traditionele ontwikkeling: code, van het pakket worden geleverd en Herhaal. Na verloop van tijd de snelheid van wijziging sneller klein en sneller. Voorschriften vaak worden gewijzigd en klanten nodig gemakkelijker financiële gegevens verwerken en samenwerken met hun accountants en government die wijzigingen bijhouden.

'Software van de back-upfunctie voor klanten is kostbaar en beperkende,' volgens Henry is, software architect. 'Productie, pakketten en kosten van de back-upfunctie voor beperkte hoe vaak kan brengen we software. Er was een pakket updates voor de periodieke verzendingen, maar die was het moeilijk te voldoen aan veranderende behoeften van onze klanten. Er kan nooit worden gewaarborgd die onze klanten naar de nieuwste versie van het product bijgewerkt. Daarom hebben we moeten ondersteuning voor meerdere versies, waardoor de taak ondersteuning moeilijk te en."

Is toegevoegd, ' wilden We een manier om het programma en release-updates op een versnelde tempo, zodat we kan sneller innoveren en maken van nieuwe services voor onze klanten. Ook wilden we een manier om meer processen automatiseren om het beheer van zakelijke behoeften van onze klanten te vereenvoudigen."

Voor SnelStart is de oplossing uit te breiden de services door een cloud-gebaseerde SaaS-provider. De Azure SQL Database-platform heeft geholpen SnelStart daar zonder de belangrijke IT-overhead die een oplossing voor infrastructuur-as-a-service (IaaS) zijn vereist.

Het cloudmodel kan ook SnelStart fouten corrigeren en nieuwe functies snel, zonder klanten hoeven te downloaden en software bijwerken. Volgens is, 'Using Azure cloudservices kunnen wij snel reageren op het wijzigen van de vereisten van een derde partij. In plaats van een nieuwe versie verzenden naar duizenden klanten, kunt we aanpassen informatie verzonden van onze bureaubladtoepassing naar nieuwe indelingen vereist door derden."

## <a name="a-modern-company-with-traditional-roots"></a>Een moderne bedrijf met traditionele hoofdmappen
SnelStart is een moderne, flexibele, hightech bedrijf met eenvoudig hoofdmappen tot naar 1964, wanneer de grondleggers het bedrijf als een fabrikant van muziek instrument delen gestart. De kern van het bedrijf van de software SnelStart gestart echt kloppen in de race in de jaren 1980, tijdens de komst van de personal computer. Het bedrijf nodig een beter alternatief voor het beschikbaar is op het moment boekhoudsoftware dus belangrijk is in een eigen handen nodig was. In 1982, worden in het bedrijf de basis van wat uiteindelijk SnelStart boekhoudsoftware zou worden gemaakt. De software is vanaf het begin admired voor de bijbehorende eenvoud en snelheid, dus veel zodat het bedrijf uiteindelijk focus wordt gewijzigd en reinvented zelf in een softwarebedrijf.

In 1995 uitgebracht SnelStart de eerste boekhouding-toepassing voor Windows. De toepassing, gebaseerd op Microsoft Visual Basic 1.0 en Microsoft Access-databases, kon de klant base groeien aan meer dan 5000 gebruikers.

Vandaag de dag is SnelStart een belangrijke provider van een line-of-business (LOB) en bedrijven beheertoepassing gericht op Nederlandse SMB's en zelfstandige ondernemers. Als Carlo Kuip, IT-architecten staat, is"ons doel voor automatisering van zakelijke beheerservices voor onze klanten 100 procent."

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Optimalisatie van prestaties en kosten met elastische pools
SnelStart is een grootschalige eerste gebruiker van elastische pools. Elastische pools helpen bij het bedrijf kosten beperken en prestatie-eisen efficiënter beheren. Volgens is, 'met elastische pools kunnen we kunt optimaliseren op basis van de behoeften van onze klanten, zonder overprovisioning. Als we hebben gehad om in te richten op basis van piekbelasting, zou deze behoorlijk kostbaar zijn. In plaats daarvan de optie voor het delen van bronnen tussen meerdere, lage gebruik databases kan worden gemaakt van een oplossing die goed werkt en betaalbaar. "

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Azure SQL-Databases te containerize-gegevens voor de isolatie en beveiligingsvereisten
Azure SQL Database kunt SnelStart eenvoudig en transparant verplaatsen klanten on-premises beheer van zakelijke gegevens naar Azure. De Azure SQL Database is een handige container biedt isolatie, een grens voor verificatie, autorisatie en eenvoudig back-up en herstel mogelijkheden. Databases bieden een conceptuele model dat is geschikt voor bedrijven-beheer. Volgens Carlo Kuip, IT-architecten 'artikelen in deze container grens gevoelige gegevens bevatten die is van cruciaal belang om een bedrijf en opslaan van de items in een geïsoleerde-database, worden deze beveiligd. We kunnen machtigingen op het databaseniveau van de beheren en zelfs de beheer- en scale-out voor deze databases automatiseren zonder databasebeheerders (DBA's) dat in dienst."

Azure SQL Data Warehouse speelt een rol ook in het artikel van beveiliging en beheer SnelStart door het bedrijf verzamelen van telemetriegegevens, zoals inbraakdetectie, gebruiker activiteitenregistratie en connectiviteit.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure verwijderen overhead zodat ontwikkelaars kunnen meer tijd voor het leveren van waarde besteden
Het Azure-platform-model verwijderd infrastructuur overhead en SnelStart voor het automatiseren van implementaties met C#-managementbibliotheken ingeschakeld. Als Kuip statussen, kunnen' We onze huidige bewerkingen met een zeer kleine personeel groeien terwijl tegelijkertijd de schaalbaarheid, de snelheid en noodherstel herstelopties voor onze klanten. De verschuiving naar services ontwikkeling vrijgemaakt resources gericht op nieuwe services en functies in plaats van alleen bijwerken van bestaande code te houden met nieuwe regelgeving of btw-codes." Hij voegt 'door te automatiseren beheer en de aanbieden, er zijn meer waarde voor onze klanten leveren zonder voor grote investeringen in de operationele medewerkers SaaS." Bijvoorbeeld, met behulp van Azure en elastische pools kon SnelStart toevoegen tal van nieuwe functies, krachtiger klantgegevens integratie met banken, waaronder nieuwe facturering services, achtergrondcontroles voor kleine bedrijven en e-services.

> 'In alleen de eerste paar maanden 2016 we onze Azure SQL Database-implementaties van ongeveer 5,500 naar meer dan 12.000 uitgebreid en we momenteel ongeveer 1000 databases per maand toevoegt.'
> 
> — Henry is, Software Architect
> 
> 

Database-beheer is verder met de functie elastische taken geautomatiseerd. Als Kuip, raken Bedankt' maximaal voor de automatische detectie van databases in een [server]-exemplaar van SQL-database." Hierdoor SnelStart beheerbewerkingen uitvoeren via hun dynamisch groeiende klanten zonder extra overhead.

SnelStart is ook een API die als broker tussen klantgegevens en apps gebouwd door partners van de software van derden fungeert ontwikkelen. Als Kuip statussen, wordt 'deze API inschakelen functionaliteit toevoegen aan onze software, zoals de gegevensinvoer voor facturen en andere documenten elimineren van andere leveranciers.' Klanten moeten niet langer handmatig facturen typt in hun boekhoudsoftware kleine bedrijven; de software SnelStart exchange de benodigde informatie rechtstreeks. Zodoende kunnen klanten hun zakelijke beheertaken met het ecosysteem van informatie die wordt bepaald door digitale transformatie in de branche te koppelen.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Hoe Azure-services SaaS voor SnelStart inschakelen
Met behulp van Azure kan SnelStart fungeren zijn klanten en hun accountants naadloze in de cloud. Bijvoorbeeld, kunnen klanten en accountants gegevens delen door rechtstreeks toegang hebben tot SnelStart van client-API, gehost in Azure. Kuip statussen, 'Deze herbruikbare services beschikbaar zijn voor onze klantgerichte web-apps en bieden gemeenschappelijke infrastructuur en -functies waarmee toegang tot zakelijke beheer voor klanten en software van derden die worden gebruikt door onze klanten. Voorbeelden zijn onder meer het bieden van configuratie van het product, het beheren van firewallregels en het beheren van langlopende processen zoals back-ups."

> Ons doel is om te bieden van 100 procent automatisering van zakelijke beheerservices voor onze klanten." 
> 
> — Carlo Kuip, IT-architecten
> 
> 

Bovendien toestaan SnelStart webservices klanten en accountants eenvoudig toegang tot gegevens in Azure SQL Database elastische pools. Dit model SaaS, samen met de database elasticiteit en Azure Resource Manager biedt SnelStart functies voor schaalbaarheid aanvulling op elke Azure-implementatie. De uitvoering is volledig geautomatiseerd met C# management-bibliotheken.

![SnelStart-architectuur](./media/sql-database-implementation-snelstart/figure1.png)

Afbeelding 1. Vanaf juni 2016 heeft SnelStart meer dan 11.000 databases en meer dan 50 elastische pools

## <a name="simplicity-from-the-cloud"></a>Eenvoud vanuit de cloud
Sinds verplaatst naar een Azure-cloud-gebaseerde oplossing, is SnelStart kunnen klant snelle groei en biedt tegelijkertijd innovatieve functies en services ondersteunen. Volgens is, 'met Azure, we updates zijn in de buurt van continue voor onze klanten zonder uit te vouwen ons personeel bewerkingen. En krijgt u alle andere fantastische Azure functies, zoals schaalbaarheid en herstel na noodgevallen: gebundeld. "

> 'Wanneer we werden daadwerkelijk via in Redmond... Ik heb ontvangen een aanroep van een ontwikkelaar terug in de Nederlandse mij over een bepaald probleem te. We kunnen ophalen van Microsoft voor het leveren van een wijziging in de productieomgeving binnen 48 uur om het probleem te verhelpen."
> 
> — Henry is, Software Architect
> 
> 

SnelStart waardeert ook de sterke samenwerking die ze hebt ontwikkeld met het team van Microsoft Azure SQL-database. Als Kuip statussen, 'hebben we discussies over functies en het gebruik van de technologie die iets is aan beide zijden gewaardeerd.'
De onmiddellijke doel voor SnelStart is de klant tevreden base blijven toenemen. Zoals is aangegeven, 'zonder de technische en resource beperkingen die we als een ISV had, er is geen limiet voor hoe ver we kunnen groeien. "

## <a name="more-information"></a>Meer informatie
* Zie voor meer informatie over Azure elastische pools, [elastische pools](sql-database-elastic-pool.md).
* Zie voor meer informatie over webrollen en werkrollen, [werkrollen](../fundamentals-introduction-to-azure.md#compute).    
* Zie voor meer informatie over Azure SQL Data Warehouse, [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* Zie voor meer informatie over SnelStart, [SnelStart](http://www.snelstart.nl).

