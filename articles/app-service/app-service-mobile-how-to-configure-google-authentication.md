---
title: Google-verificatie voor uw App Services-toepassing configureren
description: Informatie over het configureren van Google-verificatie voor uw App Services-toepassing.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.openlocfilehash: 6af0dfb00ff791b8a73ebf1ad14b35d99f67a483
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227413"
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Uw App Service-toepassing voor het gebruik van Google-aanmelding configureren
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe het configureren van Azure App Service voor het gebruik van Google als een verificatieprovider.

Voor de procedure in dit onderwerp, moet u een Google-account met een geverifieerde e-mailadres hebben. Als u een nieuw Google-account wilt maken, gaat u naar [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Uw toepassing registreren bij Google
1. Meld u aan bij de [Azure Portal], en navigeer naar uw toepassing. Kopieer uw **URL**, die u later gebruiken om uw Google-app te configureren.
2. Navigeer naar de [Google API's](https://go.microsoft.com/fwlink/p/?LinkId=268303) website, meld u aan met de referenties van uw Google-account, klikt u op **Project maken**, bieden een **projectnaam**, klikt u vervolgens op  **Maak**.
3. Zodra het project is gemaakt, selecteert u deze. Klik in het projectdashboard op **gaat u naar de API's-overzicht**.
4. Selecteer **inschakelen API's en services**. Zoeken naar **API voor Google +**, en selecteer deze. Klik vervolgens op **inschakelen**.
5. In het linkernavigatievenster **referenties** > **OAuth instemmingsscherm**en selecteer vervolgens uw **e-mailadres**, voer een **Productnaam**, en klikt u op **opslaan**.
6. In de **referenties** tabblad **referenties maken** > **OAuth-Clientidentiteit**.
7. Selecteer op het scherm 'Client-ID maken' **webtoepassing**.
8. Plak de App Service **URL** u eerder hebt gekopieerd in **JavaScript oorsprongen toegestaan**, plak uw omleidings-URI in **omleidings-URI mag**. De omleidings-URI is de URL van uw toepassing met het pad, het achtervoegsel */.auth/login/google/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/google/callback`. Zorg ervoor dat u van het HTTPS-schema gebruikmaakt. Klik vervolgens op **Maken**.
9. Maak een notitie van de waarden van de client-ID en clientgeheim in het volgende scherm.

    > [!IMPORTANT]
    > Het clientgeheim is een belangrijke beveiligingsreferentie. Niet dit geheim met iedereen delen en distribueren binnen een clienttoepassing.


## <a name="secrets"> </a>Google-informatie toevoegen aan uw toepassing
1. Klik in de [Azure Portal], gaat u naar uw toepassing. Klik op **instellingen**, en vervolgens **verificatie / autorisatie**.
2. Als de verificatie / autorisatie-functie niet is ingeschakeld, schakelt u de switch op **op**.
3. Klik op **Google**. Plak in de App-ID en App-geheim waarden die u eerder hebt verkregen, en schakel eventueel alle scopes die uw toepassing vereist. Klik vervolgens op **OK**.
   
   ![][1]
   
   Standaard is App Service-verificatie biedt, maar biedt geautoriseerde toegang tot uw API's en site-inhoud niet beperken. U moet autoriseren van gebruikers in uw app-code.
4. (Optioneel) Instellen om toegang te beperken naar uw site tot alleen gebruikers die zijn geverifieerd door Google, **te ondernemen actie wanneer de aanvraag niet is geverifieerd** naar **Google**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Google voor verificatie.
5. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Google voor verificatie in uw app.

## <a name="related-content"> </a>Gerelateerde inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure Portal]: https://portal.azure.com/

