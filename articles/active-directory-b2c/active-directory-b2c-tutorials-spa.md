---
title: 'Zelfstudie: verificatie van apps met één pagina inschakelen met behulp van Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Azure Active Directory B2C voor het opgeven van gebruikersaanmeldingsreferenties voor een toepassing met één pagina (JavaScript).
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 0632d97c85e4baf837329a2b573f4abe5c15bf26
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195701"
---
# <a name="tutorial-enable-single-page-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Zelfstudie: Verificatie van apps met één pagina inschakelen met behulp van Azure Active Directory B2C

Deze zelfstudie laat u zien hoe u Azure Active Directory B2C (Azure AD) kunt gebruiken voor het aanmelden en registreren van gebruikers in een toepassing met één pagina (SPA). Met Azure AD B2C zijn uw apps in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociale accounts, Enterprise-accounts en Azure Active Directory-accounts.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een voorbeeldtoepassing met één pagina registreren in uw Azure AD B2C-directory.
> * Gebruikersstromen maken voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord.
> * De voorbeeld-app configureren voor gebruik van uw Azure AD B2C-directory.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Maak uw eigen [Azure AD B2C-directory](active-directory-b2c-get-started.md)
* U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload** installeren.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) of hoger
* [Node.js](https://nodejs.org/en/download/) installeren

## <a name="register-single-page-app"></a>Registreer de app met één pagina

Toepassingen moeten worden [geregistreerd](../active-directory/develop/developer-glossary.md#application-registration) in uw directory voordat ze [toegangstokens](../active-directory/develop/developer-glossary.md#access-token) van Azure Active Directory kunnen ontvangen. Als een app wordt geregistreerd, wordt er een [toepassings-id](../active-directory/develop/developer-glossary.md#application-id-client-id) voor de app in uw directory gemaakt. 

Meld u als globale beheerder van de Azure AD B2C-directory aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecteer **Azure AD B2C** in de lijst met services in Azure Portal. 

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **Toevoegen**. 

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-app in uw directory:
    
    ![Een nieuw app toevoegen](media/active-directory-b2c-tutorials-spa/spa-registration.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Mijn voorbeeld-app met één pagina | Voer een **naam** in die uw app voor consumenten beschrijft. | 
    | **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een app met één pagina. |
    | **Impliciete stroom toestaan** | Ja | Selecteer **Ja** omdat de app gebruikmaakt van [aanmelding via OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Antwoord-URL** | `http://localhost:6420` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die zijn aangevraagd voor de app. In deze zelfstudie wordt het voorbeeld lokaal (localhost) uitgevoerd en luistert dit op poort 6420. |
    | **Systeemeigen client opnemen** | Nee | Aangezien dit een app met één pagina en geen systeemeigen client is, moet u Nee selecteren. |
    
3. Klik op **Maken** om uw app te registreren.

Geregistreerde apps worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-directory. Selecteer de app met één pagina in de lijst. Het deelvenster met de eigenschappen van de geregistreerde app met één pagina wordt weergegeven.

![Eigenschappen van een app met één pagina](./media/active-directory-b2c-tutorials-spa/b2c-spa-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de app uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de app gaat configureren.

## <a name="create-user-flows"></a>Gebruikersstromen maken

Een Azure AD B2C-gebruikersstroom definieert de gebruikerservaring voor een identiteitstaak. Zo zijn acties als registreren, aanmelden, het wijzigen van wachtwoorden en het bewerken van profielen veelvoorkomende gebruikersstromen.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Een gebruikersstroom voor registratie of aanmelding maken

Als u gebruikers wilt registreren om ze toegang te geven tot de web-app zodat ze zich daar vervolgens bij kunnen aanmelden, dan maakt u daarvoor een **gebruikersstroom voor registratie of aanmelding**.

1. Selecteer op de Azure AD B2C-portalpagina de optie **Gebruikersstromen** en klik op **Nieuwe gebruikersstroom**.
2. Klik op het tabblad **Aanbevolen** op **Registreren en aanmelden**.

    Gebruik de volgende instellingen voor het configureren van uw gebruikersstroom:

    ![Een gebruikersstroom voor registratie of aanmelding toevoegen](media/active-directory-b2c-tutorials-spa/add-susi-user-flow.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiUpIn | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **B2C_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **B2C_1_SiUpIn**. | 
    | **Id-providers** | E-mailregistratie | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |

3. Klik onder **Gebruikerskenmerken en claims** op **Meer weergeven** en selecteer de volgende instellingen:

    ![Een gebruikersstroom voor registratie of aanmelding toevoegen](media/active-directory-b2c-tutorials-spa/add-attributes-and-claims.png)

    | Kolom      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Kenmerk verzamelen** | Weergavenaam en postcode | Selecteer de kenmerken die tijdens de registratie moeten worden verzameld van de gebruiker. |
    | **Claim retourneren** | Weergavenaam, Postcode, Gebruiker is nieuw, Object-id van gebruiker | Selecteer [claims](../active-directory/develop/developer-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/developer-glossary.md#access-token). |

4. Klik op **OK**.
5. Klik op **Maken** om de gebruikersstroom te maken. 

### <a name="create-a-profile-editing-user-flow"></a>Een gebruikersstroom voor profielbewerking maken

Als u wilt toestaan dat gebruikers zelf hun gebruikersprofielgegevens opnieuw kunnen instellen, moet u een **gebruikersstroom voor profielbewerking** maken.

1. Selecteer op de Azure AD B2C-portalpagina de optie **Gebruikersstromen** en klik op **Nieuwe gebruikersstroom**.
2. Klik op het tabblad **Aanbevolen** op **Profielbewerking**.

    Gebruik de volgende instellingen voor het configureren van uw gebruikersstroom:

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiPe | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **B2C_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **B2C_1_SiPe**. | 
    | **Id-providers** | Aanmelding met lokaal account | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |

3.  Klik onder **Gebruikerskenmerken** op **Meer weergeven** en selecteer de volgende instellingen:

    | Kolom      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Kenmerk verzamelen** | Weergavenaam en postcode | Selecteer kenmerken die gebruikers bij het bewerken van het profiel kunnen wijzigen. |
    | **Claim retourneren** | Weergavenaam, Postcode, Object-id van gebruiker | Selecteer [claims](../active-directory/develop/developer-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/developer-glossary.md#access-token) nadat een profiel is bewerkt. |

4. Klik op **OK**.
5. Klik op **Maken** om de gebruikersstroom te maken. 

### <a name="create-a-password-reset-user-flow"></a>Een gebruikersstroom voor het opnieuw instellen van het wachtwoord maken

Als u wilt dat het wachtwoord voor uw toepassing opnieuw kan worden ingesteld, moet u een **gebruikersstroom voor het opnieuw instellen van wachtwoorden** maken. Deze gebruikersstroom beschrijft de ervaring van consumenten wanneer ze het wachtwoord opnieuw instellen, evenals de inhoud van tokens die de toepassing ontvangt als de bewerking is voltooid.

1. Selecteer op de Azure AD B2C-portalpagina de optie **Gebruikersstromen** en klik op **Nieuwe gebruikersstroom**.
2. Klik op het tabblad **Aanbevolen** op **Wachtwoord opnieuw instellen**.

    Gebruik de volgende instellingen om de gebruikersstroom te configureren.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SSPR | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **B2C_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **B2C_1_SSPR**. | 
    | **Id-providers** | Het wachtwoord opnieuw instellen met het e-mailadres | Dit is de identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |

3. Klik onder **Toepassingsclaims** op **Meer weergeven** en selecteer de volgende instellingen:

    | Kolom      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Claim retourneren** | Object-id van gebruiker | Selecteer [claims](../active-directory/develop/developer-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/developer-glossary.md#access-token) nadat een wachtwoord opnieuw is ingesteld. |

4. Klik op **OK**.
5. Klik op **Maken** om de gebruikersstroom te maken. 

## <a name="update-single-page-app-code"></a>Code van een app met één pagina bijwerken

Nadat u een app hebt geregistreerd en gebruikersstromen hebt gemaakt, moet u uw app configureren voor het gebruik van uw Azure AD B2C-directory. In deze zelfstudie gaat u een voorbeeld NSPA JavaScript-app configureren die u kunt downloaden vanuit GitHub. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```
De voorbeeldapp laat zien hoe een app met één pagina Azure AD B2C gebruikt voor gebruikersregistratie, aanmelding en om een beveiligde web-API aan te roepen. U moet wijzigingen aanbrengen aan de app om de app-registratie in uw directory te kunnen gebruiken en de gemaakte gebruikersstromen te configureren. 

De app-instellingen wijzigen:

1. Open het bestand `index.html` in de Node.js-voorbeeld-app van één pagina.
2. Configureer het voorbeeld met de registratiegegevens voor Azure AD B2C-directory. Wijzig de volgende regels met code (vervang de waarden door de namen van uw directory en API's):

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://fabrikamb2c.b2clogin.com/tfp/fabrikamb2c.onmicrosoft.com/B2C_1_<Sign-up or sign-in policy name>",
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://fabrikamb2chello.azurewebsites.net/hello',
    };
    ```

    De naam van de gebruikersstroom die in deze zelfstudie wordt gebruikt is **B2C_1_SiUpIn**. Als u de gebruikersstroom een andere naam hebt gegeven, gebruikt u deze naam in `authority` waarde.

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

De standaard-app biedt ondersteuning voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord. In deze zelfstudie leest u hoe gebruikers zich registreren om de app met behulp van een e-mailadres te gebruiken. U kunt zelf de andere scenario's verkennen.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

1. Klik op **Aanmelden** om aan te melden als een gebruiker van de SPA-app. Hiervoor wordt de gebruikersstroom **B2C_1_SiUpIn** gebruikt die u hebt gedefinieerd in een vorige stap.

2. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**. 

3. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in de gebruikersstroom zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op. 

    ![Aanmeldingswerkstroom](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Klik op **Maken** om een lokaal account te maken in de Azure AD B2C-directory.

Gebruikers kunnen nu hun e-mailadres gebruiken om zich aan te melden en de SPA-app te gebruiken.

> [!NOTE]
> Na aanmelding geeft de app de foutmelding 'onvoldoende machtigingen'. U krijgt deze foutmelding omdat u toegang probeert te krijgen tot een resource van de demo-directory. Omdat uw toegangstoken alleen geldig is voor uw Azure AD-directory, is de API-aanroep niet geautoriseerd. Ga door met de volgende zelfstudie voor het maken van een beveiligde web-API voor uw directory. 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-directory gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-directory verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Azure AD B2C-directory maakt, gebruikersstromen maakt, en de app met één pagina bijwerkt om uw Azure AD B2C-directory te gebruiken. Ga verder met de volgende zelfstudie als u meer wilt weten over het registreren, configureren en aanroepen van een ASP.NET-web-API vanaf een desktop-app.

> [!div class="nextstepaction"]
> [Azure AD B2C-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
