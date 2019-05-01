---
title: Systeemeigen client-apps - Azure AD publiceren | Microsoft Docs
description: Bevat informatie over het inschakelen van systeemeigen client-apps om te communiceren met Azure AD Application Proxy Connector voor veilige externe toegang tot uw on-premises toepassingen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4614d8190436ad89faa200f83b1a71bde10a8acb
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64684946"
---
# <a name="how-to-enable-native-client-applications-to-interact-with-proxy-applications"></a>Systeemeigen client-toepassingen om te communiceren met de proxy-toepassingen inschakelen

U kunt Azure Active Directory (Azure AD) Application Proxy gebruiken voor web-apps publiceren, maar het kan ook worden gebruikt voor het publiceren van systeemeigen client-toepassingen die zijn geconfigureerd met de Azure AD Authentication Library (ADAL). Native clienttoepassingen verschillen van web-apps omdat ze zijn geïnstalleerd op een apparaat, terwijl de web-apps worden geopend via een browser.

Ter ondersteuning van native clienttoepassingen, geaccepteerd Application Proxy Azure AD-uitgegeven tokens die worden verzonden in de header. De service voor toepassingsproxy wordt bij de verificatiemethode voor de gebruikers. Deze oplossing biedt geen toepassing tokens gebruiken voor verificatie.

![Relatie tussen de eindgebruikers, Azure Active Directory en gepubliceerde toepassingen](./media/application-proxy-configure-native-client-application/richclientflow.png)

Gebruik de Azure AD Authentication Library, die zorgt voor verificatie en biedt ondersteuning voor omgevingen met veel client voor het publiceren van toepassingen. Toepassingsproxy past de [systeemeigen toepassing voor de Web-API-scenario](../develop/native-app.md).

In dit artikel begeleidt u bij de vier stappen voor het publiceren van een systeemeigen toepassing met Application Proxy en de Azure AD-Verificatiebibliotheek.

## <a name="step-1-publish-your-proxy-application"></a>Stap 1: Uw proxytoepassing publiceren

Uw proxytoepassing te publiceren, net als elke andere toepassing en gebruikers toegang tot uw toepassing toe te wijzen. Zie voor meer informatie, [publiceren van toepassingen met toepassingsproxy](application-proxy-add-on-premises-application.md).

## <a name="step-2-register-your-native-application"></a>Stap 2: Uw native toepassing registreren

Nu moet u uw toepassing registreren in Azure AD wordt als volgt:

1. Aanmelden bij de [Azure Active Directory-portal](https://aad.portal.azure.com/). De **Dashboard** voor de **Azure Active Directory-beheercentrum** wordt weergegeven.
2. Selecteer in de zijbalk **Azure Active Directory**. De **Azure Active Directory** overzichtspagina wordt weergegeven.
3. Selecteer in de zijbalk Azure AD-overzicht **App-registraties**. De lijst van alle app-registraties wordt weergegeven.
4. Selecteer **registratie van nieuwe**. De **registreren van een toepassing** pagina wordt weergegeven.

   ![Maak een nieuwe app-registratie](./media/application-proxy-configure-native-client-application/create.png)
5. In de **naam** kop, Geef een gebruikersgerichte weergavenaam voor uw toepassing.
6. Onder de **ondersteund accounttypen** kop, selecteert u een niveau van toegang met behulp van deze richtlijnen:
   - Om u te richten alleen accounts die intern voor uw organisatie zijn, selecteer **Accounts in deze organisatie-map alleen**.
   - Om u te richten alleen bedrijven of onderwijsinstellingen klanten, selecteer **Accounts in een organisatie-map**.
   - Als u wilt de breedste set Microsoft-id's als doel, selecteert u **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts**.
7. In de **omleidings-URI** kop, selecteer **openbare client (mobiele en desktop)**, en typ vervolgens de omleidings-URI voor uw toepassing.
8. Selecteer en lees de **voor Microsoft-Platform**, en selecteer vervolgens **registreren**. Een overzichtspagina voor de registratie van nieuwe toepassing is gemaakt en weergegeven.

Zie voor meer informatie over het maken van een nieuwe toepassing registreren, [toepassingen integreren met Azure Active Directory](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="step-3-grant-access-to-your-proxy-application"></a>Stap 3: Toegang verlenen tot uw proxytoepassing

Nu dat u uw systeemeigen toepassing hebt geregistreerd, kunt u deze toegang verlenen voor andere toepassingen in uw directory in dit geval voor toegang tot de proxytoepassing. De systeemeigen toepassing worden blootgesteld aan de application proxy inschakelen:

1. Selecteer in de zijbalk aan de pagina voor registratie van nieuwe toepassing **API-machtigingen**. De **API-machtigingen** pagina voor de registratie van nieuwe toepassing wordt weergegeven.
2. Selecteer **toevoegen van een machtiging**. De **aanvragen API-machtigingen** pagina wordt weergegeven.
3. Onder de **Select an API** optie **API's maakt gebruik van mijn organisatie**. Er wordt een lijst weergegeven met de toepassingen die beschikbaar maken van API's in uw directory.
4. Typ in het zoekvak in of blader om te vinden van de proxytoepassing die u hebt gepubliceerd in [stap 1: Uw proxytoepassing publiceren](#step-1-publish-your-proxy-application), en selecteer vervolgens de proxytoepassing.
5. In de **wat voor soort machtigingen is uw toepassing vereist?** kop, selecteer het machtigingstype. Als uw systeemeigen toepassing nodig tot de API van de application proxy als de gebruiker is aangemeld heeft, kiest u **overgedragen machtigingen**. Als uw systeemeigen toepassing wordt uitgevoerd als een achtergrondservice of -daemon zonder een aangemelde gebruiker, kiest u **Toepassingsmachtigingen**.
6. In de **machtigingen selecteren** heen gaat, selecteer de gewenste machtiging en selecteert u **machtigingen toevoegen**. De **API-machtigingen** pagina voor uw systeemeigen toepassing nu ziet u de proxy-toepassing en de toestemming API die u hebt toegevoegd.

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

De vereiste gegevens in de voorbeeldcode vindt u in de Azure AD-portal als volgt:

| Gegevens zijn vereist | Hoe kunt u vinden in de Azure AD-portal |
| --- | --- |
| \<Tenant-ID > | **Azure Active Directory** > **Properties** > **Directory ID** |
| \<Externe Url van Proxy-App > | **Bedrijfstoepassingen** > *uw proxytoepassing* > **toepassingsproxy** > **externe Url** |
| \<App-ID van de systeemeigen app > | **Bedrijfstoepassingen** > *uw systeemeigen toepassing* > **eigenschappen** > **toepassings-ID** |
| \<Omleidings-URI van de systeemeigen App > | **Azure Active Directory** > **App-registraties** > *uw systeemeigen toepassing* > **omleidings-URI's** |
| \<De Url van de API App proxy > | **Azure Active Directory** > **App-registraties** > *uw systeemeigen toepassing* > **API-machtigingen**  >  **API / MACHTIGINGEN naam** |

Nadat u de ADAL met deze parameters hebt bewerkt, worden uw gebruikers kunnen verifiëren voor native clienttoepassingen, zelfs als ze buiten het bedrijfsnetwerk.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de stroom van de systeemeigen toepassing [systeemeigen apps in Azure Active Directory](../develop/native-app.md).

Meer informatie over het instellen van [eenmalige aanmelding voor toepassingen in Azure Active Directory](what-is-single-sign-on.md#choosing-a-single-sign-on-method).
