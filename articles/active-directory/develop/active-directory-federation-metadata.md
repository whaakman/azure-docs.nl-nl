---
title: Azure AD-Federatiemetagegevens | Microsoft Docs
description: Dit artikel wordt het document met federatieve metagegevens die Azure Active Directory publiceert voor services die Azure Active Directory-tokens accepteert.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 58e5f62009e4e8b688108c6098ea8eabe8020e51
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="federation-metadata"></a>Federatieve metagegevens
Azure Active Directory (Azure AD) publiceert een document met federatieve metagegevens voor services die is geconfigureerd voor het accepteren van de beveiligingstokens die Azure AD geeft. De indeling van federatieve metagegevens document wordt beschreven in de [Web Services Federation Language (WS-Federation) versie 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), die breidt [metagegevens voor de OASIS Security Assertion Markup Language (SAML) 2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Tenant-specifieke en Tenant-onafhankelijke metagegevens-eindpunten
Azure AD publiceert-tenant-specifieke en tenant-onafhankelijke eindpunten.

Tenant-specifieke eindpunten zijn ontworpen voor een bepaalde tenant. De tenant-specifieke federatiemetagegevens bevat informatie over de tenant, met inbegrip van de verlener en eindpunt informatie tenantspecifieke. Toepassingen die toegang tot een enkel tenant beperken gebruiken tenantspecifieke eindpunten.

Tenant-onafhankelijke eindpunten bevatten informatie die geldt voor alle Azure AD-tenants. Deze informatie is van toepassing op tenants die worden gehost op *login.microsoftonline.com* en wordt gedeeld door tenants. Tenant-onafhankelijke eindpunten worden aanbevolen voor multitenant-toepassingen, omdat ze niet gekoppeld aan een bepaalde tenant zijn.

## <a name="federation-metadata-endpoints"></a>Federatieve metagegevens-eindpunten
Azure AD publiceert federatiemetagegevens op `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Voor **tenantspecifieke eindpunten**, wordt de `TenantDomainName` een van de volgende typen:

* Een geregistreerde domeinnaam van een Azure AD-tenant, zoals: `contoso.onmicrosoft.com`.
* De niet-wijzigbaar tenant-ID van het domein, zoals `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Voor **tenant-onafhankelijke eindpunten**, wordt de `TenantDomainName` is `common`. Dit document worden de Federatiemetagegevens-elementen die gemeenschappelijk zijn voor alle Azure AD-tenants die worden gehost op login.microsoftonline.com.

Bijvoorbeeld, een eindpunt tenantspecifieke mogelijk `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Het eindpunt van de tenant-onafhankelijke [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). U kunt het document met federatieve metagegevens weergeven door deze URL te typen in een browser.

## <a name="contents-of-federation-metadata"></a>Inhoud van de federatieve metagegevens
De volgende sectie bevat informatie die door services die de tokens die zijn uitgegeven door Azure AD gebruiken die nodig is.

### <a name="entity-id"></a>Entiteit-ID
De `EntityDescriptor` element bevat een `EntityID` kenmerk. De waarde van de `EntityID` kenmerk vertegenwoordigt de verlener, dat wil zeggen, de beveiligingstokenservice (STS) die het token heeft uitgegeven. Het is belangrijk om te valideren van de verlener wanneer u een token ontvangt.

De volgende metagegevens toont een voorbeeld van een tenantspecifieke `EntityDescriptor` element met een `EntityID` element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
U kunt de tenant-ID uit het eindpunt van de tenant-onafhankelijke vervangen door uw tenant-ID voor het maken van een tenantspecifieke `EntityID` waarde. De resulterende waarde is hetzelfde als de uitgever van het beveiligingstoken. De strategie kan een multitenant-toepassing voor het valideren van de verlener voor een bepaalde tenant.

De volgende metagegevens toont een voorbeeld van een tenant-onafhankelijke `EntityID` element. Houd er rekening mee dat de `{tenant}` is een letterlijke tekenreeks, niet een tijdelijke aanduiding.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Certificaten voor tokenondertekening
Wanneer een service een token dat is uitgegeven door een Azure AD-tenant ontvangt, kan de handtekening van het token moet worden gevalideerd met een ondertekeningssleutel die is gepubliceerd in het document met federatieve metagegevens. De federatiemetagegevens bevat voor het openbare deel van de certificaten die gebruikmaken van de tenants voor token-ondertekening. De onbewerkte bytes certificaat worden weergegeven in de `KeyDescriptor` element. Het token-ondertekening certificaat is geldig voor de ondertekening alleen wanneer de waarde van de `use` kenmerk `signing`.

Een document met federatieve metagegevens gepubliceerd door Azure AD kan meerdere handtekeningsleutels, zoals wanneer Azure AD wordt voorbereid om bij te werken van het handtekeningcertificaat hebben. Wanneer een document met federatieve metagegevens meer dan één certificaat bevat, moet een service die het valideren van de tokens ondersteuning voor alle certificaten in het document.

De volgende metagegevens toont een voorbeeld van een `KeyDescriptor` element met een ondertekeningssleutel.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

De `KeyDescriptor` element wordt weergegeven op twee plaatsen in het document met federatieve metagegevens; in de WS-Federation-specifieke en de SAML-specifieke-sectie. De certificaten die zijn gepubliceerd in beide secties wordt hetzelfde zijn.

In de sectie WS-Federation-specifieke leest een lezer WS-Federation-metagegevens van de certificaten van een `RoleDescriptor` element met de `SecurityTokenServiceType` type.

De volgende metagegevens toont een voorbeeld van een `RoleDescriptor` element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

In de sectie SAML-specifieke leest een lezer WS-Federation-metagegevens van de certificaten van een `IDPSSODescriptor` element.

De volgende metagegevens toont een voorbeeld van een `IDPSSODescriptor` element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Er zijn geen verschillen in de indeling van certificaten met tenantspecifieke en onafhankelijk van de tenant.

### <a name="ws-federation-endpoint-url"></a>WS-Federation-eindpunt-URL
De federatiemetagegevens bevat de URL die wordt gebruikt voor één aanmelding toe en één afmelden in het protocol WS-Federation Azure AD. Dit eindpunt wordt weergegeven in de `PassiveRequestorEndpoint` element.

De volgende metagegevens toont een voorbeeld van een `PassiveRequestorEndpoint` element voor een tenantspecifieke-eindpunt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Voor het eindpunt van de tenant-onafhankelijke, de WS-Federation-URL wordt weergegeven in de WS-Federation-eindpunt, zoals wordt weergegeven in het volgende voorbeeld.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>De eindpunt-URL voor SAML-protocol
De federatiemetagegevens bevat de URL die gebruikmaakt van Azure AD voor één aanmelding toe en één afmelden in SAML 2.0-protocol. Deze eindpunten worden weergegeven in de `IDPSSODescriptor` element.

De URL's voor aanmelden en afmelden worden weergegeven in de `SingleSignOnService` en `SingleLogoutService` elementen.

De volgende metagegevens toont een voorbeeld van een `PassiveResistorEndpoint` voor een tenantspecifieke-eindpunt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Op dezelfde manier worden de eindpunten voor de algemene SAML 2.0-protocoleindpunten worden gepubliceerd in de tenant-onafhankelijke federatiemetagegevens, zoals wordt weergegeven in het volgende voorbeeld.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
