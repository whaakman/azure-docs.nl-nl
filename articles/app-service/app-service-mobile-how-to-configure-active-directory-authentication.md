---
title: Azure Active Directory-verificatie voor uw toepassing App Services configureren
description: Informatie over het configureren van Azure Active Directory-verificatie voor uw App Services-toepassing.
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 990fab9aeea71b8cf344b9a49a5ed438db6663c0
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-login"></a>Configureer uw App Service-app voor het gebruik van Azure Active Directory-aanmelding
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit artikel leest u hoe Azure App Services voor het gebruik van Azure Active Directory als verificatieprovider te configureren.

## <a name="express"></a>Azure Active Directory configureren met behulp van snelle instellingen
1. In de [Azure-portal], gaat u naar uw App Service-app. Selecteer in het linkernavigatievenster **verificatie / autorisatie**.
2. Als **verificatie / autorisatie** niet is ingeschakeld, selecteert **op**.
3. Selecteer **Azure Active Directory**, en selecteer vervolgens **Express** onder **beheermodus**.
4. Selecteer **OK** registreren van de App Service-app in Azure Active Directory. Hiermee maakt u een nieuwe app-registratie. Als u een bestaande app-registratie in plaats daarvan kiezen wilt, klikt u op **Selecteer een bestaande app** en zoek vervolgens naar de naam van de registratie van een eerder gemaakte app binnen uw tenant.
   Klik op de registratie van de app en klik op **OK**. Klik vervolgens op **OK** op de pagina Azure Active Directory-instellingen.
   Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code.
5. (Optioneel) Instellen om toegang te beperken tot uw site tot alleen gebruikers die zijn geverifieerd door Azure Active Directory, **te ondernemen actie wanneer de aanvraag is niet geverifieerd** naar **aanmelden met Azure Active Directory**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.
6. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Azure Active Directory voor verificatie in uw App Service-app.

## <a name="advanced"></a>(Alternatieve methode) handmatig configureren van Azure Active Directory met geavanceerde instellingen
U kunt er ook voor kiezen om configuratie-instellingen handmatig. Dit is de beste oplossing als de AAD-tenant die u wilt gebruiken, wijkt af van de tenant waarmee u zich bij Azure aanmelden. Voor het voltooien van de configuratie, moet u eerst een registratie in Azure Active Directory maken en vervolgens u enkele van de registratiedetails van de in App Service moet opgeven.

### <a name="register"></a>Uw App Service-app registreren bij Azure Active Directory
1. Meld u aan bij de [Azure-portal], en navigeer naar uw App Service-app. Kopieer uw app **URL**. U gebruikt dit voor het configureren van de registratie van uw Azure Active Directory-app.
2. Navigeer naar **Active Directory**, selecteer vervolgens de **App registraties**, klikt u vervolgens op **registratie van de nieuwe toepassing** boven de registratie van een nieuwe app te starten. 
3. In de **maken** pagina, voert u een **naam** voor de registratie van uw app, selecteer de **Web-App / API** typt, in de **aanmeldings-URL** plakken vak de de URL van de toepassing (uit stap 1). Klik vervolgens op naar **maken**.
4. In enkele seconden ziet u de nieuwe app-inschrijving die u zojuist hebt gemaakt.
5. Wanneer de app-registratie is toegevoegd, klikt u op de naam van de app-registratie, klikt u op **instellingen** aan de bovenkant en klik vervolgens op **eigenschappen** 
6. In de **App ID URI** vak, plak de URL van de toepassing (uit stap 1), ook in de **startpagina URL** plakken in de URL van de toepassing (uit stap 1), en klik vervolgens op **opslaan**
7. Klik nu op de **antwoord-URL's**, bewerk de **antwoord-URL**, plakt u in de URL van de toepassing (uit stap 1), wijzigt u het protocol om ervoor te zorgen dat u hebt **https://** http:// (protocol niet), vervolgens worden toegevoegd aan het einde van de URL */.auth/login/aad/callback* (bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/aad/callback`). Klik op **Opslaan**.   
8.  Op dit punt kopieert de **toepassings-ID** voor de app. Houd het voor later gebruik. U moet het configureren van uw App Service-app.
9. Sluit de **geregistreerde app** pagina. Op de **App registraties** pagina, klikt u op de **eindpunten** knop aan de bovenkant en kopieer de **Document met federatieve metagegevens** URL. 
10. Open een nieuw browservenster en navigeer naar de URL door plakken en naar de XML-pagina bladeren. Aan de bovenkant van het document is een **EntityDescriptor** element, moet er een **id van de entiteit** kenmerk van het formulier `https://sts.windows.net/` gevolgd door een specifieke GUID voor uw tenant (een 'tenant-ID' genoemd). Deze waarde voor kopiëren - het fungeert als uw **URL-verlener**. Configureert u uw toepassing voor later gebruik.

### <a name="secrets"></a>Informatie van de Azure Active Directory toevoegen aan uw App Service-app
1. Terug in de [Azure-portal], gaat u naar uw App Service-app. Klik op **verificatie/autorisatie**. Als de verificatie/autorisatie-functie niet is ingeschakeld, schakelt u de schakeloptie voor **op**. Klik op **Azure Active Directory**, onder verificatieproviders voor het configureren van uw app. (Optioneel) Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code. Stel **te ondernemen actie wanneer de aanvraag is niet geverifieerd** naar **aanmelden met Azure Active Directory**. Deze optie vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.
2. in de configuratie van Active Directory-verificatie, klikt u op **Geavanceerd** onder **beheermodus**. Plak de toepassings-ID in het Client-ID (uit stap 8) en plak de URL-verlener-waarde in de id van de entiteit (uit stap 10). Klik vervolgens op **OK**.
3. Klik op de configuratiepagina van Active Directory-verificatie op **opslaan**.

U bent nu klaar voor gebruik van Azure Active Directory voor verificatie in uw App Service-app.

## <a name="optional-configure-a-native-client-application"></a>(Optioneel) Een systeemeigen clienttoepassing configureren
Azure Active Directory kunt u ook systeemeigen clients registreren die biedt meer controle over de machtigingen toewijzen. U moet dit als u wilt uitvoeren aanmeldingen met een bibliotheek, zoals de **Active Directory Authentication Library**.

1. Navigeer naar **Azure Active Directory** in de [Azure-portal].
2. Selecteer in het linkernavigatievenster **App registraties**. Klik op **nieuwe app-registratie** aan de bovenkant.
4. In de **maken** pagina, voert u een **naam** voor de registratie van uw app. Selecteer **systeemeigen** in **toepassingstype**.
5. In de **omleidings-URI** Voer van uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet er ongeveer als *https://contoso.azurewebsites.net/.auth/login/done*. Als een Windows-toepassing maakt in plaats daarvan gebruikt de [pakket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als de URI.
5. Klik op **Create**.
6. Wanneer de app-registratie is toegevoegd, selecteert u deze te openen. Zoek de **toepassings-ID** en noteer deze waarde.
7. Klik op **alle instellingen** > **vereist machtigingen** > **toevoegen** > **selecteert u een API**.
8. Typ de naam van de App Service-app die u eerder hebt geregistreerd om te zoeken en vervolgens te selecteren en op **Selecteer**. 
9. Selecteer **toegang \<app_naam >**. Klik vervolgens op **Selecteer**. Klik vervolgens op **Gereed**.

U hebt nu een systeemeigen clienttoepassing die toegang heeft tot uw App Service-app geconfigureerd.

## <a name="related-content"></a>Verwante inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[alternative method]:#advanced
