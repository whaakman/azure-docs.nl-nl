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
ms.date: 06/06/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: e91d55c29d325301b8ac70ddc63fb408961fbb2c
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742968"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Uw App Service-toepassing configureren voor het gebruik van Facebook-aanmelding
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Dit onderwerp ziet u hoe u Azure App Service configureren voor Facebook gebruiken als een verificatieprovider.

U moet een Facebook-account met een geverifieerde e-mailadres en een mobiel telefoonnummer hebben voor de procedure in dit onderwerp. Voor het maken van een nieuwe Facebook-account, gaat u naar [facebook.com].

## <a name="register"> </a>Uw toepassing registreren met Facebook
1. Navigeer naar de [Facebook-ontwikkelaars] website en aanmelden met uw Facebook-accountreferenties.
3. (Optioneel) Als u een Facebook voor ontwikkelaars-account hebt, klikt u op **aan de slag** en volg de registratiestappen.
4. Klik op **mijn Apps** > **nieuwe App toevoegen**.
5. In **weergavenaam**, typ een unieke naam voor uw app. Bieden ook uw **Neem contact op met e-mailadres**, en klik vervolgens op **App-ID maken** en voltooi de beveiligingscontrole. Hiermee gaat u naar het dashboard voor ontwikkelaars voor uw nieuwe Facebook-app.
6. Klik op **Dashboard** > **aanmelden via Facebook** > **instellen** > **Web**.
1. In de linkernavigatiebalk onder **aanmelden via Facebook**, klikt u op **instellingen**.
1. In **geldig OAuth omleidings-URI's**, type `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback` en vervang  *\<app-naam >* met de naam van uw app in Azure App Service. Klik op **Wijzigingen opslaan**.
8. Klik in de navigatiebalk links op **instellingen** > **Basic**. Op de **Appgeheim** veld, klikt u op **weergeven**. Kopieer de waarden van **App-ID** en **Appgeheim**. Met deze later kunt u uw App Service-app kunt configureren in Azure.
   
   > [!IMPORTANT]
   > De app-geheim is een belangrijke beveiligingsreferentie. Niet dit geheim met iedereen delen en distribueren binnen een clienttoepassing.
   > 
   > 
9. De Facebook-account dat is gebruikt voor het registreren van de toepassing is een beheerder van de app. Op dit moment kunnen alleen beheerders zich bij deze toepassing. Als u wilt andere Facebook-accounts worden geverifieerd, klikt u op **App-revisie** en in te schakelen **maken \<uw app-naam > openbare** aan het algemene publiek toegang met behulp van de Facebook-authenticatie inschakelen.

## <a name="secrets"> </a>Facebook-gegevens toevoegen aan uw toepassing
1. Aanmelden bij de [Azure Portal] en navigeer naar uw App Service-app. Klik op **instellingen** > **verificatie / autorisatie**, en zorg ervoor dat **App Service-verificatie** is **op**.
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
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure Portal]: https://portal.azure.com/
