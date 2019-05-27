---
title: Overzicht van Azure Resource Graph
description: Begrijpen hoe de Azure Resource Graph-service mogelijk maakt complexe query's uitvoeren met resources op schaal.
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/06/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 9d3385b688208065e5854b6358819b5afad8fe65
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66162083"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Overzicht van de Azure Resource Graph-service

Azure Resource Graph is een service in Azure die is ontworpen voor het uitbreiden van Azure Resource Management biedt efficiënte en goed presterende resource verkennen met de mogelijkheid om query's op grote schaal een bepaalde set abonnementen zodat u effectief kunt beheren uw -omgeving. Deze query's bieden de volgende mogelijkheden:

- De mogelijkheid om resources op te vragen met geavanceerde opties voor filteren, groeperen en sorteren op resource-eigenschappen.
- De mogelijkheid om resources op basis van de vereisten voor iteratief te verkennen.
- De mogelijkheid om de impact van het toepassen van een beleid in een grote cloudomgeving te beoordelen.
- Mogelijkheid om te [informatie over wijzigingen in de resource-eigenschappen](./how-to/get-resource-changes.md) (preview).

In deze documentatie komt elke mogelijkheid gedetailleerd aan bod.

> [!NOTE]
> Azure Resource-grafiek wordt gebruikt door de zoekbalk van Azure portal, de nieuwe Bladeren ' alle bronnen ' en Azure-beleid [wijzigingsoverzicht](../policy/how-to/determine-non-compliance.md#change-history-preview)
> _visual diff_. Het is ontworpen om u te helpen klanten grootschalige omgevingen beheren.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Hoe Resource Graph Azure Resource Manager aanvult

Azure Resource Manager ondersteunt momenteel query's via eenvoudige bron velden, specifiek - Resource naam-ID, Type, resourcegroep, abonnement en locatie. Resource Manager biedt ook faciliteiten voor het aanroepen van afzonderlijke resourceproviders voor gedetailleerde eigenschappen één resource tegelijkertijd.

Met Azure Resource Graph hebt u toegang tot de eigenschappen die de resourceproviders retourneren zonder dat u elke resourceprovider afzonderlijk moet aanroepen. Zoek voor een lijst van ondersteunde resourcetypen, een **Ja** in de [Resources voor volledige-implementaties](../../azure-resource-manager/complete-mode-deletion.md) tabel.

Met Azure Resource Graph, kunt u het volgende doen:

- Toegang tot de eigenschappen die zijn geretourneerd door de resourceproviders zonder afzonderlijke aanroepen naar elke resourceprovider.
- Weergeven van de afgelopen 14 dagen van de wijzigingsgeschiedenis die zijn aangebracht in de bron om te zien wat er eigenschappen gewijzigd en wanneer. (preview-versie)

## <a name="how-resource-graph-is-kept-current"></a>Hoe Resourcegrafiek wordt bijgewerkt

Wanneer een Azure-resource wordt bijgewerkt, Resource Graph is op de hoogte gesteld door Resource Manager van de wijziging.
Grafiek van resources werkt vervolgens de database. Grafiek van resources is ook een gewone _volledige scan_. Deze scan zorgt ervoor dat Resource de grafiekgegevens zijn huidige als er gemiste meldingen of wanneer een resource buiten Resource Manager wordt bijgewerkt.

## <a name="the-query-language"></a>De querytaal

Nu u weet wat Azure Resource Graph is, leert u hoe u query's kunt maken.

Het is belangrijk te weten dat de querytaal van Azure Resource Graph is gebaseerd op de [querytaal van Kusto](../../data-explorer/data-explorer-overview.md) die wordt gebruikt door Azure Data Explorer.

Voor meer informatie over bewerkingen en functies die kunnen worden gebruikt met Azure Resource Graph raadpleegt u eerst [De querytaal van Resource Graph](./concepts/query-language.md).
Zie [Resources verkennen](./concepts/explore-resources.md) als u de resources wilt bekijken.

## <a name="permissions-in-azure-resource-graph"></a>Machtigingen in Azure Resource Graph

Om Resource Graph te kunnen gebruiken, moet u over de juiste machtigingen beschikken in [Op rollen gebaseerd toegangsbeheer](../../role-based-access-control/overview.md) (RBAC), met minimaal leestoegang tot de resources die u wilt zoeken. Zonder ten minste `read`-machtigingen voor het Azure-object of de objectgroep worden er geen resultaten geretourneerd.

> [!NOTE]
> Grafiek van resources maakt gebruik van de abonnementen die beschikbaar zijn voor een principal tijdens het aanmelden. Als u wilt zien van resources van een nieuw abonnement gedurende een actieve sessie toegevoegd, moet de principal de context vernieuwen. Deze actie wordt automatisch uitgevoerd wanneer u zich af te melden en opnieuw aanmelden.

## <a name="throttling"></a>Beperken

Als een gratis service, worden query's voor Resource Graph beperkt voor de beste ervaring en antwoord-tijd voor alle klanten. Als uw organisatie wil de Resource Graph API gebruiken voor grootschalige en regelmatig query's, gebruiken die portal 'Feedback' in de [Resource Graph-portalpagina](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/ResourceGraph).
Geef uw bedrijfsscenario en schakelt u het selectievakje 'Microsoft kunt u een e-mail over uw feedback, zodat het team contact met u opnemen.

Grafiek van resources beperkt query's op het gebruikersniveau van de. Antwoord van de service bevat de volgende HTTP-headers:

- `x-ms-user-quota-remaining` (int): Het resterende resourcequotum voor de gebruiker. Deze waarde wordt toegewezen aan het aantal query's.
- `x-ms-user-quota-resets-after` het uu: mm (:): De tijdsduur van de totdat van een gebruiker quotumverbruik wordt opnieuw ingesteld

Zie voor meer informatie, [beperking Resource Manager-aanvragen](../../azure-resource-manager/resource-manager-request-limits.md).

## <a name="running-your-first-query"></a>Uw eerste query uitvoeren

Resourcegrafiek biedt ondersteuning voor Azure CLI, Azure PowerShell en Azure SDK voor .NET. De query is opgebouwd hetzelfde voor elke taal. Meer informatie over het inschakelen van Resource Graph in [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) en [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Volgende stappen

- Voer uw eerste query's uitvoeren met [Azure CLI](first-query-azurecli.md).
- Voer uw eerste query's uitvoeren met [Azure PowerShell](first-query-powershell.md).
- Beginnen met [Starter query's](./samples/starter.md).
- Vergroot u uw kennis met [geavanceerde query's](./samples/advanced.md).