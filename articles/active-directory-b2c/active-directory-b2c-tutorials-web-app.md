---
title: 'Zelfstudie: Een webtoepassing inschakelen om te verifiëren met accounts met behulp van Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Azure Active Directory B2C voor het opgeven van gebruikersaanmelding voor een ASP.NET-webtoepassing.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 714d733e765f28d1244f6ee1c7b1cb237c0c4b1f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198347"
---
# <a name="tutorial-enable-a-web-application-to-authenticate-with-accounts-using-azure-active-directory-b2c"></a>Zelfstudie: Een webtoepassing inschakelen om te verifiëren met accounts met behulp van Azure Active Directory B2C

Deze zelfstudie laat u zien hoe u Azure Active Directory B2C (Azure AD) kunt gebruiken voor het aanmelden en registreren van gebruikers in een ASP.NET-web-app. Met Azure AD B2C zijn uw apps in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociale accounts, Enterprise-accounts en Azure Active Directory-accounts.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een voorbeeld van een ASP.NET-web-app te registreren in uw Azure AD B2C-tenant.
> * Gebruikersstromen maken voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord.
> * De voorbeeld-web-app te configureren om uw Azure AD B2C-tenant te gebruiken. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* U moet uw eigen [Azure AD B2C-tenant](active-directory-b2c-get-started.md) maken.
* U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload** installeren.

## <a name="register-web-app"></a>Web-app registreren

Toepassingen moeten worden [geregistreerd](../active-directory/develop/developer-glossary.md#application-registration) in uw tenant voordat ze [toegangstokens](../active-directory/develop/developer-glossary.md#access-token) van Azure Active Directory kunnen ontvangen. Als een app wordt geregistreerd, wordt een [toepassings-id](../active-directory/develop/developer-glossary.md#application-id-client-id) voor de app in uw tenant gemaakt. 

Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze. U gebruikt nu de tenant die u hebt gemaakt in de vorige zelfstudie. 

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **Toevoegen**. 

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-app in uw tenant.

    ![Een nieuw app toevoegen](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Mijn voorbeeld-web-app | Voer een **naam** in die uw app voor consumenten beschrijft. | 
    | **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een web-app. |
    | **Impliciete stroom toestaan** | Ja | Selecteer **Ja** omdat de app gebruikmaakt van [aanmelding via OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Antwoord-URL** | `https://localhost:44316` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die zijn aangevraagd voor de app. In deze zelfstudie wordt het voorbeeld lokaal (localhost) uitgevoerd, en wordt op poort 44316 geluisterd. |
    | **Systeemeigen client opnemen** | Nee | Aangezien dit een web-app en geen systeemeigen client is, moet u Nee selecteren. |
    
3. Klik op **Maken** om uw app te registreren.

Geregistreerde apps worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-app in de lijst. Het eigenschappenvenster van de web-app wordt weergegeven.

![Eigenschappen van webtoepassing](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Noteer de **toepassings-id**. Met de id wordt de app uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de app gaat configureren.

### <a name="create-a-client-password"></a>Een wachtwoord voor de client maken

Azure AD B2C maakt voor [clienttoepassingen](../active-directory/develop/developer-glossary.md#client-application) gebruik van OAuth2-autorisatie. Web-apps zijn [vertrouwelijke clients](../active-directory/develop/developer-glossary.md#web-client) en vereisen een client-ID of app-ID en een clientgeheim, clientwachtwoord of app-sleutel.

1. Selecteer de pagina Sleutels voor de geregistreerde web-app en klik op **Sleutel genereren**.

2. Klik op **Opslaan** om de app-sleutel weer te geven.

    ![pagina algemene sleutels app](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

De sleutel wordt één keer in de portal weergegeven. Het is belangrijk om de waarde van de sleutel te kopiëren en op te slaan. U hebt deze waarde nodig als u uw app gaat configureren. Bewaar de sleutel op een veilige plek. Deel de sleutel niet met anderen.

## <a name="create-user-flows"></a>Gebruikersstromen maken

Een Azure AD B2C-gebruikersstroom definieert de gebruikerservaring voor een identiteitstaak. Zo zijn acties als registreren, aanmelden, het wijzigen van wachtwoorden en het bewerken van profielen veelvoorkomende gebruikersstromen.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Een gebruikersstroom voor registratie of aanmelding maken

Als u gebruikers wilt registreren om ze toegang te geven tot de web-app zodat ze zich daar vervolgens bij kunnen aanmelden, dan maakt u daarvoor een **gebruikersstroom voor registratie of aanmelding**.

1. Selecteer op de Azure AD B2C-portalpagina de optie **Gebruikersstromen** en klik op **Nieuwe gebruikersstroom**.
2. Klik op het tabblad **Aanbevolen** op **Registreren en aanmelden**.

    Gebruik de volgende instellingen voor het configureren van uw gebruikersstroom:

    ![Een gebruikersstroom voor registratie of aanmelding toevoegen](media/active-directory-b2c-tutorials-web-app/add-susi-user-flow.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiUpIn | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **b2c_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **b2c_1_SiUpIn**. | 
    | **Id-providers** | E-mailregistratie | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |

3. Klik onder **Gebruikerskenmerken en claims** op **Meer weergeven** en selecteer de volgende instellingen:

    ![Een gebruikersstroom voor registratie of aanmelding toevoegen](media/active-directory-b2c-tutorials-web-app/add-attributes-and-claims.png)

    | Kolom      | Aanbevolen waarden  | Beschrijving                                        |
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
    | **Naam** | SiPe | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **b2c_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **b2c_1_SiPe**. | 
    | **Id-providers** | Aanmelding met lokaal account | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |

3. Klik onder **Gebruikerskenmerken** op **Meer weergeven** en selecteer de volgende instellingen:

    | Kolom      | Aanbevolen waarden  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Kenmerk verzamelen** | Weergavenaam en postcode | Selecteer kenmerken die gebruikers bij het bewerken van het profiel kunnen wijzigen. |
    | **Claim retourneren** | Weergavenaam, Postcode, Object-id van gebruiker | Selecteer [claims](../active-directory/develop/developer-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/developer-glossary.md#access-token) nadat een profiel is bewerkt. |

4. Klik op **OK**.
5. Klik op **Maken** om de gebruikersstroom te maken. 

### <a name="create-a-password-reset-user-flow"></a>Een gebruikersstroom voor het opnieuw instellen van het wachtwoord maken

Als u wilt dat het wachtwoord voor uw toepassing opnieuw kan worden ingesteld, moet u een **gebruikersstroom voor het opnieuw instellen van wachtwoorden** maken. Deze gebruikersstroom beschrijft de ervaring van consumenten wanneer ze het wachtwoord opnieuw instellen, evenals de inhoud van tokens die de toepassing ontvangt als de bewerking is voltooid.

1. Selecteer in de Azure AD B2C-portalpagina **Beleid voor het opnieuw instellen van het wachtwoord** en klik op **Toevoegen**.
2. Klik op het tabblad **Aanbevolen** op **Wachtwoord opnieuw instellen**.

    Gebruik de volgende instellingen om de gebruikersstroom te configureren.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SSPR | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **b2c_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **b2c_1_SSPR**. | 
    | **Id-providers** | Het wachtwoord opnieuw instellen met het e-mailadres | Dit is de identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |

3. Klik onder **Toepassingsclaims** op **Meer weergeven** en selecteer de volgende instellingen:
    | Kolom      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Claim retourneren** | Object-id van gebruiker | Selecteer [claims](../active-directory/develop/developer-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/developer-glossary.md#access-token) nadat een wachtwoord opnieuw is ingesteld. |

4. Klik op **OK**.
5. Klik op **Maken** om de gebruikersstroom te maken. 

## <a name="update-web-app-code"></a>Web-app-code bijwerken

Nu u een web-app hebt geregistreerd en gebruikersstromen hebt gemaakt, moet u uw app configureren voor het gebruik van uw Azure AD B2C-tenant. In deze zelfstudie configureert u een voorbeeld-web-app die u kunt downloaden uit GitHub. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub. Zorg ervoor dat u het voorbeeldbestand uitpakt in een map en dat het aantal tekens van het pad minder is dan 260.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

De voorbeeld-ASP.NET-web-app is een eenvoudige takenlijst-app waarmee u een takenlijst kunt maken en bijwerken. De app maakt gebruik van [Microsoft OWIN-middlewareonderdelen](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/) zodat gebruikers zich kunnen aanmelden en de app in uw Azure AD B2C-tenant kunnen gebruiken. Als u een Azure AD B2C-gebruikersstroom maakt, kunnen gebruikers een sociaal account gebruiken, of een account maken om te gebruiken als identiteit waarmee ze toegang krijgen tot de app. 

Er bevinden zich twee projecten in de voorbeeldoplossing:

**Voorbeeld van web-app (TaskWebApp):** Web-app voor het maken en bewerken van een takenlijst. De web-app gebruikt de **gebruikersstroom voor registratie of aanmelding** om gebruikers te registreren of aan te melden.

**Web API-voorbeeld-app (TaskService):** Web-API die ondersteuning biedt voor functionaliteit voor het maken, lezen, bijwerken en verwijderen van takenlijsten. De web-API wordt beveiligd door Azure AD B2C en wordt aangeroepen door de web-app.

U moet instellen dat de app de app-registratie in uw tenant gebruikt. Dit omvat de toepassings-id en de sleutel die u eerder hebt geregistreerd. U moet de gebruikersstromen die u hebt gemaakt ook configureren. De voorbeeld-web-app definieert de configuratiewaarden als app-instellingen in het bestand Web.config. De app-instellingen wijzigen:

1. Open de oplossing **B2C-WebAPI-DotNet** in Visual Studio.

2. In het project **TaskWebApp** in de web-app, opent u het bestand **Web.config**. Vervang de waarde voor `ida:Tenant` door de naam van de tenant die u hebt gemaakt. Vervang de waarde voor `ida:ClientId` door de toepassings-id die u hebt geregistreerd. Vervang de waarde voor `ida:ClientSecret` door de sleutel die u hebt geregistreerd.

3. Vervang in het bestand **Web.config** de waarde voor `ida:SignUpSignInPolicyId` door `b2c_1_SiUpIn`. Vervang de waarde voor `ida:EditProfilePolicyId` door `b2c_1_SiPe`. Vervang de waarde voor `ida:ResetPasswordPolicyId` door `b2c_1_SSPR`.

## <a name="run-the-sample-web-app"></a>De voorbeeld-web-app uitvoeren

Klik in Solution Explorer met de rechtermuisknop op het project **TaskWebApp** en klik op **Instellen als opstartproject**.

Druk op **F5** om de web-app te starten. De standaardbrowser wordt gestart met het adres van de lokale site `https://localhost:44316/`. 

De standaard-app biedt ondersteuning voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord. In deze zelfstudie leest u hoe gebruikers zich registreren om de app met behulp van een e-mailadres te gebruiken. U kunt zelf de andere scenario's verkennen.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

1. Klik op de koppeling **Registreren/Aanmelden** in de bovenste banner om te registreren als een gebruiker van de web-app. Hiervoor wordt de gebruikersstroom **b2c_1_SiUpIn** gebruikt die u hebt gedefinieerd in een vorige stap.

2. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**. 

3. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in de gebruikersstroom zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op. 

    ![Aanmeldingswerkstroom](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Klik op **Maken** als u een lokaal account wilt maken in de Azure AD B2C-tenant.

Gebruikers kunnen nu hun e-mailadres gebruiken om zich aan te melden en de web-app te gebruiken.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Azure AD B2C-tenant maakt, gebruikersstromen maakt en de voorbeeld-web-app bijwerkt om uw Azure AD B2C-tenant te gebruiken. Ga verder met de volgende zelfstudie als u meer wilt weten over het registreren, configureren en aanroepen van een ASP.NET-web-API die is beveiligd door uw Azure AD B2C-tenant.

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Active Directory B2C gebruiken voor het beveiligen van een ASP.NET-web-API](active-directory-b2c-tutorials-web-api.md)
