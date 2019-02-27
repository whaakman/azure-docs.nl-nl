---
title: Welkom bij Wingtips app - Azure SQL Database | Microsoft Docs
description: Meer informatie over database tenants modellen en het voorbeeld Wingtips SaaS-toepassing voor Azure SQL Database in de cloudomgeving.
keywords: zelfstudie sql-database
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: a21b9f45d70151c2a2c2ae8ac6328a3eeb609b29
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883331"
---
# <a name="the-wingtip-tickets-saas-application"></a>De Wingtip Tickets SaaS-toepassing

Dezelfde *Wingtip Tickets* SaaS-toepassing is geïmplementeerd in elk van de drie voorbeelden. De app is een eenvoudige gebeurtenis aanbieding en ticketing SaaS-app die gericht is op kleine venues - theaters, stadions, enzovoort. Elke venue is een tenant van de app en de eigen gegevens: venue details, een lijst met gebeurtenissen, klanten, ticket orders, enzovoort.  De app, samen met de management scripts en zelfstudies, brengt een end-to-end SaaS-scenario. Dit omvat inrichting tenants, controleren en beheren van prestaties, Schemabeheer, en cross-tenant-rapportage en analyse.

## <a name="three-saas-application-and-tenancy-patterns"></a>Drie beheerpatronen voor SaaS-toepassing en tenants

Er zijn drie versies van de app beschikbaar. elk gaat een andere database tenants patroon in op Azure SQL Database.  De eerste maakt gebruik van een zelfstandige toepassing per tenant met een eigen database. De tweede maakt gebruik van een app met meerdere tenants met een database per tenant. Het derde voorbeeld wordt een app met meerdere tenants met shard multitenant-databases.

![Drie tenants patronen][image-three-tenancy-patterns]

 Elk voorbeeld bevat de toepassingscode, plus management scripts en zelfstudies die een reeks ontwerp- en -patronen Verken.  Elk voorbeeld wordt geïmplementeerd in minder die vijf minuten.  Alle drie mag geïmplementeerde side-by-side zodat u de verschillen in ontwerp en beheer kunt vergelijken.

## <a name="standalone-application-per-tenant-pattern"></a>Zelfstandige toepassing per tenant-patroon

De zelfstandige app per tenant patroon maakt gebruik van een toepassing met één tenant met een database voor elke tenant. Van elke tenant-app, met inbegrip van de database ervan, wordt geïmplementeerd in een afzonderlijke Azure-resourcegroep. De resourcegroep kan worden geïmplementeerd in het abonnement van de serviceprovider of van de tenant-abonnement en beheerd door de provider voor andere gebruikers van de tenant. De zelfstandige app per tenant patroon biedt de grootste isolatie van tenants, maar is doorgaans het meest duur omdat er geen mogelijkheid om resources tussen meerdere tenants te delen.  Dit patroon is zeer geschikt voor toepassingen die mogelijk meer complexe en die zijn geïmplementeerd naar een kleiner aantal tenants.  Bij implementaties op een zelfstandige de app kan worden aangepast voor elke tenant eenvoudiger dan in andere patronen.  

Bekijk de [zelfstudies] [ docs-tutorials-for-wingtip-sa] en de code op GitHub [.../Microsoft/WingtipTicketsSaaS-StandaloneApp][github-code-for-wingtip-sa].

## <a name="database-per-tenant-pattern"></a>Database per tenant-patroon

De database per tenant patroon is met ingang van serviceproviders die betrokken zijn bij de isolatie van tenants en wilt een gecentraliseerde service waarmee u rendabele gebruik van gedeelde resources uit te voeren. Een database wordt gemaakt voor elke locatie, of de tenant en alle databases worden centraal beheerd. Databases kunnen worden gehost in elastische pools voor rendabele en eenvoudig prestatiebeheer, die gebruikmaakt van de patronen onvoorspelbare werkbelasting van de tenants. Een catalogusdatabase bevat de toewijzing tussen tenants en de bijbehorende databases. Deze toewijzing wordt beheerd met behulp van de beheerfuncties van de shard-kaart van de [elastische Database-clientbibliotheek](sql-database-elastic-database-client-library.md), waarmee u efficiënt beheer van de toepassing.

Bekijk de [zelfstudies] [ docs-tutorials-for-wingtip-dpt] en de code op GitHub [.../Microsoft/WingtipTicketsSaaS-DbPerTenant][github-code-for-wingtip-dpt].

## <a name="sharded-multi-tenant-database-pattern"></a>Patroon shard multitenant-database

Databases van meerdere tenants zijn met ingang van serviceproviders lagere kosten per tenant en oké met verminderde tenantisolatie op zoek. Dit patroon maakt het mogelijk grote aantallen tenants verpakken in een individuele database, waardoor de kosten-per-tenant. Bijna oneindige schaalbaarheid mogelijk is met sharding is de tenants voor meerdere databases. Een catalogusdatabase toegewezen tenants aan databases.  

Dit patroon kan ook een *hybride* model waarin u kunt optimaliseren voor kosten met meerdere tenants in een database of optimaliseren voor isolatie met één tenant in hun eigen database. De keuze kan worden gemaakt op basis van het tenant-per-tenant, hetzij als de tenant ingericht of hoger, zonder gevolgen voor de toepassing is.  Dit model kan effectief worden gebruikt wanneer groepen van tenants moeten anders worden behandeld. Lage kosten tenants kunnen bijvoorbeeld worden toegewezen aan gedeelde databases, terwijl premium-tenants kunnen worden toegewezen aan hun eigen databases. 

Bekijk de [zelfstudies] [ docs-tutorials-for-wingtip-mt] en de code op GitHub [.../Microsoft/WingtipTicketsSaaS-MultiTenantDb][github-code-for-wingtip-mt].

## <a name="next-steps"></a>Volgende stappen

#### <a name="conceptual-descriptions"></a>Conceptuele beschrijvingen

- Een meer gedetailleerde uitleg van de tenants toepassingspatronen is beschikbaar op [multitenant SaaS-patronen voor tenants database][saas-tenancy-app-design-patterns-md]

#### <a name="tutorials-and-code"></a>Zelfstudies en code

- Zelfstandige app per tenant:
    - [Zelfstudies voor zelfstandige app][docs-tutorials-for-wingtip-sa].
    - [Code voor zelfstandige app op GitHub][github-code-for-wingtip-sa].

- Database per tenant:
    - [Zelfstudies voor database per tenant][docs-tutorials-for-wingtip-dpt].
    - [Code voor de database per tenant op GitHub][github-code-for-wingtip-dpt].

- Een shard met meerdere tenants:
    - [Zelfstudies voor multitenant shard][docs-tutorials-for-wingtip-mt].
    - [Code voor shard met meerdere tenants, op GitHub][github-code-for-wingtip-mt].



<!-- Image references. -->

[image-three-tenancy-patterns]: media/saas-tenancy-welcome-wingtip-tickets-app/three-tenancy-patterns.png "Drie tenants patronen."

<!-- Docs.ms.com references. -->

[saas-tenancy-app-design-patterns-md]: saas-tenancy-app-design-patterns.md

<!-- WWWeb http references. -->

[docs-tutorials-for-wingtip-sa]: https://aka.ms/wingtipticketssaas-sa
[github-code-for-wingtip-sa]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp

[docs-tutorials-for-wingtip-dpt]: https://aka.ms/wingtipticketssaas-dpt
[github-code-for-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant

[docs-tutorials-for-wingtip-mt]: https://aka.ms/wingtipticketssaas-mt
[github-code-for-wingtip-mt]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb

