---
title: Facturerings- of waarschuwingen voor Azure-abonnementen instellen | Microsoft Docs
description: Hierin wordt beschreven hoe u kunt waarschuwingen instellen op uw Azure-factuur zodat u kunt facturering verrassingen vermijden.
keywords: tegoed waarschuwing facturering waarschuwing
services: 
documentationcenter: 
author: vikdesai
manager: tonguyen
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2017
ms.author: vikdesai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1fc0cb2b036e835450ee0fc404cce12439fabc77
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>Facturerings- of waarschuwingen instellen voor uw Microsoft Azure-abonnementen
Als u de accountbeheerder voor een Azure-abonnement bent, kunt u de Azure Billing waarschuwingsservice voor het maken van aangepaste factureringsmeldingen die u helpen controleren en beheren van facturerings activiteit voor uw Azure-accounts.

Deze service is in de preview, dus moet u eerst op de pagina met Preview-functies inschakelen.

## <a name="set-the-alert-threshold-and-email-recipients"></a>De drempelwaarde en e-ontvangers van de waarschuwing instellen
1. Ga naar [de Preview-functies pagina](https://account.windowsazure.com/PreviewFeatures) en schakel **waarschuwingsservice facturering**.

1. Nadat u het e-mailbevestiging die de facturering-service is ingeschakeld voor uw abonnement ontvangt, gaat u naar [de pagina abonnementen](https://account.windowsazure.com/Subscriptions) in de accountportal. Klik op het abonnement dat u wilt bewaken, en klik vervolgens op **waarschuwingen**.

    ![Schermafbeelding van de weergave abonnementen van Azure-accountcentrum, met waarschuwingen die zijn gemarkeerd][Image1]

2. Klik vervolgens op **waarschuwing toevoegen** zelf een maken. U kunt een totaal van vijf factureringsmeldingen per abonnement, met een andere drempelwaarde en maximaal twee e-mailontvangers voor elke waarschuwing instellen.

    ![Schermafbeelding van de weergave van waarschuwingen, waarin u de waarschuwing kunt toevoegen][Image2]

3. Wanneer u een waarschuwing toevoegt, u een unieke naam geven, kies een bestedingslimiet drempelwaarde, en kies de e-mailadressen waarop waarschuwingen worden verzonden. Wanneer de drempel instelt, kunt u ofwel een **totale facturering** of een **financieel tegoed** van de **waarschuwing voor** lijst. Voor een totaal facturering, wordt een waarschuwing verzonden wanneer abonnement uitgaven de drempelwaarde overschrijdt. Voor een financieel tegoed is een waarschuwing verzonden wanneer monetaire krediet minder wordt dan de limiet. Monetaire krediet doorgaans de toepassing aan abonnementen, gratis proefversie en Visual Studio.

    ![Schermafbeelding van de weergave van waarschuwing toevoegen, waar u de geadresseerden kunt configureren][Image3]

Azure biedt ondersteuning voor elk e-mailadres, maar niet verifiëren dat de e-mailadres werken, dus Controleer voor typfouten.

## <a name="check-on-your-alerts"></a>Controleer op waarschuwingen
Na het instellen van waarschuwingen, wordt het Account Center vermeldt ze en toont hoe veel meer kunt u instellen. Voor elke waarschuwing ziet u de datum en tijdstip van verzending, of een waarschuwing voor totale facturering of financieel tegoed is en de limiet die u instelt. De datum en tijd-indeling is coördineren 24-uurs-Universal Time (UTC) en de datum is jjjj-mm-dd-indeling. Klik op het plusteken voor een waarschuwing in de lijst om het te bewerken, of klik op de Prullenbak om het te verwijderen.

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>Waarschuwingen voor facturering voor klanten met Enterprise Agreement (EA)
EA klanten kunnen waarschuwingen krijgen voor elke afdeling onder een inschrijving door de instelling uitgaven quota's. Zie [afdeling uitgaven quota](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in de portal EA aan de slag.

## <a name="learn-more-about-azure-cost-management"></a>Meer informatie over het kostenbeheer van Azure
- Geschatte kosten met behulp van de [prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/), [totale kosten van eigendom Rekenmachine](https://aka.ms/azure-tco-calculator), en wanneer u een service toevoegen.
- [Controleer de informatie over het gebruik en kosten regelmatig in Azure-portal](billing-getting-started.md#costs).
- Schakel [Azure Advisor kosten aanbevelingen](../advisor/advisor-cost-recommendations.md).

Zie voor meer informatie, [Azure kosten management richtlijnen](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
