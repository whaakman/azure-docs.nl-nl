---
title: Rollen in Azure AD Privileged Identity Management | Microsoft Docs
description: Meer informatie over welke functies worden gebruikt voor bevoegde identiteiten met de extensie Azure Privileged Identity Management.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ac812ccc-cf4e-4ac2-b981-69598056c9ed
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: c20aca4202319154b01d6398570f745636120f49
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="different-administrative-role-in-azure-active-directory-pim"></a>Andere administratieve rol in Azure Active Directory PIM
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

U kunt gebruikers in uw organisatie voor verschillende beheerdersrechten rollen in Azure AD. Deze roltoewijzingen bepalen welke taken, zoals het toevoegen of verwijderen van gebruikers of wijzigen van service-instellingen, de gebruikers kunnen uitvoeren op Azure AD, Office 365 en andere Microsoft Online Services en verbonden toepassingen.  

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen.

Een globale beheerder kunt bijwerken die gebruikers **permanent** toegewezen aan rollen in Azure AD, met behulp van PowerShell-cmdlets, zoals `Add-MsolRoleMember` en `Remove-MsolRoleMember`, of via de klassieke portal zoals beschreven in [ beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD Privileged Identity Management (PIM) beheert de beleidsregels voor bevoorrechte toegang voor gebruikers in Azure AD. PIM wijst gebruikers toe aan een of meer rollen in Azure AD en u iemand permanent in de rol, of in aanmerking komen voor de rol kunt toewijzen. Wanneer een gebruiker permanent wordt toegewezen aan een functie of een in aanmerking komende roltoewijzing activeert vervolgens zij Azure Active Directory, Office 365 en andere toepassingen met de machtigingen worden toegewezen aan hun rollen beheren kunnen.

Er is geen verschil in het toegang krijgen tot iemand met een permanente ten opzichte van een in aanmerking komende roltoewijzing. Het enige verschil is dat sommige gebruikers hebben geen die toegang nodig voortdurend. Ze zijn gemaakt in aanmerking komen voor de rol en kunnen inschakelen en uitschakelen wanneer ze moeten.

## <a name="roles-managed-in-pim"></a>Rollen die worden beheerd in PIM
Privileged Identity Management kunt u gebruikers toewijzen aan de algemene beheerdersrollen, met inbegrip van:

* **Globale beheerder** (ook wel bekend als bedrijfsbeheerder) toegang heeft tot alle beheerfuncties. U kunt meer dan één globale beheerder in uw organisatie hebben. De persoon die zich aanmeldt om aan te schaffen Office 365 automatisch wordt een globale beheerder.
* **Beheerder met bevoorrechte rol** beheert de Azure AD PIM en toewijzingen van rollen voor andere gebruikers.  
* **Financieel medewerker** doet aankopen, beheert abonnementen, beheert ondersteuningstickets en bewaakt de servicestatus.
* **Wachtwoordbeheerder** wachtwoorden opnieuw instellen, beheert serviceaanvragen en bewaakt de servicestatus. Wachtwoord beheerders zijn beperkt tot het opnieuw instellen van wachtwoorden voor gebruikers.
* **Servicebeheerder** beheert serviceaanvragen en bewaakt de servicestatus.
  
  > [!NOTE]
  > Als u van Office 365 gebruikmaakt, vervolgens alvorens de beheerdersrol toe te wijzen aan een gebruiker, eerst de gebruiker beheerdersrechten machtigingen toewijzen aan een service, zoals Exchange Online.
  > 
  > 
* **Gebruikerstoegangbeheerder** wachtwoorden opnieuw instellen, bewaakt de servicestatus en beheert gebruikersaccounts, gebruikersgroepen en serviceaanvragen. De gebruiker management-beheerder kan niet verwijderen van een globale beheerder, andere beheerdersrollen maken of opnieuw instellen van wachtwoorden voor financieel medewerkers, algemeen beheerders en servicebeheerders.
* **Exchange-beheerder** over beheerderstoegang beschikt tot Exchange Online via het Exchange-beheercentrum (tijdens elke Exportactie) en bijna alle taken kunt uitvoeren in Exchange Online.
* **SharePoint-beheerder** beheerderstoegang tot SharePoint Online via het SharePoint Online beheercentrum en bijna alle taken kunt uitvoeren in SharePoint Online.
* **Skype voor bedrijven beheerder** over beheerderstoegang beschikt tot Skype voor bedrijven via het Skype voor bedrijven-beheercentrum en bijna alle taken kunt uitvoeren in Skype voor bedrijven Online.

Deze artikelen voor meer informatie lezen over [beheerdersrollen toewijzen in Azure AD](active-directory-assign-admin-roles.md) en [beheerdersrollen toewijzen in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Vanuit PIM, kunt u [deze rollen toewijzen aan een gebruiker](active-directory-privileged-identity-management-how-to-add-role-to-user.md) zodat de gebruiker kan [activeren van de rol wanneer deze nodig](active-directory-privileged-identity-management-how-to-activate-role.md).

Als u gebruikerstoegang geven een andere op PIM zelf beheren wilt, de functies die PIM moet de gebruiker hebben worden beschreven verder in [hoe toegang geven tot PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Functies die niet worden beheerd in PIM
Rollen in Exchange Online of SharePoint Online, behalve die hierboven vermeld, worden niet weergegeven in Azure AD en dus zijn niet zichtbaar in PIM. Zie voor meer informatie over het wijzigen van fijnmazig roltoewijzingen in deze Office 365-services [machtigingen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-abonnementen en resourcegroepen worden ook niet weergegeven in Azure AD. Zie voor het beheren van Azure-abonnementen [toevoegen of wijzigen van de Azure-beheerdersrollen](../billing/billing-add-change-azure-subscription-administrator.md) en voor meer informatie over Azure RBAC Zie [rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Gebruikersrollen en aanmelden
Voor sommige Microsoft-services en toepassingen, een gebruiker toewijzen aan een rol mogelijk niet voldoende zijn voor die gebruiker moet een beheerder.

Toegang tot de klassieke Azure portal moet de gebruiker een servicebeheerder of medebeheerder van een Azure-abonnement, zelfs als de gebruiker niet hoeft voor het beheren van de Azure-abonnementen.  Voor het beheren van configuratie-instellingen voor Azure AD in de klassieke portal, moet een gebruiker bijvoorbeeld niet zowel een globale beheerder in Azure AD als de medebeheerder voor een abonnement op een Azure-abonnement.  Zie voor meer informatie over hoe u gebruikers toevoegen aan Azure-abonnementen, [toevoegen of wijzigen van de Azure-beheerdersrollen](../billing/billing-add-change-azure-subscription-administrator.md).

Toegang tot Microsoft Online Services moet mogelijk de gebruiker ook een licentie worden toegewezen voordat ze kunnen de service portal opent of beheertaken uit te voeren.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Een licentie toewijzen aan een gebruiker in Azure AD
1. Aanmelden bij de [klassieke Azure-portal](http://manage.windowsazure.com) met een globale beheerdersaccount of een collega administrator-account.
2. Selecteer **alle Items** vanuit het hoofdmenu.
3. Selecteer de map die u werken wilt met en met licenties die zijn gekoppeld.
4. Selecteer **licenties**. De lijst met beschikbare licenties wordt weergegeven.
5. Selecteer het licentieabonnement waarin de licenties die u wilt distribueren.
6. Selecteer **gebruikers toewijzen**.
7. Selecteer de gebruiker die u wilt een licentie toewijzen.
8. Klik op de **toewijzen** knop.  De gebruiker zich nu aanmelden bij Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

