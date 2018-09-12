---
title: TrustFrameworkPolicy - Azure Active Directory B2C | Microsoft Docs
description: Geef het TrustFrameworkPolicy-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 16e98811b65e215d8688e030ea8dcbb1f9446a5b
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381337"
---
# <a name="trustframeworkpolicy"></a>TrustFrameworkPolicy

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Een aangepast beleid wordt weergegeven als een of meer XML-bestanden, die in een hiërarchische keten naar elkaar verwijzen. De XML-elementen definiëren elementen van het beleid, zoals schema voor claims, claimtransformaties, inhoudsdefinities, claims-providers, technische profielen, gebruikersbeleving en indelingsstappen. Elk beleidsbestand is gedefinieerd in het bovenste niveau **TrustFrameworkPolicy** element van een beleid-bestand. 

```XML
<TrustFrameworkPolicy
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
  ...
```


De **TrustFrameworkPolicy** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
|---------- | -------- | ----------- |
| PolicySchemaVersion | Ja | De schemaversie die moet worden gebruikt voor het uitvoeren van het beleid. De waarde moet liggen `0.3.0.0` |
| TenantObjectId | Nee | De unieke id van de Azure Active Directory (Azure AD) B2C-tenant. |
| TenantId | Ja | De unieke id van de tenant die dit beleid behoort. |
| PolicyId | Ja | De unieke id voor het beleid. Deze id moet worden voorafgegaan door *B2C_1A_* |
| PublicPolicyUri | Ja | De URI voor het beleid, waarvan een combinatie van de tenant-ID en de beleids-ID. |
| DeploymentMode | Nee | Mogelijke waarden: `Production`, `Debugging`, of `Development`. `Production` is de standaardwaarde. Gebruik deze eigenschap fouten opsporen in uw beleid. Zie voor meer informatie, [verzamelen van logboeken](active-directory-b2c-troubleshoot-custom.md). |
| UserJourneyRecorderEndpoint | Nee | Het eindpunt dat wordt gebruikt als **DeploymentMode** is ingesteld op `Development`. De waarde moet `urn:journeyrecorder:applicationinsights`. Zie voor meer informatie, [verzamelen van logboeken](active-directory-b2c-troubleshoot-custom.md). |


Het volgende voorbeeld ziet u hoe u de **TrustFrameworkPolicy** element:

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="http://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkBase"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
```

## <a name="inheritance-model"></a>Van overnamemodel

Deze typen bestanden voor Groepsbeleid worden meestal gebruikt in een gebruikersbeleving:

- Een **Base** -bestand met de meeste van de definities. Om u te helpen bij het oplossen van problemen en de lange termijn onderhouden van uw beleid, is het raadzaam dat u een minimum aantal wijzigingen in dit bestand aanbrengt.
- Een **extensies** bestand waarin de unieke configuratiewijzigingen voor uw tenant. In dit bestand is afgeleid van de Base-bestand. Dit bestand gebruiken om nieuwe functionaliteit toevoegen of bestaande functionaliteit overschrijven. Bijvoorbeeld: dit bestand gebruiken om te federeren met nieuwe id-providers.
- Een **Relying Party (RP)** bestand dat één taak gerichte bestand die wordt aangeroepen door de relying party-toepassing, zoals uw webtoepassingen, mobiele of bureaubladtoepassingen kunnen rechtstreeks. Elke unieke taak, zoals registreren of aanmelden, wachtwoord opnieuw instellen of bewerken van het profiel, moet een eigen RP beleid-bestand. In dit bestand is afgeleid van het bestand extensies. 

Een relying party-toepassing roept de RP-beleidsbestand voor het uitvoeren van een specifieke taak. Als u bijvoorbeeld de stroom aanmelden start. De Identity-Ervaringsframework in Azure AD B2C telt alle elementen eerst uit het basis-bestand en klik in het bestand extensies en ten slotte uit het bestand van de RP-beleid voor het samenstellen van het huidige beleid van kracht. Elementen van hetzelfde type en met de naam in de RP-bestand overschrijven deze elementen in de extensies en extensies onderdrukkingen Base. Het volgende diagram toont de relatie tussen de bestanden voor Groepsbeleid en de relying party-toepassingen.

![Van overnamemodel](./media/trustframeworkpolicy/custom-policy-Inheritance-model.png)

De overnamemodel is als volgt:

- Het beleid van de bovenliggende en onderliggende zijn van hetzelfde schema.
- Het beleid onderliggende op elk niveau kan overnemen van het beleid van de bovenliggende en deze kunt uitbreiden door nieuwe elementen toe te voegen.
- Er is geen limiet voor het aantal niveaus.

Zie voor meer informatie, [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).

## <a name="base-policy"></a>Basisbeleid

Een beleid worden overgenomen van een ander beleid, een **BasePolicy** element moet worden gedeclareerd onder de **TrustFrameworkPolicy** element van het beleid-bestand. De **BasePolicy** element is een verwijzing naar het Basisbeleid waarop dit beleid is afgeleid.  

De **BasePolicy** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | --------|
| TenantId | 1:1 | De id van uw Azure AD B2C-tenant. |
| PolicyId | 1:1 | De id van het bovenliggende beleid. |


Het volgende voorbeeld ziet hoe u kunt een Basisbeleid opgeven. Dit **B2C_1A_TrustFrameworkExtensions** beleid is afgeleid van de **B2C_1A_TrustFrameworkBase** beleid. 

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="http://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkExtensions"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkExtensions">

  <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkBase</PolicyId>
  </BasePolicy>
  ...
</TrustFrameworkPolicy>
```

## <a name="policy-execution"></a>Beleid kan worden uitgevoerd

Een relying party-toepassing, zoals een web-, mobiele of bureaubladtoepassingen-toepassing, roept de [relying party (RP) beleid](relyingparty.md). De RP-beleidsbestand voert een specifieke taak, zoals het aanmelden, een wachtwoord in te stellen of bewerken van een profiel. Het RP-beleid wordt de lijst met claims ontvangt relying party-toepassing geconfigureerd als onderdeel van het token dat is uitgegeven. Meerdere toepassingen kunnen gebruikmaken van hetzelfde beleid. Alle toepassingen hetzelfde token met claims ontvangt en de gebruiker de dezelfde gebruikersbeleving doorloopt. Een toepassing kunt meerdere beleidsregels gebruiken.

In de RP-beleid-bestand, geeft u de **DefaultUserJourney** element, dat naar verwijst de [UserJourney](userjourneys.md). De gebruikersbeleving wordt gewoonlijk gedefinieerd in het beleid voor basis- of -extensies.

Beleid voor B2C_1A_signup_signin:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn">
  ...
```

B2C_1A_TrustFrameWorkBase of B2C_1A_TrustFrameworkExtensionPolicy:

```XML
<UserJourneys>
  <UserJourney Id="SignOrSignIn">
  ...
```

Een gebruikersbeleving definieert de bedrijfslogica van een gebruiker doorlopen wordt. Elke gebruikersbeleving is een reeks indelingsstappen die een reeks acties in de reeks in termen van verificatie- en informatie-verzameling uitvoert. 

De **SocialAndLocalAccounts** beleid-bestand in de [beginnerspakket](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom#download-starter-pack-and-modify-policies) bevat de SignUpOrSignIn ProfileEdit, PasswordReset gebruiker reizen. U kunt meer gebruikers reizen voor een andere scenario's, zoals het wijzigen van een e-mailadres, koppelen en ontkoppelen van een sociaal account of het opnieuw instellen van een wachtwoord toevoegen. 

De orchestration-stappen vragen mogelijk een [technisch profiel](technicalprofiles.md). Een technisch profiel biedt een raamwerk met een ingebouwd mechanisme om te communiceren met verschillende soorten partijen. Een technisch profiel kan bijvoorbeeld deze acties onder andere uitvoeren:

- De ervaring van een gebruiker worden weergegeven.
- Toestaan dat gebruikers zich aanmelden met sociale netwerken of een enterprise-account, zoals Facebook, Microsoft-account, Google, Salesforce of andere id-provider.
- Instellen van verificatie via de telefoon voor MFA.
- Lezen en schrijven van gegevens naar en van een Azure AD B2C-identiteitenarchief.
- Een aangepaste Restful-API-service aanroepen.

![Beleid kan worden uitgevoerd](./media/trustframeworkpolicy/custom-policy-execution.png)

 De **TrustFrameworkPolicy** element bevat de volgende elementen:

- BasePolicy zoals opgegeven bovenstaande
- [BuildingBlocks](buildingblocks.md)
- [ClaimsProviders](claimsproviders.md)
- [UserJourneys](userjourneys.md)
- [RelyingParty](relyingparty.md)

