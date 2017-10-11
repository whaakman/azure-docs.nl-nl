---
title: Azure AD v2 ASP.NET Web Server ophalen gestart - testen | Microsoft Docs
description: Implementeren van Microsoft-aanmeldingspagina op een ASP.NET-oplossing met een traditioneel browser gebaseerde webtoepassing met behulp van standaard OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 00cb963e85111274c36c3a84489894811ad2dabd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Testen van uw code

Druk op `F5` uw project in Visual Studio wilt uitvoeren. De browser wordt geopend en u directe *http://localhost: {poort}* Hier ziet u de *aanmelden met Microsoft* knop. Opwekken en klik op aan te melden.

Wanneer u klaar bent om te testen, gebruikt u een werk- of schoolaccount (Azure Active Directory) of een persoonlijke (live.com, outlook.com)-account aan te melden. 

![Aanmelden met Microsoft-browservenster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Aanmelden met Microsoft-browservenster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Verwachte resultaten
Na het aanmelden, wordt de gebruiker omgeleid naar de startpagina van de website die de HTTPS-URL die is opgegeven in uw toepassing registratie-informatie in de Registratieportal voor Microsoft-toepassing. Deze pagina wordt nu *Hallo {User}* en een koppeling naar afmelden en een koppeling om te zien van claims van de gebruiker – dit is een koppeling naar de controller autoriseren eerder hebt gemaakt.

### <a name="see-users-claims"></a>Zie de claims van de gebruiker
Selecteer de hyperlink om te zien van claims van de gebruiker. Dit leidt u naar de domeincontroller en de weergave die alleen beschikbaar voor gebruikers die zijn geverifieerd.

#### <a name="expected-results"></a>Verwachte resultaten
 Hier ziet u een tabel met de basiseigenschappen van de aangemelde gebruiker:

| Eigenschap | Waarde | Beschrijving|
|---|---|---|
| Naam | {De volledige gebruikersnaam} | De voor- en achternaam van de gebruiker
|Gebruikersnaam | <span>user@domain.com</span>| De gebruikersnaam die wordt gebruikt voor het identificeren van de aangemelde gebruiker
| Onderwerp| {Onderwerp}|Een tekenreeks als unieke identificatie van de aanmelding van de gebruiker op het web|
| Tenant-id| {Guid}| Een *guid* uniek vertegenwoordigt de organisatie Azure Active Directory van de gebruiker.|

Bovendien ziet u een tabel met inbegrip van alle gebruikersclaims in verificatieaanvraag opgenomen. Voor een lijst van alle claims in een Token Id en de uitleg raadpleegt u dit [artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lijst van Claims in Token Id").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test een methode die heeft toegang tot een *[autoriseren]* kenmerk (optioneel)
In deze stap wordt u de toegang tot de controller geverifieerd als anonieme gebruiker testen:<br/>
Selecteer de koppeling voor afmelden van de gebruiker en de afmelden proces te voltooien.<br/>
Typ nu in uw browser http://localhost: {poort} / geverifieerde toegang tot uw domeincontroller die is beveiligd met de `[Authorize]` kenmerk

#### <a name="expected-results"></a>Verwachte resultaten
U ontvangt de prompt dat u hoeft te worden geverifieerd om te zien van de weergave.

## <a name="additional-information"></a>Aanvullende informatie

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Beveiligen van uw hele website
Omwille van de gehele website toevoegen de `AuthorizeAttribute` naar `GlobalFilters` in `Global.asax` `Application_Start` methode:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Hoe kunt u voorkomen dat gebruikers slechts één organisatie aan te melden bij uw toepassing**

> Standaard persoonlijke accounts (inclusief outlook.com, live.com en anderen), evenals werken en schoolaccounts van een bedrijf of organisatie die is geïntegreerd met Azure Active Directory kunnen aanmelden bij uw toepassing. 

> Als u wilt dat uw toepassing te accepteren aanmeldingen van slechts één Azure Active Directory-organisatie, vervangt u de `Tenant` parameter in *web.config* van `Common` naar de tenantnaam van de organisatie – bijvoorbeeld *contoso.onmicrosoft.com*. Daarna wijzigen de `ValidateIssuer` argument in uw *OWIN-Opstartklasse* naar `true`.

> Instellen zodat gebruikers alleen een lijst met specifieke organisaties `ValidateIssuer` op waar en gebruik de `ValidIssuers` parameter om een lijst met organisaties.

> Er is een andere optie voor het implementeren van een aangepaste methode voor het valideren van de uitgevers van certificaten met de parameter IssuerValidator. Voor meer informatie over `TokenValidationParameters`, Zie [dit](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-artikel") MSDN-artikel.

