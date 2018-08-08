---
title: Azure AD-iOS aan de slag | Microsoft Docs
description: Over het bouwen van een iOS-toepassing die kan worden geïntegreerd met Azure AD voor aanmelden en Azure AD-aanroepen beveiligd API's met behulp van OAuth.
services: active-directory
documentationcenter: ios
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: jmprieur
ms.openlocfilehash: c370a90cf050a88e66ea0417f018429f7815b7c9
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592234"
---
# <a name="azure-ad-ios-getting-started"></a>Azure AD-iOS aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Azure Active Directory (Azure AD) biedt de Active Directory Authentication Library of ADAL voor iOS-clients die toegang moeten krijgen tot beveiligde bronnen. ADAL vereenvoudigt het proces dat uw app gebruikmaakt van toegangstokens te verkrijgen. Om te laten zien hoe eenvoudig het is, in dit artikel wordt een takenlijst voor Objective-C-toepassing ontwikkelen die:

* Hiermee toegang tot tokens voor het aanroepen van de Azure AD Graph API met behulp van de [OAuth 2.0-verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Zoekt naar een map voor gebruikers met een opgegeven alias.

Voor het bouwen van de volledige werkende toepassing, moet u naar:

1. Uw toepassing registreren met Azure AD.
2. Installeer en configureer ADAL.
3. Gebruikmaken van ADAL om op te halen van tokens van Azure AD.

Aan de slag [het app-basisproject downloaden](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) of [downloaden van het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). U moet ook een Azure AD-tenant in die u kunt gebruikers maken en registreren van een toepassing. Als u nog een tenant hebt [informatie over het verkrijgen van een](quickstart-create-new-tenant.md).


> [!TIP]
> Probeer de preview van onze nieuwe [ontwikkelaarsportal](https://identity.microsoft.com/Docs/iOS) waarmee u kunt aan de slag met Azure AD in een paar minuten. De developer-portal leidt u door het proces van een app registreren en Azure AD integreren in uw code. Wanneer u klaar bent, hebt u een eenvoudige toepassing die gebruikers in uw tenant kan verifiëren en een back-end die kunnen tokens accepteren en valideren. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Bepalen welke uw omleidings-URI voor iOS is
Voor het veilig starten van uw toepassingen in bepaalde scenario's met eenmalige aanmelding, moet u een *omleidings-URI* in een bepaalde opmaak. Een omleidings-URI wordt gebruikt om ervoor te zorgen dat de tokens terugkeren naar de juiste toepassing die voor hen gevraagd.


De iOS-indeling voor een omleidings-URI is:

```
<app-scheme>://<bundle-id>
```

* **App-schema** -dit is geregistreerd in uw XCode-project. Het is hoe andere toepassingen u kunnen aanroepen. U vindt dit onder Info.plist -> URL-types-URL-id >. Als u nog een of meer geconfigureerd hebt, moet u een maken.
* **bundel-id** -dit is de bundel-id te vinden onder 'id' in de instellingen van uw XCode-project.

Een voorbeeld voor deze Quick Start-code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. De toepassing DirectorySearcher registreren
Als u uw app instelt om op te halen van tokens, moet u eerst deze te registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph-API:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Op de bovenste balk, klikt u op uw account. Onder de **Directory** kiest u de Active Directory-tenant waar u uw toepassing registreren.
3. Klik op **alle services** in de meest linkse navigatievenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App-registraties**, en selecteer vervolgens **toevoegen**.
5. Volg de aanwijzingen voor het maken van een nieuwe **systeemeigen clienttoepassing**.
  * De **naam** beschrijving van de toepassing van uw toepassing aan eindgebruikers.
  * De **omleidings-Uri** is een combinatie van schema en de tekenreeks die Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde die is specifiek voor uw toepassing en is gebaseerd op de bovenstaande informatie van de omleidings-URI.
6. Nadat u de registratie hebt voltooid, wijst Azure AD uw app een unieke toepassings-ID. U moet deze waarde in de volgende secties, dus kopieer deze op het toepassingstabblad.
7. Uit de **instellingen** weergeeft, schakelt **vereiste machtigingen** en selecteer vervolgens **toevoegen**. Selecteer **Microsoft Graph** als de API, en voeg de **mapgegevens lezen** machtiging onder **gedelegeerde machtigingen**. Hiermee stelt u uw toepassing om te vragen uit de Azure AD Graph-API voor gebruikers.

## <a name="3-install-and-configure-adal"></a>3. Installeren en configureren van ADAL
Nu dat u een toepassing in Azure AD hebt, kunt u ADAL installeert en uw identiteit gerelateerde code te schrijven. Voor de ADAL om te communiceren met Azure AD, moet u deze voorzien van enkele gegevens over uw app-registratie.

1. Beginnen met het toevoegen van ADAL voor het project DirectorySearcher via CocoaPods.

    ```
    $ vi Podfile
    ```
2. Voeg het volgende aan de podfile toe:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Laad de podfile nu met CocoaPods. Deze stap maakt u een nieuwe XCode-werkruimte die u hebt geladen.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Open de plist-bestand in de Quick Start-project `settings.plist`. Vervang de waarden van de elementen in de sectie in overeenstemming met de waarden die u hebt ingevoerd in de Azure-portal. Uw code wordt verwezen naar deze waarden wanneer deze gebruikmaakt van ADAL.
  * De `tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
  * De `clientId` is de client-ID van uw toepassing die u hebt gekopieerd uit de portal.
  * De `redirectUri` is de omleidings-URL die u in de portal hebt geregistreerd.

## <a name="4-use-adal-to-get-tokens-from-azure-ad"></a>4. Gebruikmaken van ADAL om op te halen van tokens van Azure AD
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, deze een completionBlock roept `+(void) getToken : `, en doet de rest van ADAL. 

1. In de `QuickStart` project, open `GraphAPICaller.m` en zoek de `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` opmerking aan de bovenkant. Dit is waar het doorgeven van ADAL de coördinaten via een CompletionBlock, om te communiceren met Azure AD en hoe deze tokens in de cache.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Nu we moeten dit token wordt gebruikt om te zoeken naar gebruikers in de grafiek. Zoek de `// TODO: implement SearchUsersList` opmerking. Deze methode maakt een GET-aanvraag voor de Azure AD Graph-API aan query voor gebruikers wiens UPN met de opgegeven zoekterm begint. Als u wilt de Azure AD Graph-API een query uitvoert, moet u een access_token in de `Authorization` -header van de aanvraag. Dit is waar de ADAL wordt geleverd.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```


3. Wanneer uw app een token aanvraagt door het aanroepen van `getToken(...)`, ADAL probeert te retourneren van een token zonder dat de gebruiker om referenties wordt gevraagd. Als de ADAL wordt vastgesteld dat de gebruiker moet zich aanmelden bij een token verkrijgen, wordt een dialoogvenster voor aanmelding bij weergeven, verzamelen van de referenties van de gebruiker en retourneert een token na een geslaagde authenticatie. Als u ADAL niet kan worden geretourneerd van een token voor een bepaalde reden, dit genereert een `AdalException`.

> [!Note] 
> De `AuthenticationResult` -object bevat een `tokenCacheStoreItem` -object dat kan worden gebruikt voor het verzamelen van de informatie die uw app mogelijk nodig hebt. In de snelstartgids `tokenCacheStoreItem` wordt gebruikt om te bepalen als verificatie is al voltooid.
>
>

## <a name="5-build-and-run-the-application"></a>5. De toepassing bouwen en uitvoeren.
Gefeliciteerd! U hebt nu een werkende iOS-toepassing die kan gebruikers verifiëren, veilig aanroepen van Web-API's met behulp van OAuth 2.0 en elementaire informatie over de gebruiker. Als u niet hebt gedaan, is nu de tijd voor het vullen van uw tenant waarbij sommige gebruikers. Start uw Quick Start-app en meld u aan met een van deze gebruikers. Zoeken naar andere gebruikers op basis van hun UPN. Sluit de app en start het opnieuw. U ziet dat de sessie van de gebruiker intact blijft.

ADAL kunt eenvoudig opnemen van al deze algemene identiteitsfuncties in uw toepassing. Dit zorgt dat al het werk dirty bij u past, Cachebeheer, zoals OAuth-protocolondersteuning, dat de gebruiker met een gebruikersinterface aan te melden bij, en vernieuwen van tokens verlopen. Alles wat u moet weten is dat één API-aanroep, `getToken`.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is beschikbaar op [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). 

## <a name="next-steps"></a>Volgende stappen
U kunt nu verder met aanvullende scenario's. U wilt proberen:

* [Een Node.JS-Web-API met Azure AD beveiligen](quickstart-v1-nodejs-webapi.md)
* Informatie over [SSO cross-app voor iOS met behulp van ADAL inschakelen](howto-v1-enable-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

