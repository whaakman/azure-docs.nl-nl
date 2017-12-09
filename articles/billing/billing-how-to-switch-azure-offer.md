---
title: Wijziging Azure-abonnement aanbieding | Microsoft Docs
description: Meer informatie over het wijzigen van uw Azure-abonnement en schakel over naar een andere aanbieding die met behulp van Azure-Accountcentrum
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: aae227b3-6d64-4550-a5b6-d359f53f0a59
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/30/2017
ms.author: genli
ms.openlocfilehash: 381994079b7bcaeff08802b06573b977bf631e9d
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="change-your-azure-pay-as-you-go-subscription-to-a-different-offer"></a>Uw abonnement op Azure betalen naar gebruik voor een andere aanbieding wijzigen

Als een [betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) klant, kunt u uw Azure-abonnement overschakelen naar een andere aanbieding in de [Accountcentrum](https://account.windowsazure.com/Subscriptions). Bijvoorbeeld, kunt u deze functie om te profiteren van de [maandelijks tegoed voor Visual Studio-abonnees](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). 

**Willen een upgrade uitvoert van een gratis proefversie?** Zie [upgrade naar betalen per gebruik](billing-upgrade-azure-subscription.md).

## <a name="whats-supported"></a>Wat wordt ondersteund:

| Vanaf | Handeling |
| --- | --- |
| Betalen per gebruik |[Betalen naar gebruik ontwikkelen en testen](https://azure.microsoft.com/offers/ms-azr-0023p/) |
| Betalen per gebruik |[Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/) |
| Betalen per gebruik |[Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/) |
| Betalen per gebruik |[MSDN-Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/) |
| Betalen per gebruik |[Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) |
| Betalen per gebruik |[Visual Studio Enterprise (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [!NOTE]
> Voor andere wijzigingen aanbieding [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="switch-subscription-offer"></a>Switch-abonnement aanbieding

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Switch-to-a-different-Azure-offer/player]
>
>

1. Meld u aan bij [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions).
1. Selecteer uw abonnement op basis van betalen per gebruik.
1. Klik op **Overstappen naar een andere aanbieding**. De knop is alleen beschikbaar als u betaalt per gebruik en uw eerste factureringsperiode voorbij is.

   ![Let op de knop schakelen aanbod aan de rechterkant van de pagina](./media/billing-how-to-switch-azure-offer/switchbutton.png)
1. **Selecteer de gewenste aanbieding** uit de lijst met aanbiedingen uw abonnement kan worden overgeschakeld naar de. Deze lijst varieert op basis van de lidmaatschappen waaraan uw account is gekoppeld. Als niets beschikbaar is, controleert u de [lijst met beschikbare aanbiedingen die u naar overschakelen kunt](#whats-supported) en controleert u of u het juiste lidmaatschap. 

   ![Selecteer een aanbieding die u overschakelen wilt naar](./media/billing-how-to-switch-azure-offer/selectoffer.png)
1. Afhankelijk van de aanbieding die u naar omschakelt, ziet u mogelijk een opmerking over de consequenties van overschakelen. Deze lijst zorgvuldig doorlopen en volg de instructies voordat u doorgaat.

   ![Notities bekijken](./media/billing-how-to-switch-azure-offer/thingstonote.png)
1. U kunt de naam van uw abonnement kunt wijzigen. Standaard wordt deze instellen op de nieuwe aanbiedingsnaam. Klik op **Switch bieden** om het proces te voltooien.

   ![Klik op de groene knop](./media/billing-how-to-switch-azure-offer/confirmpage.png)
1. Voltooid Uw abonnement wordt nu overgeschakeld naar de nieuwe aanbieding.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-is-an-azure-offer"></a>Wat is een aanbieding voor Azure?

Een Azure-aanbieding is de *type* van de Azure-abonnement hebt. Bijvoorbeeld: [betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/), [Azure in Open](https://azure.microsoft.com/offers/ms-azr-0111p/), en [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) zijn alle Azure-aanbiedingen. Elke aanbieding heeft verschillende [voorwaarden](https://azure.microsoft.com/support/legal/offer-details/) en sommige hebben speciale voordelen. De aanbieding van uw abonnement vindt u in de pagina van de abonnement-Accountcentrum. Klik op de aanbiedingsnaam van de voor meer informatie.

   ![Klik op de koppeling van de aanbieding in Accountcentrum voor meer informatie](./media/billing-how-to-switch-azure-offer/offerlink.png)

### <a name="why-dont-i-see-the-button"></a>Waarom zie ik niet de knop?

U ziet mogelijk ook geen de **overschakelen naar een andere aanbieding** als knop:

* U bent niet op [betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/). Op dit moment alleen betalen naar gebruik abonnementen kunnen worden geconverteerd naar een andere aanbieding.
  * Als u van gebruikmaakt [gratis proefversie](https://azure.microsoft.com/free/), informatie over hoe [upgrade naar betalen per gebruik](billing-upgrade-azure-subscription.md).
  * Overschakelen van de aanbieding van een ander abonnement [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
* U bent nog steeds op uw eerste factureringsperiode; u moet wachten voor uw eerste factureringsperiode beëindigen voordat u aanbiedingen kunt overschakelen.

### <a name="why-do-i-see-there-are-no-offers-available-in-your-region-or-country-at-this-time"></a>Waarom zie ik 'Er zijn geen aanbiedingen beschikbaar in uw regio of land op dit moment'?

* Mogelijk niet in aanmerking komen voor geen aanbieding switches. Controleer de [lijst met beschikbare aanbiedingen die u naar overschakelen kunt](#whats-supported) en zorg ervoor dat u de juiste voordelen met Visual Studio of Bizspark hebt geactiveerd.
* Sommige aanbiedingen zijn mogelijk niet in alle landen beschikbaar.

### <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Wat doet Azure aanbiedingen wilt overschakelen naar Mijn service en facturering?

Hier volgen de details van wat er gebeurt wanneer u overschakelt dat Azure biedt in het midden van het Account.

#### <a name="no-service-downtime"></a>Er is geen onderhoudstijd

Er is geen uitvaltijd service voor alle gebruikers die zijn gekoppeld aan het abonnement. De aanbieding die u naar overschakelt hebben echter beperkingen. Sommige aanbiedingen verbieden bijvoorbeeld gebruik in productieomgevingen, dus moet u productieresources verplaatsen naar een ander abonnement.

#### <a name="quota-increases-are-reset"></a>Quotum toeneemt worden opnieuw ingesteld.

Als u biedt, elk overschakelt [limiet of quotum toeneemt boven de standaardlimiet](../azure-supportability/resource-manager-core-quotas-request.md) worden opnieuw ingesteld. Er is geen service-uitvaltijd, zelfs als er meer bronnen voorbij de standaardlimiet. Bijvoorbeeld, u 200 kernen op uw abonnement en uw quotum voor kernen terug naar de standaardwaarde van 20 kernen overschakelen aanbiedingen herstelt. De virtuele machines die gebruikmaken van de 200 kernen blijft de situatie ongewijzigd en zou worden uitgevoerd. Als u een andere quotum verhogen aanvraag geen maakt, echter, kan niet u alle meer cores inrichten.

#### <a name="billing"></a>Facturering

Op de dag dat u wilt overschakelen, wordt een factuur gegenereerd voor alle openstaande bedragen. Uw abonnement wordt vervolgens gefactureerd per prijsvoorwaarden van de nieuwe aanbieding. Uw abonnement facturering Verjaardag wijzigingen aan de datum op waarop u aanbiedingen is gewijzigd. Informatie over het gebruik en facturering van gegevens voordat de wijziging van de aanbieding niet behouden, zodat het is raadzaam dat u een exemplaar voordat u overschakelt downloaden.

### <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Kan ik migreren van betalen naar gebruik voor [Cloud Solution Provider](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (CSP) of [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

* Als u wilt migreren naar de CSP, Zie [Azure Pa's-als-u-Go Abonnementmigratie met CSP](https://docs.microsoft.com/azure/cloud-solution-provider/migration/migration-from-payg-to-csp).
* Als u wilt migreren naar EA, hebt u uw registratie-beheerder uw account toevoegen aan de EA. Volg de instructies in de uitnodiging e-mail voor uw abonnementen verplaatst onder EA inschrijving hebt. Zie voor meer informatie, [koppelen van een bestaand Account](https://ea.azure.com/helpdocs/associateExistingAccount) in de portal EA.

### <a name="can-i-migrate-data-and-services-to-a-new-subscription"></a>Kan ik gegevens en services migreren naar een nieuw abonnement?

* U kunt de bronnen migreren rechtstreeks naar het nieuwe abonnement, Zie [resources verplaatsen naar de nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).
* Als u wilt het eigendom overdraagt van een Azure-abonnement en alle erin aan iemand anders, Zie [overdracht van eigendom van een Azure-abonnement](billing-subscription-transfer.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
