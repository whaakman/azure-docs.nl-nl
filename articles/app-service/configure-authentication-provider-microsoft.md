---
title: Verificatie van micro soft-account configureren-Azure App Service
description: Meer informatie over het configureren van micro soft-account verificatie voor uw App Services-toepassing.
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
ms.custom: seodec18
ms.openlocfilehash: 10b661f0c4b7dc45284b907e83df3c0372f97cab
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561546"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Uw App Service-toepassing configureren voor het gebruik van aanmelding met een micro soft-account
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp wordt beschreven hoe u Azure App Service kunt configureren voor gebruik van micro soft-account als verificatie provider. 

## <a name="register-microsoft-account"> </a>Uw app registreren bij micro soft-account
1. Meld u aan bij de [Azure-portal]en navigeer naar uw toepassing. Kopieer uw **URL**, die u later gebruikt voor het configureren van uw app met micro soft-account.
2. Ga naar [**app-registraties**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)en meld u aan met uw Microsoft-account, indien gewenst.
3. Klik op **een app toevoegen**, typ een toepassings naam en klik op **maken**.
4. Noteer de **toepassings-id**, zoals u deze later nodig hebt. 
5. Klik onder ' platforms ' op **platform toevoegen** en selecteer ' Web '.
6. Geef het eind punt voor uw toepassing op onder ' omleidings-Uri's ' en klik vervolgens op **Opslaan**. 
   
   > [!NOTE]
   > De omleidings-URI is de URL van uw toepassing die is toegevoegd aan het pad */.auth/login/MicrosoftAccount/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > Zorg ervoor dat u het HTTPS-schema gebruikt.
   
7. Klik onder ' toepassings geheimen ' op **Nieuw wacht woord genereren**. Noteer de waarde die wordt weer gegeven. Zodra u de pagina verlaat, wordt deze niet meer weer gegeven.

    > [!IMPORTANT]
    > Het wacht woord is een belang rijke beveiligings referentie. Deel het wacht woord niet met iemand of distribueer het in een client toepassing.
    
8. Klik op **Opslaan**

## <a name="secrets"> </a>Micro soft-account gegevens toevoegen aan uw app service-toepassing
1. Ga terug naar uw toepassing in het [Azure-portal], klik op **instellingen** > **verificatie/autorisatie**.
2. Als de functie voor verificatie/autorisatie niet is ingeschakeld, schakelt u deze **in**.
3. Klik op **micro soft-account**. Plak de waarden van de toepassings-ID en het wacht woord die u eerder hebt verkregen en schakel desgewenst de scopes in die voor uw toepassing nodig zijn. Klik vervolgens op **OK**.
   
    ![][1]
   
    App Service biedt standaard verificatie, maar beperkt geen geautoriseerde toegang tot uw site-inhoud en Api's. U moet gebruikers in uw app-code autoriseren.
4. Beschrijving Om de toegang tot uw site te beperken tot alleen gebruikers die zijn geverifieerd door Microsoft-account, stelt **u een actie in die moet worden uitgevoerd wanneer de aanvraag niet is geverifieerd** voor een **micro soft-account**. Hiervoor moeten alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Microsoft-account voor authenticatie.
5. Klik op **Opslaan**.

U bent nu klaar om micro soft-account te gebruiken voor verificatie in uw app.

## <a name="related-content"> </a>Gerelateerde inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portal]: https://portal.azure.com/
