---
title: Meer informatie over de verschillende token en claimtypen die worden ondersteund door Azure AD | Microsoft Docs
description: Een handleiding voor informatie over en evaluatie van de claims in het SAML 2.0 en JSON Web Tokens (JWT)-tokens die zijn uitgegeven door Azure Active Directory (AAD)
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 2c67e40cf540062aeadd533962c0fb296648fb86
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099999"
---
# <a name="azure-ad-saml-token-reference"></a>Azure AD-SAML-token naslaginformatie

Azure Active Directory (Azure AD) verzendt verschillende typen beveiligingstokens bij het verwerken van elke verificatiestroom. Dit document beschrijft de indeling, de beveiligingskenmerken en de inhoud van elk type token.

## <a name="claims-in-saml-tokens"></a>Claims in het SAML-tokens

> [!div class="mx-codeBreakAll"]
| Name | Equivalent JWT Claim | Beschrijving | Voorbeeld |
| --- | --- | --- | ------------|
|Doelgroep | `aud` |De beoogde ontvanger van het token. De toepassing die de token ontvangt moet controleren of de waarde van de doelgroep juist is en geen tokens die bestemd zijn voor een andere doelgroep afwijzen. | `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>`  |
| Verificatiemoment | |Registreert de datum en tijd waarop de verificatie heeft plaatsgevonden. | `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | 
|Verificatiemethode | `amr` |Hiermee geeft u op hoe de onderwerpnaam van het token is geverifieerd. | `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` |
|Voornaam | `given_name` |De eerste biedt of als u ' ' naam van de gebruiker, zoals ingesteld op de Azure AD-gebruiker-object. | `<Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">`<br>`<AttributeValue>Frank<AttributeValue>`  |
|Groepen | `groups` |Object-id's die staan voor groepslidmaatschappen van de certificaathouder biedt. Deze waarden zijn unieke (Zie de Object-ID) en veilig kunnen worden gebruikt voor het beheren van toegang, zoals het afdwingen van machtiging voor toegang tot een resource. De groepen die zijn opgenomen in de claim van groepen zijn geconfigureerd op basis van per toepassing, via de eigenschap 'groupMembershipClaims' van het toepassingsmanifest. Een null-waarde wordt alle groepen uitsluit, een waarde van 'Toewijzingsmodule' bevat alleen lidmaatschappen voor Active Directory-beveiligingsgroepen, en de waarde 'Alle' wordt opgenomen beveiligingsgroepen en distributielijsten van Office 365. <br><br> **Opmerkingen bij de**: <br> Als het nummer van de gebruiker zich in groepen via een limiet (150 voor SAML, 200 voor JWT gaat) en vervolgens een overschrijding claim wordt de claim bronnen aan te wijzen aan de Graph-eindpunt met de lijst met groepen voor de gebruiker worden toegevoegd. (in. | `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` |
| Overschrijding Indicator van groepen | `groups:src1` | Token aanvragen die niet beperkt de lengte zijn (Zie `hasgroups` hierboven), maar nog steeds te groot is voor het token, een koppeling naar de volledige lijst van de gebruiker worden opgenomen. Voor SAML wordt dit toegevoegd als een nieuwe claim in plaats van de `groups` claim. | `<Attribute Name=" http://schemas.microsoft.com/claims/groups.link">`<br>`<AttributeValue>https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects<AttributeValue>` |
|Identiteitsprovider | `idp` |Registreert de identiteitsprovider waarmee het onderwerp van het token is geverifieerd. Deze waarde is gelijk aan de waarde van de claim van verlener, tenzij het gebruikersaccount dat in een andere tenant dan de verlener is. | `<Attribute Name=" http://schemas.microsoft.com/identity/claims/identityprovider">`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` |
|IssuedAt | `iat` |Slaat de tijd waarop het token is uitgegeven. Dit wordt vaak gebruikt voor het meten van token webdocumenten. | `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` |
|Verlener | `iss` |Identificeert de beveiligingstokenservice (STS) die wordt gemaakt en het token retourneert. In de tokens die Azure AD als resultaat geeft, wordt de verlener sts.windows.net. De GUID in de uitgeverwaarde van de claim is de tenant-ID van de Azure AD-directory. De tenant-ID is een onveranderbare en betrouwbare id van de map. | `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` |
|Achternaam | `family_name` |Biedt de achternaam, achternaam, of familienaam van de gebruiker zoals gedefinieerd in de Azure AD-gebruiker-object. | `<Attribute Name=" http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">`<br>`<AttributeValue>Miller<AttributeValue>` |
|Name | `unique_name` |Biedt een voor mensen leesbare waarde waarmee het onderwerp van het token wordt geïdentificeerd. Deze waarde is niet noodzakelijkerwijs uniek zijn binnen een tenant en is ontworpen om te worden gebruikt alleen ter informatie weergegeven. | `<Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>`|
|Object-id | `oid` |Bevat een unieke id van een object in de Azure AD. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. De object-ID gebruiken om een object in query's naar Azure AD te identificeren. | `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` |
|Rollen | `roles` |Hiermee geeft u alle toepassingsrollen die het onderwerp heeft gekregen zowel direct als indirect via een groepslidmaatschap en kan worden gebruikt om af te dwingen op rollen gebaseerd toegangsbeheer. Toepassingsrollen zijn gedefinieerd op basis van per toepassing via de `appRoles` eigenschap van het toepassingsmanifest. De `value` eigenschap van de toepassingsrol van elke is de waarde die wordt weergegeven in de rollenclaim. | `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`|
|Onderwerp | `sub` |Hiermee geeft u de principal waarover het token worden bevestigd met gegevens, zoals de gebruiker van een toepassing. Deze waarde is onveranderbaar en kan niet opnieuw worden toegewezen of opnieuw worden gebruikt, zodat deze kan worden gebruikt om autorisatie controles veilig uitvoeren. Omdat het onderwerp altijd in de tokens die de problemen met Azure AD aanwezig is, wordt u aangeraden deze waarde in een systeem van de autorisatie voor algemene doeleinden te gebruiken. <br> `SubjectConfirmation` is niet een claim. Hierin wordt beschreven hoe het onderwerp van het token is geverifieerd. `Bearer` Geeft aan dat het onderwerp wordt bevestigd door iemand met kennis van het token. | `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>`|
|Tenant-id | `tid` |Een onveranderbare, herbruikbare-id waarmee de directory-tenant die het token is uitgegeven. U kunt deze waarde gebruiken voor toegang tot mapresources van de tenant-specifieke in een toepassing met meerdere tenants. Bijvoorbeeld, kunt u deze waarde om de tenant in een aanroep van de Graph API te identificeren. | `<Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>`|
|Levensduur van token | `nbf`, `exp` |Definieert gedurende welke periode een token geldig is. De service die het token valideert moet controleren of dat de huidige datum valt binnen de levensduur van tokens, anders die wordt het token moet afwijzen. De service mogelijk voor maximaal vijf minuten buiten het bereik van de levensduur van vernieuwingstoken ter compensatie van eventuele verschillen in clock-tijd ("tijd scheeftrekken') tussen Azure AD en de service. | `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br>|

## <a name="sample-saml-token"></a>Voorbeeld van SAML-Token

Dit is een voorbeeld van een typische SAML-token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="https://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="https://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="https://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="https://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

## <a name="related-content"></a>Gerelateerde inhoud
* Zie de Azure AD Graph [beleidsbewerkingen](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) en de [beleidsentiteit](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), voor meer informatie over het beheren van de levensduur van vernieuwingstoken beleid via de Azure AD Graph API.
* Zie voor meer informatie over en voorbeelden over het beheren van beleid via PowerShell-cmdlets, waaronder voorbeelden, [configureerbare levensduur van tokens in Azure AD](active-directory-configurable-token-lifetimes.md). 
* Voeg [aangepast en een optionele claims](active-directory-optional-claims.md) naar de tokens voor uw toepassing.
* Gebruik [eenmalige aanmelding (SSO) met SAML](single-sign-on-saml-protocol.md).
* Gebruik de [Azure eenmalige aanmelding van SAML-Protocol](single-sign-out-saml-protocol.md)
