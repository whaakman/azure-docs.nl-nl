---
title: Azure AD Pass-through-verificatie - snel aan de slag | Microsoft Docs
description: Dit artikel wordt beschreven hoe u aan de slag met Azure Active Directory (Azure AD) Pass through-verificatie.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: cbedb87722d1c230f3b8003cadd069947881f25d
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory Pass-through-verificatie: Snel starten

## <a name="how-to-deploy-azure-ad-pass-through-authentication"></a>Azure AD Pass-through-verificatie implementeren

Azure Active Directory (Azure AD)-Pass through-verificatie kunnen uw gebruikers zich aanmelden bij zowel on-premises en cloudtoepassingen met dezelfde wachtwoorden. Deze zich gebruikers aanmeldt door het valideren van hun wachtwoorden rechtstreeks op uw lokale Active Directory.

>[!IMPORTANT]
>Als u deze functie via preview gebruikt hebt, moet u ervoor zorgen dat u de preview-versies van de verificatie-Agents upgraden met behulp van de instructies [hier](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

U moet de volgende instructies voor het implementeren van Pass through-verificatie:

## <a name="step-1-check-prerequisites"></a>Stap 1: Controle van vereisten

Zorg ervoor dat de volgende vereisten voldaan is:

### <a name="on-the-azure-active-directory-admin-center"></a>Op het Azure Active Directory-beheercentrum

1. Maak een account van de globale beheerder alleen in de cloud op uw Azure AD-tenant. Op deze manier kunt u de configuratie van uw tenant beheren moeten uw on-premises-services mislukt of niet meer beschikbaar. Meer informatie over [toevoegen van een cloudconfiguratie globale beheerdersaccount](../active-directory-users-create-azure-portal.md). Tijdens het doorzoeken van deze stap is het belangrijk om ervoor te zorgen dat u geen toegang buiten uw tenant.
2. Voeg een of meer [aangepast domein namen](../active-directory-add-domain.md) naar uw Azure AD-tenant. Uw gebruikers zich aanmelden met een van deze domeinnamen.

### <a name="in-your-on-premises-environment"></a>In uw on-premises-omgeving

1. Zoek een server met Windows Server 2012 R2 of hoger waarop Azure AD Connect uitgevoerd. De server toevoegen aan hetzelfde AD-forest als de gebruikers waarvan de wachtwoorden moeten worden gevalideerd.
2. Installeer de [meest recente versie van Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) op de server die wordt geïdentificeerd in de vorige stap. Als u al Azure AD Connect uitgevoerd, ervoor zorgen dat de versie 1.1.644.0 of hoger.

    >[!NOTE]
    >Azure AD Connect-versies 1.1.557.0, 1.1.558.0, 1.1.561.0 en 1.1.614.0 hebben een probleem is gerelateerd aan **synchronisatie van wachtwoordhash**. Als u _niet_ wilt synchronisatie van wachtwoordhash gebruiken in combinatie met Pass through-verificatie, lees de [opmerkingen bij de release van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) voor meer informatie.

3. Identificeren van een extra server met Windows Server 2012 R2 of hoger waarop een zelfstandige verificatie-Agent wordt uitgevoerd. De verificatie-Agent-versie moet worden 1.5.193.0 of hoger. Deze server is nodig voor hoge beschikbaarheid van aanmeldingsaanvragen. De server toevoegen aan hetzelfde AD-forest als de gebruikers waarvan de wachtwoorden moeten worden gevalideerd.
4. Als er een firewall tussen uw servers en Azure AD, moet u de volgende items configureren:
   - Zorg ervoor dat de verificatie-Agents kunnen leveren **uitgaande** aanvragen naar Azure AD via de volgende poorten:
   
   | Poortnummer | Hoe deze wordt gebruikt |
   | --- | --- |
   | **80** | Downloaden van certificaatintrekking (CRL's) worden tijdens het valideren van het SSL-certificaat |
   | **443** | Alle uitgaande communicatie met onze service |
   
   Als uw firewall afdwingt regels op basis van de herkomst van gebruikers, opent u deze poorten voor verkeer van de Windows-services die worden uitgevoerd als een netwerkservice.
   - Als uw firewall of proxyserver kan DNS-whitelisting, geaccepteerde verbindingen met  **\*. msappproxy.net** en  **\*. servicebus.windows.net**. Als dit niet het geval is, zodat u toegang hebben tot [Azure DataCenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653), die wekelijks worden bijgewerkt.
   - De verificatie-Agents moet toegang hebben tot **login.windows.net** en **login.microsoftonline.com** voor initiële registratie, zodat uw firewall openen voor deze URL's ook.
   - Deblokkeren om certificaten te valideren, de volgende URL's: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** en **www.microsoft.com:80**. Deze URL's worden gebruikt voor validatie van het servercertificaat met andere Microsoft-producten, dus u al deze URL's gedeblokkeerd hebt mogelijk.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Stap 2: (Optioneel) Exchange ActiveSync-ondersteuning inschakelen

Volg deze instructies voor het Exchange ActiveSync-ondersteuning inschakelen:

1. Gebruik [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) de volgende opdracht uitvoeren:
```
Get-OrganizationConfig | fl per*
```

2. Controleer de waarde van de `PerTenantSwitchToESTSEnabled` instelling. Als de waarde **true**, uw tenant is juist geconfigureerd - doorgaans is dit het geval is voor de meeste klanten. Als de waarde **false**, voer de volgende opdracht:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Controleer de waarde van de `PerTenantSwitchToESTSEnabled` is nu ingesteld op **true**. Wacht een uur voordat u doorgaat met de volgende stap.

Als u problemen mee te tijdens deze stap maken, controleert u onze [probleemoplossingsgids](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) voor meer informatie.

## <a name="step-3-enable-the-feature"></a>Stap 3: De functie inschakelen

Pass through-verificatie kan worden ingeschakeld met [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Pass through-verificatie kan worden ingeschakeld op de primaire of staging-server van Azure AD Connect. Het is raadzaam dat u het inschakelen van de primaire server.

Als u Azure AD Connect voor het eerst installeert, kiest u de [aangepaste installatiepad](active-directory-aadconnect-get-started-custom.md). Op de **gebruikersaanmelding** pagina **Pass through-verificatie** als de aanmelding methode. Is gelukt, wordt een Pass through-verificatie-agent geïnstalleerd op dezelfde server als Azure AD Connect. Bovendien is de functie Pass through-verificatie ingeschakeld op uw tenant.

![Azure AD Connect - gebruiker aanmelden](./media/active-directory-aadconnect-sso/sso3.png)

Als u Azure AD Connect al hebt geïnstalleerd (met behulp van de [snelle installatie](active-directory-aadconnect-get-started-express.md) of de [aangepaste installatie](active-directory-aadconnect-get-started-custom.md) pad), selecteer **wijzigen gebruikersaanmelding** taak op Azure AD Verbinding maken en klikt u op **volgende**. Selecteer vervolgens **Pass through-verificatie** als de aanmelding methode. Een Pass through-verificatie-agent is geïnstalleerd op dezelfde server als Azure AD Connect is gelukt, en de functie is ingeschakeld op uw tenant.

![Azure AD Connect - wijziging gebruiker aanmelden](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Pass through-verificatie is een functie op tenantniveau. Het inschakelen van invloed is op aanmeldingspagina voor gebruikers in _alle_ beheerde domeinen in uw tenant. Als u van de AD FS op Pass through-verificatie overschakelt, raden wij aan dat u ten minste 12 uur voordat het afsluiten van uw AD FS-infrastructuur wacht - wachttijd is om ervoor te zorgen dat gebruikers kunnen houden aanmelding bij Exchange ActiveSync tijdens de overgang.

## <a name="step-4-test-the-feature"></a>Stap 4: De functie testen

Volg deze instructies om te controleren of Pass through-verificatie correct ingeschakeld:

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met de referenties van de globale beheerder voor uw tenant.
2. Selecteer **Azure Active Directory** op de linkernavigatiebalk.
3. Selecteer **Azure AD Connect**.
4. Controleer de **Pass through-verificatie** functie wordt weergegeven als **ingeschakeld**.
5. Selecteer **Pass through-verificatie**. Deze blade worden de servers waarop de verificatie-Agents zijn geïnstalleerd.

![Azure Active Directory-beheercentrum - blade van Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory-beheercentrum - blade Pass through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

In deze fase, kunnen gebruikers van alle beheerde domeinen in uw tenant zich aanmelden bij het gebruik van Pass through-verificatie. Gebruikers van federatieve domeinen blijven echter aan te melden met behulp van Active Directory Federation Services (AD FS) of een andere federatieprovider die u eerder hebt geconfigureerd. Als u een domein via federatieve converteert beheerd, start alle gebruikers van dat domein automatisch aanmelden met behulp van Pass through-verificatie. Alleen in de cloud gebruikers worden niet beïnvloed door de functie Pass through-verificatie.

## <a name="step-5-ensure-high-availability"></a>Stap 5: Hoge beschikbaarheid garanderen

Als u van plan bent een Pass through-verificatie implementeren in een productieomgeving, moet u een zelfstandige verificatie-Agent installeren. Deze tweede verificatie-Agent installeren op een server _andere_ dan de één actief Azure AD Connect en de eerste verificatie-Agent. Deze instelling biedt u hoge beschikbaarheid van aanmeldingsaanvragen. Volg deze instructies voor het implementeren van een zelfstandige verificatie-Agent:

1. **Download de nieuwste versie van de verificatie-Agent (versies 1.5.193.0 of hoger)**: aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met uw globale tenantbeheerderreferenties.
2. Selecteer **Azure Active Directory** op de linkernavigatiebalk.
3. Selecteer **Azure AD Connect** en vervolgens **Pass through-verificatie**. En selecteer **agent downloaden**.
4. Klik op de **voorwaarden accepteren & downloaden** knop.
5. **Installeer de nieuwste versie van de Agent verificatie**: uitvoeren van het uitvoerbare bestand in de vorige stap hebt gedownload. Geef uw tenant hoofdbeheerder referenties wanneer u wordt gevraagd.

![Azure Active Directory-beheercentrum - knop downloaden van de verificatie-Agent](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory-beheercentrum - blade Agent downloaden](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>U kunt ook de verificatie-Agent van downloaden [hier](https://aka.ms/getauthagent). Zorg ervoor dat u lees en accepteer de verificatie-Agent [servicevoorwaarden](https://aka.ms/authagenteula) _voordat_ te installeren.

## <a name="next-steps"></a>Volgende stappen
- [**Vergrendelen van smartcard** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [**Huidige beperkingen** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -welke scenario's worden ondersteund en welke worden niet meer.
- [**Technische diepgaand** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -begrijpen hoe deze functie werkt.
- [**Veelgestelde vragen** ](active-directory-aadconnect-pass-through-authentication-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**Beveiliging diepgaand** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -extra uitgebreide technische informatie over de functie.
- [**Azure AD naadloze eenmalige aanmelding** ](active-directory-aadconnect-sso.md) -meer informatie over deze aanvullende functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
