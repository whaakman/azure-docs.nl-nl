---
title: Ophalen van een token met behulp van een toepassing voor iOS - Azure AD B2C | Microsoft Docs
description: "In dit artikel wordt beschreven hoe u een iOS-app die gebruikmaakt van AppAuth met Azure Active Directory B2C gebruikersidentiteiten te beheren en verifiëren van gebruikers te maken."
services: active-directory-b2c
documentationcenter: ios
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: d818a634-42c2-4cbd-bf73-32fa0c8c69d3
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objectivec
ms.topic: article
ms.date: 03/07/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: ebec5d910b8987dcc8155cd4ead00f87d219941c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Meld u aan met een iOS-toepassing

Op het Microsoft Identity-platform wordt gebruikgemaakt van open standaarden, zoals OAuth2 en OpenID Connect. Gebruik van een open standaard protocol biedt meer mogelijkheden voor ontwikkelaars bij het selecteren van een bibliotheek integreren met onze services. We bieden in dit scenario en andere zoals deze ontwikkelaars helpen bij het schrijven van toepassingen die verbinding met de Microsoft Identity-platform maken. De meeste bibliotheken die implementeren [de RFC6749 OAuth2-specificatie](https://tools.ietf.org/html/rfc6749) kunnen geen verbinding maken met de Microsoft Identity-platform.

> [!WARNING]
> Microsoft biedt geen oplossingen voor de bibliotheken van derden en een overzicht van deze bibliotheken niet uitgevoerd. Dit voorbeeld maakt gebruik van een derde partij bibliotheek aangeroepen AppAuth dat is getest op compatibiliteit in algemene scenario's met de Azure AD B2C. Problemen en functie-aanvragen moeten worden omgeleid naar de bibliotheek open source-project. Raadpleeg [dit artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) voor meer informatie.
>
>

Als u geen ervaring met OAuth2 of OpenID Connect, wellicht veel van de configuratie van deze niet verstandig veel voor u. U wordt geadviseerd eerst een beknopt [overzicht van het hier gedocumenteerde protocol te bekijken](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken
Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor alle gebruikers, apps en groepen. Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u verdergaat.

## <a name="create-an-application"></a>Een app maken
Vervolgens maakt u een app in uw B2C-directory. De app-registratie biedt Azure AD-informatie die nodig is om veilig te communiceren met uw app. Volg voor het maken van een mobiele app [deze instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

* Omvatten een **Native client** in de toepassing.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U hebt deze GUID later nodig.
* Instellen van een **omleidings-URI** met een aangepast schema (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). U hebt deze URI later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Het beleid maken
In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). Deze app bevat één identiteit ervaring: een gecombineerde aanmelden en registreren. Maak van dit beleid, zoals beschreven in de [naslagartikel](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Wanneer u het beleid maakt:

* Onder **registratiekenmerken**, selecteert u het kenmerk **weergavenaam**.  U kunt ook andere kenmerken selecteren.
* Onder **toepassingsclaims**, selecteert u de claims **weergavenaam** en **Object-ID van gebruiker**. U kunt ook andere claims selecteren.
* Kopieert u de **naam** van elk beleid nadat u dit hebt gemaakt. De beleidsnaam van uw wordt voorafgegaan door `b2c_1_` wanneer u het beleid opslaat.  U de naam van het beleid later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u beleidsregels hebt gemaakt, kunt u de app maken.

## <a name="download-the-sample-code"></a>De voorbeeldcode downloaden
We hebben een werkende-voorbeeldtoepassing die gebruikmaakt van AppAuth met Azure AD B2C opgegeven [op GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). U kunt de code downloaden en uitvoeren. Voor het gebruik van uw eigen Azure AD B2C-tenant, volg de instructies in de [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Dit voorbeeld is gemaakt door de instructies Leesmij-bestand door de [AppAuth iOS-project op GitHub](https://github.com/openid/AppAuth-iOS). Voor meer informatie over de werking van de bibliotheek en de voorbeelden verwijzen naar de AppAuth README op GitHub.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Uw app voor het gebruik van Azure AD B2C met AppAuth wijzigen

> [!NOTE]
> AppAuth biedt ondersteuning voor iOS 7 en hoger.  Ter ondersteuning van sociale aanmeldingen op Google, SFSafariViewController is echter vereist dat iOS 9 of hoger vereist.
>

### <a name="configuration"></a>Configuratie

U kunt de communicatie met Azure AD B2C configureren door het eindpunt voor autorisatie en de token eindpunt URI's te geven.  Als deze URI's worden gegenereerd, moet u de volgende informatie:
* Tenant-ID (bijvoorbeeld: contoso.onmicrosoft.com)
* De naam van beleid (bijvoorbeeld B2C\_1\_SignUpIn)

Eindpunt van het token URI kan worden gegenereerd door het vervangen van de Tenant\_-ID en het beleid\_naam in de volgende URL:

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

De autorisatie-eindpunt URI kan worden gegenereerd door het vervangen van de Tenant\_-ID en het beleid\_naam in de volgende URL:

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Voer de volgende code om uw AuthorizationServiceConfiguration-object te maken:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>Autoriseren

Na het configureren van of bij het ophalen van een configuratie van de service autorisatie, kan een autorisatieaanvraag worden samengesteld. Voor het maken van de aanvraag, moet u de volgende informatie:  
* Client-ID (bijvoorbeeld 00000000-0000-0000-0000-000000000000)
* Omleidings-URI met een aangepast schema (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Beide items moeten zijn opgeslagen als u zijn [u uw app registreert](#create-an-application).

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

Als u uw toepassing voor het afhandelen van de omleiding naar de URI met het aangepaste schema instelt, moet u de lijst met URL's in uw Info.pList bijwerken:
* Open Info.pList.
* Beweeg de muisaanwijzer over een rij zoals bundel OS typecode en klik op de \+ symbool.
* Wijzig de naam van de nieuwe rij URL-typen.
* Klik op de pijl naar links van de URL-types openen van de boomstructuur.
* Klik op de pijl naar links van ' 0' om te openen van de structuur-Item.
* Wijzig de naam van eerste onderliggende Item 0 om te URL-schema-item.
* Klik op de pijl naar links van 'URL Schemes' te openen van de boomstructuur.
* In de kolom 'Value' is een leeg veld aan de linkerkant van '0 Item' onder de URL-schema.  Stel de waarde voor uw unieke toepassingsschema.  De waarde moet overeenkomen met het schema in URL omleiding gebruikt bij het maken van het object OIDAuthorizationRequest.  In ons voorbeeld hebben we het schema 'com.onmicrosoft.fabrikamb2c.exampleapp' gebruikt.

Raadpleeg de [AppAuth handleiding](https://openid.github.io/AppAuth-iOS/) voor het voltooien van de rest van het proces. Als u snel aan de slag met een werkende app wilt, Bekijk [ons voorbeeld](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Volg de stappen in de [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) invoeren van de configuratie van uw eigen Azure AD B2C.

We kunnen worden altijd feedback en suggesties! Als u problemen met dit onderwerp ondervindt of aanbevelingen voor het verbeteren van de inhoud hebben, zouden we stellen uw feedback onder aan de pagina. Voor functieaanvragen, voeg deze toe aan [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
