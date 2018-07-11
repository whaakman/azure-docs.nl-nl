---
title: Pass through-verificatie Azure AD - snel starten | Microsoft Docs
description: In dit artikel wordt beschreven hoe u aan de slag met Azure Active Directory (Azure AD) Pass through-verificatie.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, installatie van Active Directory, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: fc98f15303f23937d58131de971d5c60017c9034
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917707"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory Pass through-verificatie: Snel starten

## <a name="deploy-azure-ad-pass-through-authentication"></a>Azure AD Pass-through-verificatie implementeren

Azure Active Directory (Azure AD) Pass through-verificatie kunnen uw gebruikers zich aanmeldt bij zowel on-premises en cloudgebaseerde toepassingen met behulp van dezelfde wachtwoorden. Pass through-verificatie aanmeldt gebruikers door het valideren van hun wachtwoord direct op basis van on-premises Active Directory.

>[!IMPORTANT]
>Als u deze functie via een preview-versie gebruiken, zorgt u ervoor dat u de preview-versies van de verificatie-Agents bijwerken met behulp van de instructies in [Azure Active Directory Pass through-verificatie: Upgrade preview Verificatie-Agents](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Volg deze instructies voor het implementeren van Pass through-verificatie:

## <a name="step-1-check-the-prerequisites"></a>Stap 1: Controleer de vereisten

Zorg ervoor dat de volgende vereisten voldaan is.

### <a name="in-the-azure-active-directory-admin-center"></a>In het Azure Active Directory-beheercentrum

1. Een globale beheerder van alleen-cloud-account maken in uw Azure AD-tenant. Op deze manier kunt u de configuratie van uw tenant beheren moeten uw on-premises services mislukken of niet beschikbaar. Meer informatie over [toevoegen van een alleen-cloud globale beheerdersaccount](../active-directory-users-create-azure-portal.md). Het voltooien van deze stap is essentieel om ervoor te zorgen dat u geen toegang meer hebt tot uw tenant.
2. Voeg een of meer [aangepaste domeinnamen](../active-directory-domains-add-azure-portal.md) aan uw Azure AD-tenant. Uw gebruikers kunnen zich aanmelden met een van deze domeinnamen.

### <a name="in-your-on-premises-environment"></a>In uw on-premises-omgeving

1. Identificeer een server met Windows Server 2012 R2 of later om uit te voeren van Azure AD Connect. De server toevoegen aan hetzelfde Active Directory-forest als de gebruikers met wachtwoorden die u wilt valideren.
2. Installeer de [meest recente versie van Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) op de server die is geïdentificeerd in de vorige stap. Als u al Azure AD Connect wordt uitgevoerd hebt, controleert u of de versie 1.1.644.0 of hoger.

    >[!NOTE]
    >Azure AD Connect-versies 1.1.557.0, 1.1.558.0 1.1.561.0 en 1.1.614.0 is een probleem met betrekking tot wachtwoord-hashsynchronisatie. Als u _niet_ wilt wachtwoord-hashsynchronisatie gebruiken in combinatie met Pass through-verificatie, lees de [opmerkingen bij de release van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

3. Identificeren van een extra server (met Windows Server 2012 R2 of hoger) waar u een zelfstandige verificatie-Agent kunt uitvoeren. De verificatie-Agent-versie moet 1.5.193.0 of hoger. Deze extra server nodig is om te controleren of de hoge beschikbaarheid van aanvragen voor het aanmelden. De server toevoegen aan hetzelfde Active Directory-forest als de gebruikers met wachtwoorden die u wilt valideren.
4. Als er een firewall tussen uw servers en Azure AD, configureert u de volgende items:
   - Zorg ervoor dat de verificatie-Agents kunt aanbrengen *uitgaande* aanvragen voor Azure AD via de volgende poorten:
   
    | Poortnummer | Hoe deze wordt gebruikt |
    | --- | --- |
    | **80** | De certificaatintrekkingslijsten (CRL's) downloaden tijdens het valideren van het SSL-certificaat |
    | **443** | Alle uitgaande communicatie met de service worden verwerkt |
   
    Als uw firewall-regels op basis van de oorspronkelijke gebruikers afgedwongen, opent u deze poorten voor verkeer via Windows-services die worden uitgevoerd als een netwerkservice.
   - Als u uw firewall of proxyserver kan DNS-whitelist, lijst met toegestane adressen verbindingen met  **\*. msappproxy.net** en  **\*. servicebus.windows.net**. Als dit niet het geval is, kunt u toegang tot de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653), die elke week worden bijgewerkt.
   - De verificatie-Agents moeten toegang hebben tot **login.windows.net** en **login.microsoftonline.com** voor registratie. Open uw firewall voor ook deze URL's.
   - Blokkering voor validatie van het servercertificaat, de volgende URL's: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80**, en  **www.Microsoft.com:80**. Deze URL's worden gebruikt voor validatie van het servercertificaat met andere Microsoft-producten. Mogelijk hebt u al deze URL's geblokkeerd.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Stap 2: Exchange ActiveSync-ondersteuning (optioneel) inschakelen

Volg deze instructies voor het inschakelen van Exchange ActiveSync-ondersteuning:

1. Gebruik [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) de volgende opdracht uit te voeren:
```
Get-OrganizationConfig | fl per*
```

2. Controleer de waarde van de `PerTenantSwitchToESTSEnabled` instelling. Als de waarde **waar**, uw tenant correct is geconfigureerd. Dit is meestal het geval is bij de meeste klanten. Als de waarde **false**, voer de volgende opdracht uit:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Controleer de waarde van de `PerTenantSwitchToESTSEnabled` is nu ingesteld op **waar**. Wacht een uur voordat u doorgaat met de volgende stap.

Als u geen problemen tijdens deze stap worden geconfronteerd, controleert u de [problemen oplossen met](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues).

## <a name="step-3-enable-the-feature"></a>Stap 3: De functie inschakelen

Inschakelen van Pass-through-verificatie via [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>U kunt Pass through-verificatie inschakelen op de primaire of staging-server van Azure AD Connect. U moet het inschakelen van de primaire server.

Als u Azure AD Connect voor het eerst installeert, kiest u de [aangepaste installatiepad](active-directory-aadconnect-get-started-custom.md). Op de **aanmelden van gebruikers** pagina, kies **Pass through-verificatie** als de **aanmelding methode**. Bewerking is voltooid, wordt een Pass through-verificatie-Agent geïnstalleerd op dezelfde server als Azure AD Connect. Bovendien is de functie voor Pass through-verificatie ingeschakeld op uw tenant.

![Azure AD Connect: Aanmelden van gebruikers](./media/active-directory-aadconnect-sso/sso3.png)

Als u Azure AD Connect al hebt geïnstalleerd met behulp van de [snelle installatie](active-directory-aadconnect-get-started-express.md) of de [aangepaste installatie](active-directory-aadconnect-get-started-custom.md) pad, selecteer de **aanmelden van gebruikers wijzigen** taak in Azure AD Verbinding maken en selecteer vervolgens **volgende**. Selecteer vervolgens **Pass through-verificatie** als de methode aanmelden. Een Pass through-verificatie-Agent is geïnstalleerd op dezelfde server als Azure AD Connect is gelukt, en de functie is ingeschakeld op uw tenant.

![Azure AD Connect: Aanmelden van gebruikers wijzigen](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Pass through-verificatie is een functie op tenantniveau. Het inschakelen van is van invloed op de aanmelding voor gebruikers in _alle_ de beheerde domeinen in uw tenant. Als u van Active Directory Federation Services (AD FS) naar Pass-through-verificatie overstapt, moet u ten minste 12 uur vóór het afsluiten van de AD FS-infrastructuur wacht. Deze wachttijd is om ervoor te zorgen dat gebruikers houden bij Exchange ActiveSync tijdens de overgang aanmelden kunnen.

## <a name="step-4-test-the-feature"></a>Stap 4: De functie testen

Volg deze instructies om te controleren of u Pass-through-verificatie juist hebt ingeschakeld:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties voor uw tenant.
2. Selecteer **Azure Active Directory** in het linkerdeelvenster.
3. Selecteer **Azure AD Connect**.
4. Controleer de **Pass through-verificatie** functie wordt weergegeven als **ingeschakeld**.
5. Selecteer **Pass through-verificatie**. De **Pass through-verificatie** deelvenster worden de servers waarop de verificatie-Agents zijn geïnstalleerd.

![Azure Active Directory-beheercentrum: Azure AD Connect-deelvenster](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory-beheercentrum: deelvenster Pass through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

In deze fase, kunnen gebruikers van de beheerde domeinen in uw tenant kunnen aanmelden met behulp van Pass through-verificatie. Gebruikers van federatieve domeinen blijven echter aan te melden met behulp van AD FS of een andere federatieprovider die u eerder hebt geconfigureerd. Als u een domein via federatieve converteert beheerd, start alle gebruikers van dat domein automatisch aanmelden met behulp van Pass through-verificatie. De functie voor Pass through-verificatie, heeft geen invloed op gebruikers alleen in de cloud.

## <a name="step-5-ensure-high-availability"></a>Stap 5: Zorg voor hoge beschikbaarheid

Als u van plan bent te Pass through-verificatie in een productieomgeving implementeert, moet u ten minste één meer zelfstandige verificatie-Agent installeren. Deze verificatie-agent (s) installeren op de server (s) _andere_ dan de één actief Azure AD Connect. Deze instelling biedt hoge beschikbaarheid voor gebruiker aanmeldingsaanvragen.

Volg deze instructies om de verificatie-Agent-software te downloaden:

1. Voor het downloaden van de meest recente versie van de verificatie-Agent (versie 1.5.193.0 of hoger), zich aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties van uw tenant.
2. Selecteer **Azure Active Directory** in het linkerdeelvenster.
3. Selecteer **Azure AD Connect**, selecteer **Pass through-verificatie**, en selecteer vervolgens **-Agent downloaden**.
4. Selecteer de **voorwaarden accepteren en downloaden** knop.

![Azure Active Directory-beheercentrum: de knop verificatie-Agent downloaden](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory-beheercentrum: deelvenster van de Agent downloaden](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>U kunt ook rechtstreeks downloaden de verificatie-agentsoftware [hier](https://aka.ms/getauthagent). Lees en accepteer de verificatie-Agent [servicevoorwaarden](https://aka.ms/authagenteula) _voordat_ installeren.

Er zijn twee manieren voor het implementeren van een zelfstandige verificatie-Agent:

Eerst, u kunt dit doen interactief door alleen waarop de gedownloade uitvoerbare verificatie-Agent wordt uitgevoerd en het leveren van uw tenant-hoofdbeheerdersreferenties wanneer hierom wordt gevraagd.

Ten tweede kunt u maken en uitvoeren van een script zonder toezicht implementatie. Dit is handig als u wilt implementeren meerdere verificatie-Agents in één keer of verificatie-Agents installeren op Windows-servers die geen gebruikersinterface is ingeschakeld, of dat u geen toegang tot met extern bureaublad. Hier vindt u de instructies over het gebruik van deze benadering:

1. Voer de volgende opdracht om een verificatie-Agent te installeren: `AADConnectAuthAgentSetup.exe REGISTERCONNECTOR="false" /q`.
2. U kunt de verificatie-Agent registreren met onze service met behulp van Windows PowerShell. Maken van een object PowerShell-referenties `$cred` dat een globale beheerder van gebruikersnaam en wachtwoord voor uw tenant bevat. Voer de volgende opdracht vervangt *\<gebruikersnaam\>* en  *\<wachtwoord\>*:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
3. Ga naar **C:\Program Files\Microsoft Azure AD Connect Authentication-Agent** en voer het volgende script uit via de `$cred` -object dat u hebt gemaakt:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature PassthroughAuthentication

## <a name="next-steps"></a>Volgende stappen
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): informatie over het configureren van de functie Smart Lockout op uw tenant om te beveiligen van gebruikersaccounts.
- [Huidige beperkingen](active-directory-aadconnect-pass-through-authentication-current-limitations.md): informatie over welke scenario's met de Pass-through-verificatie worden ondersteund en welke niet.
- [Technische details](active-directory-aadconnect-pass-through-authentication-how-it-works.md): informatie over de werking van de functie voor Pass through-verificatie.
- [Veelgestelde vragen over](active-directory-aadconnect-pass-through-authentication-faq.md): vind antwoorden op veelgestelde vragen.
- [Problemen oplossen](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Grondig onderzoek van beveiliging](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): technische informatie over de functie voor Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

