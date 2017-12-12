---
title: Aanmelden automatisch-versnelling voor een toepassing met behulp van een Thuisrealmdetectie-beleid configureren | Microsoft Docs
description: Wordt uitgelegd wat een Azure AD-tenant is en hoe u Azure beheert via Azure Active Directory.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: e2e6e5c40dc4a9f67f94c45f8394512db3f777f5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-by-using-a-home-realm-discovery-policy"></a>Aanmelden automatisch-versnelling voor een toepassing met een Thuisrealmdetectie-beleid configureren

Het volgende document bevat een inleiding tot Thuisrealmdetectie en auto-versnelling.

## <a name="home-realm-discovery"></a>Detectie van thuisrealm
Thuis Realm detectie (HRD) is het proces waarmee Azure Active Directory (Azure AD) om te bepalen, op tijd van aanmelden, waarbij een gebruiker moet worden geverifieerd.  Wanneer een gebruiker zich aanmeldt met een Azure AD-tenant voor toegang tot een bron of naar de Azure AD algemene aanmeldingspagina, typt u de naam van een gebruiker (UPN). Azure AD gebruikt om te ontdekken waar de gebruiker moet aan te melden. 

De gebruiker moet mogelijk worden uitgevoerd om een van de volgende locaties moet worden geverifieerd:

- De oorspronkelijke tenant van de gebruiker (mogelijk dezelfde tenant als de resource die de gebruiker toegang probeert te krijgen zijn). 

- Microsoft-account.  De gebruiker is een gast in de resource-tenant.

- Een andere id-provider die gefedereerd met Azure AD-tenant.

-  Een id-provider voor het lokale zoals Active Directory Federation Services (AD FS).

## <a name="auto-acceleration"></a>Automatische-versnelling 
Sommige organisaties configureren hun Azure Active Directory-tenant wilt federeren met een andere IdP, zoals AD FS voor verificatie van gebruikers.  

In dergelijke gevallen wanneer een gebruiker zich in een toepassing ze eerst eerst krijgt een Azure AD-aanmeldingspagina. Nadat ze hun UPN hebt getypt, worden ze vervolgens naar de aanmeldingspagina IdP geleid. Onder bepaalde omstandigheden beheerders mogelijk willen gebruikers verwijzen naar de aanmeldingspagina wanneer ze zijn aangemeld bij specifieke toepassingen. 

Dit betekent dat gebruikers de eerste pagina van de Azure Active Directory kunnen overslaan. Dit proces wordt aangeduid als "aanmelden automatisch-versnelling."

In gevallen waar de tenant naar een andere IdP voor aanmelding is gefedereerd, maakt automatisch versnelling gebruiker aanmelden sneller.  U kunt automatische-versnelling voor afzonderlijke toepassingen kunt configureren.

>[!NOTE]
>Als u een toepassing voor automatische versnelling configureert, aanmelden gastgebruikers niet. Als u een gebruiker direct door naar een federatieve IdP voor verificatie, is er geen manier om ze terugkeren naar de aanmeldingspagina van Azure Active Directory. Gastgebruikers die worden omgeleid naar de andere tenants of een externe IdP zoals een Microsoft-account moeten mogelijk, kunnen niet aanmelden bij die toepassing omdat ze bij het overslaan van de stap Thuisrealmdetectie.  

Er zijn twee manieren om te bepalen voor een federatieve IdP-auto-versnelling:   

- Gebruik een geheugensteun voor het domein op verificatieaanvragen voor een toepassing. 
- Configureer een beleid Thuisrealmdetectie als auto-versnelling wilt inschakelen.

## <a name="domain-hints"></a>Domein-hints 
Domein-hints zijn richtlijnen die zijn opgenomen in de verificatieaanvraag van een toepassing. Ze kunnen worden gebruikt voor de gebruiker om hun federatieve IdP-aanmeldingspagina te versnellen. Of ze kunnen worden gebruikt door een toepassing met meerdere tenants te versnellen van de gebruiker rechtstreeks naar de huisstijl Azure AD-aanmeldingspagina voor hun tenant.  

Bijvoorbeeld, de toepassing 'largeapp.com' mogelijk hun klanten toegang krijgen tot de toepassing op een aangepaste URL 'contoso.largeapp.com'. De app kan ook een hint domein contoso.com in de verificatieaanvraag omvatten. 

De syntaxis van de domein-hint varieert, afhankelijk van het protocol dat wordt gebruikt en dit standaard geconfigureerd in de toepassing.

**WS-Federation**: whr=contoso.com in de queryreeks.

**SAML**: ofwel een SAML-verificatieaanvraag met een geheugensteun voor het domein of een whr=contoso.com query-tekenreeks.

**Open ID Connect**: een domain_hint=contoso.com query-tekenreeks. 

Als een geheugensteun voor het domein is opgenomen in de verificatieaanvraag van de toepassing en de tenant is gefedereerd met dat domein, probeert Azure AD aanmelden omgeleid naar de IdP die geconfigureerd voor het domein. 

Als de domein-hint niet naar een geverifieerd federatieve domein verwijst, wordt dit genegeerd en normale Thuisrealmdetectie wordt aangeroepen.

Zie voor meer informatie over automatische-versnelling met behulp van de domein-hints die worden ondersteund door Azure Active Directory, de [Enterprise Mobility + Security-blog](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/).

>[!NOTE]
>Als een geheugensteun voor het domein is opgenomen in een verificatieaanvraag, overschrijft de aanwezigheid HRD beleid dat is ingesteld voor de toepassing.

## <a name="home-realm-discovery-policy"></a>Thuis Realm detectie-beleid
Sommige toepassingen beschikken niet over een manier voor het configureren van de authenticatie-aanvraag die ze verzenden. In dergelijke gevallen is het niet mogelijk te domein hints gebruiken automatisch versnelling beheren. Automatische versnelling kan worden geconfigureerd via beleid voor hetzelfde gedrag.  

### <a name="set-hrd-policy"></a>HRD-beleid instellen
Er zijn drie stappen voor het aanmelden automatisch-versnelling instellen op een toepassing:


1. Maken van een HRD-beleid voor automatische versnelling.

2. Zoeken naar het principe van de service waarop het beleid te koppelen.

3. Het koppelen van het beleid aan het principe van de service. Beleid kunnen worden gemaakt in een tenant, maar hebben geen effect totdat deze zijn gekoppeld aan een entiteit. 

Een HRD-beleid kan worden gekoppeld aan een service-principal en slechts één HRD beleid kan op elk gewenst moment actief op een bepaalde entiteit zijn.  

U kunt de Microsoft Azure Active Directory Graph API rechtstreeks of Azure Active Directory PowerShell-cmdlets gebruiken voor het instellen van automatische versnelling met behulp van HRD beleid.

De Graph-API die beleid bewerkt, wordt beschreven in de [bewerkingen op beleid](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) artikel op MSDN.

Hieronder volgt een voorbeeld van de beleidsdefinitie HRD:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu"
    }
   }
```

Het type beleid dat is 'HomeRealmDiscoveryPolicy'.

Als **AccelerateToFederatedDomain** is ingesteld op false, het beleid heeft geen effect.

**PreferredDomain** zou moeten aangeven waarom een domein waarnaar u wilt versnellen. Het kan worden overgeslagen als de tenant slechts één federatieve domein heeft.  Als dit wordt weggelaten en er is meer dan één federatieve domein geverifieerd, heeft het beleid geen effect.

Als **PreferredDomain** is opgegeven, moet overeenkomen met een geverifieerde, federatieve domein voor de tenant. Alle gebruikers van de toepassing moeten kunnen aanmelden met dat domein.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Prioriteit en beoordeling van beleid HRD
HRD-beleid kunnen worden gemaakt en vervolgens toegewezen aan specifieke organisaties en service-principals. Dit betekent dat het mogelijk voor meerdere beleidsregels toepassen op een bepaalde toepassing is. Het HRD-beleid dat van kracht volgt deze regels:


- Als een geheugensteun voor het domein in de verificatieaanvraag aanwezig is, wordt eventueel beleid HRD genegeerd. Het gedrag op dat opgegeven door de domein-hint wordt gebruikt.

- Anders als een beleid expliciet aan de service-principal is toegewezen, dat deze wordt afgedwongen. 

- Als er geen geheugensteun voor het domein, en er geen beleid expliciet is toegewezen aan de service-principal, wordt een beleid dat expliciet toegewezen aan de bovenliggende organisatie van de service-principal afgedwongen. 

- Als er geen geheugensteun voor het domein, en er geen beleid is toegewezen aan de service-principal of de organisatie, wordt het standaardgedrag HRD gebruikt.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-by-using-an-hrd-policy"></a>Zelfstudie voor het aanmelden automatisch-versnelling instellen op een toepassing met behulp van een HRD-beleid
We Azure AD PowerShell-cmdlets gebruiken om enkele scenario's, waaronder doorlopen:


- Instellen van automatische-versnelling voor een toepassing voor een tenant met één federatieve domein.

- Instellen van automatische-versnelling voor een toepassing op een van de verschillende domeinen die zijn geverifieerd voor uw tenant.

- Lijst met de toepassingen waarvoor een beleid is geconfigureerd.

### <a name="prerequisites"></a>Vereisten
In de volgende voorbeelden u maken, bijwerken, koppelen en verwijderen van beleidsregels op de service-principals toepassing in Azure AD.

1.  Download de meest recente preview van Azure AD PowerShell-cmdlet om te beginnen. 

2.  Nadat u de Azure AD PowerShell-cmdlets hebt gedownload, kunt u de opdracht Connect aan te melden bij Azure AD met uw beheerdersaccount uitvoeren:

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Voer de volgende opdracht om te zien van het beleid in uw organisatie:

    ``` powershell
    Get-AzureADPolicy
    ```

Als er niets wordt geretourneerd, betekent dat u hebt geen beleidsregels die zijn gemaakt in uw tenant.

### <a name="example-set-auto-acceleration-for-an-application"></a>Voorbeeld: Stel automatisch-versnelling voor een toepassing 
In dit voorbeeld maakt u een beleid dat automatische-versneld gebruikers aan een AD FS in het scherm wanneer ze voor een toepassing aanmelden zich. Gebruikers kunnen aanmelden bij AD FS zonder eerst een gebruikersnaam invoeren op de aanmeldingspagina van Azure AD. 

#### <a name="step-1-create-an-hrd-policy"></a>Stap 1: Een HRD-beleid maken
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Als u een één federatieve domein dat verificatie van gebruikers voor toepassingen hebt, moet u slechts één HRD-beleid maken.  

Als u wilt zien van het nieuwe beleid en de **ObjectID**, voer de volgende opdracht:

``` powershell
Get-AzureADPolicy
```


Automatische-als versnelling wilt inschakelen nadat u een beleid HRD hebt, kunt u deze toewijzen aan meerdere service principes van de toepassing.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>Stap 2: Zoek de service-principal waarop het beleid toewijzen  
U moet de **ObjectID** van de service-principals waarnaar u wilt toewijzen van het beleid. Er zijn verschillende manieren zoeken naar de **ObjectID** van service-principals.    

U kunt de portal of u kunt een query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). U kunt ook gaan naar de [grafiek Explorer Tool](https://graphexplorer.cloudapp.net/) en meld u aan bij uw Azure AD-account om te zien van de service-principals van uw organisatie. Omdat u met behulp van PowerShell, kunt u de cmdlet get-AzureADServicePrincipal voor een lijst met de principes van de service en de id's gebruiken.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Stap 3: Het beleid aan uw service-principal toewijzen  
Nadat u hebt de **ObjectID** van de service-principal van de toepassing waarvoor u voor het configureren van automatische-versnelling wilt de volgende opdracht uitvoeren. Met deze opdracht wordt gekoppeld aan het beleid HRD die u in stap 1 hebt gemaakt met de service-principal die u in stap 2.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

U kunt deze opdracht herhalen voor elke service-principal waarnaar u wilt toevoegen van het beleid.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>Stap 4: Controleren welke toepassing service-principals uw automatisch versnelling beleid is toegewezen aan
Om te controleren welke toepassingen automatisch versnelling beleid geconfigureerd hebt, gebruikt u de **Get-AzureADPolicyAppliedObject** cmdlet. Geef dit door de **ObjectID** van het beleid dat u controleren wilt op.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Stap 5: U bent nu klaar!
Probeer de toepassing om te controleren of het nieuwe beleid werkt.

### <a name="example-list-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Voorbeeld: Overzicht van de toepassingen waarvoor een beleid voor automatische versnelling is geconfigureerd

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>Stap 1: Lijst met alle beleidsregels die zijn gemaakt in uw organisatie 

``` powershell
Get-AzureADPolicy
```

Opmerking de **ObjectID** van het beleid dat u wilt de toewijzingen van de lijst voor.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>Stap 2: Overzicht van de service-principals waarop het beleid is toegewezen  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-auto-acceleration-policy-for-an-application"></a>Voorbeeld: Een beleid voor automatische versnelling voor een toepassing verwijderen
#### <a name="step-1-get-the-objectid"></a>Stap 1: Haal de object-id
Gebruik van het vorige voorbeeld om de **ObjectID** van het beleid en die van de toepassingsservice-principal van waaruit u wilt verwijderen. 

#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Stap 2: De beleidstoewijzing verwijderen uit de service-principal van toepassing  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-to-which-the-policy-is-assigned"></a>Stap 3: Selectievakje verwijderen door de service-principals waarop het beleid is toegewezen aan te bieden 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de werking van verificatie in Azure AD [verificatie scenario's voor Azure AD](develop/active-directory-authentication-scenarios.md).
- Zie voor meer informatie over eenmalige aanmelding gebruiker [toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).
- Ga naar de [ontwikkelaarshandleiding Active Directory](develop/active-directory-developers-guide.md) voor een overzicht van alle inhoud die relevant zijn voor ontwikkelaars.
