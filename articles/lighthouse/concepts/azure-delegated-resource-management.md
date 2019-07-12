---
title: Azure Resourcemanagement - gedelegeerd naar Azure
description: Beheerde services aanbiedingen kunnen serviceproviders resource management aanbiedingen aan klanten in Azure Marketplace verkopen.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: cec6453cdf339e82420a1b12af6c8e60526fdc03
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809965"
---
# <a name="azure-delegated-resource-management"></a>Azure-gedelegeerde Resourcemanagement

Azure-gedelegeerde Resourcemanagement is een van de belangrijkste onderdelen van Azure naar. Met Azure gedelegeerde resource Manager, kunnen serviceproviders customer engagement en onboarding is en de gedelegeerde resources op schaal beheren met meer flexibiliteit en precisie vereenvoudigen.

## <a name="what-is-azure-delegated-resource-management"></a>Wat is Azure Resourcemanagement overgedragen?

Azure-gedelegeerde Resourcemanagement kunt logische projectie van resources van één tenant naar een andere tenant. Hiermee kunnen geautoriseerde gebruikers in een tenant van Azure Active Directory (Azure AD) beheerbewerkingen uitvoeren voor andere Azure AD tenants die horen bij hun klanten. Serviceproviders kunnen zich aanmelden bij hun eigen Azure AD-tenant en gemachtigd om te werken in gedelegeerde klantabonnementen en resourcegroepen. Hiermee kunt u beheertaken namens hun klanten uitvoeren zonder te hoeven aanmelden bij elke tenant afzonderlijke klant.

> [!NOTE]
> Azure-resource gedelegeerd beheer kan ook worden gebruikt in een onderneming met meerdere Azure AD-tenants van de eigen voor het vereenvoudigen van beheer cross-tenant.

Geautoriseerde gebruikers kunnen met Azure gedelegeerde resource Manager, rechtstreeks in de context van een klantabonnement werken zonder dat een account in de tenant van de klant of een mede-eigenaar van de tenant van de klant wordt. Ze kunnen ook [weergeven en beheren van alle abonnementen van de gedelegeerde gebruiker in de nieuwe **mijn klanten** pagina](../how-to/view-manage-customers.md) in Azure portal.

De [cross-tenant beheerervaring](cross-tenant-management-experience.md) helpt u efficiënter gebruik van Azure management-services zoals Azure Policy, Azure Security Center en meer. Alle provider serviceactiviteit wordt bijgehouden in het activiteitenlogboek, dat is opgeslagen in zowel de serviceprovider en van de klant tenants. Dit betekent dat de klant en de service provider de gebruiker die is gekoppeld met de wijzigingen eenvoudig kunt herkennen.

Wanneer u onboarding een klant naar Azure gedelegeerd voor het resourcebeheer, ze toegang hebben tot de nieuwe **serviceproviders** pagina in de Azure portal, waar ze kunnen [controleren en beheren van hun aanbiedingen, serviceproviders, en resources overgedragen](../how-to/view-manage-service-providers.md). Als de klant wil ooit de toegang intrekken voor een serviceprovider, kunnen ze dat hier doen op elk gewenst moment.

U kunt [het type van de nieuwe beheerde Services aanbieding publiceren op Azure Marketplace](../how-to/publish-managed-services-offers.md) overgedragen eenvoudig om klanten te onboarden naar Azure resourcemanagement. U kunt ook [het onboarding-proces voltooien door het implementeren van Azure Resource Manager-sjablonen](../how-to/onboard-customer.md).

## <a name="how-azure-delegated-resource-management-works"></a>Hoe Azure Resourcemanagement overgedragen werkt

Op hoog niveau is dit hoe Azure resource management werkt overgedragen:

1. Als een serviceprovider, moet u de toegang (rollen) die uw groepen, service-principals of gebruikers voor het beheren van Azure-resources van de klant moet identificeren. De definitie van de toegang tot de tenant-ID van de serviceprovider, samen met de vereiste toegang voor de aanbieding, gedefinieerd met behulp van bevat **principalId** identiteiten van uw tenant die is toegewezen aan [ingebouwde  **roleDefinition** waarden](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) (Inzender, VM-Inzender, lezer, enz.).
2. U opgeven dat deze toegang en onboarding de klant naar Azure Resourcemanagement op twee manieren overgedragen:
   - [Een aanbieding van Azure Marketplace beheerde services publiceren](../how-to/publish-managed-services-offers.md) (particulier of openbaar) waarmee de klant worden geaccepteerd
   - [Een Azure Resource Manager-sjabloon implementeren in de tenant van de klant](../how-to/onboard-customer.md) voor een of meer specifieke abonnementen of resourcegroepen
3. Nadat de klant toegevoegd is, kunnen gemachtigde gebruikers zich aanmelden bij uw serviceprovider-tenant en beheertaken uitvoeren op het bereik van bepaalde klant, op basis van de toegang die u hebt gedefinieerd.

## <a name="support-for-azure-delegated-resource-management"></a>Ondersteuning voor Azure overgedragen Resourcemanagement

Als u hulp met betrekking tot Azure Resourcemanagement overgedragen, kunt u een ondersteuningsaanvraag openen in de Azure-portal. Voor **type probleem**, kiest u **technische**. Selecteer een abonnement en selecteer vervolgens **gedelegeerde Resource Management** (onder **controle en beheer**).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ervaringen voor cross-tenant](cross-tenant-management-experience.md).
- Meer informatie over [managed services-aanbiedingen op Azure Marketplace](managed-services-offers.md).