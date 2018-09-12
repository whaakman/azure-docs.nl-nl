---
title: Een RESTful technisch profiel definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer een RESTful technisch profiel in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 930cdddd8a9e039fa9c29a348a0a66eb25d254fe
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381276"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een RESTful technisch profiel definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C biedt ondersteuning voor uw eigen RESTful-service. Azure AD B2C verzendt gegevens naar de RESTful-service in invoer verzameling claims en ontvangt gegevens terug in een verzameling van de claims uitvoer. Met integratie van RESTful-service, kunt u het volgende doen:

- **Valideren van de gebruiker ingevoerde gegevens** -voorkomt dat onjuist gevormde gegevens opslaan op Azure AD B2C. Als de waarde van de gebruiker niet geldig is, retourneert de RESTful-service een foutbericht weergegeven waarin de gebruiker voor een vermelding wordt geïnstrueerd. U kunt bijvoorbeeld controleren of het e-mailadres dat is opgegeven door de gebruiker in de database van uw klant bestaat.
- **Invoerclaims overschrijven** -kunt u waarden in invoerclaims formatteren. Als een gebruiker moet invoeren voor de eerste naam in alleen kleine letters of alle hoofdletters, kunt u bijvoorbeeld de naam van de opmaken met alleen de eerste letter in hoofdletters zijn.
- **Gebruikersgegevens verrijken** -kunt u om verder te integreren met zakelijke line-of-business-toepassingen. Uw RESTful-service kan bijvoorbeeld ontvangen van e-mailadres van de gebruiker, query uitvoeren op database van de klant en loyaliteit-nummer van de gebruiker terug naar Azure AD B2C. De geretourneerde claims kunnen worden opgeslagen, geëvalueerd in de volgende stappen in de indeling of opgenomen in het toegangstoken.
- **Uitvoeren van aangepaste bedrijfslogica** - kunt u pushmeldingen te verzenden, zakelijke databases bijwerken, het migratieproces van een gebruiker worden uitgevoerd, machtigingen beheren en controleren van databases en andere acties worden uitgevoerd.

Uw beleid kan invoerclaims om uw REST-API te verzenden. De REST-API kan ook retourneren uitvoerclaims die u later in uw beleid kunt gebruiken kan, of het een foutbericht weergegeven. U kunt de integratie met de RESTful-services ontwerpen in de volgende manieren:

- **Validatie van technisch profiel** -een profiel van de technische roept de RESTful-service. Het technische validatieprofiel valideert de gegevens van de gebruiker opgegeven voordat de gebruikersbeleving wordt hervat. Met het technische validatieprofiel een foutbericht wordt weergegeven op een zelf-gecontroleerde pagina en geretourneerd in de uitvoerclaims.
- **Exchange-claims** -er wordt een aanroep uitgevoerd naar de RESTful-service via een orchestration-stap. In dit scenario is er geen gebruikersinterface om het foutbericht weer te geven. Als de REST-API een fout retourneert, wordt de gebruiker wordt omgeleid naar de relying party-toepassing met het foutbericht.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `Proprietary`. De **handler** kenmerk mag de volledig gekwalificeerde naam van de handler-assembly voor protocol dat wordt gebruikt door Azure AD B2C: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Het volgende voorbeeld ziet u een RESTful technisch profiel:

```XML
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...    
```

## <a name="input-claims"></a>Invoerclaims

De **InputClaims** element bevat een lijst met claims verzenden naar de REST-API. U kunt ook de naam van de claim met de naam die is gedefinieerd in de REST-API toewijzen. Volgende voorbeeld ziet u de toewijzing tussen het beleid en de REST-API. De **givenName** claim wordt verzonden naar de REST-API als **firstName**, terwijl **achternaam** wordt verzonden als **lastName**. De **e** claim is ingesteld als is.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

De **InputClaimsTransformations** element kan bevatten een verzameling van **InputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de invoerclaims of nieuwe labels voordat ze worden verzonden naar de REST-API te genereren.

## <a name="output-claims"></a>Gebruikersclaims

De **OutputClaims** element bevat een lijst met claims die zijn geretourneerd door de REST-API. Mogelijk moet u de naam van de claim die is gedefinieerd in uw beleid aan de naam die is gedefinieerd in de REST-API toewijzen. U kunt ook claims die niet zijn geretourneerd door de identiteitsprovider REST-API opnemen, zolang u de `DefaultValue` kenmerk.

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.

Het volgende voorbeeld ziet u de claim die wordt geretourneerd door de REST-API:

- De **MembershipId** claim die is toegewezen aan de **loyaltyNumber** naam van claim.

Het technische profiel retourneert ook claims, die niet zijn geretourneerd door de id-provider: 

- De **loyaltyNumberIsNew** claim die ingesteld is op standaardwaarde `true`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| ServiceUrl | Ja | De URL van de REST API-eindpunt. | 
| AuthenticationType | Ja | Het type verificatie wordt uitgevoerd door de RESTful-claims-provider. Mogelijke waarden: `None`, `Basic`, of `ClientCertificate`. De `None` waarde geeft aan dat de REST-API is niet anoniem. De `Basic` waarde geeft aan dat de REST-API is beveiligd met HTTP-basisverificatie. Alleen geverifieerde gebruikers, met inbegrip van Azure AD B2C, hebben toegang tot uw API. De `ClientCertificate` (aanbevolen) waarde geeft aan dat de REST-API toegang wordt beperkt tot met clientverificatie via certificaat. Alleen services die u de juiste certificaten, zoals Azure AD B2C hebt hebben toegang tot uw service. | 
| SendClaimsIn | Nee | Hiermee geeft u op hoe de invoer claims worden verzonden naar de RESTful-claims-provider. Mogelijke waarden: `Body` (standaard), `Form`, `Header`, of `QueryString`. De `Body` waarde is de invoer claim die wordt verzonden in de hoofdtekst van de aanvraag in JSON-indeling. De `Form` waarde is de invoer claim die wordt verzonden in de hoofdtekst van de aanvraag in een en-teken ' &' gescheiden sleutelwaarde-indeling. De `Header` waarde is de invoer claim die in de aanvraagheader is verzonden. De `QueryString` waarde is de invoer claim die in de querytekenreeks van de aanvraag is verzonden. | 
| ClaimsFormat | Nee | Hiermee geeft u de indeling voor de uitvoerclaims. Mogelijke waarden: `Body` (standaard), `Form`, `Header`, of `QueryString`. De `Body` waarde is de uitvoerclaim die wordt verzonden in de hoofdtekst van de aanvraag in JSON-indeling. De `Form` waarde is de uitvoerclaim die wordt verzonden in de hoofdtekst van de aanvraag in een en-teken ' &' gescheiden sleutelwaarde-indeling. De `Header` waarde is de uitvoerclaim die in de aanvraagheader is verzonden. De `QueryString` waarde is de uitvoerclaim die in de querytekenreeks van de aanvraag is verzonden. | 
| Fouten opsporen-modus | Nee | Het technische profiel uitgevoerd in de foutopsporingsmodus. De REST-API kunt meer informatie retourneren in de foutopsporingsmodus. Zie de sectie retourneert fout. | 

## <a name="cryptographic-keys"></a>Cryptografische sleutels

Als het type verificatie is ingesteld op `None`, wordt de **CryptographicKeys** element wordt niet gebruikt.

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
</TechnicalProfile>
```

Als het type verificatie is ingesteld op `Basic`, wordt de **CryptographicKeys** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | Ja | De gebruikersnaam die wordt gebruikt voor verificatie. | 
| BasicAuthenticationPassword | Ja | Het wachtwoord dat wordt gebruikt voor verificatie. |

Het volgende voorbeeld ziet u een technisch profiel met basisverificatie wordt gebruikt:

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Basic</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
  </CryptographicKeys>
</TechnicalProfile>
```

Als het type verificatie is ingesteld op `ClientCertificate`, wordt de **CryptographicKeys** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| ClientCertificate | Ja | De X509 certificaat (RSA-sleutelpaar) gebruiken om te verifiëren. | 

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ClientCertificate</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
  </CryptographicKeys>
</TechnicalProfile>
```

## <a name="returning-error-message"></a>Foutbericht retourneren

De REST-API moet mogelijk een foutbericht retourneren, zoals 'de gebruiker is niet gevonden in de CRM-systeem'. In een fout optreedt, de REST-API moet retourneren een foutbericht HTTP 409 (Conflict antwoordstatuscode) met de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| versie | Ja | 1.0.0 | 
| status | Ja | 409 | 
| Code | Nee | Een foutcode op basis van de RESTful-eindpunt-provider die wordt weergegeven wanneer `DebugMode` is ingeschakeld. | 
| aanvraag-id | Nee | Een aanvraag-id van de RESTful-eindpunt-provider die wordt weergegeven wanneer `DebugMode` is ingeschakeld. | 
| userMessage | Ja | Een foutbericht dat wordt weergegeven aan de gebruiker. | 
| developerMessage | Nee | De uitgebreide beschrijving van het probleem en aanwijzingen om op te lossen, die wordt weergegeven wanneer `DebugMode` is ingeschakeld. | 
| moreInfo | Nee | Een URI die verwijst naar aanvullende informatie die wordt weergegeven wanneer `DebugMode` is ingeschakeld. | 

Het volgende voorbeeld ziet u een REST-API waarmee een foutbericht weergegeven dat is geformatteerd in JSON geretourneerd:

```JSON
{
  "version": "1.0.0",
  "status": 409,
  "code": "API12345",
  "requestId": "50f0bd91-2ff4-4b8f-828f-00f170519ddb",
  "userMessage": "Message for the user", 
  "developerMessage": "Verbose description of problem and how to fix it.", 
  "moreInfo": "https://restapi/error/API12345/moreinfo" 
}
```

Het volgende voorbeeld ziet u een C#-klasse die een foutbericht weergegeven wordt:

```C#
public class ResponseContent
{
  public string version { get; set; }
  public int status { get; set; }
  public string code { get; set; }
  public string userMessage { get; set; }
  public string developerMessage { get; set; }
  public string requestId { get; set; }
  public string moreInfo { get; set; }
}
```

## <a name="examples"></a>Voorbeelden:
- [Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als validatie van de gebruikersinvoer](active-directory-b2c-custom-rest-api-netfw.md) 
- [Uw RESTful-services beveiligen met behulp van HTTP-basisverificatie](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
- [Beveiligen van uw RESTful-service met behulp van clientcertificaten](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
- [Walkthrough: Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als validatie op invoer van de gebruiker](active-directory-b2c-rest-api-validation-custom.md)

 














