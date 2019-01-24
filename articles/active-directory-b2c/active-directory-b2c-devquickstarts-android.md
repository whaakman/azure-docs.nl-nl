---
title: Ophalen van een token met behulp van een Android-toepassing in Azure Active Directory B2C | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een Android-app die gebruikmaakt van AppAuth met Azure Active Directory B2C voor het beheren van gebruikers-id's en verifiëren van gebruikers maakt.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8a9f6e52f589ab68c3bf8b394d6875082bc0ddf1
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845789"
---
# <a name="sign-in-using-an-android-application-in-azure-active-directory-b2c"></a>Meld u aan met een Android-toepassing in Azure Active Directory B2C

Op het Microsoft Identity-platform wordt gebruikgemaakt van open standaarden, zoals OAuth2 en OpenID Connect. Deze standaarden kunnen u gebruikmaken van een bibliotheek die u wilt integreren met Azure Active Directory B2C. Als u andere bibliotheken gebruiken, kunt u een scenario zoals deze te laten zien hoe het configureren van 3e partij bibliotheken die u moet verbinding maken met het Microsoft identity-platform. De meeste bibliotheken die implementeren [de RFC6749 OAuth2-specificatie](https://tools.ietf.org/html/rfc6749) verbinding kan maken met het Microsoft Identity-platform.

> [!WARNING]
> Microsoft biedt geen oplossingen voor 3e partij bibliotheken en een overzicht van deze bibliotheken niet heeft gedaan. In dit voorbeeld maakt gebruik van een 3e partij library, die AppAuth die is getest op compatibiliteit in algemene scenario's met de Azure AD B2C. Problemen en functie-aanvragen moeten worden omgeleid naar de open source-project van de bibliotheek. Raadpleeg [in dit artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) voor meer informatie.  
>
>

Als u nog geen ervaring hebt met OAuth2 of OpenID Connect, zal een groot gedeelte van deze voorbeeldconfiguratie u niet veel zeggen. U wordt geadviseerd eerst een beknopt [overzicht van het hier gedocumenteerde protocol te bekijken](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken

Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor alle gebruikers, apps, groepen en meer. Als u nog geen directory hebt, [maakt u een B2C-directory](tutorial-create-tenant.md) voordat u verdergaat.

## <a name="create-an-application"></a>Een app maken

Vervolgens maakt u een app in uw B2C-directory. Hiermee geeft u informatie door aan Azure AD die nodig is om veilig te communiceren met uw app. Voor het maken van een mobiele app, gaat u als volgt [deze instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

* Bevatten een **Native Client** in de toepassing.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U moet dit later opnieuw.
* Instellen van een systeemeigen client **omleidings-URI** (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Deze hebt u ook later nodig.

## <a name="create-your-user-flows"></a>Uw gebruikersstromen maken

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [gebruikersstroom](active-directory-b2c-reference-policies.md), dit is een set beleidsregels die de werking van Azure AD beheren. Deze toepassing moet een stroom aanmelden en meld u aan de gebruiker. Wanneer u de gebruikersstroom maakt, moet u naar:

* Kies de **weergavenaam** als een aanmelding kenmerk in de gebruikersstroom.
* Kies de **weergavenaam** en **Object-ID** toepassingsclaims in de gebruikersstroom van elke. U kunt ook andere claims kiezen.
* Kopieer de **naam** van elk beleid nadat u dit hebt gemaakt. Deze moet het voorvoegsel `b2c_1_` bevatten.  U moet de flow-naam van de gebruiker later opnieuw.

Nadat u uw gebruikersstromen hebt gemaakt, bent u klaar om uw app te bouwen.

## <a name="download-the-sample-code"></a>Download de voorbeeldcode

We bieden een werkende-voorbeeld dat gebruikmaakt van AppAuth met Azure AD B2C [op GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). U kunt de code downloaden en uitvoeren. U kunt snel aan de slag met uw eigen app met behulp van uw eigen Azure AD B2C-configuratie door de instructies in de [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

Het voorbeeld is een wijziging van het voorbeeld dat is opgegeven door [AppAuth](https://openid.github.io/AppAuth-Android/). Ga naar de pagina voor meer informatie over AppAuth en de bijbehorende functies.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Wijzigen van uw app voor het gebruik van Azure AD B2C met AppAuth

> [!NOTE]
> AppAuth biedt ondersteuning voor Android API 16 (Jellybean) en hoger. Wordt geadviseerd om API 23 en hoger.
>

### <a name="configuration"></a>Configuratie

U kunt de communicatie met Azure AD B2C configureren door een van beide op te geven de detectie URI of door de autorisatie-eindpunt en de token eindpunt-URI's op te geven. In beide gevallen moet u de volgende informatie:

* Tenant-ID (bijvoorbeeld contoso.onmicrosoft.com)
* Userjourney-naam (bijvoorbeeld B2C\_1\_SignUpIn)

Als u ervoor kiest voor het automatisch detecteren van de autorisatie en token-eindpunt URI's, moet u voor het ophalen van gegevens uit de URI van de detectie. De detectie URI worden gegenereerd door het vervangen van de Tenant\_-ID en het beleid\_naam in de volgende URL:

```java
String mDiscoveryURI = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Vervolgens kunt u de autorisatie en token-eindpunt URI's verkrijgen en maken van een object AuthorizationServiceConfiguration door het uitvoeren van de volgende:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

In plaats van detectie voor het verkrijgen van de autorisatie en token-eindpunt URI's, kunt u ook opgeven expliciet door de Tenant\_-ID en het beleid\_naam in van de URL hieronder:

```java
String mAuthEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Voer de volgende code om uw AuthorizationServiceConfiguration-object te maken:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Autoriseren

Na het configureren van of bij het ophalen van een serviceconfiguratie autorisatie, kan een autorisatieaanvraag worden samengesteld. Voor het maken van de aanvraag, moet u de volgende informatie:

* Client-ID (bijvoorbeeld 00000000-0000-0000-0000-000000000000)
* Omleidings-URI met een aangepast schema (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)

Beide items moeten zijn opgeslagen wanneer u [uw app registreert](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Raadpleeg de [AppAuth handleiding](https://openid.github.io/AppAuth-Android/) voor het uitvoeren van de rest van het proces. Als u snel aan de slag met een werkende app wilt, bekijk dan [ons voorbeeld](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Volg de stappen in de [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) in te voeren van uw eigen Azure AD B2C-configuratie.

