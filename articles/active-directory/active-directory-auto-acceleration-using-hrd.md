---
title: Aanmelden automatisch-versnelling configureren voor een toepassing met behulp van Home Realm Discovery beleid | Microsoft Docs
description: Hierin wordt uitgelegd wat een Azure AD-tenant is en hoe u Azure beheert via Azure Active Directory
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: b1f0e5da09ef1d6acd234b72878cc71d66bb551e
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-using-home-realm-discovery-hrd-policy"></a>Aanmelden automatisch-versnelling voor een toepassing met behulp van thuis realm detectie (HRD)-beleid configureren

## <a name="introduction-to-home-realm-discovery-and-auto-acceleration"></a>Inleiding tot thuisrealmdetectie en auto-versnelling
Het volgende document bevat een inleiding tot thuisrealmdetectie en auto-versnelling.

### <a name="home-realm-discovery"></a>Detectie van thuisrealm
Thuis Realm detectie is het proces waarmee Azure Active Directory om te bepalen tijdens het aanmelden, waarbij een gebruiker moet worden geverifieerd.  Tijdens het aanmelden bij een Azure AD-tenant voor toegang tot een resource of de Azure AD algemene aanmeldingspagina, betekent dit dat de gebruiker een (user Principal name) typt.  Azure AD gebruikt om te ontdekken waar de gebruiker moet aanmelden.   De gebruiker moet mogelijk worden uitgevoerd om een van de volgende moet worden geverifieerd:

- De oorspronkelijke tenant van de gebruiker (mogelijk dezelfde tenant als de bron van de gebruiker toegang probeert te krijgen). 
- Microsoft-account.   De gebruiker is een gast in de resource-tenant
- Een andere id-provider gefedereerd met Azure AD-tenant.  Een on-premises-id-provider zoals AD FS voor exemplaar.

### <a name="auto-acceleration"></a>Automatische-versnelling 
Sommige organisaties configureren hun Azure Active Directory-tenant wilt federeren met een andere IdP, zoals AD FS voor verificatie.  In dergelijke gevallen tijdens de aanmelding in een toepassing, krijgt de gebruiker eerst met een Azure AD-aanmeldingspagina en wanneer ze hun UPN ze vervolgens naar de aanmeldingspagina IdP gaat hebt getypt.  In situaties waar het zinvol beheerders willen mogelijk gebruikers rechtstreeks naar de aanmeldingspagina geleid tijdens het aanmelden bij specifieke toepassingen voor het overslaan van de eerste pagina van de Azure Active Directory. Dit wordt ook wel 'aanmelden automatisch-versnelling' genoemd.

In gevallen waar de tenant naar een andere IdP voor aanmelding is gefedereerd, maakt automatisch versnelling inschakelen gebruiker aanmelden sneller in gevallen waarin u weet dat iedereen aanmelden kan worden geverifieerd door die IdP.  U kunt automatische-versnelling voor afzonderlijke toepassingen kunt configureren.

>[!NOTE]
>Als u een toepassing voor automatische versnelling configureert, gastgebruikers kunnen niet aanmelden. Maken van de gebruiker rechtstreeks naar een federatieve IdP voor verificatie is een eenzijdige straat er is geen manier om terug te gaan naar de aanmeldingspagina van Azure Active Directory.  Gastgebruikers die worden omgeleid naar de andere tenants of een externe IdP zoals Microsoft-account moet mogelijk moet worden geverifieerd, niet kunnen aanmelden bij die toepassing, zoals de stap Thuisrealmdetectie wordt overgeslagen.  

Er zijn twee manieren om te bepalen voor een federatieve IdP-auto-versnelling.  Beide:   

- Gebruik een geheugensteun voor het domein op verificatieaanvragen voor een toepassing 
- Configureer een beleid HomeRealmDiscovery als auto-versnelling wilt inschakelen

## <a name="domain-hints"></a>Domein-hints 
Domein-hints zijn richtlijnen die zijn opgenomen in de verificatieaanvraag van een toepassing.  Ze kunnen worden gebruikt om te versnellen van de gebruiker naar hun federatieve IdP-aanmeldingspagina of ze kunnen worden gebruikt door een toepassing met meerdere tenants te versnellen van de gebruiker rechtstreeks naar de huisstijl Azure AD-aanmeldingspagina voor hun tenant.  Bijvoorbeeld, een toepassing largeapp.com mogelijk hun klanten toegang krijgen tot de toepassing op een aangepaste URL contoso.largeapp.com en bevat mogelijk een hint domein Contoso.com in de verificatieaanvraag. De syntaxis van de domein-hint varieert, afhankelijk van het protocol dat wordt gebruikt en ze zijn doorgaans geconfigureerd in de toepassing.

**WS-Federation**: whr=contoso.com in de queryreeks

**SAML**: ofwel een SAML authenticatie-aanvraag met een geheugensteun voor het domein of een query-tekenreeks whr=contoso.com

**Open ID Connect**: een query-tekenreeks domain_hint=contoso.com 

Als een geheugensteun voor het domein is opgenomen in de verificatieaanvraag van de toepassing en de tenant is gefedereerd met dat domein, probeert Azure AD aanmelden omgeleid naar de IdP geconfigureerd voor het domein.  Als de domein-hint niet naar een geverifieerd federatieve domein verwijst, wordt dit genegeerd en normale thuisrealmdetectie wordt aangeroepen.

Zie dit [blogbericht](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/) voor meer informatie over automatische-versnelling met behulp van de domein-hints ondersteund door Azure Active Directory.

>[!NOTE]
>Als een geheugensteun voor het domein is opgenomen in een verificatieaanvraag overschrijft de aanwezigheid HRD beleid dat is ingesteld voor de toepassing.

## <a name="home-realm-discovery-hrd-policy"></a>Thuis realm detectie (HRD) beleid
Sommige toepassingen bieden een manier voor het configureren van de authenticatie-aanvraag die ze verzenden en in dergelijke gevallen is het niet mogelijk voor domein-hints gebruiken om te bepalen van automatische versnelling.   Automatische versnelling kan worden geconfigureerd via beleid voor hetzelfde gedrag.  

### <a name="setting-hrd-policy"></a>HRD beleid instellen
Er zijn drie stappen voor het aanmelden automatisch-versnelling instellen op een toepassing


1. Maken van een HRD-beleid voor automatische-versnelling
2. Zoeken naar het principe van de Service waarop het beleid te koppelen
3. Het koppelen van het beleid aan het principe van de service.  Beleidsregels zijn gemaakt in een tenant, maar hebben geen effect totdat deze zijn gekoppeld aan een entiteit.  Een HRD-beleid kan worden gekoppeld aan een Service-Principal en slechts één HRD beleid kan op elk gewenst moment actief op een bepaalde entiteit zijn.  

U kunt de Microsoft Azure Active Directory Graph API rechtstreeks of de Azure Active Directory PowerShell-Cmdlets gebruiken voor het instellen van automatische versnelling met behulp van HRD-beleid

De Graph-API die beleid bewerkt, wordt beschreven [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations).

Hier volgt een voorbeeld van de beleidsdefinitie HRD:
    
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

Als **AccelerateToFederatedDomain** is ingesteld op false en vervolgens het beleid geen effect heeft **PreferredDomain** zou moeten aangeven waarom een domein te versnellen en kunnen worden weggelaten als de tenant over één en slechts een federatieve een domein.  Als dit wordt weggelaten en er meer dan één gecontroleerd is, federatieve domein, is het beleid heeft geen effect.

Als **PreferredDomain** wordt opgegeven moet overeenkomen met een geverifieerde, federatieve domein voor de tenant en alle gebruikers van de desbetreffende toepassing moeten kunnen aanmelden bij dat domein.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Prioriteit en beoordeling van beleid HRD
HRD-beleid kunnen worden gemaakt en vervolgens toegewezen aan specifieke organisaties en service-principals. Dit betekent dat het mogelijk voor meerdere beleidsregels toepassen op een bepaalde toepassing is. Het HRD-beleid dat van kracht volgt deze regels:


- Als een geheugensteun voor het domein aanwezig zijn in de verificatieaanvraag is een HRD-beleid wordt genegeerd en het gedrag dat is opgegeven door de domein-hint wordt gebruikt.
- Anders als een beleid expliciet aan de service-principal is toegewezen, dat deze wordt afgedwongen. 
- Als er geen geheugensteun voor het domein en er geen beleid expliciet is toegewezen aan de service-principal, wordt een expliciet is toegewezen aan de bovenliggende organisatie van de service-principal-beleid afgedwongen. 
- Als er geen geheugensteun voor het domein, en er geen beleid is toegewezen aan de service-principal of de organisatie, wordt het standaardgedrag HRD gebruikt.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-using-hrd-policy-with-azure-active-directory-powershell-cmdlets"></a>Zelfstudie voor het aanmelden automatisch-versnelling instellen op een toepassing met behulp van HRD-beleid met Azure Active Directory PowerShell-cmdlets
We doorlopen enkele scenario's, waaronder:


- Automatische-versnelling voor een toepassing in te stellen voor een tenant met één federatieve domein
- Instellen van automatische-versnelling voor een toepassing op een van de verschillende domeinen die zijn geverifieerd voor uw tenant
- De toepassingen waarvoor een beleid is geconfigureerd weergeven

### <a name="prerequisites"></a>Vereisten
In de onderstaande voorbeelden u maken, bijwerken, koppelen en verwijderen van beleidsregels op de service-principals toepassing in Azure AD.

1.  Download de meest recente Preview van Azure AD PowerShell-Cmdlet om te beginnen. 
2.  Zodra u de Azure AD PowerShell-Cmdlets hebt, voert u de opdracht Connect zich aanmeldt bij Azure AD met uw beheerdersaccount.

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Voer de volgende opdracht om te zien van het beleid in uw organisatie.

    ``` powershell
    Get-AzureADPolicy
    ```

Als er niets wordt geretourneerd, hebt u geen beleid dat is gemaakt in uw tenant

### <a name="example-setting-auto-acceleration-for-an-application"></a>Voorbeeld: Instelling automatisch-versnelling voor een toepassing 
In dit voorbeeld maakt u een beleid dat automatische-gebruikers aan een AD FS in het scherm versnelt tijdens het aanmelden bij een toepassing.  Dit wordt gedaan zonder dat ze hoeven eerst invoeren van een gebruikersnaam op de aanmeldingspagina van Azure AD. 

#### <a name="step-1-create-an-hrd-policy"></a>Stap 1: Een HRD-beleid maken
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Als u een één federatieve domein dat verificatie van gebruikers voor toepassingen hebt, moet u slechts één HRD-beleid maken.  
Voer de volgende opdracht om het nieuwe beleid bekijken en krijgt u de object-id.

``` powershell
Get-AzureADPolicy
```


Zodra u een HRD-beleid hebt, zodat automatisch-versnelling, kunt u deze toewijzen aan meerdere service principes van de toepassing.

#### <a name="step-2-locate-the-service-principal-to-assign-the-policy-to"></a>Stap 2: De service-principal voor het toewijzen van het beleid te vinden.  
U moet de object-id van de service-principals die u wilt toewijzen van het beleid. Er zijn verschillende manieren zoeken naar de object-ID van de service-principals.    

U kunt de portal gebruiken, u kunt een query de [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) of Ga naar onze [grafiek Explorer Tool](https://graphexplorer.cloudapp.net/) en meld u aan bij uw Azure AD-account om te zien van de service-principals van uw organisatie, of omdat u gebruikt PowerShell, kunt u de cmdlet get-AzureADServicePrincipal voor een lijst met de principes van de service en de id's.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Stap 3: Het beleid aan uw service-principal toewijzen  
Zodra u de object-id van de service-principal van de toepassing die u wilt configureren, auto-versnelling voor hebt, voer de volgende opdracht om de HRD-beleid te koppelen die u in stap 1 hebt gemaakt met de service-principal die u in stap 2 hebt gevonden.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

U kunt deze opdracht voor elke Service-Principal toe te voegen van het beleid te herhalen.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>Stap 4: Controleren welke toepassing service-principals uw automatisch versnelling beleid is toegewezen aan
Om te controleren welke toepassingen hebben automatisch versnelling beleid geconfigureerd gebruikt u de cmdlet Get-AzureADPolicyAppliedObject en geef dit door de object-id van het beleid dat u controleren wilt op.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Stap 5: U bent nu klaar!
Probeer de toepassing om te controleren of het nieuwe beleid werkt.

### <a name="example-listing-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Voorbeeld: De toepassingen waarvoor een beleid voor automatische versnelling is geconfigureerd weergeven

#### <a name="step-1-list-all-policies-created-in-your-organization"></a>Stap 1: Lijst met alle beleidsregels die zijn gemaakt in uw organisatie 

``` powershell
Get-AzureADPolicy
```

Opmerking de **Object-ID** van het beleid dat u wilt de toewijzingen van de lijst voor.

#### <a name="step-2-list-the-service-principals-the-policy-is-assigned-to"></a>Stap 2: De service-principals die het beleid is toegewezen aan een lijst.  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-removing-an-auto-acceleration-policy-for-an-application"></a>Voorbeeld: Een beleid voor automatische versnelling voor een toepassing verwijderen
#### <a name="step-1-use-the-previous-example-to-get-the-objectid-of-the-policy-and-that-of-the-application-service-principal-you-wish-to-remove-it-from"></a>Stap 1: Het vorige voorbeeld gebruiken om op te halen van de object-id van het beleid en die van de service-principal voor toepassing die u wilt verwijderen uit
#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Stap 2: De beleidstoewijzing uit de service-principal van toepassing verwijderen.  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-the-policy-is-assigned-to"></a>Stap 3: Selectievakje verwijderen door de service-principals op te nemen het beleid is toegewezen aan 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de werking van verificatie in Azure AD [verificatie scenario's voor Azure AD](develop/active-directory-authentication-scenarios.md).
- Zie voor meer informatie over eenmalige aanmelding gebruiker [toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-enterprise-apps-manage-sso.md)
- Ga naar de [ontwikkelaarshandleiding Active Directory](develop/active-directory-developers-guide.md) voor een overzicht van alle inhoud die relevant zijn voor ontwikkelaars.
