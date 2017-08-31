---
title: Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory | Microsoft Docs
description: Problemen met aanmelden bij Microsoft Azure en verwante problemen, zoals problemen met de relatie tussen een Azure-abonnement en de Azure Active Directory.
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
ms.date: 08/22/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: b2f8cc774b6cc245a4000e5c6924a8bb817707bc
ms.contentlocale: nl-nl
ms.lasthandoff: 08/24/2017

---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>How Azure subscriptions are associated with Azure Active Directory (Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory)
In dit artikel vindt u informatie over de relatie tussen een Azure-abonnement en Azure Active Directory (Azure AD).

## <a name="your-azure-subscriptions-relationship-to-azure-ad"></a>De relatie tussen uw Azure-abonnement en Azure AD
Uw Azure-abonnement heeft een vertrouwensrelatie met Azure AD, wat betekent dat de directory wordt vertrouwd voor de authenticatie van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde directory vertrouwen, maar een abonnement kan slechts één directory vertrouwen. 

De vertrouwensrelatie die een abonnement heeft met een directory is anders dan de relatie die een abonnement heeft met andere resources in Azure (websites, databases, enzovoort). Als een abonnement is verlopen, wordt de toegang tot de andere resources die zijn gekoppeld aan het abonnement, ook geblokkeerd. De directory blijft echter wel aanwezig in Azure, en u kunt er een ander abonnement aan koppelen om de directorygebruikers te blijven beheren.

![diagram om te laten zien hoe abonnementen worden gekoppeld](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Azure AD werkt niet zoals andere services in uw Azure-abonnement. Andere Azure-services zijn ondergeschikt aan het Azure-abonnement. Maar wat u ziet in Azure AD verschilt niet per abonnement. De service biedt toegang tot directory's op basis van de aangemelde gebruiker.

Alle gebruikers hebben één basisdirectory waarmee ze worden geverifieerd, maar ze kunnen ook gasten zijn in andere directory’s. In Azure AD ziet u alleen de directory’s waarvan uw gebruikersaccount lid is. Een directory kan ook worden gesynchroniseerd met on-premises Active Directory.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Azure AD en abonnementen voor cloudservices
Azure AD levert de kerndirectory en identiteitsbeheermogelijkheden voor de meeste cloudservices van Microsoft, waaronder:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

U krijgt de Azure AD-service gratis wanneer u zich registreert voor een van deze Microsoft-cloudservices. Als u een extra Azure-abonnement wilt toevoegen aan een Azure AD-directory, kan dit alleen als u bent aangemeld met een Microsoft-account. Als u zich bijvoorbeeld registreert voor Azure met behulp van een Microsoft-account, en u zich ook registreert voor een andere Microsoft-cloudservice maar met een werk- of schoolaccount, beschikt u over exemplaren van Azure AD:
1. De standaarddirectory voor uw Azure-abonnement. U kunt een ander Azure-abonnement toevoegen aan deze directory of aan andere directory’s die u maakt als u bent aangemeld met een Microsoft-account omdat u dan namelijk kunt worden geverifieerd door Azure.
2. De basisdirectory voor uw werk- of schoolaccount. Als u bij Azure bent aangemeld met een werk- of schoolaccount, kunt u geen Azure-abonnement toevoegen aan een bestaande directory omdat uw werk- of schoolaccount niet rechtstreeks door Azure kan worden geverifieerd. 
 
Zie [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md) voor meer informatie over het wijzigen van de beheerders voor een Azure-abonnement.

## <a name="suggestions-to-manage-both-a-subscription-and-a-directory"></a>Tips voor het beheren van zowel een abonnement als een directory
Met de beheerdersrollen voor een Azure-abonnement worden de resources beheerd die zijn gekoppeld aan het Azure-abonnement. In deze sectie worden de verschillen uitgelegd tussen beheerders van Azure-abonnementen en beheerders van Azure AD-directory’s. Beheerdersrollen en andere tips voor het gebruiken van deze rollen om uw abonnement te beheren, vindt u in [Beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles.md).

De rol Servicebeheerder wordt standaard aan u toegewezen wanneer u zich registreert. Als anderen zich moeten aanmelden en services willen gebruiken met hetzelfde abonnement, kunt u hen toevoegen als medebeheerders. 

Azure AD heeft een andere set beheerdersrollen voor het beheren van de directory en identiteitsgerelateerde functies. De hoofdbeheerder van een directory kan bijvoorbeeld gebruikers en groepen toevoegen aan de directory, of Multi-Factor Authentication vereisen voor gebruikers. Wanneer een gebruiker een directory maakt, wordt deze automatisch de hoofdbeheerder. De hoofdbeheerder kan beheerdersrollen toewijzen aan andere gebruikers. Azure AD-beheerdersrollen worden ook gebruikt door andere services, zoals Office 365 en Microsoft Intune. 

Azure-abonnementsbeheerders en Azure AD-directorybeheerders zijn twee verschillende rollen. 
* Azure-abonnementsbeheerders kunnen resources in Azure beheren en kunnen Azure AD gebruiken in Azure Portal (omdat de portal zelf een Azure-resource is). 
* Directorybeheerders kunnen alleen eigenschappen in de Azure AD-directory beheren.

Een persoon kan beide rollen hebben, maar dit is niet vereist. De rol van hoofdbeheerder van een directory kan aan een gebruiker worden toegewezen, maar deze komt dan niet meer in aanmerking voor de rol van servicebeheerder of medebeheerder van een Azure-abonnement. Als de gebruiker geen beheerder is van het abonnement, kan de gebruiker zich wel aanmelden bij Azure Portal, maar heeft dan niet de mogelijkheid om de directory’s voor dat abonnement te beheren in de portal. Deze gebruiker kan directory’s beheren met andere hulpprogramma's, zoals Azure AD PowerShell of het Office 365-beheercentrum.

## <a name="next-steps"></a>Volgende stappen
* Zie [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md) voor meer informatie over het wijzigen van de beheerders voor een Azure-abonnement.
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](active-directory-understanding-resource-access.md)
* Zie [Beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) voor meer informatie over het toewijzen van rollen in Azure AD.

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG

