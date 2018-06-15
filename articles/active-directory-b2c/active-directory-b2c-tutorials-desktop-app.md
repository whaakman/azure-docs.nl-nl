---
title: 'Zelfstudie: verificatie van desktop-apps met accounts inschakelen met behulp van Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Azure Active Directory B2C voor het opgeven van gebruikersaanmeldingsreferenties voor een .NETdesktop-app.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 2/28/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: ff9cfd0f1f3d8ee62b7f93d88023b3dedce3e7be
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711729"
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Zelfstudie: verificatie van desktop-apps inschakelen met accounts met behulp van Azure Active Directory B2C

Deze zelfstudie laat u zien hoe u Azure Active Directory B2C (Azure AD) kunt gebruiken voor het aanmelden en registreren van gebruikers in een Windows Presentation Foundation-desktop-toepassing (WPF). Met Azure AD B2C zijn uw apps in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociale accounts, Enterprise-accounts en Azure Active Directory-accounts.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Registreer de voorbeeld desktop-app in uw Azure AD B2C-tenant.
> * Beleid te maken voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord.
> * Configureer de voorbeeld-app om uw Azure AD B2C-tenant te gebruiken.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* U moet uw eigen [Azure AD B2C-tenant](active-directory-b2c-get-started.md) maken.
* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de workloads **.NET desktopontwikkeling** en **ASP.NET- en webontwikkeling**.

## <a name="register-desktop-app"></a>Desktop-app registreren

Toepassingen moeten worden [geregistreerd](../active-directory/develop/active-directory-dev-glossary.md#application-registration) in uw tenant voordat ze [toegangstokens](../active-directory/develop/active-directory-dev-glossary.md#access-token) van Azure Active Directory kunnen ontvangen. Als een app wordt geregistreerd, wordt een [toepassings-id](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) voor de app in uw tenant gemaakt. 

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
    | **Aangepaste omleidings-URI** | `com.onmicrosoft.contoso.appname://redirect/path` | Voer `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` in Beleid verzend tokens naar deze URI. |
    
3. Klik op **Maken** om uw app te registreren.

Geregistreerde apps worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de desktop-app in de lijst. Het deelvenster met de eigenschappen van het geregistreerde desktop-deelvenster wordt weergegeven.

![Eigenschappen van de desktop-app](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de app uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de app gaat configureren.

## <a name="create-policies"></a>Beleidsregels maken

Een Azure AD B2C-beleid definieert gebruikerswerkstromen. Zo zijn acties als registreren, aanmelden, wijzigen van wachtwoorden en het bewerken van profielen veelvoorkomende werkstromen.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Registratie- of aanmeldingsbeleid maken

Als u gebruikers wilt registreren om ze toegang te geven tot de desktop-app zodat ze zich daar vervolgens bij kunnen aanmelden, dan maakt u daarvoor een **registratie- of aanmeldingsbeleid**.

1. Selecteer in de Azure AD B2C-portalpagina **Registratie- of aanmeldingsbeleid** en klik op **Toevoegen**.

    Gebruik de volgende instellingen voor het configureren van uw beleid:

    ![Een registratie- of aanmeldingsbeleid toevoegen](media/active-directory-b2c-tutorials-desktop-app/add-susi-policy.png)

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

## <a name="update-desktop-app-code"></a>Code van de desktop-app bijwerken

Nadat u een desktop-app hebt geregistreerd en beleid hebt gemaakt, moet u uw app configureren voor het gebruik van uw Azure AD B2C-tenant. In deze zelfstudie gaat u een voorbeeld-desktop-app configureren. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Het voorbeeld van de WPF-bureaubladapp laat zien hoe u een bureaublad-app kunt gebruiken van Azure AD B2C voor gebruikersregistratie, aanmelding en om een beveiligde web-API aan te roepen.

U moet wijzigingen aanbrengen aan de app om de app-registratie in uw tenant te kunnen gebruiken en het gemaakte beleid te configureren. 

De app-instellingen wijzigen:

1. Open de oplossing `active-directory-b2c-wpf` in Visual Studio.

2. Open het bestand **App.xaml.cs** in het project `active-directory-b2c-wpf`, en voer de volgende updates uit:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. Update de variabele **PolicySignUpSignIn** met de naam van het *beleid voor registreren of aanmelden* dat u in de vorige stap hebt gemaakt. Vergeet niet om het voorvoegsel *B2C_1_* op te nemen.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Voer de voorbeeld-desktoptoepassing uit

Druk op **F5** om de desktop-app op te bouwen en uit te voeren. 

De standaard-app biedt ondersteuning voor gebruikersregistratie, -aanmelding, het bewerken van een profiel en het opnieuw instellen van een wachtwoord. In deze zelfstudie leest u hoe gebruikers zich registreren om de app met behulp van een e-mailadres te gebruiken. U kunt zelf de andere scenario's verkennen.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

1. Klik op de knop **Aanmelden** om als een gebruiker van de desktop-app te registreren. Er wordt gebruikgemaakt van het beleid **B2C_1_SiUpIn** dat u hebt gedefinieerd in een vorige stap.

2. Azure AD B2C toont een aanmeldingspagina met een koppeling voor registratie. Aangezien u nog geen account hebt, klikt u op de koppeling **Nu registreren**. 

3. Tijdens de aanmeldingswerkstroom wordt een pagina weergegeven waarmee de identiteit wordt opgehaald en gecontroleerd van de gebruiker die een e-mailadres heeft gebruikt. Tijdens de aanmeldingswerkstroom worden ook het wachtwoord van de gebruiker en de aangevraagde kenmerken opgehaald die in het beleid zijn gedefinieerd.

    Gebruik een geldig e-mailadres en voer de verificatie uit met de verificatiecode. Stel een wachtwoord in. Geef waarden voor de aangevraagde kenmerken op. 

    ![Aanmeldingswerkstroom](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Klik op **Maken** als u een lokaal account wilt maken in de Azure AD B2C-tenant.

Gebruikers kunnen nu hun e-mailadres gebruiken om zich aan te melden en de desktop-app te gebruiken.

> [!NOTE]
> Als u op de knop **API aanroepen** klinkt, ontvangt u de foutmelding 'Onbevoegd'. U ontvangt deze foutmelding omdat u toegang probeert te krijgen tot een resource van de demo-tenant. Omdat uw toegangstoken alleen geldig is voor uw Azure AD-tenant, is de API-aanroep niet geautoriseerd. Ga door met de volgende zelfstudie voor het maken van een beveiligde web-API voor uw tenant. 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Azure AD B2C-tenant maakt, beleid maakt, en de voorbeeld-desktop-app moet bijwerken om uw Azure AD B2C-tenant te gebruiken. Ga verder met de volgende zelfstudie als u meer wilt weten over het registreren, configureren en aanroepen van een ASP.NET-web-API vanaf een desktop-app.

> [!div class="nextstepaction"]
> 