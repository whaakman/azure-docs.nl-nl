---
title: Verificatie op basis van tokens (HTTP/2) voor APN's in Azure Notification Hubs | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe u gebruik kunt maken van de nieuwe tokenverificatie voor APN 's
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 02/13/2019
ms.author: jowargo
ms.openlocfilehash: 890577c013a96fc06acf3b05881649ad8202a083
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56671638"
---
# <a name="token-based-http2-authentication-for-apns"></a>Verificatie op basis van tokens (HTTP/2) voor APN 's

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe het gebruik van het nieuwe APNS HTTP/2-protocol met tokens gebaseerde verificatie.

De belangrijkste voordelen van het gebruik van het nieuwe-protocol zijn:

* Genereren van tokens is relatief probleemloos (vergeleken met certificaten)
* Er is geen meer vervaldatums – zijn controle over uw verificatietokens en hun intrekken
* Nettoladingen kunnen nu worden maximaal 4 KB
* Synchrone feedback
* U bent op de meest recente protocol van Apple-certificaten gebruiken nog steeds het binaire protocol, dat is gemarkeerd voor afschaffen

Met deze nieuwe methode kan worden uitgevoerd in twee stappen in een paar minuten:

1. De benodigde informatie verkrijgen via de portal voor Apple Developer-Account
2. Uw notification hub configureren met de nieuwe informatie

Notification Hubs is nu klaar voor gebruik van het nieuwe verificatiesysteem met APNS.

Merk op dat als u een migratie van met behulp van referenties van het computercertificaat voor APN's:

* de tokeneigenschappen overschrijven uw certificaat in ons systeem
* maar om meldingen te ontvangen naadloos blijft uw toepassing.

## <a name="obtaining-authentication-information-from-apple"></a>Het ophalen van verificatiegegevens van Apple

Als u wilt inschakelen op tokens gebaseerde verificatie, moet u de volgende eigenschappen van uw Apple Developer-Account:

### <a name="key-identifier"></a>Id van sleutel

De sleutel-id kan worden verkregen via de pagina 'Sleutels' in uw Apple Developer-Account

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Toepassings-id & toepassingsnaam

Naam van de toepassing is beschikbaar via de pagina App-id's in de Developer-Account.

![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

De toepassings-id is beschikbaar via de pagina met het lidmaatschap in de Developer-Account.

![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)

### <a name="authentication-token"></a>Het verificatietoken

Het verificatietoken kan worden gedownload nadat u een token voor uw toepassing genereren. Raadpleeg voor meer informatie over het maken van dit token [van Apple-documentatie voor ontwikkelaars](https://help.apple.com/xcode/mac/current/#/devdfd3d04a1).

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a>Uw notification hub voor het gebruik van verificatie op basis van tokens configureren

### <a name="configure-via-the-azure-portal"></a>Configureren via Azure portal

Als u wilt inschakelen op tokens gebaseerde verificatie in de portal, meld u aan bij de Azure-portal en Ga naar uw Notification Hub > Notification Services > Configuratiescherm APNS.

Er is een nieuwe eigenschap, *verificatiemodus*. Token selecteren, kunt u uw hub bijwerken met de relevante token eigenschappen.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

* Voer de eigenschappen die u hebt opgehaald via uw Apple developer-account
* Kies uw toepassingsmodus (productie- of Sandbox)
* Klik op de **opslaan** om uw APNS-referenties bijwerken

### <a name="configure-via-management-api-rest"></a>Configureren via de beheer-API (REST)

U kunt onze [beheer-API's](https://msdn.microsoft.com/library/azure/dn495827.aspx) om bij te werken van uw notification hub voor het gebruik van verificatie op basis van tokens.
Afhankelijk van of de toepassing die u configureert een Sandbox of productie-app (opgegeven in uw Apple Developer-Account) is, moet u een van de bijbehorende eindpunten gebruiken:

* Sandboxeindpunt: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)
* Productie-eindpunt: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Verificatie op basis van tokens vereist een API-versie van: **2017-04 of hoger**.

Hier volgt een voorbeeld van een PUT-aanvraag voor het bijwerken van een hub met verificatie op basis van tokens:

    ```text
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
    ```

### <a name="configure-via-the-net-sdk"></a>Configureren via de .NET SDK

U kunt uw hub voor het gebruik van tokens gebaseerde verificatie met behulp van onze [nieuwste client SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).

Hier volgt een voorbeeld van code ter illustratie van de juiste syntaxis:

```csharp
NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
string token = "YOUR TOKEN HERE";
string keyId = "YOUR KEY ID HERE";
string appName = "YOUR APP NAME HERE";
string appId = "YOUR APP ID HERE";
NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
nm.UpdateNotificationHubAsync(desc);
```

## <a name="reverting-to-using-certificate-based-authentication"></a>Herstellen van de verificatie op basis van certificaat

U kunt op elk gewenst moment herstellen voor het gebruik van verificatie op basis van certificaten met behulp van een voorgaande methode en het certificaat in plaats van de eigenschappen van het token wordt doorgegeven. Deze actie wordt de eerder opgeslagen referenties overschreven.
