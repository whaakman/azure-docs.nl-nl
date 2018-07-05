---
title: Preview van beheer van beheereenheden in Azure Active Directory
description: Administratieve eenheden gebruik voor gedetailleerdere delegatie van machtigingen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.component: users-groups-roles
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 0884726e59d9ab3f5a5cfe7bb0608f6b5a5da250
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449844"
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Administratieve eenheden beheren in Azure AD - openbare preview
Dit artikel beschrijft administratieve eenheden: een nieuwe Azure Active Directory-container van resources die kunnen worden gebruikt voor het overdragen van machtigingen via subsets van gebruikers en -beleid toepassen op een subset van gebruikers. In Azure Active Directory kunnen administratieve eenheden centrale beheerders machtigingen delegeren naar regionale beheerders of gedetailleerde beleidsregels instellen.

Dit is handig in organisaties met onafhankelijke divisies, bijvoorbeeld een grote universiteit die bestaat uit veel autonome scholen (Business school, school Engineering, enzovoort) die onafhankelijk van elkaar. Dergelijke afdelingen hebben hun eigen IT-beheerders die toegang beheren, gebruikers beheren en beleidsregels die specifiek voor hun afdeling in te stellen. Centrale beheerders wilt kunnen verlenen deze afdelingen beheerders machtigingen over de gebruikers in hun specifieke afdelingen. Meer specifiek, kunt met behulp van dit voorbeeld, de beheerder van een centrale, bijvoorbeeld een administratieve eenheid voor een bepaalde school (Business school) maken en deze vullen met alleen de zakelijke school-gebruikers. Een centrale beheerder kan de school zakelijke IT-afdeling toevoegen aan een rol binnen het bereik met andere woorden, Verleen de IT-personeel van zakelijke school-beheerdersmachtigingen alleen via het Business school administratieve eenheid.

> [!IMPORTANT]
> Voor het gebruik van administratieve eenheden moeten de beheerder binnen het bereik van administratieve eenheid beschikken over een licentie voor Azure Active Directory Premium en Azure Active Directory Basic-licenties voor alle gebruikers in de administratieve eenheid. Zie voor meer informatie, [aan de slag met Azure AD Premium](../fundamentals/active-directory-get-started-premium.md).
>


Uit oogpunt van de centrale beheerder is een administratieve eenheid een Active directory-object dat kan worden gemaakt en gevuld met resources. **Deze resources kunnen alleen gebruikers worden in deze preview-versie.** Eenmaal gemaakt en ingevuld, kan de administratieve eenheid als een bereik worden gebruikt om te beperken van de verleende machtiging alleen via de bronnen die zich bevinden in de administratieve eenheid.

## <a name="managing-administrative-units"></a>Administratieve eenheden beheren
In deze preview-versie, kunt u maken en beheren van administratieve eenheden met de Azure Active Directory-Module voor Windows PowerShell-cmdlets. Voor meer informatie over hoe u kunt doen, Zie [werken met Beheereenheden](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Zie voor meer informatie over de softwarevereisten en de Azure AD-module installeren en voor informatie over de Azure AD-Module-cmdlets voor het beheren van administratieve eenheden, inclusief syntaxis, parameterbeschrijvingen en voorbeelden, [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory-edities](../fundamentals/active-directory-whatis.md)
