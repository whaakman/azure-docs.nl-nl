---
title: Azure Lighthouse en het programma Cloud Solution Provider
description: Wanneer met behulp van Azure Resourcemanagement overgedragen, is het belangrijk dat beveiliging en toegangsbeheer.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 09552c66d2dc841cff8b5cb52e26dc657568e08f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809958"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse en het programma Cloud Solution Provider

Als u bent een [CSP (Cloud Solution Provider)](https://docs.microsoft.com/partner-center/csp-overview) partner, u hebt toegang tot de Azure-abonnementen voor uw klanten via het CSP-programma gemaakt met behulp van de [beheren namens (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) functionaliteit. Deze toegang kunt u rechtstreeks ondersteuning, configureren en beheren van abonnementen voor uw klanten.

Het mechanisme voor AOBO, hebben volledige toegang tot de omgevingen van de klant. Met behulp van Azure-gedelegeerde Resourcemanagement samen met AOBO helpt de beveiliging verbeteren doordat u overbodige toegang beperken door in te schakelen gedetailleerdere machtigingen voor uw gebruikers. 

## <a name="administer-on-behalf-of-aobo"></a>Beheer namens (AOBO)

Met AOBO, elke gebruiker met de [Admin Agent](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) rol in uw tenant heeft AOBO toegang tot Azure-abonnementen die u hebt gemaakt via het CSP-programma. Alle gebruikers die toegang nodig tot alle klanten abonnementen moet een lid van deze groep. AOBO toegestaan niet in de flexibiliteit om verschillende groepen die met verschillende klanten werken te maken of om in te schakelen van groepen of gebruikers met verschillende rollen.

![Beheer van tenants met behulp van AOBO](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Azure-gedelegeerde Resourcemanagement

Met behulp van Azure Resourcemanagement overgedragen, kunt u verschillende groepen toewijzen aan verschillende klanten of rollen, zoals wordt weergegeven in het volgende diagram. Omdat gebruikers het juiste niveau van toegang via Azure gedelegeerde Resourcemanagement, kunt u het aantal gebruikers die de rol van beheerder-Agent (en dus hebben volledige toegang tot de AOBO) verminderen. Dit biedt een betere beveiliging door onnodige toegang tot resources van uw klanten te beperken. Het biedt ook meer flexibiliteit voor het beheren van meerdere klanten op schaal.

Onboarding van een abonnement dat u hebt gemaakt via het CSP-programma in de volgende stappen wordt beschreven in [onboarding een abonnement op Azure Resourcemanagement overgedragen](../how-to/onboard-customer.md). Elke gebruiker die de rol Admin-Agent in uw tenant, kan deze onboarding uitvoeren.

Houd er rekening mee dat voor abonnementen die zijn gemaakt via de CSP-programma's, ondersteuningsaanvragen kunnen alleen worden gegenereerd door gebruikers met de rol Admin-Agent in de tenant van de serviceprovider. Gebruikers die zijn toegevoegd via Azure gedelegeerde Resourcemanagement niet mogelijk om te openen voor ondersteuningsaanvragen voor gedelegeerde resources in deze abonnementen.

![Beheer van tenants met AOBO en Azure Resourcemanagement overgedragen](../media/csp-2.jpg)

## <a name="partner-admin-link"></a>Koppeling van de beheerder van partners

U kunt uw Microsoft Partner Network (MPN)-ID koppelen aan uw onboarding-abonnementen voor het bijhouden van uw invloed op klant-engagements.

Als u [een aanbieding van beheerde services publiceren naar de Azure Marketplace](../how-to/publish-managed-services-offers.md), uw MPN-ID is gekoppeld aan uw uitgeverprofiel en is automatisch gekoppeld aan de aanbieding. Opbrengst van Azure-resources via deze aanbieding wordt vervolgens worden toegeschreven aan uw organisatie. In de rapportage van systemen, zoals het Partnercentrum of MPN-partner, weergegeven de attribution als Partner Admin koppeling (PAL).

Als u [Onboarding van klanten voor Azure Resourcemanagement met behulp van Azure Resource Manager-sjablonen overgedragen](../how-to/onboard-customer.md), u kunt nog steeds uw MPN ID in om te kunnen ontvangen van erkenning voor uw invloed op klant-engagements koppelen, maar hebt u op dit handmatig worden gedaan. Zie voor meer informatie, [een partner-ID koppelen aan uw Azure-accounts](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started). 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ervaringen voor cross-tenant](cross-tenant-management-experience.md).
- Meer informatie over de [programma Cloud Solution Provider](https://docs.microsoft.com/partner-center/csp-overview).
