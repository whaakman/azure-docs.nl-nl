---
title: Aanmelden voor Office 365 met Azure-account | Microsoft Docs
description: Informatie over het maken van een Office 365-abonnement met behulp van een Azure-account
services: ''
documentationcenter: ''
author: JiangChen79
manager: adpick
editor: ''
tags: billing,top-support-issue
ms.assetid: ''
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.openlocfilehash: 0b5e7a36c89c594dd0a7ec681b112ea523884461
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904148"
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Aanmelden voor Office 365-abonnement met uw Azure-account
Als u Azure-abonnee bent, kunt u uw Azure-account om u te registreren voor een Office 365-abonnement. Als u deel uitmaakt van een organisatie met een Azure-abonnement bent, kunt u Office 365-abonnementen voor gebruikers in uw bestaande Azure Active Directory (Azure AD). Meld u aan bij Office 365 met een account met globale beheerder of factureringsbeheerder machtigingen in uw Azure Active Directory-tenant. Zie voor meer informatie, [Controleer de accountmachtigingen van mijn in Azure AD](#RoleInAzureAD) en [beheerdersrollen toewijzen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

Als u al een Office 365-account en een Azure-abonnement hebt, kunt u [een Office 365-tenant met een Azure-abonnement koppelen](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Office 365-abonnement ophalen met behulp van uw Azure-account

1. Ga naar de [Office 365-productpagina](https://products.office.com/business), en een abonnement selecteren.
2. Klik op **aanmelden** in de rechterbovenhoek van de pagina.

    ![schermopname van de proefversie Office 365-pagina](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Meld u aan met uw Azure-accountreferenties. Als u een abonnement voor uw organisatie maakt, gebruikt u een Azure-account dat lid is van de rol globale beheerder of factureringsbeheerder directory in uw Azure Active Directory-tenant.

    ![Schermafbeelding van Office 365-aanmelding](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. Klik op **Probeer nu**.

    ![Schermafbeelding waarmee wordt bevestigd uw order voor Office 365 dat.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. Klik op de pagina van de ontvangst bestelling **doorgaan**.

    ![Schermopname van het ontvangstbewijs Office 365](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

Nu bent nu u klaar. Als u het Office 365-abonnement voor uw organisatie hebt gemaakt, gebruikt u de volgende stappen uit om te controleren of uw Azure AD-gebruikers zich nu in Office 365.

1. De Office 365-beheercentrum te openen.
2. Vouw **gebruikers**, en klik vervolgens op **actieve gebruikers**.

    ![Schermafbeelding van de gebruikers van Office 365 admin center](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

Nadat u bent aangemeld, wordt het Office 365-abonnement wordt toegevoegd aan hetzelfde exemplaar van Azure Active Directory dat uw Azure-abonnement behoort. Zie voor meer informatie, [meer over Azure en Office 365-abonnementen](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) en [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Controleer de accountmachtigingen van mijn in Azure AD
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **alle services**, en zoek vervolgens **Active Directory**.

    ![Schermafbeelding van de Active Directory in Azure portal](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. Klik op **gebruikers en groepen** > **alle gebruikers**.
4. Selecteer de naam van de gebruiker. 

    ![Schermafbeelding van de Azure Active Directory-gebruikers](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. Klik op **maprol**.
  
    ![Schermafbeelding van de rol van de directory van Azure portal](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  De rol **hoofdbeheerder** of **beperkte beheerder** > **factureringsbeheerder** is vereist voor het maken van een Office 365-abonnement voor gebruikers in uw bestaande Azure Active Directory.

    ![Schermafbeelding van Azure portal directory-rol factureringsbeheerder](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). 
