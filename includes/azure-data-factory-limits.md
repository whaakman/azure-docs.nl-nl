---
title: bestand opnemen
description: bestand opnemen
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 05/16/2018
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 63653795ad8c52e2743fb02fa804dd2edbf0d2ab
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371317"
---
Gegevensfactory is een multitenant-service die beschikt over de volgende standaardlimiet om ervoor te zorgen klantabonnementen zijn beveiligd tegen elkaars werkbelastingen. Veel van de limieten kunnen eenvoudig gegeven voor uw abonnement tot het maximum aantal contact opnemen met de ondersteuning.

### <a name="version-2"></a>Versie 2

| Resource | Standaardlimiet | Maximumaantal | 
| -------- | ------------- | ------------- | 
| Data Factory's in een Azure-abonnement | 50 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| pijplijnen binnen een gegevensfactory | 2500 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| gegevenssets binnen een gegevensfactory | 2500 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Triggers binnen een gegevensfactory | 2500 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gekoppelde services binnen een gegevensfactory | 2500 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Integratie runtimes binnen een gegevensfactory <sup>4</sup> | 2500 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Totale CPU-kernen voor Azure SSIS-integratie Runtime(s) onder één abonnement | 100 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gelijktijdige pipeline wordt per pijplijn | 100 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Maximum aantal activiteiten per pijplijn | 20 | 40 |
| Maximum aantal parameters per pijplijn | 20 | 30 |
| Aantal bytes per object voor pipeline-objecten <sup>1</sup> | 200 KB | 200 KB |
| Aantal bytes per object voor de gegevensset en objecten van de gekoppelde service <sup>1</sup> | 100 KB | 2000 KB |
| Cloud data movement eenheden per activiteit die wordt uitgevoerd <sup>3</sup> | 256 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Aantal voor pijplijn activiteiten bij uitvoering nieuwe pogingen | 1 day(timeout) | 1 dag (time-out) |
| API-aanroepen schrijven | 2500/hr<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Neem contact op met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Lees API-aanroepen | 12.500/hr<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>Versie 1

| **Resource** | **Standaardlimiet** | **Maximumaantal** |
| --- | --- | --- |
| Data Factory's in een Azure-abonnement |50 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| pijplijnen binnen een gegevensfactory |2500 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| gegevenssets binnen een gegevensfactory |5000 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| gelijktijdige segmenten per gegevensset |10 |10 |
| Aantal bytes per object voor pipeline-objecten <sup>1</sup> |200 KB |200 KB |
| Aantal bytes per object voor de gegevensset en objecten van de gekoppelde service <sup>1</sup> |100 KB |2000 KB |
| HDInsight-cluster op aanvraag kernen binnen een abonnement <sup>2</sup> |60 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Cloud data movement eenheden per activiteit die wordt uitgevoerd <sup>3</sup> |32 |[Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Aantal voor pijplijn activiteiten bij uitvoering nieuwe pogingen |1000 |MaxInt (32 bits) |

<sup>1</sup> pipeline, gegevensset en objecten van de gekoppelde service vertegenwoordigen een logische groepering van uw workload. Limieten voor deze objecten geen betrekking hebben op de hoeveelheid gegevens die u kunt verplaatsen en met de Azure Data Factory-service verwerken. Gegevensfactory is ontworpen om te schalen voor petabytes aan gegevens.

<sup>2</sup> on-demand HDInsight-kernen worden toegewezen uit het abonnement dat u de gegevensfactory bevat. Als gevolg hiervan is de bovenstaande limiet voor de Data Factory afgedwongen limiet kernen voor bellen op HDInsight kernen en verschilt van de core limiet die is gekoppeld aan uw Azure-abonnement.

<sup>3</sup> cloud gegevensverplaatsing gegevenseenheid (DMU) wordt gebruikt in een cloud-naar-cloud kopieerbewerking. Is een meting met de kracht (een combinatie van CPU, geheugen en netwerkresourcetoewijzing) van één eenheid in de Data Factory. Met behulp van meer DMUs voor sommige scenario's kunt u hogere kopie doorvoer behalen. Raadpleeg [Cloud data movement eenheden (V2)](../articles/data-factory/copy-activity-performance.md#cloud-data-movement-units) en [Cloud data movement eenheden (V1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) sectie voor meer informatie.

<sup>4</sup> de integratie Runtime (IR) wordt de beheerinfrastructuur gebruikt door Azure Data Factory om te voorzien in de volgende mogelijkheden voor integratie van verschillende netwerkomgevingen: verplaatsing van gegevens tijdens het verzenden van activiteiten voor de compute-services, uitvoeren van SSIS-pakketten. Zie voor meer informatie [integratie Runtime overzicht](../articles/data-factory/concepts-integration-runtime.md).

| **Resource** | **De ondergrens standaard** | **Minimale limiet** |
| --- | --- | --- |
| Plannen van interval |15 minuten |15 minuten |
| Interval tussen nieuwe pogingen |1 seconde |1 seconde |
| Probeer de time-outwaarde |1 seconde |1 seconde |

#### <a name="web-service-call-limits"></a>Limieten voor aanroep van Web service
Azure Resource Manager heeft limieten voor API-aanroepen. U kunt de API-aanroepen maken met een frequentie binnen de [Azure Resource Manager-API beperkt](../articles/azure-subscription-service-limits.md#resource-group-limits).
