---
title: "Video geïndexeerd, SaaS-SQL Azure-app | Microsoft Docs"
description: "In dit artikel indexeert verschillende tijdstippen in onze video over SaaS-DB-tenancymodus app ontwerp van de Ignite conferentie 11 oktober 2017 81 minuten. U kunt gaat u verder met het onderdeel dat u bent geïnteresseerd. Ten minste 3 patronen worden beschreven. Azure-functies die de ontwikkeling en het beheer vereenvoudigen worden beschreven."
ms.custom: 
ms.date: 12/06/2017
ms.prod: sql-server
ms.reviewer: billgib
ms.suite: 
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: article
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: db47d1dd0a6114ebca0715da179cc0629b26781a
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="video-indexed-and-annotated-for-mulit-tenant-saas-app-using-azure-sql-database"></a>Video geïndexeerd en aangetekend voor mulit-tenant SaaS app met Azure SQL Database

Dit artikel is een aantekeningen index in de locaties van de tijd van een video over SaaS-tenancymodus modellen of patronen 81 minuut. In dit artikel kunt u op vorige of doorsturen in de video op welk gedeelte u bent geïnteresseerd. De video worden de ontwerpopties belangrijke voor een databasetoepassing met meerdere tenants in Azure SQL-Database. De video omvat demo's, scenario's van management code en op tijdstippen meer details op de hoogte door ervaring dan mogelijk in onze documentatie is geschreven.

De video versterkt de informatie in onze schriftelijke documentatie, vinden op: 
- *Conceptuele:* [multitenant SaaS-database tenancymodus patronen][saas-concept-design-patterns-563e]
- *Zelfstudies:* [de Wingtip Tickets SaaS-toepassing][saas-how-welcome-wingtip-app-679t]

De video en de artikelen beschrijven de veel fasen voor het maken van een multitenant-toepassing in Azure SQL-Database in de cloud. Speciale functies van Azure SQL Database gemakkelijker te ontwikkelen en implementeren van multitenant-apps die beide gemakkelijker zijn te beheren en betrouwbaar zodat.

We bijwerken onze documentatie geschreven regelmatig. De video wordt niet bewerkt of bijgewerkt, dus uiteindelijk meer van de bijbehorende gegevens mogelijk verouderd.



## <a name="sequence-of-38-time-indexed-screenshots"></a>Reeks 38 tijd geïndexeerd schermafbeeldingen

Deze sectie indexeert de locatie van de tijd voor 38 discussies gedurende de 81 minuten durende video. Elke tijdsindex van aantekeningen is voorzien met een schermafbeelding van de video en soms met aanvullende informatie.

Elke keer index bevindt zich in de indeling van *h*. Bijvoorbeeld, de tweede tijd locatie, met het label geïndexeerd **sessie doelstellingen**, begint op de locatie bij benadering de tijd van **0:03:11**.


### <a name="compact-links-to-video-indexed-time-locations"></a>Compacte koppelingen naar video geïndexeerde tijd locaties

De volgende titels vindt u koppelingen naar hun bijbehorende aantekeningen secties verderop in dit artikel:

- [1. **(Start)**  Welkom dia 0:00:03](#anchor-image-wtip-min00001)
- [2. Sessie doelstellingen, 0:03:11](#anchor-image-wtip-min00311)
- [3. Agenda, 0:04:17](#anchor-image-wtip-min00417)
- [4. Multitenant-web-app: 0:05:05](#anchor-image-wtip-min00505)
- [5. App-webformulier in actie, 0:05:55](#anchor-image-wtip-min00555)
- [6. Tenant-kosten (schaal, isolatie, herstel), 0:09:31](#anchor-image-wtip-min00931)
- [7. Database-modellen voor multitenant: voor- en nadelen, 0:11:59](#anchor-image-wtip-min01159)
- [8. Hybride modellen laat voordelen van MT/ST, 0:13:01](#anchor-image-wtip-min01301)
- [9. Multitenant voor één tenant tegenover: voor- en nadelen, 0:16:44](#anchor-image-wtip-min01644)
- [10. Groepen zijn rendabele bij onvoorspelbare werkbelastingen, 0:19:36](#anchor-image-wtip-min01936)
- [11. Demonstratie van de database per tenant en hybride ST/MT, 0:20:08](#anchor-image-wtip-min02008)
- [12. Live app formulier met Dojo, 0:20:29](#anchor-image-wtip-min02029)
- [13. MYOB en niet een DBA zichtbaar is en 0:28:54](#anchor-image-wtip-min02854)
- [14. MYOB-voorbeeld voor het gebruik van elastische pool, 0:29:40](#anchor-image-wtip-min02940)
- [15. Leren van MYOB en andere ISV's, 0:31:36](#anchor-image-wtip-min03136)
- [16. Patronen opstellen in E2E SaaS scenario 0:43:15](#anchor-image-wtip-min04315)
- [17. Canonieke hybride multitenant SaaS-app, 0:47:33](#anchor-image-wtip-min04733)
- [18. Wingtip SaaS voorbeeld-app 0:48:10](#anchor-image-wtip-min04810)
- [19. Scenario's en patronen in de zelfstudies, 0 verkend: 49:10](#anchor-image-wtip-min04910)
- [20. Demo voor zelfstudies en Github-opslagplaats, 0:50:18](#anchor-image-wtip-min05018)
- [21. Github-repo Microsoft/WingtipSaaS, 0:50:38](#anchor-image-wtip-min05038)
- [22. De patronen, 0 verkennen: 56:20](#anchor-image-wtip-min05620)
- [23. Inrichting van tenants en voorbereiding, 0:57:44](#anchor-image-wtip-min05744)
- [24. Inrichting van tenants en 0-toepassingsverbinding: 58:58](#anchor-image-wtip-min05858)
- [25. Demo voor het beheer van scripts voor het inrichten van een één tenant 0:59:43](#anchor-image-wtip-min05943)
- [26. PowerShell wilt inrichten, en 1:00:02-catalogus](#anchor-image-wtip-min10002)
- [27. T-SQL SELECT * van TenantsExtended, 1:03:30](#anchor-image-wtip-min10330)
- [28. Onvoorspelbare tenantwerkbelastingen, 1:04:36 beheren](#anchor-image-wtip-min10436)
- [29. Bewaking, elastische groep 1:06:39](#anchor-image-wtip-min10639)
- [30. Generatie en prestatiebewaking, 1:09:42 laden](#anchor-image-wtip-min10942)
- [31. Schemabeheer op grote schaal, 1:10:33](#anchor-image-wtip-min11033)
- [32. Gedistribueerde query voor tenant-databases, 1:12:21](#anchor-image-wtip-min11221)
- [33. Demo voor het ticket wordt gegenereerd, 1:12:32](#anchor-image-wtip-min11232)
- [34. SSMS ad-hoc analytics, 1:12:46](#anchor-image-wtip-min11246)
- [35. Tenant-gegevens in SQL DW, 1:16:32 extraheren](#anchor-image-wtip-min11632)
- [36. Grafiek van dagelijkse verkoop-distributie, 1:16:48](#anchor-image-wtip-min11648)
- [37. Inpakken en roept u aan de actie, 1:19:52](#anchor-image-wtip-min11952)
- [38. Bronnen voor meer informatie, 1:20:42](#anchor-image-wtip-min12042)


&nbsp;

### <a name="annotated-index-time-locations-in-the-video"></a>Aantekeningen index tijd locaties in de video

De installatiekopie van een schermafbeelding te klikken gaat u naar de locatie van de exacte tijd in de video.


&nbsp; <a name="anchor-image-wtip-min00001"/>
#### <a name="1-start-welcome-slide-00001"></a>1. *(Start)*  Welkom dia 0:00:01

*Leren van MYOB: ontwerppatronen voor SaaS-toepassingen voor Azure SQL Database - BRK3120*

[![Welkom dia][image-wtip-min00003-brk3120-whole-welcome]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1)

- Titel: Leren van MYOB: ontwerppatronen voor SaaS-toepassingen in Azure SQL-Database
- Bill.Gibson@microsoft.com
- Principal Program Manager, Azure SQL Database
- Microsoft Ignite-sessie BRK3120, Orlando, FL VS oktober/11 2017


&nbsp; <a name="anchor-image-wtip-min00311"/>
#### <a name="2-session-objectives-00153"></a>2. Sessie doelstellingen, 0:01:53
[![De doelstellingen van de sessie][image-wtip-min00311-session]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=113)

- Alternatieve modellen voor multitenant-apps met de voor- en nadelen.
- SaaS-patronen ontwikkeling, management en resource kosten te verlagen.
- Een voorbeeld-app + scripts.
- PaaS-functies + SaaS patronen SQL Database maken een zeer schaalbaar, rendabele platform voor multitenant SaaS.


&nbsp; <a name="anchor-image-wtip-min00417"/>
#### <a name="3-agenda-00409"></a>3. Agenda, 0:04:09
[![Agenda][image-wtip-min00417-agenda]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=249)


&nbsp; <a name="anchor-image-wtip-min00505"/>
#### <a name="4-multi-tenant-web-app-00500"></a>4. Multitenant-web-app: 0:05:00 uur
[![Wingtip SaaS-app: multitenant-web-app][image-wtip-min00505-web-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=300)


&nbsp; <a name="anchor-image-wtip-min00555"/>
#### <a name="5-app-web-form-in-action-00539"></a>5. App-webformulier in actie, 0:05:39
[![App-webformulier in actie][image-wtip-min00555-app-web-form]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=339)


&nbsp; <a name="anchor-image-wtip-min00931"/>
#### <a name="6-per-tenant-cost-scale-isolation-recovery-00658"></a>6. Tenant-kosten (schaal, isolatie, herstel), 0:06:58
[![Per-tenant kosten, schaal, isolatie, herstel][image-wtip-min00931-per-tenant-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=418)


&nbsp; <a name="anchor-image-wtip-min01159"/>
#### <a name="7-database-models-for-multi-tenant-pros-and-cons-00952"></a>7. Database-modellen voor multitenant: voor- en nadelen, 0:09:52
[![Database-modellen voor multitenant: voor- en nadelen][image-wtip-min01159-db-models-pros-cons]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=592)


&nbsp; <a name="anchor-image-wtip-min01301"/>
#### <a name="8-hybrid-model-blends-benefits-of-mtst-01229"></a>8. Hybride modellen laat voordelen van MT/ST, 0:12:29
[![Hybride modellen laat voordelen van MT/ST][image-wtip-min01301-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=749)


&nbsp; <a name="anchor-image-wtip-min01644"/>
#### <a name="9-single-tenant-vs-multi-tenant-pros-and-cons-01311"></a>9. Multitenant voor één tenant tegenover: voor- en nadelen, 0:13:11
[![Multitenant voor één tenant tegenover: voor- en nadelen][image-wtip-min01644-st-vs-mt]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=791)


&nbsp; <a name="anchor-image-wtip-min01936"/>
#### <a name="10-pools-are-cost-effective-for-unpredictable-workloads-01749"></a>10. Groepen zijn rendabele bij onvoorspelbare werkbelastingen, 0:17:49
[![Groepen zijn rendabele voor onvoorspelbare werkbelastingen][image-wtip-min01936-pools-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1069)


&nbsp; <a name="anchor-image-wtip-min02008"/>
#### <a name="11-demo-of-database-per-tenant-and-hybrid-stmt-01959"></a>11. Demonstratie van de database per tenant en hybride ST/MT, 0:19:59
[![Demonstratie van de database per tenant en hybride ST/MT][image-wtip-min02008-demo-st-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1199)


&nbsp; <a name="anchor-image-wtip-min02029"/>
#### <a name="12-live-app-form-showing-dojo-02010"></a>12. Live app formulier met Dojo, 0:20:10
[![Live app formulier Dojo weergeven][image-wtip-min02029-live-app-form-dojo]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1210)

&nbsp; <a name="anchor-image-wtip-min02854"/>
#### <a name="13-myob-and-not-a-dba-in-sight-02506"></a>13. MYOB en niet een DBA zichtbaar is en 0:25 06:
[![MYOB en niet een DBA in zicht][image-wtip-min02854-myob-no-dba]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1506)


&nbsp; <a name="anchor-image-wtip-min02940"/>
#### <a name="14-myob-elastic-pool-usage-example-02930"></a>14. MYOB-voorbeeld voor het gebruik van elastische pool, 0:29:30
[![Voorbeeld van gebruik MYOB de elastische groep][image-wtip-min02940-myob-elastic]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1770)


&nbsp; <a name="anchor-image-wtip-min03136"/>
#### <a name="15-learning-from-myob-and-other-isvs-03125"></a>15. Leren van MYOB en andere ISV's, 0:31:25
[![Leren van MYOB en andere ISV 's][image-wtip-min03136-learning-isvs]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1885)


&nbsp; <a name="anchor-image-wtip-min04315"/>
#### <a name="16-patterns-compose-into-e2e-saas-scenario-03142"></a>16. Patronen opstellen in E2E SaaS scenario 0:31:42
[![Patronen opstellen in E2E SaaS-scenario][image-wtip-min04315-patterns-compose]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1902)


&nbsp; <a name="anchor-image-wtip-min04733"/>
#### <a name="17-canonical-hybrid-multi-tenant-saas-app-04604"></a>17. Canonieke hybride multitenant SaaS-app, 0:46:04
[![Canonieke hybride multitenant SaaS-app][image-wtip-min04733-canonical-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2764)


&nbsp; <a name="anchor-image-wtip-min04810"/>
#### <a name="18-wingtip-saas-sample-app-04801"></a>18. Wingtip SaaS voorbeeld-app 0:48:01
[![Wingtip SaaS voorbeeld-app][image-wtip-min04810-wingtip-saas-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2881)


&nbsp; <a name="anchor-image-wtip-min04910"/>
#### <a name="19-scenarios-and-patterns-explored-in-the-tutorials-04900"></a>19. Scenario's en patronen in de zelfstudies, 0 verkend: 49:00 uur
[![Scenario's en patronen in de zelfstudies verkend][image-wtip-min04910-scenarios-tutorials]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2940)


&nbsp; <a name="anchor-image-wtip-min05018"/>
#### <a name="20-demo-of-tutorials-and-github-repository-05012"></a>20. Demo voor zelfstudies en Github-opslagplaats, 0:50:12
[![Demo zelfstudies en Github-repo-][image-wtip-min05018-demo-tutorials-github]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3012)


&nbsp; <a name="anchor-image-wtip-min05038"/>
#### <a name="21-github-repo-microsoftwingtipsaas-05032"></a>21. Github-repo Microsoft/WingtipSaaS, 0:50:32
[![Github-repo Microsoft/WingtipSaaS][image-wtip-min05038-github-wingtipsaas]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3032)


&nbsp; <a name="anchor-image-wtip-min05620"/>
#### <a name="22-exploring-the-patterns-05615"></a>22. De patronen, 0 verkennen: 56:15
[![De patronen verkennen][image-wtip-min05620-exploring-patterns]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3375)


&nbsp; <a name="anchor-image-wtip-min05744"/>
#### <a name="23-provisioning-tenants-and-onboarding-05619"></a>23. Inrichting van tenants en voorbereiding, 0:56:19
[![Inrichting van tenants en voorbereiding][image-wtip-min05744-provisioning-tenants-onboarding-1]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3379)


&nbsp; <a name="anchor-image-wtip-min05858"/>
#### <a name="24-provisioning-tenants-and-application-connection-05752"></a>24. Inrichting van tenants en 0-toepassingsverbinding: 57:52
[![Inrichting van tenants en de toepassingsverbinding][image-wtip-min05858-provisioning-tenants-app-connection-2]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3472)


&nbsp; <a name="anchor-image-wtip-min05943"/>
#### <a name="25-demo-of-management-scripts-provisioning-a-single-tenant-05936"></a>25. Demo voor het beheer van scripts voor het inrichten van een één tenant 0:59:36
[![Demo voor het inrichten van een tenant één management-scripts][image-wtip-min05943-demo-management-scripts-st]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3576)


&nbsp; <a name="anchor-image-wtip-min10002"/>
#### <a name="26-powershell-to-provision-and-catalog-05956"></a>26. PowerShell wilt inrichten, en 0-catalogus: 59:56
[![PowerShell wilt inrichten, en catalogus][image-wtip-min10002-powershell-provision-catalog]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3596)


&nbsp; <a name="anchor-image-wtip-min10330"/>
#### <a name="27-t-sql-select--from-tenantsextended-10325"></a>27. T-SQL SELECT * van TenantsExtended, 1:03:25
[![T-SQL SELECT * van TenantsExtended][image-wtip-min10330-sql-select-tenantsextended]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3805)


&nbsp; <a name="anchor-image-wtip-min10436"/>
#### <a name="28-managing-unpredictable-tenant-workloads-10334"></a>28. Onvoorspelbare tenantwerkbelastingen, 1:03:34 beheren
[![Onvoorspelbare tenantwerkbelastingen beheren][image-wtip-min10436-managing-unpredictable-workloads]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3814)


&nbsp; <a name="anchor-image-wtip-min10639"/>
#### <a name="29-elastic-pool-monitoring-10632"></a>29. Bewaking, elastische groep 1:06:32
[![Bewaking van de elastische groep][image-wtip-min10639-elastic-pool-monitoring]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3992)


&nbsp; <a name="anchor-image-wtip-min10942"/>
#### <a name="30-load-generation-and-performance-monitoring-10937"></a>30. Generatie en prestatiebewaking, 1:09:37 laden
[![Generatie van de Load- en prestatiebewaking][image-wtip-min10942-load-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4117)


&nbsp; <a name="anchor-image-wtip-min11033"/>
#### <a name="31-schema-management-at-scale-10940"></a>31. Schemabeheer op grote schaal, 1:09:40
[![Schemabeheer op grote schaal][image-wtip-min11033-schema-management-scale]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=34120)


&nbsp; <a name="anchor-image-wtip-min11221"/>
#### <a name="32-distributed-query-across-tenant-databases-11118"></a>32. Gedistribueerde query voor tenant-databases, 1:11:18
[![Gedistribueerde query voor tenant-databases][image-wtip-min11221-distributed-query]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4278)


&nbsp; <a name="anchor-image-wtip-min11232"/>
#### <a name="33-demo-of-ticket-generation-11228"></a>33. Demo voor het ticket wordt gegenereerd, 1:12:28
[![Demo van ticket genereren][image-wtip-min11232-demo-ticket-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4348)


&nbsp; <a name="anchor-image-wtip-min11246"/>
#### <a name="34-ssms-adhoc-analytics-11235"></a>34. SSMS ad-hoc analytics, 1:12:35
[![SSMS ad-hoc analytics][image-wtip-min11246-ssms-adhoc-analytics]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4355)


&nbsp; <a name="anchor-image-wtip-min11632"/>
#### <a name="35-extract-tenant-data-into-sql-dw-11546"></a>35. Tenant-gegevens in SQL DW, 1:15:46 extraheren
[![Tenant-gegevens in SQL DW extraheren][image-wtip-min11632-extract-tenant-data-sql-dw]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4546)


&nbsp; <a name="anchor-image-wtip-min11648"/>
#### <a name="36-graph-of-daily-sale-distribution-11638"></a>36. Grafiek van dagelijkse verkoop-distributie, 1:16:38
[![Grafiek van dagelijkse verkoop-distributie][image-wtip-min11648-graph-daily-sale-distribution]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4598)


&nbsp; <a name="anchor-image-wtip-min11952"/>
#### <a name="37-wrap-up-and-call-to-action-11743"></a>37. Inpakken en roept u aan de actie, 1:17:43
[![Inpakken en roept u aan de actie][image-wtip-min11952-wrap-up-call-action]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4663)


&nbsp; <a name="anchor-image-wtip-min12042"/>
#### <a name="38-resources-for-more-information-12035"></a>38. Bronnen voor meer informatie, 1:20:35
[![Bronnen voor meer informatie][image-wtip-min12042-resources-more-info]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4835)

- [Blogbericht 22 mei 2017][resource-blog-saas-patterns-app-dev-sql-db-768h]

- *Conceptuele:* [multitenant SaaS-database tenancymodus patronen][saas-concept-design-patterns-563e]

- *Zelfstudies:* [de Wingtip Tickets SaaS-toepassing][saas-how-welcome-wingtip-app-679t]

- Github-opslagplaatsen voor versies van de toepassing Wingtip Tickets SaaS-tenancymodus:
    - [Github-repo-voor - zelfstandige toepassingsmodel][github-wingtip-standaloneapp].
    - [Github-repo-voor - DB Per Tenant model][github-wingtip-dbpertenant].
    - [Github-repo-voor - Multitenant-DB model][github-wingtip-multitenantdb].





## <a name="next-steps"></a>Volgende stappen

- [Eerste zelfstudie artikel][saas-how-welcome-wingtip-app-679t]




<!-- Image link reference IDs. -->

[image-wtip-min00003-brk3120-whole-welcome]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00003-brk3120-welcome-myob-design-saas-app-sql-db.png "Welkom dia"

[image-wtip-min00311-session]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00311-session-objectives-takeaway.png "De doelstellingen van de sessie."

[image-wtip-min00417-agenda]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00417-agenda-app-management-models-patterns.png "Agenda."

[image-wtip-min00505-web-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00505-wingtip-saas-app-mt-web.png "Wingtip SaaS-app: multitenant-web-app"

[image-wtip-min00555-app-web-form]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00555-app-form-contoso-concert-hall-night-opera.png "App-webformulier in actie"

[image-wtip-min00931-per-tenant-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00931-saas-data-management-concerns.png "Per-tenant kosten, schaal, isolatie, herstel"

[image-wtip-min01159-db-models-pros-cons]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01159-db-models-multi-tenant-saas-apps.png "Database-modellen voor multitenant: voor- en nadelen"

[image-wtip-min01301-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01301-hybrib-model-blends-benefits-mt-st.png "Hybride modellen laat voordelen van MT/ST"

[image-wtip-min01644-st-vs-mt]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01644-st-mt-pros-cons.png "Multitenant voor één tenant tegenover: voor- en nadelen"

[image-wtip-min01936-pools-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01936-pools-cost-effective-unpredictable-workloads.png "Groepen zijn rendabele voor onvoorspelbare werkbelastingen"

[image-wtip-min02008-demo-st-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02008-demo-st-hybrid.png "Demonstratie van de database per tenant en hybride ST/MT"

[image-wtip-min02029-live-app-form-dojo]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02029-app-form-dogwwod-dojo.png "Live app formulier Dojo weergeven"

[image-wtip-min02854-myob-no-dba]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02854-myob-no-dba.png "MYOB en niet een DBA in zicht"

[image-wtip-min02940-myob-elastic]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02940-myob-elastic-pool-usage-example.png "Voorbeeld van gebruik MYOB de elastische groep"

[image-wtip-min03136-learning-isvs]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min03136-myob-isv-saas-patterns-design-scale.png "Leren van MYOB en andere ISV 's"

[image-wtip-min04315-patterns-compose]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04315-patterns-compose-into-e2e-saas-scenario-st-mt.png "Patronen opstellen in E2E SaaS-scenario"

[image-wtip-min04733-canonical-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04733-canonical-hybrid-mt-saas-app.png "Canonieke hybride multitenant SaaS-app"

[image-wtip-min04810-wingtip-saas-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04810-saas-sample-app-descr-of-modules-links.png "Wingtip SaaS voorbeeld-app"

[image-wtip-min04910-scenarios-tutorials]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04910-scenarios-patterns-explored-tutorials.png "Scenario's en patronen in de zelfstudies verkend"

[image-wtip-min05018-demo-tutorials-github]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05018-demo-saas-tutorials-github-repo.png "Demo voor zelfstudies en Github-repo-"

[image-wtip-min05038-github-wingtipsaas]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05038-github-repo-wingtipsaas.png "Github-repo Microsoft/WingtipSaaS"

[image-wtip-min05620-exploring-patterns]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05620-exploring-patterns-tutorials.png "De patronen verkennen"

[image-wtip-min05744-provisioning-tenants-onboarding-1]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05744-provisioning-tenants-connecting-run-time-1.png "Inrichting van tenants en voorbereiding"

[image-wtip-min05858-provisioning-tenants-app-connection-2]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05858-provisioning-tenants-connecting-run-time-2.png "Inrichting van tenants en de toepassingsverbinding"

[image-wtip-min05943-demo-management-scripts-st]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05943-demo-management-scripts-provisioning-st.png "Demo voor het inrichten van een tenant één management-scripts"

[image-wtip-min10002-powershell-provision-catalog]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10002-powershell-code.png "PowerShell wilt inrichten, en catalogus"

[image-wtip-min10330-sql-select-tenantsextended]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10330-ssms-tenantcatalog.png "T-SQL SELECT * van TenantsExtended"

[image-wtip-min10436-managing-unpredictable-workloads]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10436-managing-unpredictable-tenant-workloads.png "Onvoorspelbare tenantwerkbelastingen beheren"

[image-wtip-min10639-elastic-pool-monitoring]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10639-elastic-pool-monitoring.png "Bewaking van de elastische groep"

[image-wtip-min10942-load-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10942-schema-management-scale.png "Generatie van de Load- en prestatiebewaking"

[image-wtip-min11033-schema-management-scale]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11033-schema-manage-1000s-dbs-one.png "Schemabeheer op grote schaal"

[image-wtip-min11221-distributed-query]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11221-distributed-query-all-tenants-asif-single-db.png "Gedistribueerde query voor tenant-databases"

[image-wtip-min11232-demo-ticket-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11232-demo-ticket-generation-distributed-query.png "Demo van ticket genereren"

[image-wtip-min11246-ssms-adhoc-analytics]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11246-tsql-adhoc-analystics-db-elastic-query.png "SSMS ad-hoc analytics"

[image-wtip-min11632-extract-tenant-data-sql-dw]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11632-extract-tenant-data-analytics-db-dw.png "Tenant-gegevens in SQL DW extraheren"

[image-wtip-min11648-graph-daily-sale-distribution]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11648-graph-daily-sale-contoso-concert-hall.png "Grafiek van dagelijkse verkoop-distributie"

[image-wtip-min11952-wrap-up-call-action]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11952-wrap-call-action-saasfeedback.png "Inpakken en roept u aan de actie"

[image-wtip-min12042-resources-more-info]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min12042-resources-blog-github-tutorials-get-started.png "Bronnen voor meer informatie"




<!-- Article link reference IDs. -->

[saas-concept-design-patterns-563e]: saas-tenancy-app-design-patterns.md

[saas-how-welcome-wingtip-app-679t]: saas-tenancy-welcome-wingtip-tickets-app.md


[video-on-youtube-com-478y]: https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1

[video-on-channel9-479c]: https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3120


[resource-blog-saas-patterns-app-dev-sql-db-768h]: https://azure.microsoft.com/blog/saas-patterns-accelerate-saas-application-development-on-sql-database/


[github-wingtip-standaloneapp]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp/

[github-wingtip-dbpertenant]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/

[github-wingtip-multitenantdb]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/

