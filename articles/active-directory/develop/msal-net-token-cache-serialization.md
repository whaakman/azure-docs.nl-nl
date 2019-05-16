---
title: Cache-serialisatie in Microsoft Authentication Library voor .NET-token | Azure
description: Meer informatie over serialisatie en serialisatie van de klant van de token cache met behulp van Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4a4c4ca1925a501b10cb86a2cf60646af1e5b57
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544253"
---
# <a name="token-cache-serialization-in-msalnet"></a>Serialisatie-tokencache in MSAL.NET
Na een [token wordt verkregen](msal-acquire-cache-tokens.md), het is in de cache opgeslagen door Microsoft Authentication Library (MSAL).  Toepassingscode moet proberen op te halen van een token uit de cache voor het ophalen van een token via een andere methode.  In dit artikel wordt beschreven standaard en aangepaste serialisatie van de tokencache in MSAL.NET.

In dit artikel is bedoeld voor MSAL.NET 3.x. Als u geïnteresseerd in MSAL.NET bent 2.x gebruikt, Zie [Token cache serialisatie in MSAL.NET 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Token-cache-serialization-2x).

## <a name="default-serialization-for-mobile-platforms"></a>Standaard-serialisatie voor mobiele platforms

In MSAL.NET, wordt een token in-memory-cache standaard opgegeven. Serialisatie wordt standaard opgegeven voor platforms waar beveiligde opslag beschikbaar voor een gebruiker als onderdeel van het platform is. Dit geldt voor het Universal Windows Platform (UWP), Xamarin.iOS en Xamarin.Android.

> [!Note]
> Wanneer u een Xamarin.Android-project migreert van MSAL.NET 1.x om MSAL.NET 3.x, kunt u om toe te voegen `android:allowBackup="false"` aan uw project om te voorkomen dat oude tokens van binnenkort terug bij het activeren van de Visual Studio-implementaties een herstelbewerking van lokale opslag in cache opgeslagen. Zie [uitgeven #659](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/659#issuecomment-436181938).

## <a name="custom-serialization-for-windows-desktop-apps-and-web-appsweb-apis"></a>Aangepaste serialisatie voor Windows-bureaublad-apps en web apps/web-API 's

Let op: aangepaste serialisatie is niet beschikbaar op mobiele platforms (UWP, Xamarin.iOS en Xamarin.Android). MSAL wordt al gedefinieerd voor een veilige en goed presterende mechanisme voor serialisatie voor deze platforms. .NET-desktop- en .NET Core-toepassingen, echter verschillende architecturen en MSAL een mechanisme voor algemeen gebruik serialisatie kan niet worden geïmplementeerd. Web sites kan bijvoorbeeld kiezen voor het opslaan van tokens in een Redis-cache of bureaublad-apps store tokens in een versleuteld bestand. Serialisatie is geen dus out-of-the-box opgegeven. Als u wilt een permanente tokencache-toepassing in .NET-desktop- of .NET Core, moet u de serialisatie aanpassen.

De volgende klassen en interfaces worden gebruikt in de serialisatie-tokencache:

- `ITokenCache`, waarmee wordt gedefinieerd gebeurtenissen abonneren op tokencache serialisatie aanvragen, evenals de methoden voor het serialiseren of deserialiseren van de cache in verschillende indelingen (ADAL v3.0, MSAL 2.x en MSAL 3.x ADAL 5.0 =).
- `TokenCacheCallback` een retouraanroep doorgegeven aan de gebeurtenissen zodat u de serialisatie kan verwerken. Deze wordt aangeroepen met argumenten van het type `TokenCacheNotificationArgs`.
- `TokenCacheNotificationArgs` biedt alleen de `ClientId` van de toepassing en een verwijzing naar de gebruiker waarvoor het token beschikbaar is.

  ![Klassediagram](media/msal-net-token-cache-serialization/class-diagram.png)

> [!IMPORTANT]
> MSAL.NET token caches voor u gemaakt en biedt u de `IToken` bij het aanroepen van een toepassing in de cache `GetUserTokenCache` en `GetAppTokenCache` methoden. U mag niet voor het implementeren van de interface. Uw eigen verantwoordelijkheid, wanneer u een serialisatie aangepaste tokencache implementeren is:
> - Reageren op `BeforeAccess` en `AfterAccess` "events". De `BeforeAccess` gemachtigde is verantwoordelijk voor het deserialiseren van de cache, terwijl de `AfterAccess` een is verantwoordelijk voor het serialiseren van de cache.
> - Onderdeel van deze gebeurtenissen opslaan of -blobs, die de gebeurtenis-argument worden doorgegeven aan de opslag die u wilt laden.

De strategieën zijn afhankelijk van verschillende als u een serialisatie-tokencache voor schrijft een [openbare clienttoepassing](msal-client-applications.md) (desktop), of een [vertrouwelijke clienttoepassing](msal-client-applications.md)) (web-app / web-API, daemon-app ).

### <a name="token-cache-for-a-public-client"></a>Tokencache voor een openbare client 

U hebt verschillende opties voor het serialiseren van de token cache van een openbare client sinds MSAL.NET v2.x. U kunt de cache alleen naar de MSAL.NET-indeling (de uniforme indeling-cache is gemeenschappelijk voor de platforms en MSAL) serialiseren.  U kunt ook ondersteuning voor de [verouderde](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) cache serialisatie van ADAL V3-token.

Aanpassen van de serialisatie-tokencache delen van de status voor eenmalige aanmelding tussen ADAL.NET 3.x ADAL.NET 5.x en MSAL.NET in het onderdeel van het volgende voorbeeld wordt uitgelegd: [active-directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2).

> [!Note]
> De indeling van MSAL.NET 1.1.4-preview tokencache wordt niet meer ondersteund in MSAL 2.x. Als u toepassingen met behulp van MSAL.NET 1.x, uw gebruikers moeten opnieuw-aanmelden-in. U kunt ook wordt de migratie van ADAL 4.x (en 3.x) ondersteund.

#### <a name="simple-token-cache-serialization-msal-only"></a>Eenvoudige tokencache serialisatie (alleen MSAL)

Hieronder volgt een voorbeeld van een implementatie naïve van aangepaste serialisatie van een token cache voor pc-toepassingen. Hier is het token cache van de gebruiker een bestand in dezelfde map als de toepassing.

Nadat u de toepassing bouwt, u de serialisatie inschakelen door het aanroepen van de `TokenCacheHelper.EnableSerialization()` methode en de toepassing doorgeven `UserTokenCache`.

```csharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

De `TokenCacheHelper` helperklasse wordt gedefinieerd als:

```csharp
static class TokenCacheHelper
 {
  public static void EnableSerialization(ITokenCache tokenCache)
  {
   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Path to the token cache
  /// </summary>
  public static readonly string CacheFilePath = System.Reflection.Assembly.GetExecutingAssembly().Location + ".msalcache.bin3";

  private static readonly object FileLock = new object();


  private static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeMsalV3(File.Exists(CacheFilePath)
            ? ProtectedData.Unprotect(File.ReadAllBytes(CacheFilePath),
                                      null,
                                      DataProtectionScope.CurrentUser)
            : null);
   }
  }

  private static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     // reflect changesgs in the persistent store
     File.WriteAllBytes(CacheFilePath,
                         ProtectedData.Protect(args.TokenCache.SerializeMsalV3(),
                                                 null,
                                                 DataProtectionScope.CurrentUser)
                         );
    }
   }
  }
 }
```

Een voorbeeld van een product kwaliteit tokencache bestand op basis van serializer voor openbare clienttoepassingen (voor bureaublad toepassingen die worden uitgevoerd op Windows, Mac en Linux) is beschikbaar via de [Microsoft.Identity.Client.Extensions.Msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal) Open-source-bibliotheek. U kunt opnemen in uw toepassingen uit de volgende nuget-pakket: [Microsoft.Identity.Client.Extensions.Msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

#### <a name="dual-token-cache-serialization-msal-unified-cache-and-adal-v3"></a>Dual-tokencache serialisatie (MSAL unified-cache en ADAL v3)

Als u wilt implementeren tokencache serialisatie, beide met de geïntegreerde cache-indeling (gemeenschappelijk ADAL.NET 4.x MSAL.NET 2.x en andere MSALs van dezelfde generatie of ouder, op hetzelfde platform), bekijk de volgende code:

```csharp
string appLocation = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location;
string cacheFolder = Path.GetFullPath(appLocation) + @"..\..\..\..");
string adalV3cacheFileName = Path.Combine(cacheFolder, "cacheAdalV3.bin");
string unifiedCacheFileName = Path.Combine(cacheFolder, "unifiedCache.bin");

IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
                                    .Build();
FilesBasedTokenCacheHelper.EnableSerialization(app.UserTokenCache,
                                               unifiedCacheFileName,
                                               adalV3cacheFileName);

```

Deze keer de helperklasse zoals is gedefinieerd als:

```csharp
using System;
using System.IO;
using System.Security.Cryptography;
using Microsoft.Identity.Client;

namespace CommonCacheMsalV3
{
 /// <summary>
 /// Simple persistent cache implementation of the dual cache serialization (ADAL V3 legacy
 /// and unified cache format) for a desktop applications (from MSAL 2.x)
 /// </summary>
 static class FilesBasedTokenCacheHelper
 {
  /// <summary>
  /// Get the user token cache
  /// </summary>
  /// <param name="adalV3CacheFileName">File name where the cache is serialized with the
  /// ADAL V3 token cache format. Can
  /// be <c>null</c> if you don't want to implement the legacy ADAL V3 token cache
  /// serialization in your MSAL 2.x+ application</param>
  /// <param name="unifiedCacheFileName">File name where the cache is serialized
  /// with the Unified cache format, common to
  /// ADAL V4 and MSAL V2 and above, and also across ADAL/MSAL on the same platform.
  ///  Should not be <c>null</c></param>
  /// <returns></returns>
  public static void EnableSerialization(ITokenCache cache, string unifiedCacheFileName, string adalV3CacheFileName)
  {
   usertokenCache = cache;
   UnifiedCacheFileName = unifiedCacheFileName;
   AdalV3CacheFileName = adalV3CacheFileName;

   usertokenCache.SetBeforeAccess(BeforeAccessNotification);
   usertokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Token cache
  /// </summary>
  static ITokenCache usertokenCache;

  /// <summary>
  /// File path where the token cache is serialized with the unified cache format
  /// (ADAL.NET V4, MSAL.NET V3)
  /// </summary>
  public static string UnifiedCacheFileName { get; private set; }

  /// <summary>
  /// File path where the token cache is serialized with the legacy ADAL V3 format
  /// </summary>
  public static string AdalV3CacheFileName { get; private set; }

  private static readonly object FileLock = new object();

  public static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeAdalV3(ReadFromFileIfExists(AdalV3CacheFileName));
    try
    {
     args.TokenCache.DeserializeMsalV3(ReadFromFileIfExists(UnifiedCacheFileName));
    }
    catch(Exception ex)
    {
     // Compatibility with the MSAL v2 cache if you used one
     args.TokenCache.DeserializeMsalV2(ReadFromFileIfExists(UnifiedCacheFileName));
    }
   }
  }

  public static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     WriteToFileIfNotNull(UnifiedCacheFileName, args.TokenCache.SerializeMsalV3());
     if (!string.IsNullOrWhiteSpace(AdalV3CacheFileName))
     {
      WriteToFileIfNotNull(AdalV3CacheFileName, args.TokenCache.SerializeAdalV3());
     }
    }
   }
  }

  /// <summary>
  /// Read the content of a file if it exists
  /// </summary>
  /// <param name="path">File path</param>
  /// <returns>Content of the file (in bytes)</returns>
  private static byte[] ReadFromFileIfExists(string path)
  {
   byte[] protectedBytes = (!string.IsNullOrEmpty(path) && File.Exists(path))
       ? File.ReadAllBytes(path) : null;
   byte[] unprotectedBytes = encrypt ?
       ((protectedBytes != null) ? ProtectedData.Unprotect(protectedBytes, null, DataProtectionScope.CurrentUser) : null)
       : protectedBytes;
   return unprotectedBytes;
  }

  /// <summary>
  /// Writes a blob of bytes to a file. If the blob is <c>null</c>, deletes the file
  /// </summary>
  /// <param name="path">path to the file to write</param>
  /// <param name="blob">Blob of bytes to write</param>
  private static void WriteToFileIfNotNull(string path, byte[] blob)
  {
   if (blob != null)
   {
    byte[] protectedBytes = encrypt
      ? ProtectedData.Protect(blob, null, DataProtectionScope.CurrentUser)
      : blob;
    File.WriteAllBytes(path, protectedBytes);
   }
   else
   {
    File.Delete(path);
   }
  }

  // Change if you want to test with an un-encrypted blob (this is a json format)
  private static bool encrypt = true;
 }
}
```

### <a name="token-cache-for-a-web-app-confidential-client-application"></a>Tokencache voor een web-app (servertoepassing vertrouwelijke client)

De cache kan gebruikmaken van de sessie, een Redis-cache of een database in de web-apps of web-API's.

Een belangrijke dingen om te onthouden is dat voor web-apps en web-API's, moet er een tokencache per gebruiker (per account). U moet de tokencache voor elk account serialiseren.

Voorbeelden van het token caches gebruiken voor web-apps en web-API's zijn beschikbaar in de [zelfstudie voor ASP.NET Core web-apps](https://ms-identity-aspnetcore-webapp-tutorial) in de fase [2-2-tokencache](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache). Voor implementaties een overzicht van de volgende map hebben [TokenCacheProviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) in de [microsoft-verificatie-extensies-voor-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) bibliotheek (in de [ Microsoft.Identity.Client.Extensions.Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) map. 

## <a name="next-steps"></a>Volgende stappen
De volgende voorbeelden laten zien tokencache serialisatie.

| Voorbeeld | Platform | Description|
| ------ | -------- | ----------- |
|[active-directory-dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | Desktop (WPF) | Windows Desktop .NET (WPF)-toepassing voor het aanroepen van de Microsoft Graph API. ![Topologie](media/msal-net-token-cache-serialization/topology.png)|
|[active-directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2) | Bureaublad (Console) | Reeks oplossingen voor Visual Studio ter illustratie van de migratie van Azure AD v1.0-toepassingen (met behulp van ADAL.NET) naar Azure AD v2.0-toepassingen, ook met de naam geconvergeerde toepassingen (met behulp van MSAL.NET), met name [Token Cache-migratie](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2/blob/master/TokenCacheMigration/README.md)|