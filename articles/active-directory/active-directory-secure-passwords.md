---
title: Gelaagde wachtwoordbeveiliging van Azure AD | Microsoft Docs
description: Hier wordt uitgelegd hoe Azure AD sterke wachtwoorden afdwingt en wachtwoorden van gebruikers beschermt tegen cybercriminelen,
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/28/2017
ms.author: lizross
ms.openlocfilehash: e3e97a5a9b768b3b9d4a36627d28955be1f3d9df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056916"
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a>Een meerlaagse benadering van beveiliging van Azure AD-wachtwoordbeveiliging

In dit artikel vindt u enkele aanbevolen procedures die u als gebruiker of beheerder kunt volgen om uw accounts van Azure Active Directory (Azure AD) of Microsoft-account te beveiligen.

 > [!NOTE]
 > **Bent u hier terechtgekomen omdat u problemen ondervindt met het aanmelden?** Als dat het geval is, vindt u hier meer informatie over het [wijzigen en opnieuw instellen van uw eigen wachtwoord](user-help/active-directory-passwords-update-your-own-password.md).
 >
 > Azure AD-beheerders kunnen wachtwoorden van gebruikers opnieuw instellen overeenkomstig de richtlijnen in het artikel [Het wachtwoord voor een gebruiker in Azure Active Directory opnieuw instellen](fundamentals/active-directory-users-reset-password-azure-portal.md).
 >

## <a name="password-requirements"></a>Vereisten voor wachtwoorden

In Azure AD worden de volgende algemene benaderingen gebruikt om wachtwoorden te beveiligen:

* Vereisten voor de lengte van het wachtwoord
* Vereisten voor de complexiteit van het wachtwoord
* Gepland en automatisch verlopen van wachtwoorden

Zie het onderwerp [Azure AD-selfservice voor wachtwoordherstel voor IT-professionals](user-help/active-directory-passwords-update-your-own-password.md) voor informatie over het opnieuw instellen van wachtwoorden in Azure Active Directory.

## <a name="azure-ad-password-protections"></a>Wachtwoordbeveiliging in Azure AD

Azure AD en het Microsoft-accountsysteem maken gebruik van gangbare methoden uit de branche om de wachtwoorden van gebruikers en beheerders optimaal te beveiligen, waaronder:

* Dynamisch uitsluiten van wachtwoorden
* Smart Password Lockout

Zie het Engelstalige document [Password Guidance](https://aka.ms/passwordguidance) voor informatie over wachtwoordbeheer op basis van actueel onderzoek.

### <a name="dynamically-banned-passwords"></a>Dynamisch uitsluiten van wachtwoorden

Azure AD en Microsoft Accounts garanderen de veiligheid van wachtwoorden door vaak voorkomende wachtwoorden dynamisch uit te sluiten. Het Azure AD Identity Protection-team maakt regelmatig analyses van lijsten met uitgesloten wachtwoorden, om te voorkomen dat gebruikers wachtwoorden selecteren die regelmatig worden gebruikt. Deze service is beschikbaar voor klanten van Azure AD en de Microsoft-accountservice.

Het is belangrijk dat beheerders hun gebruikers adviseren om bij het maken van wachtwoorden een wachtwoord te kiezen dat bestaat uit een unieke combinatie van letters, cijfers en tekens. Op deze manier wordt het bijna onmogelijk om gebruikerswachtwoorden te achterhalen, maar kunnen gebruikers hun wachtwoorden gemakkelijker onthouden.

#### <a name="password-breaches"></a>Wachtwoordschending

Microsoft stelt altijd alles in het werk om cybercriminelen een stap voor te blijven.

Het Azure AD Identity Protection-team analyseert voortdurend wachtwoorden die vaak worden gebruikt. Cybercriminelen maken gebruik van vergelijkbare strategieën voor hun aanvallen, zoals het samenstellen van een [rainbow-tabel](https://en.wikipedia.org/wiki/Rainbow_table) om wachtwoord-hashes te kraken.

Microsoft analyseert voortdurend [gegevensschendingen](https://www.privacyrights.org/data-breaches) om een dynamisch bijgewerkte lijst met uitgesloten wachtwoorden te onderhouden die ervoor zorgt dat kwetsbare wachtwoorden worden uitgesloten voordat ze een echte bedreiging kunnen vormen voor klanten van Azure AD. Raadpleeg het [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx) voor meer informatie over onze inspanningen op dit gebied. Dit rapport is alleen in het Engels beschikbaar.

### <a name="smart-password-lockout"></a>Smart Password Lockout

Als Azure AD vaststelt dat een mogelijke cybercrimineel probeert om het wachtwoord van een gebruiker te achterhalen, wordt het gebruikersaccount vergrendeld met Smart Password Lockout. Azure AD bevat logica om het risico te bepalen dat samenhangt met specifieke aanmeldingssessies. Met behulp van de meest recente beveiligingsgegevens wordt er semantiek voor accountvergrendeling toegepast om bedreigingen door cybercriminelen te stoppen.

Als de toegang van een gebruiker tot Azure AD tijdelijk is geblokkeerd, zien ze een scherm zoals dit:

  ![Toegang tot Azure AD geblokkeerd](./media/active-directory-secure-passwords/locked-out-azuread.png)

Voor andere Microsoft-accounts ziet het scherm er ongeveer zo uit:

  ![Toegang tot Microsoft-account geblokkeerd](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Zie het onderwerp [Azure AD-selfservice voor wachtwoordherstel voor IT-professionals](user-help/active-directory-passwords-update-your-own-password.md) voor informatie over het opnieuw instellen van wachtwoorden in Azure Active Directory.

  >[!NOTE]
  >Als u een Azure AD-beheerder bent, kunt u [Windows Hello](https://www.microsoft.com/windows/windows-hello) gebruiken om ervoor te zorgen dat uw gebruikers helemaal geen traditionele wachtwoorden meer hoeven te maken.
  >

## <a name="next-steps"></a>Volgende stappen

* [Uw eigen wachtwoord bijwerken](user-help/active-directory-passwords-update-your-own-password.md)
* [The fundamentals of Azure identity management](fundamentals-identity.md) (De grondbeginselen van Azure-identiteitsbeheer)
* [Activiteit voor wachtwoord opnieuw instellen melden](authentication/howto-sspr-reporting.md)
