---
title: Het configureren van Azure Active Directory-verificatie voor uw toepassing App Services
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
ms.openlocfilehash: 25f0578a9e273c30ecc98af5b66c6dd43305aa03
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Uw App Service-toepassing configureren voor het gebruik van Azure Active Directory-aanmelding
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Dit onderwerp leest u over het configureren van Azure App Services voor het gebruik van Azure Active Directory als verificatieprovider.

## <a name="express"></a>Azure Active Directory configureren met behulp van snelle instellingen
1. In de [Azure-portal], gaat u naar uw toepassing. Klik op **instellingen**, en vervolgens **verificatie/autorisatie**.
2. Als de verificatie / autorisatie-functie niet is ingeschakeld, schakelt u de schakeloptie voor **op**.
3. Klik op **Azure Active Directory**, en klik vervolgens op **Express** onder **beheermodus**.
4. Klik op **OK** registreren van de toepassing in Azure Active Directory. Hiermee maakt u een nieuwe registratie. Als u een bestaande registratie in plaats daarvan kiezen wilt, klikt u op **Selecteer een bestaande app** en zoek vervolgens naar de naam van de registratie van een eerder gemaakte binnen uw tenant.
   Klik op de registratie te selecteren en klik op **OK**. Klik vervolgens op **OK** op de blade voor Azure Active Directory-instellingen.
   Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code.
5. (Optioneel) Instellen om toegang te beperken tot uw site tot alleen gebruikers die zijn geverifieerd door Azure Active Directory, **te ondernemen actie wanneer de aanvraag is niet geverifieerd** naar **aanmelden met Azure Active Directory**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.
6. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Azure Active Directory voor verificatie in uw app.

## <a name="advanced"></a>(Alternatieve methode) handmatig configureren van Azure Active Directory met geavanceerde instellingen
U kunt er ook voor kiezen om configuratie-instellingen handmatig. Dit is de beste oplossing als de AAD-tenant die u wilt gebruiken, wijkt af van de tenant waarmee u zich bij Azure aanmelden. Voor het voltooien van de configuratie, moet u eerst een registratie in Azure Active Directory maken en vervolgens u enkele van de registratiedetails van de in App Service moet opgeven.

### <a name="register"></a>Uw toepassing registreren met Azure Active Directory
1. Meld u aan bij de [Azure-portal], en navigeer naar uw toepassing. Kopieer uw toepassing **URL**. U gebruikt dit voor het configureren van uw app in Azure Active Directory.
2. Navigeer naar **Active Directory**, selecteer vervolgens de **App registraties**, klikt u vervolgens op **registratie van de nieuwe toepassing** boven de registratie van een nieuwe app te starten. 
3. Voer in het dialoogvenster Create toepassing registratie een **naam** voor uw toepassing, selecteert u de **Web App API** typt, in de **aanmeldings-URL** vak plak de URL van de toepassing (van stap 1). Klik vervolgens op naar **maken**.
4. In enkele seconden, ziet u de nieuwe toepassing registreren die u zojuist hebt gemaakt weergegeven.
5. Wanneer de toepassing is toegevoegd, klikt u op de naam van de registratie van toepassing, klikt u op **instellingen** aan de bovenkant en klik vervolgens op **eigenschappen** 
6. In de **App ID URI** vak, plak de URL van de toepassing (uit stap 1), ook in de **startpagina URL** plakken in de URL van de toepassing (uit stap 1), en klik vervolgens op **opslaan**
7. Klik nu op de **antwoord-URL's**, bewerk de **antwoord-URL**, plakt u in de URL van de toepassing (uit stap 1), wijzigt u het protocol om ervoor te zorgen dat u hebt **https://** http:// (protocol niet), vervolgens worden toegevoegd aan het einde van de URL */.auth/login/aad/callback* . (Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/aad/callback`.) Klik op **Opslaan**.   
8.  Op dit punt kopieert de **toepassings-ID** voor de app. Houd dit voor later gebruik. U moet dit voor het configureren van uw webtoepassing.
9. Sluit de blade van de details van registratie van toepassing. U moet terugkeren naar de samenvatting van Azure Active Directory-App-registratie, klik op de **eindpunten** knop aan de bovenkant en kopieer de **Document met federatieve metagegevens** URL. 
10. Open een nieuw browservenster en navigeer naar de URL door plakken en naar de XML-pagina bladeren. Op de bovenkant van het document is een **EntityDescriptor** element, moet er een **id van de entiteit** kenmerk van het formulier `https://sts.windows.net/` gevolgd door een specifieke GUID voor uw tenant (een 'tenant-ID' genoemd). Deze waarde voor kopiëren - fungeren als uw **URL-verlener**. Configureert u uw toepassing voor dit later gebruik.

### <a name="secrets"></a>Toevoegen Azure Active Directory-informatie voor uw toepassing
1. Terug in de [Azure-portal], gaat u naar uw toepassing. Klik op **verificatie/autorisatie**. Als de verificatie/autorisatie-functie niet is ingeschakeld, schakelt u de schakeloptie voor **op**. Klik op **Azure Active Directory**, onder verificatieproviders om uw toepassing te configureren. (Optioneel) Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code. Selecteer de **te ondernemen actie wanneer de aanvraag is niet geverifieerd**, stel dit in op **aanmelden met Azure Active Directory**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.
2. in de configuratie van Active Directory-verificatie, klikt u op **Geavanceerd** onder **beheermodus**. Plak de toepassings-ID in het Client-ID (uit stap 8) en plak de URL-verlener-waarde in de id van de entiteit (uit stap 10). Klik vervolgens op **OK**.
3. Klik op de blade van de configuratie van Active Directory-verificatie op **opslaan**.

U bent nu klaar voor gebruik van Azure Active Directory voor verificatie in uw app.

## <a name="optional-configure-a-native-client-application"></a>(Optioneel) Een systeemeigen clienttoepassing configureren
Azure Active Directory kunt u ook systeemeigen clients registreren die biedt meer controle over de machtigingen toewijzen. U moet dit als u wilt uitvoeren aanmeldingen met een bibliotheek, zoals de **Active Directory Authentication Library**.

1. Navigeer naar **Active Directory** in de [klassieke Azure-portal].
2. Selecteer uw directory en selecteer vervolgens de **toepassingen** boven op het tabblad. Klik op **toevoegen** onder een nieuwe app-registratie maken.
3. Klik op **mijn organisatie ontwikkelt toepassing toevoegen**.
4. Voer in de Wizard toepassing toevoegen, een **naam** voor uw toepassing en klik op de **systeemeigen clienttoepassing** type. Klik vervolgens op om door te gaan.
5. In de **omleidings-URI** Voer van uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet er ongeveer als *https://contoso.azurewebsites.net/.auth/login/done*. Als een Windows-toepassing maakt in plaats daarvan gebruikt de [pakket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als de URI.
6. Wanneer de systeemeigen toepassing is toegevoegd, klikt u op de **configureren** tabblad. Zoek de **Client-ID** en noteer deze waarde.
7. Blader omlaag naar de **machtigingen voor andere toepassingen** sectie en klik op **toepassing toevoegen**.
8. Zoek de webtoepassing die u eerder hebt geregistreerd en klik op het plusteken. Klik vervolgens op het selectievakje om het dialoogvenster te sluiten. Als de webtoepassing niet kan worden gevonden, gaat u naar de registratie en een nieuwe antwoord-URL (bijv, de HTTP-versie van de huidige URL) toevoegen, klikt u op Opslaan en Herhaal deze stappen: de toepassing weergegeven in de lijst.
9. Open op het nieuwe item dat u zojuist hebt toegevoegd, de **gedelegeerde machtigingen** vervolgkeuzelijst en selecteer **toegang (appName)**. Klik vervolgens op **Opslaan**.

U hebt nu een native client-toepassing die uw App Service-toepassing kunt openen geconfigureerd.

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
[klassieke Azure-portal]: https://manage.windowsazure.com/
[alternative method]:#advanced
