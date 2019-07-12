---
title: Klanten en gedelegeerde resources in Azure portal weergeven en beheren
description: Als een serviceprovider met behulp van Azure Resourcemanagement gedelegeerd, kunt u al uw resources van de klant gedelegeerd en -abonnementen weergeven door te gaan naar mijn klanten in de Azure-portal.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: acc90afa258fa7140cd7dfa8711dd64b554df45d
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809853"
---
# <a name="view-and-manage-customers-and-delegated-resources"></a>Klanten en gedelegeerde resources weergeven en beheren

Met behulp van serviceproviders [Azure Resourcemanagement overgedragen](../concepts/azure-delegated-resource-management.md) kunt gebruiken de **mijn klanten** pagina in de [Azure-portal](https://portal.azure.com) om resources van de gedelegeerde klant weer te geven en abonnementen. Hoewel we naar serviceproviders en klanten hier verwijzen, kunnen ondernemingen die meerdere tenants beheren hetzelfde proces gebruiken om te consolideren van hun ervaring.

Toegang krijgen tot de **mijn klanten** pagina in de Azure portal, selecteer **alle services**, zoek vervolgens naar **mijn klanten** en selecteer deze. U kunt deze ook vinden door te voeren 'Mijn klanten' in het zoekvak boven aan de Azure-portal.

Houd er rekening mee dat de **mijn klanten** pagina bevat alleen informatie over klanten die-abonnementen of resourcegroepen gedelegeerde. Als u met andere klanten werkt (zoals via de [programma Cloud Solution Provider](https://docs.microsoft.com/partner-center/csp-overview), informatie over de klanten die hier niet worden weergegeven, tenzij u onboarding hun resources voor gedelegeerd resourcebeheer van.

> [!NOTE]
> Uw klanten kunnen informatie over serviceproviders weergeven door te navigeren naar **serviceproviders** in Azure portal. Zie voor meer informatie, [weergeven en beheren van serviceproviders](view-manage-service-providers.md).

## <a name="view-and-manage-customer-details"></a>Weergeven en beheren van klantgegevens

Als u wilt weergeven van details van de klant, selecteer **klanten** aan de linkerkant van de **mijn klanten** pagina.

Voor elke klant ziet u de naam van de klant, klant-ID (tenant-ID) en de aanbieding die zijn gekoppeld aan de engagement. In de **overdrachten** kolom ziet u het aantal overgedragen abonnementen en/of het aantal overgedragen resourcegroepen.

Filters aan de bovenkant van de pagina kunt u sorteren en groeperen uw klantgegevens of filteren op specifieke klanten, aanbiedingen of trefwoorden.

U kunt de volgende gegevens op deze pagina bekijken:

- Als u wilt zien van alle abonnementen, biedt en overdrachten die zijn gekoppeld aan een klant, selecteert u de naam van de klant.
- Voor meer informatie over een aanbieding en de overdrachten, selecteert u de aanbiedingsnaam van de.
- Als u meer details over acrolecess toewijzingen voor gedelegeerde-abonnementen of resourcegroepen, selecteer de vermelding in de **overdrachten** kolom.

## <a name="view-delegations"></a>Weergave-overdrachten

Overdrachten tonen de is overgedragen, samen met de gebruikers en machtigingen die toegang tot het hebben abonnement-/ resourcegroep. Als u wilt deze gegevens weergeven, selecteert u **overdrachten** aan de linkerkant van de **mijn klanten** pagina.

Filters aan de bovenkant van de pagina kunt u sorteren en groeperen uw toewijzing toegangsgegevens of filteren op specifieke klanten, aanbiedingen of trefwoorden.

De gebruikers en machtigingen die zijn gekoppeld aan elke delegatie weergegeven in de **roltoewijzingen** kolom. U kunt elk item om weer te geven van de volledige lijst met gebruikers, groepen en service-principals die zijn verleend aan de toegang tot het abonnement of resourcegroep selecteren. Van daaruit kunt u een bepaalde gebruiker, groep of service-principal-naam voor meer informatie.

## <a name="work-in-the-context-of-a-delegated-subscription"></a>Werkt in de context van een gedelegeerde abonnement

Rechtstreeks in de context van een gedelegeerde abonnement in Azure portal, kunt u werken zonder dat de map die u werkt in te schakelen. Dit doet u als volgt:

1. Selecteer de **map + abonnement** pictogram aan de bovenkant van de Azure-portal.
2. In de **globaal abonnementfilter** filteren, zorg ervoor dat alleen het vak voor dat overgedragen aan het abonnement is geselecteerd. U kunt de **huidige + gedelegeerde mappen** vervolgkeuzelijst om weer te geven alleen abonnementen binnen een specifieke map. (Gebruik niet de **schakelen tussen mappen** optie, omdat de wijzigingen die de map waaraan u bent aangemeld.)

Als u vervolgens toegang tot een service die ondersteuning biedt voor [ervaringen voor cross-tenant](../concepts/cross-tenant-management-experience.md), de service wordt standaard de context van de gedelegeerde abonnement dat u hebt geselecteerd. U kunt dit wijzigen door de stappen hierboven en controleren of de **Alles selecteren** vak (of een of meer abonnementen om te werken in in plaats daarvan te kiezen).

> [!NOTE]
> Als u hebt gekregen tot een of meer resourcegroepen, in plaats van toegang tot een hele abonnement, kunt u het abonnement waartoe deze resourcegroep behoort. U gaat vervolgens gebruiken in de context van het abonnement, maar worden pas toegang tot de aangewezen resourcegroepen.

U kunt ook toegang tot de functionaliteit die betrekking hebben op gedelegeerde-abonnementen of resourcegroepen uit binnen de services die ondersteuning bieden voor cross-tenant-ervaringen door het selecteren van het abonnement of de resource-groep in die service.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ervaringen voor cross-tenant](../concepts/cross-tenant-management-experience.md).
- Informatie over hoe uw klanten kunnen [weergeven en beheren van serviceproviders](view-manage-service-providers.md) door te gaan naar **serviceproviders** in Azure portal.
