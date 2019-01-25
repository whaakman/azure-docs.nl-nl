---
title: Geen abonnementen gevonden fout - Azure portal aanmelden | Microsoft Docs
description: Is de oplossing voor een probleem opgetreden bij het waarin er geen abonnementen gevonden fout treedt op wanneer zich aanmelden bij Azure portal of Azure-accountcentrum.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: c6a2f14900d3a0d6f9e16e9b0c6d0bdfff97d6b5
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54903833"
---
# <a name="no-subscriptions-found-sign-in-error-for-azure-portal-or-azure-account-center"></a>Geen abonnementen gevonden sign in fout voor Azure-portal of Azure-accountcentrum

Mogelijk krijgt u een foutbericht 'Er zijn geen abonnementen gevonden' weergegeven wanneer u probeert te melden bij de [Azure-portal](https://portal.azure.com/) of de [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions). In dit artikel biedt een oplossing voor dit probleem.

## <a name="symptom"></a>Symptoom

Als u probeert te melden bij de [Azure-portal](https://portal.azure.com/) of de [Azure account center](https://account.windowsazure.com/Subscriptions), ontvangt u de volgende strekking weergegeven: 'Geen abonnementen gevonden'.

## <a name="cause"></a>Oorzaak

Dit probleem treedt op als u hebt geselecteerd op de verkeerde directory, of als uw account beschikt niet over voldoende machtigingen. 

## <a name="solution"></a>Oplossing

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a>Scenario 1: Foutbericht wordt ontvangen in de [Azure-portal](https://portal.azure.com)

Dit probleem oplossen:

* Zorg ervoor dat de juiste Azure-map is geselecteerd door te klikken op uw account in de rechterbovenhoek.

  ![Selecteer de map aan de bovenkant van de Azure-portal](./media/billing-no-subscriptions-found/directory-switch.png)
* Als de juiste Azure-map is geselecteerd, maar u nog steeds het foutbericht ontvangen [de rol van eigenaar toewijzen aan uw account](../role-based-access-control/role-assignments-portal.md).

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Scenario 2: Foutbericht wordt ontvangen in de [Accountcentrum van Azure](https://account.windowsazure.com/Subscriptions)

Controleer of het account dat u gebruikt de accountbeheerder. Om te controleren wie de accountbeheerder is, de volgende stappen uit:

1. Aanmelden bij de [abonnementen weergeven in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren, en ga vervolgens naar **instellingen**.
1. Selecteer **eigenschappen**. De accountbeheerder voor het abonnement wordt weergegeven in de **accountbeheerder** vak.  

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
