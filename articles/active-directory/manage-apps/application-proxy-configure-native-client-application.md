---
title: Systeemeigen client-apps - Azure AD publiceren | Microsoft Docs
description: Bevat informatie over het inschakelen van systeemeigen client-apps om te communiceren met Azure AD Application Proxy Connector voor veilige externe toegang tot uw on-premises toepassingen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 85a7b133655a3b1e4ca60c28e695e3057b293fdc
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504512"
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Systeemeigen client-apps om te communiceren met de proxy-toepassingen inschakelen

Naast web-apps, worden Azure Active Directory Application Proxy ook gebruikt voor het publiceren van systeemeigen client-apps die zijn geconfigureerd met de Azure AD Authentication Library (ADAL). Systeemeigen client-apps verschillen van web-apps omdat ze zijn geïnstalleerd op een apparaat, terwijl de web-apps worden geopend via een browser. 

Application Proxy biedt ondersteuning voor systeemeigen client-apps door waarin Azure AD dat is uitgegeven tokens die zijn verzonden in de koptekst. De Application Proxy-service uitvoert verificatie namens de gebruikers. Deze oplossing gebruikt geen toepassing tokens voor verificatie. 

![Relatie tussen de eindgebruikers, Azure Active Directory en gepubliceerde toepassingen](./media/application-proxy-configure-native-client-application/richclientflow.png)

Gebruik Azure AD Authentication Library, die zorgt voor verificatie en biedt ondersteuning voor omgevingen met veel client, systeemeigen toepassingen te publiceren. Toepassingsproxy past de [systeemeigen toepassing voor de Web-API-scenario](../develop/authentication-scenarios.md#native-application-to-web-api). 

In dit artikel begeleidt u bij de vier stappen voor het publiceren van een systeemeigen toepassing met Application Proxy en de Azure AD-Verificatiebibliotheek. 

## <a name="step-1-publish-your-application"></a>Stap 1: Uw toepassing publiceren
Uw proxytoepassing te publiceren, net als elke andere toepassing en gebruikers toegang tot uw toepassing toe te wijzen. Zie voor meer informatie, [publiceren van toepassingen met toepassingsproxy](application-proxy-publish-azure-portal.md).

## <a name="step-2-configure-your-application"></a>Stap 2: Uw toepassing configureren
Configureer uw systeemeigen toepassing als volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **App-registraties**.
3. Selecteer **Nieuwe toepassing registreren**.
4. Geef een naam voor uw toepassing, selecteert **systeemeigen** als het toepassingstype en de omleidings-URI voor uw toepassing. 

   ![Maak een nieuwe app-registratie](./media/application-proxy-configure-native-client-application/create.png)
5. Selecteer **Maken**.

Zie voor meer informatie over het maken van een nieuwe app-registratie, [toepassingen integreren met Azure Active Directory](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).


## <a name="step-3-grant-access-to-other-applications"></a>Stap 3: Verlenen toegang tot andere toepassingen
De systeemeigen toepassing worden blootgesteld aan andere toepassingen in uw directory inschakelen:

1. Klik in **App-registraties**, selecteer de nieuwe systeemeigen toepassing die u zojuist hebt gemaakt.
2. Selecteer **vereiste machtigingen**.
3. Selecteer **Toevoegen**.
4. Open de eerste stap **Select an API**.
5. Gebruik de zoekbalk om de Application Proxy-app die u hebt gepubliceerd in de eerste sectie te vinden. Kies de app en klik vervolgens op **Selecteer**. 

   ![Zoek naar de app proxy](./media/application-proxy-configure-native-client-application/select_api.png)
6. Open de tweede stap **machtigingen selecteren**.
7. Gebruik het selectievakje om uw systeemeigen App toegang verlenen tot uw proxytoepassing en klik vervolgens op **Selecteer**.

   ![Toegang verlenen tot de proxy-app](./media/application-proxy-configure-native-client-application/select_perms.png)
8. Selecteer **Done**.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>Stap 4: De Active Directory-Verificatiebibliotheek bewerken
De systeemeigen toepassingscode bewerken in de verificatiecontext van de Active Directory Authentication Library (ADAL) om op te nemen van de volgende tekst:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = await authContext.AcquireTokenAsync("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

De variabelen in de voorbeeldcode moeten als volgt worden vervangen:

* **Tenant-ID** kunt u vinden in de Azure-portal. Navigeer naar **Azure Active Directory** > **eigenschappen** en kopieer de map-ID. 
* **Externe URL** is de front-end-URL die u hebt ingevoerd in de Proxy-toepassing. Als u deze waarde zoekt, gaat u naar de **toepassingsproxy** sectie van de app proxy.
* **App-ID** van de systeemeigen app kan worden gevonden op de **eigenschappen** pagina van de systeemeigen toepassing.
* **Omleidings-URI van de systeemeigen app** kunt u vinden op de **omleidings-URI's** pagina van de systeemeigen toepassing.

Zodra de ADAL wordt bewerkt met deze parameters, moeten uw gebruikers kunnen verifiëren native client-apps, zelfs als ze buiten het bedrijfsnetwerk zijn. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de stroom van de systeemeigen toepassing [systeemeigen toepassing voor de web-API](../develop/authentication-scenarios.md#native-application-to-web-api)

Meer informatie over het instellen van [eenmalige aanmelding voor de toepassingsproxy](application-proxy-single-sign-on.md)
