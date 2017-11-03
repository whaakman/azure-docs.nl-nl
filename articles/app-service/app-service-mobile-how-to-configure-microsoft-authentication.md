---
title: Het configureren van Microsoft-Account-verificatie voor uw toepassing App Services
description: Informatie over het configureren van Microsoft-Account-verificatie voor uw App Services-toepassing.
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 67386b03ae4cc683fe00e11e8dad19d1442eff09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Het configureren van uw App Service-toepassing te gebruiken Microsoft-Account aanmelding
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Dit onderwerp leest u over het configureren van Azure App Service voor het gebruik van Microsoft-Account als een verificatieprovider. 

## <a name="register-microsoft-account"></a>Uw app registreren bij Microsoft-Account
1. Meld u aan bij de [Azure-portal], en navigeer naar uw toepassing. Kopieer uw **URL**, die u later gebruiken voor het configureren van uw app met Microsoft-Account.
2. Navigeer naar de [mijn toepassingen] pagina in het Microsoft-Account Developer Center en meld u aan met uw Microsoft-account, indien nodig.
3. Klik op **een app toevoegen**vervolgens typt u de naam van een toepassing en klik op **-toepassing maken**.
4. Noteer de **toepassings-ID**, zoals u deze later hebt. 
5. Klik onder 'Platforms,' op **toevoegen Platform** en selecteert u 'Web'.
6. Geef het eindpunt voor uw toepassing onder 'Omleidings-URI' en klik op **opslaan**. 
   
   > [!NOTE]
   > De omleidings-URI de URL van uw toepassing toegevoegd aan het pad is */.auth/login/microsoftaccount/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > Zorg ervoor dat u van het HTTPS-schema gebruikmaakt.
   
7. Klik onder 'Toepassing geheimen,' op **nieuw wachtwoord genereren**. Noteer de waarde die wordt weergegeven. Zodra u de pagina verlaat, wordt deze niet opnieuw worden weergegeven.

    > [!IMPORTANT]
    > Het wachtwoord is een belangrijke beveiligingsreferentie. Het wachtwoord met anderen delen of deze te distribueren vanuit een clienttoepassing niet.

## <a name="secrets"></a>Informatie van Microsoft-Account toevoegen aan uw App Service-toepassing
1. Terug in de [Azure-portal], gaat u naar uw toepassing, klik op **instellingen** > **verificatie / autorisatie**.
2. Als de verificatie / autorisatie-functie is niet ingeschakeld, schakel deze **op**.
3. Klik op **Microsoft-Account**. Plak in de toepassings-ID en wachtwoord waarden die u eerder hebt verkregen en optioneel in staat alle scopes die vereist zijn voor uw toepassing. Klik vervolgens op **OK**.
   
    ![][1]
   
    Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code.
4. (Optioneel) Instellen om toegang te beperken tot uw site tot alleen gebruikers die zijn geverifieerd door de Microsoft-account, **te ondernemen actie wanneer de aanvraag is niet geverifieerd** naar **Microsoft-Account**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar de Microsoft-account voor verificatie.
5. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Microsoft-Account voor de verificatie in uw app.

## <a name="related-content"></a>Verwante inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[mijn toepassingen]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portal]: https://portal.azure.com/
