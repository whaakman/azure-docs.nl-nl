---
title: Pass through-verificatie Azure AD - snel starten | Microsoft Docs
description: In dit artikel wordt beschreven hoe u aan de slag met Azure Active Directory (Azure AD) Pass through-verificatie.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, installatie van Active Directory, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51fc93f9508bada40885e41b39e8a87cf4e0bf3c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58101003"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory Pass through-verificatie: Snel starten

## <a name="deploy-azure-ad-pass-through-authentication"></a>Azure AD Pass-through-verificatie implementeren

Azure Active Directory (Azure AD) Pass through-verificatie kunnen uw gebruikers zich aanmeldt bij zowel on-premises en cloudgebaseerde toepassingen met behulp van dezelfde wachtwoorden. Pass through-verificatie aanmeldt gebruikers door het valideren van hun wachtwoord direct op basis van on-premises Active Directory.

>[!IMPORTANT]
>Als u van AD FS (of andere technologieën voor federatie) naar Pass-through-verificatie migreert, is het raadzaam dat u onze gedetailleerde Implementatiehandleiding gepubliceerd volgt [hier](https://aka.ms/adfstoPTADPDownload).

Volg deze instructies voor het implementeren van Pass through-verificatie voor uw tenant:

## <a name="step-1-check-the-prerequisites"></a>Stap 1: Vereisten controleren

Zorg ervoor dat de volgende vereisten voldaan is.

### <a name="in-the-azure-active-directory-admin-center"></a>In het Azure Active Directory-beheercentrum

1. Een globale beheerder van alleen-cloud-account maken in uw Azure AD-tenant. Op deze manier kunt u de configuratie van uw tenant beheren moeten uw on-premises services mislukken of niet beschikbaar. Meer informatie over [toevoegen van een alleen-cloud globale beheerdersaccount](../active-directory-users-create-azure-portal.md). Het voltooien van deze stap is essentieel om ervoor te zorgen dat u geen toegang meer hebt tot uw tenant.
2. Voeg een of meer [aangepaste domeinnamen](../active-directory-domains-add-azure-portal.md) aan uw Azure AD-tenant. Uw gebruikers kunnen zich aanmelden met een van deze domeinnamen.

### <a name="in-your-on-premises-environment"></a>In uw on-premises-omgeving

1. Identificeer een server met Windows Server 2012 R2 of later om uit te voeren van Azure AD Connect. Als niet is ingeschakeld, [TLS 1.2 op de server inschakelen](./how-to-connect-install-prerequisites.md#enable-tls-12-for-azure-ad-connect). De server toevoegen aan hetzelfde Active Directory-forest als de gebruikers met wachtwoorden die u wilt valideren.
2. Installeer de [meest recente versie van Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) op de server die is geïdentificeerd in de vorige stap. Als u al Azure AD Connect wordt uitgevoerd hebt, controleert u of de versie 1.1.750.0 of hoger.

    >[!NOTE]
    >Azure AD Connect-versies 1.1.557.0, 1.1.558.0 1.1.561.0 en 1.1.614.0 is een probleem met betrekking tot wachtwoord-hashsynchronisatie. Als u _niet_ wilt wachtwoord-hashsynchronisatie gebruiken in combinatie met Pass through-verificatie, lees de [opmerkingen bij de release van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#116470).

3. Een of meer extra servers identificeren (met Windows Server 2012 R2 of hoger, met TLS 1.2 ingeschakeld) waar u de zelfstandige verificatie-Agents kunt uitvoeren. Deze extra servers er nodig zijn om te controleren of de hoge beschikbaarheid van aanvragen voor het aanmelden. De servers toevoegen aan hetzelfde Active Directory-forest als de gebruikers met wachtwoorden die u wilt valideren.

    >[!IMPORTANT]
    >In een productieomgeving, wordt aangeraden dat u hebt een minimum van 3 verificatie-Agents die worden uitgevoerd op uw tenant. Er is een limiet van 40 verificatie-Agents per tenant. En als best practice, behandelt u alle servers met verificatie-Agents als laag 0-systemen (Zie [verwijzing](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

4. Als er een firewall tussen uw servers en Azure AD, configureert u de volgende items:
   - Zorg ervoor dat de verificatie-Agents kunt aanbrengen *uitgaande* aanvragen voor Azure AD via de volgende poorten:

     | Poortnummer | Hoe dat wordt gebruikt |
     | --- | --- |
     | **80** | De certificaatintrekkingslijsten (CRL's) downloaden tijdens het valideren van het SSL-certificaat |
     | **443** | Alle uitgaande communicatie met de service worden verwerkt |
     | **8080** (optioneel) | Verificatie-Agents rapporteren hun status om de tien minuten via poort 8080, als poort 443 niet beschikbaar is. Deze status wordt weergegeven op de Azure AD-portal. Poort 8080 is _niet_ gebruikt voor gebruikersaanmeldingen. |
     
     Als uw firewall-regels op basis van de oorspronkelijke gebruikers afgedwongen, opent u deze poorten voor verkeer via Windows-services die worden uitgevoerd als een netwerkservice.
   - Als u uw firewall of proxyserver kan DNS-whitelist, lijst met toegestane adressen verbindingen met  **\*. msappproxy.net** en  **\*. servicebus.windows.net**. Als dit niet het geval is, kunt u toegang tot de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653), die elke week worden bijgewerkt.
   - De verificatie-Agents moeten toegang hebben tot **login.windows.net** en **login.microsoftonline.com** voor registratie. Open uw firewall voor ook deze URL's.
   - Blokkering voor validatie van het servercertificaat, de volgende URL's: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80**, en **www \.microsoft.com:80**. Aangezien deze URL's voor validatie van het servercertificaat met andere Microsoft-producten worden gebruikt mogelijk hebt u al deze URL's geblokkeerd.

## <a name="step-2-enable-the-feature"></a>Stap 2: De functie inschakelen

Inschakelen van Pass-through-verificatie via [Azure AD Connect](whatis-hybrid-identity.md).

>[!IMPORTANT]
>U kunt Pass through-verificatie inschakelen op de primaire of staging-server van Azure AD Connect. Het is raadzaam dat u het inschakelen van de primaire server. Als u een Azure AD Connect staging-server in de toekomst worden instellen u **moet** blijven Pass through-verificatie als de optie aanmelden kiezen; wordt een andere optie te kiezen **uitschakelen** Pass through-verificatie voor de tenant en overschrijven de instelling in de primaire server.

Als u Azure AD Connect voor het eerst installeert, kiest u de [aangepaste installatiepad](how-to-connect-install-custom.md). Op de **aanmelden van gebruikers** pagina, kies **Pass through-verificatie** als de **aanmelding methode**. Bewerking is voltooid, wordt een Pass through-verificatie-Agent geïnstalleerd op dezelfde server als Azure AD Connect. Bovendien is de functie voor Pass through-verificatie ingeschakeld op uw tenant.

![Azure AD Connect: Gebruikersaanmelding](./media/how-to-connect-pta-quick-start/sso3.png)

Als u Azure AD Connect al hebt geïnstalleerd met behulp van de [snelle installatie](how-to-connect-install-express.md) of de [aangepaste installatie](how-to-connect-install-custom.md) pad, selecteer de **aanmelden van gebruikers wijzigen** taak in Azure AD Verbinding maken en selecteer vervolgens **volgende**. Selecteer vervolgens **Pass through-verificatie** als de methode aanmelden. Een Pass through-verificatie-Agent is geïnstalleerd op dezelfde server als Azure AD Connect is gelukt, en de functie is ingeschakeld op uw tenant.

![Azure AD Connect: Gebruikersaanmelding wijzigen](./media/how-to-connect-pta-quick-start/changeusersignin.png)

>[!IMPORTANT]
>Pass through-verificatie is een functie op tenantniveau. Het inschakelen van is van invloed op de aanmelding voor gebruikers in _alle_ de beheerde domeinen in uw tenant. Als u van Active Directory Federation Services (AD FS) naar Pass-through-verificatie overstapt, moet u ten minste 12 uur vóór het afsluiten van de AD FS-infrastructuur wacht. Deze wachttijd is om ervoor te zorgen dat gebruikers houden bij Exchange ActiveSync tijdens de overgang aanmelden kunnen. Voor meer informatie over het migreren van AD FS naar Pass-through-verificatie, Bekijk onze gedetailleerde implementatieplan gepubliceerd [hier](https://aka.ms/adfstoptadpdownload).

## <a name="step-3-test-the-feature"></a>Stap 3: De functie testen

Volg deze instructies om te controleren of u Pass-through-verificatie juist hebt ingeschakeld:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties voor uw tenant.
2. Selecteer **Azure Active Directory** in het linkerdeelvenster.
3. Selecteer **Azure AD Connect**.
4. Controleer de **Pass through-verificatie** functie wordt weergegeven als **ingeschakeld**.
5. Selecteer **Pass through-verificatie**. De **Pass through-verificatie** deelvenster worden de servers waarop de verificatie-Agents zijn geïnstalleerd.

![Azure Active Directory-beheercentrum: Deelvenster in de Azure AD Connect](./media/how-to-connect-pta-quick-start/pta7.png)

![Azure Active Directory-beheercentrum: Deelvenster voor Pass through-verificatie](./media/how-to-connect-pta-quick-start/pta8.png)

In deze fase, kunnen gebruikers van de beheerde domeinen in uw tenant kunnen aanmelden met behulp van Pass through-verificatie. Gebruikers van federatieve domeinen blijven echter aan te melden met behulp van AD FS of een andere federatieprovider die u eerder hebt geconfigureerd. Als u een domein via federatieve converteert beheerd, start alle gebruikers van dat domein automatisch aanmelden met behulp van Pass through-verificatie. De functie voor Pass through-verificatie, heeft geen invloed op gebruikers alleen in de cloud.

## <a name="step-4-ensure-high-availability"></a>Stap 4: Zorg voor hoge beschikbaarheid

Als u van plan bent te Pass through-verificatie in een productieomgeving implementeert, moet u aanvullende zelfstandige verificatie-Agents installeren. Deze verificatie-agent (s) installeren op de server (s) _andere_ dan de één actief Azure AD Connect. Deze instelling biedt hoge beschikbaarheid voor gebruiker aanmeldingsaanvragen.

>[!IMPORTANT]
>In een productieomgeving, wordt aangeraden dat u hebt een minimum van 3 verificatie-Agents die worden uitgevoerd op uw tenant. Er is een limiet van 40 verificatie-Agents per tenant. En als best practice, behandelt u alle servers met verificatie-Agents als laag 0-systemen (Zie [verwijzing](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

Volg deze instructies om de verificatie-Agent-software te downloaden:

1. Voor het downloaden van de meest recente versie van de verificatie-Agent (versie 1.5.193.0 of hoger), zich aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de hoofdbeheerdersreferenties van uw tenant.
2. Selecteer **Azure Active Directory** in het linkerdeelvenster.
3. Selecteer **Azure AD Connect**, selecteer **Pass through-verificatie**, en selecteer vervolgens **-Agent downloaden**.
4. Selecteer de **voorwaarden accepteren en downloaden** knop.

![Azure Active Directory-beheercentrum: De knop verificatie-Agent downloaden](./media/how-to-connect-pta-quick-start/pta9.png)

![Azure Active Directory-beheercentrum: Deelvenster van de Agent downloaden](./media/how-to-connect-pta-quick-start/pta10.png)

>[!NOTE]
>U kunt ook rechtstreeks [downloaden van de verificatie-agentsoftware](https://aka.ms/getauthagent). Lees en accepteer de verificatie-Agent [servicevoorwaarden](https://aka.ms/authagenteula) _voordat_ installeren.

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
- [Migreren van AD FS naar Pass-through-verificatie](https://aka.ms/adfstoptadp) -een uitgebreide handleiding voor het migreren van AD FS (of andere technologieën voor federatie) naar Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): Informatie over het configureren van de functie Smart Lockout op uw tenant om te beveiligen van gebruikersaccounts.
- [Huidige beperkingen](how-to-connect-pta-current-limitations.md): Informatie over welke scenario's met de Pass-through-verificatie worden ondersteund en welke niet.
- [Technische details](how-to-connect-pta-how-it-works.md): Informatie over de werking van de functie voor Pass through-verificatie.
- [Veelgestelde vragen over](how-to-connect-pta-faq.md): Vind antwoorden op veelgestelde vragen.
- [Problemen oplossen](tshoot-connect-pass-through-authentication.md): Informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Grondig onderzoek van beveiliging](how-to-connect-pta-security-deep-dive.md): Technische informatie over de functie voor Pass through-verificatie ophalen.
- [Naadloze eenmalige aanmelding voor Azure AD](how-to-connect-sso.md): Meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): De Active Directory-Forum van Azure gebruiken om nieuwe functieaanvragen.
