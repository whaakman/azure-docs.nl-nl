---
title: Wat is er anders in het Azure AD v2.0-eindpunt? | Microsoft Docs
description: Een vergelijking tussen de oorspronkelijke Azure AD en het v2.0-eindpunten.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: celested
ms.reviewer: elisol, jmprieur, hirsin
ms.custom: aaddev
ms.openlocfilehash: 0e344f6e9dfee3793320dc9cb79e3231c2eeda87
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581391"
---
# <a name="whats-different-about-the-v20-endpoint"></a>Wat is er anders bij het v2.0-eindpunt?

Als u bekend met Azure Active Directory (Azure AD bent) of apps hebt geïntegreerd met Azure AD in het verleden, zijn er enkele verschillen in het v2.0-eindpunt dat mogelijk niet verwacht. In dit artikel worden de verschillen voor uw begrip aangeroepen.

> [!NOTE]
> Niet alle Azure AD-scenario's en functies worden ondersteund door het v2.0-eindpunt. Meer informatie over om te bepalen als u het v2.0-eindpunt moet gebruiken, [v2.0 beperkingen](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft-accounts en Azure AD-accounts

Het v2.0-eindpunt kan ontwikkelaars apps waarin aanmelding vanaf zowel Microsoft-Accounts en Azure AD-accounts, met behulp van een eindpunt één auth schrijven. Dit biedt u de mogelijkheid om uw app volledig account-onafhankelijke, wat betekent dat de app kan worden ignorant van het type account dat de gebruiker zich aanmeldt. U kunt uw app op de hoogte van het type account dat wordt gebruikt in een bepaalde sessie maken, maar u hoeft te.

Bijvoorbeeld, als uw app roept de [Microsoft Graph](https://graph.microsoft.io), enkele aanvullende functionaliteit en de gegevens is beschikbaar voor zakelijke gebruikers, zoals hun SharePoint-sites of de Directory-gegevens. Maar voor veel bewerkingen, zoals [van een gebruiker e-mail lezen](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), de code kan worden geschreven exact hetzelfde zijn voor zowel Microsoft-Accounts en Azure AD-accounts. 

Uw app integreren met Microsoft-Accounts en Azure AD-accounts is nu een eenvoudig proces. U kunt één set met eindpunten, één bibliotheek en de registratie van een één-app toegang te krijgen tot de consument- en enterprise werelden. Bekijk voor meer informatie over het v2.0-eindpunt, [het overzicht](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Nieuwe app-registratieportal

Voor het registreren van een app die geschikt is voor het v2.0-eindpunt, moet u de Microsoft app-registratieportal: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Dit is de portal waar u een toepassings-ID kan krijgen pas het uiterlijk van de aanmeldingspagina van uw app, en meer. Alles wat u nodig voor toegang tot de portal is een Microsoft aangestuurd account - persoonlijke of werk/school-account.

## <a name="one-app-id-for-all-platforms"></a>Een app-ID voor alle platforms

Als u Azure AD hebt gebruikt, hebt u waarschijnlijk meerdere verschillende apps voor een enkel project geregistreerd. Bijvoorbeeld, als u zowel een website en een iOS-app hebt gebouwd, moest u deze afzonderlijk registreren met behulp van twee verschillende toepassings-id's. De Azure AD-app-registratieportal moest u om dit onderscheid tijdens de registratie:

![Oude Toepassingsregistratie UI](./media/azure-ad-endpoint-comparison/old_app_registration.PNG)

Op dezelfde manier als u had een website en een back-end web-api, u mogelijk hebt geregistreerd als een afzonderlijke app in Azure AD. Of als u had een iOS-app en een Android-app, ook mogelijk hebt u geregistreerd twee verschillende apps. Registreren van elk onderdeel van een toepassing heeft geleid tot een onverwacht gedrag voor ontwikkelaars en hun klanten:

* Elk onderdeel weergegeven als een afzonderlijke app in de Azure AD-tenant van elke klant.
* Wanneer een tenantbeheerder heeft geprobeerd om beleid voor het beheren van toegang tot of verwijderen van een app te passen, zouden ze hebben om dit te doen voor elk onderdeel van de app.
* Wanneer klanten ingestemd met een toepassing, wordt elk onderdeel in het instemmingsscherm weergegeven als een afzonderlijke toepassing.

Met het v2.0-eindpunt kunt u nu alle onderdelen van uw project als een registratie-één-app registreren en een enkele toepassings-Id gebruiken voor uw gehele project. U kunt verschillende 'platforms' toevoegen aan een elk project en geef de juiste gegevens voor elk platform dat u toevoegt. U kunt natuurlijk zo veel apps als u, afhankelijk van uw vereisten wilt, maar voor de meeste gevallen moet slechts één toepassings-Id nodig zijn maken.

Ons doel is dit zal leiden tot een meer vereenvoudigd beheer van Apps en ontwikkelervaring en maken van een meer geconsolideerde weergave van een enkel project dat u werkt op.

## <a name="scopes-not-resources"></a>Bereiken, geen resources

In Azure AD, een app kan functioneren als een **resource**, of een ontvanger van tokens. Een resource kan een aantal definiëren **scopes** of **oAuth2Permissions** die wordt begrepen, zodat de clientcomputers apps om aan te vragen van tokens, voor die bron voor een bepaalde set bereiken. Houd rekening met de Azure AD Graph-API als een voorbeeld van een resource:

* Resource-id of `AppID URI`: `https://graph.windows.net/`
* Bereiken, of `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, enzovoort. 

Dit geldt voor het v2.0-eindpunt. Een app nog steeds kan functioneren als resource, bereiken definiëren en worden aangeduid met een URI. Client-apps kunnen nog steeds aanvragen voor toegang tot deze bereiken. De manier waarop dat een client deze machtigingen aanvraagt, is echter gewijzigd. In het verleden een OAuth 2.0 autoriseren aanvraag naar Azure AD kan hebt bekeken, zoals:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

waar de **resource** parameter aangegeven welke resource vraagt de client-app-autorisatie voor. Azure AD berekend de machtigingen die vereist zijn voor de app op basis van statische configuratie in Azure portal en uitgegeven tokens dienovereenkomstig. Nu autoriseren de dezelfde OAuth 2.0 aanvraag er ongeveer zo uitziet zoals:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

waar de **bereik** parameter geeft aan welke resource en machtigingen voor de app vraagt autorisatie voor. De gewenste resource is nog steeds aanwezig in de aanvraag - deze gewoon is opgenomen in elk van de waarden van de scope-parameter. Met behulp van de bereikparameter op deze manier kunt het v2.0-eindpunt meer voldoet aan de OAuth 2.0-specificatie en beter aansluit bij de algemene procedures voor de bedrijfstak. Bovendien kunnen apps om uit te voeren [incrementele toestemming](#incremental-and-dynamic-consent), die in de volgende sectie wordt beschreven.

## <a name="incremental-and-dynamic-consent"></a>Incrementele en dynamische toestemming

Apps in Azure AD eerder die nodig zijn om op te geven van de vereiste machtigingen voor OAuth 2.0 in Azure portal tijdens de aanmaak van de app worden geregistreerd:

![Machtigingen voor registratie van de gebruikersinterface](./media/azure-ad-endpoint-comparison/app_reg_permissions.PNG)

De machtigingen die zijn geconfigureerd met een app vereist **statisch**. Terwijl dit toegestaan configuratie van de app aanwezig zijn in Azure portal en de code leuk en eenvoudige blijven, wordt deze enkele problemen voor ontwikkelaars:

* Een app heeft alle machtigingen die ooit tijdens de aanmaak van de app moet weten. Machtigingen toe te voegen na verloop van tijd is een moeilijk proces.
* Een app heeft alle resources, dan ooit tevoren zijn toegankelijk weten. Het is moeilijk om apps die toegang een willekeurig aantal resources tot te maken.
* Een app heeft om aan te vragen van alle machtigingen die ooit moet de eerste aanmelden van de gebruiker bij. In sommige gevallen is die dit tot een lange lijst met machtigingen leidde die eindgebruikers van het goedkeuren van de app-toegang op de eerste aanmelding afgeraden.

Met het v2.0-eindpunt, kunt u de machtigingen voor de behoeften van uw app **dynamisch**, tijdens runtime, tijdens het normale gebruik van uw app. Om dit te doen, kunt u de bereiken die uw app op een willekeurig moment in de tijd moet door ze in de `scope` parameter van een autorisatieaanvraag:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

De bovenstaande aanvragen machtiging voor lezen van de gebruiker van een Azure AD directory-gegevens, evenals gegevens schrijven naar de map met de app. Als de gebruiker heeft ingestemd met de juiste machtigingen in het verleden bevindt voor de desbetreffende app, worden ze zullen zijn referenties invoeren en worden aangemeld bij de app. Als de gebruiker heeft niet ingestemd met een van deze machtigingen, vraagt het v2.0-eindpunt de gebruiker om toestemming voor de juiste machtigingen. Voor meer informatie, kunt u tot lezen op [machtigingen en toestemming scopes](v2-permissions-and-consent.md).

Een app voor aanvullende machtigingen dynamisch via zodat de `scope` parameter hebt u volledige controle over uw gebruikerservaring. Als u wilt, kunt u frontload uw toestemming optreden en vraagt u om alle machtigingen in een eerste autorisatie-aanvraag. Of als uw app nodig een groot aantal machtigingen heeft, kunt u kiezen voor het verzamelen van deze machtigingen van de gebruiker en als ze proberen om het gebruik van bepaalde functies van uw app na verloop van tijd.

## <a name="well-known-scopes"></a>Bekende bereiken

### <a name="offline-access"></a>Offline toegang

Apps met behulp van het v2.0-eindpunt mogelijk het gebruik van een nieuwe bekende machtiging voor apps - de `offline_access` bereik. Alle apps moeten deze machtiging aanvragen als ze nodig hebben voor toegang tot resources namens een gebruiker voor een langere periode, zelfs wanneer de gebruiker niet actief de app gebruikt mogelijk. De `offline_access` bereik weergegeven voor de gebruiker in dialoogvensters als 'Toegang tot uw gegevens offline', die de gebruiker moet instemmen. Aanvragen van de `offline_access` machtiging wordt uw web-app voor het ontvangen van OAuth 2.0-refresh_tokens van het v2.0-eindpunt inschakelen. Refresh_tokens zijn lange levensduur hebben en kunnen worden uitgewisseld voor nieuwe OAuth 2.0-access_tokens gedurende langere perioden van toegang. 

Als uw app geen heeft aangevraagd de `offline_access` bereik, profiteert deze niet refresh_tokens. Dit betekent dat wanneer u een authorization_code in de OAuth 2.0-autorisatiecodestroom inwisselen, ontvangt u alleen weer een access_token uit de `/token` eindpunt. Deze access_token blijft geldig voor een korte periode (doorgaans één uur), maar uiteindelijk verlopen. Op dat punt in tijd, uw app wilt omleiden van de gebruiker terug naar de `/authorize` eindpunt om op te halen van een nieuwe authorization_code. Tijdens deze omleiding, wordt de gebruiker kan of mogelijk niet naar hun referenties opnieuw invoeren of opnieuw toestemming geven voor machtigingen, afhankelijk van het type app.

Bekijk voor meer informatie over het OAuth 2.0, refresh_tokens en access_tokens de [protocolnaslaginformatie voor v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>OpenID, het profiel en e-mailadres

In het verleden was het meest eenvoudige OpenID Connect-aanmeldingsstroom met Azure AD kan veel informatie over de gebruiker in de resulterende id_token bieden. De claims in een id_token kunnen opnemen van de gebruiker de naam, gewenste gebruikersnaam, e-mailadres, object-ID en meer.

De informatie die de `openid` bereik kan uw app is nu toegang tot beperkte toegang. De `openid` bereik wordt alleen uw app en meld u aan de gebruiker ontvangt een app-specifieke id voor de gebruiker toestaan. Als u ophalen van persoonlijke gegevens over de gebruiker in uw app wilt, moet uw app voor aanvullende machtigingen van de gebruiker. Twee nieuwe scopes – de `email` en `profile` bereiken, kunt u extra machtigingen aanvragen.

De `email` bereik maakt het mogelijk de toegang tot uw Apps naar de primaire e-mailadres van de gebruiker via de `email` claim in de id_token. 

De `profile` bereik kan uw apptoegang tot alle andere algemene informatie over de gebruiker – de naam, de gewenste gebruikersnaam, object-ID, enzovoort.

Hiermee kunt u uw app op een manier vrijgeven van minimale code – u kunt alleen de gebruiker vragen voor de set met informatie over dat uw app nodig heeft om zijn werk te doen. Zie voor meer informatie over deze scopes [de bereikverwijzing v2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Tokenclaims

De claims in de tokens die zijn uitgegeven door het v2.0-eindpunt, worden niet identiek zijn aan de tokens die zijn uitgegeven door de algemeen beschikbare Azure AD-eindpunten. Apps migreren naar de nieuwe service moeten niet wordt ervan uitgegaan dat een specifieke claim in id_tokens of access_tokens aanwezig. Zie voor meer informatie over de specifieke claims worden weergegeven in het v2.0-tokens, de [v2.0 tokenverwijzing](v2-id-and-access-tokens.md).

## <a name="limitations"></a>Beperkingen

Er zijn enkele beperkingen rekening mee moet houden bij het gebruik van het v2.0-punt. Zie voor meer informatie als een van deze beperkingen van toepassing op uw specifieke scenario, de [v2.0 beperkingen doc](active-directory-v2-limitations.md).
