---
title: Toepassingen integreren met Azure Active Directory
description: Informatie over hoe u een toepassing bijwerkt in Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: e705bbbd08882f56020192a3b42c311e05bfa399
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191722"
---
# <a name="quickstart-update-an-application-in-azure-active-directory"></a>Quickstart: Een toepassing bijwerken in Azure Active Directory

Bedrijfsontwikkelaars en SaaS-providers (software-as-a-service) die toepassingen hebben geregistreerd bij Azure Active Directory (Azure AD) moeten mogelijk uw toepassingen configureren voor toegang tot andere resources, zoals web-API's, zodat deze beschikbaar zijn in andere organisaties, en meer.

In deze snelstart leert u de verschillende manieren waarop u uw toepassing kunt configureren of bijwerken om te voldoen aan uw eigen vereisten of behoeften en die van andere organisaties.

## <a name="prerequisites"></a>Vereisten

Als u aan de slag wilt gaan, moet u de volgende stappen hebben voltooid:

* Het overzicht van het [toestemmingsframework van Azure Active Directory](consent-framework.md) lezen. Een goed begrip hiervan is belangrijk bij het bouwen van toepassingen die moeten worden gebruikt door andere gebruikers of toepassingen.
* Een Azure Active Directory-tenant hebben waarvoor toepassingen zijn geregistreerd.
  * Als u nog geen tenant hebt, vindt u [hier informatie over het verkrijgen van een tenant](quickstart-create-new-tenant.md).
  * Als u geen apps hebt die zijn geregistreerd in uw tenant, [leest u hoe u een toepassing aan Azure Active Directory kunt toevoegen](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="configure-a-client-application-to-access-web-apis"></a>Een clienttoepassing configureren voor toegang tot web-API's

Voordat een web-/vertrouwelijke clienttoepassing kan deelnemen aan een werkstroom voor autorisatieverlening die verificatie (en het verkrijgen van een toegangstoken) vereist, moeten beveiligde referenties worden vastgesteld. De standaardmethode voor verificatie die wordt ondersteund door Azure Portal is de client-ID + geheime sleutel. In deze sectie worden de configuratiestappen behandeld die vereist zijn voor het verstrekken van de geheime sleutel met de referenties van uw client.

Voordat een client toegang krijgt tot een web-API die beschikbaar is gemaakt door een resourcetoepassing (zoals de Microsoft Graph API), zorgt het toestemmingsframework ervoor dat de client de vereiste machtiging verkrijgt, op basis van de machtigingen die zijn aangevraagd. Standaard kunnen alle toepassingen machtigingen kiezen uit **Azure Active Directory** (Graph API) en het klassieke Azure-implementatiemodel. De [Graph API-machtiging 'Aanmelden en gebruikersprofiel lezen'](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) is ook standaard geselecteerd. Als de client wordt geregistreerd in een tenant met accounts die zijn geabonneerd op Office 365, zijn web-API’s en machtigingen voor SharePoint en Exchange Online beschikbaar voor selectie. U kunt kiezen uit twee typen machtigingen voor elke gewenste web-API:

- Toepassingsmachtigingen: de clienttoepassing heeft rechtstreeks als zichzelf (geen gebruikerscontext) toegang nodig tot de web-API. Dit type machtiging vereist toegang van de beheerder en is ook niet beschikbaar voor systeemeigen clienttoepassingen.
- Gedelegeerde machtigingen: de clienttoepassing heeft toegang tot de web-API als de aangemelde gebruiker nodig, maar met toegang die wordt beperkt door de geselecteerde machtiging. Dit type machtiging kan worden verleend door een gebruiker tenzij de machtiging toestemming van de beheerder vereist.

  > [!NOTE]
  > Wanneer een gedelegeerde machtiging aan een toepassing wordt toegevoegd, wordt niet automatisch toestemming verleend aan de gebruikers binnen de tenant. Gebruikers moeten nog steeds handmatig toestemming geven voor de toegevoegde gedelegeerde machtigingen tijdens runtime, tenzij de beheerder toestemming namens alle gebruikers verleent.

### <a name="add-application-credentials-or-permissions-to-access-web-apis"></a>Toepassingsreferenties, of toepassingsmachtigingen, toevoegen voor toegang tot web-API's

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
3. Selecteer in het navigatiedeelvenster aan de linkerkant de **Azure Active Directory**-service en selecteer **App-registraties**. Zoek/selecteer vervolgens de toepassing die u wilt configureren.

   ![De registratie van een toepassing bijwerken](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

4. U gaat naar de hoofdregistratiepagina van de toepassing, die de pagina **Instellingen** voor de toepassing bevat. Ga als volgt te werk om een referentie voor uw webtoepassing toe te voegen:
  1. Selecteer de sectie **Sleutels** op de pagina **Instellingen**.
  2. Ga als volgt te werk om een certificaat toe te voegen:
    - Selecteer **Openbare sleutel uploaden**.
    - Selecteer het bestand dat u wilt uploaden. Dit moet een van de volgende bestandstypen zijn: .cer, .pem, .crt.
  - Ga als volgt te werk om een wachtwoord toe te voegen:
    - Voeg een beschrijving voor uw sleutel toe.
    - Selecteer een duur.
    - Selecteer **Opslaan**. De meest rechtse kolom bevat de waarde van de sleutel, nadat u de wijzigingen in de configuratie hebt opgeslagen. **Vergeet niet de sleutel te kopiëren** voor gebruik in de code van uw clienttoepassing. Deze is namelijk niet meer toegankelijk wanneer u deze pagina verlaat.

5. Machtiging(en) voor toegang tot resource-API's van uw client toevoegen
  1. Selecteer de sectie **Vereiste machtigingen** op de pagina **Instellingen** en selecteer vervolgens **Toevoegen**.
  1. Selecteer **Een API selecteren** om het type resources te selecteren waaruit u een keuze wilt maken.
  1. Blader door de lijst met beschikbare API's of gebruik het zoekvak om een selectie te maken uit de beschikbare resourcetoepassingen in uw directory die een web-API beschikbaar maken. Selecteer de resource waarin u geïnteresseerd bent in en klik vervolgens op **Selecteren**.
  1. Selecteer op de pagina **Toegang inschakelen** de toepassingsmachtigingen en/of gedelegeerde machtigingen die uw toepassing nodig heeft bij het openen van de API.
   
  ![De registratie van een toepassing bijwerken - machtigings-API](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-api.png)

  ![De registratie van een toepassing bijwerken - machtigings-perms](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms.png)

6. Selecteer wanneer u klaar bent de knop **Selecteren** op de pagina **Toegang inschakelen** en vervolgens de knop **Gereed** op de pagina **API-toegang toevoegen**. U keert terug naar de pagina **Vereiste machtigingen**, waar de nieuwe resource aan de lijst met API's is toegevoegd.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Een resourcetoepassing configureren voor het beschikbaar maken van web-API's

U kunt een web-API ontwikkelen en deze beschikbaar maken voor clienttoepassingen door toegangs[bereiken](developer-glossary.md#scopes) en -[rollen](developer-glossary.md#roles) beschikbaar te stellen. Een correct geconfigureerde web-API wordt net als de andere Microsoft web-API's beschikbaar gesteld, met inbegrip van de Graph API en de Office 365-API's. Toegangsbereiken en -rollen worden weergegeven via uw [toepassingsmanifest](developer-glossary.md#application-manifest). Dit is een JSON-bestand met de configuratie van de identiteit van uw toepassing.

In de volgende sectie leest u hoe u toegangsbereiken weergeeft door het wijzigen van het toepassingsmanifest van de resource.

### <a name="add-access-scopes-to-your-resource-application"></a>Toegangsbereiken toevoegen aan uw resourcetoepassing

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
3. Selecteer in het navigatiedeelvenster aan de linkerkant **Azure Active Directory > App-registraties**. Zoek/selecteer vervolgens de toepassing die u wilt configureren.

   ![De registratie van een toepassing bijwerken](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

4. U gaat naar de hoofdregistratiepagina van de toepassing, waarmee de pagina **Instellingen** voor de toepassing wordt geopend. Schakel over naar de pagina **Manifest bewerken** door te klikken op **Manifest** op de registratiepagina van de toepassing. Er wordt een webgebaseerde manifesteditor geopend, zodat u het manifest binnen de portal kunt **bewerken**. Klik desgewenst op **Downloaden** en bewerk het manifest lokaal. Gebruik daarna **Uploaden** om het opnieuw toe te passen op uw toepassing.
5. In dit voorbeeld geven we een nieuw bereik met de naam `Employees.Read.All` weer in onze resource/API door het volgende JSON-element aan de verzameling `oauth2Permissions` toe te voegen. Het bestaande `user_impersonation`-bereik wordt standaard opgegeven tijdens de registratie. `user_impersonation` stelt een clienttoepassing in staat om toestemming te vragen voor toegang tot de resource, onder de identiteit van de aangemelde gebruiker. Zorg ervoor dat u de komma toevoegt na het bestaande `user_impersonation`-bereikelement en wijzig de waarden van de eigenschappen in overeenstemming met de behoeften van uw resource. 

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
  > De waarde `id` moet worden gegenereerd via een programma of met behulp van een hulpprogramma voor het genereren van een GUID zoals [guidgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx). De `id` staat voor een unieke id voor het bereik zoals het wordt weergegeven door de web-API. Nadat een client op de juiste wijze is geconfigureerd met machtigingen voor toegang tot uw web-API, wordt er een OAuth 2.0-toegangstoken voor uitgegeven door Azure Active Directory. Wanneer de client de web-API aanroept, presenteert deze het toegangstoken waarvan de bereikclaim (scp is) is ingesteld op de machtigingen die zijn aangevraagd in de registratie van de toepassing.
  >
  > U kunt aanvullende bereiken indien nodig later weergeven. Houd er rekening mee dat uw web-API mogelijk meerdere bereiken weergeeft die zijn gekoppeld aan een verscheidenheid van verschillende functies. Uw resource kan toegang tot de web-API tijdens runtime beheren, door de bereikclaim(s) (`scp`) te evalueren in het ontvangen OAuth 2.0-toegangstoken.

6. Klik op **Opslaan** als u klaar bent. Uw web-API is nu geconfigureerd voor gebruik door andere toepassingen in uw directory.

  ![De registratie van een toepassing bijwerken](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-manifest.png)

### <a name="verify-the-web-api-is-exposed-to-other-applications-in-your-tenant"></a>Controleren of de web-API beschikbaar wordt gemaakt voor andere toepassingen in uw tenant

1. Ga terug naar uw Azure Active Directory-tenant, selecteer opnieuw **App-registraties** en zoek/selecteer de clienttoepassing die u wilt configureren.

   ![De registratie van een toepassing bijwerken](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

2. Herhaal stap 5 op dezelfde manier als in [Een clienttoepassing configureren voor toegang tot web-API's](#configure-a-client-application-to-access-web-apis). Wanneer u bij de stap **Een API selecteren** bent, zoekt u uw resource op door de toepassingsnaam in het zoekveld in te voeren en op **Selecteren** te klikken.

3. Op de pagina **Toegang inschakelen** ziet u het nieuwe bereik, dat beschikbaar is voor machtigingsaanvragen voor clients.

  ![Nieuwe machtigingen worden weergegeven](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms-newscopes.png)

### <a name="more-on-the-application-manifest"></a>Meer informatie over het toepassingsmanifest

Het toepassingsmanifest fungeert als een mechanisme voor het bijwerken van de toepassingsentiteit, dat alle kenmerken van de configuratie van een Azure Active Directory-toepassingsidentiteit definieert, met inbegrip van de API-toegangsbereiken die zijn besproken. Zie de [documentatie over de toepassingsidentiteit van de Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity) voor meer informatie over de toepassingentiteit en het bijbehorende schema. Het artikel bevat volledige naslaginformatie over de toepassingsentiteitsleden die worden gebruikt voor het opgeven van machtigingen voor uw API, met inbegrip van:  

- Het lid appRoles, dat een verzameling van [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type)-entiteiten is, dat wordt gebruikt voor het definiëren van [toepassingsmachtigingen](developer-glossary.md#permissions) voor een web-API. 
- Het lid oauth2Permissions, dat een verzameling van [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type)-entiteiten is, dat wordt gebruikt voor het definiëren van [gedelegeerde machtigingen](developer-glossary.md#permissions) voor een web-API.

Zie [Azure Active Directory-toepassingsmanifest](reference-app-manifest.md) voor meer informatie over toepassingsmanifestconcepten in het algemeen.

## <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Toegang tot de Azure AD Graph en Office 365 via Microsoft Graph API's  

Zoals eerder aangegeven, kunt u naast het weergeven/openen van API's voor uw eigen toepassingen, uw clienttoepassing registreren voor toegang tot API's die worden weergegeven door Microsoft-resources. De Microsoft Graph API, aangeduid als 'Microsoft Graph' in de resource/API-lijst van de portal, is beschikbaar voor alle toepassingen die zijn geregistreerd bij Azure Active Directory. Als u uw clienttoepassing registreert in een tenant met accounts die zich hebben geregistreerd voor een Office 365-abonnement, hebt u ook toegang tot de bereiken die worden weergegeven door de verschillende Office 365-resources.

Zie het naslagartikel over [Microsoft Graph-machtigingen](https://docs.microsoft.com/graph/permissions-reference) voor een volledige bespreking van de bereiken die worden weergegeven door Microsoft Graph API.

> [!NOTE]
> Vanwege een huidige beperking kunnen systeemeigen clienttoepassingen de Graph API van Azure Active Directory alleen aanroepen als ze de machtiging 'Toegang tot de adreslijst van uw organisatie' gebruiken. Deze beperking geldt niet voor webtoepassingen.

## <a name="configuring-multi-tenant-applications"></a>Apps voor meerdere tenants configureren

Bij het registreren van een toepassing in Azure Active Directory, wilt u mogelijk dat uw toepassing alleen kan worden geopend door gebruikers in uw organisatie. Het kan ook zijn dat u wilt dat uw toepassing kan worden geopend door gebruikers in externe organisaties. Deze twee typen toepassingen worden één tenant en multitenant genoemd. In deze sectie wordt besproken hoe u de configuratie van een toepassing met één tenant kunt wijzigen in een multitenant toepassing.

Het is belangrijk de verschillen te weten tussen een toepassing met één tenant en een multitenant toepassing:  

- Een toepassing met één tenant is bedoeld voor gebruik in één organisatie. Deze is doorgaans een LOB-toepassing (Line-of-business) die is geschreven door een bedrijfsontwikkelaar. Een toepassing met één tenant kan alleen worden geopend door gebruikers met een account in dezelfde tenant als de toepassingsregistratie. Als gevolg hiervan hoeft deze maar in één directory te worden ingericht.
- Een multitenant toepassing is bedoeld voor gebruik in veel organisaties. Deze wordt ook wel aangeduid als een SaaS-toepassing (software-as-a-service) en wordt doorgaans geschreven door een onafhankelijke softwareleverancier. Multitenant toepassingen moeten worden ingericht in elke tenant waar gebruikers toegang nodig hebben. Voor andere tenants dan de regio waarin de toepassing is geregistreerd, is toestemming van de gebruiker of beheerder nodig om deze toepassingen te registreren. Houd er rekening mee dat systeemeigen clienttoepassingen standaard multitenant zijn omdat ze op het apparaat van de resource-eigenaar zijn geïnstalleerd. Zie de voorgaande sectie Overzicht van het toestemmingsframework voor meer informatie over het toestemmingsframework.

Het multitenant maken van een toepassing vereist zowel wijzigingen in de toepassingsregistratie als wijzigingen in de web-app zelf. In de volgende secties komen beide aan de orde.

### <a name="changing-the-application-registration-to-support-multi-tenant"></a>De toepassingsregistratie wijzigen voor ondersteuning van multitenant

Als u een toepassing ontwikkelt die u beschikbaar wilt maken voor uw klanten of partners buiten uw organisatie, moet u de definitie van de toepassing bijwerken in Azure Portal.

> [!IMPORTANT]
> Azure AD vereist dat de URI van de app-id van multitenant toepassingen wereldwijd uniek is. De URI van de app-id is een van de manieren waarop een toepassing wordt geïdentificeerd in protocolberichten. Voor een toepassing met één tenant is het voldoende dat de URI van de app-id uniek is binnen die tenant. Voor een multitenant toepassing moet deze wereldwijd uniek zijn, zodat Azure Active Directory de toepassing in alle tenants kan vinden.
> Wereldwijde uniekheid wordt afgedwongen door te vereisen dat de URI van de app-id een hostnaam heeft die overeenkomt met een geverifieerd domein van de Azure Active Directory-tenant. Als de naam van uw tenant bijvoorbeeld contoso.onmicrosoft.com is, zou https://contoso.onmicrosoft.com/myapp een geldige URI voor de app-id zijn. Als uw tenant een geverifieerd domein van contoso.com heeft, zou https://contoso.com/myapp ook een geldige URI voor de app-id zijn. Als de URI van de app-id dit patroon niet volgt, mislukt het instellen van een multitenant toepassing.

Ga als volgt te werk om externe gebruikers toegang tot uw toepassing te bieden:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als u via uw account toegang tot meer dan één tenant hebt, klikt u op uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
3. Klik in het navigatiedeelvenster aan de linkerkant op de **Azure Active Directory**-service en klik op **App-registraties**. Zoek/selecteer vervolgens de toepassing die u wilt configureren. U gaat naar de hoofdregistratiepagina van de toepassing, waarmee de pagina **Instellingen** voor de toepassing wordt geopend.
4. Klik op de pagina **Instellingen** op **Eigenschappen** en wijzig de schakeloptie **Met meerdere tenants** in **Ja**.

Nadat u de wijzigingen hebt aangebracht, kunnen gebruikers en beheerders van andere organisaties hun gebruikers de mogelijkheid bieden om zich te melden bij uw toepassing, en heeft uw toepassing toegang tot resources die worden beveiligd door hun tenant.

### <a name="changing-the-application-to-support-multi-tenant"></a>De toepassing wijzigen voor ondersteuning van multitenant

Ondersteuning voor toepassingen met meerdere tenants is sterk afhankelijk van het toestemmingsframework van Azure Active Directory. Toestemming is het mechanisme waarmee een gebruiker van een andere tenant de toepassing toegang kan toestaan tot resources die worden beveiligd door de tenant van de gebruiker. Dit wordt aangeduid als 'gebruikerstoestemming'.

Uw webtoepassing biedt mogelijk ook:

- De mogelijkheid voor beheerders om 'mijn bedrijf aan te melden'. Dit, dat wordt aangeduid als 'beheerderstoestemming', biedt een beheerder de mogelijkheid toestemming te verlenen namens *alle gebruikers* in de betreffende organisatie. Alleen een gebruiker die verifieert met een account dat deel uitmaakt van de rol van Globale beheerder kan beheerderstoestemming geven; voor andere gebruikers wordt een foutbericht weergegeven.
- Een aanmeldingsmogelijkheid voor gebruikers. Er wordt verwacht dat de gebruiker de knop 'Aanmelden' te zien krijgt waarmee de browser wordt omgeleid naar het Azure Active Directory OAuth 2.0 `/authorize`-eindpunt of een OpenID Connect `/userinfo`-eindpunt. Met behulp van deze eindpunten kan de toepassing informatie verkrijgen over de nieuwe gebruiker door de id_token te inspecteren. Na de aanmeldingsfase krijgt de gebruiker een toestemmingsprompt te zien, die vergelijkbaar is met degene die in de sectie Overzicht van het toestemmingsframework wordt getoond.

Zie voor meer informatie over de toepassingswijzigingen die vereist zijn voor de ondersteuning van multitenant aanmeldings-/registratiemogelijkheden:

- [Een Azure Active Directory-gebruiker aanmelden met behulp van het patroon voor multitenant-toepassingen](howto-convert-app-to-be-multi-tenant.md)
- De lijst met [multitenant codevoorbeelden](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant).
- [Snelstart: Huisstijl toevoegen aan uw aanmeldingspagina in Azure Active Directory](../fundamentals/customize-branding.md)

## <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Impliciete goedkeuring voor OAuth 2.0 inschakelen voor toepassingen met één pagina

Toepassingen met één pagina (SPA's) zijn doorgaans geordend met een front-end met veel JavaScript die wordt uitgevoerd in de browser, die de back-end van de web-API van de toepassing aanroept om de bedrijfslogica uit te voeren. Voor SPA's die worden gehost in Azure Active Directory gebruikt u impliciete goedkeuring voor OAuth 2.0 om de gebruiker bij Azure Active Directory te verifiëren en een token te verkrijgen dat u kunt gebruiken om aanroepen van de JavaScript-client van de toepassing naar de web-API van de back-end te beveiligen.

Nadat de gebruiker toestemming heeft verleend, kan ditzelfde verificatieprotocol worden gebruikt om tokens op te halen voor het beveiligen van aanroepen tussen de client en andere web-API-resources die voor de toepassing zijn geconfigureerd. Zie [Inzicht in de impliciete goedkeuringsstroom voor OAuth2 in Azure Active Directory verlenen](v1-oauth2-implicit-grant-flow.md) voor meer informatie over de impliciete goedkeuringsstroom en om te bepalen of dit geschikt is voor uw toepassingsscenario.

Impliciete goedkeuring voor OAuth 2.0 is standaard uitgeschakeld voor toepassingen. U kunt de impliciete toekenning van OAuth 2.0 voor uw toepassing inschakelen door de waarde `oauth2AllowImplicitFlow` in het bijbehorende [toepassingsmanifest](reference-app-manifest.md) in te stellen.

### <a name="to-enable-oauth-20-implicit-grant"></a>Impliciete goedkeuring voor OAuth 2.0 inschakelen

> [!NOTE]
> Voor meer informatie over het bewerken van het manifest van de toepassing dient u eerst de vorige sectie, [Een resourcetoepassing configureren voor het beschikbaar maken van web-API's](#configuring-a-resource-application-to-expose-web-apis), te hebben gelezen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als u via uw account toegang tot meer dan één tenant hebt, klikt u op uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
3. Klik in het navigatiedeelvenster aan de linkerkant op de **Azure Active Directory**-service en klik op **App-registraties**. Zoek/selecteer vervolgens de toepassing die u wilt configureren. U gaat naar de hoofdregistratiepagina van de toepassing, waarmee de pagina **Instellingen** voor de toepassing wordt geopend.
4. Schakel over naar de pagina **Manifest bewerken** door te klikken op **Manifest** op de registratiepagina van de toepassing. Er wordt een webgebaseerde manifesteditor geopend, zodat u het manifest binnen de portal kunt **bewerken**. Zoek de waarde 'oauth2AllowImplicitFlow' op en stel deze in op 'true'. Standaard is deze ingesteld op 'false'.
   
  ```json
  "oauth2AllowImplicitFlow": true,
  ```
5. Sla het bijgewerkte manifest op. Zodra het is opgeslagen, is uw web-API geconfigureerd voor het gebruik van impliciete goedkeuring voor OAuth 2.0 om gebruikers te verifiëren.

## <a name="next-steps"></a>Volgende stappen

Lees meer over andere gerelateerde snelstarts voor app-beheer met behulp van het Azure Active Directory v1.0-eindpunt:
- [Een toepassing toevoegen aan Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Een toepassing verwijderen uit Azure Active Directory](quickstart-v1-remove-azure-ad-app.md)

Zie [Toepassingsobjecten en service-principal-objecten](app-objects-and-service-principals.md) voor meer informatie over de twee Azure Active Directory-objecten die een geregistreerde toepassing vertegenwoordigen en de relatie ertussen.

Zie [Huisstijlrichtlijnen voor apps](howto-add-branding-in-azure-ad-apps.md) voor meer informatie over de huisstijlrichtlijnen die u moet gebruiken bij het ontwikkelen van toepassingen met Azure Active Directory.
