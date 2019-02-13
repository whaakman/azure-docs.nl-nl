---
title: 'Administrator-Rolinhoud voor Microsoft 365-werkbelastingen: Azure AD | Microsoft Docs'
description: Zoeken naar inhoud en API-verwijzingen voor de beheerdersrollen voor Microsoft 365-workloads in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/24/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7460c3a7d957634d14186d313994c65d89fa4a32
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217867"
---
# <a name="administrator-roles-for-microsoft-365-workloads"></a>Voor workloads Microsoft 365-beheerdersrollen

Alle producten van Microsoft 365 kunnen worden beheerd met beheerdersrollen in Azure AD. Sommige producten bieden ook aanvullende functies die specifiek voor dat product zijn. Voor informatie over de rollen die worden ondersteund door elk product, Zie de onderstaande tabel. Algemene discussies van overdracht oplossen kunnen u vinden in [rol delegeren in Azure Active Directory plannen](roles-concept-delegation.md).

## <a name="where-to-find-content"></a>Waar vind ik inhoud

Microsoft 365 werkbelasting | Obsah Role | API-inhoud
---------------------- | ------------------ | -----------------
Beheerdersrollen in Office 365 en Microsoft 365 bedrijfsplannen | [Office 365-beheerdersrollen](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles?view=o365-worldwide) | Niet beschikbaar
Azure Active Directory (Azure AD) en Azure AD Identity Protection| [Azure AD-beheerdersrollen](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Exchange Online| [Exchange op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help) |  [PowerShell voor Exchange](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
SharePoint Online | [Azure AD-beheerdersrollen](directory-assign-admin-roles.md)<br>Ook [over de SharePoint-beheerdersrollen in Office 365](https://docs.microsoft.com/sharepoint/sharepoint-admin-role) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Teams/Skype voor bedrijven | [Azure AD-beheerdersrollen](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Beveiliging en compliance (Office 365 Advanced Threat Protection, Exchange Online Protection, Information Protection) | [Office 365-beheerdersrollen](https://docs.microsoft.com/office365/SecurityCompliance/permissions-in-the-security-and-compliance-center) | [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
Secure Score | [Azure AD-beheerdersrollen](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Voor naleving | [Rollen voor naleving](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud#permissions-and-role-based-access-control) | Niet beschikbaar
Azure Information Protection | [Azure AD-beheerdersrollen](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Microsoft Cloud App Security | [Op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/cloud-app-security/manage-admins) | [API-naslaginformatie](https://docs.microsoft.com/cloud-app-security/api-tokens) 
Azure Advanced Threat Protection | [Azure ATP-functiegroepen](https://docs.microsoft.com/azure-advanced-threat-protection/atp-role-groups) | Niet beschikbaar
Windows Defender Advanced Threat Protection | [Windows Defender ATP op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/rbac-windows-defender-advanced-threat-protection) | Niet beschikbaar
Privileged Identity Management | [Azure AD-beheerdersrollen](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Intune | [Intune op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/intune/role-based-access-control) | [Graph API](https://docs.microsoft.com/graph/api/resources/intune-rbac-conceptual?view=graph-rest-beta)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/intune-rbac-roledefinition-list?view=graph-rest-beta)
Beheerde Desktop | [Azure AD-beheerdersrollen](directory-assign-admin-roles.md) | [Graph API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Roltoewijzingen ophalen](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)

## <a name="next-steps"></a>Volgende stappen

* [Het toewijzen of verwijderen van Azure AD-beheerdersrollen](directory-manage-roles-portal.md)
* [Azure AD-beheerdersrollen verwijzen naar](directory-assign-admin-roles.md)
