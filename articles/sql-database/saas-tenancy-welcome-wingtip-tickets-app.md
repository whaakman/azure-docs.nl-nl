---
title: Welkom bij Wingtips app - Azure SQL Database | Microsoft Docs
description: Meer informatie over database tenancymodus modellen en over de Wingtips SaaS voorbeeldtoepassing voor Azure SQL Database in de cloudomgeving.
keywords: zelfstudie sql-database
services: sql-database
author: billgib
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/17/2017
ms.author: billgib
ms.openlocfilehash: bb69a03333ed9dcdba1456d053c0080be5ba4d10
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="the-wingtip-tickets-saas-application"></a>De Wingtip Tickets SaaS-toepassing

Dezelfde *Wingtip Tickets* SaaS-toepassing is geïmplementeerd in elk van de drie steekproeven. De app is een eenvoudige gebeurtenis aanbieden en tickets SaaS-app die gericht is op kleine plaatsen - theaters, clubs, enzovoort. Elke locatie vast is een tenant van de app en de eigen gegevens: u wilt gegevens, een lijst met gebeurtenissen, klanten, ticket orders, enzovoort.  De app, samen met de scripts en beheer zelfstudies gepresenteerd een end-to-end SaaS-scenario. Dit omvat inrichting tenants, controleren en beheren van prestaties, het Schemabeheer van het en cross-tenant rapportage en analyse.

## <a name="three-saas-application-and-tenancy-patterns"></a>Drie SaaS-toepassing en tenancymodus patronen

Er zijn drie versies van de app beschikbaar; elk behandelt een andere database tenancymodus patroon in Azure SQL-Database.  De eerste maakt gebruik van een zelfstandige toepassing per tenant met een eigen database. De tweede maakt gebruik van een multitenant-app met een database per tenant. Het derde voorbeeld gebruikt een multitenant-app met shard multitenant-databases.

![Drie tenancymodus patronen][image-three-tenancy-patterns]

 Elk voorbeeld bevat de toepassingscode, plus scripts en beheer zelfstudies waarin een bereik van de ontwerp- en patronen te verkennen.  Elke steekproef wordt geïmplementeerd in minder die vijf minuten.  Alle drie mag geïmplementeerde side-by-side zodat u kunt de verschillen in ontwerp en beheer vergelijken.

## <a name="standalone-application-per-tenant-pattern"></a>Zelfstandige toepassing per tenant patroon

De zelfstandige app per tenant patroon gebruikt een toepassing voor één tenant met een database voor elke tenant. Elke tenant-app, met inbegrip van de database wordt geïmplementeerd in een afzonderlijke Azure-resourcegroep. De resourcegroep kan worden geïmplementeerd in de serviceprovider abonnement of de tenant-abonnement en beheerd door de provider namens de tenant. De zelfstandige app per tenant patroon biedt de grootste isolatie van tenants, maar is meestal de meest dure omdat er geen mogelijkheid om resources tussen meerdere tenants te delen.  Dit patroon is zeer geschikt voor toepassingen die mogelijk meer complexe en die zijn geïmplementeerd naar een kleiner aantal tenants.  Met zelfstandige implementaties, de app kan worden aangepast voor elke tenant eenvoudiger dan in andere patronen.  

Bekijk de [zelfstudies] [ docs-tutorials-for-wingtip-sa] en code op GitHub [.../Microsoft/WingtipTicketsSaaS-StandaloneApp][github-code-for-wingtip-sa].

## <a name="database-per-tenant-pattern"></a>Database per tenant patroon

De database per tenant patroon is geschikt voor serviceproviders die betrokken zijn bij isolatie van tenants en wilt uitvoeren van een gecentraliseerde service waarmee u rendabele gebruik van gedeelde resources. Een database wordt gemaakt voor elke wetten of tenant en alle databases worden centraal beheerd. Databases kunnen worden gehost in elastische pools te bieden een betaalbare en eenvoudig prestatiebeheer die gebruikmaakt van de patronen onvoorspelbare werkbelasting van de tenants. Een catalogusdatabase bevat de toewijzing tussen tenants en hun databases. Deze toewijzing wordt beheerd met behulp van de beheerfuncties van de shard-kaart van de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md), waarmee u op efficiënte Verbindingsbeheer naar de toepassing.

Bekijk de [zelfstudies] [ docs-tutorials-for-wingtip-dpt] en code op GitHub [.../Microsoft/WingtipTicketsSaaS-DbPerTenant][github-code-for-wingtip-dpt].

## <a name="sharded-multi-tenant-database-pattern"></a>Patroon shard multitenant-database

Multitenant databases zijn geschikt voor lagere kosten per tenant en OK met verminderde tenantisolatie zoekt serviceproviders. Dit patroon kan grote aantallen tenants verpakken in een individuele database, waardoor de kosten per tenant. Near oneindige schalen door sharding mogelijk is de tenants tussen meerdere databases. Een catalogusdatabase toegewezen tenants aan databases.  

Dit patroon kan ook een *hybride* model waarin u kunt optimaliseren voor kosten met meerdere tenants in een database of optimaliseren voor isolatie met een enkele tenant in hun eigen database. De keuze kan worden gemaakt op basis van de tenant door tenant ofwel wanneer de tenant ingericht of hoger, zonder impact op de toepassing is.  Dit model kan effectief worden gebruikt wanneer groepen van tenants moeten anders worden behandeld. Lage kosten tenants kunnen bijvoorbeeld worden toegewezen aan de gedeelde databases, terwijl premium tenants kunnen worden toegewezen aan hun eigen databases. 

Bekijk de [zelfstudies] [ docs-tutorials-for-wingtip-mt] en code op GitHub [.../Microsoft/WingtipTicketsSaaS-MultiTenantDb][github-code-for-wingtip-mt].

## <a name="next-steps"></a>Volgende stappen

#### <a name="conceptual-descriptions"></a>Conceptuele beschrijvingen

- Een meer gedetailleerde uitleg van de toepassing tenancymodus patronen is beschikbaar op [multitenant SaaS-database tenancymodus patronen][saas-tenancy-app-design-patterns-md]

#### <a name="tutorials-and-code"></a>Zelfstudies en code

- Zelfstandige app per tenant:
    - [Zelfstudies voor zelfstandige app ] [ docs-tutorials-for-wingtip-sa].
    - [Code voor zelfstandige app op GitHub][github-code-for-wingtip-sa].

- Database per tenant:
    - [Zelfstudies voor database per tenant][docs-tutorials-for-wingtip-dpt].
    - [Code voor de database per tenant op GitHub][github-code-for-wingtip-dpt].

- Shard multitenant:
    - [Zelfstudies voor shard multitenant][docs-tutorials-for-wingtip-mt].
    - [Code voor de shard multitenant op GitHub][github-code-for-wingtip-mt].



<!-- Image references. -->

[image-three-tenancy-patterns]: media/saas-tenancy-welcome-wingtip-tickets-app/three-tenancy-patterns.png "Drie tenancymodus patronen."

<!-- Docs.ms.com references. -->

[saas-tenancy-app-design-patterns-md]: saas-tenancy-app-design-patterns.md

<!-- WWWeb http references. -->

[docs-tutorials-for-wingtip-sa]: https://aka.ms/wingtipticketssaas-sa
[github-code-for-wingtip-sa]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp

[docs-tutorials-for-wingtip-dpt]: https://aka.ms/wingtipticketssaas-dpt
[github-code-for-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant

[docs-tutorials-for-wingtip-mt]: https://aka.ms/wingtipticketssaas-mt
[github-code-for-wingtip-mt]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb

