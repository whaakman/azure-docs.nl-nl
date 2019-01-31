---
title: 'Snelstartgids: aanmelden instellen voor een app met één pagina met Azure Active Directory B2C | Microsoft Docs'
description: Lees hoe u een voorbeeld van een toepassing met één pagina uitvoert die gebruikmaakt van Azure Active Directory B2C voor aanmelden bij een account.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 10/24/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: c841497fe0a072497b622876b9b0205097ccb25e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191700"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Quickstart: aanmelden instellen voor een app met één pagina met Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C bevat functionaliteit voor identiteitsbeheer in de cloud ter bescherming van uw toepassing, bedrijf en klanten. Met Azure AD B2C zijn uw toepassingen in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociaalnetwerk- en Enterprise-accounts. In deze snelstart gebruikt u een toepassing met één pagina. Deze toepassing wordt gebruikt voor het aanmelden via een id-provider voor sociale netwerken en voor het aanroepen van een met Azure AD B2C beveiligde web-API.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

- [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload**.
- [Node.js](https://nodejs.org/en/download/) installeren
- Een sociaalnetwerkaccount van Facebook, Google, Microsoft of Twitter.
- [Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
    ```

## <a name="run-the-application"></a>De toepassing uitvoeren

1. Start de server door de volgende opdrachten uit te voeren vanaf de Node.js-opdrachtprompt: 

    ```
    cd active-directory-b2c-javascript-msal-singlepageapp
    npm install && npm update
    node server.js
    ```

    Server.js levert het poortnummer waarnaar de server op localhost luistert.

    ```
    Listening on port 6420...
    ```

2. Blader naar de URL van de toepassing. Bijvoorbeeld `http://localhost:6420`.

## <a name="sign-in-using-your-account"></a>Aanmelden met uw account

1. Klik op **Aanmelden** om de werkstroom te starten.

    ![Voorbeeld-app in de browser](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

    In de voorbeeldtoepassing worden verschillende registratiemogelijkheden ondersteund. U kunt bijvoorbeeld een id-provider voor sociale netwerken gebruiken of een lokaal account maken met behulp van een e-mailadres. Voor deze snelstart gebruikt u een account van een id-provider voor sociale netwerken (Facebook, Google, Microsoft of Twitter). 

2. In Azure AD B2C wordt een aangepaste aanmeldingspagina voor het fictieve merk Wingtip Toys weergegeven voor de voorbeeldweb-app. Klik op de knop van de id-provider voor sociale netwerken die u wilt gebruiken om u aan te melden via een id-provider voor sociale netwerken.

    ![Provider voor aanmelden of registreren](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

    U moet zich verifiëren (aanmelden) met behulp van de referenties van uw sociaalnetwerkaccount en de toepassing autoriseren om informatie uit uw sociaalnetwerkaccount te lezen. Door toegang te verlenen, kan de toepassing profielgegevens van het sociaalnetwerkaccount ophalen, zoals uw naam en plaats. 

3. Voltooi het aanmeldingsproces voor de id-provider.

## <a name="access-a-protected-api-resource"></a>Toegang tot een beveiligde API-resource

Klik op **Web-API aanroepen** om de weergavenaam van de web-API-aanroep als JSON-object te retourneren. 

![Web-API-reactie](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

De voorbeeldtoepassing met één pagina bevat het toegangstoken van Azure AD in de aanvraag voor de beveiligde web-API-resource.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere snelstarts of zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een voorbeeldtoepassing met één pagina gebruikt. Deze app wordt gebruikt voor het aanmelden via een aangepaste aanmeldingspagina, het aanmelden via een id-provider voor sociale netwerken, het maken van een Azure AD B2C-account en het aanroepen van een met Azure AD B2C beveiligde web-API. 

Aan de slag met het maken van uw eigen Azure AD B2C-tenant.

> [!div class="nextstepaction"]
> [Een Azure Active Directory B2C-tenant maken in Azure Portal](tutorial-create-tenant.md)
