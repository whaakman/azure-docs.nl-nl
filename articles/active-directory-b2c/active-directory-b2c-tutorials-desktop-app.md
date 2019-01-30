---
title: 'Zelfstudie: verificatie van desktop-apps met accounts inschakelen met behulp van Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Azure Active Directory B2C voor het opgeven van gebruikersaanmeldingsreferenties voor een .NETdesktop-app.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 357b9f4d307624db838b22581097799d7d7fef4c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856992"
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Zelfstudie: Verificatie van desktop-apps inschakelen met accounts met behulp van Azure Active Directory B2C

Deze zelfstudie laat u zien hoe u Azure Active Directory B2C (Azure AD) kunt gebruiken voor het aanmelden en registreren van gebruikers in een Windows Presentation Foundation-desktop-toepassing (WPF). Met Azure AD B2C zijn uw apps in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociale accounts, Enterprise-accounts en Azure Active Directory-accounts.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Registreer de voorbeeld desktop-app in uw Azure AD B2C-tenant.
> * Gebruikersstromen maken voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord.
> * Configureer de voorbeeld-app om uw Azure AD B2C-tenant te gebruiken.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* U moet uw eigen [Azure AD B2C-tenant](active-directory-b2c-get-started.md) maken.
* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de workloads **.NET desktopontwikkeling** en **ASP.NET- en webontwikkeling**.

## <a name="register-desktop-app"></a>Desktop-app registreren

Toepassingen moeten worden [geregistreerd](../active-directory/develop/developer-glossary.md#application-registration) in uw tenant voordat ze [toegangstokens](../active-directory/develop/developer-glossary.md#access-token) van Azure Active Directory kunnen ontvangen. Als een app wordt geregistreerd, wordt een [toepassings-id](../active-directory/develop/developer-glossary.md#application-id-client-id) voor de app in uw tenant gemaakt. 

Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecteer **Azure AD B2C** in de lijst met services in Azure Portal. 

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **Toevoegen**. 

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-app in uw tenant.
    
    ![Een nieuw app toevoegen](media/active-directory-b2c-tutorials-desktop-app/desktop-app-registration.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Mijn voorbeeld-WPF-app | Voer een **naam** in die uw app voor consumenten beschrijft. | 
    | **Web-app / web-API opnemen** | Nee | Selecteer **Nee** voor een desktop-app. |
    | **Systeemeigen client opnemen** | Ja | Omdat dit een desktop-app is, wordt deze beschouwd als een native client. |
    | **Omleidings-URI** | Standaardwaarden | Een unieke id waarnaar de gebruikersagent in een OAuth 2.0-antwoord wordt omgeleid met Azure AD B2C. |
    | **Aangepaste omleidings-URI** | `com.onmicrosoft.contoso.appname://redirect/path` | Voer `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` in in Gebruikersstromen verzenden tokens naar deze URI. |
    
3. Klik op **Maken** om uw app te registreren.

Geregistreerde apps worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de desktop-app in de lijst. Het deelvenster met de eigenschappen van het geregistreerde desktop-deelvenster wordt weergegeven.

![Eigenschappen van de desktop-app](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de app uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de app gaat configureren.

## <a name="create-user-flows"></a>Gebruikersstromen maken

Een Azure AD B2C-gebruikersstroom definieert de gebruikerservaring voor een identiteitstaak. Zo zijn acties als registreren, aanmelden, het wijzigen van wachtwoorden en het bewerken van profielen veelvoorkomende gebruikersstromen.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Een gebruikersstroom voor registratie of aanmelding maken

Als u gebruikers wilt registreren om ze toegang te geven tot de desktop-app zodat ze zich daar vervolgens bij kunnen aanmelden, dan maakt u daarvoor een **gebruikersstroom voor registratie of aanmelding**.

1. Selecteer op de Azure AD B2C-portalpagina de optie **Gebruikersstromen** en klik op **Nieuwe gebruikersstroom**.
2. Klik op het tabblad **Aanbevolen** op **Registreren en aanmelden**.

    Gebruik de volgende instellingen voor het configureren van uw gebruikersstroom:

    ![Een gebruikersstroom voor registratie of aanmelding toevoegen](media/active-directory-b2c-tutorials-desktop-app/add-susi-user-flow.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiUpIn | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **B2C_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **B2C_1_SiUpIn**. | 
    | **Id-providers** | E-mailregistratie | De identiteitsprovider waarmee de gebruiker uniek wordt aangeduid. |

3.  Klik onder **Gebruikerskenmerken en claims** op **Meer weergeven** en selecteer de volgende instellingen:

    ![Gebruikerskenmerken en claims toevoegen](media/active-directory-b2c-tutorials-desktop-app/add-attributes-and-claims.png)

    | Kolom      | Aanbevolen waarden  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Kenmerk verzamelen** | Weergavenaam en postcode | Selecteer de kenmerken die tijdens de registratie moeten worden verzameld van de gebruiker. |
    | **Claim retourneren** | Weergavenaam, Postcode, Gebruiker is nieuw, Object-id van gebruiker | Selecteer [claims](../active-directory/develop/developer-glossary.md#claim) die u wilt opnemen in de [toegangstoken](../active-directory/develop/developer-glossary.md#access-token). |

4. Klik op **OK**.

5. Klik op **Maken** om de gebruikersstroom te maken. 

### <a name="create-a-profile-editing-user-flow"></a>Een gebruikersstroom voor profielbewerking maken

Als u wilt toestaan dat gebruikers zelf hun gebruikersprofielgegevens opnieuw kunnen instellen, moet u een **gebruikersstroom voor profielbewerking** maken.

1. Selecteer op de Azure AD B2C-portalpagina de optie **Gebruikersstroom** en klik op **Nieuwe gebruikersstroom**.
2. Klik op het tabblad **Aanbevolen** op **Profielbewerking**.

    Gebruik de volgende instellingen voor het configureren van uw gebruikersstroom:

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | SiPe | Voer een **Naam** in voor de gebruikersstroom. De naam van de gebruikersstroom wordt voorafgegaan door **B2C_1_**. In de voorbeeldcode gebruikt u de volledige naam van de gebruikersstroom **B2C_1_SiPe**. | 
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

## <a name="update-desktop-app-code"></a>Code van de desktop-app bijwerken

Nu u een desktop-app hebt geregistreerd en gebruikersstromen hebt gemaakt, moet u uw app configureren voor het gebruik van uw Azure AD B2C-tenant. In deze zelfstudie gaat u een voorbeeld-desktop-app configureren. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [blader door de opslagplaats](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) of kloon de voorbeeldweb-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Het voorbeeld van de WPF-bureaubladapp laat zien hoe u een bureaublad-app kunt gebruiken van Azure AD B2C voor gebruikersregistratie, aanmelding en om een beveiligde web-API aan te roepen.

U moet wijzigingen aanbrengen aan de app om de app-registratie in uw tenant te kunnen gebruiken en de gemaakte gebruikersstromen te configureren. 

De app-instellingen wijzigen:

1. Open de oplossing `active-directory-b2c-wpf` in Visual Studio.

2. Open het bestand **App.xaml.cs** in het project `active-directory-b2c-wpf`, en voer de volgende updates uit:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. Werk de variabele **PolicySignUpSignIn** bij met de naam van de *gebruikersstroom voor registreren of aanmelden* die u in de vorige stap hebt gemaakt. Vergeet niet om het voorvoegsel *B2C_1_* op te nemen.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Voer de voorbeeld-desktoptoepassing uit

Druk op **F5** om de desktop-app op te bouwen en uit te voeren. 

De standaard-app biedt ondersteuning voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord. In deze zelfstudie leest u hoe gebruikers zich registreren om de app met behulp van een e-mailadres te gebruiken. U kunt zelf de andere scenario's verkennen.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

1. Klik op de knop **Aanmelden** om als een gebruiker van de desktop-app te registreren. Hiervoor wordt de gebruikersstroom **B2C_1_SiUpIn** gebruikt die u hebt gedefinieerd in een vorige stap.

2. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**. 

3. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in de gebruikersstroom zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op. 

    ![Aanmeldingswerkstroom](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Klik op **Maken** als u een lokaal account wilt maken in de Azure AD B2C-tenant.

Gebruikers kunnen nu hun e-mailadres gebruiken om zich aan te melden en de desktop-app te gebruiken.

> [!NOTE]
> Als u op de knop **API aanroepen** klinkt, ontvangt u de foutmelding 'Onbevoegd'. U ontvangt deze foutmelding omdat u toegang probeert te krijgen tot een resource van de demo-tenant. Omdat uw toegangstoken alleen geldig is voor uw Azure AD-tenant, is de API-aanroep niet geautoriseerd. Ga door met de volgende zelfstudie voor het maken van een beveiligde web-API voor uw tenant. 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Azure AD B2C-tenant maakt, gebruikersstromen maakt en de voorbeeld-desktop-app bijwerkt om uw Azure AD B2C-tenant te gebruiken. Ga verder met de volgende zelfstudie als u meer wilt weten over het registreren, configureren en aanroepen van een ASP.NET-web-API vanaf een desktop-app.

> [!div class="nextstepaction"]
> 
