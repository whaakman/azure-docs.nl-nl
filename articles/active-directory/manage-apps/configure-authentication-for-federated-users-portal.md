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
ms.topic: it-pro
ms.date: 06/08/2018
ms.author: barbkess
ms.openlocfilehash: f2ab0a4458c83aa9e5c9cee4875e41c24f615018
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301196"
---
# <a name="configure-azure-active-directory-sign-in-behavior-for-an-application-by-using-a-home-realm-discovery-policy"></a>Azure Active Directory aanmeldingsopties configureren in het gedrag voor een toepassing met een Thuisrealmdetectie-beleid

Het volgende document bevat een inleiding tot het gedrag van Azure Active Directory-verificatie voor federatieve gebruikers configureren.   Deze heeft configuratie van automatische-versnelling en verificatie beperkingen voor gebruikers in de federatieve domeinen.

## <a name="home-realm-discovery"></a>Detectie van thuisrealm
Thuis Realm detectie (HRD) is het proces waarmee Azure Active Directory (Azure AD) om te bepalen waar een gebruiker moet worden geverifieerd op de tijd van aanmelden.  Wanneer een gebruiker zich aanmeldt met een Azure AD-tenant voor toegang tot een bron of naar de Azure AD algemene aanmeldingspagina, typt u de naam van een gebruiker (UPN). Azure AD gebruikt om te ontdekken waar de gebruiker moet aan te melden. 

De gebruiker moet mogelijk worden uitgevoerd om een van de volgende locaties moet worden geverifieerd:

- De oorspronkelijke tenant van de gebruiker (mogelijk dezelfde tenant als de resource die de gebruiker toegang probeert te krijgen zijn). 

- Microsoft-account.  De gebruiker is een gast in de resource-tenant.

-  Een id-provider voor het lokale zoals Active Directory Federation Services (AD FS).

- Een andere id-provider die gefedereerd met Azure AD-tenant.

## <a name="auto-acceleration"></a>Automatische-versnelling 
Sommige organisaties configureren domeinen in de Azure Active Directory-tenant wilt federeren met een andere IdP, zoals AD FS voor verificatie van gebruiker.  

Wanneer een gebruiker zich in een toepassing, wordt ze eerst een Azure AD-aanmeldingspagina weergegeven. Nadat ze hun UPN hebt getypt als ze in een federatieve domein ze vervolgens gaat naar de aanmeldingspagina van de IdP dat domein dient. Onder bepaalde omstandigheden beheerders mogelijk willen gebruikers verwijzen naar de aanmeldingspagina wanneer ze zijn aangemeld bij specifieke toepassingen. 

Als gevolg hiervan kunnen gebruikers de eerste pagina van de Azure Active Directory overslaan. Dit proces wordt aangeduid als "aanmelden automatisch-versnelling."

In gevallen waar de tenant naar een andere IdP voor aanmelding is gefedereerd, maakt automatisch versnelling gebruiker aanmelden sneller.  U kunt automatische-versnelling voor afzonderlijke toepassingen kunt configureren.

>[!NOTE]
>Als u een toepassing voor automatische versnelling configureert, aanmelden gastgebruikers niet. Als u een gebruiker direct door naar een federatieve IdP voor verificatie, is er geen manier om ze terugkeren naar de aanmeldingspagina van Azure Active Directory. Gastgebruikers die worden omgeleid naar de andere tenants of een externe IdP zoals een Microsoft-account moeten mogelijk, kunnen niet aanmelden bij die toepassing omdat ze bij het overslaan van de stap Thuisrealmdetectie.  

Er zijn twee manieren om te bepalen voor een federatieve IdP-auto-versnelling:   

- Gebruik een geheugensteun voor het domein op verificatieaanvragen voor een toepassing. 
- Configureer een beleid Thuisrealmdetectie als auto-versnelling wilt inschakelen.

### <a name="domain-hints"></a>Domein-hints    
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
>Als een geheugensteun voor het domein is opgenomen in een verificatieaanvraag, overschrijft de aanwezigheid automatisch-versnelling die is ingesteld voor de toepassing in HRD-beleid.

### <a name="home-realm-discovery-policy-for-auto-acceleration"></a>Thuis Realm detectie beleid voor automatische-versnelling
Sommige toepassingen beschikken niet over een manier voor het configureren van de authenticatie-aanvraag die ze verzenden. In dergelijke gevallen is het niet mogelijk te domein hints gebruiken automatisch versnelling beheren. Automatische versnelling kan worden geconfigureerd via beleid voor hetzelfde gedrag.  

## <a name="enable-direct-authentication-for-legacy-applications"></a>Directe-verificatie inschakelen voor oudere toepassingen
Aanbevolen procedure is voor toepassingen kunnen gebruikmaken van AAD-bibliotheken en interactief aanmelden om gebruikers te verifiëren. De bibliotheken zorgt voor de federatieve gebruiker stromen.  Soms oudere toepassingen niet worden geschreven om te begrijpen federation. Deze detectie van thuisrealm niet uitvoeren en worden niet gebruikt door het juiste federatieve eindpunt een gebruiker te verifiëren. Als u wilt, kunt u HRD beleid voor specifieke oudere toepassingen die referenties van de gebruikersnaam en wachtwoord voor verificatie rechtstreeks met Azure Active Directory indienen. Wachtwoordhashsynchronisatie moet zijn ingeschakeld. 

> [!IMPORTANT]
> Alleen directe verificatie inschakelen als u Wachtwoordhashsynchronisatie ingeschakeld hebt en u kunt deze toepassing verifiëren zonder eventuele beleidsregels geïmplementeerd door uw lokale IdP weet. Als u Wachtwoordhashsynchronisatie uitschakelen of Directory Synchronization met AD Connect voor een bepaalde reden uitschakelen, moet u dit beleid om te voorkomen dat de mogelijkheid om direct verificatie met behulp van een verlopen wachtwoord-hash verwijderen.

## <a name="set-hrd-policy"></a>HRD-beleid instellen
Er zijn drie stappen voor de instelling HRD beleid op een toepassing voor federatieve aanmelding automatisch-versnelling of directe cloud-gebaseerde toepassingen:

1. Maak een HRD-beleid.

2. Zoek de service-principal waarop het beleid te koppelen.

3. Het beleid aan de service-principal koppelen. 

Beleid alleen van kracht voor een bepaalde toepassing wanneer deze zijn gekoppeld aan een service-principal. 

Slechts één HRD beleid kan op elk gewenst moment actief is op een service-principal zijn.  

Maken en beheren van HRD beleid kunt u de Microsoft Azure Active Directory Graph API rechtstreeks of Azure Active Directory PowerShell-cmdlets.

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

Het type beleid dat is 'HomeRealmDiscoveryPolicy'.

**AccelerateToFederatedDomain** is optioneel. Als **AccelerateToFederatedDomain** is ingesteld op false, het beleid heeft geen invloed op automatische versnelling. Als **AccelerateToFederatedDomain** is true en er is slechts één geverifieerd en federatieve domein in de tenant en vervolgens gebruikers gaat rechte naar de federatieve IdP voor aanmelding. Als dit waar is en er meer dan één gecontroleerd domein in de tenant is **PreferredDomain** moet worden opgegeven.

**PreferredDomain** is optioneel. **PreferredDomain** zou moeten aangeven waarom een domein waarnaar u wilt versnellen. Het kan worden overgeslagen als de tenant slechts één federatieve domein heeft.  Als dit wordt weggelaten en er is meer dan één federatieve domein geverifieerd, heeft het beleid geen effect.

 Als **PreferredDomain** is opgegeven, moet overeenkomen met een geverifieerde, federatieve domein voor de tenant. Alle gebruikers van de toepassing moeten kunnen aanmelden met dat domein.

**AllowCloudPasswordValidation** is optioneel. Als **AllowCloudPasswordValidation** is ingesteld op true en vervolgens de toepassing is toegestaan voor het verifiëren van een federatieve gebruiker in de vorm van gebruikersnaam en wachtwoord referenties rechtstreeks naar het eindpunt van het Azure Active Directory-token. Dit werkt alleen als Wachtwoordhashsynchronisatie is ingeschakeld.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Prioriteit en beoordeling van beleid HRD
HRD-beleid kunnen worden gemaakt en vervolgens toegewezen aan specifieke organisaties en service-principals. Dit betekent dat het mogelijk voor meerdere beleidsregels toepassen op een bepaalde toepassing is. Het HRD-beleid dat van kracht volgt deze regels:


- Als een geheugensteun voor het domein in de verificatieaanvraag aanwezig is, wordt de HRD beleid voor automatische versnelling genegeerd. Het gedrag op dat opgegeven door de domein-hint wordt gebruikt.

- Anders als een beleid expliciet aan de service-principal is toegewezen, dat deze wordt afgedwongen. 

- Als er geen geheugensteun voor het domein, en er geen beleid expliciet is toegewezen aan de service-principal, wordt een beleid dat expliciet toegewezen aan de bovenliggende organisatie van de service-principal afgedwongen. 

- Als er geen geheugensteun voor het domein, en er geen beleid is toegewezen aan de service-principal of de organisatie, wordt het standaardgedrag HRD gebruikt.

## <a name="tutorial-for-setting-hrd-policy-on-an-application"></a>Zelfstudie HRD beleid worden ingesteld voor een toepassing 
We Azure AD PowerShell-cmdlets gebruiken om enkele scenario's, waaronder doorlopen:


- HRD beleid instellen voor een toepassing in een tenant met één federatieve domein-auto-versnelling doen.

- HRD beleid instellen om te doen auto-versnelling voor een toepassing op een van de verschillende domeinen die zijn geverifieerd voor uw tenant.

- HRD beleid instellen om in te schakelen van een oudere toepassing gebruikersnaam en wachtwoord verificatie met Azure Active Directory voor een federatieve gebruiker wordt omgeleid.

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

### <a name="example-set-hrd-policy-for-an-application"></a>Voorbeeld: Set HRD beleid voor een toepassing 

In dit voorbeeld wordt maken u een beleid dat als deze is toegewezen aan een toepassing ofwel: 
- Automatische versnelt-gebruikers aan een AD FS aanmeldingsscherm wanneer ze voor een toepassing aanmelden zich als er één domein in uw tenant. 
- Gebruikers automatisch versnelt aan een AD FS in het scherm er is meer dan één federatieve domein in uw tenant.
- Hiermee kunt niet-interactieve gebruikersnaam en wachtwoord aanmelden bij Azure Active Directory voor federatieve gebruikers voor de toepassingen die het beleid is toegewezen aan rechtstreeks.

#### <a name="step-1-create-an-hrd-policy"></a>Stap 1: Een HRD-beleid maken

Het volgende beleid versnelt automatisch-gebruikers aan een AD FS aanmeldingsscherm wanneer ze voor een toepassing aanmelden zich als er één domein in uw tenant.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```
Het volgende beleid versnelt automatisch-gebruikers aan een AD FS in het scherm er is meer dan één federatieve domein in uw tenant. Als u meer dan één federatieve domein hebt dat verificatie van gebruikers voor toepassingen, moet u het domein om automatisch versnellen opgeven.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true, `"PreferredDomain`":`"federated.example.edu`"}}") -DisplayName MultiDomainAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Voer de volgende opdracht voor het maken van een beleid voor verificatie van de gebruikersnaam en wachtwoord voor federatieve gebruikers rechtstreeks met Azure Active Directory voor specifieke toepassingen inschakelen:

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AllowCloudPasswordValidation`":true}}") -DisplayName EnableDirectAuthPolicy -Type HomeRealmDiscoveryPolicy
```


Als u wilt zien van het nieuwe beleid en de **ObjectID**, voer de volgende opdracht:

``` powershell
Get-AzureADPolicy
```


Als u wilt toepassen het HRD beleid nadat u deze hebt gemaakt, kunt u deze toewijzen aan meerdere service-principals van toepassing.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>Stap 2: Zoek de service-principal waarop het beleid toewijzen  
U moet de **ObjectID** van de service-principals waarnaar u wilt toewijzen van het beleid. Er zijn verschillende manieren zoeken naar de **ObjectID** van service-principals.    

U kunt de portal of u kunt een query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). U kunt ook gaan naar de [grafiek Explorer Tool](https://graphexplorer.cloudapp.net/) en meld u aan bij uw Azure AD-account om te zien van de service-principals van uw organisatie. Omdat u met behulp van PowerShell, kunt u de cmdlet get-AzureADServicePrincipal voor een lijst met de service-principals en hun id.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Stap 3: Het beleid aan uw service-principal toewijzen  
Nadat u hebt de **ObjectID** van de service-principal van de toepassing waarvoor u voor het configureren van automatische-versnelling wilt de volgende opdracht uitvoeren. Met deze opdracht wordt gekoppeld aan het beleid HRD die u in stap 1 hebt gemaakt met de service-principal die u in stap 2.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

U kunt deze opdracht herhalen voor elke service-principal waarnaar u wilt toevoegen van het beleid.

In het geval waar al een toepassing een HomeRealmDiscovery beleid dat is toegewezen heeft, niet mogelijk voor het toevoegen van een tweede account.  De definitie van het Thuisrealmdetectie-beleid dat is toegewezen aan de toepassing voor het toevoegen van extra parameters in dat geval wijzigen.

#### <a name="step-4-check-which-application-service-principals-your-hrd-policy-is-assigned-to"></a>Stap 4: Controleren welke toepassing service-principals uw HRD-beleid is toegewezen aan
Om te controleren welke toepassingen hebt HRD beleid dat is geconfigureerd, gebruikt u de **Get-AzureADPolicyAppliedObject** cmdlet. Geef dit door de **ObjectID** van het beleid dat u controleren wilt op.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Stap 5: U bent nu klaar!
Probeer de toepassing om te controleren of het nieuwe beleid werkt.

### <a name="example-list-the-applications-for-which-hrd-policy-is-configured"></a>Voorbeeld: De toepassingen voor welke HRD beleid geconfigureerd weergeven

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>Stap 1: Lijst met alle beleidsregels die zijn gemaakt in uw organisatie 

``` powershell
Get-AzureADPolicy
```

Opmerking de **ObjectID** van het beleid dat u wilt de toewijzingen van de lijst voor.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>Stap 2: Overzicht van de service-principals waarop het beleid is toegewezen  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-hrd-policy-for-an-application"></a>Voorbeeld: Een HRD-beleid voor een toepassing verwijderen
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
- Zie voor meer informatie over de werking van verificatie in Azure AD [verificatie scenario's voor Azure AD](../develop/active-directory-authentication-scenarios.md).
- Zie voor meer informatie over eenmalige aanmelding gebruiker [toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](configure-single-sign-on-portal.md).
- Ga naar de [ontwikkelaarshandleiding Active Directory](../develop/active-directory-developers-guide.md) voor een overzicht van alle inhoud die relevant zijn voor ontwikkelaars.
