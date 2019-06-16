---
title: bestand opnemen
description: bestand opnemen
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 5/30/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: ac0747dcff4e74363a58dc9aaf6da4dbd4c6a1c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427605"
---
Azure Data Factory is een multitenant service met de volgende standaardlimieten in plaats om te controleren of abonnementen van de gebruiker zijn beveiligd tegen elkaars werkbelastingen. Als u wilt de limieten tot het maximum voor uw abonnement verhogen, contact op met ondersteuning.

### <a name="version-2"></a>Versie 2

| Resource | Standaardlimiet | Maximumaantal |
| -------- | ------------- | ------------- |
| Data Factory's in een Azure-abonnement | 50 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Totaal aantal entiteiten, zoals pijplijnen, gegevenssets, triggers, gekoppelde services en integratieruntimes in een data factory | 5,000 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Totale CPU-kernen voor Azure-SSIS-Integratieruntimes in één abonnement | 256 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gelijktijdige pijplijnuitvoeringen per gegevensfactory die wordt gedeeld door alle pijplijnen in de fabriek | 10\.000  | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gelijktijdige externe activiteit wordt uitgevoerd per abonnement per [regio voor de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Externe activiteiten in integratieruntime worden beheerd, maar op gekoppelde services, met inbegrip van Databricks, opgeslagen procedure, HDInsights en anderen worden uitgevoerd.</small> | 3000 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gelijktijdige Pipeline-activiteit wordt uitgevoerd per abonnement per [regio voor de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) <br><small>Pipeline-activiteiten worden uitgevoerd op een integratieruntime,-Lookup, GetMetadata, inclusief en verwijderen. </small>| 1000 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gelijktijdige bewerkingen per abonnement per ontwerpen [regio voor de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Voorbeeld van gegevens met inbegrip van de testverbinding, de lijst van de map bladeren en de Tabellijst met. | 200 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Integratie van gelijktijdige gegevenseenheden<sup>1</sup> verbruik per abonnement per [regio voor de Azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)| Regio-groep 1<sup>2</sup>: 6000<br>Regio-groep 2<sup>2</sup>: 3000<br>, Regiogroep 3<sup>2</sup>: 1500 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Maximum aantal activiteiten per pijplijn, waaronder interne activiteiten voor containers | 40 | 40 |
| Maximum aantal gekoppelde integratieruntimes die kunnen worden gemaakt op basis van een enkele zelf-hostende integratieruntime | 100 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Maximum aantal parameters per pijplijn | 50 | 50 |
| ForEach-items | 100\.000 | 100\.000 |
| Parallelle uitvoering ForEach | 20 | 50 |
| Tekens per expressie | 8\.192 | 8\.192 |
| Minimale tumblingvenstertrigger venster trigger interval | 15 min | 15 min |
| Maximale time-out voor pipeline-activiteit wordt uitgevoerd | 7 dagen | 7 dagen |
| Bytes per object voor pijplijnobjecten<sup>3</sup> | 200 KB | 200 KB |
| Bytes per object voor gegevensset en de gekoppelde serviceobjecten<sup>3</sup> | 100 KB | 2000 KB |
| Integratie van gegevenseenheden<sup>1</sup> per uitvoering van activiteit kopiëren | 256 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Schrijven van API-aanroepen | 2\.500/h<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Lezen API-aanroepen | 12\.500/h<br/><br/> Deze limiet is opgelegd door Azure Resource Manager, niet-Azure Data Factory. | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bewaking van query's per minuut | 1000 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Entiteit CRUD-bewerkingen per minuut | 50 | [Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |

<sup>1</sup> integratie gegevenseenheid (DIU) wordt gebruikt in een cloud-naar-cloud kopieerbewerking, meer informatie uit [eenheden van de integratie van gegevens (versie 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units). Zie voor meer informatie over facturering [prijzen voor Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>2</sup> [azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) is [wereldwijd beschikbaar](https://azure.microsoft.com/global-infrastructure/services/) om de gegevensnaleving, efficiëntie en verminderde netwerk uitgaand verkeer kosten. 

| Regio-groep | Regio's | 
| -------- | ------ |
| Regio-groep 1 | VS-midden, VS-Oost, VS-Oost 2, Noord-Europa, West-Europa, VS-West, VS-West 2 |
| Regio-groep 2 | Australië-Oost, Australië-Zuidoost, Brazilië-Zuid, centraal-India, Japan-Oost, Northcentral VS, VS Zuid-centraal, Zuidoost-Azië, West-Centraal VS |
| , Regiogroep 3 | Canada centraal, Zuidoost-Azië, Frankrijk-centraal, Korea centraal, VK Zuid |

<sup>3</sup> pijplijn, gegevens en objecten van de gekoppelde service vertegenwoordigt een logische groepering van uw workload. Limieten voor deze objecten niet hebben betrekking op de hoeveelheid gegevens die u kunt verplaatsen en verwerken met Azure Data Factory. Data Factory is ontworpen om te schalen voor het afhandelen van petabytes aan gegevens.

### <a name="version-1"></a>Versie 1

| **Resource** | **Standaardlimiet** | **Maximumlimiet** |
| --- | --- | --- |
| Data Factory's in een Azure-abonnement |50 |[Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Pijplijnen in een data factory |2,500 |[Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gegevenssets binnen een data factory |5,000 |[Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gelijktijdige segmenten per gegevensset |10 |10 |
| Bytes per object voor pijplijnobjecten<sup>1</sup> |200 KB |200 KB |
| Bytes per object voor gegevens instellen en de gekoppelde serviceobjecten<sup>1</sup> |100 KB |2000 KB |
| Azure HDInsight-cluster op aanvraag kernen binnen een abonnement<sup>2</sup> |60 |[Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Eenheden voor gegevensverplaatsing per exemplaar activiteit die wordt uitgevoerd in de cloud<sup>3</sup> |32 |[Neem contact op met ondersteuning voor](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Aantal van de pijplijnactiviteit nieuwe pogingen |1000 |MaxInt (32 bits) |

<sup>1</sup> pijplijn, gegevens en objecten van de gekoppelde service vertegenwoordigt een logische groepering van uw workload. Limieten voor deze objecten niet hebben betrekking op de hoeveelheid gegevens die u kunt verplaatsen en verwerken met Azure Data Factory. Data Factory is ontworpen om te schalen voor het afhandelen van petabytes aan gegevens.

<sup>2</sup> on-demand HDInsight-cores worden toegewezen uit het abonnement met de data factory. De eerdere limiet is als gevolg hiervan de limiet voor het Data Factory afgedwongen kernen voor on-demand HDInsight-cores. Dit wijkt af van de core-limiet die is gekoppeld aan uw Azure-abonnement.

<sup>3</sup> het hulpprogramma voor het verkeer van de cloud-gegevenseenheid (DMU) voor versie 1 wordt gebruikt in een cloud-naar-cloud kopieerbewerking meer [eenheden voor gegevensverplaatsing (versie 1) in de Cloud](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Zie voor meer informatie over facturering [prijzen voor Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

| **Resource** | **Standaard ondergrens** | **Minimale versie** |
| --- | --- | --- |
| Interval plannen |15 minuten |15 minuten |
| Interval tussen nieuwe pogingen |1 seconde |1 seconde |
| Probeer time-outwaarde |1 seconde |1 seconde |

#### <a name="web-service-call-limits"></a>Limieten voor Web service-aanroep
Azure Resource Manager heeft limieten voor API-aanroepen. U kunt API-aanroepen met een snelheid binnen de [limieten van Azure Resource Manager API](../articles/azure-subscription-service-limits.md#resource-group-limits).
