---
title: 'Azure AD Connect-synchronisatie: AD-Prullenbak inschakelen | Microsoft Docs'
description: In dit onderwerp raadt het gebruik van de functie Prullenbak van AD met Azure AD Connect.
services: active-directory
keywords: Prullenbak van AD, per ongeluk verwijderen, bronanker
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 45bff0585eb6f1990a7300ae3d16632eab78a83c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect-synchronisatie: AD-Prullenbak inschakelen
Het is raadzaam dat u de Prullenbak van AD-functie inschakelen voor uw lokale Active Directory's die worden gesynchroniseerd naar Azure AD. 

Als u een on-premises per ongeluk hebt verwijderd AD-gebruiker-object maken en terugzetten met de functie, Azure AD worden hersteld het bijbehorende object van de Azure AD-gebruiker.  Raadpleeg voor informatie over de Prullenbak van AD-functie, artikel [Scenario-overzicht voor het herstellen van verwijderde Active Directory-objecten](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>Voordelen van de AD-Prullenbak inschakelen
Deze functie helpt bij het herstellen van Azure AD-gebruikersobjecten als volgt:

* Als u een on-premises per ongeluk hebt verwijderd object AD-gebruiker, het bijbehorende object van de Azure AD-gebruiker wordt verwijderd in de volgende synchronisatiecyclus. Standaard Azure AD houdt het verwijderde object van Azure AD-gebruiker voorlopig verwijderde status voor 30 dagen.

* Als u beschikt over on-premises AD Recycle Bin functie is ingeschakeld, kunt u de verwijderde herstellen lokale AD-gebruiker-object zonder dat u de waarde Bronanker wijzigt. Wanneer de herstelde on-premises AD-gebruiker-object wordt gesynchroniseerd naar Azure AD, Azure AD wordt gebruikersobject terugzetten van de bijbehorende voorlopig verwijderde Azure AD. Raadpleeg voor informatie over Bronanker kenmerk artikel [Azure AD Connect: concepten ontwerpen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Als u geen lokale AD-Prullenbak-functie is ingeschakeld, kan u worden gevraagd een AD-gebruikersobject ter vervanging van het verwijderde object te maken. Als Azure AD Connect-synchronisatieservice is geconfigureerd voor het systeem gegenereerde AD-kenmerk (zoals ObjectGuid) gebruiken voor het kenmerk Bronanker, is het gemaakte object van de AD-gebruiker heeft niet dezelfde Bronanker waarde als het verwijderde object van de AD-gebruiker. Wanneer het gemaakte object van de AD-gebruiker is gesynchroniseerd met Azure AD, Azure AD maakt een nieuw object van de Azure AD-gebruiker in plaats van het herstellen van het voorlopig verwijderde object in de Azure AD.

> [!NOTE]
> Standaard verwijderd Azure AD houdt objecten van Azure AD-gebruiker in staat voorlopig verwijderde voor 30 dagen voordat ze worden permanent verwijderd. Beheerders kunnen echter de verwijdering van dergelijke objecten te versnellen. Zodra de objecten worden permanent verwijderd, kunnen ze niet meer worden hersteld, zelfs als lokale AD-Prullenbak-functie is ingeschakeld.



## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)

* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
