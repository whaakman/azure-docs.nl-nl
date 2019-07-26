---

title: Wat is Azure naar?

description: Met Azure Lighthouse kunnen serviceproviders beheerde services voor hun klanten met een hogere automation en efficiëntie leveren op schaal.
author: JnHs
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
ms.service: lighthouse
manager: carmonm
ms.openlocfilehash: eb55af5a1121eb193bb76efc9f9e0b833f4b5a1f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809818"
---
# <a name="what-is-azure-lighthouse"></a>Wat is Azure Lighthouse?

Azure Lighthouse biedt serviceproviders één besturingsvlak waarin ze Azure voor al hun klanten kunnen weergeven en beheren met een verbeterde automatisering, schaal en beheer. Met Azure Lighthouse kunnen serviceproviders beheerde services leveren met behulp van uitgebreide en krachtige beheertools die zijn ingebouwd in het Azure-platform. Azure Lighthouse biedt ook voordelen voor grote IT-organisaties die resources voor meerdere tenants beheren.

![Overzichtsdiagram van Azure naar](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Voordelen

Met Azure Lighthouse kunt u beheerde services op een rendabele en efficiënte manier ontwikkelen en leveren aan uw klanten. U profiteert van de volgende voordelen:

- **Beheer op grote schaal**: Customer engagement en levenscyclus van bewerkingen voor het beheren van resources van de klant zijn eenvoudiger en beter schaalbaar.
- **Meer zichtbaarheid van en precisie voor klanten**: Klanten waarvan bronnen die u beheert wordt meer inzicht in uw acties en nauwkeurige controle over het bereik hebben ze delegeren voor beheer, terwijl uw IP-adres behouden blijft.
- **Platform met uitgebreide en geïntegreerde tooling**: Onze ervaring voor hulpprogramma's adressen sleutel serviceprovider-scenario's, met inbegrip van meerdere licentiemodellen zoals EA, CSP en betalen per gebruik. De nieuwe mogelijkheden werken met bestaande hulpprogramma's en API's, modellen, licentieverlening en programma's van partners, zoals de [programma Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/csp-overview). De Azure-naar-opties die u kiest in uw bestaande werkstromen en toepassingen kunnen worden geïntegreerd, en kunt u uw invloed op klant-engagements door bijhouden [uw partner-ID koppelen](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started).

Er zijn geen extra kosten die zijn gekoppeld aan uw klanten Azure-resources beheren met behulp van Azure naar.

## <a name="capabilities"></a>Functionaliteit

Azure Lighthouse biedt meerdere manieren waarop u de klantbetrokkenheid en het klantenbeheer kunt stroomlijnen.

- **Azure Resourcemanagement overgedragen**: Beheren van uw klanten Azure-resources veilig binnen uw eigen tenant zonder om over te schakelen van context en controle vlakken. Zie voor meer informatie, [Azure Resourcemanagement overgedragen](./concepts/azure-delegated-resource-management.md).
- **Nieuwe Azure-portal ervaringen**: Cross-tenant-gegevens weergeven in de nieuwe **mijn klanten** pagina in de [Azure-portal](https://portal.azure.com). Een bijbehorende **serviceproviders** blade kan uw klanten weergeven en beheren van toegang tot de service-provider. Zie voor meer informatie, [weergeven en beheren van klanten](./how-to/view-manage-customers.md) en [weergeven en beheren van serviceproviders](./how-to/view-manage-service-providers.md).
- **Azure Resource Manager-sjablonen**: Gemakkelijker beheertaken uitvoeren, met inbegrip van onboarding van klanten voor Azure Resourcemanagement overgedragen. Zie voor meer informatie onze [voorbeelden opslagplaats](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates) en [onboarding een klant naar Azure Resourcemanagement overgedragen](how-to/onboard-customer.md).
- **Beheerde Services aanbiedingen op Azure Marketplace**: Uw services te bieden aan klanten via persoonlijke of openbare aanbiedingen, en deze automatisch toegevoegd aan Azure-gedelegeerde resource-beheer, als alternatief voor onboarding met behulp van Azure Resource Manager-sjablonen. Zie voor meer informatie, [Managed services-aanbiedingen op Azure Marketplace](./concepts/managed-services-offers.md).
- **Azure beheerde toepassingen**: Pakket- en ontwikkelteams toepassingen eenvoudig voor uw klanten te implementeren en gebruiken in hun eigen abonnementen. De toepassing wordt geïmplementeerd in een brongroep of u toegang hebt tot uit uw tenant, zodat u de service beheren als onderdeel van de algemene Azure-naar-ervaring. Zie voor meer informatie, [Azure beheerde toepassingen-overzicht](https://docs.microsoft.com/azure/managed-applications/overview).

> [!NOTE]
> De mogelijkheden die hierboven worden beschreven zijn momenteel beschikbaar in openbare clouds. Zie voor de regionale beschikbaarheid van afzonderlijke services [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/).
