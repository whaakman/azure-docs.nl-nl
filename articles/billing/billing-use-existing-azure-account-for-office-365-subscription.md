---
title: Aanmelden voor Office 365 met Azure-account | Microsoft Docs
description: Informatie over het maken van een Office 365-abonnement met behulp van een Azure-account
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: 
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: cjiang
ms.openlocfilehash: cd6103924571ea6fee0cb1e696884a6106082d7a
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Aanmelden voor een Office 365-abonnement met uw Azure-account
Als u Azure-abonnee bent, kunt u uw Azure-account aan te melden voor een Office 365-abonnement. Als u deel uitmaakt van een organisatie met een Azure-abonnement bent, kunt u Office 365-abonnementen voor gebruikers in uw bestaande Azure Active Directory (Azure AD). Meld u aan bij Office 365 met een account met machtigingen voor facturerings-beheerder of de globale beheerder in uw Azure Active Directory-tenant. Zie voor meer informatie [Controleer de accountmachtigingen van mijn in Azure AD](#RoleInAzureAD) en [beheerdersrollen toewijzen in Azure Active Directory](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

Als u al een Office 365-account en een Azure-abonnement hebt, kunt u [koppelen van een Office 365-tenant met een Azure-abonnement](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Office 365-abonnement met behulp van uw Azure-account ophalen

1. Ga naar de [Office 365-productpagina](https://products.office.com/business), en selecteert u een plan.
2. Klik op **aanmelden** in de rechterbovenhoek van de pagina.

    ![Schermafbeelding van pagina proef met Office 365](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Aanmelden met de referenties van uw Azure-account. Als u een abonnement voor uw organisatie maakt, gebruikt u een Azure-account dat lid is van de rol globale beheerder of beheerder facturering directory in uw Azure Active Directory-tenant.

    ![Schermopname van Office 365-aanmeldingspagina](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. Klik op **Probeer nu**.

    ![Schermopname die wordt bevestigd uw bestelling voor Office 365 dat.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. Klik op de pagina van de ontvangst bestelling **doorgaan**.

    ![Schermafbeelding van de ontvangst van de Office 365-order](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

Nu bent u nu klaar. Als u de Office 365-abonnement hebt gemaakt voor uw organisatie, gebruikt u de volgende stappen uit om te controleren dat uw Azure AD-gebruikers zich nu in Office 365.

1. Open het Office 365-beheercentrum.
2. Vouw **gebruikers**, en klik vervolgens op **actieve gebruikers**.

    ![Schermafbeelding van de Office 365 admin center-gebruikers](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

Nadat u zich aanmeldt, wordt de Office 365-abonnement toegevoegd aan het Azure Active Directory-exemplaar dat uw Azure-abonnement behoort. Zie voor meer informatie [meer over Azure en Office 365-abonnementen](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) en [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Controleer de accountmachtigingen van mijn in Azure AD
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **meer services**, en zoek vervolgens naar **Active Directory**.

    ![Schermopname van Active Directory in de Azure portal](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. Klik op **gebruikers en groepen** > **alle gebruikers**.
4. Selecteer de naam van de gebruiker. 

    ![Schermafbeelding van de Azure Active Directory-gebruikers](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. Klik op **functie Directory**.
  
    ![Schermafbeelding van de functie Azure portal directory](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  De rol **hoofdbeheerder** of **beperkt beheerder** > **financieel medewerker** is vereist voor gebruikers in Office 365-abonnement maken uw bestaande Azure Active Directory.

    ![Schermafbeelding van Azure portal directory-rol beheerder facturering](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost. 
