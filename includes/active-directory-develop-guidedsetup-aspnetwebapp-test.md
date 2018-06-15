---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7e9518f8a90faa0566b96d58992b01e4b0a642f4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32202719"
---
## <a name="test-your-code"></a>Testen van uw code

Testen van uw toepassing in Visual Studio, drukt u op **F5** aan uw project uitvoeren. De browser geopend met de http://<span></span>localhost: {poort} locatie en u ziet de **aanmelden met Microsoft** knop. Selecteer de knop om het proces aanmelden te starten.

Wanneer u klaar bent om uit te voeren van uw test, gebruiken een Microsoft Azure Active Directory (Azure AD)-account (werk of school-account) of een persoonlijk Microsoft-account (<span>live.</span> COM of <span>outlook.</span> com) aan te melden.

![Meld u aan met Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Aanmelden bij je Microsoft-account](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich aanmeldt, wordt de gebruiker wordt omgeleid naar de startpagina van uw website. De startpagina van is de HTTPS-URL die is opgegeven in uw toepassing registratie-informatie in de Registratieportal voor Microsoft-toepassing. De introductiepagina bevat een welkomstbericht *' Hello \<gebruiker >, '* een koppeling om af te melden en een koppeling om de claims van de gebruiker weer te geven. De koppeling voor de gebruiker claims bladert naar de *Claims* domeincontroller die u eerder hebt gemaakt.

### <a name="browse-to-see-the-users-claims"></a>Ga naar de claims van de gebruiker
Als u wilt zien van de gebruiker claims, selecteer de koppeling om te bladeren naar de weergave van de domeincontroller die is alleen beschikbaar voor geverifieerde gebruikers.

#### <a name="view-the-claims-results"></a>Bekijk de resultaten van claims
Nadat u de weergave controller bladert, ziet u een tabel met de belangrijkste eigenschappen voor de gebruiker:

|Eigenschap |Waarde |Beschrijving |
|---|---|---|
|**Naam** |Volledige naam van gebruiker | De gebruiker eerst de- en achternaam.
|**Gebruikersnaam** |Gebruiker<span>@domain.com</span> | De gebruikersnaam die wordt gebruikt om de gebruiker te identificeren.
|**Onderwerp** |Onderwerp |Een tekenreeks die een unieke identificatie van de gebruiker op het web.|
|**Tenant-ID** |GUID | Een **guid** die Azure AD-organisatie van de gebruiker een unieke vertegenwoordigt.|

Bovendien ziet u een tabel van alle claims die zich in de verificatieaanvraag. Zie voor meer informatie de [lijst met claims die zich in een Azure AD-ID Token](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Test de toegang tot een methode die een kenmerk autoriseren (optioneel)
Voor het testen van toegang als een anonieme gebruiker naar een domeincontroller die is beveiligd met de `Authorize` kenmerk, als volgt te werk:
1. Selecteer de koppeling naar de gebruiker afmelden en de afmelden proces te voltooien.
2. Typ in uw browser http://<span></span>localhost: {poort} / claims voor toegang tot uw domeincontroller die is beveiligd met de `Authorize` kenmerk.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Verwachte resultaten na toegang tot een beveiligde domeincontroller
U wordt gevraagd om te verifiëren voor de weergave beveiligde domeincontroller gebruiken.

## <a name="advanced-options"></a>Geavanceerde opties

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Beveiligen van uw gehele website
Beveiligen van uw gehele website in de **Global.asax** bestand, het toevoegen van de `AuthorizeAttribute` kenmerk de `GlobalFilters` filteren de `Application_Start` methode:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Beperken die zich aanmelden bij uw toepassing
Standaard bij het samenstellen van de toepassing gemaakt door deze handleiding uw toepassing accepteert de aanmelding modules van persoonlijke accounts (inclusief outlook.com, live.com en anderen), evenals werken en schoolaccounts zijn uit een bedrijf of organisatie die is geïntegreerd met Azure Active Directory. Dit is een aanbevolen optie voor SaaS-toepassingen.

Gebruiker aanmelden om toegang te beperken voor uw toepassing, zijn meerdere opties zijn beschikbaar:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Optie 1: Voorkomen dat gebruikers slechts één organisatie Active Directory-exemplaar aan te melden bij uw toepassing (single-tenant)

Deze optie is een veelvoorkomend scenario voor *LOB-toepassingen*: als u wilt dat uw toepassing accepteren aanmeldingen alleen van accounts die deel uitmaken van een specifiek exemplaar van Azure Active Directory (inclusief *gastaccounts*van dat exemplaar) het volgende doen:

1. In de **web.config** bestand, wijzig de waarde voor de `Tenant` parameter vanuit `Common` naar de tenantnaam van de organisatie, zoals `contoso.onmicrosoft.com`.
2. In uw [OWIN-Opstartklasse](#configure-the-authentication-pipeline)stelt de `ValidateIssuer` argument voor `true`.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>Optie 2: De toegang tot uw toepassing voor gebruikers in een specifieke lijst van organisaties beperken

U kunt-toegang beperken tot alleen gebruikersaccounts die in een Azure AD-organisatie die zich in de lijst met toegestane organisaties zijn:
1. In uw [OWIN-Opstartklasse](#configure-the-authentication-pipeline)stelt de `ValidateIssuer` argument voor `true`.
2. Stel de waarde van de `ValidIssuers` parameter aan de lijst met toegestane organisaties.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Optie 3: Een aangepaste methode gebruiken voor het valideren van de uitgevers van certificaten
U kunt implementeren een aangepaste methode voor het valideren van uitgevers van certificaten met behulp van de **IssuerValidator** parameter. Lees voor meer informatie over het gebruik van deze parameter over de [TokenValidationParameters klasse](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) op MSDN.

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
