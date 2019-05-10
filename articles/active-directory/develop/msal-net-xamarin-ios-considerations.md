---
title: Overwegingen voor Xamarin iOS (Microsoft Authentication Library voor .NET) | Azure
description: Meer informatie over specifieke aandachtspunten bij het gebruik van Xamarin iOS met de Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb07fa00b9e1d917cb64df18fff6466dd5c0193d
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406996"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>Overwegingen voor Xamarin iOS-specifieke met MSAL.NET
Voor Xamarin iOS geldt zijn er enkele overwegingen waarmee u rekening houden moet bij het gebruik van MSAL.NET

- [Bekende problemen met iOS 12 en verificatie](#known-issues-with-ios-12-and-authentication)
- [Overschrijven en implementeren van de `OpenUrl` werken in de `AppDelegate`](#implement-openurl)
- [Inschakelen van sleutelketengroepen](#enable-keychain-groups)
- [-Tokencache delen inschakelen](#enable-token-cache-sharing-across-ios-applications)
- [Toegang tot sleutelhanger inschakelen](#enable-keychain-access)

## <a name="known-issues-with-ios-12-and-authentication"></a>Bekende problemen met iOS 12 en verificatie
Microsoft heeft een [beveiligingsadvies](https://github.com/aspnet/AspNetCore/issues/4647) informatie over compatibiliteitsproblemen tussen iOS12 en bepaalde typen verificatie opgeven. De incompatibiliteit einden sociale, WSFed en OIDC-aanmeldingen. Dit document bevat ook richtlijnen voor ontwikkelaars doen kunnen om te verwijderen van huidige beveiligingsbeperkingen door ASP.NET aan hun toepassingen compatibel zijn met iOS12 worden toegevoegd.  

Bij het ontwikkelen van toepassingen voor Xamarin iOS MSAL.NET, wordt er een oneindige lus bij het aanmelden bij websites van iOS 12 (ongeveer als volgt [ADAL probleem](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329). 

U ziet ook een onderbreking in ASP.NET Core OIDC-verificatie met iOS 12 Safari zoals beschreven in dit [via WebKit probleem](https://bugs.webkit.org/show_bug.cgi?id=188165).

## <a name="implement-openurl"></a>Implement OpenUrl

U moet eerst overschrijven de `OpenUrl` -methode van de `FormsApplicationDelegate` afgeleide klasse en aanroep `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

U moet ook een URL-schema definiëren, moet machtigingen voor uw app naar een andere app aanroepen, hebt een specifiek formulier voor de omleidings-URL en registreer deze Omleidings-URL in de [Azure-portal](https://portal.azure.com).

## <a name="enable-keychain-groups"></a>Inschakelen van Sleutelketengroepen

Als u wilt maken van het token werk in de cache en hebben de `AcquireTokenSilentAsync` methode werk, meerdere stappen moeten worden gevolgd:
1. Inschakelen van toegang tot sleutelhanger in uw *`* Entitlements.plist* bestands- en geef de **Sleutelketengroepen** in de bundel-id.
2. Selecteer *`*Entitlements.plist*`* -bestand in de **Custom Entitlements** in het iOS-project opties venster **bundel ondertekening weergave**.
3. Wanneer een certificaat voor ondertekening, zorg ervoor dat XCode maakt gebruik van dezelfde Apple-ID.

## <a name="enable-token-cache-sharing-across-ios-applications"></a>-Tokencache delen met een iOS-toepassingen inschakelen

Vanaf MSAL 2.x gebruikt, kunt u een beveiligingsgroep voor de sleutelhanger te gebruiken voor het opslaan van de tokencache voor meerdere toepassingen. Hiermee kunt u voor het delen van de tokencache tussen verschillende toepassingen met dezelfde sleutelhanger beveiligingsgroep met inbegrip van die zijn ontwikkeld met [ADAL.NET](https://aka.ms/adal-net), MSAL.NET Xamarin.iOS-toepassingen en systeemeigen iOS-toepassingen die zijn ontwikkeld met [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) of [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc)).

De tokencache delen kan eenmalige aanmelding (SSO) tussen alle toepassingen die gebruikmaken van dezelfde sleutelhanger beveiligingsgroep.

Om in te schakelen eenmalige aanmelding, moet u instellen de `PublicClientApplication.iOSKeychainSecurityGroup` eigenschap op dezelfde waarde in alle toepassingen.

Een voorbeeld hiervan met MSAL v3.x zou zijn:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.msalrocks")
     .Build();
```

Een voorbeeld hiervan met MSAL v2.7.x zou zijn:

```csharp
PublicClientApplication.iOSKeychainSecurityGroup = "com.microsoft.msalrocks";
```

> [!NOTE]
> De `KeychainSecurityGroup` eigenschap is afgeschaft. Voorheen in MSAL 2.x ontwikkelaars moest toevoeging van het team-id-voorvoegsel bij het gebruik van de `KeychainSecurityGroup` eigenschap. 
> 
> Nu starten in de MSAL 2.7.x, MSAL wordt kunt oplossen door het team-id-voorvoegsel tijdens runtime bij het gebruik van de `iOSKeychainSecurityGroup` eigenschap. Wanneer u deze eigenschap gebruikt, is de waarde mag niet het team-id-voorvoegsel. 
> 
> Gebruik de nieuwe `iOSKeychainSecurityGroup` eigenschap, die geen ontwikkelaars voor het team-id vereist. De `KeychainSecurityGroup` eigenschap is nu verouderd. 

## <a name="enable-keychain-access"></a>Toegang tot sleutelhanger inschakelen

In MSAL 2.x en ADAL 4.x de team-id wordt gebruikt voor toegang tot de sleutelketen, waarmee de verificatiebibliotheken voor eenmalige aanmelding (SSO) tussen aanvragen van dezelfde uitgever. 

Wat is de [TeamIdentifierPrefix](/xamarin/ios/deploy-test/provisioning/entitlements?tabs=vsmac) (TeamId)? Dit is een unieke id (bedrijf of persoonlijk) in de App Store. De toepassings-id is uniek voor een app. Als u meer dan één app hebt, de TeamId voor alle apps zijn hetzelfde, maar de AppId afwijken. De toegangsgroep wordt voorafgegaan door TeamId automatisch voor elke groep door het systeem. Het is hoe het besturingssysteem wordt afgedwongen dat apps van dezelfde uitgever toegang heeft tot de gedeelde sleutelketen. 

Bij het initialiseren van de `PublicClientApplication`, als u ontvangt een `MsalClientException` met het bericht: `TeamId returned null from the iOS keychain...`, moet u in de Xamarin iOS-app het volgende doen:

1. In Visual Studio, onder foutopsporingstabblad, gaat u naar nameOfMyApp.iOS eigenschappen...
2. Ga vervolgens naar iOS-bundel ondertekenen 
3. Klik onder aangepaste rechten, op de... het bestand en selecteer Entitlements.plist vanuit uw app.
4. In het csproj-bestand van de iOS-app hebt u deze regel nu opgenomen: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. **Opnieuw opbouwen** het project.

Dit is *bovendien* op het inschakelen van toegang tot sleutelhanger in de `Entitlements.plist` -bestand, met behulp van de onderstaande toegangsgroep of uw eigen:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "https://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
</plist>
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie vindt u in de [iOS specifieke aandachtspunten](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations) lid van het readme.md-bestand voor het volgende voorbeeld:

Voorbeeld | Platform | Description 
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Een eenvoudige Xamarin Forms-app die laat zien hoe u MSAL MSA en Azure AD via het AAD-V2.0-eindpunt te verifiëren en toegang tot de Microsoft Graph met het resulterende token. <br>![Topologie](media/msal-net-xamarin-ios-considerations/topology.png)