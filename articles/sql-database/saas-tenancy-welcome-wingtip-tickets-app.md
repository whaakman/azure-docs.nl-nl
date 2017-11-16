---
title: Welkom bij Wingtips app - Azure SQL Database | Microsoft Docs
description: Meer informatie over database tenancymodus modellen en over de Wingtips SaaS voorbeeldtoepassing voor Azure SQL Database in de cloudomgeving.
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: billgib
manager: craigg
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: billgib;genemi
ms.openlocfilehash: 96e031835905057a9ab2b3ee4023b08de092dd8e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="welcome-to-the-wingtip-tickets-sample-saas-azure-sql-database-tenancy-app"></a>Welkom bij de Wingtip Tickets SaaS Azure SQL Database tenancymodus voorbeeldapp

Welkom bij de Wingtip Tickets SaaS Azure SQL Database tenancymodus voorbeeldtoepassing en de zelfstudies. Database-tenancymodus verwijst naar de modus voor isolatie waarmee uw app aan uw clients die worden gehost in uw toepassing betalen. Om te vereenvoudigen via momenteel, heeft elke client een hele database aan zichzelf, of deze een database deelt met andere client.

## <a name="wingtip-tickets-app"></a>Wingtip Tickets app

De voorbeeldtoepassing Wingtip Tickets worden de effecten van verschillende databasemodellen tenancymodus op het ontwerp en het beheer van multitenant SaaS-toepassingen. De bijbehorende zelfstudies beschrijven rechtstreeks die dezelfde effecten. Wingtip Tickets is gebouwd op Azure SQL Database.

Wingtip Tickets is ontworpen voor verschillende ontwerp- en scenario's die worden gebruikt door de werkelijke SaaS-clients verwerken. Het patroon van gebruik die ontstaan worden in Wingtip Tickets verwerkt.

U kunt de app Wingtip Tickets installeren in uw eigen Azure-abonnement in vijf minuten. De installatie omvat het invoegen van voorbeeldgegevens voor meerdere tenants. U kunt de toepassing en het van beheerscripts voor alle modellen veilig installeren omdat de installaties communiceren of niet met elkaar beïnvloeden.

#### <a name="code-in-github"></a>De code in Github

Toepassingscode en het management-scripts, zijn allemaal beschikbaar op GitHub:

- **Zelfstandige app** model: [WingtipTicketsSaaS StandaloneApp opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
- **Database per tenant** model: [WingtipTicketsSaaS DbPerTenant opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
- **Shard multitenant** model: [WingtipTicketsSaaS MultiTenantDB opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB).

Dezelfde één codebasis voor de app Wingtip Tickets opnieuw wordt gebruikt voor alle voorgaande modellen die worden vermeld. De code van Github kunt u uw eigen SaaS-projecten starten.



## <a name="major-database-tenancy-models"></a>Primaire database tenancymodus modellen

Wingtip Tickets is een gebeurtenis aanbieden en tickets SaaS-toepassing. Wingtip biedt services die worden door plaatsen vereist. De volgende items zijn van toepassing op elke locatie vast:

- Betaalt u worden gehost in uw toepassing.
- Is een *tenant* in Wingtip.
- Gebeurtenissen van de hosts. De volgende gebeurtenissen zijn betrokken:
    - Ticket prijzen.
    - Ticket verkopen.
    - Klanten die kaartjes kopen.

De app, samen met de scripts en beheer zelfstudies, een volledige SaaS-scenario gepresenteerd. Het scenario omvat de volgende activiteiten:

- Het inrichten van tenants.
- Bewaken en beheren van prestaties.
- Schemabeheer van het.
- Cross-tenant, rapportage en analyse.

Alle deze activiteiten zijn beschikbaar op elke schaal nodig is.



## <a name="code-samples-for-each-tenancy-model"></a>Codevoorbeelden voor elk model tenancymodus

Een set van toepassingsmodellen worden benadrukt. Andere implementaties kunnen echter combinatie van elementen van twee of meer modellen.

#### <a name="standalone-app-model"></a>Zelfstandige app-model

![Zelfstandige app-model][standalone-app-model-62s]

Dit model maakt gebruik van een toepassing voor één tenant. Daarom dit model moet slechts één database en gegevens worden opgeslagen voor alleen de één tenant. De tenant leuk vindt volledig geïsoleerd van andere tenants in de database.

U kunt dit model gebruiken wanneer u exemplaren van uw app aan veel verschillende clients voor elke client worden uitgevoerd op een eigen verkopen. De client is de enige tenant. Terwijl de database voor slechts één client-gegevens opslaat, worden gegevens in de database opgeslagen voor veel klanten van de client.

#### <a name="database-per-tenant"></a>Database per tenant

![Database per tenant model][database-per-tenant-model-35d]

Dit model heeft meerdere tenants in het exemplaar van de toepassing. Nog voor elke nieuwe tenant is een andere database toegewezen voor gebruik door de nieuwe tenant.

Dit model biedt volledige databaseback-isolatie voor elke tenant. De Azure SQL Database-service heeft de verfijning waarmee dit model aannemelijke.

- [Inleiding tot een voorbeeld van een SQL-Database multitenant SaaS-app] [ saas-dbpertenant-wingtip-app-overview-15d] -bevat meer informatie over dit model.

#### <a name="sharded-multi-tenant-databases-the-hybrid"></a>Shard multitenant-databases en het hybride

![Shard multitenant databasemodel, de hybride][sharded-multitenantdb-model-hybrid-79m]

Dit model heeft meerdere tenants in het exemplaar van de toepassing. Dit model heeft ook meerdere tenants in enkele of alle van de databases. Dit model is geschikt is voor het aanbieden van verschillende Servicelagen zodat clients meer betalen kunnen als de waarde volledige isolatie van de database.

Het schema van elke database bevat een tenant-id. De tenant-id is zelfs in die databases waarin slechts één tenant.

- [Inleiding tot een voorbeeld van een SQL-Database multitenant SaaS-app][saas-multitenantdb-get-started-deploy-89i]



## <a name="tutorials-for-each-tenancy-model"></a>Zelfstudies voor elk model tenancymodus

Elk model tenancymodus is gedocumenteerd door het volgende:

- Een set van zelfstudie artikelen.
- Broncode is opgeslagen in een Github-opslagplaats die is aan het model toegewezen:
    - De code voor de toepassing Wingtip Tickets.
    - De scriptcode voor scenario's voor beheer.

#### <a name="tutorials-for-management-scenarios"></a>Zelfstudies voor scenario's voor beheer

De zelfstudie artikelen voor elk model ingegaan op de volgende scenario's voor beheer:

- Tenant-inrichting.
- Bewaking van toepassingsprestaties en beheer.
- Schemabeheer van het.
- Cross-tenant, rapportage en analyse.
- Herstel van een tenant naar een eerder tijdstip.
- Herstel na noodgevallen.



## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot een voorbeeld van een SQL-Database multitenant SaaS-app] [ saas-dbpertenant-wingtip-app-overview-15d] -bevat meer informatie over dit model.

- [Multitenant SaaS-database tenancymodus patronen][multi-tenant-saas-database-tenancy-patterns-60p]



<!-- Image references. -->

[standalone-app-model-62s]: media/saas-tenancy-welcome-wingtip-tickets-app/model-standalone-app.png "Zelfstandige app-model"

[database-per-tenant-model-35d]: media/saas-tenancy-welcome-wingtip-tickets-app/model-database-per-tenant.png "Database per tenant model"

[sharded-multitenantdb-model-hybrid-79m]: media/saas-tenancy-welcome-wingtip-tickets-app/model-sharded-multitenantdb-hybrid.png "Shard multitenant databasemodel, de hybride"



<!-- Article references. -->

[saas-dbpertenant-wingtip-app-overview-15d]: saas-dbpertenant-wingtip-app-overview.md

[multi-tenant-saas-database-tenancy-patterns-60p]: saas-tenancy-app-design-patterns.md

[saas-multitenantdb-get-started-deploy-89i]: saas-multitenantdb-get-started-deploy.md


