---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 3a141bcde75872f2384aedf982ffef5cba9666a3
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843367"
---
## <a name="test-your-code"></a>Testen van uw code

Als u wilt testen van uw toepassing in Visual Studio, drukt u op **F5** om uit te voeren van uw project. De browser wordt geopend op de http://<span></span>localhost: {poort} locatie en u ziet de **aanmelden bij Microsoft** knop. Selecteer de knop om de aanmelding-proces te starten.

Wanneer u klaar bent om uw test uitvoert, een Microsoft Azure Active Directory (Azure AD)-account (werk of school-account) of een persoonlijk Microsoft-account gebruiken (<span>live.</span> COM of <span>outlook.</span> com) aan te melden.

![Aanmelden bij Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Meld u aan bij uw Microsoft-account](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich hebt aangemeld, wordt de gebruiker wordt omgeleid naar de startpagina van uw website. De startpagina wordt de HTTPS-URL die is opgegeven in de registratiegegevens van uw toepassing in de Portal voor App-registratie van Microsoft. De introductiepagina bevat een welkomstbericht *' Hello \<gebruiker >, "* een koppeling naar het afmelden, en een koppeling om claims van de gebruiker weer te geven. U gaat de koppeling voor claims van de gebruiker naar de *Claims* controller die u eerder hebt gemaakt.

### <a name="browse-to-see-the-users-claims"></a>Ga naar de claims van de gebruiker
Als u wilt zien van de gebruiker claims, selecteer de koppeling om te bladeren naar de weergave van de domeincontroller die is alleen beschikbaar voor geverifieerde gebruikers.

#### <a name="view-the-claims-results"></a>De claims-resultaten weergeven
Nadat u naar de weergave van de controller bladert, ziet u een tabel met de belangrijkste eigenschappen voor de gebruiker:

|Eigenschap |Waarde |Beschrijving |
|---|---|---|
|**Naam** |Volledige naam van gebruiker | En achternaam van eerst de gebruiker.
|**Gebruikersnaam** |Gebruiker<span>@domain.com</span> | De gebruikersnaam die wordt gebruikt om de gebruiker te identificeren.
|**Onderwerp** |Onderwerp |Een tekenreeks die een unieke identificatie van de gebruiker op het web.|
|**Tenant-ID** |GUID | Een **guid** die Azure AD-organisatie van de gebruiker een unieke vertegenwoordigt.|

Bovendien ziet u een tabel met alle claims die zich in de verificatieaanvraag. Zie voor meer informatie de [lijst met claims die zich in een Azure AD-ID-Token](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Test de toegang tot een methode die een kenmerk autoriseren (optioneel)
Voor het testen van toegang als een anonieme gebruiker naar een domeincontroller die is beveiligd met de `Authorize` kenmerk, als volgt te werk:
1. Selecteer de koppeling naar de gebruiker afmelden en de afmelding proces te voltooien.
2. Typ in de browser http://<span></span>localhost: {poort} / claims voor toegang tot uw domeincontroller die is beveiligd met de `Authorize` kenmerk.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Verwachte resultaten na de toegang tot een beveiligde domeincontroller
U wordt gevraagd om te verifiëren voor het gebruik van de weergave beveiligde domeincontroller.

## <a name="advanced-options"></a>Geavanceerde opties

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Beveiligen van uw hele website
Ter bescherming van uw hele website in de **Global.asax** bestand, voeg de `AuthorizeAttribute` kenmerk aan de `GlobalFilters` filteren de `Application_Start` methode:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Beperken die kunnen zich aanmelden bij uw toepassing
Standaard als u bij het maken van de toepassing hebt gemaakt in deze handleiding, uw toepassing accepteert aanmeldingen van persoonlijke accounts (waaronder outlook.com, live.com en andere) evenals werk en schoolaccounts zijn uit een bedrijf of organisatie die is geïntegreerd met Azure Active Directory. Dit is een aanbevolen optie voor SaaS-toepassingen.

Gebruiker aanmelden om toegang te beperken voor uw toepassing, zijn er meerdere opties beschikbaar:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Optie 1: Voorkomen dat gebruikers slechts één organisatie Active Directory-exemplaar om aan te melden bij uw toepassing (Eén tenant)

Deze optie is een veelvoorkomend scenario voor *LOB-toepassingen*: als u wilt dat uw toepassing om te accepteren van aanmeldingen alleen vanaf de accounts die deel uitmaken van een specifieke Azure Active Directory-exemplaar (met inbegrip van *gastaccounts*van die sessie) het volgende doen:

1. In de **web.config** bestand, wijzig de waarde voor de `Tenant` parameter van `Common` op de naam van de tenant van de organisatie, zoals `contoso.onmicrosoft.com`.
2. In uw [OWIN-Opstartklasse](#configure-the-authentication-pipeline), stel de `ValidateIssuer` argument voor `true`.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>Optie 2: De toegang tot uw toepassing voor gebruikers in een specifieke lijst van organisaties beperken

U kunt aanmelding toegang beperken tot alleen gebruikersaccounts die deel uitmaakt van een Azure AD-organisatie die zich in de lijst met toegestane organisaties:
1. In uw [OWIN-Opstartklasse](#configure-the-authentication-pipeline), stel de `ValidateIssuer` argument voor `true`.
2. Stel de waarde van de `ValidIssuers` parameter aan de lijst met toegestane organisaties.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Optie 3: Een aangepaste methode gebruiken om te valideren van uitgevers van certificaten
U kunt implementeren om een aangepaste methode voor het valideren van uitgevers van certificaten met behulp van de **IssuerValidator** parameter. Lees voor meer informatie over het gebruik van deze parameter de [TokenValidationParameters klasse](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) op MSDN.

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
