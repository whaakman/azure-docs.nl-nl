---
title: Gebruik Microsoft Authentication Library (MSAL) in nationale clouds - identiteitsplatform van Microsoft
description: Microsoft Authentication Library (MSAL) kunnen ontwikkelaars van toepassingen kunnen verkrijgen van tokens om beveiligde web-API's aanroepen. Deze web-API's kan worden Microsoft Graph, andere Microsoft-APIs, partner web-API's of uw eigen web-API. MSAL biedt ondersteuning voor meerdere architecturen voor toepassingen en platforms.
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
ms.openlocfilehash: f9958356cae3c486ecf68e280f33d63c6a537b14
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235270"
---
# <a name="use-msal-in-a-national-cloud-environment"></a>MSAL gebruiken in een nationale cloudomgeving

[Nationale clouds](authentication-national-cloud.md) zijn fysiek geïsoleerd exemplaren van Azure. Deze regio's van Azure ervoor te zorgen dat gegevens gegevenslocatie, soevereiniteit, en vereisten binnen geografische grenzen worden herkend.

Microsoft Authentication Library (MSAL) kunnen naast de wereldwijde Microsoft-cloud toepassingsontwikkelaars in nationale clouds te verkrijgen van tokens om te verifiëren en beveiligde web-API's aanroepen. Deze web-API's is Microsoft Graph of andere Microsoft-APIs.

Azure Active Directory (Azure AD) is geïmplementeerd met inbegrip van de globale cloud in de volgende nationale clouds:  

- Azure Government
- Azure China 21Vianet
- Azure Duitsland

Deze handleiding laat zien hoe u zich aanmeldt bij het werk en schoolaccounts, een toegangstoken ophalen en de Microsoft Graph-API aanroepen de [Azure Government-cloud](https://azure.microsoft.com/global-infrastructure/government/) omgeving.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, controleert u dat u voldoet aan deze vereisten.

### <a name="choose-the-appropriate-identities"></a>Kies de juiste identiteit

[Azure Government](https://docs.microsoft.com/azure/azure-government/) toepassingen kunnen Azure AD-Government-identiteiten en openbare Azure AD-identiteiten gebruiken om gebruikers te verifiëren. Omdat u een van deze identiteiten te gebruiken kunt, moet u besluiten welke autoriteitseindpunt u voor uw scenario kiezen moet:

- Openbare Azure AD: Doorgaans gebruikt als uw organisatie al een openbare Azure AD-tenant om ondersteuning voor Office 365 (openbaar of GCC) of een andere toepassing.
- Azure AD Government: Doorgaans gebruikt als uw organisatie al een Azure Government voor AD-aan ondersteuning voor Office 365 tenant (GCC hoog of DoD) of het maken van een nieuwe tenant in Azure AD-Government.

Nadat u hebt besloten, is een speciale aandacht waar u uw app-registratie uitvoeren. Als u openbare Azure AD-identiteiten voor uw Azure Government-toepassing, moet u de toepassing registreren in uw openbare Azure AD-tenant.

### <a name="get-an-azure-government-subscription"></a>Neem een abonnement op Azure Government

Als u een Azure Government-abonnement, raadpleegt u [beheren en verbinding maken met uw abonnement in Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions).

Als u een Azure Government-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/global-infrastructure/government/request/) voordat u begint.

## <a name="javascript"></a>Javascript

### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren

1. Meld u aan bij [Azure Portal](https://portal.azure.us/).
    
   Als u Azure portal-eindpunten voor andere nationale clouds, bezoekt [eindpunten voor App-registratie](authentication-national-cloud.md#app-registration-endpoints).

1. Als uw account hebt u toegang tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en instellen van uw portal-sessie op de gewenste Azure AD-tenant.
1. Ga naar de [App-registraties](https://aka.ms/ra/ff) pagina op de Microsoft identity-platform voor ontwikkelaars.
1. Wanneer de pagina **Een toepassing registreren** wordt weergegeven, voert u een naam in voor de toepassing.
1. Onder **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map**.
1. In de **omleidings-URI** sectie, selecteer de **Web** platform en stel de waarde in de URL van de toepassing op basis van uw webserver. Zie de volgende secties voor meer informatie over het instellen en ophalen van de omleidings-URL in Visual Studio en het knooppunt.
1. Selecteer **Registreren**.
1. Noteer de waarde **Toepassings-id (client)** op de app-pagina **Overzicht**.
1. Deze zelfstudie moet u om in te schakelen de [impliciete stroom](v2-oauth2-implicit-grant-flow.md). Selecteer in het linkerdeelvenster van de geregistreerde toepassing **verificatie**.
1. In **geavanceerde instellingen**onder **impliciete**, selecteer de **ID-tokens** en **toegangstokens** selectievakjes. ID-tokens en toegangstokens zijn vereist, omdat deze app moet het aanmelden van gebruikers en aanroepen van een API.
1. Selecteer **Opslaan**.

### <a name="step-2--set-up-your-web-server-or-project"></a>Stap 2:  Instellen van uw webserver of project

- [Download de projectbestanden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) voor een lokale webserver, zoals knooppunt.

  of

- [Het Visual Studio-project downloaden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).

Ga vervolgens verder met [configureren van uw JavaScript-SPA](#step-4-configure-your-javascript-spa) het codevoorbeeld configureren voordat u deze uitvoert.

### <a name="step-3-use-the-microsoft-authentication-library-to-sign-in-the-user"></a>Stap 3: Gebruik de Microsoft Authentication Library aan te melden bij de gebruiker

Volg de stappen in de [JavaScript zelfstudie](tutorial-v2-javascript-spa.md#create-your-project) om uw project maken en integreren met MSAL aan te melden bij de gebruiker.

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

In deze code:

- `Enter_the_Application_Id_here` is de **(client) toepassings-ID** waarde voor de toepassing die u hebt geregistreerd.
- `Enter_the_Tenant_Info_Here` is ingesteld op een van de volgende opties:
    - Als uw toepassing ondersteunt **Accounts in deze organisatie-map**, vervang deze waarde door de tenant-ID of tenant-naam (bijvoorbeeld contoso.microsoft.com).
    - Als uw toepassing ondersteunt **Accounts in een organisatie-map**, vervang deze waarde met `organizations`.
    
    Verificatie-eindpunten voor de nationale clouds, Zie [eindpunten van Azure AD-verificatie](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints).

    > [!NOTE]
    > Persoonlijke Microsoft-accounts worden niet ondersteund in nationale clouds.
  
- `graphEndpoint` is de Microsoft Graph-eindpunt voor de Microsoft-cloud voor de Amerikaanse overheid.

   Microsoft Graph-eindpunten voor de nationale clouds, Zie [Microsoft Graph-eindpunten in nationale clouds](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints).

## <a name="net"></a>.NET

U kunt MSAL.NET het aanmelden van gebruikers, tokens verkrijgen en aanroepen van de Microsoft Graph API in nationale clouds.

De volgende zelfstudies tonen hoe u een .NET Core 2.2 MVC-Web-app. De app maakt gebruik van de OpenID Connect gebruikers zich aanmelden met een werk- en schoolaccounts-account in een organisatie die deel uitmaakt van een nationale cloud.

- Meld u aan gebruikers en tokens verkrijgen, gaat u als volgt [in deze zelfstudie](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform).
- Voor het aanroepen van de Microsoft Graph API, gaat u als volgt [in deze zelfstudie](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Duitsland](https://docs.microsoft.com/azure/germany/)