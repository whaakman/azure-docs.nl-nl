---
title: Beheer van beheer eenheden (preview)-Azure Active Directory | Microsoft Docs
description: Beheer eenheden gebruiken voor meer gedetailleerde delegering van machtigingen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 08/01/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b4bdced50f806367a53881d5ef0abd0a3710496
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68736789"
---
# <a name="administrative-units-management-in-azure-active-directory-public-preview"></a>Beheer van beheer eenheden in Azure Active Directory (open bare preview)

In dit artikel worden administratieve eenheden in Azure Active Directory (Azure AD) beschreven. Dit zijn een container met resources die kunnen worden gebruikt voor het delegeren van beheerders machtigingen voor subsets van gebruikers en het Toep assen van beleid op een subset van gebruikers. In azure AD kunnen centrale beheerders met administratieve eenheden machtigingen delegeren aan regionale beheerders of beleid op gedetailleerd niveau instellen.

Dit is nuttig in organisaties met onafhankelijke divisies, bijvoorbeeld een grote universiteit die bestaat uit een groot aantal zelfstandige scholen (zakelijke school, technische school, enzovoort) die onafhankelijk van elkaar zijn. Deze divisies hebben hun eigen IT-beheerders die de toegang beheren, gebruikers beheren en beleids regels instellen die specifiek zijn voor hun afdeling. Centrale beheerders willen deze afdelings beheerders machtigingen kunnen verlenen via de gebruikers in hun specifieke divisies. Met name in dit voor beeld kan een centrale beheerder bijvoorbeeld een administratieve eenheid voor een bepaalde school (zakelijke school) maken en deze vullen met alleen de zakelijke school gebruikers. Vervolgens kan een centrale beheerder de IT-afdeling van de zakelijke school toevoegen aan een bereik functie, met andere woorden, de IT-afdeling van de beheer machtigingen van de zakelijke school alleen verlenen via de beheer eenheid van de zakelijke school.

> [!IMPORTANT]
> Als u administratieve eenheden wilt gebruiken, moet de beheerder van de beheer eenheid een Azure Active Directory Premium licentie hebben. Zie [aan de slag met Azure AD Premium](../fundamentals/active-directory-get-started-premium.md)voor meer informatie.
>

Vanuit het oogpunt van de centrale beheerder is een beheer eenheid een Directory-object dat kan worden gemaakt en gevuld met resources. **In deze preview-versie kunnen deze resources alleen gebruikers zijn.** Zodra de beheer eenheid is gemaakt en ingevuld, kan deze worden gebruikt als een bereik om de verleende machtiging alleen te beperken voor bronnen die zich in de beheer eenheid bevinden.

## <a name="managing-administrative-units"></a>Beheer eenheden beheren

In deze preview-versie kunt u beheer eenheden maken en beheren met behulp van de Azure Active Directory-module voor Windows Power shell-cmdlets. Zie [werken met beheer eenheden](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0) voor meer informatie over hoe u dit doet.

Zie Azure Active Directory voor meer informatie over software vereisten en het installeren van de Azure AD-module, en voor meer informatie over de Azure AD-module-cmdlets voor het beheren van beheer eenheden, inclusief syntaxis, parameter beschrijvingen en voor beelden. [ Power shell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Volgende stappen

[Azure Active Directory-edities](../fundamentals/active-directory-whatis.md)
