---
title: Azure AD gelaagde wachtwoordbeveiliging | Microsoft Docs
description: Legt uit hoe u Azure AD worden afgedwongen sterke wachtwoorden en wachtwoorden van gebruikers beschermt tegen cyberbeveiliging criminelen,
services: active-directory
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: barlan
ms.openlocfilehash: 683badcfb67dd9e98058d560a6b13d1a3474d3e9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a>Een benadering meerdere lagen van beveiliging voor Azure AD-wachtwoord

Dit artikel worden enkele aanbevolen procedures kunt u als een gebruiker of als een beheerder om uw Azure Active Directory (Azure AD) of de Microsoft-Account te beschermen.

 > [!NOTE]
 > **Bent u hier terechtgekomen omdat u problemen ondervindt met het aanmelden?** Als dat het geval is, vindt u hier meer informatie over het [wijzigen en opnieuw instellen van uw eigen wachtwoord](active-directory-passwords-update-your-own-password.md).
 >
 > Azure AD-beheerders kunnen gebruikerswachtwoorden met behulp van de instructies in het artikel [opnieuw instellen van het wachtwoord voor een gebruiker in Azure Active Directory](active-directory-users-reset-password-azure-portal.md).
 >

## <a name="password-requirements"></a>Vereisten voor wachtwoorden

In Azure AD worden de volgende algemene benaderingen gebruikt om wachtwoorden te beveiligen:

* Vereisten voor de lengte van het wachtwoord
* Vereisten voor wachtwoordcomplexiteit
* Gepland en automatisch verlopen van wachtwoorden

Zie het onderwerp voor informatie over het wachtwoord opnieuw instellen in Azure Active Directory, [Azure AD selfservice voor wachtwoordherstel voor IT-professionals](active-directory-passwords-update-your-own-password.md).

## <a name="azure-ad-password-protections"></a>Bescherming van Azure AD-wachtwoord

Azure AD en de Microsoft-Accountsysteem bedrijfstak bewezen benaderingen gebruiken om te controleren of de beveiliging van gebruikers- en wachtwoorden, waaronder:

* Dynamisch uitsluiten van wachtwoorden
* Smart Password Lockout

Zie voor informatie over wachtwoordbeheer op basis van huidige research in dit technisch document [wachtwoord richtlijnen](http://aka.ms/passwordguidance).

### <a name="dynamically-banned-passwords"></a>Dynamisch uitsluiten van wachtwoorden

Azure AD en Microsoft-Accounts wachtwoordbeveiliging veiligstellen door dynamisch verboden veelgebruikte wachtwoorden. Het Azure AD Identity Protection-team analyseert regelmatig verboden wachtwoordenlijsten, voorkomen dat gebruikers wachtwoorden veelgebruikte kiezen. Deze service is beschikbaar voor klanten van Azure AD en de Microsoft-accountservice.

Bij het maken van wachtwoorden, is het belangrijk zijn voor beheerders om aan te raden gebruikers kiezen wachtwoord zinnen met een unieke combinatie van letters, cijfers, tekens of woorden. Deze aanpak kunt u gebruikerswachtwoorden nagenoeg onmogelijk waarmee is geknoeid, maar gebruikers te onthouden gemakkelijker te maken.

#### <a name="password-breaches"></a>Wachtwoord schendingen

Microsoft werkt altijd om te blijven van één stap tevoren cyberbeveiliging criminelen.

Het Azure AD Identity Protection-team analyseert voortdurend wachtwoorden die vaak worden gebruikt. Cybercriminelen maken gebruik van vergelijkbare strategieën voor hun aanvallen, zoals het samenstellen van een [rainbow-tabel](https://en.wikipedia.org/wiki/Rainbow_table) om wachtwoord-hashes te kraken.

Microsoft analyseert voortdurend [gegevensschendingen](https://www.privacyrights.org/data-breaches) om een dynamisch bijgewerkte lijst met uitgesloten wachtwoorden te onderhouden die ervoor zorgt dat kwetsbare wachtwoorden worden uitgesloten voordat ze een echte bedreiging kunnen vormen voor klanten van Azure AD. Raadpleeg het [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx) voor meer informatie over onze inspanningen op dit gebied. Dit rapport is alleen in het Engels beschikbaar.

### <a name="smart-password-lockout"></a>Smart Password Lockout

Als Azure AD vaststelt dat een mogelijke cybercrimineel probeert om het wachtwoord van een gebruiker te achterhalen, wordt het gebruikersaccount vergrendeld met Smart Password Lockout. Azure AD bevat logica om het risico te bepalen dat samenhangt met specifieke aanmeldingssessies. Vervolgens wordt met de meest recente beveiligingsgegevens toegepast accountvergrendeling, zodat u cyberbeveiliging bedreigingen te stoppen.

Als een gebruiker is vergrendeld buiten Azure AD, wordt het scherm op de link die erna lijkt:

  ![Toegang tot Azure AD geblokkeerd](./media/active-directory-secure-passwords/locked-out-azuread.png)

Voor andere Microsoft-accounts lijkt het scherm op de link die erna:

  ![Toegang tot Microsoft-account geblokkeerd](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Zie het onderwerp voor informatie over het wachtwoord opnieuw instellen in Azure Active Directory, [Azure AD selfservice voor wachtwoordherstel voor IT-professionals](active-directory-passwords-update-your-own-password.md).

  >[!NOTE]
  >Als u een Azure AD-beheerder bent, kunt u [Windows Hello](https://www.microsoft.com/windows/windows-hello) gebruiken om ervoor te zorgen dat uw gebruikers helemaal geen traditionele wachtwoorden meer hoeven te maken.
  >

## <a name="next-steps"></a>Volgende stappen

* [Uw eigen wachtwoord bijwerken](active-directory-passwords-update-your-own-password.md)
* [The fundamentals of Azure identity management](fundamentals-identity.md) (De grondbeginselen van Azure-identiteitsbeheer)
* [Activiteit voor het rapport op wachtwoord opnieuw instellen](active-directory-passwords-reporting.md)
