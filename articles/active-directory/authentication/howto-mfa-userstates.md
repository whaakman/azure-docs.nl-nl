---
title: Status van Microsoft Azure multi-factor Authentication gebruikers
description: Meer informatie over gebruikersstatussen in Azure multi-factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: b809097e50a17178da12fdb424eba08dc8e0c4cb
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
ms.locfileid: "33866888"
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Verificatie in twee stappen voor een gebruiker of groep vereisen

Er zijn twee manieren voor verificatie in twee stappen. De eerste mogelijkheid is het inschakelen van MFA voor een gebruiker. Wanneer gebruikers afzonderlijk zijn ingeschakeld, worden ze gevraagd zich te authenticeren met behulp van hun tweede stap telkens wanneer ze zich aanmelden (met enkele uitzonderingen, zoals wanneer ze zich vanaf vertrouwde-IP-adressen aanmelden of wanneer de functie _dit apparaat onhouden_ is ingeschakeld). De tweede optie is het instellen van een beleid voor voorwaardelijke toegang waarvoor verificatie in twee stappen is vereist onder bepaalde omstandigheden.

>[!TIP] 
>Kies een van deze methoden om te vereisen verificatie in twee stappen, niet beide. Een gebruiker inschakelen voor Azure multi-factor Authentication overschrijft alle beleidsregels voor voorwaardelijke toegang.

## <a name="which-option-is-right-for-you"></a>Welke optie is geschikt voor u?

**Azure multi-factor Authentication inschakelen door het wijzigen van de status van gebruikers** is de traditionele aanpak voor het vereisen van verificatie in twee stappen. De tool werkt voor zowel Azure MFA in de cloud en Azure MFA-Server. Alle gebruikers waarvoor u tweestapsverificatie inschakelt, worden gevraagd verificatie in twee stappen uit te voeren telkens wanneer ze zich aanmelden. Alle beleidsregels voor voorwaardelijke toegang die mogelijk gevolgen hebben voor die gebruiker worden onderdrukt. 

**Azure multi-factor Authentication inschakelen met een beleid voor voorwaardelijke toegang** is een flexibelere benadering voor het vereisen van verificatie in twee stappen. Deze functie werkt alleen voor Azure MFA in de cloud en _voorwaardelijke toegang_ is een [functie van Azure Active Directory Premium](https://www.microsoft.com/cloud-platform/azure-active-directory-features). U kunt een beleid voor voorwaardelijke toegang voor zowel afzonderlijke gebruikers als groepen maken. U kunt bijvoorbeeld een hoog risico-groep en een laag risico-groep maken, waarbij verificatie in twee stappen alleen nodig is voor cloud-apps met een hoog risico. 

Beide opties vragen gebruikers om zich te registreren voor Azure multi-factor Authentication de eerste keer dat ze zich aanmelden nadat u de vereisten hebt ingeschakeld. Beide opties werken ook met de configureerbare [Azure multi-factor Authentication-instellingen](howto-mfa-mfasettings.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Azure MFA inschakelen door het wijzigen van de gebruikersstatus

Gebruikersaccounts in Azure multi-factor Authentication hebben de volgende drie afzonderlijke statussen:

| Status | Beschrijving | Van invloed op een niet-browsertoepassingen | Van invloed op een browser-apps | Moderne authenticatie die van invloed op een |
|:---:|:---:|:---:|:--:|:--:|
| Uitgeschakeld |De standaardstatus voor een nieuwe gebruiker die niet zijn geregistreerd bij Azure MFA. |Nee |Nee |Nee |
| Ingeschakeld |De gebruiker is geregistreerd in Azure MFA, maar is niet geregistreerd. Ze gevraagd om u te registreren van de volgende keer dat ze zich aanmelden. |Nee.  Ze blijven werken totdat het registratieproces is voltooid. | Ja. Nadat de sessie is verlopen, is de registratie van de Azure MFA is vereist.| Ja. Nadat het toegangstoken is verlopen, kan de registratie van de Azure MFA is vereist. |
| Afgedwongen |De gebruiker is ingeschreven en het registratieproces is voltooid voor de Azure MFA. |Ja.  Apps vereisen app-wachtwoorden. |Ja. Azure MFA is vereist bij het aanmelden. | Ja. Azure MFA is vereist bij het aanmelden. |

De status van een gebruiker zijn of een beheerder heeft ingeschreven ze in Azure MFA en of ze het registratieproces voltooid.

Alle gebruikers beginnen *uitgeschakelde*. Als u inschrijven voor gebruikers in de Azure MFA, hun status gewijzigd in *ingeschakeld*. Wanneer ingeschakelde gebruikers aanmelden en het registratieproces volledig doorlopen, hun status verandert in *afgedwongen*.  

### <a name="view-the-status-for-a-user"></a>De status voor een gebruiker weergeven

Gebruik de volgende stappen uit voor toegang tot de pagina waar u kunt bekijken en beheren van de status van gebruikers:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
   ![Selecteer de multi-factor Authentication](./media/howto-mfa-userstates/selectmfa.png)
4. Een nieuwe pagina met de status van de gebruikers wordt geopend.
   ![Gebruikersstatus voor meervoudige verificatie - schermafbeelding](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>De status van een gebruiker wijzigen

1. De voorgaande stappen gebruiken om te krijgen tot de Azure multi-factor Authentication **gebruikers** pagina.
2. Zoek de gebruiker die u wilt inschakelen voor Azure MFA. Mogelijk moet u de weergave bovenaan wijzigen. 
   ![Zoek de gebruiker - schermafbeelding](./media/howto-mfa-userstates/enable1.png)
3. Schakel het selectievakje naast hun naam.
4. Aan de rechterkant, onder **snelle stappen**, kies **inschakelen** of **uitschakelen**.
   ![Inschakelen van de geselecteerde gebruiker - schermafbeelding](./media/howto-mfa-userstates/user1.png)

   >[!TIP]
   >*Ingeschakeld* gebruikers automatisch worden overgeschakeld naar de *afgedwongen* wanneer ze zich registreren voor Azure MFA. Kan niet handmatig wijzigen de gebruikersstatus *afgedwongen*. 

5. Bevestig uw selectie in het pop-upvenster dat wordt geopend. 

Nadat u gebruikers hebt ingeschakeld, gebruikers informeren via e-mail. Laat ze weten dat ze gevraagd om te registreren van de volgende keer dat ze zich aanmelden. Als uw organisatie gebruikmaakt van niet-browser-apps die moderne verificatie niet ondersteunen, moeten zij ook app-wachtwoorden maken. U kunt ook een koppeling naar de [Azure MFA eindgebruiker handleiding](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md) waarmee ze aan de slag.

### <a name="use-powershell"></a>PowerShell gebruiken
Status van de gebruiker te wijzigen met behulp van [Azure AD PowerShell](/powershell/azure/overview), wijzigen `$st.State`. Er zijn drie mogelijke statussen:

* Ingeschakeld
* Afgedwongen
* Uitgeschakeld  

Verplaats niet gebruikers rechtstreeks naar de *afgedwongen* status. Als u dit doet, niet op browser gebaseerde apps werken niet meer omdat de gebruiker niet heeft doorlopen Azure MFA-registratie en verkregen een [app-wachtwoord](howto-mfa-mfasettings.md#app-passwords). 

Met behulp van PowerShell is een goede optie wanneer u gebruikers bulksgewijs wilt. Maak een PowerShell-script waarmee een lijst met gebruikers doorlopen en kan ze:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Het volgende script is een voorbeeld:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Azure MFA met beleid voor voorwaardelijke toegang inschakelen

_Voorwaardelijke toegang_ is een betaald onderdeel van Azure Active Directory, met veel configuratie-opties. Deze stappen doorlopen die een manier om een beleid te maken. Lees voor meer informatie over [voorwaardelijke toegang in Azure Active Directory](../active-directory-conditional-access-azure-portal.md).

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **voorwaardelijke toegang**.
3. Selecteer **nieuw beleid**.
4. Onder **toewijzingen**, selecteer **gebruikers en groepen**. Gebruik de **opnemen** en **uitsluiten** tabbladen om op te geven welke gebruikers en groepen wordt beheerd door het beleid.
5. Onder **toewijzingen**, selecteer **Cloud-apps**. Opneemt **alle cloud-apps**.
6. Onder **toegangscontroles**, selecteer **Grant**. Kies **meervoudige authenticatie**.
7. Schakel **beleid inschakelen** naar **op**, en selecteer vervolgens **opslaan**.

De andere opties in het beleid voor voorwaardelijke toegang bieden u de mogelijkheid om op te geven precies zien wanneer verificatie in twee stappen vereist is. Bijvoorbeeld, kunt u een beleid zoals deze: wanneer aannemers probeert te krijgen van de app inkoop van niet-vertrouwde netwerken op apparaten die niet lid zijn van een domein, moet u verificatie in twee stappen. 

## <a name="next-steps"></a>Volgende stappen

- Tips krijgen voor de [aanbevolen procedures voor voorwaardelijke toegang](../active-directory-conditional-access-best-practices.md).

- Beheren van Azure multi-factor Authentication-instellingen voor [uw gebruikers en hun apparaten](howto-mfa-userdevicesettings.md).
