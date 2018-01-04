---
title: Azure AD iOS aan de slag | Microsoft Docs
description: "Het bouwen van een iOS-toepassing die kan worden geïntegreerd met Azure AD voor aanmelden en Azure AD-aanroepen beveiligd API's met behulp van OAuth."
services: active-directory
documentationcenter: ios
author: brandwe
manager: mtillman
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 11/30/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 36c6f6d2449d1e137f85e0f657f0399f9df8ee55
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-ad-ios-getting-started"></a>Azure AD iOS aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Azure Active Directory (Azure AD) biedt de Active Directory Authentication Library of ADAL voor iOS-clients die toegang moeten krijgen tot beveiligde bronnen. ADAL vereenvoudigt het proces dat uw app gebruikmaakt van toegangstokens te verkrijgen. Om te demonstreren hoe eenvoudig het is, in dit artikel gaan we verder met een takenlijst Objective-C-toepassing die:

* Krijgt toegang tot tokens voor de Azure AD Graph-API aanroept met behulp van de [OAuth 2.0-verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Zoekt een directory voor gebruikers met een alias voor een gegeven.

De volledige werkende toepassing bouwen, moet u:

1. Uw toepassing registreren met Azure AD.
2. Installeren en configureren van ADAL.
3. ADAL gebruikt om tokens van Azure AD.

Aan de slag [de basis van de app downloaden](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) of [het voltooide voorbeeld downloaden](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). U moet ook een Azure AD-tenant kunt u gebruikers maken en een toepassing registreren. Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).


> [!TIP]
> Deze Preview-versie van onze nieuwe [ontwikkelaarsportal](https://identity.microsoft.com/Docs/iOS) waarmee u leren werken met Azure AD in een paar minuten. De portal voor ontwikkelaars leidt u door het proces van registreren van een app en Azure AD integreren in uw code. Wanneer u klaar bent, hebt u een eenvoudige toepassing die u kunt verificatie van gebruikers in uw tenant en een back-endnetwerk, kan tokens accepteren en valideren. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Bepalen welke uw omleidings-URI voor iOS is
U start uw toepassingen veilig in bepaalde SSO-scenario's, moet u een *omleidings-URI* in een bepaalde opmaak. Een omleidings-URI wordt gebruikt om ervoor te zorgen dat de tokens terugkeren naar de juiste toepassing die voor hen gevraagd.


De iOS-indeling voor een omleidings-URI is:

```
<app-scheme>://<bundle-id>
```

* **App-schema** -dit is geregistreerd in uw XCode-project. Het is hoe andere toepassingen u kunnen aanroepen. U vindt dit onder Info.plist -> URL typen-URL-id >. Als u nog een of meer geconfigureerd hebt, moet u een maken.
* **bundel-id** -dit is de bundel-id vinden onder 'id' in de instellingen van uw XCode-project.

Een voorbeeld van deze Quick Start-code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. De toepassing DirectorySearcher registreren
Als u uw app om op te halen van tokens instelt, moet u eerst registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph API:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. Onder de **Directory** kiest u de Active Directory-tenant waar u uw toepassing registreren.
3. Klik op **meer Services** in de meest linkse navigatievenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Volg de aanwijzingen voor het maken van een nieuwe **systeemeigen clienttoepassing**.
  * De **naam** beschrijving van de toepassing van uw toepassing aan eindgebruikers.
  * De **omleidings-Uri** is een combinatie schema en de tekenreeks die gebruikmaakt van Azure AD token antwoorden retourneren.  Voer een waarde die specifiek is voor uw toepassing en is gebaseerd op de vorige omleidings-URI-gegevens.
6. Nadat u de registratie hebt voltooid, wijst Azure AD van uw app een unieke toepassings-ID.  U moet deze waarde in de volgende secties, dus kopiëren vanaf het toepassingstabblad.
7. Van de **instellingen** pagina **Required Permissions** en selecteer vervolgens **toevoegen**. Selecteer **Microsoft Graph** als de API en voeg vervolgens de **Directory-gegevens lezen** machtiging onder **gedelegeerde machtigingen**.  Hiermee stelt u uw toepassing query uitvoeren op de Azure AD Graph API voor gebruikers.

## <a name="3-install-and-configure-adal"></a>3. Installeren en configureren van ADAL
Nu dat u een toepassing in Azure AD hebt, kunt u ADAL installeert en uw identiteitsgerelateerde code schrijven.  Voor ADAL om te communiceren met Azure AD, moet u voorzien enige informatie over de registratie van uw app.

1. Beginnen met het ADAL toevoegen aan het project DirectorySearcher via CocoaPods.

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

3. Nu de podfile laden via CocoaPods. Deze stap maakt een nieuwe XCode-werkruimte die u hebt geladen.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Open het plist-bestand in de Quick Start-project `settings.plist`.  Vervang de waarden van de elementen in de sectie in overeenstemming met de waarden die u hebt ingevoerd in de Azure-portal. Uw code verwijst naar deze waarden wanneer deze gebruikmaakt van ADAL.
  * De `tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld: contoso.onmicrosoft.com.
  * De `clientId` is de client-ID van uw toepassing die u hebt gekopieerd uit de portal.
  * De `redirectUri` is de omleidings-URL die u hebt geregistreerd in de portal.

## <a name="4----use-adal-to-get-tokens-from-azure-ad"></a>4.    Gebruikmaken van ADAL voor het ophalen van tokens van Azure AD
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, een completionBlock aanroept `+(void) getToken : `, en doet de rest van ADAL.  

1. In de `QuickStart` project, open `GraphAPICaller.m` en zoek de `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` opmerking aan de bovenkant.  Dit is waar het doorgeven van ADAL de coördinaten via een CompletionBlock, om te communiceren met Azure AD en hoe deze tokens in de cache.

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

2. Nu moet dit token gebruiken om te zoeken naar gebruikers in de grafiek. Zoek de `// TODO: implement SearchUsersList` opmerking. Deze methode maakt een GET-aanvraag voor Azure AD Graph API aan query voor gebruikers wiens UPN met de opgegeven zoekterm begint.  Om te vragen van de Azure AD Graph API, moet u een access_token in de `Authorization` koptekst van de aanvraag. Dit is waar de ADAL wordt geleverd.

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


3. Wanneer uw app een token aanvragen door het aanroepen van `getToken(...)`, ADAL probeert te retourneren van een token zonder dat de gebruiker om referenties gevraagd.  Als ADAL wordt vastgesteld dat de gebruiker zich aanmelden moet bij een token verkrijgen, wordt deze weergegeven in het dialoogvenster voor aanmelding, verzamelen van referenties van de gebruiker en retourneert een token na een geslaagde authenticatie.  Als ADAL niet kan worden geretourneerd van een token voor een bepaalde reden, genereert een `AdalException`.

> [!Note] 
> De `AuthenticationResult` object bevat een `tokenCacheStoreItem` -object dat kan worden gebruikt voor het verzamelen van de informatie die uw app mogelijk nodig. In de snelstartgids `tokenCacheStoreItem` wordt gebruikt om te bepalen of de verificatie al wordt uitgevoerd.
>
>

## <a name="5-build-and-run-the-application"></a>5. De toepassing bouwen en uitvoeren.
Gefeliciteerd! U hebt nu een werkende iOS-toepassing kunt verificatie van gebruikers, veilig aanroepen van Web-API's met behulp van OAuth 2.0 en algemene informatie over de gebruiker ophalen.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van uw tenant waarbij sommige gebruikers.  Start uw app Quick Start en vervolgens weer aanmelden met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van de UPN.  Sluit de app en start het opnieuw.  U ziet dat de gebruikerssessie intact blijft.

ADAL kunt eenvoudig gebruikmaken van al deze algemene identiteit functies in uw toepassing.  Dit zorgt voor al het werk dirty voor u, zoals het Cachebeheer van de OAuth-protocolondersteuning, dat de gebruiker een gebruikersinterface aan te melden, en vernieuwen van tokens verlopen.  Alles wat u moet weten één API-aanroep is `getToken`.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) wordt aangeboden op [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="next-steps"></a>Volgende stappen
U kunt nu verder met aanvullende scenario's.  U wilt proberen:

* [Een Node.JS-Web-API met Azure AD beveiligen](active-directory-devquickstarts-webapi-nodejs.md)
* Meer informatie over [het inschakelen van eenmalige aanmelding voor cross-app voor iOS met ADAL](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

