---
title: bestand opnemen
description: bestand opnemen
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 1/10/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 2e9e4fe06f4d3802f6c0b3ff13e6cf6cdc218050
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211826"
---
Data factory is een multitenant-service met de volgende standaardlimieten in plaats om te controleren of abonnementen van de gebruiker zijn beveiligd tegen elkaars werkbelastingen. Veel van de limieten kunnen worden eenvoudig gegenereerd voor uw abonnement tot de maximale limiet door contact op met ondersteuning.

### <a name="version-2"></a>Versie 2

| Resource | Standaardlimiet | Maximumaantal |
| -------- | ------------- | ------------- |
| Data Factory's in een Azure-abonnement | 50 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Totaal aantal entiteiten (Pipeline, gegevenssets, Triggers, gekoppelde Services, integratieruntimes) in een data factory | 5000 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Totale CPU-kernen voor Azure-SSIS-integratie Runtime(s) onder één abonnement | 256 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gelijktijdige pijplijnuitvoeringen per gegevensfactory (gedeeld tussen alle pijplijnen in de fabriek) | 10.000  | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Maximum aantal activiteiten per pijplijn (inclusief interne activiteiten voor containers) | 40 | 40 |
| Maximumaantal gekoppelde Integration Runtime die kunnen worden gemaakt op basis van een enkele zelfgehoste Cloudintegratieruntime | 20 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Maximum aantal parameters per pijplijn | 50 | 50 |
| ForEach-items | 100.000 | 100.000 |
| Parallelle uitvoering ForEach | 20 | 50 |
| Tekens per expressie | 8.192 | 8.192 |
| Minimale Tumblingvenstertriggers interval | 15 minuten | 15 minuten |
| Max Timeout voor pipeline-activiteit wordt uitgevoerd | 7 dagen | 7 dagen |
| Bytes per object voor pijplijnobjecten <sup>1</sup> | 200 KB | 200 KB |
| Bytes per object voor gegevensset en de gekoppelde serviceobjecten <sup>1</sup> | 100 KB | 2000 KB |
| Integratie gegevenseenheden per uitvoering van activiteit kopiëren <sup>3</sup> | 256 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Schrijven van API-aanroepen | 2500/uur<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Lezen API-aanroepen | 12.500/uur<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Bewaking van query's per minuut | 1000 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Entiteit CRUD-bewerkingen per minuut | 50 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>Versie 1

| **Resource** | **Standaardlimiet** | **Maximumaantal** |
| --- | --- | --- |
| Data Factory's in een Azure-abonnement |50 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Pijplijnen in een data factory |2500 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gegevenssets in een data factory |5000 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gelijktijdige segmenten per gegevensset |10 |10 |
| Bytes per object voor pijplijnobjecten <sup>1</sup> |200 KB |200 KB |
| Bytes per object voor gegevensset en de gekoppelde serviceobjecten <sup>1</sup> |100 KB |2000 KB |
| HDInsight-cluster op aanvraag kernen binnen een abonnement <sup>2</sup> |60 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Eenheden voor gegevensverplaatsing per exemplaar activiteit die wordt uitgevoerd in de cloud <sup>3</sup> |32 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Aantal van de pijplijnactiviteit nieuwe pogingen |1000 |MaxInt (32 bits) |

<sup>1</sup> pijplijn, gegevensset en objecten van de gekoppelde service vertegenwoordigt een logische groepering van uw workload. Limieten voor deze objecten staan niet in verband met de hoeveelheid gegevens die u kunt verplaatsen en verwerken met de Azure Data Factory-service. Data factory is ontworpen om te schalen voor het afhandelen van petabytes aan gegevens.

<sup>2</sup> on-demand HDInsight-cores worden toegewezen uit het abonnement met de data factory. Als gevolg hiervan is de bovenstaande limiet voor de Data Factory afgedwongen kernlimiet voor on-demand HDInsight-cores en wijkt af van de core-limiet die is gekoppeld aan uw Azure-abonnement.

<sup>3</sup> gegevens integratie-eenheid (DIU) voor v2 of Cloud Data Movement-eenheid (DMU) voor v1 wordt gebruikt in een cloud-naar-cloud kopieerbewerking. Het is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in Data Factory. U kunt hogere doorvoer van de kopie bereiken met behulp van meer DMUs voor enkele scenario's. Raadpleeg [eenheden van de integratie van gegevens (V2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) en [eenheden voor gegevensverplaatsing (V1) in de Cloud](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) sectie voor meer informatie en [pagina met prijzen van Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) voor de gevolgen voor de facturering.

<sup>4</sup> de Integration Runtime (IR) is de rekeninfrastructuur die is gebruikt door Azure Data Factory om te voorzien van de volgende mogelijkheden voor gegevensintegratie in verschillende netwerkomgevingen: gegevensverplaatsing, activiteiten voor de compute-services, verzenden de uitvoering van SSIS-pakketten. Zie voor meer informatie, [Integration Runtime overzicht](../articles/data-factory/concepts-integration-runtime.md).

| **Resource** | **Standaard ondergrens** | **Minimale versie** |
| --- | --- | --- |
| Interval plannen |15 minuten |15 minuten |
| Interval tussen nieuwe pogingen |1 seconde |1 seconde |
| Probeer time-outwaarde |1 seconde |1 seconde |

#### <a name="web-service-call-limits"></a>Limieten voor Web service-aanroep
Azure Resource Manager heeft limieten voor API-aanroepen. U kunt API-aanroepen met een snelheid binnen de [limieten van Azure Resource Manager API](../articles/azure-subscription-service-limits.md#resource-group-limits).
