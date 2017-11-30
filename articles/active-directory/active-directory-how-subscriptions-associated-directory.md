---
title: Een bestaand Azure-abonnement toevoegen aan Azure Active Directory | Microsoft Docs
description: Een bestaand abonnement toevoegen aan Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/17/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 67df6d893c0770b9210fc73e96865d5c6396796c
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-add-an-azure-subscription-to-azure-active-directory"></a>Een Azure-abonnement toevoegen aan Azure Active Directory
In dit artikel vindt u informatie over de relatie tussen een Azure-abonnement en Azure Active Directory (Azure AD), en instructies voor het toevoegen van een bestaand abonnement aan uw Azure AD-directory. Uw Azure-abonnement heeft een vertrouwensrelatie met Azure AD, wat betekent dat de directory wordt vertrouwd voor de authenticatie van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde directory vertrouwen, maar een abonnement kan slechts één directory vertrouwen. 

De vertrouwensrelatie die een abonnement heeft met een directory is anders dan de relatie die een abonnement heeft met andere resources in Azure (websites, databases, enzovoort). Als een abonnement is verlopen, wordt de toegang tot de andere resources die zijn gekoppeld aan het abonnement, ook geblokkeerd. Een Azure AD-directory blijft echter wel aanwezig in Azure, en u kunt er een ander abonnement aan koppelen om de directory te beheren met het nieuwe abonnement.

Alle gebruikers hebben één basisdirectory waarmee ze worden geverifieerd, maar ze kunnen ook gasten zijn in andere directory’s. In Azure AD kunt u de directory’s zien waarvan uw gebruikersaccount lid of gast is.

## <a name="to-add-an-existing-subscription-to-your-azure-ad-directory"></a>Een bestaand abonnement toevoegen aan uw Azure AD-directory
U moet zich aanmelden met een account dat bestaat in zowel de huidige directory die aan het abonnement is gekoppeld als in de directory waaraan u het abonnement wilt toevoegen. 

1. Meld u aan bij het [Azure-accountcentrum](https://account.azure.com/Subscriptions) met een account van de accountbeheerder voor het abonnement waarvan u het eigendom wilt overdragen.
2. Zorg ervoor dat de gebruiker die u wilt instellen als de eigenaar van het abonnement, aanwezig is in de doeldirectory.
3. Klik op **Abonnement overdragen**.
4. Geef de ontvanger op. De ontvanger ontvangt automatisch een e-mail met een acceptatielink.
5. De ontvanger klikt op de link en volgt de instructies, waaronder het invoeren van zijn of haar betaalgegevens. Als de ontvanger klaar is, wordt het abonnement overgedragen. 
6. De standaarddirectory van het abonnement wordt gewijzigd in de directory waarin de gebruiker zich bevindt.


## <a name="next-steps"></a>Volgende stappen
* Zie [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md) voor meer informatie over het wijzigen van de beheerders voor een Azure-abonnement.
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](active-directory-understanding-resource-access.md)
* Zie [Beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) voor meer informatie over het toewijzen van rollen in Azure AD.

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
