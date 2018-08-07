---
title: Azure eenmalige aanmelding van SAML-Protocol | Microsoft Docs
description: Dit artikel wordt beschreven voor het Protocol voor eenmalige afmelding SAML in Azure Active Directory
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
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
ms.openlocfilehash: b1c0dddec93e913e543558893979b95c8f53ad47
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581392"
---
# <a name="single-sign-out-saml-protocol"></a>Eenmalige afmelding SAML-Protocol

Azure Active Directory (Azure AD) ondersteunt het SAML 2.0 web browser-profiel voor eenmalige afmelding. Voor eenmalige afmelding goed, de **LogoutURL** voor de toepassing moet expliciet worden geregistreerd bij Azure AD tijdens de toepassingsregistratie. Azure AD maakt gebruik van de LogoutURL om te leiden van gebruikers nadat ze bent afgemeld.

Het volgende diagram toont de werkstroom van de Azure AD proces voor eenmalige afmelding.

![Azure AD-eenmalige aanmelding van de werkstroom](./media/single-sign-out-saml-protocol/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest
De cloud service verzendt een `LogoutRequest` bericht naar Azure AD om aan te geven dat een sessie is beëindigd. Het volgende fragment toont een voorbeeld van een `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest
De `LogoutRequest` verzonden naar Azure AD-element vereist de volgende kenmerken:

* `ID` -Hiermee wordt aangegeven met de aanvragen voor afmelden. De waarde van `ID` mogen niet beginnen met een cijfer. De gebruikelijke procedure is om toe te voegen **id** naar de tekenreeksweergave van een GUID.
* `Version` -Hiermee stelt u de waarde van dit element aan **2.0**. Deze waarde is verplicht.
* `IssueInstant` -Dit is een `DateTime` tekenreeks met een waarde voor de coördinatie van Universal Time (UTC) en [traject indeling ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD een waarde van dit type verwacht, maar deze niet afdwingen.

### <a name="issuer"></a>Certificaatverlener
De `Issuer` -element in een `LogoutRequest` moet exact overeenkomen met een van de **ServicePrincipalNames** in de cloudservice in Azure AD. Meestal dit is ingesteld op de **App ID URI** die is opgegeven tijdens de toepassingsregistratie.

### <a name="nameid"></a>NameID
De waarde van de `NameID` element moet exact overeenkomen met de `NameID` van de gebruiker die wordt afgemeld.

## <a name="logoutresponse"></a>LogoutResponse
Azure AD-verzendt een `LogoutResponse` in reactie op een `LogoutRequest` element. Het volgende fragment toont een voorbeeld van een `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse
Azure AD-stelt de `ID`, `Version` en `IssueInstant` waarden in de `LogoutResponse` element. Stelt ook het `InResponseTo` element aan de waarde van de `ID` kenmerk van de `LogoutRequest` die opgewekt door het antwoord.

### <a name="issuer"></a>Certificaatverlener
Azure AD wordt deze waarde ingesteld op `https://login.microsoftonline.com/<TenantIdGUID>/` waar <TenantIdGUID> is de tenant-ID van de Azure AD-tenant.

Om te evalueren van de waarde van de `Issuer` -element, gebruikt u de waarde van de **App ID URI** opgegeven tijdens de toepassingsregistratie.

### <a name="status"></a>Status
Azure AD gebruikt de `StatusCode` -element in de `Status` element om aan te geven van het slagen of mislukken van afmelding. Wanneer de afmelding poging mislukt, de `StatusCode` -element kan ook aangepaste foutberichten bevatten.
