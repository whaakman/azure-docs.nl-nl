---
title: Verificatie van de Microsoft-Account voor uw App Services-toepassing configureren
description: Informatie over het configureren van verificatie van de Microsoft-Account voor uw App Services-toepassing.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.openlocfilehash: abf09444e92c6faded42a9143b4b5c849a4cf41d
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48853263"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Uw App Service-toepassing voor het gebruik aanmelding voor Microsoft-Account configureren
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe het configureren van Azure App Service voor het gebruik van Microsoft-Account als een verificatieprovider. 

## <a name="register-microsoft-account"> </a>Uw app registreren bij Microsoft-Account
1. Meld u aan bij de [Azure Portal], en navigeer naar uw toepassing. Kopieer uw **URL**, waarmee u later uw app configureren met Microsoft-Account.
2. Navigeer naar de [mijn toepassingen] pagina in het Microsoft-Account Developer Center en meld u aan met uw Microsoft-account, indien nodig.
3. Klik op **een app toevoegen**, klikt u vervolgens typt u een toepassingsnaam in en klikt u op **maken**.
4. Noteer de **toepassings-ID**, zoals u deze later nodig. 
5. Klik onder 'Platforms' **Platform toevoegen** en selecteer 'Web'.
6. Geef het eindpunt voor uw toepassing onder 'Omleidings-URI' en klik op **opslaan**. 
   
   > [!NOTE]
   > Uw omleidings-URI de URL van uw toepassing met het pad, het achtervoegsel is */.auth/login/microsoftaccount/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > Zorg ervoor dat u van het HTTPS-schema gebruikmaakt.
   
7. Klik onder "Toepassingsgeheimen," **nieuw wachtwoord genereren**. Noteer de waarde die wordt weergegeven. Nadat u de pagina verlaat, wordt deze niet opnieuw worden weergegeven.

    > [!IMPORTANT]
    > Het wachtwoord is een belangrijke beveiligingsreferentie. Het wachtwoord met iedereen delen en distribueren binnen een clienttoepassing niet.
    
8. Klik op **Opslaan**.

## <a name="secrets"> </a>Informatie voor Microsoft-Account toevoegen aan uw App Service-toepassing
1. Klik in de [Azure Portal], gaat u naar uw toepassing, klikt u op **instellingen** > **verificatie / autorisatie**.
2. Als de verificatie / autorisatie-functie niet is ingeschakeld, schakelt u het **op**.
3. Klik op **Microsoft-Account**. Plak in de waarden van de toepassings-ID en het wachtwoord die u eerder hebt verkregen, en schakel eventueel alle scopes die uw toepassing vereist. Klik vervolgens op **OK**.
   
    ![][1]
   
    Standaard is App Service-verificatie biedt, maar biedt geautoriseerde toegang tot uw API's en site-inhoud niet beperken. U moet autoriseren van gebruikers in uw app-code.
4. (Optioneel) Om toegang te beperken naar uw site tot alleen gebruikers die zijn geverifieerd door Microsoft-account, ingesteld **te ondernemen actie wanneer de aanvraag niet is geverifieerd** naar **Microsoft-Account**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar de Microsoft-account voor de verificatie.
5. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Microsoft-Account voor de verificatie in uw app.

## <a name="related-content"> </a>Gerelateerde inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Mijn toepassingen]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure Portal]: https://portal.azure.com/
