---
title: Gebruik Microsoft Authentication Library (MSAL) in nationale Clouds - identiteitsplatform van Microsoft
description: Microsoft Authentication Library (MSAL) kunnen ontwikkelaars van toepassingen kunnen verkrijgen van tokens om beveiligde web-API's aanroepen. Deze web-API's kan worden de Microsoft Graph, andere Microsoft-APIS, web-API's van derden of uw eigen web-API. MSAL biedt ondersteuning voor meerdere architecturen voor toepassingen en platforms.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18eccd6b6d29a43cc3fa13d133d449bf0b9be657
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075788"
---
# <a name="use-msal-in-national-cloud-environment"></a>Gebruik MSAL in nationale cloudomgeving

[Nationale clouds](authentication-national-cloud.md) zijn fysiek geïsoleerd exemplaren van Azure. Deze regio's van Azure zijn ontworpen om ervoor te zorgen dat gegevens gegevenslocatie, soevereiniteit, en vereisten binnen geografische grenzen worden herkend.

Naast de wereldwijde Microsoft-cloud kunnen Microsoft Authentication Library (MSAL) ook toepassingsontwikkelaars in nationale clouds te verkrijgen van tokens om te verifiëren en beveiligde web-API's aanroepen. Deze web-API's is de Microsoft Graph of andere Microsoft-APIS.

Globale cloud, met inbegrip van Azure Active Directory (Azure AD) is geïmplementeerd in de volgende nationale clouds:  

- Azure van de Amerikaanse overheid
- Azure China 21Vianet
- Azure Duitsland

Deze handleiding laat zien hoe u zich aanmelden werk en schoolaccounts, een toegangstoken ophalen en de Microsoft Graph-API aanroepen [Microsoft cloud voor de Amerikaanse overheid](https://azure.microsoft.com/global-infrastructure/government/) omgeving.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u voldoet aan deze vereisten voordat u begint.

### <a name="choose-the-appropriate-identities"></a>Kies de juiste identiteit

[Azure Government](https://docs.microsoft.com/azure/azure-government/) toepassingen kunnen Azure AD-Government-identiteiten, evenals openbare Azure AD-identiteiten gebruiken om gebruikers te verifiëren. Aangezien u een van deze identiteiten gebruiken kunt, moet u om te begrijpen en bepalen welke instantie van eindpunt u voor uw scenario kiezen moet:

- Openbare Azure AD: Doorgaans gebruikt als uw organisatie al een openbare Azure AD-tenant om ondersteuning voor Office 365 (openbaar of GCC) of een andere toepassing.
- Azure AD Government: Doorgaans gebruikt als uw organisatie al een Azure Government voor AD-tenant aan ondersteuning voor Office 365 (GCC hoog of DoD) of maakt een nieuwe tenant in Azure AD-Government.

Als u had besloten, is de speciale aandacht waar u uw app-registratie uitvoeren. Als u openbare Azure AD-identiteiten voor uw Azure Government-toepassing, moet u de toepassing registreren in uw openbare Azure AD-tenant.

### <a name="get-an-azure-government-subscription"></a>Neem een abonnement op Azure Government

- Als u een Azure Government-abonnement, raadpleegt u [beheren en verbinding maken met uw abonnement in Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions).
- Als u een Azure Government-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/global-infrastructure/government/request/) voordat u begint.

## <a name="javascript"></a>Javascript

### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren

1. Meld u aan bij de [Azure-portal](https://portal.azure.us/) om een toepassing te registreren.
    1. Als u Azure portal-eindpunten voor andere nationale clouds, bezoekt [eindpunten voor App-registratie](authentication-national-cloud.md#app-registration-endpoints)

1. Als uw account hebt u toegang tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en instellen van uw portal-sessie op de gewenste Azure AD-tenant.
1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://aka.ms/ra/ff) pagina.
1. Wanneer de pagina **Een toepassing registreren** wordt weergegeven, voert u een naam in voor de toepassing.
1. Onder **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map**.
1. Onder de **omleidings-URI** sectie, selecteer de **Web** platform en stel de waarde in de URL van de toepassing op basis van uw webserver. Zie de secties hieronder voor instructies over het instellen en ophalen van de omleidings-URL in Visual Studio en het knooppunt.
1. Selecteer **Registreren** wanneer u klaar bent.
1. Noteer de waarde **Toepassings-id (client)** op de app-pagina **Overzicht**.
1. In deze zelfstudie vereist de [impliciet verlenen stroom](v2-oauth2-implicit-grant-flow.md) zijn ingeschakeld. Selecteer in het deelvenster aan de linkerkant van de geregistreerde toepassing de optie **Verificatie**.
1. Schakel in **Geavanceerde instellingen**, onder **Impliciete toekenning**, de selectievakjes **Id-tokens** en **toegangstoken** in. Id-tokens en toegangstokens zijn vereist, omdat via de app gebruikers moeten worden aangemeld en een API moet worden aangeroepen.
1. Selecteer **Opslaan**.

### <a name="step-2--set-up-your-web-server-or-project"></a>Stap 2:  Instellen van uw webserver of project

- [Download de projectbestanden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) voor een lokale webserver, zoals knooppunt.

  of

- [Het Visual Studio-project downloaden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).

En ga vervolgens verder met de [configureren van uw JavaScript-SPA](#step-4-configure-your-javascript-spa) configureren in het codevoorbeeld voordat deze wordt uitgevoerd.

### <a name="step-3-use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Stap 3: Gebruik de Microsoft Authentication Library (MSAL) aan te melden bij de gebruiker

Volg de stappen in [Javascript zelfstudie](tutorial-v2-javascript-spa.md#create-your-project) aan uw project maken en integreren met Microsoft Authentication Library (MSAL) aan te melden bij de gebruiker.

### <a name="step-4-configure-your-javascript-spa"></a>Stap 4: Configureren van uw JavaScript-SPA

In de `index.html` bestand gemaakt tijdens het instellen van het project, de registratiegegevens van de toepassing toevoegen. Voeg de volgende code aan de bovenkant binnen de `<script></script>` tags in de hoofdtekst van uw `index.html` bestand:

```javascript
const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here",
        }
}

const graphConfig = {
        graphEndpoint: "https://graph.microsoft.us",
        graphScopes: ["user.read"],
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

Waar:

- `Enter_the_Application_Id_here`: is de **toepassings-id (client-id)** voor de toepassing die u hebt geregistreerd.
- `Enter_the_Tenant_Info_Here`: is ingesteld op een van de volgende opties:
    - Als uw toepassing ondersteunt **Accounts in deze organisatie-map**, vervang deze waarde met de **Tenant-ID** of **tenantnaam** (bijvoorbeeld: Contoso.Microsoft.com)
    - Als uw toepassing **Accounts in elke organisatiemap** ondersteunt, vervang deze waarde dan door `organizations`
    -  Verificatie-eindpunten voor de nationale clouds, Zie [eindpunten van Azure AD-verificatie](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints)

     > [!NOTE]
     > Scenario's voor persoonlijke Microsoft-accounts (MSA) worden niet ondersteund in nationale clouds.
  
-   `graphEndpoint` -is de Microsoft Graph-eindpunt voor Microsoft-cloud voor de Amerikaanse overheid.
    -  Microsoft Graph-eindpunten voor de nationale clouds, Zie [Microsoft Graph-eindpunten in nationale cloud](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints)

## <a name="net"></a>.NET

MSAL .NET kunt u het aanmelden van gebruikers, token ophalen en aanroepen van Microsoft Graph API in nationale Clouds.

De volgende zelfstudie wordt gedemonstreerd hoe u een .NET Core 2.2 MVC-Web-app die gebruikmaakt van OpenID Connect gebruikers zich aanmelden met hun accounts 'werk en school' in hun organisatie die behoren tot nationale clouds.

- Meld u aan gebruikers en token ophalen, gaat u als volgt [in deze zelfstudie](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform).
- Voor het aanroepen van Microsoft Graph API, gaat u als volgt [in deze zelfstudie](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Duitsland](https://docs.microsoft.com/azure/germany/)