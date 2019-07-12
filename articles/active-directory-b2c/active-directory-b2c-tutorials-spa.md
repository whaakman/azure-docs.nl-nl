---
title: Zelfstudie - verificatie inschakelen in een toepassing met één pagina - Azure Active Directory B2C
description: Informatie over het gebruik van Azure Active Directory B2C voor gebruikersaanmelding voor een toepassing met één pagina (JavaScript).
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 07/08/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 496cf801a44638af61306b43791abce9466e2cb2
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835675"
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

Voordat u doorgaat met de stappen in deze zelfstudie hebt u de volgende Azure AD B2C-resources in locatie nodig:

* [Azure AD B2C-tenant](tutorial-create-tenant.md)
* [Toepassing geregistreerd](tutorial-register-applications.md) in uw tenant
* [Gebruikersstromen gemaakt](tutorial-create-user-flows.md) in uw tenant

Daarnaast moet u het volgende in uw lokale ontwikkelomgeving:

* Code-editor, bijvoorbeeld [Visual Studio Code](https://code.visualstudio.com/) of [2019 van Visual Studio](https://www.visualstudio.com/downloads/)
* [.NET core SDK 2.0.0](https://www.microsoft.com/net/core) of hoger
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>De toepassing bijwerken

In de tweede zelfstudie die u als onderdeel van de vereisten hebt voltooid, moet u een webtoepassing in Azure AD B2C geregistreerd. Om te communiceren met het voorbeeld in deze zelfstudie, moet u een omleidings-URI toevoegen aan de toepassing in Azure AD B2C.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zorg ervoor dat u de map gebruikt met uw Azure AD B2C-tenant door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **Toepassingen** en selecteer vervolgens de toepassing *webapp1*.
1. Voeg onder **Antwoord-URL** `http://localhost:6420` toe.
1. Selecteer **Opslaan**.
1. Noteer op de eigenschappenpagina van de **toepassings-ID**. U kunt de app-ID in een latere stap gebruiken wanneer u de code in de webtoepassing van één pagina bijwerken.

## <a name="get-the-sample-code"></a>De voorbeeldcode halen

In deze zelfstudie configureert u een codevoorbeeld dat u vanuit GitHub downloaden. Het voorbeeld laat zien hoe een toepassing met één pagina Azure AD B2C kunt gebruiken voor de gebruiker zich aanmelden en meld u in, en op een beveiligde web-API-aanroep.

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Bijwerken van het voorbeeld

Nu dat u het voorbeeld hebt aangeschaft, kunt u de code bijwerken met de naam van uw Azure AD B2C-tenant en de toepassings-ID die u in een eerdere stap hebt genoteerd.

1. Open de `index.html` bestand in de hoofdmap van de voorbeeld-map.
1. In de `msalConfig` definitie, wijzigen de **clientId** waarde met de toepassings-ID die u in een eerdere stap hebt genoteerd. Werk vervolgens de **instantie** URI-waarde met de naam van uw Azure AD B2C-tenant. De URI ook bijwerken met de naam van de aanmelden-up-to-date/aanmelden gebruikersstroom die u hebt gemaakt in een van de vereisten (bijvoorbeeld *B2C_1_signupsignin1*).

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", //This is your client ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    De naam van de gebruikersstroom die in deze zelfstudie wordt gebruikt, is **B2C_1_signupsignin1**. Als u een andere gebruikersnaam voor de stroom gebruikt, geeft u die naam bestaat in de `authority` waarde.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

1. Open een consolevenster en wijzig in de map met het voorbeeld. Bijvoorbeeld:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Voer de volgende opdrachten uit:

    ```
    npm install && npm update
    node server.js
    ```

    Het consolevenster geeft het poortnummer van de Node.js-server lokaal wordt uitgevoerd:

    ```
    Listening on port 6420...
    ```

1. Navigeer naar `http://localhost:6420` in uw browser om de toepassing weer te geven.

Het voorbeeld biedt ondersteuning voor registratie, aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord. In deze zelfstudie leest u hoe gebruikers zich kunnen registreren met behulp van een e-mailadres.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

1. Klik op **Aanmelden** om u aan te melden als een gebruiker van de app. Dit maakt gebruik van de **B2C_1_signupsignin1** gebruikersstroom die u hebt opgegeven in de vorige stap.
1. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**.
1. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in de gebruikersstroom zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op.

    ![Pagina voor het registreren gepresenteerd door de gebruikersstroom sign-in/aanmelden-van](./media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.PNG)

1. Klik op **Maken** om een lokaal account te maken in de Azure AD B2C-directory.

Wanneer u klikt op **maken**, de aanmeldpagina wordt gesloten en de aanmeldingspagina wordt opnieuw weergegeven.

U kunt nu uw e-mailadres en wachtwoord gebruiken om aan te melden bij de toepassing.

### <a name="error-insufficient-permissions"></a>Fout: onvoldoende machtigingen

Nadat u zich aanmeldt, een fout onvoldoende machtigingen door de app wordt weergegeven: dit is **verwacht**:

`ServerError: AADB2C90205: This application does not have sufficient permissions against this web resource to perform the operation.`

U ontvangt deze foutmelding omdat u probeert te krijgen tot een bron van de demo-map, maar uw toegangstoken alleen geldig voor uw Azure AD-directory is. De API-aanroep is daarom niet gemachtigd.

Ga door met de volgende zelfstudie in de reeks (Zie [Vervolgstappen](#next-steps)) te maken van een beveiligde web-API voor uw directory.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * De toepassing bijwerken in Azure AD B2C
> * Het voorbeeld configureren voor gebruik van de toepassing
> * Aanmelden met behulp van de gebruikersstroom

Nu verder met de volgende zelfstudie in de reeks om toegang te verlenen tot een beveiligde web-API van de beveiligd-WACHTWOORDVERIFICATIE:

> [!div class="nextstepaction"]
> [Zelfstudie: Toegang verlenen aan een web-API van ASP.NET Core vanuit een app met één pagina met behulp van Azure Active Directory B2C](active-directory-b2c-tutorials-spa-webapi.md)
