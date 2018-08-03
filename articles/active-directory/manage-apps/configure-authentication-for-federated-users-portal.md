---
title: Aanmelden automatisch-versnelling voor een toepassing met behulp van een Thuisrealmdetectie-beleid configureren | Microsoft Docs
description: Wordt uitgelegd wat een Azure AD-tenant is en hoe u Azure beheert via Azure Active Directory.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/08/2018
ms.author: barbkess
ms.openlocfilehash: eccf570a0bdcf896a40d8c49f400647e247f6d65
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435774"
---
# <a name="configure-azure-active-directory-sign-in-behavior-for-an-application-by-using-a-home-realm-discovery-policy"></a>Azure Active Directory-aanmelding in gedrag voor een toepassing met behulp van een Thuisrealmdetectie-beleid configureren

Het volgende document bevat een inleiding tot Azure Active Directory-verificatiegedrag voor federatieve gebruikers configureren.   Het bevat informatie over de configuratie van automatische-versnelling en verificatie beperkingen voor gebruikers in federatieve domeinen.

## <a name="home-realm-discovery"></a>Detectie van thuisrealm
Thuis Realm detectie (HRD) is het proces waarmee Azure Active Directory (Azure AD) om te bepalen wanneer een gebruiker moet worden geverifieerd tijdens het aanmelden.  Wanneer een gebruiker zich aanmeldt met een Azure AD-tenant voor toegang tot een resource, of naar de Azure AD algemene aanmelden pagina, typt u de naam van een gebruiker (UPN). Azure AD gebruikt om te ontdekken waar de gebruiker moet zich aanmelden. 

De gebruiker moet mogelijk worden uitgevoerd op een van de volgende locaties worden geverifieerd:

- De starttenant van de gebruiker (mogelijk als de bron die de gebruiker probeert zich te krijgen tot dezelfde tenant is). 

- Microsoft-account.  De gebruiker is een gast in de resource-tenant.

-  Een on-premises id-provider, zoals Active Directory Federation Services (AD FS).

- Een andere id-provider die gefedereerd met de Azure AD-tenant.

## <a name="auto-acceleration"></a>Automatisch-versnelling 
Sommige organisaties configureren domeinen in hun tenant Azure Active Directory te federeren met een andere id-provider, zoals AD FS voor verificatie van de gebruiker.  

Wanneer een gebruiker zich in een toepassing, worden ze eerst met een Azure AD-aanmeldingspagina weergegeven. Nadat ze hun UPN, hebt getypt als ze zich in een federatief domein ze vervolgens gaat naar de aanmeldingspagina van de id-provider voor dat domein. Onder bepaalde omstandigheden, beheerders mogelijk willen gebruikers naar de aanmeldingspagina leiden wanneer ze voor specifieke toepassingen aanmeldt zich. 

Gebruikers, kan de eerste pagina van Azure Active Directory als gevolg hiervan kunnen overslaan. Dit proces wordt aangeduid als "aanmelden automatisch-versnelling."

In gevallen waar de tenant is gefedereerd naar een andere id-provider voor aanmelden, wordt auto-versnelling de gebruiker aanmelden sneller.  U kunt automatisch-versnelling voor afzonderlijke toepassingen kunt configureren.

>[!NOTE]
>Als u een toepassing voor automatische-versnelling configureren, aanmelden gastgebruikers ook kunnen niet. Als u een gebruiker rechtstreeks naar een federatieve id-provider voor verificatie, is er geen manier om deze terug te gaan naar de aanmeldingspagina van Azure Active Directory. Gastgebruikers die worden doorgestuurd naar andere tenants of een externe id-provider, zoals een Microsoft-account moeten mogelijk, kunnen niet aanmelden bij die toepassing omdat ze bij het overslaan van de stap start Realm detectie.  

Er zijn twee manieren voor het beheren van auto-versnelling voor een federatieve id-provider:   

- Gebruik een geheugensteun voor het domein op verificatieaanvragen voor een toepassing. 
- Configureer een beleid Thuisrealmdetectie voor het automatisch-versnelling inschakelen.

### <a name="domain-hints"></a>Domein-hints    
Domein-hints zijn richtlijnen die zijn opgenomen in de verificatieaanvraag van een toepassing. Ze kunnen worden gebruikt om te versnellen van de gebruiker naar de federatieve IdP-aanmelden pagina. Of ze kunnen worden gebruikt door een toepassing met meerdere tenants te versnellen van de gebruiker rechtstreeks naar de merknaam Azure AD-aanmeldingspagina voor hun tenant.  

Bijvoorbeeld, de toepassing 'largeapp.com' mogelijk hun klanten te krijgen tot de toepassing op een aangepaste URL "contoso.largeapp.com." De app ook advies inwinnen een geheugensteun voor het domein contoso.com in de verificatieaanvraag. 

De syntaxis van de domein-hint is afhankelijk van het protocol dat wordt gebruikt en deze standaard geconfigureerd in de toepassing.

**WS-Federation**: whr=contoso.com in de querytekenreeks.

**SAML**: hetzij een SAML-verificatie-aanvraag die een geheugensteun voor het domein of een whr=contoso.com query-tekenreeks bevat.

**Open ID Connect**: een domain_hint=contoso.com query-tekenreeks. 

Als een geheugensteun voor het domein is opgenomen in de verificatieaanvraag van de toepassing en de tenant is gefedereerd met het desbetreffende domein, probeert Azure AD aanmelden omleiden naar de id-provider die geconfigureerd voor het domein. 

Als de domeinhint niet naar een geverifieerde federatief domein verwijst, wordt dit genegeerd en normale start Realm detectie wordt aangeroepen.

Zie voor meer informatie over automatische-versnelling met behulp van de domein-hints die worden ondersteund door Azure Active Directory, de [Enterprise Mobility + Security-blog](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/).

>[!NOTE]
>Als een geheugensteun voor het domein is opgenomen in een verificatieaanvraag, overschrijft de aanwezigheid auto-versnelling die is ingesteld voor de toepassing in HRD-beleid.

### <a name="home-realm-discovery-policy-for-auto-acceleration"></a>Thuis Realm detectie-beleid voor automatische-versnelling
Sommige toepassingen bieden een manier om te configureren van de verificatieaanvraag die deze onderdelen introduceren. In dergelijke gevallen is het niet mogelijk om te domein-hints gebruiken voor het beheren van auto-versnelling. Automatisch-versnelling kan worden geconfigureerd via Groepsbeleid om hetzelfde gedrag.  

## <a name="enable-direct-authentication-for-legacy-applications"></a>Direct-verificatie voor oudere toepassingen inschakelen
Aanbevolen procedure is voor toepassingen kunnen gebruikmaken van AAD-bibliotheken en interactief aanmelden om gebruikers te verifiëren. De bibliotheken zorgen voor de stromen federatieve gebruiker.  Soms oudere toepassingen niet worden geschreven naar het federation begrijpen. Deze detectie van thuisrealm niet uitvoeren en werken niet met de juiste federatief eindpunt om een gebruiker te verifiëren. Als u ervoor kiest, kunt u HRD-beleid gebruiken om in te schakelen specifieke oudere toepassingen die de referenties van de gebruikersnaam en wachtwoord voor verificatie rechtstreeks met Azure Active Directory indienen. Synchronisatie van Wachtwoordhashes moet zijn ingeschakeld. 

> [!IMPORTANT]
> Directe verificatie alleen inschakelen als u synchronisatie van Wachtwoordhashes ingeschakeld hebt en u weet dit geen probleem dat voor het verifiëren van deze toepassing zonder een beleid dat is geïmplementeerd door uw on-premises id-provider. Als u synchronisatie van Wachtwoordhashes uitschakelen of Directory Synchronization met AD Connect voor een bepaalde reden uitschakelen, moet u dit beleid om te voorkomen dat de mogelijkheid om direct met behulp van een verlopen wachtwoord-hash-verificatie verwijderen.

## <a name="set-hrd-policy"></a>HRD-beleid instellen
Er zijn drie stappen voor het instelling HRD-beleid op een toepassing voor federatieve aanmelding automatisch-versnelling of direct cloud-gebaseerde toepassingen:

1. Maak een HRD-beleid.

2. Ga naar de service-principal waarop het beleid te koppelen.

3. Het beleid aan de service-principal koppelen. 

Beleid pas van kracht voor een bepaalde toepassing wanneer ze zijn gekoppeld aan een service-principal. 

Slechts één HRD-beleid kan op elk willekeurig moment actief zijn op een service-principal zijn.  

De Microsoft Azure Active Directory Graph API rechtstreeks of de Azure Active Directory PowerShell-cmdlets kunt u HRD-beleid maken en beheren.

De Graph-API die beleid bewerkt, wordt beschreven in de [bewerkingen op beleid](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) artikel op MSDN.

Hieronder volgt een voorbeeld van de beleidsdefinitie HRD:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu",
    "AllowCloudPasswordValidation":true
    }
   }
```

Het beleidstype is "HomeRealmDiscoveryPolicy."

**AccelerateToFederatedDomain** is optioneel. Als **AccelerateToFederatedDomain** is ingesteld op false, het beleid heeft geen invloed op auto-versnelling. Als **AccelerateToFederatedDomain** is true en er is slechts één geverifieerd en federatief domein in de tenant en vervolgens gebruikers gaat rechtstreeks naar de federatieve id-provider voor aanmelden. Als dit het geval is en er meer dan één geverifieerd domein in de tenant **PreferredDomain** moet worden opgegeven.

**PreferredDomain** is optioneel. **PreferredDomain** moet wijzen op een domein waarnaar u wilt versnellen. Het kan worden overgeslagen als de tenant slechts één federatief domein heeft.  Als deze wordt weggelaten, en er meer dan een federatief domein geverifieerd, wordt het beleid heeft geen effect.

 Als **PreferredDomain** is opgegeven, moet deze overeenkomen met een geverifieerde, federatieve domein voor de tenant. Alle gebruikers van de toepassing moet kunnen zich aanmelden bij dat domein.

**AllowCloudPasswordValidation** is optioneel. Als **AllowCloudPasswordValidation** is ingesteld op true en vervolgens de toepassing is toegestaan voor een federatieve gebruiker verifiëren met gebruikersnaam en wachtwoord referenties rechtstreeks naar het eindpunt van de Azure Active Directory-token wordt gepresenteerd. Dit werkt alleen als de synchronisatie van Wachtwoordhashes is ingeschakeld.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Prioriteit en beoordeling van HRD-beleid
HRD-beleid kunnen worden gemaakt en worden vervolgens toegewezen aan specifieke organisaties en service-principals. Dit betekent dat het is mogelijk voor meerdere beleidsregels toe te passen op een specifieke toepassing. Het HRD-beleid dat van kracht wordt met deze regels de volgende:


- Als een domeinhint in de verificatieaanvraag aanwezig is, wordt klikt u vervolgens een HRD-beleid genegeerd voor automatische-versnelling. Het gedrag op dat opgegeven door de geheugensteun voor het domein wordt gebruikt.

- Anders als een beleid expliciet is toegewezen aan de service-principal, dat deze wordt afgedwongen. 

- Als er geen geheugensteun voor het domein, en er geen beleid expliciet is toegewezen aan de service-principal, wordt een beleid dat expliciet toegewezen aan de bovenliggende organisatie van de service-principal wordt afgedwongen. 

- Als er geen geheugensteun voor het domein, en er geen beleid is toegewezen aan de service-principal of de organisatie, wordt het standaardgedrag van HRD gebruikt.

## <a name="tutorial-for-setting-hrd-policy-on-an-application"></a>Zelfstudie voor het instellen van HRD-beleid in een toepassing 
We gebruiken Azure AD PowerShell-cmdlets om te zien hoe een aantal scenario's, waaronder:


- Instellen van HRD-beleid te doen, auto-versnelling voor een toepassing in een tenant met een enkele federatief domein.

- Instellen van HRD-beleid voor het automatisch-versnelling voor een toepassing op een van verschillende domeinen die zijn geverifieerd doen voor uw tenant.

- HRD-beleid instellen om in te schakelen van een oudere toepassing te directe verificatie voor gebruikersnaam/wachtwoord aan Azure Active Directory voor een federatieve gebruiker.

- Lijst met de toepassingen waarvoor een beleid is geconfigureerd.


### <a name="prerequisites"></a>Vereisten
In de volgende voorbeelden u maken, bijwerken, koppelen en verwijderen van beleidsregels op de toepassingsservice-principals in Azure AD.

1.  Download de nieuwste preview voor Azure AD PowerShell-cmdlet om te beginnen. 

2.  Nadat u de Azure AD PowerShell-cmdlets hebt gedownload, kunt u de opdracht Connect zich aanmeldt bij Azure AD met uw beheerdersaccount uitvoeren:

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Voer de volgende opdracht uit om te zien van alle beleidsregels in uw organisatie:

    ``` powershell
    Get-AzureADPolicy
    ```

Als er niets wordt geretourneerd, betekent dat u hebt geen beleidsregels die zijn gemaakt in uw tenant.

### <a name="example-set-hrd-policy-for-an-application"></a>Voorbeeld: Stel HRD-beleid voor een toepassing 

In dit voorbeeld maken u een beleid dat als deze is toegewezen aan een toepassing een: 
- Automatisch versnelt-gebruikers aan een AD FS-aanmeldingsscherm wanneer ze bij een toepassing aanmeldt zich wanneer er een enkel domein in uw tenant. 
- Gebruikers automatisch versnelt aan een AD FS-aanmeldingsscherm er is meer dan één federatief domein in uw tenant.
- Hiermee kunt niet-interactieve gebruikersnaam en wachtwoord aanmelden bij Azure Active Directory voor federatieve gebruikers voor de toepassingen die het beleid is toegewezen aan rechtstreeks.

#### <a name="step-1-create-an-hrd-policy"></a>Stap 1: Een HRD-beleid maken

Het volgende beleid versnelt auto-gebruikers aan een AD FS-aanmeldingsscherm wanneer ze bij een toepassing aanmeldt zich wanneer er een enkel domein in uw tenant.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```
Het volgende beleid versnelt auto-gebruikers aan een AD FS-aanmeldingsscherm er is meer dan één federatieve domein in uw tenant. Hebt u meer dan één federatief domein waarmee gebruikers voor toepassingen worden geverifieerd, moet u het domein dat moet automatisch versnellen.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true, `"PreferredDomain`":`"federated.example.edu`"}}") -DisplayName MultiDomainAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Voer de volgende opdracht voor het maken van een beleid voor het inschakelen van verificatie voor gebruikersnaam en wachtwoord voor federatieve gebruikers rechtstreeks met Azure Active Directory voor specifieke toepassingen:

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AllowCloudPasswordValidation`":true}}") -DisplayName EnableDirectAuthPolicy -Type HomeRealmDiscoveryPolicy
```


Om te zien van het nieuwe beleid en krijg de **ObjectID**, voer de volgende opdracht uit:

``` powershell
Get-AzureADPolicy
```


Als u wilt het HRD-beleid wordt toegepast nadat u deze hebt gemaakt, kunt u deze toewijzen aan meerdere service-principals van toepassing.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>Stap 2: Ga naar de service-principal waarop het beleid toewijzen  
U moet de **ObjectID** van de service-principals die u wilt toewijzen van het beleid. Er zijn verschillende manieren zoeken naar de **ObjectID** van service-principals.    

U kunt de portal of u kunt een query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). U kunt ook gaan naar de [Graph Explorer hulpprogramma](https://developer.microsoft.com/graph/graph-explorer) en aanmelden bij uw Azure AD-account om te zien van de service-principals van uw organisatie. Omdat u met behulp van PowerShell, kunt u de cmdlet get-azureadserviceprincipal namelijk niet kunt gebruiken om de service-principals en hun-id's weer te geven.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Stap 3: Het beleid toewijzen aan uw service-principal  
Nadat u hebt de **ObjectID** van de service-principal van de toepassing waarvoor u wilt configureren van automatische-versnelling, de volgende opdracht uitvoeren. Met deze opdracht wordt gekoppeld aan het HRD-beleid dat u in stap 1 hebt gemaakt met de service-principal die u in stap 2.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

U kunt deze opdracht herhalen voor elke service-principal die u wilt toevoegen van het beleid.

In het geval waarbij een toepassing al een HomeRealmDiscovery beleid is toegewezen heeft, kunt u zich niet aan het toevoegen van een tweede account.  In dat geval wordt de definitie van het Thuisrealmdetectie-beleid dat is toegewezen aan de toepassing toe te voegen extra parameters wijzigen.

#### <a name="step-4-check-which-application-service-principals-your-hrd-policy-is-assigned-to"></a>Stap 4: Controleren welke service-principals van toepassing uw HRD-beleid is toegewezen aan
Om te controleren welke toepassingen hebben HRD-beleid dat is geconfigureerd, gebruikt u de **Get-AzureADPolicyAppliedObject** cmdlet. Geeft die door de **ObjectID** van het beleid dat u controleren wilt op.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Stap 5: U bent klaar.
Probeer de toepassing om te controleren of het nieuwe beleid werkt.

### <a name="example-list-the-applications-for-which-hrd-policy-is-configured"></a>Voorbeeld: Een lijst de toepassingen voor welke HRD beleid is geconfigureerd

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>Stap 1: Lijst met alle beleidsregels die zijn gemaakt in uw organisatie 

``` powershell
Get-AzureADPolicy
```

Houd er rekening mee de **ObjectID** van het beleid dat u de lijst met toewijzingen voor wilt.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>Stap 2: Lijst met de service-principals waarop het beleid is toegewezen  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-hrd-policy-for-an-application"></a>Voorbeeld: Een HRD-beleid voor een toepassing verwijderen
#### <a name="step-1-get-the-objectid"></a>Stap 1: De object-id ophalen
Het vorige voorbeeld gebruiken om op te halen de **ObjectID** van het beleid en die van de toepassingsservice-principal van waaruit u wilt verwijderen. 

#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Stap 2: De beleidstoewijzing verwijderen uit de service-principal van toepassing  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-to-which-the-policy-is-assigned"></a>Stap 3: Controleer verwijderen door de service-principals waarop het beleid is toegewezen weer te geven 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de werking van verificatie in Azure AD [Verificatiescenario's voor Azure AD](../develop/active-directory-authentication-scenarios.md).
- Zie voor meer informatie over eenmalige gebruikersaanmelding [toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](configure-single-sign-on-portal.md).
- Ga naar de [Ontwikkelaarshandleiding voor Active Directory](../develop/azure-ad-developers-guide.md) voor een overzicht van alle inhoud die relevant zijn voor ontwikkelaars.
