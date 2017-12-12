---
title: Het oplossen van hybride Azure Active Directory die lid zijn van apparaten met Windows 10 en Windows Server 2016 | Microsoft Docs
description: Apparaten met Windows 10 en Windows Server 2016 probleemoplossing hybride Azure Active Directory worden toegevoegd.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3b98d31efcdbd61cf12e2c905f200c1e54f68f69
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a>Het oplossen van hybride Azure Active Directory die lid zijn van Windows 10 en Windows Server 2016-apparaten 

In dit onderwerp is van toepassing op de volgende clients:

-   Windows 10
-   Windows Server 2016

Zie voor andere Windows-clients [probleemoplossing hybride Azure Active Directory die lid zijn van de downlevel-apparaten](device-management-troubleshoot-hybrid-join-windows-legacy.md).

Dit onderwerp wordt ervan uitgegaan dat u hebt [geconfigureerde hybride Azure Active Directory die lid zijn van de apparaten](device-management-hybrid-azuread-joined-devices-setup.md) ter ondersteuning van de volgende scenario's:

- Voorwaardelijke toegang op basis van apparaten

- [Enterprise-instellingen voor roaming](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello voor Bedrijven](active-directory-azureadjoin-passport-deployment.md)


Dit document bevat richtlijnen voor probleemoplossing over de mogelijke problemen kunt oplossen. 


Voor Windows 10 en Windows Server 2016 hybride Azure Active Directory join biedt ondersteuning voor de Windows Update van 10 November 2015 en hoger. U wordt aangeraden met behulp van de verjaardag-update.

## <a name="step-1-retrieve-the-join-status"></a>Stap 1: De join-status niet ophalen 

**Voor het ophalen van de join-status:**

1. Open de opdrachtprompt als beheerder

2. Type **dsregcmd/status**



    +----------------------------------------------------------------------+
   | Apparaatstatus |+----------------------------------------------------------------------+
    
        AzureAdJoined: YES
     EnterpriseJoined: Geen apparaat-id: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 vingerafdruk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto-Provider TpmProtected: Ja KeySignTest:: moet worden uitgevoerd met verhoogde bevoegdheden te testen.
                  IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ja DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
   | Gebruikersstatus |+----------------------------------------------------------------------+
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    WamDefaultAuthority: organisaties WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Ja



## <a name="step-2-evaluate-the-join-status"></a>Stap 2: De join-status evalueren 

Bekijk de volgende velden en zorg ervoor dat ze de verwachte waarden hebben:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Ja  

Dit veld geeft aan of het apparaat is verbonden met Azure AD. Als de waarde **Nee**, de join naar Azure AD is nog niet voltooid. 

**Mogelijke oorzaken:**

- Verificatie van de computer voor een join is mislukt.

- Er is een HTTP-proxy in de organisatie die door de computer kan niet worden gedetecteerd

- De computer kan niet worden aangemeld bij Azure AD om te verifiëren of Azure DRS voor registratie

- De computer mogelijk niet op het interne netwerk van de organisatie of VPN-met direct zicht regel met een on-premises AD-domeincontroller.

- Als de computer een TPM heeft, kan het zijn in orde.

- Er zijn mogelijk een onjuiste configuratie van de services eerder hebt genoteerd in het document dat moet u opnieuw te verifiëren. Algemene voorbeelden zijn:

    - De federatieserver is geen WS-Trust-eindpunten die zijn ingeschakeld

    - De federatieserver kan geen binnenkomende verificatie vanaf computers in uw netwerk met behulp van geïntegreerde Windows-verificatie.

    - Er is geen object van het Service Connection Point die verwijst naar de domeinnaam van uw geverifieerde in Azure AD in de AD-forest waar de computer deel uitmaakt

---

### <a name="domainjoined--yes"></a>DomainJoined: Ja  

Dit veld geeft aan of het apparaat is verbonden met een lokale Active Directory of niet. Als de waarde **Nee**, het apparaat een hybride Azure AD join kan niet worden uitgevoerd.  

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: Nee  

Dit veld wordt aangegeven of het apparaat is geregistreerd bij Azure AD als een persoonlijk apparaat (gemarkeerd als *Workplace Joined*). Deze waarde moet **Nee** voor een domein computer die ook hybride Azure AD die lid zijn. Als de waarde **Ja**, een werk- of schoolaccount-account is toegevoegd voordat de voltooiing van de hybride Azure AD join. Het account wordt in dit geval genegeerd bij gebruik van de verjaardag van een updateversie van Windows 10 (1607).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Ja en AzureADPrt: Ja
  
Deze velden wordt aangegeven of de gebruiker heeft geverifieerd naar Azure AD tijdens het aanmelden bij het apparaat. Als de waarden zijn **Nee**, kan worden voldaan:

- Ongeldige opslagsleutel (BEURS) in de TPM die zijn gekoppeld aan het apparaat na inschrijving (Controleer de KeySignTest tijdens de uitvoering van verhoogde).

- Alternatieve aanmeldings-ID

- HTTP-Proxy is niet gevonden

## <a name="next-steps"></a>Volgende stappen

Zie voor vragen over de [Apparaatbeheer Veelgestelde vragen](device-management-faq.md) 