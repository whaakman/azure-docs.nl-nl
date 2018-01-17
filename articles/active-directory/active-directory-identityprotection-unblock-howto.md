---
title: 'Azure Active Directory: Identity Protection - hoe gebruikers | Microsoft Docs'
description: 'Meer informatie over hoe de blokkering opheffen gebruikers die zijn geblokkeerd door een Azure Active Directory: Identity Protection-beleid.'
services: active-directory
keywords: beveiliging van Azure active directory-identiteit, deblokkeren van gebruiker
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ab11f27ceed716ba90e9027eee1775c190d2d93f
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory: Identity Protection - blokkering opheffen van gebruikers
Met Azure Active Directory: Identity Protection, kunt u beleid voor het blokkeren van gebruikers als de geconfigureerde voorwaarden is voldaan. Normaal gesproken een geblokkeerde gebruiker contactpersonen helpdesk te worden gedeblokkeerd. In dit onderwerp worden de stappen beschreven die u kunt uitvoeren om de blokkering van een geblokkeerde gebruiker.

## <a name="determine-the-reason-for-blocking"></a>Bepaal de oorzaak voor het blokkeren van
Als eerste stap om de blokkering van een gebruiker te, moet u bepalen welk type beleid dat de gebruiker is geblokkeerd omdat de volgende stappen zijn afhankelijk van het.
Met Azure Active Directory: Identity Protection, kan een gebruiker ofwel worden geblokkeerd door een beleid voor aanmelden risico of gebruikersbeleid risico.

U kunt het type beleid dat een gebruiker uit de kop in het dialoogvenster dat tijdens een poging tot aanmelding is gepresenteerd aan de gebruiker is geblokkeerd krijgen:

| Beleid | Gebruikersdialoogvenster |
| --- | --- |
| Aanmeldingsrisico |![Geblokkeerde aanmelden](./media/active-directory-identityprotection-unblock-howto/02.png) |
| Gebruikersrisico |![Geblokkeerde account](./media/active-directory-identityprotection-unblock-howto/104.png) |

Een gebruiker die is geblokkeerd door:

* Een beleid aanmelden risico wordt ook wel bekend als verdachte aanmelden
* Gebruikersbeleid risico wordt ook wel een account risico

## <a name="unblocking-suspicious-sign-ins"></a>Deblokkeren verdachte aanmeldingen
Als u wilt deblokkeren een verdachte aanmelden, hebt u de volgende opties:

1. **Aanmelden van een vertrouwde locatie of het apparaat** -een veelvoorkomende reden voor geblokkeerde verdachte aanmeldingen zijn aanmeldpogingen vanaf onbekende locaties of apparaten. Uw gebruikers kunnen snel bepalen of dit de blokkerende reden is door te proberen aan te melden vanaf een vertrouwde locatie of het apparaat.
2. **Uitsluiten van beleid** : als u denkt dat de huidige configuratie van uw beleid voor aanmelden wordt veroorzaakt door problemen voor specifieke gebruikers, kunt u de gebruikers uitsluiten van deze. Zie [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) voor meer informatie.
3. **Schakel beleid** : als u denkt dat de configuratie van uw beleid wordt veroorzaakt door problemen voor alle gebruikers, kunt u het beleid uitschakelen. Zie [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) voor meer informatie.

## <a name="unblocking-accounts-at-risk"></a>Deblokkeren accounts risico
Als u wilt deblokkeren van een account risico, hebt u de volgende opties:

1. **Wachtwoord opnieuw instellen** -kunt u het wachtwoord van gebruikers opnieuw instellen. Zie [handmatige beveiligd wachtwoord opnieuw instellen](active-directory-identityprotection.md#manual-secure-password-reset) voor meer informatie.
2. **Alle risicogebeurtenissen negeren** : de gebruiker beleid blokken van het risico van een gebruiker als de geconfigureerde gebruiker voor het blokkeren van toegang tot de risiconiveau is bereikt. U kunt verkleinen dat een gebruiker het risiconiveau door handmatig sluiten risicogebeurtenissen die worden gerapporteerd. Zie voor meer informatie [risicogebeurtenissen handmatig sluiten](active-directory-identityprotection.md#closing-risk-events-manually).
3. **Uitsluiten van beleid** : als u denkt dat de huidige configuratie van uw beleid voor aanmelden wordt veroorzaakt door problemen voor specifieke gebruikers, kunt u de gebruikers uitsluiten van deze. Zie [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) voor meer informatie.
4. **Schakel beleid** : als u denkt dat de configuratie van uw beleid wordt veroorzaakt door problemen voor alle gebruikers, kunt u het beleid uitschakelen. Zie [Azure Active Directory: Identity Protection](active-directory-identityprotection.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
 Wilt u meer informatie over Azure AD Identity Protection? Bekijk [Azure Active Directory: Identity Protection](active-directory-identityprotection.md).
