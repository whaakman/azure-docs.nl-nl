---
title: Het configureren van Google-verificatie voor uw toepassing App Services
description: Informatie over het configureren van Google-verificatie voor uw App Services-toepassing.
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d6c1707f67d986487e5a45e76ffc9a02ddf16eb1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Het configureren van uw App Service-toepassing gebruik van Google aanmelding
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Dit onderwerp leest u over het configureren van Azure App Service voor het gebruik van Google als een verificatieprovider.

U moet een Google-account met een geverifieerde e-mailadres hebben voor de procedure in dit onderwerp. Als u een nieuw Google-account wilt maken, gaat u naar [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"></a>Uw toepassing registreren met Google
1. Meld u aan bij de [Azure-portal], en navigeer naar uw toepassing. Kopieer uw **URL**, die u later gebruiken voor het configureren van uw Google-app.
2. Navigeer naar de [Google API's](http://go.microsoft.com/fwlink/p/?LinkId=268303) website, meld u aan met de referenties van uw Google-account, klikt u op **Project maken**, bieden een **projectnaam**, klikt u vervolgens op **maken**.
3. Onder **sociale API's** klikt u op **Google + API** en vervolgens **inschakelen**.
4. In het linkernavigatievenster **referenties** > **OAuth toestemming scherm**, selecteer vervolgens uw **e-mailadres**, voer een **Productnaam**, en klik op **opslaan**.
5. In de **referenties** tabblad **referenties maken** > **OAuth-Clientidentiteit**, selecteer daarna **webtoepassing**.
6. Plak de App Service **URL** u eerder hebt gekopieerd in **geautoriseerd JavaScript oorsprongen**, plak uw omleiding URI in **geautoriseerd omleidings-URI**. De omleidings-URI is de URL van uw toepassing toegevoegd aan het pad */.auth/login/google/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/google/callback`. Zorg ervoor dat u van het HTTPS-schema gebruikmaakt. Klik vervolgens op **Maken**.
7. Maak een notitie van de waarden van de client-ID en clientgeheim op het volgende scherm.

    > [!IMPORTANT]
    > Het clientgeheim is een belangrijke beveiligingsreferentie. Dit geheim met anderen delen of deze te distribueren vanuit een clienttoepassing niet.


## <a name="secrets"></a>Informatie Google toevoegen aan uw toepassing
1. Terug in de [Azure-portal], gaat u naar uw toepassing. Klik op **instellingen**, en vervolgens **verificatie / autorisatie**.
2. Als de verificatie / autorisatie-functie niet is ingeschakeld, schakelt u de schakeloptie voor **op**.
3. Klik op **Google**. Plak in de App-ID en App-geheim waarden die u eerder hebt verkregen en optioneel in staat alle scopes die vereist zijn voor uw toepassing. Klik vervolgens op **OK**.
   
   ![][1]
   
   Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code.
4. (Optioneel) Instellen om toegang te beperken tot uw site tot alleen gebruikers die zijn geverifieerd door Google, **te ondernemen actie wanneer de aanvraag is niet geverifieerd** naar **Google**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Google voor verificatie.
5. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Google voor verificatie in uw app.

## <a name="related-content"></a>Verwante inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portal]: https://portal.azure.com/

