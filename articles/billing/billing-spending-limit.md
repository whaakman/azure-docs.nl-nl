---
title: Azure-bestedingslimiet | Microsoft Docs
description: Dit artikel wordt beschreven hoe een Azure-bestedingslimiet werkt en hoe u deze kunt verwijderen.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: fb43f29827309fc8986ee6b4653f5edf303cc21d
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490599"
---
# <a name="azure-spending-limit"></a>Azure-bestedingslimiet

De bestedingslimiet in Azure wordt voorkomen dat uitgaven gedurende de tegoedbedrag. Alle nieuwe klanten die zich aanmelden voor een proefversie van Azure of aanbiedingen met tegoeden over een aantal maanden opnemen hebben de bestedingslimiet standaard ingeschakeld. De bestedingslimiet is van $0. Het kan niet worden gewijzigd. De bestedingslimiet is niet beschikbaar voor abonnementen onder sommige dergelijke toegezegde abonnementen en plannen met betalen naar gebruik-abonnementen. Zie de [volledig overzicht van Azure-aanbiedingen en de beschikbaarheid van de bestedingslimiet](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="reaching-a-spending-limit"></a>Een bestedingslimiet is bereikt

Wanneer uw gebruik leidt tot kosten in rekening gebracht die wanneer de maandelijkse hoeveelheden die zijn opgenomen in uw Azure-abonnement, worden de services die u hebt geïmplementeerd zijn uitgeschakeld voor de rest van de factureringsperiode.

Bijvoorbeeld, wanneer u besteden aan alle tegoed bij uw abonnement, Azure-resources die u hebt geïmplementeerd, worden verwijderd uit de productie en uw Azure virtual machines zijn gestopt en toewijzing ongedaan gemaakt. De gegevens in uw storage-accounts zijn beschikbaar als alleen-lezen.

Aan het begin van de volgende factureringsperiode, als uw aanbieding verspreid over meerdere maanden tegoeden wordt uw abonnement opnieuw automatisch ingeschakeld. Vervolgens kunt u uw Azure-resources implementeren en volledige toegang tot uw opslagaccounts en databases hebben.

Azure stuurt e-mailbericht meldingen wanneer u de bestedingslimiet bereikt voor uw abonnement beperken. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions) naar meldingen weergegeven over abonnementen die de bestedingslimiet hebt bereikt.

Als u de bestedingslimiet bereik en een gratis proefabonnement hebt, kunt u upgraden naar een abonnement met [betalen per gebruik](billing-upgrade-azure-subscription.md) prijzen als u wilt verwijderen van de uitgaven beperken en automatisch inschakelen van het abonnement.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Verwijder de bestedingslimiet in Accountcentrum

U kunt de bestedingslimiet op elk gewenst moment verwijderen zolang er een geldige betaalmethode die zijn gekoppeld aan uw Azure-abonnement is. Voor aanbiedingen met tegoeden over een aantal maanden, kunt u ook de bestedingslimiet inschakelen aan het begin van de volgende factureringsperiode.

Ga als volgt te werk om de bestedingslimiet te verwijderen:

1. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions).
1. Selecteer een abonnement. Als het abonnement is uitgeschakeld omdat de bestedingslimiet is bereikt, klikt u op de melding: **Abonnement is de bestedingslimiet bereikt en is uitgeschakeld om kosten te voorkomen.** Klik anders op **bestedingslimiet verwijderen** in de **ABONNEMENTSSTATUS** gebied.
1. Selecteer een voor u geschikte optie.

![Een optie voor uitgavenlimiet verwijderen](./media/billing-spending-limit/remove-spending-limit.PNG)

| Optie | Effect |
| --- | --- |
| Bestedingslimiet voor onbepaalde tijd verwijderen | Hiermee wordt de bestedingslimiet verwijderd, zonder dat deze automatisch weer wordt ingeschakeld aan het begin van de volgende factureringsperiode. |
| Bestedingslimiet voor de huidige factureringsperiode verwijderen | Hiermee wordt de bestedingslimiet verwijderd, waarna deze automatisch weer wordt ingeschakeld aan het begin van de volgende factureringsperiode. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>Waarom wilt u mogelijk de bestedingslimiet verwijderen

De bestedingslimiet kan voorkomen dat u implementeren of bepaalde van derden en Microsoft-services gebruiken. Hier volgen de situaties waar u de bestedingslimiet voor uw abonnement te verwijderen.

-  U wilt implementeren eigen installatiekopieën zoals Oracle en services zoals Azure DevOps-Services. Deze situatie zorgt ervoor dat u uw bestedingslimiet bijna onmiddellijk overschrijden en zorgt ervoor dat uw abonnement wordt uitgeschakeld.
- U hebt services die niet mogen worden onderbroken.
- U hebt services en bronnen met instellingen zoals virtuele IP-adressen die u niet wilt kwijtraken. Deze instellingen gaan verloren wanneer de services en bronnen toewijzing ongedaan gemaakt zijn.

## <a name="turn-on-the-spending-limit-after-removing"></a>De bestedingslimiet na het verwijderen van inschakelen

Deze functie is alleen beschikbaar wanneer de bestedingslimiet voor onbepaalde tijd is verwijderd. Wijzig dit in het automatisch inschakelen aan het begin van de volgende factureringsperiode.

1. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/Subscriptions).
1. Klik op de gele banner om de optie voor uitgavenlimiet wijzigen.
1. Kies **uitgavenlimiet in de volgende factureringsperiode inschakelen \<begindatum van de factureringsperiode\>**

## <a name="custom-spending-limit"></a>Aangepaste bestedingslimiet

Aangepaste bestedingslimieten zijn niet beschikbaar.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Een bestedingslimiet niet voorkomen dat alle kosten

[Sommige externe services die zijn gepubliceerd in de Azure Marketplace](billing-understand-your-azure-marketplace-charges.md) kan niet worden gebruikt met uw abonnementstegoed en kunt u betaalt afzonderlijke kosten, zelfs wanneer de bestedingslimiet is ingesteld. Voorbeelden zijn onder meer licenties voor Visual Studio, Azure Active Directory premium, ondersteuningsplannen en de meeste merkproducten van derden services. Als u een nieuwe externe service inricht, wordt een waarschuwing weergegeven zodat u weet dat de services worden apart in rekening gebracht:

![Marketplace-aankoop waarschuwing](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen
- Een upgrade uitvoeren naar een abonnement met [betalen per gebruik](billing-upgrade-azure-subscription.md) prijzen.
