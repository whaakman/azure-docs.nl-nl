---
title: Informatie over Azure-bestedingslimiet | Microsoft Docs
description: Beschrijving van de werking van Azure-bestedingslimiet en hoe u deze kunt verwijderen
services: ''
documentationcenter: ''
author: genlin
manager: alherz
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: cwatson
ms.openlocfilehash: 39cb8a211aca1051be2bd71fbc234027b545b00b
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276542"
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Informatie over Azure-bestedingslimiet en hoe u deze kunt verwijderen

Bestedingslimiet in Azure bestaat om te voorkomen dat uitgaven gedurende het creditbedrag. Alle nieuwe klanten die zich aanmelden voor de proefversie of aanbiedingen met tegoeden verspreid over verschillende maanden hebben de bestedingslimiet standaard ingeschakeld. De bestedingslimiet is van $0. Het kan niet worden gewijzigd. De bestedingslimiet is niet beschikbaar voor abonnementstypen zoals Betalen per gebruik-abonnementen en toegezegde abonnementen. Zie de [volledig overzicht van Azure-aanbiedingen en de beschikbaarheid van de bestedingslimiet](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Wat gebeurt er wanneer ik de bestedingslimiet bereik?

Wanneer uw gebruik leidt tot kosten in rekening gebracht die wanneer de maandelijkse hoeveelheden die zijn opgenomen in uw abonnement, worden de services die u hebt geïmplementeerd zijn uitgeschakeld voor de rest van de factureringsperiode. 

Bijvoorbeeld, wanneer u besteden aan alle tegoed bij uw abonnement, Cloud-Services die u hebt geïmplementeerd, worden verwijderd uit de productie en uw Azure virtual machines zijn gestopt en toewijzing ongedaan gemaakt. De gegevens in uw opslagaccounts en databases zijn beschikbaar in een alleen-lezen wijze.

Aan het begin van de volgende factureringsperiode, als uw aanbieding verspreid over meerdere maanden tegoeden wordt uw abonnement opnieuw automatisch ingeschakeld. Vervolgens kunt u uw Cloud-Services implementeren en volledige toegang tot uw opslagaccounts en databases hebben.

We verzenden e-mailmeldingen wanneer u de bestedingslimiet voor uw abonnement bereikt. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions), en u meldingen weergegeven over abonnementen die de bestedingslimiet hebt bereikt.

Als u de bestedingslimiet bereik en een gratis proefversie hebt, kunt u [upgraden naar betalen per gebruik](billing-upgrade-azure-subscription.md) de bestedingslimiet verwijderen en het abonnement automatisch opnieuw ingeschakeld.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Verwijder de bestedingslimiet in Accountcentrum

U kunt de bestedingslimiet op elk gewenst moment verwijderen zolang er een geldige betaalmethode aan uw abonnement is gekoppeld. Voor aanbiedingen met tegoeden over een aantal maanden, kunt u ook opnieuw inschakelen de bestedingslimiet aan het begin van de volgende factureringsperiode.

Ga als volgt te werk om de bestedingslimiet te verwijderen:

1. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions).
1. Selecteer een abonnement. Als het abonnement is uitgeschakeld omdat de bestedingslimiet is bereikt, klikt u op deze melding: 'Abonnement de bestedingslimiet bereikt en is uitgeschakeld om kosten te voorkomen.' Klik anders op **bestedingslimiet verwijderen** in de **ABONNEMENTSSTATUS** gebied.
1. Selecteer een voor u geschikte optie.

![Een optie voor uitgavenlimiet verwijderen](./media/billing-spending-limit/remove-spending-limit.PNG)

|Optie|Effect|
|-------|-----|
|Bestedingslimiet voor onbepaalde tijd verwijderen|Hiermee wordt de bestedingslimiet verwijderd, zonder dat deze automatisch weer wordt ingeschakeld aan het begin van de volgende factureringsperiode.|
|Bestedingslimiet voor de huidige factureringsperiode verwijderen|Hiermee wordt de bestedingslimiet verwijderd, waarna deze automatisch weer wordt ingeschakeld aan het begin van de volgende factureringsperiode.|

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="why-would-i-want-to-remove-the-spending-limit"></a>Waarom zou ik wil de bestedingslimiet verwijderen?

De bestedingslimiet kan voorkomen dat u implementeren of bepaalde van derden en Microsoft-services gebruiken. Hieronder ziet u de scenario's waarin u het beste de bestedingslimiet van uw abonnement kunt verwijderen.

* U wilt implementeren eigen installatiekopieën zoals Oracle en services zoals Azure DevOps-Services. In dit scenario zorgt ervoor dat u uw bestedingslimiet bijna onmiddellijk overschrijden en zorgt ervoor dat uw abonnement wordt uitgeschakeld.
* U hebt services die niet mogen worden onderbroken.
* U hebt services en bronnen met instellingen zoals virtuele IP-adressen die u niet wilt kwijtraken. Deze instellingen gaan verloren wanneer de services en bronnen toewijzing ongedaan gemaakt zijn.

### <a name="how-do-i-turn-on-the-spending-limit-after-removing-it"></a>Hoe kan ik de bestedingslimiet na het verwijderen van deze inschakelen?

Deze functie is alleen beschikbaar wanneer de bestedingslimiet voor onbepaalde tijd is verwijderd. Wijzig dit in het automatisch inschakelen aan het begin van de volgende factureringsperiode.

1. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions).
1. Klik op de gele banner om de optie voor uitgavenlimiet wijzigen.
1. Kies **uitgavenlimiet in de volgende factureringsperiode inschakelen \<begindatum van de factureringsperiode\>**

### <a name="how-do-i-set-a-custom-spending-limit"></a>Hoe stel ik een aangepaste bestedingslimiet?

Aangepaste bestedingslimieten zijn niet beschikbaar.

### <a name="does-the-spending-limit-prevent-all-charges-from-azure"></a>Voorkomt dat de bestedingslimiet alle kosten van Azure?

[Sommige externe services die zijn gepubliceerd in de Azure Marketplace](billing-understand-your-azure-marketplace-charges.md) kan niet worden gebruikt met uw abonnementstegoed en kunt u betaalt afzonderlijke kosten, zelfs wanneer de bestedingslimiet is ingesteld. Voorbeelden zijn onder meer licenties voor Visual Studio, Azure Active Directory premium, ondersteuningsplannen en de meeste merkproducten van derden services. Als u een nieuwe externe service inricht, wordt een waarschuwing weergegeven zodat u weet dat de services worden apart in rekening gebracht:

![Marketplace-aankoop waarschuwing](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
