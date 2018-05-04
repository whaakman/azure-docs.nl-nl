---
title: 'Zelfstudie: verificatie van apps met één pagina inschakelen met behulp van Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Azure Active Directory B2C voor het opgeven van gebruikersaanmeldingsreferenties voor een toepassing met één pagina (JavaScript).
services: active-directory-b2c
author: PatAltimore
ms.author: patricka
ms.reviewer: paraj
ms.date: 3/02/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: 9f5f98ae5798cabd90c453221fe36f17052f77aa
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-enable-single-page-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Zelfstudie: verificatie van apps met één pagina inschakelen met behulp van Azure Active Directory B2C

Deze zelfstudie laat u zien hoe u Azure Active Directory B2C (Azure AD) kunt gebruiken voor het aanmelden en registreren van gebruikers in een toepassing met één pagina (SPA). Met Azure AD B2C zijn uw apps in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociale accounts, Enterprise-accounts en Azure Active Directory-accounts.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Registreer een voorbeeldtoepassing met één pagina in uw Azure AD B2C-tenant.
> * Beleid te maken voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord.
> * Configureer de voorbeeld-app om uw Azure AD B2C-tenant te gebruiken.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* U moet uw eigen [Azure AD B2C-tenant](active-directory-b2c-get-started.md) maken.
* U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload** installeren.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) of hoger
* [Node.js](https://nodejs.org/en/download/) installeren

## <a name="register-single-page-app"></a>Registreer de app met één pagina

Toepassingen moeten worden [geregistreerd](../active-directory/develop/active-directory-dev-glossary.md#application-registration) in uw tenant voordat ze [toegangstokens](../active-directory/develop/active-directory-dev-glossary.md#access-token) van Azure Active Directory kunnen ontvangen. Als een app wordt geregistreerd, wordt een [toepassings-id](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) voor de app in uw tenant gemaakt. 

Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecteer **Azure AD B2C** in de lijst met services in Azure Portal. 

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **Toevoegen**. 

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-app in uw tenant.
    
    ![Een nieuw app toevoegen](media/active-directory-b2c-tutorials-spa/spa-registration.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Mijn voorbeeld-app met één pagina | Voer een **naam** in die uw app voor consumenten beschrijft. | 
    | **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een app met één pagina. |
    | **Impliciete stroom toestaan** | Ja | Selecteer **Ja** omdat de app gebruikmaakt van [aanmelding via OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Antwoord-URL** | `http://localhost:6420` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die zijn aangevraagd voor de app. In deze zelfstudie wordt het voorbeeld lokaal (localhost) uitgevoerd en luistert dit op poort 6420. |
    | **Systeemeigen client opnemen** | Nee | Aangezien dit een app met één pagina en geen systeemeigen client is, moet u Nee selecteren. |
    
3. Klik op **Maken** om uw app te registreren.

Geregistreerde apps worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de app met één pagina in de lijst. Het deelvenster met de eigenschappen van de geregistreerde app met één pagina wordt weergegeven.

![Eigenschappen van een app met één pagina](./media/active-directory-b2c-tutorials-spa/b2c-spa-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de app uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de app gaat configureren.

## <a name="create-policies"></a>Beleidsregels maken

Een Azure AD B2C-beleid definieert gebruikerswerkstromen. Zo zijn acties als registreren, aanmelden, wijzigen van wachtwoorden en het bewerken van profielen veelvoorkomende werkstromen.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Registratie- of aanmeldingsbeleid maken

Als u gebruikers wilt registreren om ze toegang te geven tot de web-app zodat ze zich daar vervolgens bij kunnen aanmelden, dan maakt u daarvoor een **registratie- of aanmeldingsbeleid**.

1. Selecteer in de Azure AD B2C-portalpagina **Registratie- of aanmeldingsbeleid** en klik op **Toevoegen**.

    Gebruik de volgende instellingen voor het configureren van uw beleid:

    ![Een registratie- of aanmeldingsbeleid toevoegen](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiUpIn | Voer een **naam** in voor het beleid. De naam van het beleid wordt voorafgegaan door **B2C_1_**. Gebruik de volledige beleidsnaam **B2C_1_SiUpIn** in de voorbeeldcode. | 
    | **Identiteitsprovider** | E-mailregistratie | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |
    | **Registratiekenmerken** | Weergavenaam en postcode | Selecteer de kenmerken die tijdens de registratie moeten worden verzameld van de gebruiker. |
    | **Toepassingsclaims** | Weergavenaam, Postcode, Gebruiker is nieuw, Object-id van gebruiker | Selecteer [claims](../active-directory/develop/active-directory-dev-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token). |

2. Klik op **Maken** om het beleid te maken. 

### <a name="create-a-profile-editing-policy"></a>Een beleid voor profielbewerking maken

Als u wilt toestaan dat gebruikers zelf hun gebruikersprofielgegevens opnieuw kunnen instellen, moet u een **beleid voor profielbewerking** maken.

1. Selecteer in de Azure AD B2C-portalpagina **Beleid voor profielbewerking** en klik op **Toevoegen**.

    Gebruik de volgende instellingen voor het configureren van uw beleid:

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiPe | Voer een **naam** in voor het beleid. De naam van het beleid wordt voorafgegaan door **B2C_1_**. Gebruik de volledige beleidsnaam **B2C_1_SiPe** in de voorbeeldcode. | 
    | **Identiteitsprovider** | Aanmelding met lokaal account | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |
    | **Profielkenmerken** | Weergavenaam en postcode | Selecteer kenmerken die gebruikers bij het bewerken van het profiel kunnen wijzigen. |
    | **Toepassingsclaims** | Weergavenaam, Postcode, Object-id van gebruiker | Selecteer [claims](../active-directory/develop/active-directory-dev-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) nadat een profiel is bewerkt. |

2. Klik op **Maken** om het beleid te maken. 

### <a name="create-a-password-reset-policy"></a>Een beleid voor het opnieuw instellen van het wachtwoord maken

Als u wilt dat het wachtwoord voor uw toepassing opnieuw kan worden ingesteld, moet u een **beleid voor het opnieuw instellen van wachtwoorden** maken. Door dit beleid wordt de ervaring van consumenten beschreven als ze het wachtwoord opnieuw gaan instellen, evenals de inhoud van tokens die de toepassing ontvangt als de bewerking is voltooid.

1. Selecteer in de Azure AD B2C-portalpagina **Beleid voor het opnieuw instellen van het wachtwoord** en klik op **Toevoegen**.

    Gebruik de volgende instellingen voor het configureren van uw beleid.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SSPR | Voer een **naam** in voor het beleid. De naam van het beleid wordt voorafgegaan door **B2C_1_**. Gebruik de volledige beleidsnaam **B2C_1_SSPR** in de voorbeeldcode. | 
    | **Identiteitsprovider** | Het wachtwoord opnieuw instellen met het e-mailadres | Dit is de identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |
    | **Toepassingsclaims** | Object-id van gebruiker | Selecteer [claims](../active-directory/develop/active-directory-dev-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) nadat een wachtwoord opnieuw is ingesteld. |

2. Klik op **Maken** om het beleid te maken. 

## <a name="update-single-page-app-code"></a>Code van een app met één pagina bijwerken

Nadat u een app hebt geregistreerd en beleid hebt gemaakt, moet u uw app configureren voor het gebruik van uw Azure AD B2C-tenant. In deze zelfstudie gaat u een voorbeeld NSPA JavaScript-app configureren die u kunt downloaden vanuit GitHub. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```
De voorbeeldapp laat zien hoe een app met één pagina Azure AD B2C gebruikt voor gebruikersregistratie, aanmelding en om een beveiligde web-API aan te roepen. U moet wijzigingen aanbrengen aan de app om de app-registratie in uw tenant te kunnen gebruiken en het gemaakte beleid te configureren. 

De app-instellingen wijzigen:

1. Open het bestand `index.html` in de Node.js-voorbeeld-app van één pagina.
2. Configureer het voorbeeld met de registratiegegevens voor Azure AD B2C-tenant. Wijzig de volgende regels code:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_SiUpIn",
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://fabrikamb2chello.azurewebsites.net/hello',
    };
    ```

    De beleidsnaam die in deze zelfstudie wordt gebruikt is **B2C_1_SiUpIn**. Als u een andere beleidsnaam gebruikt, gebruikt u uw beleidsnaam in `authority` waarde.

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

1. Klik op **Aanmelden** om aan te melden als een gebruiker van de SPA-app. Er wordt gebruikgemaakt van het beleid **B2C_1_SiUpIn** dat u hebt gedefinieerd in een vorige stap.

2. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**. 

3. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in het beleid zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op. 

    ![Aanmeldingswerkstroom](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Klik op **Maken** als u een lokaal account wilt maken in de Azure AD B2C-tenant.

Gebruikers kunnen nu hun e-mailadres gebruiken om zich aan te melden en de SPA-app te gebruiken.

> [!NOTE]
> Na aanmelding geeft de app de foutmelding 'onvoldoende machtigingen'. U ontvangt deze foutmelding omdat u toegang probeert te krijgen tot een resource van de demo-tenant. Omdat uw toegangstoken alleen geldig is voor uw Azure AD-tenant, is de API-aanroep niet geautoriseerd. Ga door met de volgende zelfstudie voor het maken van een beveiligde web-API voor uw tenant. 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Azure AD B2C-tenant maakt, beleid maakt, en de app met één pagina moet bijwerken om uw Azure AD B2C-tenant te gebruiken. Ga verder met de volgende zelfstudie als u meer wilt weten over het registreren, configureren en aanroepen van een ASP.NET-web-API vanaf een desktop-app.

> [!div class="nextstepaction"]
> 
