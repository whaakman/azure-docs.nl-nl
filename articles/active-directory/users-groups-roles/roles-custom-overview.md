---
title: Een voor beeld van Azure-beheerders rollen met aanpas bare machtigingen-Azure Active Directory | Microsoft Docs
description: Preview van aangepaste Azure AD-rollen voor het delegeren van identiteits beheer. Azure-rollen beheren in de Azure Portal, Power shell of Graph API.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 546abdae5d7c03bb0d4b49f9485fe06b521fdc1d
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722228"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Aangepaste beheerders rollen in Azure Active Directory (preview-versie)

In dit artikel wordt beschreven hoe u inzicht krijgt in de nieuwe aangepaste RBAC (op rollen gebaseerd toegangs beheer) en bron bereiken in Azure Active Directory (Azure AD). Aangepaste RBAC-rollen hebben Opper vlakken van de onderliggende machtigingen van de [ingebouwde rollen](directory-assign-admin-roles.md) , zodat u uw eigen aangepaste rollen kunt maken en ordenen. Met resource bereiken kunt u de aangepaste rol toewijzen voor het beheren van een aantal resources (bijvoorbeeld één toepassing) zonder dat u toegang hebt tot alle resources (alle toepassingen).

Het verlenen van machtigingen met aangepaste RBAC-rollen is een proces dat uit twee stappen bestaat. Eerst maakt u een aangepaste roldefinitie en voegt u er machtigingen aan toe vanuit de lijst met vooraf ingestelde. Dit zijn dezelfde machtigingen die worden gebruikt in de ingebouwde rollen. Wanneer u uw rol hebt gemaakt, kunt u deze toewijzen aan iemand door een roltoewijzing te maken. Met dit proces met twee stappen kunt u één rol maken en deze meerdere keren toewijzen aan verschillende bereiken. Een aangepaste rol kan worden toegewezen in het bereik van de Directory of kan worden toegewezen in een object bereik. Een voor beeld van een object bereik is één toepassing. Op deze manier kan dezelfde rol worden toegewezen aan Sandra via alle toepassingen in de Directory en vervolgens Naveen via alleen de app voor onkosten rapporten van contoso.

Deze eerste versie van aangepaste RBAC-rollen biedt de mogelijkheid om een rol te maken voor het toewijzen van machtigingen voor het beheren van app-registraties. Na verloop van tijd worden extra machtigingen voor organisatie resources, zoals bedrijfs toepassingen, gebruikers en apparaten toegevoegd.

Preview-functies:

- Portal-UI-updates voor het maken en beheren van aangepaste rollen en het toewijzen van deze aan gebruikers in het hele organisatie bereik
- Een preview Power shell-module met nieuwe cmdlets voor:
  - Aangepaste rollen maken en beheren
  - Aangepaste rollen toewijzen aan het registratie bereik voor de hele organisatie of per app
  - Ingebouwde rollen toewijzen op het niveau van de organisatie (pariteit met GA-cmdlets)
  - Ondersteuning voor Azure AD-Graph API

Toegangs beheer op basis van rollen in azure AD is een open bare preview-functie van Azure AD en is beschikbaar in elk betaald licentie plan voor Azure AD. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="understand-azure-ad-role-based-access-control"></a>Meer informatie over Azure AD-op rollen gebaseerd toegangs beheer

Met op rollen gebaseerd toegangs beheer van Azure AD kunt u rollen toewijzen die zijn aangepast om toegestane acties op slechts één type Azure AD-resource toe te staan. Toegang tot Azure AD op basis van rollen werkt op concepten die vergelijkbaar zijn met op rollen gebaseerd toegangs beheer van Azure ([Azure RBAC](../../role-based-access-control/overview.md) voor Azure resource Access, maar op rollen gebaseerd toegangs beheer op basis van Azure AD is gebaseerd op Microsoft Graph en Azure RBAC is gebaseerd op Azure Resource Manager. Beide systemen baseren hun functies echter op roltoewijzingen.

### <a name="role-assignments"></a>Roltoewijzingen

De manier waarop u de toegang beheert met behulp van op rollen gebaseerd toegangs beheer van Azure AD is het maken van **roltoewijzingen**, die worden gebruikt om machtigingen af te dwingen. Een roltoewijzing bestaat uit drie elementen:

- Beveiligings-principal
- Roldefinitie
- Bron bereik

Toegang wordt verleend door een roltoewijzing te maken en toegang kan worden ingetrokken door een roltoewijzing te verwijderen. U kunt [roltoewijzingen maken](roles-create-custom.md) met behulp van de Azure Portal, Azure AD Power shell en Graph API. U kunt [de toewijzingen voor een aangepaste rol afzonderlijk weer geven](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview).

Het volgende diagram toont een voorbeeld van een roltoewijzing. In dit voor beeld is Chris groen toegewezen aan de rol van [toepassings beheerder](directory-assign-admin-roles.md#application-administrator) in het bereik van de Sales Force-toepassing. Chris heeft geen toegang tot het beheren van andere toepassingen, tenzij ze deel uitmaken van een andere roltoewijzing.

![Roltoewijzing is het afdwingen van machtigingen en heeft drie delen](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Beveiligings-principal

Een beveiligingsprincipal vertegenwoordigt de gebruiker of service-principal die toegang moet krijgen tot Azure AD-resources. Een *gebruiker* is een persoon die een gebruikers profiel heeft in azure Active Directory. Een *Service-Principal* is een beveiligings identiteit die door toepassingen of Services wordt gebruikt om toegang te krijgen tot specifieke Azure AD-resources.

Een beveiligingsprincipal is vergelijkbaar met een gebruikers identiteit in dat deze een gebruikers naam en wacht woord of een certificaat vertegenwoordigt, maar voor een toepassing of service in plaats van een gebruiker.

### <a name="role"></a>Role

Een roldefinitie of rol is een verzameling machtigingen. Met een roldefinitie worden de bewerkingen weer gegeven die kunnen worden uitgevoerd op Azure AD-resources, zoals maken, lezen, bijwerken en verwijderen. Er zijn twee soorten rollen in azure AD:

- Ingebouwde rollen die door micro soft zijn gemaakt, kunnen niet worden gewijzigd. De ingebouwde rol globale beheerder heeft alle machtigingen voor alle Azure AD-resources.
- Aangepaste rollen die door uw organisatie zijn gemaakt en beheerd.

### <a name="scope"></a>Scope

Een bereik is de beperking van toegestane acties voor een bepaalde Azure AD-resource. Wanneer u een rol toewijst, kunt u de rol aanpassen om de toegestane acties van de beheerder te beperken door het bepalen van het bereik van de actie. Als uw ontwikkel aars bijvoorbeeld geen volledig beheer van alle toepassingen nodig hebben, kunt u aangepaste Azure AD-rollen gebruiken om te voor komen dat ze alleen app-registraties beheren.

## <a name="required-license-plan"></a>Vereist licentie plan

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Volgende stappen

- Aangepaste roltoewijzingen maken met behulp van [de Azure Portal, Azure AD Power shell en Graph API](roles-create-custom.md)
- [De toewijzingen voor een aangepaste rol weer geven](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview)
