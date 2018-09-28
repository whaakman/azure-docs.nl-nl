---
title: Een iOS-app bouwen die is geïntegreerd met Azure AD voor aanmelding en die beschermde API's aanroept met behulp van OAuth 2.0 | Microsoft Docs
description: Informatie over hoe u gebruikers aanmeldt en de Microsoft Graph API aanroept vanuit uw eigen iOS-app.
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
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: brandwe
ms.openlocfilehash: 89f2a4058006687fbe64ec64d98659e38f93f618
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980573"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Snelstart: Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een iOS-app

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Azure Active Directory (Azure AD) biedt de ADAL (Active Directory Authentication Library) voor iOS-clients die toegang nodig hebben tot beveiligde bronnen. ADAL vereenvoudigt het proces dat door uw app wordt gebruikt om toegangstokens te verkrijgen. 

In deze snelstart maakt u een Objective C-toepassing voor een takenlijst die:

* Toegangstokens verkrijgt om de Azure AD Graph API aan te roepen met behulp van het OAuth 2.0-verificatieprotocol
* In een map zoekt naar gebruikers met een gegeven alias

Om de volledige, werkende toepassing te bouwen, moet u het volgende doen:

1. Registreer de toepassing bij Azure AD.
1. Installeer en configureer ADAL.
1. Gebruik ADAL om tokens op te halen uit Azure AD.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorgt u dat u aan deze vereisten voldoet:

* [Download het raamwerk van de app](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) of [download het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).
* Zorg voor een Azure AD-tenant waarin u gebruikers kunt maken en een toepassing kunt registreren. Als u nog geen tenant hebt, vindt u [hier informatie over het verkrijgen van een tenant](quickstart-create-new-tenant.md).

> [!TIP]
> Probeer de [ontwikkelaarsportal](https://identity.microsoft.com/Docs/iOS) als u binnen enkele minuten aan de slag wilt gaan met Azure AD. De ontwikkelaarsportal leidt u door het proces van het registreren van de app en de integratie van Azure AD in uw code. Wanneer u klaar bent, hebt u een eenvoudige toepassing die gebruikers in uw tenant kan verifiëren, en een back-end die tokens kan accepteren en valideren.

## <a name="step-1-determine-what-your-redirect-uri-is-for-ios"></a>Stap 1: Uw omleidings-URI voor iOS bepalen

Om uw toepassingen in bepaalde scenario's veilig te kunnen starten, moet u een *omleidings-URI* in een bepaalde opmaak maken. Een omleidings-URI wordt gebruikt om ervoor te zorgen dat de tokens terugkomen bij de juiste toepassing die erom heeft gevraagd.

De iOS-indeling voor een omleidings-URI is:

```
<app-scheme>://<bundle-id>
```

* **App-schema**: is geregistreerd in uw XCode-project en bepaalt hoe andere toepassingen uw toepassing kunnen aanroepen. U vindt het app-schema in **Info.plist > URL types > URL Identifier**. Maak een app-schema als u er nog geen hebt geconfigureerd.
* **bundel-id**: is de bundel-id die wordt weergegeven als **identiteit** in de XCode-projectinstellingen.

Een voorbeeld voor deze snelstartcode:

***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="step-2-register-the-directorysearcher-application"></a>Stap 2: De toepassing DirectorySearcher registreren

Als u de app wilt instellen voor het ophalen van tokens, moet u de app registreren in uw Azure AD-tenant en de app toegang geven tot de Azure AD Graph API.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer uw account op de bovenste balk. Kies onder **Directory** de Active Directory-tenant waar u uw toepassing wilt registreren.
3. Selecteer **Alle services** in het linkernavigatievenster en selecteer vervolgens **Azure Active Directory**.
4. Selecteer **App-registraties** en kies **Toevoegen**.
5. Volg de aanwijzingen voor het maken van een nieuwe **systeemeigen** clienttoepassing.
    * **Naam** is de naam van de toepassing en beschrijft de toepassing voor eindgebruikers.
    * **Omleidings-URI** is een combinatie van een schema en een tekenreeks die door Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde in die specifiek is voor uw toepassing en die is gebaseerd op bovenstaande informatie over de omleidings-URI.
6. Wanneer de registratie is voltooid, wijst Azure AD een unieke toepassings-id toe aan uw app. U hebt deze waarde nodig in de volgende secties. Kopieer deze daarom vanaf het toepassingstabblad.
7. Selecteer op de pagina **Instellingen** de optie **Vereiste machtigingen > Toevoegen > Microsoft Graph** en voeg onder **Gedelegeerde machtigingen** de machtiging **Mapgegevens lezen** toe. Hiermee machtigt u de toepassing om gegevens uit de Azure AD Graph-API op te vragen voor gebruikers.

## <a name="step-3-install-and-configure-adal"></a>Stap 3: ADAL installeren en configureren

Nu u een toepassing hebt in Azure AD, kunt u ADAL installeren en uw aan identiteit gerelateerde code schrijven. Als u wilt dat ADAL kan communiceren met Azure AD, moet u ADAL voorzien van bepaalde informatie gegevens over uw app-registratie.

1. Eerst moet u ADAL met behulp van CocoaPods toevoegen aan het DirectorySearcher-project.

    ```
    $ vi Podfile
    ```
1. Voeg het volgende aan de podfile toe:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

1. Laad de podfile met behulp van CocoaPods. Met deze stap maakt en laadt u een nieuwe XCode-werkruimte.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

1. Open in het snelstartproject het plist-bestand `settings.plist`.
1. Vervang de waarden van de elementen in de sectie overeenkomstig de waarden die u hebt ingevoerd in de Azure Portal. Uw code verwijst naar deze waarden wanneer deze gebruikmaakt van ADAL.
    * `tenant` is het domein van de Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    * `clientId` is de client-id van uw toepassing die u hebt gekopieerd uit de portal.
    * `redirectUri` is de omleidings-URL die u in de portal hebt geregistreerd.

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>Stap 4: ADAL gebruiken om tokens op te halen uit Azure AD

Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken nodig heeft, deze gewoon een completionBlock `+(void) getToken : `aanroept, waarna ADAL de rest doet.

1. Open `GraphAPICaller.m` in het project `QuickStart` en zoek ergens bovenaan de opmerking `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.`.

    Dit is waar u ADAL via een CompletionBlock de coördinaten doorgeeft om te communiceren met Azure AD en waar u opgeeft hoe deze tokens in de cache moeten worden opgeslagen.

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

2. U moet deze token gebruiken om gebruikers in de graaf te zoeken. Zoek de opmerking `// TODO: implement SearchUsersList`. Met deze methode maakt u een GET-aanvraag voor de Azure AD Graph API om gebruikers op te vragen van wie de UPN begint met de opgegeven zoekterm. 

    Als u gegevens wilt opvragen uit de Azure AD Graph API, moet u een access_token opnemen in de `Authorization`-header van de aanvraag. En hier komt ADAL in actie.

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

3. Wanneer uw app een token aanvraagt door `getToken(...)` aan te roepen, probeert ADAL een token te retourneren zonder de gebruiker om referenties te vragen. Als ADAL bepaalt dat de gebruiker zich moet aanmelden om een token te verkrijgen, wordt er een aanmeldingsvenster weergegeven, worden de referenties van de gebruiker verzameld en wordt er na een geslaagde verificatie een token geretourneerd. Als ADAL om welke reden dan ook geen token kan retourneren, wordt er een `AdalException` gegenereerd.

> [!NOTE]
> Het object `AuthenticationResult` bevat een object `tokenCacheStoreItem` dat u kunt gebruiken om de informatie te verzamelen die uw app mogelijk nodig heeft. In de snelstart wordt `tokenCacheStoreItem` gebruikt om te bepalen of de verificatie is al voltooid.

## <a name="step-5-build-and-run-the-application"></a>Stap 5: De toepassing bouwen en uitvoeren

Gefeliciteerd! U hebt nu een werkende iOS-toepassing die gebruikers kan verifiëren, Web-API's veilig kan aanroepen met behulp van OAuth 2.0, en basisinformatie over de gebruiker kan verkrijgen.

Nu kunt u de tenant met gebruikers gaan vullen als u dat nog niet hebt gedaan.

1. Start snelstart-app en meld u aan met een van deze gebruikers.
1. Zoek andere gebruikers op basis van hun UPN.
1. Sluit de app en start deze opnieuw. U ziet dat de sessie van de gebruiker intact is gebleven.

Met ADAL kunt u eenvoudig al deze algemene identiteitsfuncties in uw toepassing opnemen. ADAL neemt alle vervelende klusjes voor zijn rekening, zoals cachebeheer, OAuth-protocolondersteuning, het aanbieden van een gebruikersinterface waarmee de gebruiker zich kan aanmelden, en het vernieuwen van verlopen tokens. Het enige dat u hoeft te weten, is één API-aanroep, `getToken`.

Als naslaginformatie wordt het volledige voorbeeld (zonder uw configuratiewaarden) gegeven op [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).

## <a name="next-steps"></a>Volgende stappen

Nu kunt u verder met aanvullende scenario's. U wordt geadviseerd eerst de volgende te proberen:

* [Een Node.JS Web API beveiligen met Azure AD](quickstart-v1-nodejs-webapi.md)
* Informatie over [SSO tussen apps inschakelen voor iOS met behulp van ADAL](howto-v1-enable-sso-ios.md)  
