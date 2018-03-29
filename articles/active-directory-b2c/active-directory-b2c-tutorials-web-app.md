---
title: Zelfstudie Azure Active Directory B2C gebruiken voor gebruikersverificatie in een voor een ASP.NET-web-app
description: Zelfstudie over het gebruik van Azure Active Directory B2C voor het opgeven van gebruikersaanmeldingsreferenties voor een ASP.NET-web-app.
services: active-directory-b2c
author: davidmu1
ms.author: davidmu
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: c2a52a387860de640e290746b25c164090819654
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-authenticate-users-with-azure-active-directory-b2c-in-an-aspnet-web-app"></a>Zelfstudie: Verificatie van gebruikers met Azure Active Directory B2C in een ASP.NET-web-app

Deze zelfstudie laat u zien hoe u Azure Active Directory B2C (Azure AD) kunt gebruiken voor het aanmelden en registreren van gebruikers in een ASP.NET-web-app. Met Azure AD B2C zijn uw apps in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociale accounts, Enterprise-accounts en Azure Active Directory-accounts.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een voorbeeld van een ASP.NET-web-app te registreren in uw Azure AD B2C-tenant.
> * Beleid te maken voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord.
> * De voorbeeld-web-app te configureren om uw Azure AD B2C-tenant te gebruiken. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* U moet uw eigen [Azure AD B2C-tenant](active-directory-b2c-get-started.md) maken.
* U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload** installeren.

## <a name="register-web-app"></a>Web-app registreren

Toepassingen moeten worden [geregistreerd](../active-directory/develop/active-directory-dev-glossary.md#application-registration) in uw tenant voordat ze [toegangstokens](../active-directory/develop/active-directory-dev-glossary.md#access-token) van Azure Active Directory kunnen ontvangen. Als een app wordt geregistreerd, wordt een [toepassings-id](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) voor de app in uw tenant gemaakt. 

Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecteer **Azure AD B2C** in de lijst met services in Azure Portal.

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **Toevoegen**.

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-app in uw tenant.

    ![Een nieuw app toevoegen](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Mijn voorbeeld-web-app | Voer een **naam** in die uw app voor consumenten beschrijft. | 
    | **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een web-app. |
    | **Impliciete stroom toestaan** | Ja | Selecteer **Ja** omdat de app gebruikmaakt van [aanmelding via OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Antwoord-URL** | `https://localhost:44316` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die zijn aangevraagd voor de app. In deze zelfstudie wordt het voorbeeld lokaal (localhost) uitgevoerd, en wordt op poort 44316 geluisterd. |
    | **Systeemeigen client** | Nee | Aangezien dit een web-app en geen systeemeigen client is, moet u Nee selecteren. |

3. Klik op **Maken** om uw app te registreren.

Geregistreerde apps worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-app in de lijst. Het eigenschappenvenster van de web-app wordt weergegeven.

![Eigenschappen van webtoepassing](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de app uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de app gaat configureren.

### <a name="create-a-client-password"></a>Een wachtwoord voor de client maken

Azure AD B2C maakt voor [clienttoepassingen](../active-directory/develop/active-directory-dev-glossary.md#client-application) gebruik van OAuth2-autorisatie. Web-apps zijn [vertrouwelijke clients](../active-directory/develop/active-directory-dev-glossary.md#web-client) en vereisen een clientgeheim (wachtwoord). De client-id voor de toepassing en het clientgeheim worden gebruikt wanneer de web-app een verificatie uitvoert met Azure Active Directory. 

1. Selecteer de pagina Sleutels voor de geregistreerde web-app en klik op **Sleutel genereren**.

2. Klik op **Opslaan** om de sleutel weer te geven.

    ![pagina algemene sleutels app](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

De sleutel wordt één keer in de portal weergegeven. Het is belangrijk om de waarde van de sleutel te kopiëren en op te slaan. U hebt deze waarde nodig als u uw app gaat configureren. Bewaar de sleutel op een veilige plek. Deel de sleutel niet met anderen.

## <a name="create-policies"></a>Beleidsregels maken

Een Azure AD B2C-beleid definieert gebruikerswerkstromen. Zo zijn acties als registreren, aanmelden, wijzigen van wachtwoorden en het bewerken van profielen veelvoorkomende werkstromen.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Registratie- of aanmeldingsbeleid maken

Als u gebruikers wilt registreren om ze toegang te geven tot de web-app zodat ze zich daar vervolgens bij kunnen aanmelden, dan maakt u daarvoor een **registratie- of aanmeldingsbeleid**.

1. Selecteer in de Azure AD B2C-portalpagina **Registratie- of aanmeldingsbeleid** en klik op **Toevoegen**.

    Gebruik de volgende instellingen voor het configureren van uw beleid:

    ![Een registratie- of aanmeldingsbeleid toevoegen](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiUpIn | Voer een **naam** in voor het beleid. De naam van het beleid wordt voorafgegaan door **b2c_1_**. Gebruik de volledige beleidsnaam **b2c_1_SiUpIn** in de voorbeeldcode. | 
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
    | **Naam** | SiPe | Voer een **naam** in voor het beleid. De naam van het beleid wordt voorafgegaan door **b2c_1_**. Gebruik de volledige beleidsnaam **b2c_1_SiPe** in de voorbeeldcode. | 
    | **Identiteitsprovider** | Aanmelding met lokaal account | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |
    | **Profielkenmerken** | Weergavenaam en postcode | Selecteer kenmerken die gebruikers bij het bewerken van het profiel kunnen wijzigen. |
    | **Toepassingsclaims** | Weergavenaam, Postcode, Gebruiker is nieuw, Object-id van gebruiker | Selecteer [claims](../active-directory/develop/active-directory-dev-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) nadat een profiel is bewerkt. |

2. Klik op **Maken** om het beleid te maken. 

### <a name="create-a-password-reset-policy"></a>Een beleid voor het opnieuw instellen van het wachtwoord maken

Als u wilt dat het wachtwoord voor uw toepassing opnieuw kan worden ingesteld, moet u een **beleid voor het opnieuw instellen van wachtwoorden** maken. Door dit beleid wordt de ervaring van consumenten beschreven als ze het wachtwoord opnieuw gaan instellen, evenals de inhoud van tokens die de toepassing ontvangt als de bewerking is voltooid.

1. Selecteer in de Azure AD B2C-portalpagina **Beleid voor het opnieuw instellen van het wachtwoord** en klik op **Toevoegen**.

    Gebruik de volgende instellingen voor het configureren van uw beleid.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SSPR | Voer een **naam** in voor het beleid. De naam van het beleid wordt voorafgegaan door **b2c_1_**. Gebruik de volledige beleidsnaam **b2c_1_SSPR** in de voorbeeldcode. | 
    | **Identiteitsprovider** | Het wachtwoord opnieuw instellen met het e-mailadres | Dit is de identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |
    | **Toepassingsclaims** | Object-id van gebruiker | Selecteer [claims](../active-directory/develop/active-directory-dev-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) nadat een wachtwoord opnieuw is ingesteld. |

2. Klik op **Maken** om het beleid te maken. 

## <a name="update-web-app-code"></a>Web-app-code bijwerken

Nadat u een web-app hebt geregistreerd en beleid hebt gemaakt, moet u uw app configureren voor het gebruik van uw Azure AD B2C-tenant. In deze zelfstudie gaat u een voorbeeld-web-app configureren. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

De voorbeeld-ASP.NET-web-app is een eenvoudige takenlijst-app waarmee u een takenlijst kunt maken en bijwerken. De app maakt gebruik van [Microsoft OWIN-middlewareonderdelen](https://docs.microsoft.com/en-us/aspnet/aspnet/overview/owin-and-katana/) zodat gebruikers zich kunnen aanmelden en de app in uw Azure AD B2C-tenant kunnen gebruiken. Als u een Azure AD B2C-beleid maakt, kunnen gebruikers een sociaal account gebruiken, of een account maken om te gebruiken als identiteit waarmee ze toegang krijgen tot de app. 

Er bevinden zich twee projecten in de voorbeeldoplossing:

**Voorbeeld-web-app (TaskWebApp):** een web-app om een takenlijst te maken of te bewerken. De web-app gebruikt het **registratie- of aanmeldingsbeleid** om gebruikers te registreren of aan te melden.

**Voorbeeld-web-API-app (TaskService):** een web-API die ondersteuning biedt voor het maken, lezen, bijwerken en verwijderen van takenlijstfunctionaliteit. De web-API wordt beveiligd door Azure AD B2C en wordt aangeroepen door de web-app.

U moet wijzigingen aanbrengen aan de app om de app-registratie in uw tenant te kunnen gebruiken. U moet ook de beleidsregels configureren die u hebt gemaakt. De voorbeeld-web-app definieert de configuratiewaarden als app-instellingen in het bestand Web.config. De app-instellingen wijzigen:

1. Open de oplossing **B2C-WebAPI-DotNet** in Visual Studio.

2. In het project **TaskWebApp** in de web-app, opent u het bestand **Web.config** en brengt u de volgende updates aan:

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    
    <add key="ida:ClientId" value="The Application ID for your web app registered in your tenant" />
    
    <add key="ida:ClientSecret" value="Client password (client secret)" />
    ```
3. Werk de beleidsinstellingen bij met de naam die is gegenereerd toen u uw beleid maakte.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="b2c_1_SiUpIn" />
    <add key="ida:EditProfilePolicyId" value="b2c_1_SiPe" />
    <add key="ida:ResetPasswordPolicyId" value="b2c_1_SSPR" />
    ```

## <a name="run-the-sample-web-app"></a>De voorbeeld-web-app uitvoeren

Klik in Solution Explorer met de rechtermuisknop op het project **TaskWebApp** en klik op **Instellen als opstartproject**.

Druk op **F5** om de web-app te starten. De standaardbrowser wordt gestart met het adres van de lokale site `https://localhost:44316/`. 

De standaard-app biedt ondersteuning voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord. In deze zelfstudie leest u hoe gebruikers zich registreren om de app met behulp van een e-mailadres te gebruiken. U kunt zelf de andere scenario's verkennen.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

1. Klik op de koppeling **Registreren/Aanmelden** in de bovenste banner om te registreren als een gebruiker van de web-app. Er wordt gebruikgemaakt van het beleid **b2c_1_SiUpIn** dat u hebt gedefinieerd in een vorige stap.

2. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**. 

3. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in het beleid zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op. 

    ![Aanmeldingswerkstroom](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Klik op **Maken** als u een lokaal account wilt maken in de Azure AD B2C-tenant.

Gebruikers kunnen nu hun e-mailadres gebruiken om zich aan te melden en de web-app te gebruiken.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Azure AD B2C-tenant maakt, beleid maakt, en de voorbeeld-web-app moet bijwerken om uw Azure AD B2C-tenant te gebruiken. Ga verder met de volgende zelfstudie als u meer wilt weten over het registreren, configureren en aanroepen van een ASP.NET-web-API die is beveiligd door uw Azure AD B2C-tenant.

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Active Directory B2C gebruiken voor het beveiligen van een ASP.NET-web-API](active-directory-b2c-tutorials-web-api.md)
