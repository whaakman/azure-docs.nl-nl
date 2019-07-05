---
title: Wijzigen van uw creditcard is geregistreerd voor Azure
description: Beschrijft hoe u wijzigt de creditcard die is gebruikt om een Azure-abonnement te betalen.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 7c04e33d6199d3930be28ce84458e9c3a743eb8a
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491334"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Toevoegen, bijwerken of verwijderen van een creditcard voor Azure

In de Azure-portal, kunt u een nieuwe creditcard toevoegen, een bestaande creditcard bijwerken of verwijderen van een creditcard die u niet gebruikt. U moet een [accountbeheerder](billing-subscription-transfer.md#whoisaa) u deze wijzigingen aanbrengt.

Als u hebt een [Microsoft KLANTOVEREENKOMST](#check-access-to-a-microsoft-customer-agreement), uw betalingswijzen zijn gekoppeld aan de facturering van profielen. Meer informatie over het [de standaardmethode voor de betaling voor een profiel voor facturering wijzigen](#change-payment-method-for-a-billing-profile). Alleen de gebruiker die zich hebben aangemeld voor Azure kan de betalingsmethode bijwerken.

**Wilt u overschakelen naar betalen per factuur (selectievakje/overschrijving)?** Zie [voor Azure-abonnementen betalen per factuur](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## <a name="add-a-new-credit-card-to-an-azure-subscription"></a>Een nieuwe creditcard toevoegen aan een Azure-abonnement

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als de accountbeheerder.
1. Zoeken naar **kosten Management en facturering**.

    ![Schermafbeelding van search](./media/billing-how-to-change-credit-card/search.png)

1. Selecteer het abonnement dat u wilt toevoegen van de creditcard.
1. Selecteer **Betalingswijzen**.

    ![Schermopname die laat beheren methoden betalingsoptie geselecteerd zien.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Selecteer '+' In de linkerbovenhoek om een kaart toe te voegen Een creditcardformulier wordt aan de rechterkant weergegeven.
1. Geef details van de creditcard.

    ![Schermafbeelding van een nieuwe kaart toe te voegen.](./media/billing-how-to-change-credit-card/sub-add-new-x.png)

1. Deze kaart van de actieve betalingswijze, schakel het selectievakje in naast **hiervan Mijn actieve betalingswijze** boven het formulier. Deze kaart wordt de actieve betalingswijze voor alle abonnementen waarvoor dezelfde kaart wordt gebruikt als voor het geselecteerde abonnement.

1. Selecteer **Next**.

Als u een foutmelding krijgt nadat u de creditcard hebt toegevoegd, Zie [creditcard is geregistreerd bij Azure aanmelden geweigerd](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-card"></a>Bestaande creditcardgegevens bijwerken

Als uw creditcard wordt vernieuwd en het aantal hetzelfde blijft, moet u de details van de bestaande creditcard is geregistreerd, zoals de datum van afloop voor bijwerken. Als uw creditcard telefoonnummer is gewijzigd omdat de kaart verloren is, gestolen of verlopen, volg de stappen in de [creditcard is geregistreerd als een betalingswijze toevoegen](#addcard) sectie. U hoeft niet te werken van de CVW.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als de accountbeheerder.
1. Zoeken naar **kosten Management en facturering**.

    ![Schermafbeelding van search](./media/billing-how-to-change-credit-card/search.png)

1. Selecteer **Betalingswijzen**.

    ![Schermopname die laat beheren methoden betalingsoptie geselecteerd zien.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Klik op de creditcard die u wilt bewerken. Een creditcardformulier wordt aan de rechterkant weergegeven.

    ![Schermopname die laat creditcard hebt geselecteerd zien.](./media/billing-how-to-change-credit-card/edit-card-x.png)

1. De details van de creditcard bijwerken.
1. Selecteer **Opslaan**.

## <a name="use-a-different-credit-card"></a>Gebruik een andere creditcard

Als meer dan een van uw abonnementen de dezelfde actieve betalingswijze wordt gebruikt hebben, klikt u vervolgens wijzigen van de actieve betalingswijze wordt gebruikt op een van deze abonnementen ook bijgewerkt voor de actieve betalingswijze wordt gebruikt op de andere.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als de accountbeheerder.
1. Zoeken naar **kosten Management en facturering**.

    ![Schermafbeelding van search](./media/billing-how-to-change-credit-card/search.png)

1. Selecteer het abonnement dat u wilt toevoegen van de creditcard.
1. Selecteer **Betalingswijzen**.

    ![Schermopname die laat beheren methoden betalingsoptie geselecteerd zien.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Selecteer het selectievakje naast de kaart die u wilt maken van de actieve betalingswijze wordt gebruikt.
1. Klik op **actief ingesteld**.
    ![Schermafbeelding van creditcard geselecteerd en actief ingesteld.](./media/billing-how-to-change-credit-card/sub-change-active-x.png)

## <a name="remove-a-credit-card-from-the-account"></a>Een creditcard verwijderen uit het account

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als de accountbeheerder.
1. Selecteer **kostenbeheer en facturering** aan de linkerkant van de pagina.

    ![Schermafbeelding van search](./media/billing-how-to-change-credit-card/search.png)

1. Onder **facturering**, selecteer **betalingswijzen**.

    ![Schermopname die laat beheren methoden betalingsoptie geselecteerd zien.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Selecteer het selectievakje naast de kaart die u wilt verwijderen.
1. Klik op **Verwijderen**.

Als uw creditcard de actieve betalingswijze wordt gebruikt voor het gebruik van uw Microsoft-abonnementen is, kunt u deze niet verwijderen uit uw Azure-account. Wijzig de actieve betalingswijze wordt gebruikt voor alle abonnementen die zijn gekoppeld aan deze creditcard is geregistreerd en probeer het opnieuw
<!-- # Add, update, or remove a credit card for Azure

In the Account Center, you can add a new credit card, update an existing credit card, or delete a credit card that you don't use. You must be an [Account Administrator](billing-subscription-transfer.md#whoisaa) to make these changes.

**Want to switch to pay by invoice?** See [Pay for Azure subscriptions by invoice](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## Add a new credit or debit card

1. Sign in to the [Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select a subscription.
1. On the right side of the page, select **Manage payment methods**.

    ![Screenshot that shows Manage payment methods option selected.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Select “+” to add a card.

    ![Screenshot that shows the edit option next to the payment method.](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Enter credit or debit card details.
1. Select **Save**.

If you get an error after you add the credit card, see [Credit card declined at Azure sign-up](billing-credit-card-fails-during-azure-sign-up.md).

## Update existing credit or debit card

If your credit card gets renewed and the number remains the same, update the existing credit card details like the expiration date. If your credit card number changes because the card is lost, stolen, or expired, follow the steps in the [Add a credit card as a payment method](#addcard) section. You don't need to update the CVV.

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. Select **Manage payment methods**.
1. Select **Edit** next to the card you want to update.
1. Update the credit or debit card details.
1. Select **Save**.

## Use a different credit card for the Azure subscription

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. On the right side of the page, select **Manage payment methods**.
1. Click **Use Instead** next to the card that you want to use. This also updates any other subscriptions currently associated with this card.

## Remove a credit or debit card from the account

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
3. On the right side of the page, select **Manage payment methods**.
4. Click **Delete** for the credit card that you want to delete.

If your credit card is associated with other active Microsoft subscriptions, you can't remove it from your Azure account. Remove the credit card from all active subscriptions that you have with Microsoft and try again. -->

## <a name="change-payment-method-for-a-billing-profile"></a>De betalingswijze wijzigen voor een profiel voor facturering

Als u wilt de betalingsmethode voor een profiel voor facturering wijzigen, moet u de persoon die zich hebben aangemeld voor Azure.

Als u overschakelen van de standaard-betalingswijze selectievakje wilt/wire overdracht, informatie over hoe u [overschakelen van een factureringsprofiel om te controleren/wire overdracht](billing-how-to-pay-by-invoice.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoek naar **kostenbeheer en facturering**.
1. Klik in het menu aan de linkerkant op **facturering profielen**.

    ![Schermafbeelding van factureringsprofiel in het menu](./media/billing-how-to-change-credit-card/billing-profile.png)

1. Selecteer een profiel voor facturering.
1. Selecteer in het menu aan de linkerkant, **betalingswijzen**.

   ![Schermafbeelding van de betalingswijzen in het menu ziet](./media/billing-how-to-change-credit-card/billing-profile-payment-methods.png)

1. Klik boven de standaardmethode voor betaling op **wijziging**.

    ![Schermafbeelding van de knop wijzigen](./media/billing-how-to-change-credit-card/customer-led-switch-credit-card.png)

1. Selecteer een bestaande kaart of een nieuwe toevoegen.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
De volgende secties beantwoorden Veelgestelde vragen over het wijzigen van de gegevens van uw creditcard of betaalpas.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Mijn abonnement is uitgeschakeld. Waarom kan ik nu niet verwijderen mijn creditcard?

Nadat uw abonnement is uitgeschakeld of geannuleerd, wacht we 90 dagen voordat uw abonnement permanent worden verwijderd. We bewaren uw betalingswijze wordt gebruikt in het bestand tijdens de retentieperiode voor het geval u wilt het abonnement opnieuw activeren. Het abonnement wordt hierna definitief verwijderd.

Als u verwijderen van uw creditcard wilt voordat de bewaarperiode van 90 dagen afloopt, [uw abonnement opnieuw activeren](billing-subscription-become-disable.md). Als u niet opnieuw activeren, [Neem contact op met ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Waarom ik steeds 'uw aanmeldingssessie is verlopen. Klik hier om te weer aanmelden'?

Als u dit foutbericht ontvangen blijven, zelfs als u al hebt afgemeld en weer in, probeer het opnieuw met een persoonlijke browsersessie.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Hoe gebruik ik een andere kaart voor elk abonnement dat ik heb?

Helaas, als uw abonnementen zijn al met dezelfde kaart, het is niet mogelijk om ze voor het gebruik van andere kaarten te scheiden. Echter, wanneer u zich registreert voor een nieuw abonnement, kunt u kiezen voor gebruik van een nieuwe betalingswijze voor dat abonnement.

### <a name="how-do-i-make-payments"></a>Hoe maak ik betalingen?

Als u een creditcard als uw betalingswijze wordt gebruikt instellen, berekenen we uw kaart automatisch na elke betalingsperiode. U hoeft te ondernemen.

Als u bent [betalen per factuur](billing-how-to-pay-by-invoice.md), verzenden de betaling naar de locatie aan de onderkant van uw factuur weergegeven.

### <a name="how-do-i-change-the-tax-id"></a>Hoe kan ik de belasting-ID wijzigen?

Als u wilt toevoegen of bijwerken van de btw-nummer, uw profiel bijwerken in de [Azure-Accountcentrum](https://account.azure.com/Profile)en selecteer vervolgens **btw-record**. Dit btw-nummer wordt gebruikt voor de berekening van btw-vrijstelling en wordt vermeld op uw factuur.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure reserveringen](billing-save-compute-costs-reservations.md) om te zien als ze u geld kunnen besparen.
