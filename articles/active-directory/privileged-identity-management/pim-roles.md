---
title: Azure AD directory-rollen in PIM kunt u beheren | Microsoft Docs
description: Hierin wordt beschreven in de Azure AD-maprollen die u in Azure AD Privileged Identity Management (PIM beheren kunt).
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
ms.openlocfilehash: cf0c9b76a7edace9f2a9147823b292e218e20bf7
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44300270"
---
# <a name="azure-ad-directory-roles-you-can-manage-in-pim"></a>Azure AD-maprollen die kunt u in PIM beheren
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

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Gebruikersrollen en meldt u zich
Voor sommige Microsoft-services en toepassingen, een gebruiker toewijzen aan een rol mogelijk niet voldoende zijn om een beheerder zijn voor die gebruiker.

Toegang tot de Azure-portal vereist de gebruiker eigenaar van een Azure-abonnement, zelfs als de gebruiker niet hoeft voor het beheren van de Azure-abonnementen.  Bijvoorbeeld, voor het beheren van configuratie-instellingen voor Azure AD, een gebruiker moet een globale beheerder in Azure AD en een eigenaar van een Azure-abonnement.  Zie voor meer gebruikers toevoegen aan Azure-abonnementen, [toegang met RBAC en de Azure-portal beheren](../..//role-based-access-control/role-assignments-portal.md).

Toegang tot Microsoft Online Services mogelijk de gebruiker ook een licentie worden toegewezen voordat ze kunnen openen van de service-portal of administratieve taken uitvoeren.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Een licentie toewijzen aan een gebruiker in Azure AD

1. Aanmelden bij de [Azure-portal](http://portal.azure.com) met een rol globale beheerder of eigenaar.

1. Selecteer de Azure AD-directory die u wilt werken en die licenties die zijn gekoppeld aan deze heeft.

1. Klik in het linkernavigatievenster op **Azure Active Directory**.

1. Klik op **licenties**. De lijst met beschikbare licenties wordt weergegeven.

    ![Licenties voor Azure Active Directory](./media/pim-roles/licenses-overview.png)

1. Klik op uw **Product**.

1. Klik op het licentieabonnement waarin de licenties die u wilt distribueren.

    ![Producten met licenties](./media/pim-roles/licenses-products.png)

1. Klik op **toewijzen** om de licentie toewijzen deelvenster te openen.

    ![Gebruikers met licenties](./media/pim-roles/licenses-licensed-users.png)

1. Selecteer de gebruiker of groep die u wilt een licentie toewijzen.

    ![Licentie toewijzen](./media/pim-roles/licenses-assign-license.png)

1. Klik op **toewijzingsopties** om uw toewijzingsopties te configureren.

    ![Toewijzingsopties](./media/pim-roles/licenses-assignment-options.png)

1. Klik op **toewijzen** de licentie toewijzen. De gebruiker heeft nu de licentie.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [PIM gebruiken](pim-getting-started.md)
- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)

