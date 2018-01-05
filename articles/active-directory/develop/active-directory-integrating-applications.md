---
title: Toepassingen integreren met Azure Active Directory
description: Informatie over het toevoegen, bijwerken of verwijderen van een toepassing in Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: 
author: PatAltimore
manager: mtillman
editor: mbaldwin
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/04/2017
ms.author: bryanla
ms.custom: aaddev
ms.reviewer: luleon
ms.openlocfilehash: e398536ff6f660c75e4e063040eab33a831d65c6
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="integrating-applications-with-azure-active-directory"></a>Toepassingen integreren met Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Ontwikkelaars van ondernemingen en software-as-a-service (SaaS)-providers kunnen ontwikkelen commerciële cloudservices of line-of-business-toepassingen dat kunnen worden geïntegreerd met Azure Active Directory (Azure AD) voor beveiligde aanmelden en -autorisatie bieden voor hun Services. Voor het integreren van een toepassing of service met Azure AD, moet een ontwikkelaar de toepassing eerst registreren bij Azure AD.

In dit artikel laat zien hoe toevoegen, bijwerken of verwijderen van registratie van een toepassing in Azure AD. U meer informatie over de verschillende typen toepassingen die kunnen worden geïntegreerd met Azure AD, het configureren van uw toepassingen voor toegang tot andere resources, zoals web-API's en meer.

Zie voor meer informatie over de twee Azure AD-objecten die een geregistreerde toepassing en de relatie tussen deze twee vertegenwoordigen, [toepassingsobjecten en Service-Principal objecten](active-directory-application-objects.md); voor meer informatie over de huisstijlrichtlijnen moet u bij het ontwikkelen van toepassingen met Azure Active Directory, Zie [huisstijl richtlijnen voor geïntegreerde Apps](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Een toepassing toevoegen
Alle toepassingen die gebruikmaken van de mogelijkheden van Azure AD wil moet eerst worden geregistreerd in een Azure AD-tenant. Het registratieproces omvat het Azure AD-gegevens over uw toepassing, zoals de URL waar is gevonden, de URL om te antwoorden verzenden nadat een gebruiker is geverifieerd, geeft de URI die de app, enzovoort identificeert.

### <a name="to-register-a-new-application-using-the-azure-portal"></a>Registreren van een nieuwe toepassing met de Azure portal
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account biedt toegang tot meer dan één, klikt u op uw account in de rechterbovenhoek en stel uw portal-sessie op de gewenste Azure AD-tenant.
3. Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App registraties**, en klik op **registratie van de nieuwe toepassing**.

   ![Een nieuwe toepassing registreren](./media/active-directory-integrating-applications/add-app-registration.png)

4. Wanneer de **maken** pagina wordt weergegeven, voer de registratiegegevens van uw toepassing: 

  - **Naam:** een zinvolle toepassingsnaam invoeren
  - **Type van de toepassing:** 
    - Selecteer 'Native' voor [clienttoepassingen](active-directory-dev-glossary.md#client-application) die lokaal worden geïnstalleerd op een apparaat. Deze instelling wordt gebruikt voor OAuth publiek [systeemeigen clients](active-directory-dev-glossary.md#native-client).
    - Selecteer ' Web-app / API ' voor [clienttoepassingen](active-directory-dev-glossary.md#client-application) en [resource/API toepassingen](active-directory-dev-glossary.md#resource-server) die zijn geïnstalleerd op een beveiligde server. Deze instelling wordt gebruikt voor OAuth vertrouwelijke [web-clients](active-directory-dev-glossary.md#web-client) en openbare [clients op basis van gebruikers-agent](active-directory-dev-glossary.md#user-agent-based-client). Een client en de resource/API kan ook worden weergegeven door dezelfde toepassing.
  - **Aanmeldings-URL:** voor ' Web-app / API ' toepassingen, geef de basis-URL van uw app. Bijvoorbeeld: `http://localhost:31544` mogelijk de URL voor een web-app op uw lokale computer uitgevoerd. Gebruikers aan te melden bij een webtoepassing voor de client deze URL gebruikt. 
  - **Omleidings-URI:** voor 'Systeemeigen' toepassingen, geeft u de URI die wordt gebruikt door Azure AD om te retourneren van reacties token. Voer een waarde die specifiek voor uw toepassing, bijvoorbeeld`http://MyFirstAADApp`

   ![Een nieuwe toepassing registreren - maken](./media/active-directory-integrating-applications/add-app-registration-create.png)

   Als u specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen wilt, Bekijk onze [snelstartgidsen](active-directory-developers-guide.md#get-started).

5. Wanneer u klaar bent, klikt u op **maken**. Azure AD een unieke ID van de toepassing toegewezen aan uw toepassing en u bent getroffen om de belangrijkste registratiepagina van uw toepassing. Afhankelijk van of uw toepassing een web- of systeemeigen toepassing is, worden de verschillende opties opgegeven aanvullende mogelijkheden toevoegen aan uw toepassing. Zie de volgende sectie voor een overzicht van toestemming en details over het inschakelen van aanvullende configuratie-functies in uw toepassing registreren (referenties, machtigingen aanmeldingspagina inschakelen voor gebruikers van andere tenants).

  > [!NOTE]
  > De zojuist geregistreerde toepassing is standaard geconfigureerd om toe te staan **alleen** gebruikers van dezelfde tenant aan te melden bij uw toepassing.
  > 
  > 

## <a name="updating-an-application"></a>Bijwerken van een toepassing
Als uw toepassing met Azure AD is geregistreerd, moet deze mogelijk worden bijgewerkt om te voorzien van toegang tot web-API's, beschikbaar gesteld in andere organisaties en meer. Deze sectie beschrijft de verschillende manieren waarop u uw toepassing meer kunt configureren. We eerst met een overzicht van het framework toestemming, waarvan belangrijk is te weten bij het bouwen van toepassingen die moeten worden gebruikt door andere gebruikers of toepassingen.

### <a name="overview-of-the-consent-framework"></a>Overzicht van het framework toestemming

Het framework van Azure AD toestemming kunt eenvoudig ontwikkelen van multitenant-web- en native client-toepassingen, met inbegrip van toepassingen met meerdere lagen. Deze toepassingen kunnen aanmelden door gebruikersaccounts van een Azure AD-tenant, verschilt van de waar de toepassing is geregistreerd. Ze kunnen ook toegang nodig tot web-API's zoals de Microsoft Graph-API (voor toegang tot Azure Active Directory, Intune en services in Office 365) en andere Microsoft-services-API's, naast uw eigen web-API's. Het framework is gebaseerd op een gebruiker of beheerder toestemming verlenen tot een toepassing waarin u wordt gevraagd om te worden geregistreerd in de directory, waarbij toegang tot Active directory-gegevens.

Bijvoorbeeld als een webtoepassing voor de client moet lezen agenda-informatie over de gebruiker van Office 365, is die gebruiker vereist voor het eerst toestemming aan de clienttoepassing. Nadat u toestemming is opgegeven, is de clienttoepassing worden kunnen de Microsoft Graph-API aanroepen namens de gebruiker en het gebruik van de agenda-informatie, indien nodig. De [Microsoft Graph API](https://graph.microsoft.io) biedt toegang tot gegevens in Office 365 (zoals agenda's en berichten van Exchange, sites en lijsten van SharePoint, documenten van OneDrive, OneNote-notitieblokken, taken van de Planner, werkmappen van Excel, enz.) en de gebruikers en groepen van Azure AD en andere gegevensobjecten van meer Microsoft-cloudservices. 

Het framework toestemming is gebouwd op OAuth 2.0 en de verschillende stromen, zoals code verlenen en client autorisatiereferenties verlenen, met behulp van openbare of vertrouwelijke clients. Met behulp van OAuth 2.0, maakt Azure AD het mogelijk te veel verschillende soorten clienttoepassingen, zoals in een telefoon, tablet, server of een webtoepassing is opgebouwd en toegang krijgen tot de vereiste resources.

Zie voor meer informatie over het gebruik van het framework toestemming met OAuth2.0 toestemming verleent [toegang verlenen aan webtoepassingen die gebruikmaken van OAuth 2.0 en Azure AD](active-directory-protocols-oauth-code.md) en[verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md). Zie voor meer informatie over het verkrijgen van toegang tot Office 365 via Microsoft Graph [App verificatie met Microsoft Graph](https://graph.microsoft.io/docs/authorization/auth_overview).

#### <a name="example-of-the-consent-experience"></a>Voorbeeld van de toestemming-ervaring

De volgende stappen ziet u hoe de toestemming ondervinden werkt voor de ontwikkelaar van toepassingen en de gebruiker.

1. Stel dat u hebt een web-clienttoepassing die nodig zijn om aan te vragen van specifieke machtigingen voor toegang tot een resource/API. Leert u hoe u deze configuratie in de volgende sectie, maar in feite de Azure-portal wordt gebruikt om te declareren machtigingsaanvragen tijdens de configuratie. Net als andere configuratie-instellingen uitmaken ze deel van de Azure AD-registratie van de toepassing:
   
  ![Machtigingen voor andere toepassingen](./media/active-directory-integrating-applications/requiredpermissions.png)
    
2. U kunt uw toepassing worden machtigingen zijn bijgewerkt, de toepassing wordt uitgevoerd en een gebruiker is gebruikt voor het eerst. De toepassing moet eerst een autorisatiecode verkrijgen van Azure AD `/authorize` eindpunt. De autorisatiecode kan vervolgens worden gebruikt bij het aanschaffen van een nieuwe toegang en token vernieuwen.

3. Als de gebruiker nog niet is geverifieerd, Azure AD van `/authorize` eindpunt vraagt om de aanmeldingspagina.
   
  ![Gebruiker of beheerder aanmelden bij Azure AD](./media/active-directory-integrating-applications/usersignin.png)

4. Nadat de gebruiker is aangemeld, wordt Azure AD bepalen of de gebruiker moet een pagina toestemming worden weergegeven. Deze beslissing is gebaseerd op of de gebruiker (of de beheerder van hun organisatie) is al verleend de toepassing toestemming. Toestemming heeft geen toestemming gekregen, Azure AD wordt de gebruiker om toestemming gevraagd als staan de vereiste machtigingen nodig om te functioneren. De reeks machtigingen die worden weergegeven in het dialoogvenster toestemming overeen met de die u hebt geselecteerd in de gedelegeerde machtigingen in de Azure portal.
   
  ![Toestemming gebruikerservaring](./media/active-directory-integrating-applications/consent.png)

5. Nadat de gebruiker toestemming verleent, wordt een autorisatiecode wordt geretourneerd naar uw toepassing wordt ingewisseld verkrijgen van een toegangstoken en vernieuwen van tokens. Zie voor meer informatie over deze stroom, de [webtoepassing voor web-API-sectie in scenario's voor verificatie voor Azure AD](active-directory-authentication-scenarios.md#web-application-to-web-api).

6. Als beheerder, kunt u ook akkoord gedelegeerde machtigingen van een toepassing namens de gebruikers in uw tenant. Administratieve toestemming voorkomt u het dialoogvenster toestemming worden weergegeven voor elke gebruiker in de tenant en uw toepassing wordt uitgevoerd pagina in de [Azure-portal](https://portal.azure.com). Van de **instellingen** pagina voor uw toepassing, klikt u op **Required Permissions** en klik op de **machtiging verlenen** knop. 

  ![Machtigingen voor expliciete admin toestemming verlenen](./media/active-directory-integrating-applications/grantpermissions.png)
    
  > [!NOTE]
  > Verlenen expliciete toestemming geven met behulp van de **machtiging verlenen** knop momenteel vereist is voor één pagina toepassingen (SPA) die gebruikmaken van ADAL.js. Anders mislukt de toepassing wanneer het toegangstoken is aangevraagd.   

### <a name="configure-a-client-application-to-access-web-apis"></a>Een clienttoepassing voor toegang tot web-API's configureren
Om een web/vertrouwelijk clienttoepassing kunnen deelnemen aan een machtiging grant-stroom die verificatie vereist (en verkrijgen van een toegangstoken), moet deze beveiligde referenties maken. De standaardmethode voor verificatie wordt ondersteund door de Azure-portal is de Client-ID + geheime sleutel. Deze sectie bevat informatie over de benodigde referenties op te geven de geheime sleutel van de client configuratiestappen.

Bovendien voordat een client toegang heeft tot een web-API die worden weergegeven door een resource-toepassing (zoals Microsoft Graph API), het framework toestemming zorgt ervoor dat de client verkrijgt de machtiging grant vereist, op basis van de machtigingen die zijn aangevraagd. Standaard kunnen alle toepassingen machtigingen kiezen uit 'Windows Azure Active Directory' (Graph API) en 'Windows Azure Service Management-API'. De [Graph API 'aanmelden en gebruikersprofiel lezen' machtiging](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) wordt ook standaard geselecteerd. Als de client wordt geregistreerd in een tenant met accounts die zijn geabonneerd op Office 365, zijn machtigingen voor SharePoint en Exchange Online en Web-API's beschikbaar voor selectie. U kunt kiezen uit [twee soorten machtigingen](active-directory-dev-glossary.md#permissions) voor elke web-API gewenste:

- Toepassingsmachtigingen: Uw clienttoepassing moet toegang tot de web-API als zelf (geen gebruikerscontext). Dit type machtiging beheerder toestemming nodig en is ook niet beschikbaar voor systeemeigen clienttoepassingen.

- Gedelegeerde machtigingen: Uw clienttoepassing moet toegang tot de web-API als de gebruiker is aangemeld, maar met de toegang beperkt door de geselecteerde machtiging. Dit type machtigingen kan worden verleend door een gebruiker, tenzij de beheerder toestemming nodig om de machtiging. 

  > [!NOTE]
  > Een gedelegeerde machtigingen toe te voegen aan een toepassing verleent automatisch geen toestemming aan de gebruikers van de tenant. Gebruikers moeten nog steeds handmatig toestemming voor de toegevoegde gedelegeerde machtigingen tijdens runtime, tenzij de beheerder klikt op de **machtiging verlenen** knop van de **Required Permissions** sectie van de de pagina van de toepassing in de Azure portal. 

#### <a name="to-add-application-credentials-or-permissions-to-access-web-apis"></a>Referenties van de toepassing of machtigingen voor toegang tot web-API's toe te voegen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account biedt toegang tot meer dan één, klikt u op uw account in de rechterbovenhoek en stel uw portal-sessie op de gewenste Azure AD-tenant.
3. Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App registraties**, vervolgens zoeken/op de toepassing die u wilt configureren.

   ![Registratie van een toepassing bijwerken](./media/active-directory-integrating-applications/update-app-registration.png)

4. Gaat u naar de pagina belangrijkste registratie van de toepassing, die wordt geopend de **instellingen** pagina voor de toepassing. Een geheime sleutel om de referenties van uw webtoepassing toevoegen:
  - Klik op de **sleutels** sectie op de **instellingen** pagina.  
  - Een beschrijving voor uw sleutel toevoegen.
  - Selecteer een of twee jaar duur.
  - Klik op **Opslaan**. De meest rechtse kolom bevat de waarde van de sleutel, nadat u de wijzigingen in de configuratie hebt opgeslagen. **Zorg ervoor dat de sleutel wilt kopiëren** voor gebruik in uw toepassingscode client, zoals niet toegankelijk eenmaal is u deze pagina verlaten.

  ![Registratie van een toepassing - sleutels bijwerken](./media/active-directory-integrating-applications/update-app-registration-settings-keys.png)

5. Machtigingen voor toegang tot bronnen API's van de client toevoegen
  - Klik op de **Required Permissions** sectie op de **instellingen** pagina. 
  - Klik op de **toevoegen** knop.
  - Klik op **selecteert u een API** selecteren van het type bronnen die u verzamelen wilt uit.
  - Blader door de lijst met beschikbare API's of gebruik het zoekvak te selecteren in de beschikbare resource toepassingen in uw directory waarin een web-API. Klik op de resource die u geïnteresseerd bent in en klik vervolgens op **Selecteer**.
  - U hebt uitgevoerd om de **toegang inschakelen** pagina. Selecteer de toepassing worden machtigingen en/of gedelegeerde machtigingen die uw toepassing moet bij het openen van de API.
   
  ![Registratie van een toepassing - machtigingen bijwerken api](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-api.png)

  ![Registratie van een toepassing - machtigingen perms bijwerken](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms.png)

6. Wanneer u klaar bent, klikt u op de **Selecteer** knop op **toegang inschakelen** pagina wordt de **gedaan** knop op de **API toevoegen toegang** pagina. U keert terug naar de **vereist machtigingen** pagina, waar de nieuwe resource is toegevoegd aan de lijst met API's.

  > [!NOTE]
  > Te klikken op de **gedaan** knop stelt ook automatisch de machtigingen voor uw toepassing in uw directory op basis van de machtigingen voor andere toepassingen die u hebt geconfigureerd.  U kunt deze Toepassingsmachtigingen weergeven door te kijken naar de toepassing **instellingen** pagina.
  > 
  > 

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configureren van de toepassing van een resource te kunnen stellen van web-API 's

U kunt een web-API ontwikkelen en beschikbaar is voor clienttoepassingen maken bij het blootstellen van toegang [scopes](active-directory-dev-glossary.md#scopes) en [rollen](active-directory-dev-glossary.md#roles). Een juist geconfigureerde web-API wordt uitgevoerd net als de andere Microsoft-website API's, waaronder de Graph API en de Office 365-API's beschikbaar zijn. Toegangsbereiken en rollen worden weergegeven via uw [van toepassingsmanifest](active-directory-dev-glossary.md#application-manifest), dit is een JSON-bestand met de configuratie van de identiteit van uw toepassing. 

De volgende sectie leest u hoe toegangsbereiken, door het wijzigen van de resource toepassingsmanifest blootstellen.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Toegangsbereiken toe te voegen aan uw toepassing resource

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account biedt toegang tot meer dan één, klikt u op uw account in de rechterbovenhoek en stel uw portal-sessie op de gewenste Azure AD-tenant.

3. Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App registraties**, vervolgens zoeken/op de toepassing die u wilt configureren.

   ![Registratie van een toepassing bijwerken](./media/active-directory-integrating-applications/update-app-registration.png)

4. Gaat u naar de pagina belangrijkste registratie van de toepassing, die wordt geopend de **instellingen** pagina voor de toepassing. Overschakelen naar de **bewerken manifest** pagina door te klikken op **Manifest** van de registratiepagina van de toepassing. Een web gebaseerde manifest-editor wordt geopend, zodat u **bewerken** het manifest van de portal. Desgewenst kunt u **downloaden** en lokaal bewerken en vervolgens gebruik **uploaden** toe te passen aan uw toepassing.

5. In dit voorbeeld, geven we een nieuwe scope aangeroepen `Employees.Read.All` op onze resource/API door toe te voegen aan het volgende JSON-element de `oauth2Permissions` verzameling. De bestaande `user_impersonation` bereik wordt standaard opgegeven tijdens de registratie. `user_impersonation`Hiermee kunt een clienttoepassing om toestemming voor toegang tot de bron, onder de identiteit van de aangemelde gebruiker te vragen. Zorg ervoor dat de komma toe te voegen na de bestaande `user_impersonation` bereik van element en de waarden van de eigenschappen aan de behoeften van uw resource wijzigen. 

  ```json
  {
    "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
    "adminConsentDisplayName": "Read-only access to Employee records",
    "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
    "isEnabled": true,
    "type": "User",
    "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
    "userConsentDisplayName": "Read-only access to your Employee records",
    "value": "Employees.Read.All"
  }
  ```
  > [!NOTE]
  > De waarde 'id' moet worden gegenereerd met een hulpprogramma voor het genereren van GUID, zoals [guidgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) of via een programma. Een unieke id voor de scope is door de web-API beschikbaar gesteld. Zodra een client op de juiste wijze is geconfigureerd met machtigingen voor toegang tot uw web-API, is het een toegangstoken OAuth2.0 uitgegeven door Azure AD. Wanneer de clientaanroepen van de de web-API, dit geeft het toegangstoken dat het bereik (scp) heeft ingesteld claim de machtigingen die in de registratie van de toepassing wordt aangevraagd.
  >
  > Extra scopes later zo nodig kan worden blootgesteld. U kunt uw web-API kan meerdere scopes die zijn gekoppeld met tal van verschillende functies worden blootgesteld. De resource toegang tot de web-API tijdens runtime, beheren door te evalueren van het bereik (`scp`) claim (s) in de ontvangen toegangstoken van OAuth 2.0.
  > 

6. Wanneer u klaar bent, klikt u op **opslaan**. Uw web-API is nu geconfigureerd voor gebruik door andere toepassingen in uw directory.  

  ![Registratie van een toepassing bijwerken](./media/active-directory-integrating-applications/update-app-registration-manifest.png)

#### <a name="verify-the-web-api-is-exposed-to-other-applications-in-your-tenant"></a>Controleer of het web dat API wordt blootgesteld aan andere toepassingen in uw tenant
1. Ga terug naar uw Azure AD-tenant, klikt u op **App registraties** opnieuw in en zoeken/Klik de clienttoepassing die u wilt configureren.

   ![Registratie van een toepassing bijwerken](./media/active-directory-integrating-applications/update-app-registration.png)

2. Herhaal stap 5 net als bij [configureert een client voor toegang tot web-API's](#configure-a-client-application-to-access-web-apis). Wanneer de **selecteert u een API** stap en klik op zoeken voor uw resource door te voeren van de naam van de toepassing in het zoekveld **Selecteer**. 

3. Op de **toegang inschakelen** pagina ziet u de nieuwe scope en beschikbaar voor clientaanvragen machtiging.

  ![Nieuwe machtigingen worden weergegeven.](./media/active-directory-integrating-applications/update-app-registration-settings-permissions-perms-newscopes.png)

#### <a name="more-on-the-application-manifest"></a>Meer informatie over het toepassingsmanifest

Het toepassingsmanifest wordt daadwerkelijk fungeert als een mechanisme voor het bijwerken van de toepassing-entiteit waarmee alle kenmerken van een Azure AD-toepassing identiteit configuratie, met inbegrip van de API-toegangsbereiken besproken zijn gedefinieerd. Zie voor meer informatie over de entiteit van de toepassing en het schema is de [Graph API-toepassing entiteit documentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Het artikel bevat volledige naslaginformatie over de toepassing entiteitsleden gebruikt voor het opgeven van machtigingen voor uw API, met inbegrip van:  

- Het lid appRoles die een verzameling van [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) entiteiten, gebruikt om te definiëren [Toepassingsmachtigingen](active-directory-dev-glossary.md#permissions) voor een web-API. 
- Het lid oauth2Permissions die een verzameling van [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) entiteiten, gebruikt om te definiëren [overgedragen machtigingen](active-directory-dev-glossary.md#permissions) voor een web-API.

Voor meer informatie over application manifest concepten over het algemeen, Zie [inzicht in de Azure Active Directory-toepassingsmanifest](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Toegang tot de Azure AD Graph en Office 365 via Microsoft Graph API 's  

Zoals eerder gezegd blootstellen/hebt u toegang tot API's voor uw eigen toepassingen, kunt u uw clienttoepassing toegang krijgt tot de API's die worden weergegeven door de Microsoft-resources kunt registreren. De Microsoft Graph-API aangeduid als 'Microsoft Graph' in de portal-resource/API-lijst is beschikbaar voor alle toepassingen die zijn geregistreerd bij Azure AD. Als u de clienttoepassing in een tenant met accounts die zich hebben geregistreerd voor Office 365-abonnement registreert, kunt u ook toegang tot de bereiken die worden weergegeven door de verschillende bronnen voor Office 365.

Zie voor een volledige bespreking van de bereiken die worden weergegeven door Microsoft Graph API, de [-machtigingsbereiken | Microsoft Graph API concepten](https://graph.microsoft.io/docs/authorization/permission_scopes) artikel.

> [!NOTE]
> Vanwege een beperking, kunnen native client-toepassingen alleen aanroepen in de Azure AD Graph API als ze de machtiging 'Toegang tot de directory van uw organisatie' gebruiken. Deze beperking geldt niet voor webtoepassingen.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Multitenant-toepassingen configureren

Wanneer u registreert een toepassing in Azure AD, kunt u uw toepassing kan alleen worden geopend door gebruikers in uw organisatie. U kunt ook kunt u ook uw toepassing toegankelijk zijn voor gebruikers in externe organisaties. Deze twee toepassingstypen worden één tenant- en meerdere toepassingen genoemd. Deze sectie wordt beschreven hoe u de configuratie van een toepassing voor één tenant zodat het een multitenant-toepassing wijzigen.

Het is belangrijk te weten de verschillen tussen een één-tenant- en meerdere toepassing:  

- Een toepassing voor één tenant is bedoeld voor gebruik in een organisatie. Het is doorgaans een line-of-business (LoB)-toepassing geschreven door de ontwikkelaar van een onderneming. Een toepassing voor één tenant kan alleen worden benaderd door gebruikers met een account in dezelfde tenant als de toepassingsregistratie van de. Als gevolg hiervan, alleen moet worden ingericht in één directory.
- Een multitenant-toepassing is bedoeld voor gebruik in veel organisaties. Aangeduid als een software-as-a-service (SaaS)-toepassing, wordt doorgaans geschreven door een onafhankelijke softwareleverancier (ISV). Multitenant-toepassingen moeten worden ingericht in elke tenant waar gebruikers toegang moeten hebben. Voor tenants dan die waarop de toepassing is geregistreerd, is de gebruiker of beheerder toestemming nodig om u te registreren. Houd er rekening mee dat systeemeigen clienttoepassingen multitenant standaard zijn als ze zijn geïnstalleerd op de resource-eigenaar apparaat. Zie de voorgaande [overzicht van het framework toestemming](#overview-of-the-consent-framework) sectie voor meer informatie over het framework toestemming.

Maken van een multitenant toepassing vereist beide wijzigingen van de registratie van toepassing, evenals wijzigingen in de webtoepassing zelf. Beide betrekking hebben op de volgende secties.

#### <a name="changing-the-application-registration-to-support-multi-tenant"></a>Het wijzigen van de toepassingsregistratie van de voor ondersteuning van meerdere tenants

Als u een toepassing die u beschikbaar wilt maken voor uw klanten of partners buiten uw organisatie schrijft, moet u het bijwerken van de definitie van de toepassing in de Azure portal.

> [!IMPORTANT]
> Azure AD is vereist voor de App ID URI van multitenant-toepassingen voor globaal uniek zijn. De App ID URI is een van de manieren waarop die een toepassing in protocolberichten wordt geïdentificeerd. Voor een toepassing voor één tenant is het voldoende voor de URI van de App-ID moet uniek zijn binnen deze tenant. Voor een toepassing met meerdere tenants moet het wereldwijd uniek zodat Azure AD de toepassing op alle huurders kan vinden. Globale uniekheid wordt afgedwongen door de App ID URI in een hostnaam die overeenkomt met een geverifieerde domein voor de Azure AD-tenant. Bijvoorbeeld, als de naam van uw tenant contoso.onmicrosoft.com is zou vervolgens een geldige App ID URI zijn https://contoso.onmicrosoft.com/myapp. Als uw tenant een geverifieerd domein contoso.com heeft, zou klikt u vervolgens een geldig App ID URI ook zijn https://contoso.com/myapp. Als de App ID URI niet dit patroon volgen, instellen van een toepassing als multitenant is mislukt.
> 

Externe gebruikers toegang tot uw toepassing geven: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account biedt toegang tot meer dan één, klikt u op uw account in de rechterbovenhoek en stel uw portal-sessie op de gewenste Azure AD-tenant.
3. Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App registraties**, vervolgens zoeken/op de toepassing die u wilt configureren. Gaat u naar de pagina belangrijkste registratie van de toepassing, die wordt geopend de **instellingen** pagina voor de toepassing.
4. Van de **instellingen** pagina, klikt u op **eigenschappen** en wijzig de **Multi-verpachte** overschakelen naar **Ja**.

Nadat u de wijzigingen hebt aangebracht, kunnen gebruikers en beheerders van andere organisaties geven hun gebruikers de mogelijkheid aan te melden bij uw toepassing, zodat uw toepassing toegang krijgen tot bronnen die zijn beveiligd door de tenant.

#### <a name="changing-the-application-to-support-multi-tenant"></a>Het wijzigen van de toepassing voor de ondersteuning van meerdere tenants

Ondersteuning voor meerdere tenants toepassingen is sterk afhankelijk van het kader van Azure AD toestemming. Toestemming is het mechanisme dat toestaat dat een gebruiker van een andere tenant, de toepassing toegang geven tot bronnen die zijn beveiligd door de gebruiker tenant. Deze ervaring wordt aangeduid als "toestemming van de gebruiker."

De webtoepassing kan ook worden geboden:

- De mogelijkheid voor beheerders om te 'ondertekenen van mijn bedrijf'. Deze ervaring, aangeduid als 'admin toestemming', kan een beheerder de mogelijkheid om toegang te verlenen toestemming namens *alle gebruikers* binnen hun organisatie. Alleen een gebruiker die verifieert met een account dat bij de rol globale beheerder hoort kan bieden admin toestemming; andere gebruikers foutbericht een weergegeven.

- Een registratie ervaring voor gebruikers. Er wordt verwacht dat de gebruiker ontvangt een "registratie" knop waarmee de browser wordt doorgestuurd naar de Azure AD-OAuth2.0 `/authorize` eindpunt of een OpenID Connect `/userinfo` eindpunt. Deze eindpunten toe dat de toepassing voor informatie over de nieuwe gebruiker door de id_token te bekijken. Na de aanmelding fase de gebruiker een gevraagd toestemming te wordt weergegeven krijgt de [overzicht van het framework toestemming](#overview-of-the-consent-framework) sectie.

Zie voor meer informatie over de wijzigingen in de toepassing vereist ter ondersteuning van multi-verpachte toegang en sign-in/sign-up-to-date oplossingen:

- [Een Azure Active Directory-gebruiker aanmelden met behulp van het patroon voor multitenant-toepassingen](active-directory-devhowto-multi-tenant-overview.md)
- De lijst met [multitenant-codevoorbeelden](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant). 
- [Snelstartgids: Huisstijl naar de aanmeldingspagina in Azure AD toevoegen](../customize-branding.md)

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Inschakelen van OAuth verlenen 2.0 impliciete voor toepassingen met één pagina

Doorgaans structuur van één pagina van de toepassing (kuuroorden) met een JavaScript-zware front-end die in de browser, die de toepassing web-API aanroept back-end om uit te voeren van de zakelijke logica wordt uitgevoerd. Voor kuuroorden gehost in Azure AD, kunt u OAuth 2.0 impliciete Grant gebruiken voor het verifiëren van de gebruiker met Azure AD en het verkrijgen van een token dat u gebruiken kunt voor het aanroepen van de toepassing JavaScript-client naar de back-end-web-API beveiligen. 

Nadat de gebruiker heeft toestemming verleend, kan dit dezelfde authenticatieprotocol aanroepen tussen de client en andere web API-resources die zijn geconfigureerd voor de toepassing beveiligen-tokens verkrijgen worden gebruikt. Zie voor meer informatie over de impliciete authorization grant en kunt u bepalen of deze geschikt voor uw toepassingsscenario is, [inzicht in de impliciete OAuth2 stroom in Azure Active Directory verlenen](active-directory-dev-understanding-oauth2-implicit-grant.md).

Impliciete Grant OAuth 2.0 is standaard uitgeschakeld voor toepassingen. U kunt de impliciete OAuth 2.0-Grant voor uw toepassing inschakelen door in te stellen de `oauth2AllowImplicitFlow` waarde in de [toepassingsmanifest](active-directory-application-manifest.md).

#### <a name="to-enable-oauth-20-implicit-grant"></a>Impliciete OAuth 2.0-grant inschakelen

> [!NOTE]
> Voor meer informatie over het bewerken van het toepassingsmanifest Lees eerst de vorige sectie, [configureren van de toepassing van een resource te kunnen stellen web-API's](#configuring-a-resource-application-to-expose-web-apis).
>

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account biedt toegang tot meer dan één, klikt u op uw account in de rechterbovenhoek en stel uw portal-sessie op de gewenste Azure AD-tenant.
3. Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App registraties**, vervolgens zoeken/op de toepassing die u wilt configureren. Gaat u naar de pagina belangrijkste registratie van de toepassing, die wordt geopend de **instellingen** pagina voor de toepassing.
4. Overschakelen naar de **bewerken manifest** pagina door te klikken op **Manifest** van de registratiepagina van de toepassing. Een web gebaseerde manifest-editor wordt geopend, zodat u **bewerken** het manifest van de portal. Zoek en stel de waarde 'oauth2AllowImplicitFlow' op 'true'. Standaard is ingesteld op 'false'.
   
  ```json
  "oauth2AllowImplicitFlow": true,
  ```
5. Sla het bijgewerkte manifest. Wanneer opgeslagen, wordt uw web-API is nu geconfigureerd voor het gebruik van OAuth 2.0 impliciete Grant om gebruikers te verifiëren.

## <a name="removing-an-application"></a>Een toepassing verwijderen
Deze sectie wordt beschreven hoe een toepassing registreren verwijderen uit uw Azure AD-tenant.

### <a name="removing-an-application-authored-by-your-organization"></a>Verwijderen van een toepassing die is geschreven door uw organisatie
Toepassingen die uw organisatie is geregistreerd worden weergegeven onder de 'Mijn apps' filter op uw tenant belangrijkste 'App registraties' pagina. Deze toepassingen zijn die u handmatig geregistreerd via de Azure-portal of via een programma via PowerShell of de Graph API. Meer specifiek, worden ze weergegeven door een toepassing en Service-Principal object in uw tenant. Zie voor meer informatie [toepassingsobjecten en Service-Principal objecten](active-directory-application-objects.md).

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Een toepassing voor één tenant verwijderen uit uw directory
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account biedt toegang tot meer dan één, klikt u op uw account in de rechterbovenhoek en stel uw portal-sessie op de gewenste Azure AD-tenant.
3. Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App registraties**, vervolgens zoeken/op de toepassing die u wilt configureren. Gaat u naar de pagina belangrijkste registratie van de toepassing, die wordt geopend de **instellingen** pagina voor de toepassing.
4. Klik op de pagina van de toepassing de registratie van de belangrijkste **verwijderen**.
5. Klik op **Ja** in het bevestigingsbericht.

#### <a name="to-remove-a-multi-tenant-application-from-its-home-directory"></a>Een multitenant-toepassing verwijderen uit de oorspronkelijke directory
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account biedt toegang tot meer dan één, klikt u op uw account in de rechterbovenhoek en stel uw portal-sessie op de gewenste Azure AD-tenant.
3. Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App registraties**, vervolgens zoeken/op de toepassing die u wilt configureren. Gaat u naar de pagina belangrijkste registratie van de toepassing, die wordt geopend de **instellingen** pagina voor de toepassing.
4. Van de **instellingen** pagina **eigenschappen** en wijzig de **Multi-verpachte** overschakelen naar **Nee**, wijzigt u eerst uw toepassing Single-tenant, klikt u vervolgens op **opslaan**. De toepassing service SPN-objecten blijven in de tenants van alle organisaties die al toestemming hebt gegeven voor het.
5. Klik op de **verwijderen** knop van de belangrijkste registratiepagina van de toepassing.
6. Klik op **Ja** in het bevestigingsbericht.

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Verwijderen van een multitenant-toepassing geautoriseerd door een andere organisatie
Een subset van de toepassingen die worden weergegeven onder het filter 'Alle apps' (met uitzondering van de 'Mijn apps' registraties) zijn op uw tenant 'App registraties' hoofdpagina van multitenant-toepassingen. Deze toepassingen met meerdere tenants zijn van een andere tenant en in uw tenant zijn geregistreerd tijdens het proces toestemming in technische voorwaarden. Ze zijn meer specifiek, vertegenwoordigd door alleen een service principal-object in uw tenant, waarbij geen bijbehorende application-object. Zie voor meer informatie over de verschillen tussen de toepassing en service-principals [toepassing en service-principal objecten in Azure AD](active-directory-application-objects.md).

Als u wilt verwijderen een multitenant-toepassing toegang tot uw directory (na heeft toestemming verleend), moet de company administrator de service-principal verwijderen. De beheerder moet globale beheerderstoegang hebben en kunnen verwijderen via de Azure portal de [Azure AD PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) om toegang te verwijderen.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de werking van verificatie in Azure AD [verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md).
- Zie de [huisstijl richtlijnen voor geïntegreerde Apps](active-directory-branding-guidelines.md) voor tips over visual richtlijnen voor uw app.
- Zie voor meer informatie over de relatie tussen de toepassing en Service-Principal objecten van een toepassing [toepassingsobjecten en Service-Principal objecten](active-directory-application-objects.md).
- Zie voor meer informatie over de rol van de app-manifest wordt afgespeeld, [inzicht in de Azure Active Directory-toepassingsmanifest](active-directory-application-manifest.md)
- Zie de [verklarende woordenlijst voor Azure AD-ontwikkelaar](active-directory-dev-glossary.md) voor definities van de concepten voor ontwikkelaars van core Azure Active Directory (AD).
- Ga naar de [ontwikkelaarshandleiding Active Directory](active-directory-developers-guide.md) voor een overzicht van alle inhoud die relevant zijn voor ontwikkelaars.

