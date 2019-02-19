---
title: Beheer de toegang tot Azure-resources met Azure AD Privileged Identity Management (PIM)
description: Meer informatie over het beheren van toegang tot Azure-resources met behulp van Azure Active Directory Privileged Identity Management (PIM) en op rollen gebaseerd toegangsbeheer (RBAC).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 757068034868744b408c9402b521a0e4c73950f7
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338197"
---
# <a name="manage-access-to-azure-resources-with-azure-ad-privileged-identity-management"></a>Toegang tot Azure-resources met Azure AD Privileged Identity Management beheren

Als u wilt bevoegde accounts te beschermen tegen schadelijke cyberaanvallen, kunt u Azure Active Directory Privileged Identity Management (PIM) Verlaag de blootstellingstijd van bevoegdheden en vergroot uw inzicht in het gebruik ervan door middel van rapporten en waarschuwingen. PIM doet dit door gebruikers beperkt tot alleen nemen over hun bevoegdheden 'just-in-time' (JIT), of door het toewijzen van bevoegdheden voor een kortere duur waarna bevoegdheden automatisch worden ingetrokken. 

U kunt nu PIM met Azure op rollen gebaseerd toegangsbeheer (RBAC) gebruiken om te beheren, controleren en bewaken van toegang tot Azure-resources. PIM kunt het lidmaatschap van de ingebouwde en aangepaste rollen om u te helpen beheren: 

- Op aanvraag, 'just-in-time' toegang tot Azure-resources inschakelen
- Toegang tot bronnen voor toegewezen gebruikers en groepen automatisch verlopen
- Tijdelijke toegang tot Azure-resources voor snelle taken of op een aanroep schema's toewijzen
- Waarschuwingen ontvangen wanneer nieuwe gebruikers of groepen toegang tot resources worden toegewezen, en wanneer ze in aanmerking komende toewijzingen activeren

Zie voor meer informatie, [wat is Azure AD Privileged Identity Management?](../active-directory/privileged-identity-management/pim-configure.md).