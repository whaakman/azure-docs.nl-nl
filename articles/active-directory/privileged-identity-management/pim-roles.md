---
title: Rollen in Azure AD Privileged Identity Management | Microsoft Docs
description: Meer informatie over welke functies worden gebruikt voor bevoegde identiteiten met de extensie Azure Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 07/23/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: f8045d70ab4926cdc0e28a9a2290685c4f11205a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618560"
---
# <a name="directory-roles-you-can-manage-using-azure-ad-pim"></a>U kunt beheren met behulp van Azure AD PIM-Directory-rollen
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

U kunt gebruikers toewijzen in uw organisatie aan andere beheerdersrollen in Azure AD. Deze roltoewijzingen beheren welke taken, zoals het toevoegen of verwijderen van gebruikers of wijzigen van service-instellingen, de gebruikers kunnen uitvoeren op Azure AD, Office 365 en andere Microsoft Online Services en verbonden toepassingen.  

Een globale beheerder zijn van gebruikers kunt bijwerken **permanent** toegewezen aan rollen in Azure AD via de portal, zoals beschreven in [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md) of met behulp van [ PowerShell-opdrachten](/powershell/module/azuread#directory_roles).

Beleid voor bevoegde toegang voor gebruikers van beheert Azure AD Privileged Identity Management (PIM) in Azure AD. PIM gebruikers toegewezen aan een of meer rollen in Azure AD en u iemand zich permanent in de rol, of die in aanmerking komen voor de rol kunt toewijzen. Wanneer een gebruiker permanent wordt toegewezen aan een rol of een in aanmerking komende roltoewijzing activeren en vervolgens deze Active Directory van Azure, Office 365 en andere toepassingen met de machtigingen worden toegewezen aan hun rollen beheren kunnen.

Er is geen verschil in de toegang voor iemand met een permanente ten opzichte van een in aanmerking komende roltoewijzing. Het enige verschil is dat sommige gebruikers hebben geen die toegang nodig voortdurend. Ze zijn gemaakt in aanmerking komen voor de rol, en kunnen inschakelen en uitschakelen wanneer ze nodig hebben om.

## <a name="roles-managed-in-pim"></a>Rollen in PIM beheerd
Privileged Identity Management kunt u gebruikers toewijzen aan de algemene beheerdersrollen, met inbegrip van:

* **Globale beheerder** (ook wel bekend als Company administrator) heeft toegang tot alle beheerfuncties. U kunt meer dan één globale beheerder hebben in uw organisatie. De persoon die zich aanmeldt om aan te schaffen Office 365 automatisch wordt een globale beheerder.
* **Beheerder met bevoorrechte rol** beheert Azure AD PIM en updates van roltoewijzingen voor andere gebruikers.  
* **Factureringsbeheerder** doet aankopen, beheert abonnementen, beheert ondersteuningstickets en bewaakt de servicestatus.
* **Wachtwoordbeheerder** stelt wachtwoorden opnieuw, beheert serviceaanvragen en bewaakt de servicestatus. Wachtwoordbeheerders zijn beperkt tot het opnieuw instellen van wachtwoorden voor gebruikers.
* **Servicebeheerder** beheert serviceaanvragen en bewaakt de servicestatus.
  
  > [!NOTE]
  > Als u van Office 365 gebruikmaakt, klikt u vervolgens alvorens de beheerdersrol toe te wijzen aan een gebruiker eerst de gebruiker met beheerdersrechten machtigingen toewijzen aan een service, zoals Exchange Online.
  > 
  > 
* **Gebruikerstoegangbeheerder** stelt wachtwoorden opnieuw, bewaakt de servicestatus en beheert gebruikersaccounts, gebruikersgroepen en serviceaanvragen. De beheerder van de gebruiker kan een globale beheerder verwijderen, andere beheerdersrollen maken of wachtwoorden opnieuw instellen voor globale, facturerings- en servicebeheerders.
* **Exchange-beheerder** over beheerderstoegang beschikt tot Exchange Online via de Exchange-beheercentrum (tijdens elke Exportactie), en bijna alle taken kunt uitvoeren in Exchange Online.
* **SharePoint-beheerder (Preview)** over beheerderstoegang beschikt tot SharePoint Online via de SharePoint Online-beheercentrum, en bijna alle taken kunt uitvoeren in SharePoint Online. Deze functie is momenteel in preview. In aanmerking komende gebruikers kunnen met behulp van deze rol in SharePoint na het activeren van in PIM vertragingen optreden.
* **Skype voor bedrijven-beheerder** over beheerderstoegang beschikt tot Skype voor bedrijven via het Skype voor bedrijven-beheercentrum, en bijna alle taken kunt uitvoeren in Skype voor bedrijven Online.

Lees deze artikelen voor meer informatie over [beheerdersrollen toewijzen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md) en [beheerdersrollen toewijzen in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


In PIM, kunt u [deze rollen toewijzen aan een gebruiker](pim-how-to-add-role-to-user.md) zodat de gebruiker kan [activeren van de rol wanneer dat nodig is](pim-how-to-activate-role.md).

Als u een andere gebruikerstoegang geven wilt tot PIM zelf beheren, de rollen waarmee PIM vereist dat de gebruiker zijn beschreven in [hoe u toegang geven tot PIM](pim-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Functies die niet worden beheerd in PIM
Rollen in Exchange Online of SharePoint Online, met uitzondering van die hierboven vermeld, worden niet weergegeven in Azure AD en dus zijn niet zichtbaar in PIM. Zie voor meer informatie over het wijzigen van fijnmazig roltoewijzingen in deze Office 365-services, [machtigingen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-abonnementen en resourcegroepen worden ook niet weergegeven in Azure AD. Zie voor het beheren van Azure-abonnementen, [toevoegen of wijzigen van de Azure-beheerdersrollen](../../billing/billing-add-change-azure-subscription-administrator.md) en Zie voor meer informatie over Azure RBAC [op rollen gebaseerd toegangsbeheer in Azure](../../role-based-access-control/role-assignments-portal.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Gebruikersrollen en meldt u zich
Voor sommige Microsoft-services en toepassingen, een gebruiker toewijzen aan een rol mogelijk niet voldoende zijn om een beheerder zijn voor die gebruiker.

Toegang tot de Azure-portal vereist de gebruiker een servicebeheerder of CO-beheerder van een Azure-abonnement, zelfs als de gebruiker niet hoeft voor het beheren van de Azure-abonnementen.  Bijvoorbeeld, voor het beheren van configuratie-instellingen voor Azure AD, een gebruiker moet een globale beheerder in Azure AD zowel een op CO-abonnementsbeheerder van een Azure-abonnement.  Zie voor meer gebruikers toevoegen aan Azure-abonnementen, [toevoegen of wijzigen van de Azure-beheerdersrollen](../../billing/billing-add-change-azure-subscription-administrator.md).

Toegang tot Microsoft Online Services mogelijk de gebruiker ook een licentie worden toegewezen voordat ze kunnen openen van de service-portal of administratieve taken uitvoeren.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Een licentie toewijzen aan een gebruiker in Azure AD
1. Aanmelden bij de [Azure-portal](http://portal.azure.com) met een account voor globale beheerder of CO-beheerder.
3. Azure AD selecteren en de map die u werken met en die wilt heeft licenties die zijn gekoppeld.
4. Selecteer **licenties** aan de linkerkant. De lijst met beschikbare licenties wordt weergegeven.
5. Selecteer het licentieabonnement waarin de licenties die u wilt distribueren.
6. Selecteer **gebruikers toe te wijzen**.
7. Selecteer de gebruiker die u wilt een licentie toewijzen.
8. Klik op de **toewijzen** knop.  De gebruiker kan zich nu aanmelden bij Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

