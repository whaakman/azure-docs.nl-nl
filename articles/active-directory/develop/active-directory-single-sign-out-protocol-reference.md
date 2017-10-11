---
title: Azure eenmalige van SAML-Protocol | Microsoft Docs
description: "Dit artikel wordt beschreven voor het één Sign-Out SAML-Protocol in Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 45e4705f53d80b5fe852c484b5e64d18a8e24f09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# Één afmelden SAML-Protocol
Azure Active Directory (Azure AD) ondersteunt het SAML 2.0 web browser-profiel op één afmelding plaatsvindt. Voor één afmeldingen via correcte werking moet de **LogoutURL** voor de toepassing moet expliciet worden geregistreerd bij Azure AD tijdens de toepassingsregistratie. Azure AD gebruikt de LogoutURL om gebruikers te leiden nadat ze zich afgemeld.

Dit diagram toont de werkstroom van de Azure AD enkel afmelden proces.

![Eenmalige aanmelding van de werkstroom](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest
De cloud service verzendt een `LogoutRequest` bericht naar Azure AD om aan te geven dat een sessie is beëindigd. Het volgende fragment toont een voorbeeld van een `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest
De `LogoutRequest` element verzonden naar Azure AD vereist de volgende kenmerken:

* `ID`: Hiermee wordt de afmelden aanvraag geïdentificeerd. De waarde van `ID` mogen niet beginnen met een getal. De gebruikelijke manier is om toe te voegen **id** als de tekenreeksweergave van een GUID.
* `Version`: Stel de waarde van dit element kunt **2.0**. Deze waarde is verplicht.
* `IssueInstant`: Dit is een `DateTime` tekenreeks met een waarde coördineren Universal Time (UTC) en [round trip-indeling ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD een waarde van dit type verwacht, maar worden niet afgedwongen.

### certificaatverlener
De `Issuer` -element in een `LogoutRequest` moet exact overeenkomen met een van de **ServicePrincipalNames** in de cloudservice in Azure AD. Normaal gesproken deze is ingesteld op de **App ID URI** die is opgegeven tijdens de toepassingsregistratie.

### NameID
De waarde van de `NameID` element moet exact overeenkomen met de `NameID` van de gebruiker die wordt afgemeld.

## LogoutResponse
Azure AD-verzendt een `LogoutResponse` in reactie op een `LogoutRequest` element. Het volgende fragment toont een voorbeeld van een `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse
Azure AD-stelt de `ID`, `Version` en `IssueInstant` waarden in de `LogoutResponse` element. Stelt ook de `InResponseTo` element met de waarde van de `ID` kenmerk van de `LogoutRequest` die opgewekt door het antwoord.

### certificaatverlener
Azure AD wordt deze waarde ingesteld op `https://login.microsoftonline.com/<TenantIdGUID>/` waar <TenantIdGUID> is de tenant-ID van de Azure AD-tenant.

Evalueren van de waarde van de `Issuer` element, gebruik de waarde van de **App ID URI** opgegeven tijdens de toepassingsregistratie.

### Status
Azure AD gebruikt de `StatusCode` -element in de `Status` element om aan te geven voor het slagen of mislukken van afmelding plaatsvindt. Bij een poging tot het afmelden is mislukt, de `StatusCode` -element kan ook aangepaste foutberichten bevatten.