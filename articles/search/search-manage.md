---
title: Beheer van de service voor Azure Search in de Azure portal
description: Azure Search, een gehoste cloud search-service in Microsoft Azure met Azure portal beheren.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 916a08aacca428530bc4f728d5de422e04bed8bc
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>Beheer van de service voor Azure Search in de Azure portal
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Search is een volledig beheerde, cloud-gebaseerde zoekservice gebruikt voor het bouwen van een uitgebreide zoekervaring in aangepaste apps. In dit artikel bevat informatie over de *service-beheer* taken die u kunt uitvoeren in de [Azure-portal](https://portal.azure.com) voor een zoekservice die u al hebt ingericht. *Service-beheer* is een lichtgewicht standaard beperkt tot de volgende taken:

* Beheren en beveiligen van toegang tot de *api-sleutels* gebruikt voor lees- of schrijftoegang heeft tot uw service.
* Servicecapaciteit door het wijzigen van de toewijzing van partities en replica's aanpassen.
* Monitor Resourcegebruik ten opzichte van maximaal limieten van de servicelaag.

U ziet dat *upgrade* wordt niet vermeld als een beheertaak. Omdat bronnen worden toegewezen wanneer de service is ingericht, vereist verplaatsen naar een andere laag een nieuwe service. Zie voor meer informatie [maken van een Azure Search-service](search-create-service-portal.md).

> [!Tip]
> Hulp bij het zoeken verkeer of query-prestaties analyseren zoekt? Beter inzicht in de query-volume voorwaarden mensen zoeken, en hoe geslaagde zoekresultaten zijn klanten leidt tot specifieke documenten in uw index. Zie voor instructies [Search Traffic Analytics voor Azure Search](search-traffic-analytics.md), [gebruiks- en metrische gegevens controleren](search-monitor-usage.md), en [prestaties en optimalisatie](search-performance-optimization.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Administrator-rechten
Het inrichten of buiten gebruik stellen van de service zelf kan worden gedaan door de beheerder van de Azure-abonnement of CO-beheerder.

Binnen een service en heeft iedereen met toegang tot de service-URL en een admin api-sleutel lezen-schrijven toegang tot de service. Lees-/ schrijftoegang biedt de mogelijkheid toevoegen, verwijderen of wijzigen van de server-objecten, met inbegrip van api-sleutels, indexen, indexeerfuncties, gegevensbronnen, schema's en roltoewijzingen geïmplementeerd via [RBAC gedefinieerde rollen](#rbac).

Alle gebruikersinteractie met Azure Search valt binnen een van deze modi: lezen-schrijven toegang tot de service (beheerdersrechten) of alleen-lezen toegang tot de service (query rechten). Zie voor meer informatie [beheren van de api-sleutels](#manage-keys).

<a id="sys-info"></a>

## <a name="set-rbac-roles-for-administrative-access"></a>RBAC-rollen voor beheerderstoegang instellen
Azure biedt een [globale op rollen gebaseerde verificatie model](../active-directory/role-based-access-control-configure.md) beheerd via de portal of de resourcemanager-API's voor alle services. Rollen eigenaar, bijdrager en lezer bepalen het niveau van de servicebeheer voor Active Directory-gebruikers, groepen en beveiligings-principals die zijn toegewezen aan elke rol. 

RBAC machtigingen bepalen de volgende beheertaken voor Azure Search:

| Rol | Taak |
| --- | --- |
| Eigenaar |Maak of verwijder de service of een object op de service, inclusief api-sleutels, indexen, indexeerfuncties, indexeerfunctie gegevensbronnen en indexeerfunctie schema's.<p>Bekijk de servicestatus, met inbegrip van aantallen en de opslaggrootte.<p>Toevoegen of verwijderen van lidmaatschap van de rol (alleen een eigenaar kunt rollidmaatschap beheren).<p>Abonnementbeheerders en eigenaren van de service kunt u de automatische lid zijn van de rol van eigenaar. |
| Inzender |Hetzelfde niveau van toegang als eigenaar, min RBAC rollen beheren. Bijvoorbeeld, een Inzender kunt weergeven en opnieuw genereren `api-key`, maar rollidmaatschappen niet wijzigen. |
| Lezer |De status en query sleutels service weergeven. Leden van deze rol kunnen de configuratie van de service niet wijzigen, noch kunnen ze administratorsleutels weergeven. |

Rollen ken geen toegangsrechten voor het service-eindpunt. Search-servicebewerkingen, zoals beheer van de index, populatie van de index en query's op zoekgegevens, worden beheerd via api-sleutels, niet rollen. Zie voor meer informatie 'Autorisatie voor het beheer van versus gegevensbewerkingen' in [wat is er op rollen gebaseerde toegangsbeheer](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>
## <a name="logging-and-system-information"></a>Logboekregistratie en systeeminformatie
Azure Search maakt logboekbestanden voor een afzonderlijke service via de portal of programmatische interfaces niet beschikbaar. Met de basisstaffel of hoger, Microsoft bewaakt alle Azure Search-services voor beschikbaarheid van 99,9% per serviceovereenkomsten (SLA). De service is langzaam of doorvoer van aanvragen valt onder SLA drempelwaarden, ondersteuningsteams Raadpleeg de logboekbestanden die voor hen beschikbaar als het probleem op te lossen.

In termen van algemene informatie over uw service, kunt u de informatie in de volgende manieren verkrijgen:

* In de portal op het servicedashboard via meldingen, eigenschappen en statusberichten.
* Met behulp van [PowerShell](search-manage-powershell.md) of de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/) naar [service-eigenschappen ophalen](https://docs.microsoft.com/rest/api/searchmanagement/services), of de status van het Resourcegebruik van de index.
* Via [zoeken traffic analytics](search-traffic-analytics.md), zoals eerder is aangegeven.

<a id="manage-keys"></a>

## <a name="manage-api-keys"></a>Api-sleutels beheren
Alle aanvragen voor een search-service moeten een api-sleutel die is gegenereerd specifiek voor uw service. Deze api-sleutel is het enige mechanisme voor het verifiëren van toegang tot uw search-service-eindpunt. 

Een api-sleutel is een tekenreeks van willekeurige getallen en letters bestaan. Via [RBAC machtigingen](#rbac), kunt u verwijderen of de toetsen niet lezen, maar u kunt een sleutel niet vervangen met een door de gebruiker gedefinieerd wachtwoord. 

Twee soorten sleutels worden gebruikt voor toegang tot uw search-service:

* Beheerder (geldig voor een bewerking lezen-schrijven tegen de service)
* Query (geldig voor alleen-lezen bewerkingen, zoals een query uitgevoerd naar een index)

Een admin api-sleutel wordt gemaakt wanneer de service is ingericht. Er zijn twee administratorsleutels aangewezen als *primaire* en *secundaire* te houden rechtstreeks, maar in feite ze uitwisselbaar zijn. Elke service heeft twee administratorsleutels zodat u een overschakelen kunt zonder verlies van toegang tot uw service. Kunt u beide administratorsleutel opnieuw genereren, maar u kunt toevoegen aan het aantal totale-beheerder. Er is een maximum van twee administratorsleutels per zoekservice.

Querysleutels zijn ontworpen voor clienttoepassingen die rechtstreeks zoeken aanroepen. U kunt maximaal 50 querysleutels maken. In de toepassingscode geeft u de URL zoeken en een query api-sleutel voor alleen-lezen toegang tot de service. Uw toepassingscode geeft ook de index die is gebruikt door de toepassing. Het eindpunt een api-sleutel voor alleen-lezen toegang en een doelindex definiëren samen het bereik en het toegangsniveau van de verbinding van uw clienttoepassing.

Als u wilt ophalen of api-sleutels genereren, open het servicedashboard. Klik op **sleutels** naar de pagina Sleutelbeheer openen. Opdrachten voor het opnieuw genereren of het maken van sleutels zijn aan de bovenkant van de pagina. Standaard worden alleen administratorsleutels gemaakt. Query api-sleutels moeten handmatig worden gemaakt.

 ![][9]

<a id="rbac"></a>

## <a name="secure-api-keys"></a>Api-sleutels beveiligen
Sleutelbeveiliging wordt gegarandeerd door het beperken van toegang via de portal of de Resource Manager-interfaces (PowerShell of opdrachtregelinterface). Zoals is aangegeven, kunnen abonnementbeheerders weergeven en opnieuw genereren van alle api-sleutels. Bekijk de roltoewijzingen om te begrijpen wie toegang heeft tot de beheersleutels uit voorzorg.

1. Klik op het pictogram toegang dia open de blade gebruikers in het servicedashboard.
   ![][7]
2. Controleer in gebruikers, bestaande roltoewijzingen. Zoals verwacht, abonnementsbeheerders al volledige toegang hebben tot de service via de rol van eigenaar.
3. Om in te zoomen verdere, klikt u op **abonnementsbeheerders** en vouw vervolgens de lijst van de toewijzing van functie om te zien wie mede beheerdersrechten heeft op uw search-service.

Een andere manier om weer te geven van machtigingen voor toegang is te klikken op **rollen** op de blade gebruikers. Hiermee geeft u beschikbare rollen en het aantal gebruikers of groepen die zijn toegewezen aan elke rol.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Resourcegebruik van de monitor
In het dashboard is Broncontrole beperkt tot de informatie die wordt weergegeven in het servicedashboard en enkele metrische gegevens die u verkrijgen kunt door het opvragen van de service. U kunt op het servicedashboard in de sectie gebruik snel bepalen of partitie resource niveaus, geschikt voor uw toepassing zijn.

Met de Search Service REST API, krijgt u een aantal aan documenten en indexen programmatisch: 

* [Indexstatistieken opvragen](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Aantal documenten](https://docs.microsoft.com/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Disaster recovery en service uitval

Hoewel we uw gegevens restwaarde kunnen, biedt Azure Search geen chatberichten failover van de service als er een storing op het cluster of data center-niveau. Als een cluster in het datacenter uitvalt, wordt het operationele team detecteert en werken om de service te herstellen. Treedt uitvaltijd tijdens het terugzetten van de service. U kunt aanvragen servicevergoedingen om te compenseren voor de service niet beschikbaar zijn per de [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Als continue vereist in het geval van kritieke fouten optreden buiten het beheer van Microsoft is, kan [inrichten van een extra service](search-create-service-portal.md) in een andere regio en implementeer een geo-replicatie-strategie om ervoor te zorgen indexen volledig redundante zijn op alle services.

Klanten die gebruikmaken van [indexeerfuncties](search-indexer-overview.md) vullen en indexen vernieuwen herstel na noodgevallen via geo-specifieke indexeerfuncties gebruik van dezelfde gegevensbron kunnen verwerken. Twee services in verschillende regio's, elk met een indexeerfunctie uitgevoerd kunnen index in dezelfde gegevensbron als u de geografische redundantie. Als u van gegevensbronnen die ook geografisch redundante zijn indexeren, let er dan voor dat dat Azure Search indexeerfuncties alleen uitvoeren kunnen, incrementele indexering van de primaire replica's. Zorg dat u opnieuw de indexeerfunctie verwijzen naar de nieuwe primaire replica in een failover. 

Als u geen indexeerfuncties gebruikt, gebruikt u de toepassingscode voor push-objecten en gegevens naar andere zoekservices parallel. Zie voor meer informatie [prestaties en optimalisatie in Azure Search](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Back-ups en herstellen

Omdat Azure Search niet een opslagoplossing primaire gegevens is, bieden we een formele mechanisme voor self-service back-up en herstel niet. De toepassingscode die wordt gebruikt voor het maken en vullen met een index is de feitelijke optie voor terugzetten als u per ongeluk een index verwijdert. 

Als u wilt een index opnieuw worden opgebouwd, zou u verwijderen (ervan uitgaande dat deze bestaat), de index in de service opnieuw en opnieuw laden door het ophalen van gegevens van uw primaire data store. U kunt ook u kan bereiken [klantondersteuning]() restwaarde indexen als er een regionale uitval.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Omhoog of omlaag schalen
Elke search-service wordt gestart met ten minste één replica en één partitie. Als u zich hebt aangemeld voor een [laag waarmee toegewijde bronnen](search-limits-quotas-capacity.md), klikt u op de **SCALE** -tegel in het servicedashboard Resourcegebruik aanpassen.

Wanneer u capaciteit via een resource toevoegt, de service ze automatisch gebruikt. Er is geen verdere actie vereist van uw kant, maar er is een lichte vertraging optreden voordat de impact van de nieuwe resource wordt gerealiseerd. Het duurt 15 minuten of langer om in te richten aanvullende bronnen.

 ![][10]

### <a name="add-replicas"></a>Replica's toevoegen
Verhogen van query's per seconde (QPS) of bereiken van maximale beschikbaarheid wordt gedaan door replica's toe te voegen. Elke replica heeft één exemplaar van een index, zodat één meer replica toe te voegen naar een meer index beschikbaar vertaalt voor de verwerking van query-serviceaanvragen. Een minimum van 3 replica's zijn vereist voor hoge beschikbaarheid (Zie [capaciteitsplanning](search-capacity-planning.md) voor meer informatie).

Een service voor zoeken met meer replica's kunt query van aanvragen verdelen over laden via een groter aantal indexen. Uitgaande van een niveau van de query-volume, query-doorvoer is het verstandig om snellere wanneer er meer exemplaren van de index beschikbaar om de aanvraag te verwerken. Als u Querylatentie ondervindt, kunt u een positieve invloed op de prestaties verwachten zodra de extra replica online is.

Query doorvoer gaat u als u replica's toevoegt, maar deze niet exact dubbele of triple als u replica's aan uw service toevoegen. Alle zoektoepassingen zijn onderworpen aan externe factoren die van invloed op de prestaties van query's zijn kunnen. Zijn twee factoren die tot variaties in reactietijden van de query bijdragen complexe query's en netwerklatentie.

### <a name="add-partitions"></a>Partities toevoegen
De meeste servicetoepassingen hebben een ingebouwde hebben om meer replica's in plaats van partities. Voor gevallen waarin een aantal toegenomen document vereist is, kunt u partities toevoegen als u zich aangemeld voor Standard-service. Laag Basic biedt geen voor extra partities.

Op de prijscategorie Standard partities toegevoegd in veelvouden van 12 (specifiek, 1, 2, 3, 4, 6 of 12). Dit is een artefact van sharding. Een index is gemaakt in 12 shards, die kunnen worden alle opgeslagen op 1 partitie of gelijkmatig verdeeld 2, 3, 4, 6 of 12 partities (één shard per partitie).

### <a name="remove-replicas"></a>Replica's verwijderen
U kunt na perioden van hoge query volumes replica's verminderen nadat zoeken querybelasting hebben (bijvoorbeeld nadat vakantiedag verkoop via zijn) genormaliseerd.

U doet dit door de schuifregelaar replica terug op een lager getal. Er zijn geen verdere stappen vereist op uw kant. Het aantal replica's te verlagen vervangen door virtuele machines in het datacenter. Uw query's en bewerkingen voor opname wordt nu uitgevoerd op minder virtuele machines dan voordat. De minimumlimiet is één replica.

### <a name="remove-partitions"></a>Partities verwijderen
In tegenstelling tot het verwijderen van replica's waarvoor geen extra moeite ondernemen, moet u wellicht wat werk te doen als u meer opslagruimte dan kan worden verkleind. Bijvoorbeeld, als uw oplossing van drie partities gebruikmaakt, downsizing op een of twee partities genereren een fout als de nieuwe opslagruimte kleiner dan vereist is. Zoals u verwachten zou, zijn uw keuzes te verwijderen van indexen of documenten binnen een bijbehorende index ruimte vrij te maken of de huidige configuratie te behouden.

Er is geen detectiemethode die aangeeft welke shards index op specifieke partities zijn opgeslagen. Elke partitie bevat ongeveer 25 GB in de opslag, dus moet u de opslag voor een grootte die kan worden aangepast door het aantal partities die u hebt. Als u wilt terugkeren naar één partitie, moet alle 12 shards past.

Om te helpen bij het plannen van toekomstige, wilt u mogelijk opslag controleren (met behulp van [statistieken voor Index ophalen](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) om te zien hoeveel u daadwerkelijk gebruikt. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Aanbevolen procedures op schaal en implementatie
Deze 30 minuten durende video bekijkt aanbevelingen voor geavanceerde implementatiescenario's waaronder geografisch verspreide werkbelastingen. U ziet ook [prestaties en optimalisatie in Azure Search](search-performance-optimization.md) voor help-pagina's die betrekking hebben op dezelfde plaatsen.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Volgende stappen
Zodra u de basisbegrippen achter servicebeheer kunt u overwegen [PowerShell](search-manage-powershell.md) om taken te automatiseren.

Wordt ook aangeraden controleren de [prestaties en optimalisatie van het artikel](search-performance-optimization.md).

Een andere aanbeveling is de video in de vorige sectie hebt genoteerd. Het biedt diepere dekking van de technieken die in deze sectie wordt vermeld.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



