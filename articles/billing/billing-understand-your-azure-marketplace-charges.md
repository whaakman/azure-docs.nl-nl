---
title: Meer informatie over uw Azure externe servicekosten | Microsoft Docs
description: Meer informatie over de facturering van externe services, voorheen bekend als Marketplace, kosten in rekening gebracht in Azure.
author: jureid
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 278e873d01eb3dd7d614d771e5b50b8fe624800a
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490358"
---
# <a name="understand-your-azure-external-services-charges"></a>Meer informatie over de kosten van uw externe Azure-services
Externe services worden gepubliceerd door leveranciers van software van derden in de Azure marketplace. SendGrid is bijvoorbeeld een externe service die u in Azure kunt kopen, maar niet is gepubliceerd door Microsoft. Sommige Microsoft-producten worden verkocht via Azure marketplace te.

## <a name="how-external-services-are-billed"></a>Hoe externe services worden in rekening gebracht

- Als u hebt een [Microsoft KLANTOVEREENKOMST](#check-access), uw services van derden worden in rekening gebracht met de rest van uw Azure-services.
- Als u geen een KLANTOVEREENKOMST van Microsoft, uw externe services worden apart in rekening gebracht van uw Azure-services.
- Elke externe service heeft een ander model voor facturering. Sommige services worden op een manier voor betalen per gebruik gefactureerd, terwijl anderen maandelijkse kosten in rekening gebracht vaste.
- U kunt elke maand gratis tegoed niet gebruiken voor externe services. Als u een Azure-abonnement met inbegrip van [gratis tegoed](https://azure.microsoft.com/pricing/spending-limits/), ze kunnen niet worden toegepast op de kosten van externe services. Als u een nieuwe externe service of resource inricht, wordt een waarschuwing weergegeven:

    ![Marketplace-aankoop waarschuwing](./media/billing-understand-your-azure-marketplace-charges/credit-warning.png)

<!-- ## View external service spending and history in the Azure portal
You can view a list of the external services that are on each subscription within the [Azure portal](https://portal.azure.com/):

1. Sign in to the [Azure portal](https://portal.azure.com/) as the account administrator.
2. In the Hub menu, select **Subscriptions**.

    ![Select Subscriptions in the Hub menu](./media/billing-understand-your-azure-marketplace-charges/sub-button.png)
3. In the **Subscriptions** blade, select the subscription that you want to view, and then select **External services**.

    ![Select a subscription in the billing blade](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. You should see each of your external service orders, the publisher name, service tier you bought, name you gave the resource, and the current order status. To see past bills, select an external service.

    ![Select an external service](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. From here, you can view past bill amounts including the tax breakdown.

    ![View external services billing history](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png) -->

## <a name="view-and-download-invoices"></a>Weergeven en downloaden van facturen

Als u hebt een [Microsoft KLANTOVEREENKOMST](#check-access), de kosten van derden zijn op de factuur van Azure-kosten worden. Meer informatie over het [weergeven en downloaden van uw Azure-factuur](billing-download-azure-invoice.md) vanuit Azure portal om te zien van uw kosten van derden.

Als u een KLANTOVEREENKOMST van Microsoft hebt, hebt u aparte facturen voor derden kosten in rekening gebracht. U kunt bekijken en uw Azure Marketplace-facturen downloaden via de Azure portal door de volgende stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken naar **kosten Management en facturering**.
1. Selecteer in het menu links **facturen**.
1. Klik op de **Azure Marketplace en reserveringen** tabblad.  ![Afbeelding van Azure marketplace en reserveringen tabblad](./media/billing-understand-your-azure-marketplace-charges/invoice-tabs.png)
1. Selecteer het abonnement met de externe services die u wilt zien van facturen voor in de vervolgkeuzelijst abonnement.

## <a name="external-spending-for-ea-customers"></a>Externe uitgaven voor EA-klanten

EA-klanten kunnen zien externe service voor de uitgavelimiet en rapporten in de EA-portal te downloaden. Zie [Azure Marketplace voor EA-klanten](https://ea.azure.com/helpdocs/azureMarketplace) aan de slag.

## <a name="manage-payment-for-external-services"></a>Betaling voor externe services beheren

Wanneer een externe service koopt, kiest u een Azure-abonnement voor de resource. De betalingswijze van de geselecteerde Azure-abonnement, wordt de betalingsmethode voor de externe service. Als u wilt wijzigen van de betalingsmethode voor een externe service, moet u [Wijzig de betalingswijze van de Azure-abonnement](billing-how-to-change-credit-card.md) gekoppeld aan deze externe service. U kunt bepalen welk abonnement u voor uw order voor een externe service is gekoppeld aan door de volgende stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Klik op **alle Resources** in het navigatiemenu links.
     ![Schermafbeelding van alle resources menu-item](./media/billing-understand-your-azure-marketplace-charges/all-resources.png)
1. Zoeken naar de externe service.
1. Zoek de naam van het abonnement in de **abonnement** kolom.
    ![Schermafbeelding van de naam van abonnement voor de resource](./media/billing-understand-your-azure-marketplace-charges/sub-selected.png)
1. Klik op de naam van het abonnement en [bijwerken van de actieve betalingswijze](billing-how-to-change-credit-card.md).

<!-- Update your payment methods for external service orders from the [Account Center](https://account.windowsazure.com/).

> [!NOTE]
> If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to make changes to your payment method.

1. Sign in to the [Account Center](https://account.windowsazure.com/) and [navigate to the **marketplace** tab](https://account.windowsazure.com/Store)

    ![Select marketplace in the account center](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Select the external service you want to manage

    ![Select the external service you want to manage](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Click **Change payment method** on the right side of the page. This link brings you to a different portal to manage your payment method.

    ![Order summary](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Click **Edit info** and follow instructions to update your payment information.

    ![Select edit info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png) -->

## <a name="cancel-an-external-service-order"></a>Een externe serviceorder annuleren
Als u annuleren van uw order voor een externe service wilt, verwijdert u de resource in de [Azure-portal](https://portal.azure.com).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Klik op **alle Resources** in het navigatiemenu links.
    ![Schermafbeelding van alle resources menu-item](./media/billing-understand-your-azure-marketplace-charges/all-resources.png)
1. Zoeken naar de externe service.
1. Schakel het selectievakje naast de resource die u wilt verwijderen.
1. Selecteer **verwijderen** in de opdrachtbalk.
    ![Schermafbeelding van de verwijderknop](./media/billing-understand-your-azure-marketplace-charges/delete-button.png)
1. Type *'Ja'* de bevestigingsblade.
    ![Resource verwijderen](./media/billing-understand-your-azure-marketplace-charges/delete-resource.PNG)
1. Klik op **Verwijderen**.

## <a name="check-access"></a>Toegang controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen
- [Beginnen met kosten analyseren](../cost-management/quick-acm-cost-analysis.md)
