---
title: bestand opnemen
description: bestand opnemen
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 06/20/2018
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 43408ebc65d4acf581b612e8ecfb9d00679cc078
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37066098"
---
Gegevensfactory is een multitenant-service die beschikt over de volgende standaardlimiet om ervoor te zorgen klantabonnementen zijn beveiligd tegen elkaars werkbelastingen. Veel van de limieten kunnen eenvoudig gegeven voor uw abonnement tot het maximum aantal contact opnemen met de ondersteuning.

### <a name="version-2"></a>Versie 2

| Resource | Standaardlimiet | Maximumaantal |
| -------- | ------------- | ------------- |
| Data Factory's in een Azure-abonnement | 50 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Totaal aantal entiteiten (Pipeline, gegevenssets, Triggers, gekoppelde Services, Integration runtimes) binnen een gegevensfactory | 5000 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Totale CPU-kernen voor Azure SSIS-integratie Runtime(s) onder één abonnement | 100 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gelijktijdige pipeline wordt per pijplijn | 100 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gelijktijdige pipeline wordt per gegevensfactory | 10.000  | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Maximum aantal activiteiten per pijplijn (inclusief interne activiteiten voor containers) | 40 | 40 |
| Maximum aantal parameters per pijplijn | 50 | 50 |
| ForEach-items | 100,000 | 100,000 |
| ForEach parallelle uitvoering | 20 | 50 |
| Tekens per expressie | 8.192 | 8.192 |
| Minimuminterval Tumbling venster Trigger | 15 minuten | 15 minuten |
| Maximale Timeout voor pipeline-activiteit wordt uitgevoerd | 7 dagen | 7 dagen |
| Aantal bytes per object voor pipeline-objecten <sup>1</sup> | 200 KB | 200 KB |
| Aantal bytes per object voor de gegevensset en objecten van de gekoppelde service <sup>1</sup> | 100 KB | 2000 KB |
| Gegevens integratie eenheden per kopieeractiviteit uitvoeren <sup>3</sup> | 256 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| API-aanroepen schrijven | 2500/hr<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Neem contact op met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Lees API-aanroepen | 12.500/hr<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>Versie 1

| **Resource** | **Standaardlimiet** | **Maximumaantal** |
| --- | --- | --- |
| Data Factory's in een Azure-abonnement |50 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Pijplijnen binnen een gegevensfactory |2500 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gegevenssets binnen een gegevensfactory |5000 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gelijktijdige segmenten per gegevensset |10 |10 |
| Aantal bytes per object voor pipeline-objecten <sup>1</sup> |200 KB |200 KB |
| Aantal bytes per object voor de gegevensset en objecten van de gekoppelde service <sup>1</sup> |100 KB |2000 KB |
| HDInsight-cluster op aanvraag kernen binnen een abonnement <sup>2</sup> |60 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Data movement eenheden per kopieeractiviteit uitvoeren cloud <sup>3</sup> |32 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Aantal voor pijplijn activiteiten bij uitvoering nieuwe pogingen |1000 |MaxInt (32 bits) |

<sup>1</sup> pipeline, gegevensset en objecten van de gekoppelde service vertegenwoordigen een logische groepering van uw workload. Limieten voor deze objecten geen betrekking hebben op de hoeveelheid gegevens die u kunt verplaatsen en met de Azure Data Factory-service verwerken. Gegevensfactory is ontworpen om te schalen voor petabytes aan gegevens.

<sup>2</sup> on-demand HDInsight-kernen worden toegewezen uit het abonnement dat u de gegevensfactory bevat. Als gevolg hiervan is de bovenstaande limiet voor de Data Factory afgedwongen limiet kernen voor bellen op HDInsight kernen en verschilt van de core limiet die is gekoppeld aan uw Azure-abonnement.

<sup>3</sup> gegevens integratie-eenheid (DIU) voor v2 of Cloud Data Movement-eenheid (DMU) voor v1 wordt gebruikt in een cloud-naar-cloud kopieerbewerking. Is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in de Data Factory. Met behulp van meer DMUs voor sommige scenario's kunt u hogere kopie doorvoer behalen. Raadpleeg [Data integratie units (V2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) en [Cloud data movement eenheden (V1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) sectie voor meer informatie en [pagina met prijzen van Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) voor gevolgen voor de facturering.

<sup>4</sup> de integratie Runtime (IR) wordt de beheerinfrastructuur gebruikt door Azure Data Factory om te voorzien in de volgende mogelijkheden voor integratie van verschillende netwerkomgevingen: verplaatsing van gegevens tijdens het verzenden van activiteiten voor de compute-services, uitvoeren van SSIS-pakketten. Zie voor meer informatie [integratie Runtime overzicht](../articles/data-factory/concepts-integration-runtime.md).

| **Resource** | **De ondergrens standaard** | **Minimale limiet** |
| --- | --- | --- |
| Plannen van interval |15 minuten |15 minuten |
| Interval tussen nieuwe pogingen |1 seconde |1 seconde |
| Probeer de time-outwaarde |1 seconde |1 seconde |

#### <a name="web-service-call-limits"></a>Limieten voor aanroep van Web service
Azure Resource Manager heeft limieten voor API-aanroepen. U kunt de API-aanroepen maken met een frequentie binnen de [Azure Resource Manager-API beperkt](../articles/azure-subscription-service-limits.md#resource-group-limits).
