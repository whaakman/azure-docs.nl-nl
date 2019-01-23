---
title: Hoe gebruikers met Azure Active Directory Identity Protection | Microsoft Docs
description: Informatie over hoe de blokkering opheffen gebruikers die zijn geblokkeerd door een Azure Active Directory Identity Protection-beleid.
services: active-directory
keywords: Azure active directory identity protection, deblokkeren van gebruiker
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 8eccb4c1618acc88f6a3bdfa6c67d73aad055988
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54476645"
---
# <a name="how-to-unblock-users"></a>Procedures: Gebruikers deblokkeren

Met Azure Active Directory Identity Protection, kunt u beleid voor het blokkeren van gebruikers als de geconfigureerde voorwaarden is voldaan. Normaal gesproken een geblokkeerde gebruiker contactpersonen helpdesk om te worden opgeheven. In dit artikel worden de stappen beschreven die u kunt uitvoeren om een geblokkeerde gebruiker deblokkeren.

## <a name="determine-the-reason-for-blocking"></a>Bepaal de oorzaak voor het blokkeren van
Als eerste stap om de blokkering van een gebruiker te, moet u bepalen welk type beleid dat de gebruiker is geblokkeerd omdat de volgende stappen zijn afhankelijk van het.
Met Azure Active Directory Identity Protection, kan een gebruiker, ofwel worden geblokkeerd door een beleid voor aanmeldingsrisico of een beleid voor gebruikersrisico's.

U kunt het type beleid dat een gebruiker uit de kop in het dialoogvenster dat is opgegeven voor de gebruiker tijdens een poging tot aanmelden is geblokkeerd krijgen:

| Beleid | Dialoogvenster voor de gebruiker |
| --- | --- |
| Aanmeldingsrisico |![Geblokkeerde aanmelden](./media/howto-unblock-user/02.png) |
| Gebruikersrisico |![Geblokkeerde account](./media/howto-unblock-user/104.png) |

Een gebruiker die wordt geblokkeerd door:

* Een beleid voor aanmeldingsrisico wordt ook wel bekend als verdachte aanmelding
* Een beleid voor gebruikersrisico's wordt ook wel bekend als een account op risico 's

## <a name="unblocking-suspicious-sign-ins"></a>Blokkering verdachte aanmeldingen
Als u wilt deblokkeren een verdachte aanmelding, hebt u de volgende opties:

1. **Zich aanmelden vanaf een vertrouwde locatie of een apparaat** -een veelvoorkomende reden voor geblokkeerde verdachte aanmeldingen zijn aanmeldpogingen vanaf onbekende locaties of apparaten. Uw gebruikers kunnen snel te bepalen of dit de reden blokkeren is door te proberen om aan te melden vanaf een vertrouwde locatie of het apparaat.
2. **Uitsluiten van beleid** : als u denkt dat de huidige configuratie van uw beleid aanmelden wordt veroorzaakt door problemen voor specifieke gebruikers, kunt u de gebruikers uitsluiten van deze. Zie voor meer informatie, [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
3. **Beleid uitschakelen** : als u denkt dat uw beleidsconfiguratie wordt veroorzaakt door problemen voor al uw gebruikers, kunt u het beleid uitschakelen. Zie voor meer informatie, [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="unblocking-accounts-at-risk"></a>Blokkering accounts lopen risico
Als u wilt deblokkeren van een account op risico's, hebt u de volgende opties:

1. **Wachtwoord opnieuw instellen** -kunt u van de gebruiker-wachtwoord opnieuw instellen. 
2. **Alle risicogebeurtenissen verwijderen** : de gebruiker beleid blokken van het risico van een gebruiker als de geconfigureerde gebruiker voor het blokkeren van toegang tot de risiconiveau is bereikt. U kunt een gebruiker beperken het risiconiveau door handmatig sluiten risicogebeurtenissen die worden gerapporteerd. 
3. **Uitsluiten van beleid** : als u denkt dat de huidige configuratie van uw beleid aanmelden wordt veroorzaakt door problemen voor specifieke gebruikers, kunt u de gebruikers uitsluiten van deze. Zie voor meer informatie, [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
4. **Beleid uitschakelen** : als u denkt dat uw beleidsconfiguratie wordt veroorzaakt door problemen voor al uw gebruikers, kunt u het beleid uitschakelen. Zie voor meer informatie, [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="next-steps"></a>Volgende stappen
 
Wilt u meer weten over Azure AD Identity Protection? Bekijk [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
