---
title: Beheerde services aanbiedingen op Azure Marketplace
description: Beheerde services aanbiedingen kunnen serviceproviders resource management aanbiedingen aan klanten in Azure Marketplace verkopen.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: a6fcf5f1d0ac194d60f834fb8d26db019c538410
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809874"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Beheerde services aanbiedingen op Azure Marketplace

In dit artikel beschrijft de nieuwe **beheerde Services** aanbiedingstype in [Azure Marketplace](https://azuremarketplace.microsoft.com). Beheerde services biedt, kunt u resource management-services aanbieden aan klanten met Azure gedelegeerde resource Manager. U kunt deze aanbiedingen beschikbaar maken voor alle potentiële klanten, of alleen naar een of meer specifieke klanten. Omdat u rechtstreeks voor kosten met betrekking tot deze beheerde services facturen, zijn er geen kosten in rekening gebracht door Microsoft.

## <a name="understand-managed-services-offers"></a>Informatie over aanbiedingen van beheerde services

Beheerde services biedt stroomlijnen van de onboarding van klanten voor Azure Resourcemanagement gedelegeerd beheer. Wanneer een klant een aanbieding op Azure Marketplace-aankopen, ze zal het mogelijk om op te geven welke abonnementen en/of resourcegroepen uitgevoerd worden moeten zodat de opgegeven gebruikers in uw organisatie kunnen beheertaken uitvoeren voor de klant vanuit uw de tenant van de organisatie.

Hierna is er geen verdere actie door de klant of de service-provider vereist om op te halen van de Onboarding van de klant. Dit komt omdat bij het definiëren van de aanbieding in de [Cloud Partner-Portal](https://cloudpartner.azure.com/), maakt u een manifest dat Hiermee geeft u de Azure AD-gebruikers, groepen en service-principes die hebben toegang tot resources van de klant met behulp van Azure resource overgedragen beheer. samen met de functies die het niveau van toegang tot hun definiëren. U kunt machtigingen toewijzen aan een Azure AD-groep in plaats van een serie van afzonderlijke gebruikers of toepassingsaccounts, toevoegen of verwijderen van afzonderlijke gebruikers wanneer uw toegangsvereisten worden gewijzigd.

## <a name="public-and-private-offers"></a>Openbare en persoonlijke aanbiedingen

Ieder aanbod van beheerde services bevat een of meer abonnementen. Deze abonnementen kunnen privé of openbaar zijn.

Als u uw aanbieding aan specifieke klanten beperken wilt, kunt u een privé-abonnement kunt publiceren. Als u dit doet, kan het plan alleen worden aangeschaft voor de specifieke] abonnement-id's die u opgeeft. Zie voor meer informatie, [persoonlijke aanbiedingen](https://docs.microsoft.com/azure/marketplace/private-offers).

Openbare plannen kunnen u uw services voor nieuwe klanten te bevorderen. Dit zijn doorgaans meer geschikt wanneer u slechts beperkte toegang tot de tenant van de klant vereist. Wanneer u een relatie met een klant gemaakt hebt als ze besluit om uw organisatie meer toegang te verlenen, kunt u doen door het publiceren van een nieuwe persoonlijke planning voor die klant alleen, of door [onboarding ze voor verdere toegang met behulp van Azure Resource Manager-sjablonen](../how-to/onboard-customer.md).

Houd er rekening mee dat wanneer een plan is gepubliceerd als openbaar, u deze niet in een particuliere wijzigen. Bovendien beperken u niet van de planning van een openbare beschikbaarheid voor bepaalde klanten of zelfs naar een bepaald aantal klanten, maar u dat het plan volledig verkopen voorkomen kunt als u ervoor kiest om dit te doen.

Indien van toepassing, kunt u plannen voor openbare en persoonlijke opnemen in de dezelfde aanbieding.

## <a name="publish-managed-service-offers"></a>Beheerde service-aanbiedingen publiceren

Zie voor informatie over het publiceren van een aanbieding van beheerde services, [een Managed Services-aanbieding publiceren op Azure Marketplace](../how-to/publish-managed-services-offers.md). Raadpleeg voor algemene informatie over het publiceren op Azure Marketplace met behulp van de Cloud Partner-Portal [Azure Marketplace en AppSource Publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) en [Azure beheren en de AppSource-Marketplace-aanbiedingen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Resourcemanagement overgedragen](azure-delegated-resource-management.md) en [ervaringen voor cross-tenant](cross-tenant-management-experience.md).
- [Publiceren van aanbiedingen van beheerde services](../how-to/publish-managed-services-offers.md) op Azure Marketplace.
