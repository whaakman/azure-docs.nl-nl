---
title: Het oplossen van hybride Azure Active Directory gekoppelde Windows 10 en Windows Server 2016-apparaten | Microsoft Docs
description: Het oplossen van hybride Azure Active Directory gekoppelde Windows 10 en Windows Server 2016-apparaten.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: f9a32058bb9d9cb2f1fa2d04c8002f06fa80edeb
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54446099"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a>Het oplossen van hybride Azure Active Directory gekoppelde Windows 10 en Windows Server 2016-apparaten 

In dit artikel is van toepassing op de volgende clients:

-   Windows 10
-   Windows Server 2016

Zie voor andere Windows-clients, [probleemoplossing hybride Azure Active Directory gekoppelde downlevel-apparaten](troubleshoot-hybrid-join-windows-legacy.md).

In dit artikel wordt ervan uitgegaan dat u hebt [geconfigureerde hybride Azure Active Directory gekoppelde apparaten](hybrid-azuread-join-plan.md) ter ondersteuning van de volgende scenario's:

- Conditional access op basis van apparaten

- [Zakelijke roaming van instellingen](../active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello voor Bedrijven](../active-directory-azureadjoin-passport-deployment.md)


Dit document bevat richtlijnen voor probleemoplossing voor het oplossen van problemen. 


Voor Windows 10 en Windows Server 2016, hybride Azure Active Directory join biedt ondersteuning voor de Windows Update van 10 November 2015 en hoger. We raden u aan met behulp van de jubileumupdate.

## <a name="step-1-retrieve-the-join-status"></a>Stap 1: De join-status ophalen 

**De join-status ophalen:**

1. Open de opdrachtprompt als beheerder

2. Type **dsregcmd status**



    +----------------------------------------------------------------------+
    | Apparaatstatus |  +----------------------------------------------------------------------+
    
        AzureAdJoined: YES
     EnterpriseJoined: Er is geen apparaat-id: 5820fbe9-60c8-43B0-bb11-44aee233e4e7 vingerafdruk: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto Provider TpmProtected: Ja KeySignTest:: MOET worden uitgevoerd met verhoogde bevoegdheid om te testen.
                  Idp: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ja domeinnaam: CONTOSO
    
    +----------------------------------------------------------------------+
    | Gebruikersstatus |  +----------------------------------------------------------------------+
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    WamDefaultAuthority: organisaties WamDefaultId: https://login.microsoft.com       WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} AzureAdPrt (AzureAd): JA



## <a name="step-2-evaluate-the-join-status"></a>Stap 2: Evalueren van de join-status 

Bekijk de volgende velden en zorg ervoor dat ze de verwachte waarden hebben:

### <a name="azureadjoined--yes"></a>AzureAdJoined : JA  

Dit veld geeft aan of het apparaat is verbonden met Azure AD. Als de waarde **Nee**, het koppelen aan Azure AD join is nog niet voltooid. 

**Mogelijke oorzaken:**

- Verificatie van de computer voor een join is mislukt.

- Er is een HTTP-proxy in de organisatie die niet kan worden gedetecteerd door de computer

- De computer tijdelijk niet bereikbaar voor Azure AD om te verifiëren of Azure DRS voor registratie

- De computer mogelijk niet op het interne netwerk van de organisatie of VPN met directe verbinding met een on-premises AD-domeincontroller.

- Als de computer een TPM heeft, kan deze in orde zijn.

- Er is mogelijk een onjuiste configuratie van de services die eerder hebt genoteerd in het document dat moet u opnieuw te verifiëren. Enkele typische voorbeelden:

    - De federatieserver heeft geen WS-Trust-eindpunten ingeschakeld

    - De federatieserver is niet toegestaan voor binnenkomende verificatie vanaf computers in uw netwerk met behulp van geïntegreerde Windows-verificatie.

    - Er is geen Service Connection Point-object dat verwijst naar de naam van uw geverifieerde domeinnaam in Azure AD in de AD-forest waar de computer behoort

---

### <a name="domainjoined--yes"></a>DomainJoined: JA  

Dit veld geeft aan of het apparaat is toegevoegd aan een on-premises Active Directory of niet. Als de waarde **Nee**, een hybride Azure AD-join kan niet worden uitgevoerd door het apparaat.  

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: NO  

Dit veld geeft aan of het apparaat is geregistreerd bij Azure AD als een persoonlijk apparaat (gemarkeerd als *Workplace Joined*). Deze waarde moet **geen** voor een domein computer die is ook hybride Azure AD join. Als de waarde **Ja**, werk-of schoolaccount voordat de voltooiing van de hybride Azure AD join is toegevoegd. In dit geval is het account genegeerd bij het gebruik van de Jubileumupdate-versie van Windows 10 (1607).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Ja en AzureADPrt: JA
  
Deze velden wordt aangegeven of de gebruiker is geverifieerd met Azure AD tijdens het aanmelden op het apparaat. Als de waarden zijn **Nee**, kan worden voldaan:

- Ongeldige opslagsleutel (BEURS) in de TPM die zijn gekoppeld aan het apparaat bij de registratie (Raadpleeg de KeySignTest tijdens de uitvoering met verhoogde bevoegdheid).

- Alternatieve aanmeldings-ID

- HTTP-Proxy is niet gevonden

## <a name="next-steps"></a>Volgende stappen

Voor vragen, Zie de [Apparaatbeheer Veelgestelde vragen](faq.md) 