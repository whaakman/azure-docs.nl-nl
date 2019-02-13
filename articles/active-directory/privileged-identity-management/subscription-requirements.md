---
title: Licentie-vereisten voor het gebruik van PIM - Azure | Microsoft Docs
description: Beschrijft de licentievereisten duidelijk zijn Azure AD Privileged Identity Management (PIM) gebruiken.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/16/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7b7aa010b3001045566bc55fb440a2171145881
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177245"
---
# <a name="license-requirements-to-use-pim"></a>Licentievereisten PIM gaat gebruiken

Een directory moet een geldige licentie hebben voor het gebruik van Azure Active Directory (Azure AD) Privileged Identity Management (PIM). Bovendien moeten licenties worden toegewezen aan de beheerders en gebruikers. Dit artikel beschrijft de licentievereisten PIM gaat gebruiken.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van PIM, moet uw directory een van de volgende betaalde of proeflicenties hebben:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Zie [Wat is Azure Active Directory?](../fundamentals/active-directory-whatis.md) voor meer informatie.

## <a name="which-users-must-have-licenses"></a>Welke gebruikers moeten licenties hebben?

Elke beheerder of gebruiker die de interactie met of ontvangt een van de voordelen van PIM moet een licentie hebben. Voorbeelden zijn:

- Beheerders met Azure AD-rollen beheerd met PIM
- Beheerders met Azure-resourcerollen beheerd met PIM
- Beheerders die zijn toegewezen aan de rol beheerder met bevoorrechte rol
- Gebruikers die zijn toegewezen als in aanmerking voor de maprollen met PIM beheerd
- Gebruikers kunnen aanvragen in PIM goedkeuren/afwijzen
- Gebruikers die zijn toegewezen aan een Azure-resource-rol met just-in-time- of direct toewijzingen voor (op basis van tijd)  
- Gebruikers die zijn toegewezen aan een toegangsbeoordeling
- Gebruikers die toegangsbeoordelingen uitvoeren

Zie voor meer informatie over het toewijzen van licenties aan uw gebruikt [toewijzen of verwijderen met behulp van de Azure Active Directory-portal licenties](../fundamentals/license-users-groups.md).

## <a name="what-happens-when-a-license-expires"></a>Wat gebeurt er wanneer een licentie is verlopen?

Als een Azure AD Premium P2, EMS E5 of proeflicentie is verlopen, is PIM-functies niet langer beschikbaar in de map:

- Permanente roltoewijzingen aan Azure AD-rollen niet worden gewijzigd.
- De PIM-service in Azure portal, evenals de Graph API-cmdlets en PowerShell-interfaces met PIM, wordt niet langer beschikbaar voor gebruikers bevoorrechte rollen activeren, het beheren van bevoegde toegang of het uitvoeren van beoordelingen van bevoorrechte rollen.
- In aanmerking komende roltoewijzingen van Azure AD-rollen worden verwijderd, omdat gebruikers wordt niet meer kunnen bevoorrechte rollen activeren.
- Alle actieve toegangsbeoordelingen van Azure AD-rollen wordt beëindigd en PIM-configuratie-instellingen worden verwijderd.
- PIM wordt niet meer e-mailberichten verzenden op wijzigingen aan toewijzingen van rol.

## <a name="next-steps"></a>Volgende stappen

- [PIM implementeren](pim-deployment-plan.md)
- [Beginnen met PIM](pim-getting-started.md)
- [Rollen die in PIM kunt u niet beheren](pim-roles.md)
