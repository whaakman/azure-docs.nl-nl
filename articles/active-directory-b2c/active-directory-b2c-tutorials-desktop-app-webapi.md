---
title: 'Zelfstudie: toegang verlenen aan een web-API van Node.js vanuit een desktop-app met behulp van Azure Active Directory B2C | Microsoft Docs'
description: Zelfstudie over het gebruik van Active Directory B2C om een Node.js web-API te beveiligen en aan te roepen vanuit een .NET-desktop-app.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 3/01/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: b9708d7b808888fdab41fa00ce667a4418102a23
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180378"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Zelfstudie: Toegang verlenen aan een web-API van Node.js vanuit een desktop-app met behulp van Azure Active Directory B2C

Deze zelfstudie laat zien hoe u een Node.js web-API-resource moet aanroepen die wordt beveiligd door een Azure Active Directory (Azure AD) B2C, vanuit een Windows Presentation Foundation (WPF) desktop-app.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een web-API registreren in uw Azure AD B2C-tenant
> * Bereiken voor een web-API definiëren en configureren
> * App-machtigingen verlenen aan de web-API
> * Voorbeeldcode bijwerken zodat Azure AD B2C wordt gebruikt om een web-API te beveiligen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Voltooi de zelfstudie [Verificatie van gebruikers met Azure Active Directory B2C in een desktop-app](active-directory-b2c-tutorials-desktop-app.md).
* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de workloads **.NET desktopontwikkeling** en **ASP.NET- en webontwikkeling**.
* [Node.js](https://nodejs.org/en/download/) installeren

## <a name="register-web-api"></a>Web-API registreren

Web-API-resources moeten worden geregistreerd in uw tenant voordat deze in staat zijn om [beveiligde resourceaanvragen](../active-directory/develop/developer-glossary.md#resource-server) door [clienttoepassingen](../active-directory/develop/developer-glossary.md#client-application) die een [toegangstoken](../active-directory/develop/developer-glossary.md#access-token) van Azure Active Directory aanbieden, kunnen accepteren en erop kunnen reageren. Tijdens de registratie wordt de [toepassing en het service-principal-object](../active-directory/develop/developer-glossary.md#application-object) in uw tenant opgezet. 

Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecteer **Azure AD B2C** in de lijst met services in Azure Portal.

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **Toevoegen**.

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-API in uw tenant.
    
    ![Een nieuwe API toevoegen](media/active-directory-b2c-tutorials-desktop-app-webapi/web-api-registration.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Mijn Node.js-web-API-voorbeeld | Voer een **naam** in die uw web-API aan ontwikkelaars beschrijft. |
    | **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een web-API. |
    | **Impliciete stroom toestaan** | Ja | Selecteer **Ja** omdat de API gebruikmaakt van [aanmelding via OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Antwoord-URL** | `http://localhost:5000` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die worden aangevraagd door uw API. In deze zelfstudie wordt de voorbeeld-web-API lokaal (localhost) uitgevoerd en luistert deze op poort 5000. |
    | **App-id-URI** | demoapi | De URI is een unieke identificatie van de API in de tenant. Hierdoor kunt u meerdere API's per tenant registreren. [Bereiken](../active-directory/develop/developer-glossary.md#scopes) bepalen de toegang tot de beveiligde API-resource en worden per app-id-URI gedefinieerd. |
    | **Systeemeigen client** | Nee | Aangezien dit een web-API en geen systeemeigen client is, moet u Nee selecteren. |
    
3. Klik op **Maken** om uw API te registreren.

Geregistreerde API's worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-API in de lijst. Het eigenschappenvenster van de web-API wordt weergegeven.

![Eigenschappen van de web-API](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de API uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de API gaat configureren.

Door de web-API bij Azure AD B2C te registeren, wordt een vertrouwensrelatie gedefinieerd. Omdat de API bij B2C is geregistreerd, kan de API nu de B2C-toegangstokens vertrouwen die van andere toepassingen worden ontvangen.

## <a name="define-and-configure-scopes"></a>Bereiken definiëren en configureren

[Bereiken](../active-directory/develop/developer-glossary.md#scopes) bieden een manier om toegang tot beveiligde resources te reguleren. Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Sommige gebruikers kunnen bijvoorbeeld zowel lees- als schrijftoegang hebben, terwijl andere gebruikers mogelijk alleen-lezen-machtigingen hebben. In deze zelfstudie gaat u lees- en schrijfmachtigingen definiëren voor de web-API.

### <a name="define-scopes-for-the-web-api"></a>Bereiken definiëren voor de web-API

Geregistreerde API's worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-API in de lijst. Het eigenschappenvenster van de web-API wordt weergegeven.

Klik op **Gepubliceerde bereiken (preview-versie)**.

Voeg de volgende gegevens toe om bereiken voor de API te configureren. 

![bereiken die zijn gedefinieerd in een web-api](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Instelling      | Voorgestelde waarde  | Beschrijving                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Bereik** | demo.read | Leestoegang tot demo API|

Klik op **Opslaan**.

De gepubliceerde bereiken kunnen worden gebruikt om een client-appmachtiging te verlenen aan de web-API.

### <a name="grant-app-permissions-to-web-api"></a>App-machtigingen verlenen aan een web-API

Als u een beveiligde web-API wilt aanroepen vanuit een app, moet u uw app machtigingen verlenen tot de API. In deze zelfstudie gebruikt u de desktop-app die u hebt gemaakt in [Verificatie van gebruikers met Azure Active Directory B2C in een desktop-toepassing](active-directory-b2c-tutorials-desktop-app.md).

1. Selecteer in Azure Portal **Azure AD B2C** in de lijst met services, en klik op **Toepassingen** om de lijst met geregistreerde apps weer te geven.

2. Selecteer **Mijn voorbeeld-WPF-app** in de lijst met apps, klik op **API-toegang (preview-versie)** en vervolgens op **Toevoegen**.

3. In de vervolgkeuzelijst **API selecteren** selecteert u uw geregistreerde web-API **Mijn Node.js-web-API-voorbeeld**.

4. In de vervolgkeuzelijst **Bereiken selecteren** selecteert u de bereiken die u hebt gedefinieerd toen u de web-API registreerde.

    ![bereiken voor app selecteren](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Klik op **OK**.

Uw **Mijn voorbeeld-WPF-app** is geregistreerd om het beveiligde **Mijn Node.js-web-API-voorbeeld** te kunnen aanroepen. Een gebruiker [voert een verificatie uit](../active-directory/develop/developer-glossary.md#authentication) met Azure AD B2C om de WPF-bureaubladtoepassing te kunnen gebruiken. De bureaubladtoepassing verkrijgt een [autorisatietoekenning](../active-directory/develop/developer-glossary.md#authorization-grant) van Azure AD B2C voor toegang tot de beveiligde web-API.

## <a name="update-web-api-code"></a>Web-API-code bijwerken

Nu de web-API is geregistreerd en u bereiken hebt gedefinieerd, moet u de web-API-code configureren om uw Azure AD B2C-tenant te gebruiken. In deze zelfstudie gaat u een Node.js web-app-voorbeeld configureren dat u kunt downloaden vanuit GitHub. 

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```
Het Node.js web-API-voorbeeld maakt gebruik van de bibliotheek Passport.js zodat Azure AD B2C aanroepen naar de API kan beveiligen. 

### <a name="configure-the-web-api"></a>De web-API configureren

1. Open het bestand `index.js` in het Node.js-web-API-voorbeeld.
2. Configureer het voorbeeld met de registratiegegevens voor Azure AD B2C-tenant. Wijzig de volgende regels code:

```nodejs
var tenantID = "<your-tenant-name>.onmicrosoft.com";
var clientID = "<Application ID for your Node.js Web API>";
var policyName = "B2C_1_SiUpIn";  // Sign-in / sign-up policy name
```

### <a name="configure-the-desktop-app"></a>De bureaublad-app configureren

1. Open de oplossing `active-directory-b2c-wpf` van de zelfstudie [Verificatie van gebruikers met Azure Active Directory B2C in een desktop-app](active-directory-b2c-tutorials-desktop-app.md) in Visual Studio.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Voer de Node.Js-web-API uit:

1. Start een opdrachtprompt voor Node.js.
2. Ga naar de map met het Node.js-voorbeeld. Bijvoorbeeld `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Voer de volgende opdrachten uit:
    ```
    npm install && npm update
    ```
    ```
    node index.js
    ```
Voer de bureaublad-app uit:

1. Druk op **F5** om de desktop-app uit te voeren.
2. Meld u aan met het e-mailadres en het wachtwoord dat wordt gebruikt in de zelfstudie [Verificatie van gebruikers met Azure Active Directory B2C in een desktop-app](active-directory-b2c-tutorials-desktop-app.md).
3. Klik op de knop **API aanroepen**. 

De desktop-app dient een aanvraag in bij de web-API en haalt van een antwoord op met de weergavenaam van de aangemelde gebruiker. Uw beveiligde web-app roept de beveiligde web-API in uw Azure AD B2C-tenant aan.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u stapsgewijs geleerd hoe u een ASP.NET-web-API kunt beveiligen door in Azure AD B2C bereiken te registreren en te definiëren. U vindt meer informatie door te bladeren door de beschikbare Azure AD B2C-codevoorbeelden.

> [!div class="nextstepaction"]
> [Azure AD B2C-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
