---
title: Toegang tot on-premises API's met Azure AD-toepassingsproxy
description: Azure Active Directory-toepassingsproxy kunt systeemeigen apps veilig toegang tot API's en bedrijfslogica hosten van on-premises of op cloud VM's.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: c2b99525e3d0a61c02dc502fcd0927ea65993e5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108508"
---
# <a name="secure-access-to-on-premises-apis-with-azure-ad-application-proxy"></a>Beveiligde toegang tot on-premises API's met Azure AD-toepassingsproxy

U hebt mogelijk bedrijfslogica API's die on-premises uitgevoerd of die worden gehost op virtuele machines in de cloud. Uw systeemeigen Android, iOS, Mac of Windows-apps moeten communiceren met de API-eindpunten te gebruiken van gegevens of op te geven die tussenkomst van de gebruiker. Azure AD Application Proxy en de [Azure Active Directory Authentication Libraries (ADAL)](/azure/active-directory/develop/active-directory-authentication-libraries) laat uw systeemeigen apps veilig toegang krijgen tot uw on-premises API's. Azure Active Directory Application Proxy is een oplossing sneller en veiliger dan firewall-poorten te openen en beheren van verificatie en autorisatie op app-niveau. 

Dit artikel begeleidt u bij het instellen van een Azure AD Application Proxy-oplossing voor het hosten van een web-API-service die systeemeigen apps toegang hebben tot. 

## <a name="overview"></a>Overzicht

Het volgende diagram toont een traditionele manier voor het publiceren van on-premises API's. Deze aanpak vereist binnenkomende poorten 80 en 443 te openen.

![Traditionele API-toegang](./media/application-proxy-secure-api-access/overview-publish-api-open-ports.png)

Het volgende diagram toont hoe u kunt Azure AD-toepassingsproxy veilig API's publiceren zonder alle binnenkomende poorten te openen:

![Azure AD Application Proxy-API-toegang](./media/application-proxy-secure-api-access/overview-publish-api-app-proxy.png)

De Azure AD-toepassingsproxy vormt de ruggengraat van de oplossing werkt als een openbaar eindpunt voor API-toegang en verificatie en autorisatie bieden. U kunt toegang tot uw API's van een groot scala aan platformen met behulp van de [ADAL](/azure/active-directory/develop/active-directory-authentication-libraries) bibliotheken. 

Aangezien Azure AD Application Proxy-verificatie en autorisatie zijn gebaseerd op Azure AD, kunt u Azure AD voor voorwaardelijke toegang gebruiken om te controleren of alleen vertrouwde apparaten hebben toegang tot API's die zijn gepubliceerd via toepassingsproxy. Gebruik Azure AD Join of hybride van Azure AD join voor desktops en Intune voor apparaten worden beheerd. U kunt ook profiteren van Azure Active Directory Premium-functies, zoals Azure multi-factor Authentication en de machine learning-ondersteuning beveiliging van nemen [Azure Identity Protection](/azure/active-directory/active-directory-identityprotection).

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen in dit scenario, hebt u het volgende nodig:

- Beheerderstoegang tot een Azure-adreslijst, met een account dat u kunt maken en registreren van apps
- De voorbeeld-web-API en de systeemeigen client-apps uit [https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp](https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp) 

## <a name="publish-the-api-through-application-proxy"></a>Publiceren van de API via de toepassingsproxy

Voor het publiceren van een API buiten het intranet via toepassingsproxy, kunt u hetzelfde patroon voor publiceren web-apps volgen. Zie [Zelfstudie: Toevoegen van een on-premises toepassing voor externe toegang via Application Proxy in Azure Active Directory](application-proxy-add-on-premises-application.md).

De web-SecretAPI API via Application Proxy publiceren:

1. Bouw en het voorbeeldproject SecretAPI publiceren als een ASP.NET-web-app op uw lokale computer of intranet. Zorg ervoor dat u toegang tot de web-app lokaal. 
   
1. In de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory** in het linkernavigatievenster. Klik vervolgens op de **overzicht** weergeeft, schakelt **bedrijfstoepassingen**.
   
1. Aan de bovenkant van de **bedrijfstoepassingen - alle toepassingen** weergeeft, schakelt **nieuwe toepassing**.
   
1. Op de **toevoegen van een toepassing** pagina onder **toevoegen aan uw eigen app**, selecteer **On-premises toepassing**. 
   
1. Als u een Connector voor toepassingsproxy geïnstalleerd hebt, wordt u gevraagd om deze te installeren. Selecteer **Toepassingsproxyconnector downloaden** downloaden en installeren van de connector. 
   
1. Nadat u de Application Proxy-Connector hebt geïnstalleerd op de **toevoegen van uw eigen on-premises toepassing** pagina:
   
   1. Voer *SecretAPI* naast **naam**.
      
   1. Voer de URL die u gebruikt voor toegang tot de API van uw intranet naast **interne Url**. 
      
   1. Zorg ervoor dat **pre-authenticatie** is ingesteld op **Azure Active Directory**. 
      
   1. Selecteer **toevoegen** aan de bovenkant van de pagina en wacht tot de app moet worden gemaakt.
   
   ![API-app toevoegen](./media/application-proxy-secure-api-access/3-add-api-app.png)
   
1. Op de **bedrijfstoepassingen - alle toepassingen** weergeeft, schakelt de **SecretAPI** app. 
   
1. Op de **SecretAPI - overzicht** weergeeft, schakelt **eigenschappen** in het linkernavigatievenster.
   
1. U niet wilt dat API's beschikbaar zijn voor eindgebruikers in de **MyApps** deelvenster, dus Stel **zichtbaar voor gebruikers** naar **Nee** aan de onderkant van de **eigenschappen**pagina en selecteer vervolgens **opslaan**.
   
   ![Niet zichtbaar voor gebruikers](./media/application-proxy-secure-api-access/5-not-visible-to-users.png)
   
U kunt uw web-API via Azure AD-toepassingsproxy hebt gepubliceerd. Voeg gebruikers aan wie toegang heeft tot de app nu toe. 

1. Op de **SecretAPI - overzicht** weergeeft, schakelt **gebruikers en groepen** in het linkernavigatievenster.
   
1. Op de **gebruikers en groepen** weergeeft, schakelt **gebruiker toevoegen**.  
   
1. Op de **toewijzing toevoegen** weergeeft, schakelt **gebruikers en groepen**. 
   
1. Op de **gebruikers en groepen** pagina, zoek en selecteer gebruikers die toegang heeft tot de app, inclusief ten minste zelf. Na het selecteren van alle gebruikers, selecteert u **Selecteer**. 
   
   ![Selecteren en toewijzen van gebruiker](./media/application-proxy-secure-api-access/7-select-admin-user.png)
   
1. Terug op de **toevoegen toewijzing** weergeeft, schakelt **toewijzen**. 

> [!NOTE]
> API's die gebruikmaken van geïntegreerde Windows-verificatie mogelijk [extra stappen](/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

## <a name="register-the-native-app-and-grant-access-to-the-api"></a>De systeemeigen app te registreren en toegang verlenen tot de API

Systeemeigen apps zijn programma's ontwikkeld om te gebruiken op een bepaald platform of apparaat. Voordat uw systeemeigen app kan verbinding maken en toegang een API tot, registreert u deze in Azure AD. De volgende stappen laten zien hoe u een systeemeigen app registreren en toegang tot de web-API die u hebt gepubliceerd via toepassingsproxy geven.

De systeemeigen app AppProxyNativeAppSample registreren:

1. In de Azure Active Directory **overzicht** weergeeft, schakelt **App-registraties**, en aan de bovenkant van de **App-registraties** venster **nieuwe inschrijving** .
   
1. Op de **registreren van een toepassing** pagina:
   
   1. Onder **naam**, voer *AppProxyNativeAppSample*. 
      
   1. Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**. 
      
   1. Onder **Omleidings-URL**, vervolgkeuzelijst en selecteert u **openbare client (mobiele en desktop)** , en voer vervolgens *https:\//appproxynativeapp*. 
      
   1. Selecteer **registreren**, en wacht tot de app worden geregistreerd. 
      
      ![Nieuwe toepassing registreren](./media/application-proxy-secure-api-access/8-create-reg-ga.png)
   
U hebt nu de app AppProxyNativeAppSample geregistreerd in Azure Active Directory. Uw systeemeigen apptoegang geven tot de SecretAPI web-API:

1. In de Azure Active Directory **overzicht** > **App-registraties** weergeeft, schakelt de **AppProxyNativeAppSample** app. 
   
1. Op de **AppProxyNativeAppSample** weergeeft, schakelt **API-machtigingen** in het linkernavigatievenster. 
   
1. Op de **API-machtigingen** weergeeft, schakelt **toevoegen van een machtiging**.
   
1. Op de eerste **aanvragen API-machtigingen** weergeeft, schakelt de **API's maakt gebruik van mijn organisatie** tabblad en zoek en selecteer **SecretAPI**. 
   
1. Op de volgende **aanvragen API-machtigingen** pagina, schakel het selectievakje in naast **user_impersonation**, en selecteer vervolgens **machtigingen toevoegen**. 
   
    ![Een API selecteren](./media/application-proxy-secure-api-access/10-secretapi-added.png)
   
1. Terug op de **API-machtigingen** pagina die u kunt selecteren **verlenen van toestemming van een beheerder voor Contoso** om te voorkomen dat andere gebruikers hoeven afzonderlijk toestemming geven voor de app. 

## <a name="configure-the-native-app-code"></a>De systeemeigen app-code configureren

De laatste stap is het configureren van de systeemeigen app. Het volgende fragment van het *Form1.cs* bestand in de voorbeeld-app NativeClient zorgt ervoor dat de ADAL-bibliotheek te verkrijgen van het token voor het aanvragen van de API-aanroep en koppelt u deze als bearer aan de koptekst van de app. 
   
   ```csharp
       AuthenticationResult result = null;
       HttpClient httpClient = new HttpClient();
       authContext = new AuthenticationContext(authority);
       result = await authContext.AcquireTokenAsync(todoListResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
       
       // Append the token as bearer in the request header.
       httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
       
       // Call the API.
       HttpResponseMessage response = await httpClient.GetAsync(todoListBaseAddress + "/api/values/4");
   
       // MessageBox.Show(response.RequestMessage.ToString());
       string s = await response.Content.ReadAsStringAsync();
       MessageBox.Show(s);
   ```
   
Als u wilt de systeemeigen app configureren voor verbinding maken met Azure Active Directory en roep de API-App-Proxy, werken de tijdelijke aanduiding voor waarden in de *App.config* -bestand van de voorbeeld-app NativeClient met waarden uit Azure AD: 

- Plak de **map (tenant)-ID** in de `<add key="ida:Tenant" value="" />` veld. U kunt vinden en kopieer deze waarde (GUID) van de **overzicht** pagina van een van uw apps. 
  
- Plak de AppProxyNativeAppSample **(client) toepassings-ID** in de `<add key="ida:ClientId" value="" />` veld. U kunt vinden en kopieer deze waarde (GUID) van de AppProxyNativeAppSample **overzicht** pagina.
  
- Plak de AppProxyNativeAppSample **omleidings-URI** in de `<add key="ida:RedirectUri" value="" />` veld. U kunt vinden en kopieer deze waarde (URI) van de AppProxyNativeAppSample **verificatie** pagina. 
  
- Plak de SecretAPI **URI toepassings-ID** in de `<add key="todo:TodoListResourceId" value="" />` veld. U kunt vinden en kopieer deze waarde (URI) van de SecretAPI **beschikbaar maken van een API** pagina.
  
- Plak de SecretAPI **URL van startpagina** in de `<add key="todo:TodoListBaseAddress" value="" />` veld. U kunt vinden en kopieer deze waarde (URL) van de SecretAPI **Branding** pagina.

Nadat u de parameters configureren, bouwen en uitvoeren van de systeemeigen app. Wanneer u selecteert de **aanmelden** knop, de app kunt u zich aanmeldt en vervolgens wordt een scherm voor voltooide om te bevestigen dat deze is verbonden met de SecretAPI.

![Geslaagd](./media/application-proxy-secure-api-access/success.png)

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Toevoegen van een on-premises toepassing voor externe toegang via Application Proxy in Azure Active Directory](application-proxy-add-on-premises-application.md)
- [Snelstart: Een clienttoepassing voor toegang tot web-API's configureren](../develop/quickstart-configure-app-access-web-apis.md)
- [Systeemeigen client-toepassingen om te communiceren met de proxy-toepassingen inschakelen](application-proxy-configure-native-client-application.md)
