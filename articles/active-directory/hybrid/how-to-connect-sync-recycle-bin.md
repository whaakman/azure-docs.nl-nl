---
title: 'Azure AD Connect-synchronisatie: AD-Prullenbak inschakelen | Microsoft Docs'
description: In dit onderwerp raadt het gebruik van de functie Prullenbak van AD met Azure AD Connect.
services: active-directory
keywords: AD Recycle Bin, per ongeluk verwijderen, bronanker
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: be6c5ccea6bf923b408319baf4bc1743978169c7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203807"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect-synchronisatie: AD-prullenbak inschakelen
Het is raadzaam dat u de functie AD-Prullenbak inschakelen voor uw on-premises Active Directory's die zijn gesynchroniseerd met Azure AD. 

Als u onbedoeld een on-premises AD-gebruikersobject en herstellen met behulp van de functie, Azure AD worden hersteld het overeenkomstige gebruikersobject van Azure AD.  Raadpleeg het artikel voor informatie over de functie AD-Prullenbak, [Scenario-overzicht voor het herstellen van verwijderde Active Directory-objecten](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>Voordelen van de AD-Prullenbak inschakelen
Deze functie helpt met het herstellen van Azure AD-gebruikersobjecten door het volgende te doen:

* Als u een on-premises per ongeluk verwijderd object AD-gebruiker, het bijbehorende object van de Azure AD-gebruiker wordt verwijderd in de volgende synchronisatiecyclus. Standaard Azure AD houdt het verwijderde object van de Azure AD-gebruiker voorlopig verwijderde status voor 30 dagen.

* Als u beschikt over on-premises AD Recycle Bin-functie is ingeschakeld, kunt u de verwijderde herstellen op lokale AD-gebruikersobject zonder te hoeven wijzigen van de waarde van het Bronanker. Als de herstelde on-premises AD-gebruikersobject is gesynchroniseerd met Azure AD, Azure AD wordt gebruikersobject herstellen de bijbehorende voorlopig verwijderde Azure AD. Raadpleeg het artikel voor meer informatie over het kenmerk Sourceanchor [Azure AD Connect: Ontwerpconcepten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Als u geen on-premises AD-Prullenbak-functie is ingeschakeld, kan u worden gevraagd om een AD-gebruiker-object ter vervanging van het verwijderde object te maken. Als Azure AD Connect-synchronisatieservice is geconfigureerd voor het gebruik van het systeem gegenereerde AD-kenmerk (zoals ObjectGuid) voor het kenmerk Bronanker, is het gemaakte object van de AD-gebruiker heeft niet dezelfde Bronanker waarde als het verwijderde object van de AD-gebruiker. Wanneer het zojuist gemaakte AD-gebruikersobject is gesynchroniseerd met Azure AD, Azure AD maakt u een nieuwe Azure AD-gebruikersobject in plaats van het herstellen van het voorlopig verwijderde object voor Azure AD-gebruiker.

> [!NOTE]
> Standaard verwijderd Azure AD houdt objecten van Azure AD-gebruiker in staat voorlopig verwijderde voor 30 dagen voordat ze permanent worden verwijderd. Beheerders kunnen echter de verwijdering van dergelijke objecten te versnellen. Zodra de objecten worden definitief verwijderd, kunnen ze niet meer worden hersteld, zelfs niet als on-premises AD-Prullenbak-functie is ingeschakeld.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)

* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
