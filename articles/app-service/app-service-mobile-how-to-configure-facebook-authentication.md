---
title: Facebook-verificatie voor uw toepassing App Services configureren
description: Informatie over het Facebook-verificatie voor uw toepassing App Services configureren.
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: c1b4c91d384c56c4f55bf8d31ced250f51c0d837
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Uw App Service-toepassing configureren voor het gebruik van Facebook-aanmelding
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Dit onderwerp leest u over het configureren van Azure App Service voor het gebruik van Facebook als een verificatieprovider.

U moet een Facebook-account met een geverifieerde e-mailadres en een mobiel telefoonnummer hebben voor de procedure in dit onderwerp. Om een nieuwe Facebook-account maken, gaat u naar [facebook.com].

## <a name="register"></a>Uw toepassing registreren met Facebook
1. Meld u aan bij de [Azure-portal], en navigeer naar uw toepassing. Kopieer uw **URL**. U gebruikt dit voor het configureren van uw Facebook-app.
2. In een ander browservenster en navigeer naar de [Facebook-ontwikkelaars] website en aanmelden met uw Facebook-accountreferenties.
3. (Optioneel) Als u nog niet hebt geregistreerd, klikt u op **Apps** > **registreren als een ontwikkelaar**, accepteert u het beleid en volg de stappen voor inschrijving.
4. Klik op **mijn Apps** > **toevoegen van een nieuwe App** > **Website** > **overslaan en App-ID maken**. 
5. In **weergavenaam**, typ een unieke naam voor uw app, type uw **Neem contact op met E-mail**, kies een **categorie** voor uw app, klik vervolgens op **App-ID maken**en voltooi de beveiligingscontrole. Hiermee gaat u naar het dashboard voor ontwikkelaars voor uw nieuwe Facebook-app.
6. Klik onder 'Facebook aanmelding,' op **aan de slag**. Toevoegen van uw toepassing **omleidings-URI** naar **geldig OAuth omleidings-URI's**, klikt u vervolgens op **wijzigingen opslaan**. 
   
   > [!NOTE]
   > De omleidings-URI de URL van uw toepassing toegevoegd aan het pad is */.auth/login/facebook/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Zorg ervoor dat u van het HTTPS-schema gebruikmaakt.
   > 
   > 
7. Klik in de linkernavigatiebalk **instellingen**. Op de **App geheim** veld, klikt u op **weergeven**, geeft u uw wachtwoord als aangevraagd, noteert u de waarden van **App-ID** en **App geheim** . Met deze later kunt u uw toepassing configureren in Azure.
   
   > [!IMPORTANT]
   > Het app-geheim is een belangrijke beveiligingsreferentie. Dit geheim met anderen delen of deze te distribueren vanuit een clienttoepassing niet.
   > 
   > 
8. De Facebook-account dat is gebruikt voor het registreren van de toepassing is een beheerder van de app. Alleen beheerders kunnen op dit moment aanmelden bij deze toepassing. Om te verifiëren andere accounts Facebook, klikt u op **App revisie** en schakel **maken < uw app-naam > openbare** algemene openbare toegang met Facebook-verificatie inschakelen.

## <a name="secrets"></a>Toevoegen Facebook-informatie voor uw toepassing
1. Terug in de [Azure-portal], gaat u naar uw toepassing. Klik op **instellingen** > **verificatie / autorisatie**, en zorg ervoor dat **verificatie van App Service** is **op**.
2. Klik op **Facebook**, plakken in de App-ID en App-geheim waarden die u eerder hebt verkregen, eventueel alle scopes die nodig is voor uw toepassing inschakelen en klik vervolgens op **OK**.
   
    ![][0]
   
    Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code.
3. (Optioneel) Instellen om toegang te beperken tot uw site tot alleen gebruikers die zijn geverifieerd door Facebook, **te ondernemen actie wanneer de aanvraag is niet geverifieerd** naar **Facebook**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Facebook voor verificatie.
4. Wanneer u klaar verificatie configureren, klikt u op **opslaan**.

U bent nu klaar voor gebruik van Facebook voor verificatie in uw app.

## <a name="related-content"></a>Verwante inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-ontwikkelaars]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portal]: https://portal.azure.com/
