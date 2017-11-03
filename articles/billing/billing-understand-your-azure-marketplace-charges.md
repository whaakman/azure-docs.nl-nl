---
title: Inzicht in uw Azure externe servicekosten | Microsoft Docs
description: Meer informatie over de facturering van externe services, voorheen bekend als Marketplace, kosten in Azure.
services: 
documentationcenter: 
author: adpick
manager: tonguyen
editor: 
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 64bfd8581141001aa6c11ca17ec1af681054f490
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a>Inzicht in uw Azure-facturering voor de externe service kosten
Externe services zijn gepubliceerd door derden softwareleveranciers in de Azure marketplace. ClearDB en SendGrid zijn bijvoorbeeld externe services die u in Azure kunt aanschaffen, maar niet zijn gepubliceerd door Microsoft.

Wanneer u een nieuwe externe service of de resource inricht, wordt een waarschuwing weergegeven:

![Marketplace kopen waarschuwing](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> Externe services zijn gepubliceerd door bedrijven die geen Microsoft, maar soms ook Microsoft-producten zijn aangemerkt als externe services.
> 
> 

## <a name="how-external-services-are-billed"></a>Hoe externe services worden gefactureerd
- Externe services worden afzonderlijk gefactureerd. Ze worden behandeld als afzonderlijke orders binnen uw Azure-abonnement. De factureringsperiode voor elke service is ingesteld bij de aankoop van de service. Niet te verwarren met de factureringsperiode van het abonnement waaronder u die hebt aangeschaft. U krijgt ook afzonderlijk facturen en uw creditcard wordt afzonderlijk in rekening gebracht.
- Elke externe service heeft een ander facturering model. Sommige services worden gefactureerd op betalen naar gebruik wijze bij anderen gebruik maken van een maandelijkse gebaseerde betaling-model. Hebt u een creditcard nodig voor de externe Azure-services, kan niet het kopen van externe services met factuur betalen.
- U kunt maandelijks gratis tegoed niet gebruiken voor externe services. Als u een Azure-abonnement met [gratis tegoed](https://azure.microsoft.com/pricing/spending-limits/), ze kunnen niet worden toegepast op de externe service facturen. Een creditcard gebruiken voor de aanschaf van externe services.

## <a name="view-external-service-spending-and-history-in-the-azure-portal"></a>Weergave externe service uitgaven en geschiedenis in de Azure portal
U kunt een lijst weergeven met de externe services die zich op elk abonnement binnen de [Azure-portal](https://portal.azure.com/): 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als de accountbeheerder.
2. Selecteer in het menu Hub **abonnementen**.
   
    ![Selecteer de abonnementen in het Hub-menu](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. In de **abonnementen** blade, selecteer het abonnement dat u wilt weergeven en selecteer vervolgens **externe services**.
   
    ![Selecteer een abonnement op de blade facturering](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. U ziet elk van uw bestellingen externe service, de naam van uitgever, servicelaag die u hebt aangeschaft, hebt u de bron en de status van de huidige gegeven naam. Om te zien voorbij facturen, selecteert u een externe service.
   
    ![Selecteer een externe service](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. Hier kunt u weergeven uit het verleden factuur bedragen, met inbegrip van de btw-verdeling.
   
    ![Weergave van de externe services factureringsgeschiedenis](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>Weergave van de externe service uitgaven voor klanten met Enterprise Agreement (EA)
EA klanten kunnen zien van de externe service uitgaven en rapporten in de portal EA downloaden. Zie [Azure Marketplace voor EA klanten](https://ea.azure.com/helpdocs/azureMarketplace) aan de slag.

## <a name="manage-payment-methods-for-external-service-orders"></a>Betalingswijzen voor externe serviceorders beheren
Uw betaling updatemethoden voor de externe serviceorders uit de [Accountcentrum](https://account.windowsazure.com/).

> [!NOTE]
> Als u uw abonnement met een werk of School-account hebt gekocht [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) wijzigingen aanbrengen in uw betalingswijze.
> 
> 

1. Aanmelden bij de [Accountcentrum](https://account.windowsazure.com/) en [gaat u naar de **marketplace** tabblad](https://account.windowsazure.com/Store)
   
    ![Selecteer marketplace in het midden van het account](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Selecteer de externe service die u wilt beheren
   
    ![Selecteer de externe service die u wilt beheren](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Klik op **betalingsmethode wijzigen** aan de rechterkant van de pagina. Deze koppeling keert u terug naar een andere portal voor het beheren van uw betalingswijze.
   
    ![Volgorde-samenvatting](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Klik op **info bewerken** en volg de instructies voor het bijwerken van uw betalingsgegevens.
   
    ![Selecteer Bewerken info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>Een externe serviceorder annuleren
Als u uw externe service bestelling annuleert wilt, verwijdert u de resource in de [Azure-portal](https://portal.azure.com).

![Bron verwijderen](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Hebt u nog steeds vragen, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

