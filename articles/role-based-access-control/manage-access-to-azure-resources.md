---
title: Toegang tot Azure-resources met Azure Active Directory beheren
description: Meer informatie over de manieren voor het beheren van toegang tot Azure-resources met behulp van verschillende functies van Azure Active Directory.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: f66abf54-3809-436c-92b6-018e1179780e
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2017
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 55bd18b127fea8a06e58f14873a584f0b5849906
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203234"
---
# <a name="manage-access-to-azure-resources-with-azure-active-directory"></a>Toegang tot Azure-resources met Azure Active Directory beheren

Identiteit en toegang beheren voor cloudbronnen is een essentiële functie voor elke organisatie die van de cloud gebruikmaakt. Azure Active Directory (Azure AD) is het systeem identiteits- en toegangsbeheer voor Microsoft Azure.  

Voordat u de ondersteunende verkennen functiegebieden van Azure AD, bekijk de volgende video: "Vergrendelen toegang tot de Azure-Cloud via eenmalige aanmelding, rollen gebaseerd toegangsbeheer en voorwaardelijke." In dit meer over:

- Aanbevolen procedures voor het configureren van eenmalige aanmelding bij de Azure portal met de lokale Active Directory.
- Het gebruik van Azure RBAC voor fijnmazig toegangscontrole tot bronnen in abonnementen.
- Afdwingen van de regels van sterke verificatie met behulp van Azure AD voorwaardelijke toegang.
- Het concept van een beheerde Service-identiteit, waarbij Azure-resources kunnen automatisch worden geverifieerd bij Azure-services en ontwikkelaars niet nodig voor het afhandelen van API-sleutels of geheimen.

> [!VIDEO https://www.youtube.com/embed/FKBoWWKRnvI]

## <a name="feature-areas"></a>Gebieden van de functie
Azure AD levert de volgende mogelijkheden voor het beheren van toegang tot Azure-resources:

|||
|---|---|
| [Relatie tussen Azure AD-tenants en abonnementen](rbac-and-directory-admin-roles.md) | Meer informatie over het Azure AD is de vertrouwde bron van gebruikers en groepen voor een Azure-abonnement. |
| [Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) | Geavanceerd toegangsbeheer door middel van rollen toegewezen aan gebruikers, groepen of service-principals bieden. |
| [Beschermde identiteitsbeheer met RBAC](pim-azure-resource.md) | Controle over uitgebreide beheerdersmogelijkheden toegang door toe te wijzen bevoorrechte rollen just-in-time. |
| [Voorwaardelijke toegang voor het beheer van Azure](conditional-access-azure-management.md) | Instellen van beleidsregels voor voorwaardelijke toegang wilt toestaan of blokkeren van toegang tot Azure management-eindpunten. |
| [Beheerde Service-identiteit (MSI)](../active-directory/pp/msi-overview.md) | Geeft de Azure-resources zoals virtuele Machines hun eigen identiteit zodat u niet hoeft te plaatsen van referenties in uw code. |

 