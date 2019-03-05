---
title: Inzicht in de querytaal
description: Beschrijft de beschikbare Kusto-operators en kan worden gebruikt met Azure Resource Graph-functies.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/11/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 618445c5b792317d4de4b668e7ea1a186707007c
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57317914"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Inzicht krijgen in de querytaal van Azure Resource Graph

De querytaal voor de Azure Resource Graph ondersteunt een aantal operators en -functies. Elk werken en uitvoeren op basis van [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md).

De beste manier om meer informatie over de querytaal die wordt gebruikt door Resource Graph is om te beginnen met de documentatie voor Azure Data Explorer [querytaal](/azure/kusto/query/index). Het biedt een goed begrip over hoe de taal is opgebouwd en hoe de verschillende operators ondersteund en functies samenwerken.

## <a name="supported-tabular-operators"></a>Ondersteunde in tabelvorm operators

Hier volgt de lijst met ondersteunde in tabelvorm operators in Resource-grafiek:

- [count](/azure/kusto/query/countoperator)
- [distinct](/azure/kusto/query/distinctoperator)
- [Uitbreiden](/azure/kusto/query/extendoperator)
- [Limiet](/azure/kusto/query/limitoperator)
- [order by-](/azure/kusto/query/orderoperator)
- [project](/azure/kusto/query/projectoperator)
- [project-away](/azure/kusto/query/projectawayoperator)
- [sample](/azure/kusto/query/sampleoperator)
- [sample-distinct](/azure/kusto/query/sampledistinctoperator)
- [sorteren op](/azure/kusto/query/sortoperator)
- [samenvatten](/azure/kusto/query/summarizeoperator)
- [take](/azure/kusto/query/takeoperator)
- [Boven](/azure/kusto/query/topoperator)
- [top-nested](/azure/kusto/query/topnestedoperator)
- [Top-hitters](/azure/kusto/query/tophittersoperator)
- [waar](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Ondersteunde functies

Hier volgt de lijst met ondersteunde functies in Resource-grafiek:

- [ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [tostring()](/azure/kusto/query/tostringfunction)
- [zip()](/azure/kusto/query/zipfunction)

## <a name="next-steps"></a>Volgende stappen

- Zie de taal die wordt gebruikt in [Starter query's](../samples/starter.md)
- Maakt gebruik van geavanceerde Zie in [geavanceerde query's](../samples/advanced.md)
- [Resources verkennen](explore-resources.md)