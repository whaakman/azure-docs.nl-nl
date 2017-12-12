---
title: Voorbeeld van de administratieve eenheden management in Azure Active Directory
description: Administratieve eenheden gebruiken voor meer gedetailleerd overdracht van machtigingen in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 1e85300676eeee9259e40faa0e0ede94a36f6167
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Beheer van beheereenheden in Azure AD - openbare preview
In dit artikel beschrijft administratieve eenheden – een nieuwe Azure Active Directory-container van bronnen die kunnen worden gebruikt voor het overdragen van beheerdersmachtigingen via subsets van gebruikers en -beleid toepassen op een subset van gebruikers. In Azure Active Directory kunnen administratieve eenheden centrale beheerders overdragen van machtigingen voor de regionale beheerders of beleid instellen op een gedetailleerd niveau.

Dit is handig in organisaties met onafhankelijke divisies, bijvoorbeeld een grote universiteit bestaat uit veel autonome scholen (Business school, school Engineering, enzovoort) die onafhankelijk van elkaar. Dergelijke afdelingen hebben hun eigen IT-beheerders die toegang te beheren, gebruikers beheren en beleidsregels die specifiek is voor hun deling instellen. Centrale beheerders kunnen wilt verlenen deze afdelingen beheerders machtigingen via de gebruikers in hun specifieke afdelingen. Meer specifiek, kunt het volgende voorbeeld de beheerder van een centrale, bijvoorbeeld een administratieve eenheid voor een bepaalde school (Business school) maken en deze vullen met alleen de zakelijke school gebruikers. Een centrale beheerder kan de IT-personeel van Business school toevoegen aan een rol binnen het bereik met andere woorden, Verleen de IT-medewerkers van Business school-beheerdersmachtigingen alleen via de Business school administratieve eenheid.

> [!IMPORTANT]
> Alleen als u Azure Active Directory Premium inschakelt, kunt u administratieve eenheid bereik beheerdersrollen toewijzen. Zie voor meer informatie [aan de slag met Azure AD Premium](active-directory-get-started-premium.md).
>


Uit oogpunt van de centrale beheerder is een administratieve eenheid een directoryobject dat kan worden gemaakt en gevuld met resources. **In deze preview-versie, kunnen deze resources worden alleen gebruikers.** Zodra u hebt gemaakt en ingevuld, kan de administratieve eenheid als bereik worden gebruikt om te beperken van de toegekende machtiging alleen via de bronnen die zich bevinden in de administratieve eenheid.

## <a name="managing-administrative-units"></a>Het beheren van administratieve eenheden
U kunt maken en beheren van administratieve eenheden met de Azure Active Directory-Module voor Windows PowerShell-cmdlets in deze preview-versie. Voor meer informatie over hoe u kunt doen, Zie [werken met administratieve eenheden](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Zie voor meer informatie over de softwarevereisten en de Azure AD-module installeren en voor informatie over de Azure AD-Module-cmdlets voor het beheren van administratieve eenheden, inclusief syntaxis, beschrijvingen van parameters en voorbeelden [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory-edities](active-directory-editions.md)
