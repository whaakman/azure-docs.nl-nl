---
title: Verificatie via Facebook - Azure App Service configureren
description: Informatie over het configureren van Facebook-authenticatie voor uw App Services-toepassing.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: cc10c9be5bab3b84c8773d8a930473267db353ab
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256771"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Uw App Service-toepassing configureren voor het gebruik van Facebook-aanmelding
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Dit onderwerp ziet u hoe u Azure App Service configureren voor Facebook gebruiken als een verificatieprovider.

U moet een Facebook-account met een geverifieerde e-mailadres en een mobiel telefoonnummer hebben voor de procedure in dit onderwerp. Voor het maken van een nieuwe Facebook-account, gaat u naar [facebook.com].

## <a name="register"> </a>Uw toepassing registreren met Facebook
1. Meld u aan bij de [Azure Portal], en navigeer naar uw toepassing. Kopieer uw **URL**. U gebruikt deze om uw Facebook-app te configureren.
2. Navigeer in een ander browservenster naar de [Facebook-ontwikkelaars] website en aanmelden met uw Facebook-accountreferenties.
3. (Optioneel) Als u nog niet hebt geregistreerd, klikt u op **Apps** > **registreren als een ontwikkelaar**, accepteert u het beleid en volg de registratiestappen.
4. Klik op **mijn Apps** > **toevoegen van een nieuwe App**.
5. In **weergavenaam**, typ een unieke naam voor uw app. Bieden ook uw **Neem contact op met e-mailadres**, en klik vervolgens op **App-ID maken** en voltooi de beveiligingscontrole. Hiermee gaat u naar het dashboard voor ontwikkelaars voor uw nieuwe Facebook-app.
6. Onder **aanmelden via Facebook**, klikt u op **instellen**, en kies vervolgens **instellingen** in de linkernavigatiebalk onder **aanmelden via Facebook**.
7. Toevoegen van uw toepassing **omleidings-URI** naar **geldig OAuth omleidings-URI's**, klikt u vervolgens op **wijzigingen opslaan**.
   
   > [!NOTE]
   > Uw omleidings-URI de URL van uw toepassing met het pad, het achtervoegsel is */.auth/login/facebook/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Zorg ervoor dat u van het HTTPS-schema gebruikmaakt.
   > 
   > 
8. Klik in de navigatiebalk links op **instellingen** > **Basic**. Op de **Appgeheim** veld, klikt u op **weergeven**, Geef uw wachtwoord als aangevraagd, noteer de waarden van **App-ID** en **Appgeheim** . Met deze later kunt u uw toepassing configureren in Azure.
   
   > [!IMPORTANT]
   > De app-geheim is een belangrijke beveiligingsreferentie. Niet dit geheim met iedereen delen en distribueren binnen een clienttoepassing.
   > 
   > 
9. De Facebook-account dat is gebruikt voor het registreren van de toepassing is een beheerder van de app. Op dit moment kunnen alleen beheerders zich bij deze toepassing. Als u wilt andere Facebook-accounts worden geverifieerd, klikt u op **App-revisie** en in te schakelen **maken < your-app-name > openbare** aan het algemene publiek toegang met behulp van de Facebook-authenticatie inschakelen.

## <a name="secrets"> </a>Facebook-gegevens toevoegen aan uw toepassing
1. Klik in de [Azure Portal], gaat u naar uw toepassing. Klik op **instellingen** > **verificatie / autorisatie**, en zorg ervoor dat **App Service-verificatie** is **op**.
2. Klik op **Facebook**, plakken in de App-ID en App-geheim waarden die u eerder hebt verkregen, eventueel alle scopes die nodig zijn voor uw toepassing inschakelen en vervolgens klikt u op **OK**.
   
    ![][0]
   
    Standaard is App Service-verificatie biedt, maar biedt geautoriseerde toegang tot uw API's en site-inhoud niet beperken. U moet autoriseren van gebruikers in uw app-code.
3. (Optioneel) Instellen om toegang te beperken naar uw site tot alleen gebruikers die zijn geverifieerd door Facebook, **te ondernemen actie wanneer de aanvraag niet is geverifieerd** naar **Facebook**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Facebook voor verificatie.
4. Wanneer u klaar bent verificatie configureren, klikt u op **opslaan**.

U bent nu klaar voor gebruik van Facebook voor verificatie in uw app.

## <a name="related-content"> </a>Gerelateerde inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-ontwikkelaars]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure Portal]: https://portal.azure.com/
