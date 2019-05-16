---
title: Meer informatie over het integreren met Azure AD B2C met behulp van Microsoft Authentication Library (MSAL)
description: Microsoft Authentication Library (MSAL) kunnen ontwikkelaars van toepassingen te integreren met Azure AD B2C beveiligde Web-API's aanroepen hebt aanschaft, tokens. Deze Web-API's kunnen worden de Microsoft Graph, andere Microsoft-APIS, Web-API's van derden of uw eigen Web-API.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6af78a593fb8fadb836d2dc4c02115d95a7f2712
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65546038"
---
# <a name="integrate-microsoft-authentication-library-msal-with-azure-active-directory-b2c"></a>Microsoft Authentication Library (MSAL) integreren met Azure Active Directory B2C

Microsoft Authentication Library (MSAL) kunnen toepassingsontwikkelaars voor verificatie van gebruikers met sociale en lokale identiteiten met behulp van [Azure Active Directory (Azure AD) B2C](https://docs.microsoft.com/azure/active-directory-b2c/). Azure Active Directory (Azure AD) B2C is een identiteitsbeheerservice waarmee u kunt aanpassen en controleren hoe uw klanten zich registreren en aanmelden en hoe zijn hun profielen beheren wanneer ze uw toepassingen gebruiken.

Azure Active Directory (Azure AD) B2C kunt u huisstijl en aanpassen van de gebruikersinterface (UI) van uw toepassingen voor een naadloze ervaring voor uw klant.

In deze zelfstudie laat zien hoe Microsoft Authentication Library (MSAL) gebruiken om te integreren met Azure Active Directory (Azure AD) B2C.


## <a name="prerequisites"></a>Vereisten

Als u dit nog niet hebt gemaakt uw eigen [Azure AD B2C-Tenant](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), Maak nu een. U kunt een bestaande Azure AD B2C-tenant. 

## <a name="javascript"></a>Javascript

Stappen te volgen, laat zien hoe een toepassing met één pagina kunt gebruiken van Azure AD B2C voor gebruikersregistratie, aanmelding en een beveiligde web-API aanroepen.

### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren

Voor het implementeren van verificatie, moet u eerst uw toepassing registreren. Voor het registreren van uw app, gaat u als volgt [uw toepassing registreren](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c) voor gedetailleerde stappen.

### <a name="steps-2-download-applications"></a>Stap 2: Toepassingen downloaden

Download een zip-bestand of het voorbeeld uit GitHub te klonen.
>GIT-kloon https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git

### <a name="steps-3-authentication"></a>Stap 3: Verificatie

1. Open het index.html-bestand in het voorbeeld.

2. Configureren van het voorbeeld met de toepassings-ID en de sleutel die u eerder hebt vastgelegd tijdens het registreren van uw toepassing. Wijzig de volgende regels met code door de waarden te vervangen door de namen van uw map en API's:

```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>",
        b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/hello/demo.read"],
        webApi: 'http://localhost:5000/hello',
  };

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

De naam van de [gebruikersstroom](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) B2C_1_signupsignin1 gebruikt in deze zelfstudie is. Als u een andere gebruikersnaam voor de stroom, gebruikt u uw gebruikersnaam van de stroom in de waarde van de instantie.


### <a name="configure-application-to-use-b2clogincom"></a>Toepassing configureren voor gebruik `b2clogin.com`

U kunt `b2clogin.com` in plaats van `login.microsoftonline.com` als een omleidings-url, bij het instellen van een id-provider voor registratie en aanmelding in uw Azure Active Directory (Azure AD) B2C-toepassing.

**`b2clogin.com`** i.e 
`https://your-tenant-name.b2clogin.com/your-tenant-guid` wordt gebruikt voor het volgende:

- Ruimte in de kop van de cookie wordt gebruikt door Microsoft-services wordt verminderd.
- De URL's zijn niet langer een verwijzing naar Microsoft. Bijvoorbeeld: uw Azure AD B2C-toepassing waarschijnlijk verwijst naar login.microsoftonline.com


 Voor het gebruik van 'b2clogin.com', moet u de configuratie van uw toepassing bijwerken.  

1. Update ValidateAuthority: Stel de **validateAuthority** eigenschap `false`. Wanneer **validateAuthority** is ingesteld op false, omleidingen mogen b2clogin.com.

Het volgende voorbeeld ziet u hoe u de eigenschap kunt instellen:
```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
        validateAuthority: false;

};
// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

> [!NOTE]
> Uw Azure AD B2C-toepassing is waarschijnlijk heeft betrekking op login.microsoftonline.com op verschillende plaatsen, zoals uw gebruiker stroom-verwijzingen en token-eindpunten. Zorg ervoor dat de autorisatie-eindpunt, token-eindpunt en uitgever voor het gebruik van uw tenant-name.b2clogin.com zijn bijgewerkt.

Volg deze [MSAL JS voorbeeld](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) over het gebruik van de Preview van Microsoft Authentication Library voor JavaScript (msal.js) voor een toegangstoken en een API die wordt beveiligd door Azure AD B2C aanroepen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over:

- [Aangepast beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Aanpassingen van de gebruikersinterface](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)