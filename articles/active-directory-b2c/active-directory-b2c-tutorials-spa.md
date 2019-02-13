---
title: 'Zelfstudie: Verificatie inschakelen in een webtoepassing met één pagina - Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Azure Active Directory B2C voor het opgeven van gebruikersaanmeldingsreferenties voor een toepassing met één pagina (JavaScript).
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 813c7131ff0a56e843e728cd78fff969b1d90fcc
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756322"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c"></a>Zelfstudie: Verificatie inschakelen in een webtoepassing met één pagina met behulp van Azure Active Directory B2C

Deze zelfstudie laat u zien hoe u Azure Active Directory B2C (Azure AD) kunt gebruiken voor het aanmelden en registreren van gebruikers in een toepassing met één pagina. Met Azure AD B2C kunnen uw toepassingen zich met behulp van open-standaardprotocollen verifiëren bij sociale accounts, Enterprise-accounts en Azure Active Directory-accounts.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De toepassing bijwerken in Azure AD B2C
> * Het voorbeeld configureren voor gebruik van de toepassing
> * Aanmelden met behulp van de gebruikersstroom

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Gebruikersstromen maken](tutorial-create-user-flows.md) om gebruikerservaringen in uw toepassing in te schakelen. 
* U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload** installeren.
* Installeer de [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) of hoger
* [Node.js](https://nodejs.org/en/download/) installeren

## <a name="update-the-application"></a>De toepassing bijwerken

In de zelfstudie die u als onderdeel van de vereisten hebt voltooid, hebt u een webtoepassing in Azure AD B2C toegevoegd. Om te communiceren met het voorbeeld in deze zelfstudie, moet u een omleidings-URI toevoegen aan de toepassing in Azure AD B2C.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map gebruikt die uw Azure AD B2C-tenant bevat door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.
3. Kies **Alle services** linksboven in het Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **Toepassingen** en selecteer vervolgens de toepassing *webapp1*.
5. Voeg onder **antwoord-URL** `http://localhost:6420` toe.
6. Selecteer **Opslaan**.
7. Noteer op de eigenschappenpagina de toepassings-ID die u gebruikt wanneer u de web-app configureert.
8. Selecteer **Sleutels**, selecteer **Sleutel genereren** en selecteer **Opslaan**. Noteer de sleutel die u gaat gebruiken wanneer u de web-app configureert.

## <a name="configure-the-sample"></a>Configureren van het voorbeeld

In deze zelfstudie configureert u een voorbeeld dat u kunt downloaden uit GitHub. Het voorbeeld laat zien hoe een app met één pagina Azure AD B2C gebruikt voor gebruikersregistratie, aanmelding en om een beveiligde web-API aan te roepen.

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

De instellingen wijzigen:

1. Open het bestand `index.html` in het voorbeeld.
2. Configureer het voorbeeld met de toepassings-ID en -sleutel die u eerder hebt genoteerd. Wijzig de volgende regels met code door de waarden te vervangen door de namen van uw map en API's:

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID>',
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
    };
    ```

    De naam van de gebruikersstroom die in deze zelfstudie wordt gebruikt, is **B2C_1_signupsignin1**. Als u de gebruikersstroom een andere naam hebt gegeven, gebruikt u deze naam in de waarde voor `authority`.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

1. Start een opdrachtprompt voor Node.js.
2. Ga naar de map met het Node.js-voorbeeld. Bijvoorbeeld `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Voer de volgende opdrachten uit:

    ```
    npm install && npm update
    node server.js
    ```

    Het consolevenster geeft het poortnummer van waar de app wordt gehost.
    
    ```
    Listening on port 6420...
    ```

4. Gebruik een browser om te navigeren naar het adres `http://localhost:6420` om de app weer te geven.

Het voorbeeld biedt ondersteuning voor registratie, aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord. In deze zelfstudie leest u hoe gebruikers zich kunnen registreren met behulp van een e-mailadres.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

1. Klik op **Aanmelden** om u aan te melden als een gebruiker van de app. Hiervoor wordt de gebruikersstroom **B2C_1_signupsignin1** gebruikt die u in een vorige stap hebt gedefinieerd.
2. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**. 
3. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in de gebruikersstroom zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op. 

    ![Aanmeldingswerkstroom](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Klik op **Maken** om een lokaal account te maken in de Azure AD B2C-directory.

Gebruikers kunnen nu een e-mailadres gebruiken om zich aan te melden en de SPA-app te gebruiken.

> [!NOTE]
> Na aanmelding geeft de app de foutmelding 'onvoldoende machtigingen'. U krijgt deze foutmelding omdat u toegang probeert te krijgen tot een resource van de demo-directory. Omdat uw toegangstoken alleen geldig is voor uw Azure AD-directory, is de API-aanroep niet geautoriseerd. Ga door met de volgende zelfstudie voor het maken van een beveiligde web-API voor uw directory.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * De toepassing bijwerken in Azure AD B2C
> * Het voorbeeld configureren voor gebruik van de toepassing
> * Aanmelden met behulp van de gebruikersstroom

> [!div class="nextstepaction"]
> [Zelfstudie: Toegang verlenen aan een web-API van ASP.NET Core vanuit een app met één pagina met behulp van Azure Active Directory B2C](active-directory-b2c-tutorials-spa-webapi.md)
