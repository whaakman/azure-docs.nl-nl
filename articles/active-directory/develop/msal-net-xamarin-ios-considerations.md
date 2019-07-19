---
title: Xamarin iOS-overwegingen (micro soft Authentication Library voor .NET) | Azure
description: Meer informatie over specifieke overwegingen bij het gebruik van Xamarin iOS met de micro soft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7bb4aab4c217e20245a1f6ee9b2910a4558acad
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278215"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>Xamarin iOS-specifieke overwegingen met MSAL.NET
Op Xamarin iOS zijn er verschillende overwegingen die u moet meenemen bij het gebruik van MSAL.NET

- [Bekende problemen met iOS 12 en verificatie](#known-issues-with-ios-12-and-authentication)
- [De `OpenUrl` functie vervangen en implementeren in de`AppDelegate`](#implement-openurl)
- [Sleutel hanger groepen inschakelen](#enable-keychain-access)
- [Het delen van token cache inschakelen](#enable-token-cache-sharing-across-ios-applications)
- [Sleutel hanger toegang inschakelen](#enable-keychain-access)

## <a name="known-issues-with-ios-12-and-authentication"></a>Bekende problemen met iOS 12 en verificatie
Micro soft heeft een [beveiligings advies](https://github.com/aspnet/AspNetCore/issues/4647) uitgebracht om informatie te geven over een incompatibiliteit tussen iOS12 en een type verificatie. De incompatibiliteit verbreekt sociale, WSFed-en OIDC-aanmeldingen. Dit advies bevat ook richt lijnen voor het verwijderen van de huidige beveiligings beperkingen die door ASP.NET aan hun toepassingen worden toegevoegd, zodat deze compatibel zijn met iOS12.  

Wanneer u MSAL.NET-toepassingen ontwikkelt op Xamarin iOS, ziet u mogelijk een oneindige lus wanneer u zich probeert aan te melden bij websites van iOS 12 (vergelijkbaar met dit [ADAL-probleem](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329). 

Mogelijk ziet u ook een afbreek ASP.NET Core OIDC-verificatie met iOS 12 Safari, zoals beschreven in dit [webkit-probleem](https://bugs.webkit.org/show_bug.cgi?id=188165).

## <a name="implement-openurl"></a>OpenUrl implementeren

Eerst moet u de `OpenUrl` methode van de afgeleide klasse en het `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs` `FormsApplicationDelegate` aanroepen onderdrukken.

```CSharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

U moet ook een URL-schema definiëren, machtigingen voor uw app vereisen voor het aanroepen van een andere app, een specifiek formulier voor de omleidings-URL hebben en deze omleidings-URL registreren in de [Azure Portal](https://portal.azure.com)

### <a name="enable-keychain-access"></a>Sleutel hanger toegang inschakelen

Als u de toegang tot de sleutel keten wilt inschakelen, moet uw toepassing een toegangs groep voor de sleutel hanger hebben.
U kunt de toegangs groep voor de sleutel hanger `WithIosKeychainSecurityGroup()` instellen met behulp van de API bij het maken van uw toepassing, zoals hieronder wordt weer gegeven:

Om eenmalige aanmelding in te scha kelen, moet u de `PublicClientApplication.iOSKeychainSecurityGroup` eigenschap instellen op dezelfde waarde in alle toepassingen.

Een voor beeld hiervan is het gebruik van MSAL v3. x:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.msalrocks")
     .Build();
```

De rechten. plist moet worden bijgewerkt om te zien als het volgende XML-fragment:

Deze wijziging is een *aanvulling* op het inschakelen van sleutel hanger toegang in het `Entitlements.plist` bestand met behulp van de onderstaande toegangs groep of uw eigen:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.msalrocks</string>
  </array>
</dict>
</plist>
```

Een voor beeld hiervan is het gebruik van MSAL v4. x:

```csharp
PublicClientApplication.iOSKeychainSecurityGroup = "com.microsoft.msalrocks";
```

Wanneer u de `WithIosKeychainSecurityGroup()` API gebruikt, wordt uw beveiligings groep automatisch toegevoegd aan het einde van de ' team-ID ' (AppIdentifierPrefix) van de toepassing omdat u de toepassing op basis van Xcode maakt. [Zie de documentatie voor IOS-rechten voor meer informatie](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps). Daarom moet u de rechten bijwerken om $ (AppIdentifierPrefix) op te nemen vóór de toegangs groep voor de sleutel hanger in de rechten. plist.

### <a name="enable-token-cache-sharing-across-ios-applications"></a>Het delen van tokens in de cache inschakelen voor iOS-toepassingen

Vanuit MSAL 2. x kunt u een toegangs groep voor de sleutel hanger opgeven die moet worden gebruikt voor het persistent maken van de token cache in meerdere toepassingen. Met deze instelling kunt u de token cache delen tussen verschillende toepassingen die dezelfde toegangs groep voor de sleutel hanger hebben, waaronder die zijn ontwikkeld met [ADAL.net](https://aka.ms/adal-net), MSAL.net Xamarin. IOS-toepassingen en systeem eigen IOS-toepassingen die zijn ontwikkeld met [ ADAL. objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) of [MSAL. objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc)).

Door de token cache te delen, kunt u eenmalige aanmelding toestaan tussen alle toepassingen die gebruikmaken van de toegangs groep voor de sleutel hanger.

Als u deze cache wilt delen, moet u de methode ' WithIosKeychainSecurityGroup () ' gebruiken om de toegangs groep voor de sleutel hanger in te stellen op dezelfde waarde in alle toepassingen die dezelfde cache delen, zoals in het bovenstaande voor beeld wordt weer gegeven.

Voorheen werd vermeld dat MSAL $ (AppIdentifierPrefix) heeft toegevoegd wanneer u de `WithIosKeychainSecurityGroup()` API gebruikt. Dit komt doordat de AppIdentifierPrefix of de ' team-ID ' wordt gebruikt om ervoor te zorgen dat alleen toepassingen die door dezelfde uitgever zijn gemaakt, de sleutel van het toegangs rechten kunnen delen.

#### <a name="note-keychainsecuritygroup-property-deprecated"></a>Opmerking: Eigenschap KeychainSecurityGroup afgeschaft

Voorheen waren de ontwikkel aars van MSAL 2. x gedwongen het TeamId-voor voegsel toe te voegen bij het gebruik van de `KeychainSecurityGroup` eigenschap

Bij het gebruik van de nieuwe `iOSKeychainSecurityGroup` eigenschap van MSAL 2.7. x wordt het TeamId-voor voegsel tijdens runtime opgelost door MSAL. Wanneer u deze eigenschap gebruikt, mag de waarde niet het TeamId-voor voegsel bevatten.

Gebruik de nieuwe `iOSKeychainSecurityGroup` eigenschap, die ontwikkel aars niet nodig heeft om de TeamId op te geven, `KeychainSecurityGroup` omdat de vorige eigenschap nu verouderd is.

### <a name="sample-illustrating-xamarin-ios-specific-properties"></a>Voor beeld van het illustreren van specifieke eigenschappen van Xamarin iOS

Meer informatie vindt u in de para graaf [specifieke overwegingen voor IOS](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations) van het README.MD-bestand van het volgende voor beeld:

Voorbeeld | Platform | Description
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Een eenvoudige Xamarin Forms-app die laat zien hoe u MSAL kunt gebruiken om MSA en Azure AD te verifiëren via het Azure AD V 2.0-eind punt en toegang te krijgen tot de Microsoft Graph met het resulterende token.

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->
