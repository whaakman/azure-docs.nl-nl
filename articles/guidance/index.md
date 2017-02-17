---
title: Documentatie over Azure-richtlijnen - zelfstudies | Microsoft Docs
description: Aanbevolen procedures en richtlijnen voor Azure
services: guidance
author: carolz
manager: carolz
layout: LandingPage
ms.assetid: 
ms.service: guidance
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 01/23/2017
ms.author: carolz
translationtype: Human Translation
ms.sourcegitcommit: f5ced8f436f4d1426e4d2b307fcfb6b644e94aaf
ms.openlocfilehash: c05d311633b9c9aa5490e55baafe83376d0c8fbc

---
# <a name="azure-guidance"></a>Azure-richtlijnen
[!INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Het team van Microsoft voor patronen en procedures maakt deel uit van het Adviesteam voor Azure-klanten. Ons doel is om ontwikkelaars, architecten en IT-specialisten te ondersteunen bij een geslaagde implementatie van het Microsoft Azure-platform. We ontwikkelen richtlijnen met aanbevolen procedures voor het bouwen van cloudoplossingen in Azure.

## <a name="checklists"></a>Controlelijsten
Deze lijsten bieden naslaginformatie over de fundamentele aspecten van beschikbaarheid en schaalbaarheid. 

* [Controlelijst voor beschikbaarheid][AvailabilityChecklist] 
  
    Een samenvatting van de aanbevolen procedures voor het waarborgen van beschikbaarheid.
* [Controlelijst voor schaalbaarheid][ScalabilityChecklist]
  
    Een samenvatting van aanbevolen procedures voor het ontwerpen en implementeren van schaalbare services en het uitvoeren van gegevensbeheer.

## <a name="best-practices-articles"></a>Artikelen over aanbevolen procedures
Deze artikelen gaan uitgebreider in op belangrijke concepten op het gebied van cloud computing. 

* [API ontwerpen][APIDesign] 
  
    Een bespreking van de ontwerpaspecten waarmee u rekening moet houden tijdens het ontwerpen van een web-API.
* [API implementeren][APIImplementation] 
  
    Een reeks aanbevolen procedures voor het implementeren en publiceren van een web-API.
* [Richtlijnen voor API-beveiliging](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 
  
    Een bespreking van verificatie- en autorisatiekwesties (zoals tokentypen, autorisatieprotocollen, autorisatiestromen en risicobeperking).
* [Richtlijnen voor automatisch schalen][AutoscalingGuidance] 
  
    Een samenvatting van overwegingen voor het schalen van oplossingen zonder de noodzaak van handmatige tussenkomst.
* [Richtlijnen voor achtergrondtaken][BackgroundJobsGuidance] 
  
    Een beschrijving van de beschikbare opties en aanbevolen procedures voor het implementeren van taken die in de achtergrond moeten worden uitgevoerd, onafhankelijk van eventuele bewerkingen in de voorgrond of interactieve bewerkingen.
* [CDN-richtlijnen (Content Delivery Network)][CDNGuidance] 
  
    Algemene richtlijnen en aanbevolen procedures voor gebruik van het CDN om de belasting op uw toepassingen te minimaliseren en de beschikbaarheid en prestaties te maximaliseren.
* [Richtlijnen voor caching][CachingGuidance] 
  
    Een samenvatting van hoe de cache kan worden gebruikt om de prestaties en schaalbaarheid van een systeem te verbeteren.
* [Richtlijnen voor gegevenspartitionering][DataPartitioningGuidance]
  
    Strategieën voor het maken van gegevenspartities die u kunt gebruiken om de schaalbaarheid te verbeteren, conflicten te verminderen en de prestaties te optimaliseren.
* [Richtlijnen voor bewaking en diagnostiek][MonitoringandDiagnosticsGuidance] 
  
    Richtlijnen voor het bijhouden van de manier waarop uw gebruikers uw systeem en resources gebruiken en voor het controleren van de algehele de gezondheid en prestaties van uw systeem.
* [Aanbevolen naamgevingsregels][naming-conventions] 
  
    Aanbevolen naamgevingsregels voor Azure-resources.
* [Algemene richtlijnen voor opnieuw proberen][RetryGeneralGuidance] 
  
    Bespreking van de algemene concepten voor het verwerken van tijdelijke fouten.
* [Servicespecifieke richtlijnen voor opnieuw proberen][RetryServiceSpecificGuidance]
  
    Dit artikel biedt een samenvatting van de functies voor het opnieuw proberen van een groot aantal Azure-services, waaronder informatie over het gebruiken, aanpassen of uitbreiden van het mechanisme voor opnieuw proberen van de betreffende service.

## <a name="scenario-guides"></a>Scenariorichtlijnen
* [Elasticsearch uitvoeren op Azure][elasticsearch] 
  
    Elasticsearch is een zeer schaalbare open-source zoekmachine en -database. De service is geschikt voor situaties waarin een snelle analyse en detectie van informatie in grote gegevenssets is vereist. Dit artikel gaat in op een aantal belangrijke aspecten waarmee u rekening moet houden tijdens het ontwerpen van een Elasticsearch-cluster.
* [Identiteitsbeheer voor multitenant-toepassingen][identity-multitenant] 
  
    Multitenancy is een architectuur waarin meerdere tenants een toepassing delen maar van elkaar zijn geïsoleerd. In dit artikel wordt uitgelegd hoe u gebruikers-id's binnen een multitenant-toepassing kunt beheren met [Azure Active Directory][AzureAD] voor het afhandelen van aanmelding en verificatie.
* [Big data-oplossingen ontwikkelen](https://msdn.microsoft.com/library/dn749874.aspx)
  
    Dit artikel gaat in op het gebruik van HDInsight voor scenario's als iteratieve gegevensverkenning, als een datawarehouse, voor ETL-processen en voor integratie met bestaande BI-systemen. Daarnaast biedt het meer inzicht in de concepten van big data, het plannen en ontwerpen van big data-oplossingen en het implementeren van deze oplossingen.

## <a name="patterns"></a>Patronen
* [Cloudontwerppatronen: prescriptieve richtlijnen voor de architectuur van cloudtoepassingen](https://msdn.microsoft.com/library/dn568099.aspx)
  
    Dit artikel biedt een bibliotheek met ontwerppatronen en verwante hulponderwerpen. Het gaar in op de voordelen van het gebruik van patronen door te laten zien hoe elk patroon in cloudtoepassingsarchitecturen kan worden ingepast.
* [De prestaties optimaliseren voor cloudtoepassingen](https://github.com/mspnp/performance-optimization)
  
    Dit artikel gaat in op de algemene antipatronen die voorkomen dat de schaal van apps onder belasting kan worden aangepast. Het bevat voorbeelden van acht antipatronen, [informatie over prestatie-analyses](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) en richtlijnen voor het [beoordelen van de prestaties ten opzichte van metrische sleutelgegevens](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Referentiearchitecturen
Onze referentiearchitecturen zijn geordend op scenario.
Elke afzonderlijke architectuur biedt aanbevolen procedures, prescriptieve stappen en een uitvoerbare onderdeel waarin de aanbevelingen in acht zijn genomen.

De huidige bibliotheek met referentiearchitecturen is beschikbaar op [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Tolerantierichtlijnen
Deze onderwerpen beschrijven hoe u toepassingen kunt ontwerpen die tolerant omgaan met fouten in een gedistribueerde cloudomgeving.   

* [Overzicht tolerantie][ResiliencyOvervew]
  
     Dit artikel laat zien hoe u toepassingen kunt bouwen voor het Azure-platform die zich kunnen herstellen van fouten en die blijven werken. Het biedt een gestructureerde benadering voor het bereiken van tolerantie, van ontwerp en implementatie tot ingebruikname en uitvoering.
* [Controlelijst voor tolerantie][resiliency-checklist]
  
    Een controlelijst met aanbevelingen die u helpen anticiperen op diverse foutmodi die zich kunnen voordoen.
* [Analyse van foutmodus][resiliency-fma] 
  
    Analyse van foutmodus (FMA) is een proces aan de hand waarvan u tolerantie in een systeem kunt inbouwen door mogelijke punten van mislukken te identificeren. Als uitgangspunt voor het FMA-proces bevat dit artikel een catalogus van mogelijke foutmodi en de bijbehorende oplossingen. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md




<!--HONumber=Feb17_HO2-->


