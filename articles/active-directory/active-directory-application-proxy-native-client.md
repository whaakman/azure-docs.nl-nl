---
title: Native ClientApps - Azure AD publiceren | Microsoft Docs
description: Bevat informatie over het inschakelen van native client-apps om te communiceren met Azure AD Connector voor toepassingsproxy om te bieden veilige externe toegang tot uw lokale apps.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 2be62c00d144e47cef8ea4df5aa82554f2bbcc18
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Het inschakelen van native client-apps om te communiceren met de proxy-toepassingen

Naast de webtoepassingen, worden Azure Active Directory-toepassingsproxy ook gebruikt voor het publiceren van native client-apps die zijn geconfigureerd met de Azure AD Authentication Library (ADAL). Native client-apps verschillen van web-apps, omdat ze zijn geïnstalleerd op een apparaat, terwijl de web-apps via een browser worden geopend. 

Toepassingsproxy ondersteunt native client-apps door overnemende Azure AD uitgegeven tokens in de header verzonden. De service Application Proxy voert verificatie namens de gebruikers. Deze oplossing gebruikt geen toepassing tokens voor authenticatie. 

![Relatie tussen eindgebruikers, Azure Active Directory en gepubliceerde toepassingen](./media/active-directory-application-proxy-native-client/richclientflow.png)

Gebruik de Azure AD-Verificatiebibliotheek, die zorgt voor verificatie en biedt ondersteuning voor veel client-omgevingen, systeemeigen toepassingen te publiceren. Toepassingsproxy in past de [systeemeigen toepassing aan Web-API-scenario](develop/active-directory-authentication-scenarios.md#native-application-to-web-api). 

Dit artikel begeleidt u bij de vier stappen voor het publiceren van een systeemeigen toepassing met toepassingsproxy en de Azure AD-Verificatiebibliotheek. 

## <a name="step-1-publish-your-application"></a>Stap 1: Uw toepassing publiceren
Uw proxy-toepassing te publiceren, net als alle andere toepassingen en gebruikers toegang krijgen tot uw toepassing toe te wijzen. Zie voor meer informatie [publiceren van toepassingen met toepassingsproxy](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Stap 2: Uw toepassing configureren
Uw eigen toepassing als volgt configureren:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **App registraties**.
3. Selecteer **registratie van de nieuwe toepassing**.
4. Geef een naam voor uw toepassing, selecteert **systeemeigen** als het toepassingstype, en geef de omleidings-URI voor uw toepassing. 

   ![Maak een nieuwe app-registratie](./media/active-directory-application-proxy-native-client/create.png)
5. Selecteer **Maken**.

Zie voor meer informatie over het maken van een nieuwe app-registratie, [toepassingen integreren met Azure Active Directory](.//develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-to-other-applications"></a>Stap 3: Verleen toegang tot andere toepassingen
De systeemeigen toepassing worden blootgesteld aan andere toepassingen in uw directory inschakelen:

1. Nog steeds in **App registraties**, selecteer de nieuwe systeemeigen toepassing die u zojuist hebt gemaakt.
2. Selecteer **vereist machtigingen**.
3. Selecteer **Toevoegen**.
4. Open de eerste stap **selecteert u een API**.
5. Gebruik de zoekbalk de toepassingsproxy-app die u hebt gepubliceerd in de eerste sectie vinden. Kies deze app en klik vervolgens op **Selecteer**. 

   ![Zoeken naar de proxy-app](./media/active-directory-application-proxy-native-client/select_api.png)
6. Open de tweede stap **machtigingen selecteren**.
7. Gebruik de selectievakjes om uw eigen App toegang verlenen tot uw proxy-toepassing en klik vervolgens op **Selecteer**.

   ![Toegang verlenen tot de proxy-app](./media/active-directory-application-proxy-native-client/select_perms.png)
8. Selecteer **gedaan**.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>Stap 4: De Active Directory Authentication Library bewerken
De systeemeigen toepassingscode bewerken in de verificatiecontext van de Active Directory Authentication Library (ADAL) om op te nemen van de volgende tekst:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

De variabelen in de voorbeeldcode wordt vervangen als volgt:

* **Tenant-ID** vindt u in de Azure-portal. Navigeer naar **Azure Active Directory** > **eigenschappen** en kopieer de map-ID. 
* **Externe URL** is de front-URL die u hebt ingevoerd in de Proxy-toepassing. U vindt deze waarde, gaat u naar de **toepassingsproxy** sectie van de proxy-app.
* **App-ID** van de systeemeigen app kunt u vinden op de **eigenschappen** pagina van de oorspronkelijke toepassing.
* **Omleidings-URI van de systeemeigen app** vindt u op de **omleidings-URI's** pagina van de oorspronkelijke toepassing.

Nadat de ADAL is bewerkt met deze parameters, is uw gebruikers moet kunnen worden geverifieerd bij native client-apps, zelfs wanneer ze zich buiten het bedrijfsnetwerk. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de stroom systeemeigen toepassing [systeemeigen toepassing aan web-API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)

Meer informatie over het instellen van [eenmalige aanmelding voor toepassingsproxy](application-proxy-sso-overview.md)
