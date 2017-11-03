---
title: Uw Azure-abonnement annuleren | Microsoft Docs
description: Hierin wordt beschreven hoe u uw Azure-abonnement, zoals de gratis proefabonnement annuleren
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: genli
ms.openlocfilehash: 51fe2ab891e86ae4bd7402622231af47f35aa01d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="cancel-your-subscription-for-azure"></a>Uw Azure-abonnement annuleren

U kunt uw Azure-abonnement als annuleren de [accountbeheerder](billing-subscription-transfer.md#whoisaa). Nadat u het abonnement annuleert, wordt uw toegang tot Azure-services en bronnen eindigt.

Voordat u uw abonnement annuleert:

* Back-up van uw gegevens. Als u gegevens in Azure storage of SQL opslaat, bijvoorbeeld een kopie downloaden. Als u een virtuele machine hebt, kunt u een afbeelding van het lokaal opslaan.
* Sluit uw services. Ga naar de [resources pagina in de beheerportal](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), en **stoppen** een met virtuele machines, toepassingen of andere services.
* U kunt uw gegevens migreren. Zie [resources verplaatsen naar de nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

Als u een betaald annuleert [Azure-ondersteuningsplan](https://azure.microsoft.com/support/plans/), u nog steeds maandelijks voor de rest van de termijn van 6 maanden wordt gefactureerd.

## <a name="cancel-subscription-using-the-azure-portal"></a>Met de Azure portal abonnement annuleren

1. Selecteer uw abonnement uit de [pagina van de abonnementen in Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt annuleren en klik op **abonnement annuleren**.

    ![Schermafbeelding van de knop Annuleren](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
1. Volg de aanwijzingen en annulering voltooien.

## <a name="what-happens-after-i-cancel-my-subscription"></a>Wat gebeurt er wanneer ik mijn abonnement annuleren?

Zodra u annuleert, wordt onmiddellijk facturering gestopt. Echter het kan maximaal 10 minuten duren voordat de annulering weergeven in de portal.

Daarna worden uw services uitgeschakeld. Dit betekent dat uw virtuele machines worden opgeheven, tijdelijke IP-adressen worden vrijgegeven en opslag is alleen-lezen.

Als u in het midden van een factureringsperiode annuleert, sturen de laatste factuur we op typische factuurdatum na afloop van de periode. 

Wij wachten 90 dagen voordat uw gegevens permanent te verwijderen als u nodig hebt om deze te openen of u van gedachten verandert. We betaalt u niet voor het bewaren van gegevens. Zie voor meer informatie, [Microsoft Trust Center - hoe beheer van uw gegevens](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Abonnement opnieuw activeren

Als u uw abonnement op gebruiksbasis per ongeluk annuleert, kunt u [opnieuw activeren in het midden Accounts](billing-subscription-become-disable.md).

Als uw abonnement niet betalen naar gebruik, moet u contact op met ondersteuning binnen 90 dagen na annulering uw abonnement opnieuw activeren.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Hebt u nog steeds vragen, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
