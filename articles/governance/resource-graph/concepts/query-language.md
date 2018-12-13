---
title: Inzicht in de querytaal
description: Hierin wordt beschreven hoe de querytaal van Azure Resource Graph werkt.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/11/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 62f61bfea3896fd3828253f5ec16cc38fe3ca007
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316659"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Inzicht krijgen in de querytaal van Azure Resource Graph

De querytaal voor de Azure Resource Graph ondersteunt een aantal operators en -functies. Elk werken en uitvoeren op basis van [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md).

De beste manier om meer informatie over de querytaal die wordt gebruikt door Resource Graph is om te beginnen met de documentatie voor Azure Data Explorer [querytaal](/azure/kusto/query/index). Het biedt een goed begrip over hoe de taal is opgebouwd en hoe de verschillende operators ondersteund en functies samenwerken.

## <a name="supported-tabular-operators"></a>Ondersteunde in tabelvorm operators

Hier volgt de lijst met ondersteunde in tabelvorm operators in Resource-grafiek:

- [count](/azure/kusto/query/countoperator)
- [Afzonderlijke](/azure/kusto/query/distinctoperator)
- [Uitbreiden](/azure/kusto/query/extendoperator)
- [Limiet](/azure/kusto/query/limitoperator)
- [order by-](/azure/kusto/query/orderoperator)
- [Project](/azure/kusto/query/projectoperator)
- [Project-opslag](/azure/kusto/query/projectawayoperator)
- [Voorbeeld](/azure/kusto/query/sampleoperator)
- [Voorbeeld van afzonderlijke](/azure/kusto/query/sampledistinctoperator)
- [sorteren op](/azure/kusto/query/sortoperator)
- [samenvatten](/azure/kusto/query/summarizeoperator)
- [toets maken](/azure/kusto/query/takeoperator)
- [Boven](/azure/kusto/query/topoperator)
- [Top-geneste](/azure/kusto/query/topnestedoperator)
- [Top-hitters](/azure/kusto/query/tophittersoperator)
- [waar](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Ondersteunde functies

Hier volgt de lijst met ondersteunde functies in Resource-grafiek:

- [ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [toString()](/azure/kusto/query/tostringfunction)
- [ZIP()](/azure/kusto/query/zipfunction)

## <a name="next-steps"></a>Volgende stappen

- Zie de taal die wordt gebruikt in [Starter query's](../samples/starter.md)
- Maakt gebruik van geavanceerde Zie in [geavanceerde query's](../samples/advanced.md)
- [Resources verkennen](explore-resources.md)
