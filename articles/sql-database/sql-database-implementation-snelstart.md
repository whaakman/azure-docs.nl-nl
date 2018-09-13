---
title: Azure casestudy - Bedrijfsservices voor Azure SQL Database | Microsoft Docs
description: Meer informatie over hoe Bedrijfsservices gebruikmaakt van SQL Database uitgebreid snel zijn Bedrijfsservices tegen een tarief van 1000 nieuwe Azure SQL-databases per maand
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: c919a3cb47017d2f65b141e358ab318f4b764627
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713711"
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Met Azure, zijn Bedrijfsservices snel zijn Bedrijfsservices tegen een tarief van 1000 nieuwe Azure SQL-databases per maand uitgebreid
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

Bedrijfsservices maakt populaire - en business-software voor financieel beheer voor kleine en middelgrote bedrijven (SMB's) in Nederland. De 55,000 klanten worden afgehandeld door medewerkers van 110 werknemers, met inbegrip van een IT-personeel van 35. Door te verplaatsen van bureaublad-software op een software-as-a-service (SaaS)-aanbieding die is gebouwd op Azure, Bedrijfsservices bestaat de meeste ingebouwde services automatiseren-beheer met behulp van vertrouwde omgeving in C#, en het optimaliseren van prestaties en schaalbaarheid door geen van beide boven - of onder inrichting bedrijven met behulp van elastische pools. Met behulp van Azure biedt Bedrijfsservices de doorstroming verplaatsen klanten tussen on-premises en de cloud.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-the-desktop-to-the-cloud"></a>Waarom Bedrijfsservices services uitgebreid vanaf het bureaublad naar de cloud
> "Werken met Azure betekent dat we kunt lever software sneller, snel reageren op verzoeken van klanten, en oplossingen te schalen wanneer de vraag toeneemt."
> 
> — Henry Been, Softwarearchitect
> 
> 

Bedrijfsservices is een geslaagde softwarebedrijf uitgevoerd jaar gebruikmaken van een traditionele ontwikkelings-model: code, verpakken, verzenden en herhalen. Na verloop van tijd, de snelheid van de wijziging is gegroeid sneller en sneller. Voorschriften regelmatig gewijzigd en klanten die nodig zijn eenvoudige manieren voor het verwerken van financiële verslagen en samenwerken met hun accountants en de overheid met deze wijzigingen bijhouden.

"Verzending software aan klanten was kostbare en beperkende," op basis van Henry Been, softwarearchitect. 'Productie, verpakking en verzendkosten beperkt hoe vaak er software kan vrijgeven. We hadden pakket updates voor de periodieke verzendingen, maar dat het moeilijk om te voldoen aan de wisselende behoeften van onze klanten gemaakt. We kunnen nooit er zeker van zijn dat onze klanten een upgrade uitgevoerd naar de nieuwste versie van het product. Daarom moesten we ondersteuning voor meerdere versies, waardoor de taak ondersteuning zeer moeilijk is om en."

Is toegevoegd, "We wilden een manier om programma- en release-updates op een versneld hoog tempo, zodat we kunnen eenvoudig sneller innoveren en maken van nieuwe services voor onze klanten. Maar we wilden ook een manier voor het automatiseren van processen om de behoeften van onze klanten zakelijke-beheer te vereenvoudigen."

Voor Bedrijfsservices, wordt de oplossing is om uit te breiden de services door een cloud-gebaseerde SaaS-provider. De Azure SQL Database-platform heeft geholpen Bedrijfsservices daar zonder de belangrijke IT-overhead die een oplossing voor infrastructure-as-a-service (IaaS) was vereist.

Het cloudmodel kunt ook Bedrijfsservices het corrigeren van fouten en nieuwe functies snel, zonder dat klanten geen hoeven te downloaden en software bijwerken. Volgens is, "met Azure cloudservices kunnen we snel reageren op het wijzigen van de vereisten van derde partijen. In plaats van dat voor het verzenden van een nieuwe versie naar duizenden klanten, we kunnen aan te passen informatie verzonden van onze bureaubladtoepassing voor nieuwe indelingen vereist door derde partijen."

## <a name="a-modern-company-with-traditional-roots"></a>Een moderne bedrijf met traditionele toegangspunten
Bedrijfsservices is een moderne, flexibele, hightech bedrijf met eenvoudige hoofdmappen tot aan 1964, wanneer de grondleggers van het bedrijf als een fabrikant van muziek betalingsmiddelen delen gestart. Het hart van het bedrijf van de software Bedrijfsservices gestart echt kloppen in de jaren 1980, in de race tijdens de toename van de personal computer. Het bedrijf nodig een beter alternatief voor de accounting software beschikbaar is op het moment, dus het heft in eigen handen heeft. In 1982, heeft het bedrijf de basis van wat uiteindelijk Bedrijfsservices boekhoudsoftware zou worden gemaakt. De software is vanaf het begin admired voor de eenvoud en snelheid, zoveel zijn zodat het bedrijf uiteindelijk focus wordt gewijzigd en zichzelf opnieuw in een softwarebedrijf uitgevonden.

Bedrijfsservices uitgebracht in 1995, de eerste boekhouding-toepassing voor Windows. De toepassing, die is gebouwd op Microsoft Visual Basic 1.0 en Microsoft Access-databases, geholpen bij de klant base uitbreiden tot meer dan 5000 gebruikers.

Vandaag de dag is Bedrijfsservices een belangrijke aanbieder van een line-of-business (LOB) en het beheer van business-toepassingen die zijn gericht op Nederlandse midden-en kleinbedrijf en zelfstandige ondernemers. Als Carlo Kuip, IT-architecten, zegt: "ons doel is voor automatisering van business-beheerservices voor onze klanten 100 procent."

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Optimaliseren van prestaties en kosten met elastische pools
Bedrijfsservices is een grootschalige van de eersten van elastische pools. Elastische pools helpen bij het bedrijf beperken kosten en prestatievereisten efficiënter te beheren. Volgens is, 'met behulp van elastische pools, we kunnen prestaties optimaliseren op basis van de behoeften van onze klanten, zonder de capaciteit in te richten. Als we hebben gehad om in te richten op basis van de piekbelasting, zou het behoorlijk kostbaar zijn. In plaats daarvan de optie voor het delen van resources tussen meerdere databases laag gebruik kunnen we een oplossing die goed werkt en is kosteneffectief te maken. "

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Azure SQL-databases kunnen gegevens voor isolatie en beveiliging in een container plaatsen
Azure SQL-Database kunnen Bedrijfsservices eenvoudig en transparant klanten on-premises beheer van zakelijke gegevens naar Azure verplaatsen. De Azure SQL Database is een handige container waarmee isolatie, een grens voor verificatie, autorisatie en eenvoudig back-up en herstel biedt. Databases bieden een zeer geschikt conceptuele model voor het beheer van business. Op basis van Carlo Kuip, IT-architecten, ' Items in deze container grens bevatten gevoelige gegevens die essentieel zijn voor een bedrijf en opslaan van de items in een geïsoleerde database deze goed beveiligd blijven. We kunnen machtigingen op databaseniveau beheren en automatiseren zelfs het beheer en de scale-out van deze databases zonder databasebeheerders (DBA) voor personeel."

Azure SQL Data Warehouse speelt een rol ook in het verhaal van beveiliging en beheer Bedrijfsservices door het bedrijf verzamelen van telemetriegegevens, zoals inbraakdetectie, activiteit gebruikersaanmelding en connectiviteit.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure verwijdert overhead zodat meer tijd waarde levert kunnen besteden aan ontwikkelaars
Het Azure-platform-model verwijderd infrastructuur overhead en Bedrijfsservices voor het automatiseren van implementaties met behulp van C#-beheerbibliotheken ingeschakeld. Als Kuip staat, konden"We onze huidige bewerkingen met een zeer kleine medewerkers groeien terwijl het tegelijkertijd verhogen schaalbaarheid, de snelheid en disaster recovery-opties voor onze klanten. De verschuiving naar services ontwikkelen vrijgemaakt resources zich kunt richten op nieuwe services en functies, in plaats van alleen bijwerken van de bestaande code te houden aan de nieuwe regelgeving of btw-codes." Hij wordt toegevoegd, 'door het automatiseren van beheer en met behulp van de SaaS-aanbieding, kan meer waarde leveren voor onze klanten, zonder dat u hoeft voor grote investeringen in operationele medewerkers.' Bijvoorbeeld, met behulp van Azure en elastische pools, is Bedrijfsservices in staat om toe te voegen tal van nieuwe functies, waaronder krachtiger klantgegevens integratie met banken, nieuwe facturering services, kleine bedrijven achtergrondonderzoek en e-mailservices.

> "In alleen de eerste paar maanden van 2016 we onze Azure SQL Database-implementaties van ongeveer 5,500 aan meer dan 12.000 uitgebreid en er ongeveer 1000 databases per maand op dit moment worden toegevoegd."
> 
> — Henry Been, Softwarearchitect
> 
> 

Database-beheer is verder worden geautomatiseerd met behulp van de functie voor elastische taken. Als Kuip staat, Bedankt"maximaal voor de automatische detectie van databases op een [server]-exemplaar van SQL-database." Hiermee wordt Bedrijfsservices voor het uitvoeren van beheerbewerkingen in hun dynamisch groeiende klantenbestand zonder extra overhead.

Bedrijfsservices is ook het ontwikkelen van een API die als een broker tussen gegevens van de klant en de apps die zijn gebouwd door partners van de software van derden fungeert. Als Kuip Staten kunnen' deze API functionaliteit toevoegen aan onze software, zoals het invoeren van gegevens voor facturen en andere documenten elimineren van andere leveranciers." Klanten moeten niet meer handmatig typen facturen in hun small business accounting software; de software Bedrijfsservices exchange de benodigde informatie rechtstreeks. Dit kan klanten hun zakelijke-beheertaken met het ecosysteem van informatie die wordt bepaald door de digitale transformatie in de branche join.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Hoe Azure-services SaaS voor Bedrijfsservices inschakelen
Bedrijfsservices kan met behulp van Azure, dienen klanten en hun accountants naadlozer in de cloud. Bijvoorbeeld, kunnen zowel klanten als accountants informatie delen door rechtstreeks toegang hebben tot Bedrijfsservices van client-API, die worden gehost op Azure. Kuip vermeld, "Deze herbruikbare services beschikbaar zijn voor onze klantgerichte web-apps en ze een gemeenschappelijke infrastructuur en functies waarmee toegang tot zakelijke beheer voor klanten en software van derden die worden gebruikt door onze klanten bieden. Voorbeelden zijn mogelijkheden voor product-configuratie, het beheer van firewallregels en beheren van langlopende processen zoals back-ups."

> Ons doel is om te bieden van 100 procent automatisering van business-beheerservices voor onze klanten." 
> 
> — Carlo Kuip, IT-architecten
> 
> 

Bedrijfsservices webservices kunnen bovendien zowel klanten als accountants eenvoudig toegang tot gegevens in Azure SQL Database elastische pools. Deze SaaS-model, in combinatie met de flexibiliteit van de database en Azure Resource Manager biedt Bedrijfsservices met functies voor schaalbaarheid die een aanvulling vormen op elke Azure-implementatie. De implementatie is volledig worden geautomatiseerd met C#-beheerbibliotheken.

![Architectuur van Bedrijfsservices](./media/sql-database-implementation-snelstart/figure1.png)

Afbeelding 1. Vanaf juni 2016 heeft Bedrijfsservices meer dan 11.000 databases en meer dan 50 elastische pools

## <a name="simplicity-from-the-cloud"></a>Eenvoud vanuit de cloud
Sinds verplaatsen naar een Azure-cloud-gebaseerde oplossing, bleek Bedrijfsservices in staat voor de ondersteuning van snelle klant groei terwijl innovatieve functies en services. Volgens is, "met Azure, we kunnen leveren in de buurt van de continue updates voor onze klanten, zonder het uitbreiden van onze operationele medewerkers. En we krijgen alle de andere fantastische Azure-functies, zoals schaalbaarheid en herstel na noodgevallen, gebundeld. "

> "Als we waren daadwerkelijk in Redmond... Ik heb ontvangen een gesprek van een ontwikkelaar terug in Nederland mij bellen over een specifiek probleem. We konden om op te halen van Microsoft voor het leveren van een wijziging in hun productieomgeving binnen 48 uur onze probleem op te lossen."
> 
> — Henry Been, Softwarearchitect
> 
> 

Bedrijfsservices waardeert ook de sterke samenwerking die ze hebt ontwikkeld met het team van Microsoft Azure SQL-database. Als Kuip statussen, "hebben we discussies over functies en het gebruik van technologie, iets wat gewaardeerd aan beide zijden."
De onmiddellijke doel voor Bedrijfsservices is om de tevreden klanten basis blijven toenemen. Zoals is vermeld, "Zonder de technische en resource-beperkingen die we hadden maar als een ISV, er is geen limiet voor hoe ver we kan groeien."

## <a name="more-information"></a>Meer informatie
* Zie voor meer informatie over Azure elastische pools, [elastische pools](sql-database-elastic-pool.md).
* Zie voor meer informatie over Azure SQL Data Warehouse, [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* Zie voor meer informatie over Bedrijfsservices, [Bedrijfsservices](http://www.snelstart.nl).

