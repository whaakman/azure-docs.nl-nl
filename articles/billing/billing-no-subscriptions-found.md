---
title: Geen abonnementen gevonden fout wanneer probeert te melden bij Azure portal of Azure-accountcentrum | Microsoft Docs
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
ms.author: cwatson
ms.openlocfilehash: 669e597424ae936dda59564ad072ac7d77107321
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276155"
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Geen abonnementen gevonden fout in Azure portal of Azure-accountcentrum

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

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) om uw probleem snel worden opgelost. 
