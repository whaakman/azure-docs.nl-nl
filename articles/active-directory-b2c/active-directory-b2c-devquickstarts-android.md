---
title: Ophalen van een token met behulp van een Android-toepassing in Azure Active Directory B2C | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een Android-app die gebruikmaakt van AppAuth met Azure Active Directory B2C gebruikersidentiteiten te beheren en verifiëren van gebruikers te maken.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7c0a72e03eaa8d12c26b1bbbf6a05b4d94e72358
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709903"
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Azure AD B2C: Meld u aan met een Android-toepassing

Op het Microsoft Identity-platform wordt gebruikgemaakt van open standaarden, zoals OAuth2 en OpenID Connect. Deze normen wordt voldaan, kunt u gebruikmaken van een bibliotheek die u wilt integreren met Azure Active Directory B2C. Als u andere bibliotheken gebruiken, kunt u een overzicht zoals deze te laten zien hoe 3e partij mediawisselaars verbinding maken met het identiteitsplatform van Microsoft te configureren. De meeste bibliotheken die implementeren [de RFC6749 OAuth2-specificatie](https://tools.ietf.org/html/rfc6749) verbinding kunnen maken met de Microsoft Identity-platform.

> [!WARNING]
> Microsoft biedt geen oplossingen voor 3rd partij bibliotheken en een overzicht van deze bibliotheken niet uitgevoerd. Dit voorbeeld maakt gebruik van een 3e partij-bibliotheek AppAuth dat is getest op compatibiliteit in algemene scenario's met de Azure AD B2C. Problemen en functie-aanvragen moeten worden omgeleid naar de bibliotheek open source-project. Zie [in dit artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) voor meer informatie.  
>
>

Als u nog geen ervaring hebt met OAuth2 of OpenID Connect, zal een groot gedeelte van deze voorbeeldconfiguratie u niet veel zeggen. U wordt geadviseerd eerst een beknopt [overzicht van het hier gedocumenteerde protocol te bekijken](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken

Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor alle gebruikers, apps, groepen en meer. Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u verdergaat.

## <a name="create-an-application"></a>Een app maken

Vervolgens maakt u een app in uw B2C-directory. Hiermee geeft u informatie door aan Azure AD die nodig is om veilig te communiceren met uw app. Volg voor het maken van een mobiele app [deze instructies](active-directory-b2c-app-registration.md). Zorg ervoor dat:

* Omvatten een **Native Client** in de toepassing.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U nodig deze later.
* Instellen van een native client **omleidings-URI** (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Deze hebt u ook later nodig.

## <a name="create-your-policies"></a>Het beleid maken

In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). Deze app bevat één identiteit ervaring: een gecombineerde aanmelden en registreren. U moet maken dit beleid, zoals beschreven in de [naslagartikel](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Wanneer u het beleid maakt:

* Kies de **weergavenaam** als een kenmerk aanmelding in uw beleid.
* Kiest u **Weergavenaam**- en **Object-id**-toepassingsclaims voor elk beleid. U kunt ook andere claims kiezen.
* Kopieert u de **naam** van elk beleid nadat u dit hebt gemaakt. Deze moet het voorvoegsel `b2c_1_` bevatten.  U hebt de beleidsnaam later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u beleidsregels hebt gemaakt, kunt u de app maken.

## <a name="download-the-sample-code"></a>De voorbeeldcode downloaden

We hebben een werkende-voorbeeldtoepassing die gebruikmaakt van AppAuth met Azure AD B2C opgegeven [op GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). U kunt de code downloaden en uitvoeren. U kunt snel aan de slag met uw eigen app met behulp van uw eigen Azure AD B2C-configuratie door de instructies in de [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

Het voorbeeld is een wijziging van het voorbeeld dat is opgegeven door [AppAuth](https://openid.github.io/AppAuth-Android/). Ga naar de pagina voor meer informatie over AppAuth en de bijbehorende functies.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Uw app voor het gebruik van Azure AD B2C met AppAuth wijzigen

> [!NOTE]
> AppAuth biedt ondersteuning voor Android-API 16 (Jellybean) en hoger. Het is raadzaam met behulp van API 23 en hoger.
>

### <a name="configuration"></a>Configuratie

U kunt de communicatie met Azure AD B2C configureren door de URI van de detectie of door het eindpunt voor autorisatie en de token eindpunt URI's te geven. In beide gevallen moet u de volgende informatie:

* Tenant-ID (bijvoorbeeld contoso.onmicrosoft.com)
* De naam van beleid (bijvoorbeeld B2C\_1\_SignUpIn)

Als u kiest voor het automatisch detecteren van de autorisatie en -tokeneindpunt URI's, moet u informatie ophalen van de URI van de detectie. Detectie van de URI kan worden gegenereerd door het vervangen van de Tenant\_-ID en het beleid\_naam in de volgende URL:

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

U kunt de autorisatie- en eindpunt URI's van de token verkrijgen en een AuthorizationServiceConfiguration-object maken door het volgende:

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

In plaats van detectie voor informatie over de autorisatie en -tokeneindpunt URI's, kunt u ook opgeven ze expliciet door het vervangen van de Tenant\_-ID en het beleid\_hieronder de naam in de URL:

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Voer de volgende code om uw AuthorizationServiceConfiguration-object te maken:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Autoriseren

Na het configureren van of bij het ophalen van een configuratie van de service autorisatie, kan een autorisatieaanvraag worden samengesteld. Voor het maken van de aanvraag, moet u de volgende informatie:

* Client-ID (bijvoorbeeld 00000000-0000-0000-0000-000000000000)
* Omleidings-URI met een aangepast schema (bijvoorbeeld com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)

Beide items moeten zijn opgeslagen als u zijn [u uw app registreert](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Raadpleeg de [AppAuth handleiding](https://openid.github.io/AppAuth-Android/) voor het voltooien van de rest van het proces. Als u snel aan de slag met een werkende app wilt, Bekijk [ons voorbeeld](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Volg de stappen in de [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) invoeren van de configuratie van uw eigen Azure AD B2C.

We kunnen worden altijd feedback en suggesties! Als u problemen met dit artikel ondervindt of aanbevelingen voor het verbeteren van de inhoud hebben, zouden we stellen uw feedback onder aan de pagina. Voor functieaanvragen, voeg deze toe aan [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

