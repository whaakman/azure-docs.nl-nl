---
title: Beheer de toegang tot Azure met behulp van rollen facturering | Microsoft Docs
description: ''
services: ''
documentationcenter: ''
author: vikramdesai01
manager: vikdesai
editor: ''
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/22/2017
ms.author: cwatson
ms.openlocfilehash: 38cfd354f11ef3d888ad70e71549868d398495f5
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429640"
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a>Toegang tot factureringsgegevens voor Azure met behulp van op rollen gebaseerd toegangsbeheer beheren

U kunt toegang voor Azure-Factureringsinformatie verlenen aan leden van uw team door een van de volgende gebruikersrollen toewijzen aan uw abonnement: accountbeheerder, servicebeheerder, CO-beheerder, eigenaar, Inzender, lezer en facturering lezer. Ze zouden hebben toegang tot factureringsgegevens in de [Azure-portal](https://portal.azure.com/), en kan worden gebruikt de [facturering-API's](billing-usage-rate-card-overview.md) om op te halen via een programma facturen (één keer gekozen-in) en informatie over het gebruik. Voor meer informatie over die rollen kunt toewijzen, en welke functies kunt doen wat, Zie [rollen in Azure RBAC](../role-based-access-control/built-in-roles.md).

## <a name="opt-in"></a> Deze extra gebruikers toegang krijgen tot facturen

De accountbeheerder moet aanmelden met behulp van de [Azure-portal](https://portal.azure.com/) toestaan van toegang tot facturen voor andere gebruikers en via de API.

1. Als de accountbeheerder, selecteer uw abonnement uit de [blade abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.

1. Selecteer **facturen** en vervolgens **toegang tot facturen**.

    ![Schermafbeelding ziet u hoe u toegang tot facturen delegeren](./media/billing-manage-access/AA-optin.png)

1. Schakel **op** toegang tot de gevolgd door het opslaan van de wijzigingen, zodat gebruikers in het abonnement binnen het bereik van rollen voor het downloaden van facturen.

    ![Schermafbeelding ziet u in-uitschakelen voor toegang tot de factuur delegeren](./media/billing-manage-access/AA-optinAllow.png)

Service-beheerder, CO-beheerder, eigenaar, Inzender, lezer en factureren voor lezer kan inschrijving op het abonnement voor het downloaden van facturen PDF-bestand in Azure portal. Facturen die ouder zijn dan December 2016 zijn echter beschikbaar alleen naar de accountbeheerder nu.

De accountbeheerder kan ook configureren om facturen verzonden via e-mail. Zie voor meer informatie, [ophalen van uw factuur in e-mailbericht](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-to-the-billing-reader-role"></a>Gebruikers toevoegen aan de rol van lezer facturering

De rol factureren voor lezer heeft alleen-lezen toegang tot factureringsgegevens van abonnement in Azure portal en geen toegang tot services, zoals virtuele machines en opslagaccounts. De rol factureren voor lezer toewijzen aan iemand die toegang nodig tot de factureringsgegevens van het abonnement, maar niet de mogelijkheid heeft voor het beheren van Azure-services. Deze rol is geschikt voor gebruikers in een organisatie die alleen financiële en kosten beheer voor Azure-abonnementen uitvoeren.

1. Selecteer uw abonnement uit de [blade abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.

1. Selecteer **toegangsbeheer (IAM)** en klik vervolgens op **toevoegen**.

    ![Schermafbeelding ziet u IAM in de abonnementsblade](./media/billing-manage-access/select-iam.PNG)

1. Kies **factureren voor lezer** in de **Selecteer een rol** pagina.

    ![Schermafbeelding ziet u factureren voor lezer in de weergave van pop-upvenster](./media/billing-manage-access/select-roles.PNG)

1. Typ het e-mailbericht voor de gebruiker die u wilt uitnodigen en klik vervolgens op **OK** de uitnodiging te verzenden.

    ![Schermafbeelding van e-mailbericht om uit te nodigen iemand invoeren](./media/billing-manage-access/add-user.PNG)

1. Volg de instructies in het e-mailbericht voor de uitnodiging voor aanmelden als een factureren voor lezer.

    ![Schermopname die laat zien wat de factureren voor lezer kan zien in Azure portal](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> De functie factureren voor lezer is in preview en biedt niet-globale clouds nog geen ondersteuning. Enterprise-abonnementen kunnen kosten weergeven als de enterprise-beheerder kosten weergeven is ingeschakeld.

## <a name="adding-users-to-other-roles"></a>Gebruikers toevoegen aan andere rollen

Gebruikers in andere rollen, zoals eigenaar of bijdrager, hebben toegang tot niet alleen factureringsgegevens, maar ook Azure services. Zie voor het beheren van deze rollen, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a>Wie toegang heeft tot de [Accountcentrum](https://account.windowsazure.com)?

Alleen de accountbeheerder kan aanmelden bij het accountcentrum. De accountbeheerder is de juridische eigenaar van het abonnement. De persoon die zich heeft aangemeld of het Azure-abonnement hebt gekocht is de accountbeheerder, tenzij de [abonnement eigendom is overgedragen](billing-subscription-transfer.md) aan iemand anders. De accountbeheerder kan maken van abonnementen, abonnementen annuleren, het factuuradres voor een abonnement wijzigen en beheren van toegangsbeleid voor het abonnement.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
