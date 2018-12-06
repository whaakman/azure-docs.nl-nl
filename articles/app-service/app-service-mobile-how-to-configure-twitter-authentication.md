---
title: Twitter-verificatie voor uw App Services-toepassing configureren
description: Informatie over het configureren van Twitter-verificatie voor uw App Services-toepassing.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.openlocfilehash: 42d857fe712658ca3c17d824d061742cf7195e39
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52967348"
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Uw App Service-toepassing voor het gebruik Twitter-aanmelding configureren
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe het configureren van Azure App Service voor het gebruik van Twitter als een verificatieprovider.

U moet een Twitter-account met een geverifieerde e-mailadres en telefoonnummer getal hebben voor de procedure in dit onderwerp. Voor het maken van een nieuwe Twitter-account, gaat u naar <a href="https://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Uw toepassing registreren bij Twitter
1. Meld u aan bij de [Azure Portal], en navigeer naar uw toepassing. Kopieer uw **URL**. U gebruikt deze uw Twitter-app configureren.
2. Navigeer naar de [Twitter-ontwikkelaars] website, meld u aan met de referenties van uw Twitter-account en klikt u op **Create New App**.
3. Typ in het **naam** en een **beschrijving** voor uw nieuwe app. Plakken in van uw toepassing **URL** voor de **Website** waarde. Vervolgens voor de **URL voor terugbellen**, plak de **URL voor terugbellen** u eerder hebt gekopieerd. Dit is de gateway van uw mobiele App toegevoegd aan het pad */.auth/login/twitter/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Zorg ervoor dat u van het HTTPS-schema gebruikmaakt.
4. Aan de onderkant van de pagina, lees en accepteer de voorwaarden. Klik vervolgens op **uw Twitter-toepassing maken**. Dit wordt weergegeven als de app de details van de toepassing geregistreerd.
5. Klik op de **instellingen** tabblad controle **toestaan dat deze toepassing worden gebruikt voor het aanmelden met Twitter**, klikt u vervolgens op **Update-instellingen**.
6. Selecteer de **Keys and Access Tokens** tabblad. Noteer de waarden van **Consumer Key (API-sleutel)** en **Consumer secret (API-geheim)**.
   
   > [!NOTE]
   > De consumentgeheim is een belangrijke beveiligingsreferentie. Dit geheim met iedereen delen en distribueren met uw app niet.
   > 
   > 

## <a name="secrets"> </a>Twitter-gegevens toevoegen aan uw toepassing
1. Klik in de [Azure Portal], gaat u naar uw toepassing. Klik op **instellingen**, en vervolgens **verificatie / autorisatie**.
2. Als de verificatie / autorisatie-functie niet is ingeschakeld, schakelt u de switch op **op**.
3. Klik op **Twitter**. Plak in het App-ID en App-geheim waarden die u eerder hebt verkregen. Klik vervolgens op **OK**.
   
   ![][1]
   
   Standaard is App Service-verificatie biedt, maar biedt geautoriseerde toegang tot uw API's en site-inhoud niet beperken. U moet autoriseren van gebruikers in uw app-code.
4. (Optioneel) Instellen om toegang te beperken naar uw site tot alleen gebruikers die zijn geverifieerd door Twitter, **te ondernemen actie wanneer de aanvraag niet is geverifieerd** naar **Twitter**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Twitter voor verificatie.
5. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Twitter voor verificatie in uw app.

## <a name="related-content"> </a>Gerelateerde inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter-ontwikkelaars]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure Portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
