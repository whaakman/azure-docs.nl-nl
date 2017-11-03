---
title: Verificatie op basis van tokens (HTTP/2) voor APNS in Azure Notification Hubs | Microsoft Docs
description: Dit onderwerp wordt uitgelegd hoe u de authenticatie van het nieuwe token voor APNS
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 5a21bcd9f12fc3f96b17a556ba15526c35ababe2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="token-based-http2-authentication-for-apns"></a>Verificatie op basis van tokens (HTTP/2) voor APNS
## <a name="overview"></a>Overzicht
Dit artikel wordt uitgelegd hoe u het nieuwe APNS HTTP/2-protocol met token gebaseerde authenticatie.

De belangrijkste voordelen van het gebruik van het nieuwe protocol zijn onder andere:
-   Token genereren is relatief probleemloos (vergeleken met certificaten)
-   Er is geen meer vervaldatums – zijn controle over uw verificatietokens en hun intrekken
-   Nettoladingen kunnen nu worden tot 4 KB
- Synchrone feedback
-   U bent op de meest recente protocol van Apple-certificaten gebruiken nog steeds het binaire protocol, dat is gemarkeerd voor afschaffing

Met deze nieuwe mechanisme kan worden uitgevoerd in twee stappen in een paar minuten:
1.  De benodigde informatie verkrijgen via de portal Apple Developer-Account
2.  Uw notification hub configureren met de nieuwe informatie

Notification Hubs is nu klaar voor gebruik van het nieuwe verificatiesysteem met APNS. 

Houd er rekening mee dat als u een migratie van met referenties van certificaat voor APNS:
- de token eigenschappen overschrijven uw certificaat in ons systeem
- maar de toepassing blijft naadloos meldingen ontvangen.

## <a name="obtaining-authentication-information-from-apple"></a>Verificatie-informatie verkrijgen van Apple
Op tokens gebaseerde verificatie inschakelt, moet u de volgende eigenschappen uit uw Apple Developer-Account:
### <a name="key-identifier"></a>Sleutel-id
De sleutel-id kan worden verkregen via de pagina 'Sleutels' in de Apple Developer-Account

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Toepassings-id & toepassingsnaam
Naam van de toepassing is beschikbaar via de pagina App-id's in het Developer-Account. 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

De toepassings-id is beschikbaar via de pagina lidmaatschap details in het Developer-Account.
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a>Verificatietoken
Het verificatietoken kan worden gedownload nadat u een token voor uw toepassing genereren. Raadpleeg voor meer informatie over het maken van dit token [van Apple-documentatie voor ontwikkelaars](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a>Configureren van uw notification hub voor verificatie op basis van token
### <a name="configure-via-the-azure-portal"></a>Configureren via de Azure portal
Aanmelden bij de Azure-portal om tokens gebaseerde verificatie in de portal, en Ga naar uw Notification Hub > Notification Services > APNS Configuratiescherm. 

Er is een nieuwe eigenschap – *verificatiemodus*. Token kunt u uw hub bijwerken met de relevante token eigenschappen.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- Geef de eigenschappen die u hebt opgehaald via het Apple developer-account 
- Kies uw toepassingsmodus (productie of Sandbox) 
- Klik op Opslaan om uw APNS-referenties bijwerken. 

### <a name="configure-via-management-api-rest"></a>Configureren via beheer-API (REST)

U kunt onze [management API's](https://msdn.microsoft.com/library/azure/dn495827.aspx) bijwerken van uw notification hub voor verificatie op basis van een token.
Afhankelijk van of is de toepassing die u configureert een Sandbox of productie-app (opgegeven in uw ontwikkelaarsaccount Apple), een van de bijbehorende eindpunten te gebruiken:

- Sandboxeindpunt: [3-https://api.development.push.apple.com:443-apparaat](https://api.development.push.apple.com:443/3/device)
- Productie-eindpunt: [3-https://api.push.apple.com:443-apparaat](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Verificatie op basis van tokens vereist een API-versie van: **2017 04 of hoger**.
> 
> 

Hier volgt een voorbeeld van een PUT-aanvraag voor het bijwerken van een hub met verificatie op basis van tokens:


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-the-net-sdk"></a>Configureren via de .NET SDK
U kunt uw hub voor het gebruik van tokens gebaseerde verificatie met behulp van onze [nieuwste client SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8). 

Hier volgt een voorbeeld van code ter illustratie van de juiste syntaxis:


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-to-using-certificate-based-authentication"></a>Herstellen van verificatie op basis van certificaat
U kunt op elk gewenst moment herstellen voor het gebruik van verificatie op basis van certificaten met behulp van een voorgaande methode en het certificaat in plaats van de eigenschappen van het token wordt doorgegeven. Deze actie worden de eerder opgeslagen referenties overschreven.
