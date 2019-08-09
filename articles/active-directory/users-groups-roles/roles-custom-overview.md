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
ms.openlocfilehash: eabf29b10814d19e89c21f27ec66fce5355c9bfb
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68880694"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Aangepaste beheerders rollen in Azure Active Directory (preview-versie)

In dit artikel wordt beschreven hoe u de nieuwe aangepaste op rollen gebaseerde toegangs beheer (RBAC) en resource scopes in Azure Active Directory (Azure AD) begrijpt. Aangepaste RBAC-rollen hebben Opper vlakken van de onderliggende machtigingen van de [ingebouwde rollen](directory-assign-admin-roles.md) , zodat u uw eigen aangepaste rollen kunt maken en ordenen. Met deze aanpak kunt u, indien nodig, toegang verlenen op een meer granulaire manier dan ingebouwde rollen. Deze eerste versie van aangepaste RBAC-rollen biedt de mogelijkheid om een rol te maken voor het toewijzen van machtigingen voor het beheren van app-registraties. Na verloop van tijd worden extra machtigingen voor organisatie resources, zoals bedrijfs toepassingen, gebruikers en apparaten toegevoegd.  

Daarnaast ondersteunen aangepaste RBAC-rollen toewijzingen per resource, naast de meer traditionele organisatie toewijzingen voor het hele bedrijf. Deze aanpak biedt u de mogelijkheid om toegang te verlenen tot het beheren van bepaalde bronnen (bijvoorbeeld één app-registratie) zonder dat u toegang hebt tot alle resources (alle app-registraties).

Toegangs beheer op basis van rollen in azure AD is een open bare preview-functie van Azure AD en is beschikbaar in elk betaald licentie plan voor Azure AD. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="understand-azure-ad-role-based-access-control"></a>Meer informatie over Azure AD-op rollen gebaseerd toegangs beheer

Het verlenen van machtigingen met aangepaste RBAC-rollen is een proces in twee stappen waarmee u een aangepaste roldefinitie maakt en deze vervolgens toewijst met behulp van een roltoewijzing. Een aangepaste roldefinitie is een verzameling machtigingen die u toevoegt vanuit een vooraf gedefinieerde lijst. Deze machtigingen zijn dezelfde machtigingen die worden gebruikt in de ingebouwde rollen.  

Zodra u de roldefinitie hebt gemaakt, kunt u deze toewijzen aan iemand door een roltoewijzing te maken. Een roltoewijzing verleent iemand de machtigingen in een roldefinitie op een specifiek bereik. Met dit proces met twee stappen kunt u een roldefinitie maken en deze meerdere keren toewijzen aan verschillende bereiken. Een bereik definieert de set resources waarvan het gebruikersrol toegang heeft. Het meest voorkomende bereik is het hele organisatie bereik (organisatiebreed). Een aangepaste rol kan worden toegewezen in het bereik van de organisatie, wat inhoudt dat het Role-lid over de rolmachtigingen beschikt over alle resources in het bedrijf. Een aangepaste rol kan ook worden toegewezen in een object bereik. Een voor beeld van een object bereik is één toepassing. Op deze manier kan dezelfde rol worden toegewezen aan Sandra via alle toepassingen in de organisatie en Naveen alleen de app voor onkosten rapporten van contoso.  

Azure AD RBAC werkt op concepten die vergelijkbaar [zijn met toegangs beheer op basis van rollen](../../role-based-access-control/overview.md). Het verschil tussen Azure RBAC beheert de toegang tot Azure-resources, zoals virtuele machines en websites en Azure AD RBAC beheert de toegang tot Azure AD. Beide systemen maken gebruik van het concept van functie definities en roltoewijzingen.

### <a name="role-assignments"></a>Roltoewijzingen

Een roltoewijzing is het proces van het koppelen van een functie definitie aan een gebruiker in een bepaald bereik voor het verlenen van toegang. Toegang wordt verleend door een roltoewijzing te maken en toegang kan worden ingetrokken door een roltoewijzing te verwijderen. Een roltoewijzing bestaat uit drie elementen:
- Gebruiker
- Roldefinitie
- Bron bereik

U kunt [roltoewijzingen maken](roles-create-custom.md) met behulp van de Azure Portal, Azure AD Power shell of graph API. U kunt ook [de toewijzingen voor een aangepaste rol weer geven](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview).

Het volgende diagram toont een voorbeeld van een roltoewijzing. In dit voor beeld is Chris groen toegewezen aan de aangepaste rol van de beheerder van de app-registratie op het bereik van de app-registratie voor contoso-widget. Deze toewijzing verleent Chris de machtigingen van de beheerdersrol van de app-registratie alleen voor deze specifieke app-registratie.

![Roltoewijzing is het afdwingen van machtigingen en heeft drie delen](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Beveiligings-principal

Een beveiligingsprincipal vertegenwoordigt de gebruiker die toegang moet krijgen tot Azure AD-resources. Een *gebruiker* is een persoon die een gebruikers profiel heeft in azure Active Directory.

### <a name="role"></a>Role

Een roldefinitie of rol is een verzameling machtigingen. Met een roldefinitie worden de bewerkingen weer gegeven die kunnen worden uitgevoerd op Azure AD-resources, zoals maken, lezen, bijwerken en verwijderen. Er zijn twee soorten rollen in azure AD:

- Ingebouwde rollen die door micro soft zijn gemaakt, kunnen niet worden gewijzigd. De ingebouwde rol globale beheerder heeft alle machtigingen voor alle Azure AD-resources.
- Aangepaste rollen die door uw organisatie zijn gemaakt en beheerd.

### <a name="scope"></a>Scope

Een bereik is de beperking van toegestane acties voor een bepaalde Azure AD-resource als onderdeel van een roltoewijzing. Wanneer u een rol toewijst, kunt u een bereik opgeven waarmee de beheerder de toegang tot een specifieke resource beperkt. Als u bijvoorbeeld een ontwikkelaar een aangepaste rol wilt verlenen, maar alleen een bepaalde toepassings registratie wilt beheren, kunt u de specifieke toepassings registratie als een bereik in de roltoewijzing toevoegen.

  > [!Note]
  > Aangepaste rollen kunnen worden toegewezen in het bereik van de map en het resource bereik. Ze kunnen nog niet worden toegewezen in het bereik van de beheer eenheid.
  > Ingebouwde rollen kunnen worden toegewezen in het bereik van de map en in sommige gevallen beheer eenheden. Ze kunnen nog niet worden toegewezen in het object bereik.

## <a name="required-license-plan"></a>Vereist licentie plan

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Volgende stappen

- Aangepaste roltoewijzingen maken met behulp van [de Azure Portal, Azure AD Power shell en Graph API](roles-create-custom.md)
- [De toewijzingen voor een aangepaste rol weer geven](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview)
