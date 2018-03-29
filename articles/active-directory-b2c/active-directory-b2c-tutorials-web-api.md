---
title: Zelfstudie Azure Active Directory B2C gebruiken voor het beveiligen van een ASP.NET-web-API
description: Zelfstudie over het gebruik van Active Directory B2C om een ASP.NET web-api te beveiligen en aan te roepen vanuit een ASP.NET-web-app.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.author: davidmu
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: f4e1c18f151a9c815258f01ea198d3d173d0b44e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-use-azure-active-directory-b2c-to-protect-an-aspnet-web-api"></a>Zelfstudie Azure Active Directory B2C gebruiken voor het beveiligen van een ASP.NET-web-API

Deze zelfstudie laat zien hoe u een web-API-resource, die wordt beveiligd door een web-Azure Active Directory (Azure AD) B2C, aanroept vanuit een ASP.NET-web-app.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een web-API registreren in uw Azure AD B2C-tenant
> * Bereiken voor een web-API definiëren en configureren
> * App-machtigingen verlenen aan de web-API
> * Voorbeeldcode bijwerken zodat Azure AD B2C wordt gebruikt om een web-API te beveiligen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Voltooi de zelfstudie [Azure Active Directory B2C gebruiken voor gebruikersverificatie in een ASP.NET-web-app](active-directory-b2c-tutorials-web-app.md).
* U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload** installeren.

## <a name="register-web-api"></a>Web-API registreren

Web-API-resources moeten worden geregistreerd in uw tenant voordat deze in staat zijn om [beveiligde resourceaanvragen](../active-directory/develop/active-directory-dev-glossary.md#resource-server) door [clienttoepassingen](../active-directory/develop/active-directory-dev-glossary.md#client-application) die een [toegangstoken](../active-directory/develop/active-directory-dev-glossary.md#access-token) van Azure Active Directory aanbieden, kunnen accepteren en erop kunnen reageren. Tijdens de registratie wordt de [toepassing en het service-principal-object](../active-directory/develop/active-directory-dev-glossary.md#application-object) in uw tenant opgezet. 

Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecteer **Azure AD B2C** in de lijst met services in Azure Portal.

2. Klik in de B2C-instellingen op **Toepassingen** en klik vervolgens op **+Toevoegen**.

    Gebruik de volgende instellingen voor het registreren van de voorbeeld-web-API in uw tenant.
    
    ![Een nieuwe API toevoegen](media/active-directory-b2c-tutorials-web-api/web-api-registration.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Naam** | Mijn voorbeeld-web-API | Voer een **naam** in die uw web-API aan ontwikkelaars beschrijft. |
    | **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een web-API. |
    | **Impliciete stroom toestaan** | Ja | Selecteer **Ja** omdat de API gebruikmaakt van [aanmelding via OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Antwoord-URL** | `https://localhost:44332` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die worden aangevraagd door uw API. In deze zelfstudie wordt de voorbeeld-web-API lokaal (localhost) uitgevoerd, en wordt op poort 44332 geluisterd. |
    | **App-id-URI** | myAPISample | De URI is een unieke identificatie van de API in de tenant. Hierdoor kunt u meerdere API's per tenant registreren. [Bereiken](../active-directory/develop/active-directory-dev-glossary.md#scopes) bepalen de toegang tot de beveiligde API-resource en worden per app-id-URI gedefinieerd. |
    | **Systeemeigen client** | Nee | Aangezien dit een web-API en geen systeemeigen client is, moet u Nee selecteren. |
    
3. Klik op **Maken** om uw API te registreren.

Geregistreerde API's worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-API in de lijst. Het eigenschappenvenster van de web-API wordt weergegeven.

![Eigenschappen van de web-API](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

Noteer de **client-id voor de toepassing**. Met de id wordt de API uniek aangeduid. Deze hebt u nodig als u later in deze zelfstudie de API gaat configureren.

Door de web-API bij Azure AD B2C te registeren, wordt een vertrouwensrelatie gedefinieerd. Omdat de API bij B2C is geregistreerd, kan de API nu de B2C-toegangstokens vertrouwen die van andere toepassingen worden ontvangen.

## <a name="define-and-configure-scopes"></a>Bereiken definiëren en configureren

[Bereiken](../active-directory/develop/active-directory-dev-glossary.md#scopes) bieden een manier om toegang tot beveiligde resources te reguleren. Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Sommige gebruikers kunnen bijvoorbeeld zowel lees- als schrijftoegang hebben, terwijl andere gebruikers mogelijk alleen-lezen-machtigingen hebben. In deze zelfstudie gaat u lees- en schrijfmachtigingen definiëren voor de web-API.

### <a name="define-scopes-for-the-web-api"></a>Bereiken definiëren voor de web-API

Geregistreerde API's worden weergegeven in de lijst met toepassingen voor de Azure AD B2C-tenant. Selecteer de web-API in de lijst. Het eigenschappenvenster van de web-API wordt weergegeven.

Klik op **Gepubliceerde bereiken (preview-versie)**.

Voeg de volgende gegevens toe om bereiken voor de API te configureren. 

![bereiken die zijn gedefinieerd in een web-api](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Instelling      | Voorgestelde waarde  | Beschrijving                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Bereik** | Hello.Read | Leestoegang tot Hallo |
| **Bereik** | Hello.Write | Schrijftoegang tot Hallo |

De gepubliceerde bereiken kunnen worden gebruikt om een client-appmachtiging te verlenen aan de web-API.

### <a name="grant-app-permissions-to-web-api"></a>App-machtigingen verlenen aan een web-API

Als u een beveiligde web-API wilt aanroepen vanuit een app, moet u uw app machtigingen verlenen tot de API. 

1. Selecteer in Azure Portal **Azure AD B2C** in de lijst met services, en klik op **Toepassingen** om de lijst met geregistreerde apps weer te geven.

2. Selecteer **Mijn voorbeeld-web-app** in de lijst met apps, klik op **API-toegang (preview-versie)** en vervolgens op **Toevoegen**.

3. In de vervolgkeuzelijst **API selecteren** selecteert u uw geregistreerde web-API: **Mijn voorbeeld-web-app**.

4. In de vervolgkeuzelijst **Bereiken selecteren** selecteert u de bereiken die u hebt gedefinieerd toen u de web-API registreerde.

    ![bereiken voor app selecteren](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Klik op **OK**.

Uw **Mijn voorbeeld-web-app** is geregistreerd om de beveiligde **Mijn voorbeeld-web-API** te kunnen aanroepen. Een gebruiker [voert een verificatie uit](../active-directory/develop/active-directory-dev-glossary.md#authentication) bij Azure AD B2C om de web-app te kunnen gebruiken. De web-app verkrijgt een [authorisatietoekenning](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) van Azure AD B2C voor toegang tot de beveiligde web-API.

## <a name="update-web-api-code"></a>Web-API-code bijwerken

Nu de web-API is geregistreerd en u bereiken hebt gedefinieerd, moet u de web-API-code configureren om uw Azure AD B2C-tenant te gebruiken. In deze zelfstudie gaat u een voorbeeld-web-API configureren. 

De voorbeeld-web-API is opgenomen in het project dat u hebt gedownload in de zelfstudie die voorafgaande aan deze moet worden voltooid: [de zelfstudie Azure Active Directory B2C gebruiken voor gebruikersverificatie in een ASP.NET-web-app](active-directory-b2c-tutorials-web-app.md). Als u die vereiste zelfstudie nog niet hebt voltooid, moet u deze voltooien voordat u verdergaat.

Er bevinden zich twee projecten in de voorbeeldoplossing:

**Voorbeeld-web-app (TaskWebApp):** een web-app om een takenlijst te maken of te bewerken. De web-app gebruikt het **registratie- of aanmeldingsbeleid** om gebruikers met een e-mailadres te registreren of aan te melden.

**Voorbeeld-web-API-app (TaskService):** een web-API die ondersteuning biedt voor het maken, lezen, bijwerken en verwijderen van takenlijstfunctionaliteit. De web-API wordt beveiligd door Azure AD B2C en aangeroepen door de web-app.

De voorbeeld-web-app en -web-API definiëren de configuratiewaarden als app-instellingen in het bestand Web.config van elk project.

Open de oplossing **B2C-WebAPI-DotNet** in Visual Studio.

### <a name="configure-the-web-app"></a>De web-app configureren

1. Open **Web.config** in het project **TaskWebApp**.

2. Als u de API lokaal wilt uitvoeren, moet u de localhost-instelling gebruiken voor **api:TaskServiceUrl**. Ga als volgt te werk om Web.config te wijzigen: 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. Configureer de URI van de API. Dit is de URI die de web-app gebruikt om de API-aanvraag te maken. Configureer ook de machtigingen die zijn aangevraagd.

```C#
<add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/myAPISample/" />
<add key="api:ReadScope" value="Hello.Read" />
<add key="api:WriteScope" value="Hello.Write" />
```

### <a name="configure-the-web-api"></a>De web-API configureren

1. Open **Web.config** in het project **TaskService**.

2. Configureer de API om uw tenant te gebruiken.

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. Stel de client-id in op de geregistreerde toepassings-id voor uw API.

    ```C#
    <add key="ida:ClientId" value="<The Application ID for your web API obtained from the Azure portal>"/>
    ```

4. Werk de beleidsinstellingen bij met de naam die is gegenereerd toen u het registratie- en aanmeldbeleid maakte.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="b2c_1_SiUpIn" />
    ```

5. Configureer de bereikinstellingen zodat deze overeenkomen met wat u in de portal hebt gemaakt.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample-web-app-and-web-api"></a>De voorbeeld-web-app en -web-API uitvoeren

U moet zowel project **TaskWebApp** als **TaskService** uitvoeren. 

1. Klik in Solution Explorer met de rechtermuisknop op de oplossing en selecteer vervolgens **Opstartprojecten instellen...**. 
2. Selecteer het keuzerondje **Meerdere opstartprojecten**.
3. Wijzig de **actie** voor beide projecten in **Start**.
4. Klik op OK om de configuratie op te slaan.
5. Druk op **F5** om beide toepassingen uit te voeren. Elke toepassing wordt geopend in zijn eigen browsertabblad. `https://localhost:44316/` is de web-app.
    `https://localhost:44332/` is de web-API.

6. Klik in de web-app op de registratie-/aanmeldkoppeling in de banner met het menu, om u te registeren voor de webtoepassing. Gebruik het account dat u hebt gemaakt in de [zelfstudie over de web-app](active-directory-b2c-tutorials-web-app.md). 
7. Nadat u bent aangemeld, klikt u op de koppeling **Takenlijst** en maakt u een takenlijstitem.

Als u een takenlijstitem maakt, verzendt de web-app een aanvraag naar de web-API om het takenlijstitem te maken. Uw beveiligde web-app roept de beveiligde web-API in uw Azure AD B2C-tenant aan.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u stapsgewijs geleerd hoe u een ASP.NET-web-API kunt beveiligen door in Azure AD B2C bereiken te registreren en te definiëren. Meer informatie over het ontwikkelen van dit scenario, met inbegrip van een stapsgewijze uitleg van de code, vindt u in de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Een ASP.NET-web-app maken met Azure Active Directory B2C met mogelijkheden voor registreren, aanmelden, profiel bewerken en wachtwoord opnieuw instellen](active-directory-b2c-devquickstarts-web-dotnet-susi.md)