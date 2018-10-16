---
title: Overzicht van Azure Resource Graph
description: Azure Resource Graph is een service in Azure waarmee op schaal complexe query's op resources kunnen worden uitgevoerd.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d68183f4d0a928ac72f3f73ea5225ad174820cb7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162097"
---
# <a name="what-is-azure-resource-graph"></a>Wat is Azure Resource Graph

Azure Resource Graph is een service in Azure die is ontworpen om Azure Resource Management uit te breiden met efficiënte en krachtige resourceverkenning en de mogelijkheid om query's op grote schaal uit te voeren binnen alle abonnementen en beheergroepen, zodat u uw omgeving effectief kunt beheren. Deze query's bieden de volgende mogelijkheden:

- De mogelijkheid om resources op te vragen met geavanceerde opties voor filteren, groeperen en sorteren op resource-eigenschappen.
- De mogelijkheid om resources iteratief te verkennen op basis van governancevereisten en de resulterende expressie te converteren naar een beleidsdefinitie.
- De mogelijkheid om de impact van het toepassen van een beleid in een grote cloudomgeving te beoordelen.

In deze documentatie komt elke mogelijkheid gedetailleerd aan bod.

> [!NOTE]
> Azure Resource Graph wordt gebruikt door de nieuwe bladerervaring Alle resources van de Azure Portal. Deze is ontworpen voor klanten die grootschalige omgevingen moeten beheren.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Hoe Resource Graph Azure Resource Manager aanvult

Azure Resource Manager verzendt momenteel gegevens naar een beperkte resourcecache die verschillende resourcevelden beschikbaar maakt, met name Resourcenaam, Id, Type, Resourcegroep, Abonnementen en Locatie. Als u nu met meer resource-eigenschappen wilt werken, moet u elke afzonderlijke resourceprovider afzonderlijk aanroepen en de eigenschapsdetails voor elke resource afzonderlijk opvragen.

Met Azure Resource Graph hebt u toegang tot de eigenschappen die de resourceproviders retourneren zonder dat u elke resourceprovider afzonderlijk moet aanroepen.

## <a name="the-query-language"></a>De querytaal

Nu u weet wat Azure Resource Graph is, leert u hoe u query's kunt maken.

Het is belangrijk te weten dat de querytaal van Azure Resource Graph is gebaseerd op de [querytaal van Azure Data Explorer](../../data-explorer/data-explorer-overview.md).

Voor meer informatie over bewerkingen en functies die kunnen worden gebruikt met Azure Resource Graph raadpleegt u eerst [De querytaal van Resource Graph](./concepts/query-language.md). Zie [Resources verkennen](./concepts/explore-resources.md) als u de resources wilt bekijken.

## <a name="permissions-in-azure-resource-graph"></a>Machtigingen in Azure Resource Graph

Om Resource Graph te kunnen gebruiken, moet u gemachtigd zijn via [Op rollen gebaseerd toegangsbeheer](../../role-based-access-control/overview.md) (RBAC), met minimaal leestoegang tot de resources die u wilt zoeken. Als u geen `read`-machtigingen hebt voor de beheergroep, het abonnement, de resourcegroep of afzonderlijke resources, worden deze niet geretourneerd in de resultaten van een Resource Graph-query.

## <a name="running-your-first-query"></a>Uw eerste query uitvoeren

Resource Graph ondersteunt zowel Azure CLI als Azure PowerShell. De structuur van het queryonderdeel is hetzelfde, ongeacht welke taal er wordt gebruikt. Ondersteuning voor Azure Resource Graph is nog niet standaard beschikbaar in de SDK. Er moet dus een extensie of module worden geladen om de benodigde opdrachten te kunnen verstrekken.
Meer informatie over het inschakelen van Resource Graph in [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) en [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Volgende stappen

- Uw eerste query uitvoeren met [Azure CLI](first-query-azurecli.md)
- Uw eerste query uitvoeren met [Azure PowerShell](first-query-powershell.md)
- Beginnen met [Starter query's](./samples/starter.md)
- Uw kennis vergroten met [Geavanceerde query's](./samples/advanced.md)