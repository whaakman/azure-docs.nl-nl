---
title: Azure voor eenmalige aanmelding SAML-Protocol | Microsoft Docs
description: In dit artikel beschrijft de eenmalige aanmelding op SAML-protocol in Azure Active Directory
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: ad8437f5-b887-41ff-bd77-779ddafc33fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: f41402fc2cb282975b93071d998365fdb0a21941
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# Single Sign-On SAML-protocol
In dit artikel bevat informatie over het SAML 2.0-verificatieaanvragen en -antwoorden die ondersteuning biedt voor eenmalige aanmelding voor Azure Active Directory (Azure AD).

Het onderstaande diagram kunt protocol beschrijft de volgorde voor eenmalige aanmelding. De cloudservice (de serviceprovider) maakt gebruik van een HTTP-omleiding binding om door te geven een `AuthnRequest` (verificatie) aanvraagelement naar Azure AD (de id-provider). Vervolgens Azure AD maakt gebruik van een HTTP post binding te posten een `Response` element met de cloudservice.

![Eenmalige aanmelding werkstroom](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## AuthnRequest
Om aan te vragen een gebruikersverificatie, cloud services-verzenden een `AuthnRequest` element naar Azure AD. Een voorbeeld van SAML 2.0 `AuthnRequest` kan moeten uitzien:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameter |  | Beschrijving |
| --- | --- | --- |
| Id |Vereist |Azure AD maakt gebruik van dit kenmerk voor het vullen van de `InResponseTo` kenmerk van het geretourneerde antwoord. ID moet niet beginnen met een getal, zodat een gemeenschappelijke strategie is een tekenreeks, zoals 'id' naar de tekenreeksweergave van een GUID toevoegen aan het begin. Bijvoorbeeld: `id6c1c178c166d486687be4aaf5e482730` is een geldige ID. |
| Versie |Vereist |Dit moet **2.0**. |
| IssueInstant |Vereist |Dit is een datum/tijd-tekenreeks met een UTC-waarde en [round trip-indeling ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD een DateTime-waarde van dit type verwacht maar niet evalueren of gebruik de waarde. |
| AssertionConsumerServiceUrl |Optioneel |Indien opgegeven, moet dit overeen met de `RedirectUri` van de cloudservice in Azure AD. |
| ForceAuthn |Optioneel | Dit is een Booleaanse waarde. Indien waar, betekent dit dat de gebruiker wordt geforceerd opnieuw worden geverifieerd, zelfs als ze een geldige sessie met Azure AD hebben. |
| IsPassive |Optioneel |Dit is een Booleaanse waarde die aangeeft of Azure AD moet worden geverifieerd achtergrond, zonder tussenkomst van de gebruiker met behulp van de sessiecookie als er een bestaat. Als dit waar is, probeert Azure AD te verifiëren van de gebruiker met behulp van de sessie-cookie. |

Alle andere `AuthnRequest` kenmerken, zoals toestemming, bestemming, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex en ProviderName zich **genegeerd**.

Azure AD ook negeert de `Conditions` -element in `AuthnRequest`.

### certificaatverlener
De `Issuer` -element in een `AuthnRequest` moet exact overeenkomen met een van de **ServicePrincipalNames** in de cloudservice in Azure AD. Normaal gesproken deze is ingesteld op de **App ID URI** die is opgegeven tijdens de toepassingsregistratie.

Een voorbeeld SAML fragment met de `Issuer` element uitziet:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### NameIDPolicy
Dit element vraagt de indeling van een bepaalde naam-ID in het antwoord en is optioneel in `AuthnRequest` elementen die worden verzonden naar Azure AD.

Een voorbeeld van een `NameIdPolicy` element uitziet:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Als `NameIDPolicy` is opgegeven, kunt u de optionele opnemen `Format` kenmerk. De `Format` kenmerk kan hebben slechts één van de volgende waarden; een andere waarde resulteert in een fout opgetreden.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: De claim NameID problemen azure Active Directory als een pairwise id.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory verleent de claim NameID in e-mailadres.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Met deze waarde is toegestaan voor Azure Active Directory om de indeling van de claim te selecteren. Azure Active Directory de NameID als een pairwise id uitgeeft.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: De claim NameID problemen azure Active Directory als een willekeurig gegenereerde waarde die uniek is voor de huidige SSO-bewerking. Dit betekent dat de waarde tijdelijk is en kan niet worden gebruikt om de verifiërende gebruiker te identificeren.

Azure AD negeert de `AllowCreate` kenmerk.

### RequestAuthnContext
De `RequestedAuthnContext` element Hiermee geeft u de gewenste verificatiemethoden. Dit is optioneel in `AuthnRequest` elementen die worden verzonden naar Azure AD. Azure AD ondersteunt slechts één `AuthnContextClassRef` waarde: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### Bereik
De `Scoping` -element een lijst met identiteitsproviders bevat, is optioneel in `AuthnRequest` elementen die worden verzonden naar Azure AD.

Indien opgegeven, bevatten geen de `ProxyCount` kenmerk `IDPListOption` of `RequesterID` element als ze worden niet ondersteund.

### Handtekening
Neem geen een `Signature` -element in `AuthnRequest` elementen, zoals Azure AD biedt geen ondersteuning voor ondertekend verificatieaanvragen.

### Onderwerp
Azure AD negeert de `Subject` element van `AuthnRequest` elementen.

## Antwoord
Wanneer een aangevraagde eenmalige aanmelding voltooid is, boekt Azure AD een reactie op de cloudservice. Een voorbeeldreactie op een poging voor geslaagde ziet er als volgt:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### Antwoord
De `Response` element het resultaat van de autorisatieaanvraag bevat. Azure AD-stelt de `ID`, `Version` en `IssueInstant` waarden in de `Response` element. Hiermee ook de volgende kenmerken:

* `Destination`: Wanneer de aanmelding is voltooid, wordt deze ingesteld op de `RedirectUri` van de serviceprovider (cloudservice).
* `InResponseTo`: Dit is ingesteld op de `ID` kenmerk van de `AuthnRequest` element dat het antwoord wordt gestart.

### certificaatverlener
Azure AD-stelt de `Issuer` element op de `https://login.microsoftonline.com/<TenantIDGUID>/` waar <TenantIDGUID> is de tenant-ID van de Azure AD-tenant.

Voor, een voorbeeldreactie met verlener element kan als volgt uitzien:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### Status
De `Status` element het slagen of mislukken van de aanmelding worden weergegeven. Het bevat de `StatusCode` element, dat een code of een set geneste codes die de status van de aanvraag bevat. Dit omvat ook de `StatusMessage` -element bevat aangepaste foutberichten die zijn gegenereerd tijdens de aanmelding.

<!-- TODO: Add a authentication protocol error reference -->

Hier volgt een SAML-reactie op een mislukte poging voor eenmalige aanmelding.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### Verklaring
Naast de `ID`, `IssueInstant` en `Version`, Azure AD Hiermee stelt u de volgende elementen in de `Assertion` element van het antwoord.

#### certificaatverlener
Dit is ingesteld op `https://sts.windows.net/<TenantIDGUID>/`waar <TenantIDGUID> is de Tenant-ID van de Azure AD-tenant.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### Handtekening
Azure AD ondertekent de bevestiging in reactie op een geslaagde aanmelding. De `Signature` element bevat een digitale handtekening die de cloudservice gebruiken kunt voor het verifiëren van de bron om te controleren of de integriteit van de bevestiging.

Voor het genereren van de digitale handtekening, Azure AD maakt gebruik van de ondertekeningssleutel in de `IDPSSODescriptor` element van het document met metagegevens.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### Onderwerp
Hiermee geeft u de principal die de houder van de instructies in de verklaring is. Het bevat een `NameID` element, dat de geverifieerde gebruiker vertegenwoordigt. De `NameID` waarde is een gerichte id waarnaar wordt doorverwezen alleen de serviceprovider die de doelgroep voor het token. Het is permanent - kunnen worden ingetrokken, maar nooit opnieuw wordt toegewezen. Het is ook ondoorzichtig, omdat het over de gebruiker niet vrijgegeven en kan niet worden gebruikt als een id voor kenmerk query's.

De `Method` kenmerk van de `SubjectConfirmation` element is altijd ingesteld op `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### Voorwaarden
Dit element bevat de voorwaarden die de aanvaardbaar gebruik van SAML-asserties definiëren.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

De `NotBefore` en `NotOnOrAfter` kenmerken Geef het interval op gedurende welke de verklaring ongeldig is.

* De waarde van de `NotBefore` kenmerk gelijk is aan of iets (minder dan een seconde) hoger is dan de waarde van `IssueInstant` kenmerk van de `Assertion` element. Azure AD wordt geen rekening gehouden met eventuele tijdsverschil tussen zichzelf en de cloudservice (serviceprovider) en wordt een buffer niet toegevoegd aan deze tijd.
* De waarde van de `NotOnOrAfter` kenmerk is 70 minuten later is dan de waarde van de `NotBefore` kenmerk.

#### Doelgroep
Dit document bevat een URI die een doelgroep identificeert. Azure AD wordt de waarde van dit element op de waarde van `Issuer` element van de `AuthnRequest` die de aanmelding hebt gestart. Om te evalueren de `Audience` waarde, gebruikt u de waarde van de `App ID URI` dat is opgegeven tijdens de toepassingsregistratie.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Als de `Issuer` waarde, de `Audience` waarde moet exact overeenkomen met een van de SPN-namen die staat voor de cloudservice in Azure AD. Echter, als de waarde van de `Issuer` element is niet een URI-waarde de `Audience` waarde in het antwoord is de `Issuer` waarde voorafgegaan door `spn:`.

#### AttributeStatement
Hierin worden claims over de onderwerpnaam of de gebruiker. Het volgende fragment bevat een voorbeeld van een `AttributeStatement` element. Het weglatingsteken geeft aan dat het element kan bestaan uit meerdere kenmerken en kenmerkwaarden.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```        

* **Naam van Claim** : de waarde van de `Name` kenmerk (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) is de UPN van de geverifieerde gebruiker, zoals `testuser@managedtenant.com`.
* **ObjectIdentifier Claim** : de waarde van de `ObjectIdentifier` kenmerk (`http://schemas.microsoft.com/identity/claims/objectidentifier`) is de `ObjectId` van de directoryobject dat de geverifieerde gebruiker in Azure AD vertegenwoordigt. `ObjectId`is een niet-wijzigbaar, globaal unieke en veilige id van de geverifieerde gebruiker opnieuw te gebruiken.

#### AuthnStatement
Dit element wordt gesteld dat het onderwerp verklaring is geverifieerd door een bepaalde manier op een bepaald moment.

* De `AuthnInstant` kenmerk geeft de tijd waarop de gebruiker is geverifieerd met Azure AD.
* De `AuthnContext` element geeft de verificatiecontext gebruikt om de gebruiker te verifiëren.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```