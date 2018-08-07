---
title: Azure voor eenmalige aanmelding SAML-Protocol | Microsoft Docs
description: In dit artikel beschrijft de eenmalige aanmelding op SAML-protocol in Azure Active Directory
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ad8437f5-b887-41ff-bd77-779ddafc33fb
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: edb8ae501548775932a259621c19acece474018d
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581330"
---
# <a name="single-sign-on-saml-protocol"></a>Protocol voor eenmalige SAML-aanmelding

In dit artikel bevat informatie over het SAML 2.0-verificatieaanvragen en antwoorden die ondersteuning biedt voor eenmalige aanmelding voor Azure Active Directory (Azure AD).

In het onderstaande diagram voor protocol beschrijft de updatevolgorde voor eenmalige aanmelding. De service in de cloud (de serviceprovider) maakt gebruik van een HTTP-omleiding-binding om door te geven een `AuthnRequest` -element (verificatieaanvraag) naar Azure AD (de id-provider). Vervolgens Azure AD maakt gebruik van een HTTP post binden aan het bericht een `Response` element met de cloudservice.

![Werkstroom voor eenmalige aanmelding](./media/single-sign-on-saml-protocol/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Om aan te vragen een gebruikersverificatie, cloud services-verzenden een `AuthnRequest` element naar Azure AD. Een voorbeeld van een SAML 2.0 `AuthnRequest` kan eruitzien als in het volgende voorbeeld:

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
| Id | Vereist | Azure AD maakt gebruik van dit kenmerk voor het vullen van de `InResponseTo` kenmerk van het geretourneerde antwoord. ID moet niet beginnen met een getal, zodat een algemene strategie bestaat uit een tekenreeks, zoals "id" aan de tekenreeksweergave van een GUID toevoegen aan het begin. Bijvoorbeeld, `id6c1c178c166d486687be4aaf5e482730` is een geldige ID. |
| Versie | Vereist | Deze parameter moet worden ingesteld op **2.0**. |
| IssueInstant | Vereist | Dit is een datum/tijd-tekenreeks met een UTC-waarde en [traject indeling ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD wordt verwacht dat een datum / tijdwaarde van dit type is, maar niet evalueren of gebruik de waarde. |
| AssertionConsumerServiceUrl | Optioneel | Indien opgegeven, deze parameter moet overeenkomen met de `RedirectUri` van de cloudservice in Azure AD. |
| ForceAuthn | Optioneel | Dit is een Booleaanse waarde. Indien waar, betekent dit dat de gebruiker wordt geforceerd opnieuw worden geverifieerd, zelfs als er een geldige sessie met Azure AD. |
| IsPassive | Optioneel | Dit is een Booleaanse waarde waarmee wordt aangegeven of Azure AD de gebruiker moet verifiëren op de achtergrond, zonder tussenkomst van de gebruiker, met behulp van de sessiecookie als er een bestaat. Als dit correct is, probeert Azure AD te verifiëren van de gebruiker met behulp van de sessie-cookie. |

Alle andere `AuthnRequest` kenmerken, zoals toestemming, doel, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex en ProviderName **genegeerd**.

Azure AD ook negeert de `Conditions` -element in `AuthnRequest`.

### <a name="issuer"></a>Certificaatverlener

De `Issuer` -element in een `AuthnRequest` moet exact overeenkomen met een van de **ServicePrincipalNames** in de cloudservice in Azure AD. Meestal dit is ingesteld op de **App ID URI** die is opgegeven tijdens de toepassingsregistratie.

Een SAML-fragment met de `Issuer` element ziet eruit als in het volgende voorbeeld:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Dit element vraagt de indeling van een bepaalde naam-ID in het antwoord en is optioneel in `AuthnRequest` elementen naar Azure AD verzonden.

Een `NameIdPolicy` element ziet eruit als in het volgende voorbeeld:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Als `NameIDPolicy` is opgegeven, kunt u de optionele opnemen `Format` kenmerk. De `Format` kenmerk kan hebben slechts één van de volgende waarden; eventuele andere waarde resulteert in een fout.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: De claim NameID problemen azure Active Directory als pairwise-id.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory problemen met de claim NameID in e-mailadres.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Deze waarde kan Azure Active Directory om de indeling van de claim te selecteren. Azure Active Directory de NameID als een pairwise id uitgeeft.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: De claim NameID problemen azure Active Directory als een willekeurig gegenereerde waarde die uniek is voor de huidige bewerking voor eenmalige aanmelding. Dit betekent dat de waarde tijdelijk is en kan niet worden gebruikt om de verifiërende gebruiker te identificeren.

Azure AD negeert de `AllowCreate` kenmerk.

### <a name="requestauthncontext"></a>RequestAuthnContext
De `RequestedAuthnContext` element Hiermee geeft u de gewenste verificatiemethoden. Dit is optioneel in `AuthnRequest` elementen naar Azure AD verzonden. Azure AD biedt ondersteuning voor slechts één `AuthnContextClassRef` waarde: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Bereik
De `Scoping` -element, waaronder een lijst met id-providers, is optioneel in `AuthnRequest` elementen naar Azure AD verzonden.

Indien opgegeven, zijn niet opgenomen de `ProxyCount` kenmerk `IDPListOption` of `RequesterID` -element, zoals ze worden niet ondersteund.

### <a name="signature"></a>Handtekening
Neem geen een `Signature` -element in `AuthnRequest` elementen, zoals Azure AD biedt geen ondersteuning voor ondertekende aanvragen voor authenticatie.

### <a name="subject"></a>Onderwerp
Azure AD negeert de `Subject` element van `AuthnRequest` elementen.

## <a name="response"></a>Antwoord
Als een aangevraagde aanmelding is voltooid, wordt Azure AD een antwoord naar de cloudservice. Een reactie op een geslaagde poging aanmelding lijkt op het volgende voorbeeld:

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

### <a name="response"></a>Antwoord

De `Response` element bevat het resultaat van de autorisatieaanvraag. Azure AD-stelt de `ID`, `Version` en `IssueInstant` waarden in de `Response` element. Het stelt ook de volgende kenmerken:

* `Destination`: Wanneer de aanmelding is voltooid, dit is ingesteld op de `RedirectUri` van de serviceprovider (cloudservice).
* `InResponseTo`: Dit is ingesteld op de `ID` kenmerk van de `AuthnRequest` element dat het antwoord wordt gestart.

### <a name="issuer"></a>Certificaatverlener

Azure AD stelt de `Issuer` element `https://login.microsoftonline.com/<TenantIDGUID>/` waar <TenantIDGUID> is de tenant-ID van de Azure AD-tenant.

Bijvoorbeeld, kan een antwoord met de uitgever van element eruitzien als in het volgende voorbeeld:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Status

De `Status` element geeft het slagen of mislukken van aanmelding. Het bevat de `StatusCode` element, dat een code of een set van geneste codes die de status van de aanvraag bevat. Dit omvat ook de `StatusMessage` element, dat bevat aangepaste foutberichten die zijn gegenereerd tijdens de aanmelding.

<!-- TODO: Add a authentication protocol error reference -->

Het volgende voorbeeld wordt een SAML-reactie op een mislukte poging aanmelding.

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

### <a name="assertion"></a>bevestiging

Naast de `ID`, `IssueInstant` en `Version`, Azure AD Hiermee stelt u de volgende elementen in de `Assertion` element van het antwoord.

#### <a name="issuer"></a>Certificaatverlener

Deze optie is ingesteld op `https://sts.windows.net/<TenantIDGUID>/`waar <TenantIDGUID> is de Tenant-ID van de Azure AD-tenant.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Handtekening

Azure AD zich de verklaring in reactie op een geslaagde aanmelding. De `Signature` element bevat een digitale handtekening die de service in de cloud gebruiken kunt om te verifiëren van de bron om te controleren of de integriteit van de verklaring.

Voor het genereren van de digitale handtekening, Azure AD maakt gebruik van de ondertekeningssleutel in de `IDPSSODescriptor` -element van de metadata-document.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Onderwerp

Hiermee geeft u de principal die het onderwerp van de instructies in de verklaring. Deze bevat een `NameID` element, dat staat voor de geverifieerde gebruiker. De `NameID` waarde is een gerichte-id die alleen met de serviceprovider is de doelgroep voor het token wordt omgeleid. Het is permanent - deze kan worden ingetrokken, maar nooit opnieuw wordt toegewezen. Het is ook ondoorzichtig, omdat het niet. over de gebruiker vrijgegeven en kan niet worden gebruikt als een id voor kenmerk query's.

De `Method` kenmerk van de `SubjectConfirmation` element is altijd ingesteld op `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Voorwaarden

Dit element Hiermee geeft u het gebruik van SAML-asserties ondertekend voorwaarden.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

De `NotBefore` en `NotOnOrAfter` kenmerken opgeven voor het interval gedurende welke de verklaring geldig is.

* De waarde van de `NotBefore` kenmerk gelijk is aan of iets (minder dan een seconde) hoger is dan de waarde van `IssueInstant` kenmerk van de `Assertion` element. Azure AD geen rekening gehouden met eventuele tijdverschil zelf en de cloudservice (serviceprovider), en wordt een buffer niet toegevoegd aan dit moment.
* De waarde van de `NotOnOrAfter` kenmerk is 70 minuten hoger is dan de waarde van de `NotBefore` kenmerk.

#### <a name="audience"></a>Doelgroep

Dit document bevat een URI die een doelgroep identificeert. Azure AD stelt de waarde van dit element op de waarde van `Issuer` element van de `AuthnRequest` die de aanmelding hebt gestart. Om te evalueren de `Audience` waarde, gebruikt u de waarde van de `App ID URI` dat is opgegeven tijdens de toepassingsregistratie.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Net als de `Issuer` waarde, de `Audience` waarde moet exact overeenkomen met een van de SPN-namen die staat voor de cloudservice in Azure AD. Echter, als de waarde van de `Issuer` element is niet een URI-waarde, de `Audience` waarde in het antwoord is de `Issuer` waarde voorafgegaan door `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Deze bevat claims over de onderwerp of de gebruiker. Het volgende fragment bevat een voorbeeld van een `AttributeStatement` element. Het weglatingsteken geeft aan dat het element meerdere kenmerken en waarden van kenmerken bevatten kan.

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

* **Naam van Claim** -de waarde van de `Name` kenmerk (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) is de UPN van de geverifieerde gebruiker, zoals `testuser@managedtenant.com`.
* **ObjectIdentifier Claim** -de waarde van de `ObjectIdentifier` kenmerk (`http://schemas.microsoft.com/identity/claims/objectidentifier`) is de `ObjectId` van het directoryobject dat staat voor de geverifieerde gebruiker in Azure AD. `ObjectId` is een onveranderbare, unieke en veilige id van de geverifieerde gebruiker opnieuw gebruiken.

#### <a name="authnstatement"></a>AuthnStatement

Dit element wordt gesteld dat het onderwerp van de verklaring is geverifieerd op een bepaalde manier op een bepaald tijdstip.

* De `AuthnInstant` kenmerk specificeert het tijdstip waarop de gebruiker wordt geverifieerd met Azure AD.
* De `AuthnContext` element geeft u de verificatiecontext gebruikt voor het verifiëren van de gebruiker.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
