---
title: Azure uitgavenlimiet begrijpen | Microsoft Docs
description: Hierin wordt beschreven hoe Azure uitgavenlimiet werkt en hoe deze te verwijderen
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/22/2017
ms.author: genli
ms.openlocfilehash: d222300a591bcba556be5813ada2de555d25a1f0
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Inzicht in Azure uitgaven limiet en hoe u deze verwijderen

Uitgavenlimiet in Azure bestaat om te voorkomen dat uitgaven via uw tegoed bedrag. Alle nieuwe klanten die zich aanmelden voor de proefversie of de aanbiedingen die tegoed bevat over meerdere maanden hebben de bestedingslimiet standaard ingeschakeld. De bestedingslimiet is $0. Deze kan niet worden gewijzigd. De bestedingslimiet is niet beschikbaar voor abonnementstypen zoals Betalen per gebruik-abonnementen en toegezegde abonnementen. Zie de [volledige lijst met Azure biedt en de beschikbaarheid van de bestedingslimiet](https://azure.microsoft.com/support/legal/offer-details/).

**Zoekt u waarschuwingen facturering?** Zie [facturerings- of waarschuwingen instellen voor Azure-abonnementen](billing-set-up-alerts.md).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Wat gebeurt er wanneer ik de bestedingslimiet bereik?

Wanneer de informatie over het gebruik van de resultaten in kosten die tot uitputting van de maandelijkse bedragen met uw abonnement opgenomen, worden de services die u hebt geïmplementeerd voor de rest van deze factureringsperiode uitgeschakeld. 

Bijvoorbeeld, wanneer u alle tegoed bij uw abonnement besteden, Cloud-Services die u hebt geïmplementeerd, worden verwijderd uit de productie en uw virtuele machines in Azure worden gestopt en de toegewezen. De gegevens in uw storage-accounts en -databases zijn beschikbaar in een alleen-lezen manier.

Aan het begin van de volgende factureringsperiode, als uw aanbieding voor abonnement tegoed over meerdere maanden, bevat zou uw abonnement worden opnieuw ingeschakeld. Vervolgens kunt u uw Cloud-Services implementeren en volledige toegang tot uw storage-accounts en -databases hebben.

We sturen e-mailmeldingen wanneer u de bestedingslimiet voor uw abonnement bereikt. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions), en er worden meldingen over abonnementen die de bestedingslimiet hebt bereikt.

Als u een gratis proefversie hebt en de bestedingslimiet bereikt, kunt u [upgrade naar betalen per gebruik](billing-upgrade-azure-subscription.md) de bestedingslimiet verwijderen en de beschikbaarheid van het abonnement automatisch opnieuw ingeschakeld.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Verwijder de bestedingslimiet in Accountcentrum

U kunt de bestedingslimiet op elk gewenst moment verwijderen zolang er een geldige betaalmethode aan uw abonnement is gekoppeld. Voor aanbiedingen die tegoed over meerdere maanden hebt, kunt u ook opnieuw inschakelen de bestedingslimiet aan het begin van de volgende factureringsperiode.

Ga als volgt te werk om de bestedingslimiet te verwijderen:

1. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions).
1. Selecteer een abonnement.
. Als het abonnement is uitgeschakeld vanwege de bestedingslimiet wordt bereikt, klikt u op deze melding: 'Abonnement de bestedingslimiet bereikt en is uitgeschakeld om kosten te voorkomen dat.' Klik anders op **uitgavenlimiet verwijderen** in de **ABONNEMENTSSTATUS** gebied.
1. Selecteer een voor u geschikte optie.

![Een optie voor de uitgavenlimiet verwijderen](./media/billing-spending-limit/remove-spending-limit.PNG)

|Optie|Effect|
|-------|-----|
|Bestedingslimiet voor onbepaalde tijd verwijderen|Hiermee wordt de bestedingslimiet verwijderd, zonder dat deze automatisch weer wordt ingeschakeld aan het begin van de volgende factureringsperiode.|
|Bestedingslimiet voor de huidige factureringsperiode verwijderen|Hiermee wordt de bestedingslimiet verwijderd, waarna deze automatisch weer wordt ingeschakeld aan het begin van de volgende factureringsperiode.|

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="why-would-i-want-to-remove-the-spending-limit"></a>Waarom zou ik wil de bestedingslimiet verwijderen?

De bestedingslimiet kan voorkomen dat u van de implementatie van of met bepaalde van derden en Microsoft-services. Hieronder ziet u de scenario's waarin u het beste de bestedingslimiet van uw abonnement kunt verwijderen.

* U bent van plan eigen installatiekopieën zoals Oracle en services zoals Visual Studio Team Services te implementeren. In dit scenario zorgt ervoor dat u uw bestedingslimiet bijna onmiddellijk overschrijden en zorgt ervoor dat uw abonnement worden uitgeschakeld.
* U hebt services die niet mogen worden onderbroken.
* U hebt services en bronnen met instellingen zoals virtuele IP-adressen die u niet wilt kwijtraken. Deze instellingen gaan verloren wanneer de services en bronnen opgeheven worden.

### <a name="how-do-i-turn-on-the-spending-limit-after-removing-it"></a>Hoe kan ik de bestedingslimiet na het verwijderen van deze inschakelen?

Deze functie is alleen beschikbaar wanneer de bestedingslimiet voor onbepaalde tijd is verwijderd. Deze om in te schakelen automatisch aan het begin van de volgende factureringsperiode wijzigen.

1. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions).
1. Klik op de gele banner om te wijzigen van de optie voor de uitgavenlimiet.
1. Kies **uitgavenlimiet in de volgende factureringsperiode inschakelen \<startdatum van facturering periode\>**

### <a name="how-do-i-set-a-custom-spending-limit"></a>Hoe stel ik een aangepaste uitgavenlimiet?

Er zijn momenteel geen aangepaste limieten vandaag te besteden aan. Echter, u kunt deelneemt aan [factureringsmeldingen om te bepalen gebruiken uw besteden](billing-set-up-alerts.md).

### <a name="does-the-spending-limit-prevent-all-charges-from-azure"></a>Voorkomt dat de bestedingslimiet alle kosten van Azure?

[Sommige externe services is gepubliceerd in Azure Marketplace](billing-understand-your-azure-marketplace-charges.md) kan niet worden gebruikt met het tegoed van uw abonnement en kunnen afzonderlijk worden kosten in rekening zelfs wanneer u uw bestedingslimiet is ingesteld. Voorbeelden zijn onder meer licenties voor Visual Studio, Azure Active Directory premium, ondersteuningsplannen en de meeste van derden huisstijl services. Wanneer u een nieuwe externe service inricht, wordt een waarschuwing weergegeven waarmee u dat de services worden afzonderlijk gefactureerd:

![Marketplace kopen waarschuwing](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
