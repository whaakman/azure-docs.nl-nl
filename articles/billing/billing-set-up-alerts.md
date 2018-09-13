---
title: Waarschuwingen voor facturering of tegoed voor Azure-abonnementen instellen | Microsoft Docs
description: Hierin wordt beschreven hoe u kunt waarschuwingen instellen op uw Azure-factuur, zodat u cloudanalyses kunt vermijden.
keywords: tegoed waarschuwing, facturering waarschuwing
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: adpick
ms.openlocfilehash: 094bfe041ae04e52b6d998ccfd1d964abf192d6a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35917193"
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>Waarschuwingen voor facturering of creditcard voor uw Microsoft Azure-abonnementen instellen
Als u de accountbeheerder voor een Azure-abonnement bent, kunt u de Azure Service voor Facturatiemeldingen te maken van aangepaste facturatiemeldingen waarmee u kunt bewaken en beheren van de facturatieactiviteit voor uw Azure-accounts.

Deze service is in preview, dus moet u eerst op de pagina voor Preview-functies inschakelen.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="set-the-alert-threshold-and-email-recipients"></a>Instellen van de ontvangers van de waarschuwing drempelwaarde en e-mailadres
1. Ga naar [de pagina met Voorbeeldfuncties](https://account.windowsazure.com/PreviewFeatures) en in te schakelen **Service voor Facturatiemeldingen**.

1. Nadat u de e-mailbevestiging die de facturering-service is ingeschakeld voor uw abonnement ontvangt, gaat u naar [de pagina abonnementen](https://account.windowsazure.com/Subscriptions) in de accountportal. Klik op het abonnement dat u wilt controleren, en klik vervolgens op **waarschuwingen**.

    ![Schermafbeelding van de weergave abonnementen van Azure-accountcentrum, met waarschuwingen die zijn gemarkeerd][Image1]

2. Klik vervolgens op **waarschuwing toevoegen** uw eerste account te maken. U kunt een totaal van vijf mailmeldingen voor facturering per abonnement, met een andere drempelwaarde en maximaal twee e-mailontvangers voor elke waarschuwing instellen.

    ![Schermafbeelding van de weergave van waarschuwingen, waar u de waarschuwing kunt toevoegen][Image2]

3. Wanneer u een waarschuwing toevoegt, u een unieke naam geven, kiest u een bestedingslimiet drempelwaarde en kiest u het e-mailadres waarnaar meldingen worden verzonden. Bij het instellen van de drempelwaarde, kunt u ofwel een **totale facturering** of een **Geldtegoed** uit de **waarschuwing voor** lijst. Voor facturering totaal, een waarschuwing verzonden wanneer de drempelwaarde voor de uitgavelimiet abonnement overschrijdt. Voor een financieel tegoed een waarschuwing verzonden wanneer een financieel tegoed beneden de limiet. Financieel tegoed zijn gewoonlijk van toepassing op de gratis proefversie en Visual Studio-abonnementen.

    ![Schermafbeelding van de weergave van waarschuwingen toevoegen, kunt u de ontvangers configureren][Image3]

Azure biedt ondersteuning voor elk e-mailadres, maar niet verifiëren dat de e-mailadres werkt, dus Controleer voor typfouten.

## <a name="check-on-your-alerts"></a>Controleren op meldingen
Na het instellen van waarschuwingen, wordt het Accountcentrum geeft een lijst van deze en laat zien hoe veel meer kunt u instellen. Voor elke waarschuwing ziet u de datum en tijd waarop die deze is verzonden, of een waarschuwing voor de totale facturering of financieel tegoed is en de limiet die u instelt. De datum- en tijdnotatie is coördinatie van 24-uurs-Universal Time (UTC) en de datum is jjjj-mm-dd-indeling. Klik op het plusteken voor een waarschuwing in de lijst om het te bewerken of klik op de Prullenbak om het te verwijderen.

## <a name="delete-alerts-or-email-addresses-from-the-azure-billing-alert-service"></a>Waarschuwingen of e-mailadressen van de Azure Service voor Facturatiemeldingen verwijderen
Als u ooit verwijderen van alle gegevens uit de service wilt, het e-mailadres in het bestand bijwerken of volledig verwijderen van de waarschuwing.

   ![Schermafbeelding van de weergave waarschuwingen verwijderen, waar u de persoonlijke gegevens kunt verwijderen][Image4]

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>Meldingen voor facturering voor Enterprise Agreement (EA)-klanten
EA-abonnementen worden niet ondersteund door deze service, in plaats daarvan EA-klanten waarschuwingen kunnen ontvangen voor elke afdeling onder een inschrijving via het instellen van quota uitgaven. Zie [afdeling uitgaven quota](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in de EA-portal aan de slag.

## <a name="learn-more-about-azure-cost-management"></a>Meer informatie over Azure kostenbeheer
- Geschatte kosten met behulp van de [prijscalculator](https://azure.microsoft.com/pricing/calculator/), [totale kosten van eigendom Rekenmachine](https://aka.ms/azure-tco-calculator), en wanneer u een service toevoegt.
- [Uw gebruik en kosten regelmatig in Azure portal bekijken](billing-getting-started.md#costs).
- Schakel [Azure Advisor aanbevelingen kosten](../advisor/advisor-cost-recommendations.md).

Zie voor meer informatie, [Azure kosten management](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
[Image4]: ./media/azure-billing-set-up-alerts/AlertsDeleteScreen1.PNG
