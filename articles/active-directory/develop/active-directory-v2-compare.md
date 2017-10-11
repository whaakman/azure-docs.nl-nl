---
title: Wat is er in het Azure AD v2.0-eindpunt anders? | Microsoft Docs
description: Een vergelijking tussen de oorspronkelijke Azure AD en het v2.0-eindpunten.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 81de65b0e825dec64383f52b02c5ee56c9434807
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="whats-different-about-the-v20-endpoint"></a>Wat is het v2.0-eindpunt anders?
Als u bekend met Azure Active Directory bent of apps hebt geïntegreerd met Azure AD in het verleden, kan er een paar verschillen in het v2.0-eindpunt dat u niet verwacht.  Dit document, illustreert die verschillen voor uw begrip.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft-accounts en Azure AD-accounts
Het v2.0-eindpunt kunnen ontwikkelaars schrijven van apps die accepteren aanmelden van zowel Microsoft-Accounts en Azure AD-accounts, met behulp van een enkele auth-eindpunt.  Hiermee krijgt u de mogelijkheid om uw app volledig account-networkdirect; het kan zijn van het type van het account dat de gebruiker zich met aanmeldt ignorant.  Natuurlijk u *kunt* ervoor zorgen dat uw app op de hoogte van het type account wordt gebruikt in een bepaalde sessie, maar u hebt geen tot.

Bijvoorbeeld, als uw app roept de [Microsoft Graph](https://graph.microsoft.io), enkele aanvullende functionaliteit en de gegevens zijn beschikbaar voor ondernemingsgebruikers, zoals hun SharePoint-sites of Active Directory-gegevens.  Maar voor veel acties, zoals [van een gebruiker e-mail lezen](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), de code kan worden geschreven exact dezelfde voor zowel Microsoft-Accounts en Azure AD-accounts.  

Uw app integreren met Microsoft-Accounts en Azure AD-accounts is nu een eenvoudig proces.  U kunt een enkele set eindpunten, één bibliotheek en de registratie van een enkele app toegang te krijgen tot de consumer- en enterprise werelden gebruiken.  Bekijk voor meer informatie over het v2.0-eindpunt, [het overzicht](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Nieuwe app-portal voor registratie
Als u wilt een app die geschikt is voor het v2.0-eindpunt registreren, moet u een nieuwe app-portal voor wachtwoordregistratie: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dit is de portal waar u een toepassings-ID kunt verkrijgen pas het uiterlijk van de aanmeldingspagina van uw app en meer.  Alles wat u nodig hebt voor toegang tot de portal is een ingeschakeld Microsoft-account - persoonlijke of zakelijke/school-account.

## <a name="one-app-id-for-all-platforms"></a>Een app-ID voor alle platforms
Als u Azure Active Directory hebt gebruikt, hebt u waarschijnlijk meerdere verschillende apps voor één project geregistreerd.  Bijvoorbeeld, als u zowel een website en een iOS-app hebt gemaakt, moest u deze afzonderlijk registreren met behulp van twee verschillende toepassings-id's. De registratieportal voor Azure AD-app moet u dit onderscheid maken tijdens de registratie:

![Registratie van oude toepassing gebruikersinterface](../../media/active-directory-v2-flows/old_app_registration.PNG)

Op dezelfde manier als u had een website en een back-end web-api, u mogelijk hebt geregistreerd als een afzonderlijke app in Azure AD.  Of als u had een iOS-app en een Android-app, u ook mogelijk hebt geregistreerd twee verschillende apps.  Registreren van elk onderdeel van een toepassing heeft geleid tot een onverwacht gedrag voor ontwikkelaars en hun klanten:

* Elk onderdeel, weergegeven als een afzonderlijke app in de Azure Active Directory-tenant van elke klant.
* Wanneer een tenantbeheerder heeft geprobeerd om toe te passen om toegang te beheren of verwijderen van een app, zouden ze hebben om dit te doen voor elk onderdeel van de app.
* Wanneer klanten wil een toepassing, elk onderdeel wordt weergegeven in het scherm toestemming als een afzonderlijke toepassing.

Met het v2.0-eindpunt kunt u nu alle onderdelen van uw project als de registratie van een enkele app registreren en één toepassings-Id gebruiken voor uw hele project.  U kunt verschillende 'platforms' toevoegen aan een elk project en de juiste gegevens leveren voor elk platform dat u toevoegt.  Natuurlijk kunt u zoveel apps als u, afhankelijk van uw vereisten wilt, maar voor de meeste gevallen moet slechts één toepassings-Id nodig zijn.

Ons doel is dat hiermee tot een meer vereenvoudigd appbeheer en ontwikkeling biedt leiden, en maakt u een meer geconsolideerde weergave van een enkel project dat u werkt op.

## <a name="scopes-not-resources"></a>Bereiken, geen resources
In Azure Active Directory een app kan functioneren als een **resource**, of een ontvanger van tokens.  Een bron kan een aantal definiëren **scopes** of **oAuth2Permissions** die wordt begrepen, zodat de clientcomputers van apps voor het aanvragen van tokens, voor die bron voor een bepaalde set van bereiken.  Houd rekening met de Azure AD Graph API als een voorbeeld van een resource:

* Bron-id of `AppID URI`:`https://graph.windows.net/`
* Bereiken, of `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, enzovoort.  

Dit alles geldt voor de het v2.0-eindpunt.  Een app kan nog steeds zich gedragen bereik definiëren als resource, en worden geïdentificeerd door een URI.  Client-apps kunnen nog steeds toegang tot deze bereiken aanvragen.  De manier waarop een client een aanvraag die machtigingen heeft echter gewijzigd.  In het verleden een OAuth 2.0 autoriseren aanvraag naar Azure AD mogelijk hebt gezien zoals:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

waar de **resource** parameter aangegeven welke resource vraagt de client-app autorisatie voor.  Azure AD berekend de machtigingen die vereist zijn voor de app op basis van statische configuratie in de Azure Portal en uitgegeven tokens dienovereenkomstig.  Nu autoriseren de dezelfde OAuth 2.0 aanvraag lijkt erop:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

waar de **bereik** parameter geeft aan welke resource en machtigingen voor de app vraagt autorisatie voor. De gewenste resource nog steeds zeer aanwezig is in de aanvraag - deze gewoon is opgenomen in elk van de waarden van de scope-parameter.  Met behulp van de bereikparameter op deze manier kunt het v2.0-eindpunt meer voldoet aan de OAuth 2.0-specificatie en beter afgestemd op algemene procedures voor de branche.  Bovendien kunnen apps uit te voeren [incrementele toestemming](#incremental-and-dynamic-consent), die in de volgende sectie wordt beschreven.

## <a name="incremental-and-dynamic-consent"></a>Incrementele en dynamische toestemming
Apps geregistreerd in Azure AD eerder die nodig zijn voor hun vereist OAuth 2.0-machtigingen opgeven in de Azure-Portal tijdens het maken van een app:

![Machtigingen registratie gebruikersinterface](../../media/active-directory-v2-flows/app_reg_permissions.PNG)

De machtigingen die zijn geconfigureerd met een app vereist **statisch**.  Terwijl deze toegestaan configuratie van de app bestaan in de Azure-Portal en de code bewaard nice en eenvoudig, geeft dit een aantal aspecten voor ontwikkelaars:

* Een app hebben, moeten alle machtigingen die ooit de computer tijdens de aanmaak van de app moet weten.  Machtigingen toe te voegen na verloop van tijd is een moeilijk proces.
* Een app heeft alle resources die deze ooit zou toegang hebben tot tevoren weten.  Was het moeilijk om apps die toegang krijgen een willekeurig aantal resources tot kunnen te maken.
* Een app had om alle machtigingen die ooit de computer na de eerste aanmelden van de gebruiker moet.  In sommige gevallen is dit leidde tot een zeer lange lijst met machtigingen die eindgebruikers uit het goedkeuren van de app-toegang op de eerste aanmelden afgeraden.

Met het v2.0-eindpunt kunt u de machtigingen de behoeften van uw app **dynamisch**, tijdens runtime tijdens normaal gebruik van uw app.  Om dit te doen, kunt u de bereiken die uw app op elk gewenst moment in de tijd moet door ze in de `scope` parameter van een autorisatieaanvraag:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

De bovenstaande aanvragen machtiging voor de app een Azure AD-gebruiker directory om gegevens te lezen, evenals gegevens schrijven naar de directory.  Als de gebruiker heeft ingestemd met deze machtigingen in het verleden voor de desbetreffende app, deze wordt gewoon hun referenties invoeren en in de app zijn ondertekend.  Als de gebruiker niet aan een van deze machtigingen heeft ingestemd, vraagt het v2.0-eindpunt voor toestemming voor deze machtigingen de gebruiker.  Voor meer informatie, kunt u tot lezen op [machtigingen, toestemming en -scopes](active-directory-v2-scopes.md).

Machtigingen voor aanvragen dynamisch via een app zodat de `scope` parameter hebt u volledige controle over uw gebruikerservaring.  Als u wenst, kunt u frontload uw toestemming optreden en vraagt u om alle machtigingen in een initiële autorisatieaanvraag.  Of als uw app een groot aantal machtigingen vereist, kunt u kiezen voor het verzamelen van deze machtigingen van de gebruiker incrementeel, als ze proberen te bepaalde functies van uw app gebruiken gedurende een bepaalde periode.

## <a name="well-known-scopes"></a>Bekende scopes
#### <a name="offline-access"></a>Offline toegang
Apps met behulp van het v2.0-eindpunt mogelijk het gebruik van een nieuwe bekende machtiging voor apps - de `offline_access` bereik.  Alle apps moet deze machtiging aanvragen als ze nodig hebben voor toegang tot bronnen namens een gebruiker voor een langere periode, zelfs wanneer de gebruiker mogelijk niet actief gebruik van de app.  De `offline_access` bereik wordt weergegeven aan de gebruiker in de dialoogvensters toestemming als 'Toegang tot uw gegevens offline', die de gebruiker moet accepteren.  Aanvragen van de `offline_access` machtiging kunnen uw web-app voor het ontvangen van OAuth 2.0 refresh_tokens van het v2.0-eindpunt.  Refresh_tokens zijn lange levensduur hebben en kunnen worden uitgewisseld voor nieuwe OAuth 2.0 access_tokens gedurende langere perioden van toegang.  

Als uw app vraagt niet om de `offline_access` bereik, ontvangt geen refresh_tokens.  Dit betekent dat wanneer u een authorization_code in het OAuth 2.0-autorisatiecodestroom inwisselen, alleen u weer een access_token van ontvangt de `/token` eindpunt.  Die access_token blijft geldig voor een korte periode (doorgaans één uur), maar zal uiteindelijk verlopen.  Back-AT dat punt in tijd, uw app moet de gebruiker wordt omgeleid naar de `/authorize` eindpunt voor het ophalen van een nieuwe authorization_code.  Tijdens deze omleiding de gebruiker kan of kunnen niet moet hun referenties opnieuw invoeren of opnieuw toestemming geven aan machtigingen, afhankelijk van de het type app.

Bekijk voor meer informatie over het OAuth 2.0, refresh_tokens en access_tokens de [v2.0 protocolnaslaginformatie](active-directory-v2-protocols.md).

#### <a name="openid-profile-and-email"></a>OpenID, profiel en e-mailadres
In het verleden hebben biedt het meest eenvoudige OpenID Connect-in-stroom met Azure Active Directory een schat aan informatie over de gebruiker in de resulterende id_token.  De claims in een id_token kunnen opnemen van de gebruiker de naam, voorkeur gebruikersnaam, e-mailadres, object-ID en meer.

Er zijn nu beperken van de informatie die de `openid` bereik kan uw apptoegang tot.  Het bereik 'openid' wordt alleen toe zodat uw app voor het ondertekenen van de gebruiker in en ontvangen van een app-specifiek-id voor de gebruiker.  Als u ophalen van persoonsgegevens (PII) over de gebruiker in uw app wilt, moet uw app om aanvullende machtigingen van de gebruiker.  Twee nieuwe scopes – worden geïntroduceerd de `email` en `profile` scopes – waarmee u kunt doen.

De `email` bereik is zeer eenvoudig: uw app-toegang tot het primaire e-mailadres van de gebruiker via de `email` claim in de id_token.  De `profile` bereik kan uw apptoegang tot alle andere basisinformatie over de gebruiker – de naam, de voorkeur username, object-ID, enzovoort.

Hiermee kunt u uw app op een wijze minimale openbaarmaking code – u kunt alleen de gebruiker vragen voor de set met gegevens dat uw app vereist dat de taak uit te voeren.  Raadpleeg voor meer informatie over deze scopes [de bereikverwijzing v2.0](active-directory-v2-scopes.md).

## <a name="token-claims"></a>Token Claims
De claims in tokens die zijn uitgegeven door het v2.0-eindpunt zijn niet identiek zijn aan tokens die zijn uitgegeven door de algemeen beschikbaar Azure AD-eindpunten - apps migreren naar de nieuwe service moeten niet wordt ervan uitgegaan dat een bepaalde claim in id_tokens of access_tokens aanwezig. Zie voor meer informatie over de specifieke claims in v2.0 tokens worden verzonden, de [v2.0 tokenverwijzing](active-directory-v2-tokens.md).

## <a name="limitations"></a>Beperkingen
Er zijn enkele beperkingen moet houden bij het gebruik van het v2.0-punt.  Raadpleeg de [v2.0 beperkingen doc](active-directory-v2-limitations.md) om te zien als een van deze beperkingen van toepassing op uw specifieke scenario.
