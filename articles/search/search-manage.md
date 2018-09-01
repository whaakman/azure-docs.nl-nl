---
title: Beheer van de service voor Azure Search in Azure portal
description: Azure Search, een gehoste cloud search-service op Microsoft Azure, met de Azure-portal beheren.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 6e751715146d0dfa5c89ae8c2e8cb23d8337432f
ms.sourcegitcommit: e45b2aa85063d33853560ec4bc867f230c1c18ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43371224"
---
# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>Beheer van de service voor Azure Search in Azure portal
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Search is een volledig beheerde, cloud-gebaseerde search-service die wordt gebruikt voor het bouwen van een rijke zoekervaring wordt geboden in aangepaste apps. In dit artikel bevat informatie over de *servicebeheer* taken die u kunt uitvoeren in de [Azure-portal](https://portal.azure.com) voor een search-service die u al hebt ingericht. *Servicebeheer* is een lichtgewicht standaard beperkt tot de volgende taken:

* Beheren en beveiligen van toegang tot de *api-sleutels* gebruikt voor het lezen of schrijven toegang tot uw service.
* Servicecapaciteit door het veranderen van de toewijzing van partities en replica's aanpassen.
* Resourcegebruik, ten opzichte van de maximale limieten van de servicelaag te bewaken.

U ziet dat *upgrade* wordt niet vermeld als een beheertaak. Omdat resources worden toegewezen wanneer de service is ingericht, vereist verplaatsen naar een andere laag een nieuwe service. Zie voor meer informatie, [maken van een Azure Search-service](search-create-service-portal.md).

> [!Tip]
> Zoeken naar Help-informatie over het zoeken in verkeer of query-prestaties analyseren? Krijg inzicht in queryvolume, die voorwaarden mensen zoeken, en hoe succesvol zoekresultaten zijn bij klanten in specifieke documenten in uw index. Zie voor instructies [Search Traffic Analytics voor Azure Search](search-traffic-analytics.md), [controleren metrische gegevens over gebruik en query](search-monitor-usage.md), en [prestaties en optimalisatie](search-performance-optimization.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Administrator-rechten
Inrichting of buiten gebruik stellen van de service zelf kan worden gedaan door een Azure-abonnementbeheerder of CO-beheerder.

Iedereen met toegang tot de service-URL en een admin api-sleutel heeft lees-/ schrijftoegang tot de service in een service. Lees-/ schrijftoegang biedt de mogelijkheid toevoegen, verwijderen of wijzigen van serverobjecten, met inbegrip van api-sleutels, indexen, indexeerfuncties, gegevensbronnen, schema's en roltoewijzingen geïmplementeerd via [RBAC gedefinieerde rollen](search-security-rbac.md).

Alle gebruikersinteractie met Azure Search valt binnen een van deze modi: alleen-lezen toegang tot de service (administrator-rechten), of alleen-lezen toegang tot de service (query rights). Zie voor meer informatie, [beheren van de api-sleutels](search-security-api-keys.md).

<a id="sys-info"></a>

## <a name="logging-and-system-information"></a>Registratie-en systeeminformatie
Azure Search maakt geen logboekbestanden voor een afzonderlijke service via de portal of programmatische interfaces beschikbaar. Op de Basic-laag en hoger, Microsoft bewaakt alle Azure Search-services voor 99,9% beschikbaarheid per serviceovereenkomsten (SLA). De service is traag of doorvoer van aanvragen minder is dan drempelwaarden SLA, ondersteuningsteams Raadpleeg de logboekbestanden die voor hen beschikbaar als het probleem op te lossen.

Wat betreft algemene informatie over uw service, kunt u informatie vinden in de volgende manieren:

* In de portal op het servicedashboard, door middel van meldingen, eigenschappen en statusberichten.
* Met behulp van [PowerShell](search-manage-powershell.md) of de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/) naar [ophalen van de service-eigenschappen](https://docs.microsoft.com/rest/api/searchmanagement/services), of de status op index Resourcegebruik.
* Via [zoekverkeer](search-traffic-analytics.md), zoals eerder is aangegeven.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Monitor Resourcegebruik
In het dashboard is controle van bronnen beperkt tot de informatie die wordt weergegeven in het dashboard en een aantal metrische gegevens die u krijgen kunt door het opvragen van de service. Op het dashboard, in de sectie gebruik, kunt u snel bepalen of partitie resource niveaus voldoende voor uw toepassing zijn.

Met behulp van de Search Service REST API, krijgt u een aantal bestanden op documenten en indexen via een programma: 

* [Indexstatistieken ophalen](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Aantal documenten](https://docs.microsoft.com/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Disaster recovery en service storingen

Hoewel we uw gegevens redden kunt, biedt Azure Search geen onmiddellijke failover van de service als er een storing op het niveau van het cluster of data center. Als een cluster in het datacenter is mislukt, wordt het operationele team detecteert en werken om de service te herstellen. U ondervindt uitvaltijd tijdens het terugzetten van de service. U kunt service tegoed om te compenseren voor de service niet beschikbaar zijn per aanvragen de [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Als continue service vereist in het geval van een onherstelbare fouten buiten de controle van Microsoft is, kunt u [inrichten van een extra service](search-create-service-portal.md) in een andere regio en het implementeren van een strategie voor geo-replicatie om ervoor te zorgen indexen zijn volledig redundante op alle services.

Klanten die gebruikmaken van [indexeerfuncties](search-indexer-overview.md) wilt vullen en vernieuwen van indexen herstel na noodgevallen met geo-specifieke indexeerfuncties gebruik te maken van dezelfde gegevensbron kunnen verwerken. Twee services in verschillende regio's, elk uitvoeren van een indexeerfunctie kunnen indexeren van dezelfde gegevensbron te bereiken, geografische redundantie. Als u uit gegevensbronnen die ook geografisch redundante indexeert, er rekening mee dat Azure Search-Indexeerfuncties kunnen alleen uitvoeren incrementele indexeren van de primaire replica's. In een failover, moet u opnieuw de indexeerfunctie verwijzen naar de nieuwe primaire replica. 

Als u indexeerfuncties niet gebruikt, gebruikt u de code van uw toepassing voor push-objecten en gegevens naar verschillende services tegelijk. Zie voor meer informatie, [prestaties en optimalisatie in Azure Search](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Back-ups en herstellen

Omdat Azure Search niet een oplossing voor primaire gegevens is, bieden we een formele mechanisme voor selfservice back-up en herstel. De code van uw toepassing gebruikt voor het maken en invullen van een index is de gebruikelijke optie voor terugzetten als u per ongeluk een index verwijdert. 

Als u wilt een index opnieuw maken, zou u verwijderen (ervan uitgaande dat deze bestaat), maakt u de index in de service opnieuw en opnieuw laden door het ophalen van gegevens van de primaire gegevensopslag. U kunt ook kunt u bereiken uit aan de klantenservice op restwaarde indexen als er een regionale onderbreking.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Omhoog of omlaag schalen
Elke search-service wordt gestart met een minimum van een van de replica en een partitie. Als u bent aangemeld voor een [laag waarmee toegewezen resources](search-limits-quotas-capacity.md), klikt u op de **schaal** tegel in het dashboard om aan te passen van Resourcegebruik.

Wanneer u de capaciteit tot en met een resource toevoegt, de service ze automatisch gebruikt. Er is geen verdere actie is vereist op het onderdeel, maar er is een lichte vertraging voordat de impact van de nieuwe resource wordt gerealiseerd. Het duurt 15 minuten of meer om aanvullende resources inricht.

 ![][10]

### <a name="add-replicas"></a>Replica's toevoegen
Query's per seconde (QPS) verhogen of bereiken van maximale beschikbaarheid wordt uitgevoerd door het toevoegen van replica's. Elke replica heeft één exemplaar van een index, zodat het toevoegen van een meer van de replica wordt omgezet in een meer index beschikbaar voor de verwerking van query-serviceaanvragen. Een minimum van 3 replica's zijn vereist voor hoge beschikbaarheid (Zie [capaciteitsplanning](search-capacity-planning.md) voor meer informatie).

Een service voor zoeken met meer replica's kan query van aanvragen verdelen over laden via een groter aantal indexen. Uitgaande van een niveau van de queryvolume, query-doorvoer is het verstandig om sneller wanneer er meer exemplaren van de index die beschikbaar zijn voor de aanvraag. Als u latentie van query ondervindt, kunt u een positieve invloed op de prestaties verwachten als de extra replica's online zijn.

Query-doorvoer toeneemt als u replica's toevoegt, maar het niet nauwkeurig dubbele of drie replica's toevoegen aan uw service. Alle toepassingen zijn afhankelijk van externe factoren die van invloed op de prestaties van query's zijn kunnen. Zijn twee factoren die aan variaties in de reactietijden van de query bijdragen complexe query's en de netwerklatentie.

### <a name="add-partitions"></a>Partities toevoegen
De meeste servicetoepassingen hebben ingebouwde nodig meer replica's in plaats van partities. Voor gevallen waarin een aantal grotere documenten vereist is, kunt u partities toevoegen als u zich hebt geregistreerd voor de Standard-service. Basic-laag biedt geen voor aanvullende partities.

De Standard-laag, partities worden toegevoegd in veelvouden van 12 (met name 1, 2, 3, 4, 6 of 12). Dit is een artefact van sharding. Een index is gemaakt in 12 shards, die kunnen worden alle opgeslagen op 1 partitie of evenredig onderverdeeld in 2, 3, 4, 6 of 12 partities (één shard per partitie).

### <a name="remove-replicas"></a>Replica's verwijderen
U kunt na perioden van hoge query volumes, replica's verminderen nadat zoeken querybelasting zijn genormaliseerd (bijvoorbeeld, nadat de feestdagen verkoop staan).

U doet dit door de schuifregelaar replica terug op een lager getal. Er zijn geen verdere stappen vereist van uw kant. Het aantal replica's te verlagen, vervangen door virtuele machines in het datacenter. Uw query's en bewerkingen voor gegevensopname wordt nu uitgevoerd op minder VM's dan vóór. De minimumlimiet is een van de replica.

### <a name="remove-partitions"></a>Partities verwijderen
Tegenstelling tot het verwijderen van replica's waarvoor geen extra inspanningen op het onderdeel is vereist, is mogelijk wat werk te doen als u meer opslag dan kunnen worden verlaagd. Bijvoorbeeld, als uw oplossing van drie partities gebruikmaakt, wordt downsizing aan één of twee partities een fout gegenereerd als de nieuwe opslagruimte kleiner dan vereist is. Als u had verwacht, wordt uw keuzes zijn om indexen of documenten in een gekoppeld index ruimte vrij of behoud de huidige configuratie te verwijderen.

Er is geen detectiemethode die aangeeft welke index shards zijn opgeslagen op specifieke partities. Elke partitie biedt ongeveer 25 GB in de opslag, dus moet u opslag verminderen in een grootte die kan worden aangepast door het aantal partities die u hebt. Als u wilt terugkeren naar één partitie, moet alle 12 shards past.

Om u te helpen bij de planning voor de toekomst, wilt u mogelijk opslag controleren (met behulp van [indexstatistieken ophalen](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) om te zien hoeveel u daadwerkelijk gebruikt. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Best practices voor schalen en implementatie
Deze video van 30 minuten worden aanbevolen procedures voor geavanceerde implementatiescenario's, waaronder geografisch gedistribueerde werkbelastingen. U kunt ook zien [prestaties en optimalisatie in Azure Search](search-performance-optimization.md) voor help-pagina's die betrekking hebben op de dezelfde punten.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Volgende stappen
Nadat u de concepten achter servicebeheer begrijpt, kunt u overwegen [PowerShell](search-manage-powershell.md) om taken te automatiseren.

Ook raadzaam de [prestaties en optimalisatie van het artikel](search-performance-optimization.md).

Een andere aanbeveling is om te bekijken van de video in de vorige sectie hebt genoteerd. Het biedt een uitgebreidere dekking van de technieken die in deze sectie.

<!--Image references-->
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



