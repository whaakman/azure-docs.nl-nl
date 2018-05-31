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

De status van een gebruiker geeft weer of een beheerder hen heeft ingeschreven in Azure MFA en of ze het registratieproces hebben voltooid.

Alle gebruikers beginnen *Uitgeschakeld*. Als u een gebruiker zich inschrijft via Azure MFA, wordt zijn of haar status gewijzigd naar *Ingeschakeld*. Zodra de ingeschakelde gebruiker zich heeft aangemeld en het registratieproces heeft voltooid, wordt de status ingesteld op *Afgedwongen*.

### <a name="view-the-status-for-a-user"></a>De status voor een gebruiker weergeven

Gebruik de volgende stappen om de pagina te openen waar u de status van een gebruiker kunt bekijken en beheren:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
   ![Selecteer de multi-factor Authentication](./media/howto-mfa-userstates/selectmfa.png)
4. Een nieuwe pagina met de status van de gebruikers wordt geopend.
   ![Gebruikersstatus voor meervoudige verificatie - schermafbeelding](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>De status van een gebruiker wijzigen

1. Gebruik de voorgaande stappen om toegang te krijgen tot de Azure MFA **gebruikers**-pagina.
2. Zoek de gebruiker die u wilt inschakelen voor Azure MFA. Mogelijk moet u de weergave bovenaan wijzigen. 
   ![Zoek de gebruiker - schermafbeelding](./media/howto-mfa-userstates/enable1.png)
3. Schakel het selectievakje naast de naam van de gebruiker in.
4. Aan de rechterkant, onder **snelle stappen**, kiest u **inschakelen** of **uitschakelen**.
   ![Inschakelen van de geselecteerde gebruiker - schermafbeelding](./media/howto-mfa-userstates/user1.png)

   >[!TIP]
   >*Ingeschakelde* gebruikers worden automatisch omgezet naar *Afgedwongen* zodra ze zich registreren voor Azure MFA. Wijzig de status van de gebruiker niet handmatig in *Afgedwongen*.

5. Bevestig uw selectie in het pop-upvenster dat wordt geopend. 

Nadat u gebruikers hebt ingeschakeld, kunt u gebruikers informeren via e-mail. Laat ze weten dat ze gevraagd worden om zich te registreren de volgende keer dat ze zich aanmelden. Als uw organisatie gebruikmaakt van niet-browser-apps die moderne verificatie niet ondersteunen, moeten zij ook app-wachtwoorden maken. U kunt ook een koppeling naar de [Azure MFA eindgebruiker handleiding](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md) meesturen waarmee gebruikers aan de slag kunnen.

### <a name="use-powershell"></a>PowerShell gebruiken
Status van de gebruiker te wijzigen met behulp van [Azure AD PowerShell](/powershell/azure/overview), wijzigen `$st.State`. Er zijn drie mogelijke statussen:

* Ingeschakeld
* Afgedwongen
* Uitgeschakeld  

Verplaats gebruikers niet rechtstreeks naar de status *Afgedwongen*. Als u dit doet, werken apps die niet op een browser zijn gebaseerd niet meer, omdat de gebruiker het registratieproces voor Azure MFA niet heeft doorlopen en dus geen [app-wachtwoord](howto-mfa-mfasettings.md#app-passwords) heeft opgehaald.

Het inschakelen met behulp van PowerShell is een goede optie, wanneer u gebruikers bulksgewijs wilt inschakelen. Maak een PowerShell-script waarmee een lijst met gebruikers wordt doorgelopen en ingeschakeld:

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

_Voorwaardelijke toegang_ is een betaalde functie van Azure Active Directory met veel configuratieopties. Deze stappen tonen u een van de manier om een beleid te maken. Raadpleeg [voorwaardelijke toegang in Azure Active Directory](../active-directory-conditional-access-azure-portal.md) voor meer informatie.

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **voorwaardelijke toegang**.
3. Selecteer **nieuw beleid**.
4. Onder **toewijzingen**, selecteer **gebruikers en groepen**. Gebruik de **opnemen** en **uitsluiten** tabbladen om op te geven welke gebruikers en groepen wordt beheerd door het beleid.
5. Onder **toewijzingen**, selecteer **Cloud-apps**. Opneemt **alle cloud-apps**.
6. Onder **toegangscontroles**, selecteer **Grant**. Kies **meervoudige authenticatie**.
7. Schakel **beleid inschakelen** naar **op**, en selecteer vervolgens **opslaan**.

De andere opties in het beleid voor voorwaardelijke toegang bieden u de mogelijkheid om op te geven precies zien wanneer verificatie in twee stappen vereist is. U kunt bijvoorbeeld een beleid instellen waarbij gebruikers gevraagd worden zich te verifiëren met behulp van authenticatie in twee stappen als ze inloggen op apparaten die niet lid zijn van het domein.

## <a name="next-steps"></a>Volgende stappen

- Tips krijgen voor de [aanbevolen procedures voor voorwaardelijke toegang](../active-directory-conditional-access-best-practices.md).

- Beheren van Azure multi-factor Authentication-instellingen voor [uw gebruikers en hun apparaten](howto-mfa-userdevicesettings.md).
