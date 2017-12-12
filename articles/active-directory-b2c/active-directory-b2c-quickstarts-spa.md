---
title: Test station een single-page application ' Azure AD B2C | Microsoft Docs
description: Test station aanmelden, registreren, profiel te bewerken en opnieuw ingesteld wachtwoord gebruiker trajecten met behulp van een testomgeving met Azure AD B2C
services: active-directory-b2c
documentationcenter: 
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 5a8a46af-28bb-4b70-a7f0-01a5240d0255
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: ba8ee4657309ab2a541f4c7b3fd4879542eee63c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="test-drive-a-single-page-application-configured-with-azure-ad-b2c"></a>Een single-page application ' geconfigureerd met Azure AD B2C testen

## <a name="about-this-sample"></a>Over dit voorbeeld

Azure Active Directory B2C biedt cloud identity management te houden van uw toepassing, zakelijke en klanten die zijn beveiligd.  Deze snelstartgids maakt gebruik van een voorbeeldtoepassing met één pagina voor het demonstreren van:

* Met behulp van de **registreren of aanmelden** beleid maken of meld u aan met een sociale id-provider of een lokale account via een e-mailadres. 
* **Een API aanroepen** uw weergegeven naam ophalen uit een Azure AD B2C beveiligde resource.

## <a name="prerequisites"></a>Vereisten

* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - **ASP.NET- en web-ontwikkeling**

* [Node.js](https://nodejs.org/en/download/) installeren

* Een sociaal-account van Facebook, Google, Microsoft- of Twitter. Als u geen een sociaal-account hebt, moet een geldig e-mailadres is vereist.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

[Downloaden of te klonen van de voorbeeldtoepassing](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) vanuit GitHub.

## <a name="run-the-sample-application"></a>De voorbeeldtoepassing uitvoeren

Dit voorbeeld uitvoeren vanaf de Node.js-opdrachtprompt: 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

Het consolevenster ziet u het poortnummer voor de webtoepassing op uw computer uitgevoerd.

```
Listening on port 6420...
```

Open `http://localhost:6420` in een webbrowser toegang krijgen tot de webtoepassing.


![Voorbeeld-app in de browser](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a>Een account maken

Klik op de **aanmelding** om te starten van de Azure AD B2C **registreren of aanmelden** werkstroom. Wanneer u een account maakt, kunt u een bestaande sociale identiteit provider-account of een e-mailaccount.

### <a name="sign-up-using-a-social-identity-provider"></a>Aanmelden met een identiteitsprovider van sociale

Klik op de knop van de id-provider die u wilt gebruiken voor het aanmelden met een identiteitsprovider van sociale. Als u liever een e-mailadres, Ga naar de [aanmelden met een e-mailadres](#sign-up-using-an-email-address) sectie.

![Provider aanmelden of registreren](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

U moet verifiëren met uw account sociale referenties en autoriseren van de toepassing om informatie te lezen uit je account sociale (sign-in). Door toegang te verlenen, kan de toepassing profielgegevens ophalen van de sociale account zoals uw naam en plaats. 

![Verifiëren en autoriseren met een sociaal-account](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

De details van uw nieuwe profiel zijn al ingevuld met informatie uit uw sociale-account. 

![Nieuwe registratie profiel accountgegevens](media/active-directory-b2c-quickstarts-spa/new-account-sign-up-profile-details-spa.png)

De weergavenaam, functietitel en plaats velden bijwerken en klik op **doorgaan**.  De waarden die u invoert worden gebruikt voor het hulpprogramma voor het account van uw Azure AD B2C-gebruikersprofiel.

U hebt een nieuwe Azure AD B2C-gebruikersaccount die gebruikmaakt van een id-provider gemaakt. 

Volgende stap: [aanroepen van een resource](#call-a-resource) sectie.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

Als u een sociale account niet gebruikt kiest voor verificatie, kunt u een Azure AD B2C gebruikersaccount in met een geldig e-mailadres. Een lokale gebruikersaccount van Azure AD B2C gebruikt Azure Active Directory als de id-provider. Met uw e-mailadres, klikt u op de **geen account? Nu aanmelden** koppeling.

![Aanmelden of registreren via e-mail](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-email-spa.png)

Voer een geldig e-mailadres in en klikt u op **sturen verificatiecode**. Een geldig e-mailadres is vereist voor de verificatiecode ontvangen van Azure AD B2C. 

Voer de verificatiecode die u via e-mail ontvangt en klik op **code controleren**.

Je profielgegevens toevoegen en klik op **maken**.

![Aanmelden bij de nieuwe account via e-mail](media/active-directory-b2c-quickstarts-spa/sign-up-new-account-profile-email-web.png)

Een nieuwe Azure AD B2C lokaal gebruikersaccount hebt gemaakt.

## <a name="call-a-resource"></a>Aanroepen van een resource

Nadat u bent aangemeld, kunt u de **Web-API aanroepen** knop om de weergavenaam van de Web-API-aanroep als een JSON-object geretourneerd. 

![Web-API-reactie](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

## <a name="next-steps"></a>Volgende stappen

De volgende stap is uw eigen Azure AD B2C-tenant maken en configureren van het voorbeeld wilt uitvoeren met behulp van uw tenant. 

> [!div class="nextstepaction"]
> [Een Azure Active Directory B2C-tenant maken in de Azure portal](active-directory-b2c-get-started.md)