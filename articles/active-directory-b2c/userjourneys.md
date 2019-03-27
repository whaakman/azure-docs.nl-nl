---
title: UserJourneys | Microsoft Docs
description: Geef het UserJourneys-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 20ca4b9d347b9dc01e3b890fcf3758fb2fb135b9
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486135"
---
# <a name="userjourneys"></a>UserJourneys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Gebruikers reizen opgeven expliciete paden waarmee een beleid voor een relying party-toepassing om op te halen van de gewenste claims voor een gebruiker kunt. De gebruiker wordt uitgevoerd via deze paden om op te halen van de claims die moeten worden gepresenteerd aan de relying party. Gebruikers reizen definiëren met andere woorden, de bedrijfslogica van een eindgebruiker als de Azure AD B2C Identity-Ervaringsframework processen de aanvraag ondergaat.

Deze gebruiker reizen kunnen worden beschouwd als sjablonen die beschikbaar zijn om te voldoen aan de behoeften van de kern van de verschillende Relying Party's van de community van belang zijn. Gebruikers reizen vergemakkelijken de definitie van de relying party deel uitmaken van een beleid. Een beleid kan meerdere gebruikers reizen definiëren. Elke gebruikersbeleving is een reeks indelingsstappen.

Voor het definiëren van de gebruiker reizen ondersteund door het beleid, een **UserJourneys** element onder het element op het hoogste niveau van het beleid-bestand wordt toegevoegd. 

De **UserJourneys** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| UserJourney | 1: n | Een gebruikersbeleving waarmee alle van de concepten die nodig zijn voor een volledige gebruikersstroom wordt gedefinieerd. | 

De **UserJourney** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Ja | Een id van een gebruikersbeleving die kan worden gebruikt om te verwijzen naar deze uit andere elementen in het beleid. De **DefaultUserJourney** element van de [relying party beleid](relyingparty.md) verwijst naar dit kenmerk. |

De **UserJourney** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| OrchestrationSteps | 1: n | De volgorde van een indeling die moet worden gevolgd door voor een geslaagde transactie. Elke gebruikersbeleving bestaat uit een geordende lijst met de orchestration-stappen die worden uitgevoerd in de reeks. Als er een stap mislukt, wordt de transactie mislukt. |

## <a name="orchestrationsteps"></a>OrchestrationSteps

Een gebruikersbeleving wordt weergegeven als een reeks orchestration die moet worden gevolgd door voor een geslaagde transactie. Als er een stap mislukt, wordt de transactie mislukt. Deze stappen orchestration verwijzen naar de bouwstenen en de claimsproviders die zijn toegestaan in het beleid-bestand. Een orchestration-stap die moeten worden weergeven of een gebruikerservaring renderen heeft ook een verwijzing naar de bijbehorende inhoud definitie-id.

Orchestration stappen kunnen worden voorwaardelijk uitgevoerd, op basis van voorwaarden die zijn gedefinieerd in de orchestration-stap-element. U kunt bijvoorbeeld controleren om uit te voeren een indelingsstap alleen als een bepaalde claims aanwezig is, of als een claim gelijk is of niet aan de opgegeven waarde. 

Om op te geven van de geordende lijst indelingsstappen, een **OrchestrationSteps** element wordt toegevoegd als onderdeel van het beleid. Dit element is vereist.

De **OrchestrationSteps** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| OrchestrationStep | 1: n | Een geordende indelingsstap. | 

De **OrchestrationStep** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| `Order` | Ja | De volgorde van de orchestration-stappen. | 
| `Type` | Ja | Het type van de orchestration-stap. Mogelijke waarden: <ul><li>**ClaimsProviderSelection** -geeft aan dat de indelingsstap verschillende claimproviders vormt voor de gebruiker er een selecteren.</li><li>**CombinedSignInAndSignUp** -geeft aan dat de indelingsstap een gecombineerde sociale provider pagina voor het registreren van aanmelding en lokale account biedt.</li><li>**ClaimsExchange** -geeft aan dat de indelingsstap claims met een claimprovider uitwisselt.</li><li>**SendClaims** -geeft aan dat de indelingsstap de claims voor de relying party met een token dat is uitgegeven door een verlener van claims verzendt.</li></ul> | 
| ContentDefinitionReferenceId | Nee | De id van de [inhoud definitie](contentdefinitions.md) die zijn gekoppeld aan deze orchestration-stap. De inhoudsdefinitie verwijzing-ID wordt gewoonlijk gedefinieerd in de zelf-gecontroleerde technisch profiel. Er zijn maar soms bij Azure AD B2C moet iets zonder een technisch profiel wordt weergegeven. Er zijn twee voorbeelden, als het type van de orchestration-stap een van de volgende is: `ClaimsProviderSelection` of `CombinedSignInAndSignUp`. Er moet een Azure AD B2C om de selectie van de provider identiteit zonder een technisch profiel weer te geven. | 
| CpimIssuerTechnicalProfileReferenceId | Nee | Het type van de orchestration-stap is `SendClaims`. Deze eigenschap bepaalt het technische profiel-id van de claimprovider die het token heeft uitgegeven voor de relying party.  Als niet is opgegeven, is geen relying party-token wordt gemaakt. |


De **OrchestrationStep** element mag de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- | 
| Voorwaarden | 0: n | Een lijst met voorwaarden waaraan moet worden voldaan voor de orchestration-stap om uit te voeren. | 
| ClaimsProviderSelections | 0: n | Een lijst van de claims provider selecties voor de orchestration-stap. | 
| ClaimsExchanges | 0: n | Een lijst met claims worden uitgewisseld voor de orchestration-stap. | 

### <a name="preconditions"></a>Voorwaarden

De **voorwaarden** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- | 
| Voorwaarde | 0: n | Afhankelijk van het technische profiel wordt gebruikt, leidt u de client op basis van de selectie van claims-provider of maakt een server te worden aangeroepen voor het uitwisselen van claims. | 


#### <a name="precondition"></a>Voorwaarde

De **voorwaarde** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| `Type` | Ja | Het type selectievakje of de query uit te voeren voor deze voorwaarde. De waarde kan zijn **ClaimsExist**, dat aangeeft dat de acties die moeten worden uitgevoerd als de opgegeven claims aanwezig is in de huidige claimset van de gebruiker, of **ClaimEquals**, dat aangeeft dat de acties moet worden uitgevoerd als de opgegeven claim bestaat en de waarde gelijk aan de opgegeven waarde is. |
| `ExecuteActionsIf` | Ja | Gebruik een test waar of ONWAAR om te bepalen als de acties in de eerste voorwaarde moeten worden uitgevoerd. | 

De **voorwaarde** elementen bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| Value | 1: n | Een ClaimTypeReferenceId moet worden gezocht voor. Een andere waarde-element bevat de waarde moet worden gecontroleerd.</li></ul>|
| Bewerking | 1:1 | De actie die moet worden uitgevoerd als de controle van de voorwaarde in een orchestration-stap ingesteld op true is. Als de waarde van de `Action` is ingesteld op `SkipThisOrchestrationStep`, de bijbehorende `OrchestrationStep` moet niet worden uitgevoerd. | 

#### <a name="preconditions-examples"></a>Voorbeelden van voorwaarden

De volgende voorwaarden wordt gecontroleerd of de objectId van de gebruiker bestaat. De gebruiker heeft geselecteerd in de gebruikersbeleving zich aanmelden met lokaal account. Als de object-id bestaat, moet u deze orchestration-stap overslaan.

```XML
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

De volgende voorwaarden wordt gecontroleerd of de gebruiker aangemeld met een sociaalnetwerkaccount. Een poging is gedaan aan het gebruikersaccount niet vinden in de map. Als de gebruiker zich aanmeldt of zich aanmeldt met een lokaal account, orchestration Sla deze stap over.

```XML
<OrchestrationStep Order="3" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>authenticationSource</Value>
      <Value>localAccountAuthentication</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="AADUserReadUsingAlternativeSecurityId" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId-NoError" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Voorwaarden kunnen meerdere voorwaarden controleren. Het volgende voorbeeld wordt gecontroleerd of 'objectId' of 'e' komt. Als de eerste voorwaarde waar is, slaat het traject naar de volgende indelingsstap.

```XML
<OrchestrationStep Order="4" Type="ClaimsExchange">
  <Preconditions>
  <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>email</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>      
  <ClaimsExchanges>
    <ClaimsExchange Id="SelfAsserted-SocialEmail" TechnicalProfileReferenceId="SelfAsserted-SocialEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsproviderselection"></a>ClaimsProviderSelection

Een orchestration-stap van het type `ClaimsProviderSelection` of `CombinedSignInAndSignUp` een lijst met claimsproviders die een gebruiker zich met aanmelden kan kan bevatten. De volgorde van de elementen binnen de `ClaimsProviderSelections` elementen bepaalt de volgorde van de id-providers weergegeven voor de gebruiker.

De **ClaimsProviderSelection** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| ClaimsProviderSelection | 0: n | Geeft de lijst van claims-providers die kunnen worden geselecteerd.|

De **ClaimsProviderSelection** element bevat de volgende kenmerken: 

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| TargetClaimsExchangeId | Nee | De id van de claimuitwisseling, die wordt uitgevoerd in de volgende indelingsstap van de selectie van claims-provider. Dit kenmerk of het kenmerk ValidationClaimsExchangeId moet worden opgegeven, maar niet beide. | 
| ValidationClaimsExchangeId | Nee | De id van de claimuitwisseling, die wordt uitgevoerd in de huidige orchestration-stap voor het valideren van de selectie van claims-provider. Dit kenmerk of het kenmerk TargetClaimsExchangeId moet worden opgegeven, maar niet beide. |

### <a name="claimsproviderselection-example"></a>Voorbeeld van de ClaimsProviderSelection

De gebruiker kan kiezen in de volgende stap van de orchestration, zich aanmelden met Facebook, LinkIn, Twitter, Google of een lokaal account. Als de gebruiker een van de sociale id-providers selecteert, de tweede orchestration-stap wordt uitgevoerd met de geselecteerde claim exchange opgegeven in de `TargetClaimsExchangeId` kenmerk. De tweede orchestration-stap wordt de gebruiker omgeleid naar de sociale id-provider om het proces aanmelden te voltooien. Als de gebruiker wil zich aanmelden met het lokale account, wordt Azure AD B2C blijft op dezelfde indelingsstap (dezelfde pagina voor het registreren of aanmelden pagina) en de tweede orchestration-stap overslaan.

```XML
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
    <ClaimsProviderSelections>
    <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
    <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
    </ClaimsProviderSelections>
    <ClaimsExchanges>
    <ClaimsExchange Id="LocalAccountSigninEmailExchange" 
                    TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
    </ClaimsExchanges>
</OrchestrationStep>


<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsexchanges"></a>ClaimsExchanges

De **ClaimsExchanges** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| ClaimsExchange | 0: n | Afhankelijk van het technische profiel wordt gebruikt, leidt u de client op basis van de ClaimsProviderSelection dat is geselecteerd, of maakt een server te worden aangeroepen voor het uitwisselen van claims. | 

De **ClaimsExchange** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Ja | Een id van de claims exchange-stap. De id wordt gebruikt om te verwijzen naar de claimuitwisseling van van een claims provider-selectie stap in het beleid. | 
| TechnicalProfileReferenceId | Ja | De id van het technische profiel die moet worden uitgevoerd. |
