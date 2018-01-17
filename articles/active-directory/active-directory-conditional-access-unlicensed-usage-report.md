---
title: Niet-gelicentieerde gebruiksrapport | Microsoft Docs
description: Het gebruik van niet-gelicentieerde rapport helpt bij het identificeren van niet-gelicentieerde gebruikers die gebruikmaken van betaald Azure AD-functies.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: 8bcd814ee7b7bfc158e62ad26e6cc23781f5352d
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="unlicensed-usage-report"></a>Niet-gelicentieerde gebruiksrapport
Het gebruik van niet-gelicentieerde rapport helpt bij het identificeren van niet-gelicentieerde gebruikers die gebruikmaken van betaald Azure AD-functies. Hiermee kunt u beter te maken van de licenties die u hebt aangeschaft en om te identificeren die u weet wanneer u aanvullende licenties moet mogelijk. 

Dit rapport bevat actieve informatie over het gebruik van de betaalde functies in de afgelopen 30 dagen. 

## <a name="report-structure"></a>Rapport structuur
| Kolomnaam | Beschrijving |
|:--- |:--- |
| Niet-gelicentieerde gebruiker |Naam van de gebruiker |
| Functie |De naam van de functie. Bijvoorbeeld: voorwaardelijke toegang |
| Toepassing geopend |De naam van de toepassing die wordt geopend met de functie. Bijvoorbeeld: Office 365 SharePoint Online |

> [!NOTE]
> Als een gebruikersaccount is verwijderd. de kolom 'Niet-gelicentieerde User' worden ingevuld met de ID, zoals 1003000090D8B285
> 
> 

## <a name="conditional-access-feature"></a>Functie voor voorwaardelijke toegang
Niet-gelicentieerde gebruikers gemarkeerd wanneer ze toegang krijgen een service met beleid voor voorwaardelijke toegang toegepast tot als ze geen een Azure AD Premium-licentie hebt. 

Dit geldt voor MFA / locatie beleid, evenals een apparaat beleid met Intune.

## <a name="see-also"></a>Zie ook
* [Voorwaardelijke toegang gebruiken met Office 365 en andere Azure Active Directory verbonden apps](active-directory-conditional-access-azure-portal.md)
* [Aan de slag met voorwaardelijke toegang tot Azure AD](active-directory-conditional-access-azure-portal-get-started.md) 

