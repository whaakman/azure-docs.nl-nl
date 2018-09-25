---
title: Inzicht krijgen in de querytaal van Azure Resource Graph
description: Hierin wordt beschreven hoe de querytaal van Azure Resource Graph werkt.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 3600fc47a0fb318a49c1b37722cb7fffa51ec6f2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951947"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Inzicht krijgen in de querytaal van Azure Resource Graph

De querytaal voor de Azure Resource Graph ondersteunt een aantal operators en -functies. Elke werken en vergelijkbare naar de Kusto Query Language (KQL) werken. Het is echter belangrijk te begrijpen dat tijdens de querytaal van de grafiek van de Resource vergelijkbaar met is [KQL](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators), is niet hetzelfde.

> [!NOTE]
> Koppelingen naar documentatie KQL kan verificatie vereist.

De beste manier om meer informatie over de querytaal die wordt gebruikt door Resource Graph is om te beginnen met de documentatie voor KQL om op te halen van een goed begrip over hoe de taal is opgebouwd en hoe de verschillende operators ondersteund en functies samenwerken.

## <a name="supported-tabular-operators"></a>Ondersteunde in tabelvorm operators

Hier volgt de lijst met ondersteunde in tabelvorm operators in Resource-grafiek:

- [Afzonderlijke](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator)
- [Uitbreiden](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator)
- [Limiet](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/limit-operator)
- [order by-](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/order-operator)
- [Project](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator)
- [Project-opslag](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-away-operator)
- [Voorbeeld](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sample-operator)
- [Voorbeeld van afzonderlijke](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sample-distinct-operator)
- [sorteren op](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator)
- [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator)
- [toets maken](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
- [Boven](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator)
- [Top-geneste](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-nested-operator)
- [Top-hitters](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-hitters-operator)
- [waar](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

## <a name="supported-functions"></a>Ondersteunde functies

Hier volgt de lijst met ondersteunde functies in Resource-grafiek:

- [ago()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago%28%29)
- [buldschema()](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/buildschema%28%29)
- [Count()](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/count%28%29)
- [strcat()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/strcat%28%29)
- [isnotempty()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/isnotempty%28%29_-notempty%28%29)
- [toString()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/tostring%28%29)

## <a name="next-steps"></a>Volgende stappen

- Zie de taal die wordt gebruikt in [Starter query's](../samples/starter.md)
- Maakt gebruik van geavanceerde Zie in [geavanceerde query's](../samples/advanced.md)
- Meer informatie over het [resources verkennen](explore-resources.md)