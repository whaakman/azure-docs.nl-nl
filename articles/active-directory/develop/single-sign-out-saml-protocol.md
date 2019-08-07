---
title: Azure-SAML-protocol voor eenmalige afmelding | Microsoft Docs
description: In dit artikel wordt het SAML-protocol voor eenmalige afmeldingen in Azure Active Directory beschreven
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/19/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66c509b1b901889241d6837611a2c373750fdb3a
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68834790"
---
# <a name="single-sign-out-saml-protocol"></a>SAML-protocol voor eenmalige afmelding

Azure Active Directory (Azure AD) ondersteunt de SAML 2,0-webbrowser voor eenmalige afmelding. Voor een juiste werking van eenmalige afmelding moet de **LogoutURL** voor de toepassing expliciet worden geregistreerd bij Azure ad tijdens de registratie van de toepassing. Azure AD gebruikt de LogoutURL om gebruikers te omleiden nadat ze zijn afgemeld.

In het volgende diagram ziet u de werk stroom van het proces voor eenmalige aanmelding van Azure AD.

![Azure AD-werk stroom voor eenmalige afmelding](./media/single-sign-out-saml-protocol/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest
De Cloud service stuurt een `LogoutRequest` bericht naar Azure AD om aan te geven dat een sessie is beëindigd. Het volgende fragment toont een voorbeeld `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest
Het `LogoutRequest` element dat naar Azure AD wordt verzonden, vereist de volgende kenmerken:

* `ID`-Hiermee wordt de afmeldings aanvraag geïdentificeerd. De waarde van `ID` mag niet beginnen met een getal. De gang bare procedure is het toevoegen van **id** aan de teken reeks representatie van een GUID.
* `Version`-Stel de waarde van dit element in op **2,0**. Deze waarde is vereist.
* `IssueInstant`: Dit is een `DateTime` teken reeks met een UTC-waarde (Coordinate Universal Time) en een notatie voor de [retour Tour ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD verwacht een waarde van dit type, maar dwingt dit niet af.

### <a name="issuer"></a>Certificaatverlener
Het `Issuer` element in een `LogoutRequest` moet exact overeenkomen met een van de **ServicePrincipalNames** in de Cloud service in azure AD. Dit is normaal gesp roken ingesteld op de **App-ID-URI** die is opgegeven tijdens de registratie van de toepassing.

### <a name="nameid"></a>Meid
De waarde van het `NameID` element moet exact overeenkomen `NameID` met de van de gebruiker die wordt afgemeld.

## <a name="logoutresponse"></a>LogoutResponse
Azure AD stuurt een `LogoutResponse` als-antwoord naar `LogoutRequest` een-element. Het volgende fragment toont een voor `LogoutResponse`beeld.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse
Azure AD stelt de `ID`en `Version` `IssueInstant` waarden in het `LogoutResponse` -element in. Het `InResponseTo` element wordt ook ingesteld op de waarde `ID` van het kenmerk van `LogoutRequest` de die het antwoord heeft gereageerd.

### <a name="issuer"></a>Certificaatverlener
Azure AD stelt deze waarde in `https://login.microsoftonline.com/<TenantIdGUID>/` op \<waar TenantIdGUID > de Tenant-id is van de Azure AD-Tenant.

Als u de waarde van het `Issuer` element wilt evalueren, gebruikt u de waarde van de **App-ID-URI** die is geleverd tijdens de registratie van de toepassing.

### <a name="status"></a>Status
Azure AD gebruikt het `StatusCode` element in het `Status` -element om aan te geven dat de afmelding is geslaagd of mislukt. Wanneer de aanmeldings poging mislukt, kan het `StatusCode` element ook aangepaste fout berichten bevatten.
