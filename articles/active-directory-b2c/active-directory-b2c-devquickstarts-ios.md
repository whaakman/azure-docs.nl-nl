---
title: Met AppAuth in een iOS-toepassing in Azure Active Directory B2C | Microsoft Docs
description: Dit artikel laat u het maken van een iOS-app die AppAuth met Azure Active Directory B2C gebruikt om gebruikersidentiteiten te beheren en gebruikers te verifiëren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5f95b71497b59eafff09d4add2b4bb1c20656592
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339355"
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Meld u aan met een iOS-toepassing

Op het Microsoft Identity-platform wordt gebruikgemaakt van open standaarden, zoals OAuth2 en OpenID Connect. Gebruik van een open standaard-protocol biedt meer keuze voor ontwikkelaars bij het selecteren van een bibliotheek integreert met onze services. Zijn er beschikbaar gesteld in dit scenario en anderen, zoals het kunnen ontwikkelaars helpen bij het schrijven van toepassingen die verbinding met het Microsoft Identity-platform maken. De meeste bibliotheken die implementeren [de RFC6749 OAuth2-specificatie](https://tools.ietf.org/html/rfc6749) zijn geen verbinding maken met het Microsoft Identity-platform.

> [!WARNING]
> Microsoft biedt geen oplossingen voor bibliotheken van derden en een overzicht van deze bibliotheken niet heeft gedaan. In dit voorbeeld maakt gebruik van een externe bibliotheek AppAuth die is getest met de naam van de compatibiliteit in algemene scenario's met de Azure AD B2C. Problemen en functie-aanvragen moeten worden omgeleid naar de open source-project van de bibliotheek. Raadpleeg [dit artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) voor meer informatie.
>
>

Als u geen ervaring hebt met OAuth2 of OpenID Connect, wellicht veel van deze voorbeeldconfiguratie niet verstandig veel aan u. U wordt geadviseerd eerst een beknopt [overzicht van het hier gedocumenteerde protocol te bekijken](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken
Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor al uw gebruikers, apps, groepen en meer. Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u verdergaat.

## <a name="create-an-application"></a>Een app maken
Vervolgens maakt u een app in uw B2C-directory. De app-registratie biedt Azure AD-gegevens die nodig is om veilig te communiceren met uw app. Voor het maken van een mobiele app, gaat u als volgt [deze instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

* Bevatten een **Native client** in de toepassing.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U hebt deze GUID later nodig.
* Instellen van een **omleidings-URI** met een aangepast schema (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). U hebt deze URI later nodig.

## <a name="create-your-policies"></a>Het beleid maken
In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). Deze app bevat één identity-ervaring: een gecombineerde aanmelding en om te registreren. Maak van dit beleid, zoals beschreven in de [naslagartikel voor beleid](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Wanneer u het beleid maakt:

* Onder **registratiekenmerken**, selecteert u het kenmerk **weergavenaam**.  U kunt ook andere kenmerken selecteren.
* Onder **toepassingsclaims**, selecteert u de claims **weergavenaam** en **Object-ID van gebruiker**. U kunt ook andere claims kiezen.
* Kopieert u de **naam** van elk beleid nadat u dit hebt gemaakt. Naam van uw beleid wordt voorafgegaan door `b2c_1_` wanneer u het beleid opslaat.  U hebt de beleidsnaam later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u beleidsregels hebt gemaakt, kunt u de app maken.

## <a name="download-the-sample-code"></a>Download de voorbeeldcode
We bieden een werkende-voorbeeld dat gebruikmaakt van AppAuth met Azure AD B2C [op GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). U kunt de code downloaden en uitvoeren. Voor het gebruik van uw eigen Azure AD B2C-tenant, volg de instructies in de [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

In dit voorbeeld is gemaakt door de instructies Leesmij-bestand door de [AppAuth iOS-project op GitHub](https://github.com/openid/AppAuth-iOS). Verwijzen naar de README AppAuth op GitHub voor meer informatie over de werking van het voorbeeld en de bibliotheek.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Wijzigen van uw app voor het gebruik van Azure AD B2C met AppAuth

> [!NOTE]
> AppAuth biedt ondersteuning voor iOS 7 en hoger.  Ter ondersteuning van sociale aanmeldingen op Google, SFSafariViewController is echter vereist dat iOS 9 of hoger vereist.
>

### <a name="configuration"></a>Configuratie

U kunt de communicatie met Azure AD B2C configureren door de autorisatie-eindpunt en de token eindpunt-URI's op te geven.  Deze URI's genereren, hoeft u de volgende informatie:
* Tenant-ID (bijvoorbeeld contoso.onmicrosoft.com)
* De naam van beleid (bijvoorbeeld B2C\_1\_SignUpIn)

Het tokeneindpunt URI worden gegenereerd door het vervangen van de Tenant\_-ID en het beleid\_naam in de volgende URL:

```objc
static NSString *const tokenEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

Het autorisatie-eindpunt URI worden gegenereerd door het vervangen van de Tenant\_-ID en het beleid\_naam in de volgende URL:

```objc
static NSString *const authorizationEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Voer de volgende code om uw AuthorizationServiceConfiguration-object te maken:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>Autoriseren

Na het configureren van of bij het ophalen van een serviceconfiguratie autorisatie, kan een autorisatieaanvraag worden samengesteld. Voor het maken van de aanvraag, moet u de volgende informatie:  
* Client-ID (bijvoorbeeld 00000000-0000-0000-0000-000000000000)
* Omleidings-URI met een aangepast schema (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Beide items moeten zijn opgeslagen wanneer u [uw app registreert](#create-an-application).

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
* Beweeg de muisaanwijzer over een rij, zoals 'Code bundel OS Type' en klik op de \+ symbool.
* Wijzig de naam van de nieuwe rij 'URL-types'.
* Klik op de pijl links van URL-typen te openen van de structuur.
* Klik op de pijl links van ' 0' als u wilt de structuur van de open-Item.
* Wijzig de naam van eerste item onder het Item 0 'URL-schema's '.
* Klik op de pijl links van 'URL-schema's ' te openen van de structuur.
* In de kolom 'Value' is een leeg veld aan de linkerkant van '0 Item' onder 'URL-schema's '.  Stel de waarde aan uw unieke toepassingsschema.  De waarde moet overeenkomen met het schema in URL omleiding gebruikt bij het maken van het object OIDAuthorizationRequest.  In het voorbeeld wordt het schema 'com.onmicrosoft.fabrikamb2c.exampleapp' gebruikt.

Raadpleeg de [AppAuth handleiding](https://openid.github.io/AppAuth-iOS/) voor het uitvoeren van de rest van het proces. Als u snel aan de slag met een werkende app wilt, bekijk dan [het voorbeeld](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Volg de stappen in de [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) in te voeren van uw eigen Azure AD B2C-configuratie.

We zijn altijd geopend zodat u kunt feedback en suggesties. Als u problemen met dit artikel ondervindt of aanbevelingen hebt voor het verbeteren van de inhoud, zouden we waarderen uw feedback aan de onderkant van de pagina. Voor functieaanvragen, voeg deze toe aan [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
