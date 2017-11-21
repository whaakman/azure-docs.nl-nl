---
title: Status van Microsoft Azure multi-factor Authentication gebruikers
description: Meer informatie over de status van de gebruikers in de Azure MFA.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 6a0f8cb76684a6efcc5e2d4be05493f18d5d4c76
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Verificatie in twee stappen voor een gebruiker of groep vereisen

Er zijn twee manieren voor het vereisen van verificatie in twee stappen. De eerste optie is om in te schakelen elke individuele gebruiker voor de Azure multi-factor Authentication (MFA). Wanneer gebruikers afzonderlijk zijn ingeschakeld, uitvoeren ze altijd verificatie in twee stappen (met enkele uitzonderingen, zoals wanneer ze zich aanmelden vanaf vertrouwde IP-adressen of als de functie onthouden apparaten is ingeschakeld). De tweede optie is voor het instellen van een beleid voor voorwaardelijke toegang waarvoor verificatie in twee stappen onder bepaalde omstandigheden.

>[!TIP] 
>Kies een van deze methoden om te vereisen verificatie in twee stappen, niet beide. Een gebruiker inschakelen voor Azure MFA overschrijft alle beleidsregels voor voorwaardelijke toegang.

## <a name="which-option-is-right-for-you"></a>Welke optie is geschikt voor u

**Azure MFA inschakelen door het wijzigen van de status van gebruikers** de traditionele aanpak voor het vereisen van verificatie in twee stappen. De Tool werkt voor zowel Azure MFA in de cloud en Azure MFA-Server. Alle gebruikers die u in staat stellen hebben dezelfde ervaring, dat wil zeggen verificatie in twee stappen uitvoeren telkens wanneer ze zich aanmelden. Alle beleidsregels voor voorwaardelijke toegang die mogelijk gevolgen hebben voor die gebruiker inschakelen van een gebruiker worden onderdrukt. 

**Azure MFA met beleid voor voorwaardelijke toegang inschakelen** is een flexibelere benadering voor het vereisen van verificatie in twee stappen. Deze alleen werkt voor Azure MFA in de cloud echter en voorwaardelijke toegang is een [functie van Azure Active Directory betaald](https://www.microsoft.com/cloud-platform/azure-active-directory-features). U kunt beleid voor voorwaardelijke toegang die voor zowel afzonderlijke gebruikers als groepen gelden maken. Met een hoog risico groepen meer beperkingen dan laag risico groepen kunnen worden opgegeven of verificatie in twee stappen kan worden alleen vereist voor met een hoog risico cloud-apps en voor de laag risico die zijn overgeslagen. 

Beide opties gebruikers vragen om te registreren voor Azure multi-factor Authentication de eerste keer dat ze zich aanmelden nadat de vereisten hebt ingeschakeld. Beide opties werken ook met de configureerbare [Azure multi-factor Authentication-instellingen](multi-factor-authentication-whats-next.md)

## <a name="enable-azure-mfa-by-changing-user-status"></a>Azure MFA inschakelen door het wijzigen van de gebruikersstatus

Gebruikersaccounts in Azure multi-factor Authentication hebben de volgende drie afzonderlijke statussen:

| Status | Beschrijving | Van invloed op een niet-browsertoepassingen |
|:---:|:---:|:---:|
| Uitgeschakeld |De standaardstatus voor een nieuwe gebruiker is niet ingeschreven voor Azure multi-factor Authentication (MFA). |Nee |
| Ingeschakeld |De gebruiker is geregistreerd in Azure MFA, maar is niet geregistreerd. Ze wordt gevraagd om te registreren van de volgende keer dat ze zich aanmelden. |Nee.  Ze blijven werken totdat het registratieproces is voltooid. |
| Afgedwongen |De gebruiker is ingeschreven en het registratieproces is voltooid voor de Azure MFA. |Ja.  Apps vereisen app-wachtwoorden. |

De status van een gebruiker zijn of een beheerder heeft ingeschreven ze in Azure MFA en of ze het registratieproces voltooid.

Alle gebruikers beginnen *uitgeschakeld*. Wanneer u gebruikers in de Azure MFA, hun statuswijzigingen inschrijft *ingeschakeld*. Wanneer ingeschakelde gebruikers aanmelden en het registratieproces volledig doorlopen, hun status verandert in *afgedwongen*.  

### <a name="view-the-status-for-a-user"></a>De status voor een gebruiker weergeven

Gebruik de volgende stappen uit voor toegang tot de pagina waar u kunt bekijken en beheren van de status van gebruikers:

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
   ![Selecteer de multi-factor Authentication](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Een nieuwe pagina die wordt weergegeven in de status van de gebruikers, wordt geopend.
   ![Gebruikersstatus voor meervoudige verificatie - schermafbeelding](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-status-for-a-user"></a>De status van een gebruiker wijzigen

1. Gebruik de voorgaande stappen hebt uitgevoerd naar de pagina van de gebruikers multi-factor authentication-server.
2. Zoek de gebruiker die u wilt inschakelen voor Azure MFA. Mogelijk moet u de weergave bovenaan wijzigen. 
   ![Zoek de gebruiker - schermafbeelding](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Schakel het selectievakje naast hun naam.
4. Kies aan de rechterkant, onder snelle stappen **inschakelen** of **uitschakelen**.
   ![Inschakelen van de geselecteerde gebruiker - schermafbeelding](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Ingeschakeld* gebruikers automatisch overschakelen naar *afgedwongen* wanneer ze zich registreren voor Azure MFA. U mag niet de status van de gebruiker afgedwongen handmatig wijzigen. 

5. Bevestig uw selectie in het pop-upvenster dat wordt geopend. 

Nadat u gebruikers hebt ingeschakeld, moeten ze hierover te informeren via e-mail. Laat ze weten dat ze gevraagd om te registreren van de volgende keer dat ze zich aanmelden. Ook als uw organisatie gebruikmaakt van niet-browser-apps die moderne verificatie niet ondersteunen, moet ze app-wachtwoorden maken. U kunt ook een koppeling naar opnemen onze [Azure MFA eindgebruiker handleiding](./end-user/multi-factor-authentication-end-user.md) waarmee ze aan de slag.

### <a name="use-powershell"></a>PowerShell gebruiken
Wijzigen van de status van de status wordt gebruikt door gebruiker [Azure AD PowerShell](/powershell/azure/overview), wijzigen `$st.State`. Er zijn drie mogelijke statussen:

* Ingeschakeld
* Afgedwongen
* Uitgeschakeld  

Verplaats niet gebruikers rechtstreeks naar de *afgedwongen* status. Niet-browsertoepassingen werken dan niet meer omdat de gebruiker de MFA-registratie niet heeft doorlopen en geen [toepassingswachtwoord](multi-factor-authentication-whats-next.md#app-passwords) heeft verkregen. 

Met behulp van PowerShell is een goede optie wanneer u gebruikers bulksgewijs wilt. Maak een PowerShell-script waarmee een lijst met gebruikers doorlopen en kan ze:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Hier volgt een voorbeeld:

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

Voorwaardelijke toegang is een betaald functie van Azure Active Directory, met veel mogelijke configuratie-opties. Deze stappen doorlopen die een manier om een beleid te maken. Lees voor meer informatie over [voorwaardelijke toegang in Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **Azure Active Directory** > **voorwaardelijke toegang**.
3. Selecteer **nieuw beleid**.
4. Onder **toewijzingen**, selecteer **gebruikers en groepen**. Gebruik de **opnemen** en **uitsluiten** tabbladen om op te geven welke gebruikers en groepen worden beheerd door het beleid.
5. Onder **toewijzingen**, selecteer **Cloud-apps**. Opneemt **alle cloud-apps**.
6. Onder **toegangscontroles**, selecteer **Grant**. Kies **meervoudige authenticatie**.
7. Schakel **beleid inschakelen** naar **op** en selecteer vervolgens **opslaan**.

De andere opties in het beleid voor voorwaardelijke toegang kunnen u precies zien wanneer verificatie in twee stappen moet worden opgeven. Bijvoorbeeld, u kunt een beleid waarin wordt vermeld: wanneer aannemers probeert te krijgen van de app inkoop van niet-vertrouwde netwerken op apparaten die niet lid zijn van een domein, moet u verificatie in twee stappen. 

## <a name="next-steps"></a>Volgende stappen

- Tips krijgen voor de [aanbevolen procedures voor voorwaardelijke toegang](../active-directory/active-directory-conditional-access-best-practices.md)

- Beheren van multi-factor Authentication-instellingen voor [uw gebruikers en hun apparaten](multi-factor-authentication-manage-users-and-devices.md)