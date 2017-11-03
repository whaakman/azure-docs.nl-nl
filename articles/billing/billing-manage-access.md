---
title: Toegang tot Azure-facturering rollen beheren | Microsoft Docs
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: c70904097f139bc2178feed83f1cf1274f3c738d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a>Toegang tot factureringsgegevens voor Azure met behulp van op rollen gebaseerde toegangsbeheer beheren

U kunt de toegang voor Azure factureringsgegevens voor leden van uw team verlenen door een van de volgende gebruikersrollen toewijzen aan uw abonnement: accountbeheerder, servicebeheerder medebeheerder, eigenaar, bijdrager, lezer en facturering Reader. Ze zou hebben toegang tot factureringsgegevens in de [Azure-portal](https://portal.azure.com/), en ze kunnen gebruiken de [facturering API's](billing-usage-rate-card-overview.md) programmatisch ophalen facturen (eenmaal heeft gekozen-in) en informatie over het gebruik. Voor meer informatie over wie rollen kunt toewijzen, en welke rollen kan wat te doen, Zie [rollen in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).

## <a name="opt-in"></a>Deze extra gebruikers toegang krijgen tot facturen

De accountbeheerder moet aanmelden met behulp van de [Azure-portal](https://portal.azure.com/) toegang toestaan tot facturen voor andere gebruikers en via de API.

1. Als de accountbeheerder, selecteer uw abonnement uit de [abonnementen blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure-portal.

1. Selecteer **facturen** en vervolgens **toegang tot facturen**.

    ![Schermafbeelding ziet u hoe u toegang tot facturen delegeren](./media/billing-manage-access/AA-optin.png)

1. Schakel **op** rollen factuur downloaden binnen het bereik van de toegang gevolgd door het opslaan van de wijzigingen, zodat gebruikers in het abonnement.

    ![Schermafbeelding ziet u op uit een gemachtigde naar factuur](./media/billing-manage-access/AA-optinAllow.png)

Kiest servicebeheerder, medebeheerder eigenaar, bijdrager, lezer en facturering lezer, kan voor het abonnement voor het downloaden van facturen PDF-bestand in de Azure portal. Facturen die ouder zijn dan December 2016 zijn echter beschikbaar alleen naar de accountbeheerder nu.

De accountbeheerder kan ook configureren als u wilt dat facturen verzonden via e-mail. Zie voor meer informatie, [uw factuur ophalen in e-mailbericht](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-to-the-billing-reader-role"></a>Gebruikers toevoegen aan de rol Lezer facturering

De rol Lezer facturering heeft alleen-lezen toegang tot de factureringsgegevens abonnement in Azure-portal en geen toegang tot services, zoals virtuele machines en opslagaccounts. De rol Lezer facturering toewijzen aan iemand die toegang tot de factureringsgegevens abonnement, maar niet de mogelijkheid voor het beheren van Azure-services nodig. Deze rol is geschikt voor gebruikers in een organisatie die alleen financiële en kosten-beheer voor Azure-abonnementen uitvoeren.

1. Selecteer uw abonnement uit de [abonnementen blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure-portal.

1. Selecteer **toegangsbeheer (IAM)** en klik vervolgens op **toevoegen**.

    ![Schermafbeelding ziet u IAM in de abonnementsblade](./media/billing-manage-access/select-iam.PNG)

1. Kies **facturering lezer** in de **Selecteer een rol** pagina.

    ![Schermafbeelding ziet u lezer facturering in het pop-weergave](./media/billing-manage-access/select-roles.PNG)

1. Typ het e-mailadres voor de gebruiker die u wilt uitnodigen en klik vervolgens op **OK** de uitnodiging te verzenden.

    ![Schermafbeelding van e-mailadres om uit te nodigen iemand invoeren](./media/billing-manage-access/add-user.PNG)

1. Volg de instructies in de e-mail uitnodiging voor aanmelden als een lezer facturering.

    ![Schermopname die laat zien wat de lezer facturering kunnen zien in Azure-portal](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> De functie facturering lezer is in preview en ondersteunt nog geen enterprise (EA) abonnementen of niet-algemene clouds.

## <a name="adding-users-to-other-roles"></a>Gebruikers toevoegen aan andere functies

Gebruikers in andere rollen, zoals eigenaar of bijdrager, hebben toegang tot niet alleen factureringsgegevens, maar ook Azure services. Zie voor het beheren van deze rollen [toevoegen of wijzigen Azure-beheerdersrollen die het abonnement of de services beheren](billing-add-change-azure-subscription-administrator.md).

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a>Wie toegang heeft tot de [Accountcentrum](https://account.windowsazure.com)?

Alleen de accountbeheerder kunt aanmelden bij de accountcentrum. De accountbeheerder is de juridische eigenaar van het abonnement. De persoon die zich heeft aangemeld of het Azure-abonnement hebt gekocht is de accountbeheerder, tenzij de [eigendom van het abonnement is overgezet](billing-subscription-transfer.md) naar iemand anders. De accountbeheerder kan abonnementen maken, abonnementen annuleren, het factuuradres voor een abonnement te wijzigen en -beleid voor het abonnement te beheren.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
