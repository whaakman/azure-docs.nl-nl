---
title: Een bestaand Azure-abonnement toevoegen aan Azure Active Directory | Microsoft Docs
description: Een bestaand abonnement toevoegen aan Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/12/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: e063e6a46b6b99c4bbe749347e6887a930adb882
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Een Azure-abonnement koppelen of toevoegen aan Azure Active Directory

In dit artikel vindt u informatie over de relatie tussen een Azure-abonnement en Azure Active Directory (Azure AD), en instructies voor het toevoegen van een bestaand abonnement aan uw Azure AD-directory. Uw Azure-abonnement heeft een vertrouwensrelatie met Azure AD, wat betekent dat de directory wordt vertrouwd voor de authenticatie van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde directory vertrouwen, maar een abonnement kan slechts één directory vertrouwen. 

De vertrouwensrelatie die een abonnement heeft met een directory is anders dan de relatie die een abonnement heeft met andere resources in Azure (websites, databases, enzovoort). Als een abonnement is verlopen, wordt de toegang tot de andere resources die zijn gekoppeld aan het abonnement, ook geblokkeerd. Een Azure AD-directory blijft echter wel aanwezig in Azure, en u kunt er een ander abonnement aan koppelen om de directory te beheren met het nieuwe abonnement.

Alle gebruikers hebben één basisdirectory waarmee ze worden geverifieerd, maar ze kunnen ook gasten zijn in andere directory’s. In Azure AD kunt u de directory’s zien waarvan uw gebruikersaccount lid of gast is.

## <a name="before-you-begin"></a>Voordat u begint

* U moet zich aanmelden met een account waarvoor u als RBAC-eigenaar toegang heeft tot het abonnement.
* U moet zich aanmelden met een account dat bestaat in zowel de huidige directory die aan het abonnement is gekoppeld als in de directory waaraan u het abonnement wilt toevoegen. Zie [Hoe voegen beheerders van Azure Active Directory gebruikers van B2B-samenwerking toe?](active-directory-b2b-admin-add-users.md) voor meer informatie over het verkrijgen van toegang tot een andere directory.

## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Een bestaand abonnement koppelen aan uw Azure AD Directory

1. Meld u aan en selecteer een abonnement op de [pagina met abonnementen in Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Klik op **Directory wijzigen**.

    ![Schermafbeelding van de knop Directory wijzigen](./media/active-directory-how-subscriptions-associated-directory/edit-directory-button.PNG)
3. Bekijk de waarschuwingen. Alle [RBAC](role-based-access-control-configure.md)-gebruikers (Role-Based Access Control, op rollen gebaseerd toegangsbeheer) met toegewezen toegang en alle abonnementsbeheerders verliezen toegang wanneer de directory van het abonnement wordt gewijzigd.
4. Selecteer een directory.

    ![Schermafbeelding waarin de directory wordt gewijzigd](./media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.PNG)
5. Klik op **Wijzigen**.
6. Gelukt! Gebruik de directorywisselaar om naar de nieuwe directory te gaan. Het kan maximaal 10 minuten duren voordat alles goed wordt weergegeven.

    ![Schermafbeelding met het bericht dat het overschakelen naar een andere directory is geslaagd](./media/active-directory-how-subscriptions-associated-directory/edit-directory-success.PNG)

    ![Schermafbeelding van de wisselaar](./media/active-directory-how-subscriptions-associated-directory/directory-switcher.PNG)


Het wijzigen van de abonnementsdirectory is een bewerking op serviceniveau. Dit heeft geen invloed op het eigendom van het abonnement. De accountbeheerder kan de servicebeheerder nog steeds wijzigen met behulp van het [Account-centrum](https://account.azure.com/subscriptions). Als u de originele directory wilt verwijderen, moet u het eigendom van het abonnement overdragen op een nieuwe accountbeheerder. Zie [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md) voor meer informatie over het overdragen van het eigendom. 

## <a name="next-steps"></a>Volgende stappen

* Zie [Een Azure Active Directory-tenant verkrijgen](develop/active-directory-howto-tenant.md) voor meer informatie over het gratis maken van een nieuwe Azure AD-directory.
* Zie [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md) voor meer informatie over het overdragen van het eigendom van een Azure-abonnement.
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](active-directory-understanding-resource-access.md)
* Zie [Beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) voor meer informatie over het toewijzen van rollen in Azure AD.

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
