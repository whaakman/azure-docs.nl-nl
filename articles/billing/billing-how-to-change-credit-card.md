---
title: Je credit card voor Azure wijzigen
description: Hierin wordt beschreven hoe u de credit card wijzigt die wordt gebruikt voor het betalen van een Azure-abonnement.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: banders
ms.openlocfilehash: 7719ae83525883a6d3f014dbb99877b7319f2ccd
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383661"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Een credit card voor Azure toevoegen, bijwerken of verwijderen

In de Azure Portal kunt u een nieuwe credit card toevoegen, een bestaande Credit Card bijwerken of een credit card verwijderen die u niet gebruikt. U moet een [account beheerder](billing-subscription-transfer.md#whoisaa) zijn om deze wijzigingen door te voeren.

Als u een [micro soft-klant overeenkomst](#check-access-to-a-microsoft-customer-agreement)hebt, worden uw betalings wijzen aan facturerings profielen gekoppeld. Meer informatie over [het wijzigen van de standaard betalings methode voor een facturerings profiel](#change-payment-method-for-a-billing-profile). Alleen de gebruiker die zich heeft aangemeld voor Azure kan de betalings wijze bijwerken.

**Wilt u overschakelen naar betalen per factuur (cheque/overschrijving)?** Zie [betalen voor Azure-abonnementen per factuur](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## <a name="add-a-new-credit-card-to-an-azure-subscription"></a>Een nieuwe credit card toevoegen aan een Azure-abonnement

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als account beheerder.
1. Zoeken naar **Cost Management en facturering**.

    ![Scherm afbeelding waarin de zoek opdracht wordt weer gegeven](./media/billing-how-to-change-credit-card/search.png)

1. Selecteer het abonnement waaraan u de Credit Card wilt toevoegen.
1. Selecteer **Betalingswijzen**.

    ![Scherm afbeelding met de optie voor het beheren van betalings methoden geselecteerd.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Selecteer '+' In de linkerbovenhoek om een kaart toe te voegen Een creditcardformulier wordt aan de rechterkant weergegeven.
1. Voer creditcard gegevens in.

    ![Scherm opname van het toevoegen van een nieuwe kaart.](./media/billing-how-to-change-credit-card/sub-add-new-x.png)

1. Als u van deze kaart uw actieve Betalings wijze wilt maken, schakelt u het selectie vakje naast **deze mijn actieve Betalings wijze instellen** boven het formulier in. Deze kaart wordt de actieve betalingswijze voor alle abonnementen waarvoor dezelfde kaart wordt gebruikt als voor het geselecteerde abonnement.

1. Selecteer **Volgende**.

Als er een fout optreedt nadat u de credit card hebt toegevoegd, raadpleegt u [Credit Card geweigerd bij Azure-aanmelding](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-card"></a>Bestaande Credit Card bijwerken

Als uw credit card wordt vernieuwd en het nummer blijft hetzelfde, werkt u de bestaande creditcard gegevens bij, zoals de verval datum. Als uw creditcard nummer verandert omdat de kaart verloren is gegaan, wordt gestolen of is verlopen, volgt u de stappen in de sectie [een credit card toevoegen als Betalings wijze](#addcard) . U hoeft de CVW niet bij te werken.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als account beheerder.
1. Zoeken naar **Cost Management en facturering**.

    ![Scherm afbeelding waarin de zoek opdracht wordt weer gegeven](./media/billing-how-to-change-credit-card/search.png)

1. Selecteer **Betalingswijzen**.

    ![Scherm afbeelding met de optie voor het beheren van betalings methoden geselecteerd.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Klik op de credit card die u wilt bewerken. Een creditcardformulier wordt aan de rechterkant weergegeven.

    ![Scherm opname waarin de geselecteerde credit card wordt weer gegeven.](./media/billing-how-to-change-credit-card/edit-card-x.png)

1. De creditcard gegevens bijwerken.
1. Selecteer **Opslaan**.

## <a name="use-a-different-credit-card"></a>Een andere Credit Card gebruiken

Als meer dan een van uw abonnementen dezelfde actieve betalings methode heeft, dan wordt de actieve betalings methode voor een van deze abonnementen gewijzigd, maar ook de methode actief op de andere.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als account beheerder.
1. Zoeken naar **Cost Management en facturering**.

    ![Scherm afbeelding waarin de zoek opdracht wordt weer gegeven](./media/billing-how-to-change-credit-card/search.png)

1. Selecteer het abonnement waaraan u de Credit Card wilt toevoegen.
1. Selecteer **Betalingswijzen**.

    ![Scherm afbeelding met de optie voor het beheren van betalings methoden geselecteerd.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Schakel het selectie vakje in naast de kaart die u wilt instellen als actieve Betalings wijze.
1. Klik op **instellen op actief**.
    ![Scherm opname van de geselecteerde Credit Card en de set actief.](./media/billing-how-to-change-credit-card/sub-change-active-x.png)

## <a name="remove-a-credit-card-from-the-account"></a>Een credit card uit het account verwijderen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als account beheerder.
1. Selecteer **Cost Management + facturering** aan de linkerkant van de pagina.

    ![Scherm afbeelding waarin de zoek opdracht wordt weer gegeven](./media/billing-how-to-change-credit-card/search.png)

1. Selecteer onder **facturering** **betalings wijzen**.

    ![Scherm afbeelding met de optie voor het beheren van betalings methoden geselecteerd.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Schakel het selectie vakje in naast de kaart die u wilt verwijderen.
1. Klik op **Verwijderen**.

Als uw credit card de actieve betalings methode is voor uw micro soft-abonnementen, kunt u deze niet verwijderen uit uw Azure-account. Wijzig de actieve betalings methode voor alle abonnementen die zijn gekoppeld aan deze credit card en probeer het opnieuw
<!-- # Add, update, or remove a credit card for Azure

In the Account Center, you can add a new credit card, update an existing credit card, or delete a credit card that you don't use. You must be an [Account Administrator](billing-subscription-transfer.md#whoisaa) to make these changes.

**Want to switch to pay by invoice?** See [Pay for Azure subscriptions by invoice](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## Add a new credit card

1. Sign in to the [Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select a subscription.
1. On the right side of the page, select **Manage payment methods**.

    ![Screenshot that shows Manage payment methods option selected.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Select “+” to add a card.

    ![Screenshot that shows the edit option next to the payment method.](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Enter credit card details.
1. Select **Save**.

If you get an error after you add the credit card, see [Credit card declined at Azure sign-up](billing-credit-card-fails-during-azure-sign-up.md).

## Update existing credit card

If your credit card gets renewed and the number remains the same, update the existing credit card details like the expiration date. If your credit card number changes because the card is lost, stolen, or expired, follow the steps in the [Add a credit card as a payment method](#addcard) section. You don't need to update the CVV.

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. Select **Manage payment methods**.
1. Select **Edit** next to the card you want to update.
1. Update the credit card details.
1. Select **Save**.

## Use a different credit card for the Azure subscription

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. On the right side of the page, select **Manage payment methods**.
1. Click **Use Instead** next to the card that you want to use. This also updates any other subscriptions currently associated with this card.

## Remove a credit card from the account

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
3. On the right side of the page, select **Manage payment methods**.
4. Click **Delete** for the credit card that you want to delete.

If your credit card is associated with other active Microsoft subscriptions, you can't remove it from your Azure account. Remove the credit card from all active subscriptions that you have with Microsoft and try again. -->

## <a name="change-payment-method-for-a-billing-profile"></a>De betalings wijze voor een facturerings profiel wijzigen

Als u de betalings wijze voor een facturerings profiel wilt wijzigen, moet u de persoon zijn die zich heeft aangemeld voor Azure.

Als u wilt overstappen op de standaard betalings methode voor cheque/bedrading, leest u hoe u [een facturerings profiel kunt overzetten naar een cheque/bedrading](billing-how-to-pay-by-invoice.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken op **kosten Management en facturering**.
1. Klik in het menu aan de linkerkant op **facturerings profielen**.

    ![scherm afbeelding die het facturerings profiel in het menu weergeeft](./media/billing-how-to-change-credit-card/billing-profile.png)

1. Selecteer een facturerings profiel.
1. Selecteer **betalings methoden**in het menu aan de linkerkant.

   ![Scherm opname waarin de betalings wijzen in het menu worden weer gegeven](./media/billing-how-to-change-credit-card/billing-profile-payment-methods.png)

1. Klik boven de standaard betalings methode op **wijzigen**.

    ![Scherm afbeelding waarop de knop wijzigen wordt weer gegeven](./media/billing-how-to-change-credit-card/customer-led-switch-credit-card.png)

1. Selecteer een bestaande kaart of Voeg een nieuwe toe.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
In de volgende secties worden veelgestelde vragen beantwoord over het wijzigen van uw creditcard gegevens.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Mijn abonnement is uitgeschakeld. Waarom kan ik mijn credit card nu niet verwijderen?

Nadat uw abonnement is uitgeschakeld of geannuleerd, wacht u 90 dagen voordat uw abonnement permanent wordt verwijderd. We houden uw betalings wijze in het bestand tijdens de Bewaar periode voor het geval u het abonnement opnieuw wilt activeren. Daarna wordt het abonnement permanent verwijderd.

Als u uw credit card moet verwijderen voordat de Bewaar periode van 90 dagen eindigt, activeert u [uw abonnement opnieuw](billing-subscription-become-disable.md). [Neem contact op met de ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)als u niet opnieuw kunt activeren.

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Waarom blijf ik ' uw aanmeldings sessie is verlopen '. Klik hier om u opnieuw aan te melden "?

Als u dit fout bericht blijft ontvangen, zelfs als u zich al hebt afgemeld en weer ingelogd, probeert u het opnieuw met een persoonlijke browser sessie.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Hoe kan ik een andere kaart gebruiken voor elk abonnement dat ik heb?

Als uw abonnementen al gebruikmaken van dezelfde kaart, is het niet mogelijk ze te scheiden voor het gebruik van verschillende kaarten. Als u zich echter aanmeldt voor een nieuw abonnement, kunt u ervoor kiezen om een nieuwe Betalings wijze voor dat abonnement te gebruiken.

### <a name="how-do-i-make-payments"></a>Hoe kan ik betalingen doen?

Als u een credit card instelt als uw betalings wijze, wordt uw kaart na elke facturerings periode automatisch in rekening gebracht. U hoeft niets te doen.

Als u [betaalt per factuur](billing-how-to-pay-by-invoice.md), stuurt u uw betaling naar de locatie die onder aan uw factuur wordt weer gegeven.

### <a name="how-do-i-change-the-tax-id"></a>De BTW-ID Hoe kan ik wijzigen?

Als u de BTW-ID wilt toevoegen of bijwerken, werkt u uw profiel bij in het [Azure-Accountcentrum](https://account.azure.com/Profile)en selecteert u vervolgens **BTW-record**. Dit btw-nummer wordt gebruikt voor de berekening van btw-vrijstelling en wordt vermeld op uw factuur.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een micro soft-klant overeenkomst controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, kunt u [een ondersteunings aanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure-reserve ringen](billing-save-compute-costs-reservations.md) om te zien of ze geld kunnen besparen.
