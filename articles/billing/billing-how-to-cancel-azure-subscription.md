---
title: Uw Azure-abonnement annuleren | Microsoft Docs
description: Hierin wordt beschreven hoe u uw Azure-abonnement, zoals de gratis proefversie van abonnement annuleren
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: cwatson
ms.openlocfilehash: de5fd6e52ab93132920b1c98188cbea92b9900b5
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584346"
---
# <a name="cancel-your-subscription-for-azure"></a>Uw abonnement voor Azure annuleren

U kunt uw Azure-abonnement als annuleren de [accountbeheerder](billing-subscription-transfer.md#whoisaa). Nadat u het abonnement annuleert, wordt de toegang tot Azure-services en resources eindigt.

Voordat u uw abonnement annuleert:

* Back-up van uw gegevens. Bijvoorbeeld, als u gegevens in Azure storage of SQL opslaat, een kopie downloaden. Als u een virtuele machine hebt, kunt u een afbeelding van het lokaal opslaan.
* Uw services afsluiten. Ga naar de [resources pagina in de beheerportal](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), en **stoppen** alle virtuele machines, toepassingen of andere services.
* Houd rekening met uw gegevens migreren. Zie [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

Als u een betaald ondersteuningsabonnement voor Azure annuleert, wordt u nog steeds gefactureerd voor de rest van het abonnement. Zie voor meer informatie, [ondersteuningsabonnementen voor Azure](https://azure.microsoft.com/support/plans/).

## <a name="cancel-subscription-using-the-azure-portal"></a>Abonnement met behulp van de Azure portal

1. Selecteer uw abonnement uit de [pagina met abonnementen in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt annuleren en klik op **abonnement annuleren**.

    ![Schermafbeelding van de knop Annuleren](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
1. Volg de aanwijzingen en Annuleren te voltooien.

## <a name="what-happens-after-i-cancel-my-subscription"></a>Wat gebeurt er wanneer ik mijn abonnement annuleren?

Wanneer u annuleert, wordt de facturering wordt onmiddellijk gestopt. Echter kunt duurt maximaal tien minuten voor de annulering om weer te geven in de portal.

Daarna worden uw services zijn uitgeschakeld. Dit betekent dat uw virtuele machines zijn toewijzing ongedaan, tijdelijke IP-adressen worden vrijgegeven en opslag is alleen-lezen.

Als u in het midden van een factureringsperiode annuleert, sturen we de uiteindelijk factuur op uw normale factuurdatum na afloop van de periode. 

We wacht 90 dagen definitief verwijderd van uw gegevens in het geval u nodig hebt om deze te openen of u van gedachten verandert. We niet in rekening gebracht die de gegevens bewaren. Zie voor meer informatie, [Microsoft Trust Center - hoe we uw gegevens beheren](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Abonnement opnieuw activeren

Als u per ongeluk uw betalen per gebruik-abonnement annuleert, kunt u [opnieuw activeren in het Accountcentrum](billing-subscription-become-disable.md).

Als uw abonnement niet betalen per gebruik, moet u contact op met ondersteuning binnen 90 dagen na annulering uw abonnement opnieuw activeren.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
