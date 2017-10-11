---
title: Beveiligen van een Web-API-back-end met Azure Active Directory en API Management | Microsoft Docs
description: Informatie over het beveiligen van een Web-API-back-end met Azure Active Directory en API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: f856ff03-64a1-4548-9ec4-c0ec4cc1600f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 0dfb4102904c2e972e6617fd3851fb1c50147357
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Het beveiligen van een Web-API-back-end met Azure Active Directory en API Management
De volgende video ziet u hoe het bouwen van een Web-API-back-end en beveiligen met behulp van OAuth 2.0-protocol met Azure Active Directory en API Management.  Dit artikel bevat een overzicht en aanvullende informatie voor de stappen in de video. Deze 24 minuut video ziet u hoe aan:

* Een Web-API-back-end bouwen en beveilig deze met AAD - begint bij 1:30
* De API in API Management - 7:10 vanaf importeren
* De portal voor ontwikkelaars voor het aanroepen van de API - vanaf 9:09 configureren
* Configureer een bureaubladtoepassing het API - vanaf 18:08 aan te roepen
* Configureer een beleid voor de validatie van JWT vooraf om aanvragen te verifiëren - 20:47 vanaf

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a>Een Azure AD-directory maken
Voor het beveiligen van uw Web-API ondersteund met Azure Active Directory moet u eerst hebben een AAD-tenant. In deze video met de naam van een tenant **APIMDemo** wordt gebruikt. Voor het maken van een AAD-tenant, aanmelding bij de [klassieke Azure-Portal](https://manage.windowsazure.com) en klik op **nieuw**->**App Services**->**Active Directory**  -> **Directory**->**aangepast maken**. 

![Azure Active Directory][api-management-create-aad-menu]

In dit voorbeeld van een map met de naam **APIMDemo** wordt gemaakt met een standaarddomein met de naam **DemoAPIM.onmicrosoft.com**. Deze map wordt gebruikt in de video.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Een Web-API-service worden beveiligd door Azure Active Directory maken
In deze stap wordt een Web-API-back-end gemaakt met behulp van Visual Studio 2013. In deze stap van de video begint bij 1:30. Voor het maken van back-end-Web API-project in Visual Studio klikt u op **bestand**->**nieuw**->**Project**, en kies **ASP.NET-webtoepassing Toepassing** van de **Web** lijst met sjablonen. In deze video van het project de naam **APIMAADDemo**. Klik op **OK** om het project te maken. 

![Visual Studio][api-management-new-web-app]

Klik op **Web API** van de **selecteert u een lijst met sjablonen** een Web-API-project maken. Klik op Azure Directory-verificatie configureren **verificatie wijzigen**.

![Nieuw project][api-management-new-project]

Klik op **Organisatieaccounts**, en geef de **domein** van uw AAD-tenant. In dit voorbeeld voor het domein is **DemoAPIM.onmicrosoft.com**. Het domein van uw directory kan worden verkregen van de **domeinen** tabblad van uw directory.

![Domeinen][api-management-aad-domains]

Configureer de gewenste instellingen in de **verificatie wijzigen** dialoogvenster en klik op **OK**.

![Verificatie wijzigen][api-management-change-authentication]

Wanneer u klikt op **OK** Visual Studio probeert te registreren van uw toepassing bij uw Azure AD-directory en wordt u mogelijk gevraagd zich aanmelden door Visual Studio. Meld u aan met een Administrator-account voor uw directory.

![Aanmelden bij Visual Studio][api-management-sign-in-vidual-studio]

Voor het configureren van dit project als een Azure-Web-API Schakel het selectievakje voor **Host in de cloud** en klik vervolgens op **OK**.

![Nieuw project][api-management-new-project-cloud]

U wordt mogelijk gevraagd om aan te melden bij Azure en vervolgens kunt u de Web-App configureren.

![Configureren][api-management-configure-web-app]

In dit voorbeeld is een nieuwe **App Service-abonnement** met de naam **APIMAADDemo** is opgegeven.

Klik op **OK** voor het configureren van de Web-App en het project maken.

## <a name="add-the-code-to-the-web-api-project"></a>Voeg de code toe aan de Web-API-project
De code van de volgende stap in de video wordt toegevoegd aan de Web-API-project. Deze stap 4:35 wordt gestart.

De Web-API in dit voorbeeld implementeert een basisrekenmachine-service met behulp van een model en een domeincontroller. Als u wilt toevoegen in het model voor de service, met de rechtermuisknop op **modellen** in **Solution Explorer** en kies **toevoegen**, **klasse**. Naam van de klasse `CalcInput` en klik op **toevoegen**.

Voeg de volgende `using` instructie boven aan de `CalcInput.cs` bestand.

```c#
using Newtonsoft.Json;
```

De gegenereerde klasse vervangen door de volgende code.

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

Met de rechtermuisknop op **domeincontrollers** in **Solution Explorer** en kies **toevoegen**->**Controller**. Kies **Web API 2-Controller - leeg** en klik op **toevoegen**. Type **CalcController** naam voor de Controller en klik op **toevoegen**.

![Controller toevoegen][api-management-add-controller]

Voeg de volgende `using` instructie boven aan de `CalcController.cs` bestand.

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

De gegenereerde controllerklasse vervangen door de volgende code. Deze code implementeert de `Add`, `Subtract`, `Multiply`, en `Divide` bewerkingen van de basisrekenmachine-API.

```c#
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

Druk op **F6** te controleren of de oplossing.

## <a name="publish-the-project-to-azure"></a>Het project naar Azure publiceren
In deze stap van de Visual Studio is project gepubliceerd naar Azure. In deze stap van de video begint bij 5:45.

Voor het publiceren van het project naar Azure, met de rechtermuisknop op de **APIMAADDemo** in Visual Studio-project en kies **publiceren**. Behoud de standaardinstellingen de **webpublicatie** dialoogvenster en klik op **publiceren**.

![Web publiceren][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Machtigingen toekennen voor de servicetoepassing voor back-end van Azure AD
Een nieuwe aanvraag om de back-endservice wordt gemaakt in uw Azure AD-directory als onderdeel van het configureren en publiceren van uw Web-API-project. In deze stap van de video, beginnend bij 6:13 worden machtigingen verleend aan de Web-API-back-end.

![Toepassing][api-management-aad-backend-app]

Klik op de naam van de toepassing om de vereiste machtigingen te configureren. Navigeer naar de **configureren** tabblad en schuif omlaag naar de **machtigingen voor andere toepassingen** sectie. Klik op de **Toepassingsmachtigingen** -omlaag naast **Windows** **Azure Active Directory**, schakel het selectievakje voor **mapgegevens lezen** , en klik op **opslaan**.

![Machtigingen toevoegen][api-management-aad-add-permissions]

> [!NOTE]
> Als **Windows** **Azure Active Directory** wordt niet vermeld in de machtigingen voor andere toepassingen, klikt u op **toepassing toevoegen** en toe te voegen in de lijst.
> 
> 

Noteer de **App Id URI** voor gebruik in een latere stap wanneer een Azure AD-toepassing is geconfigureerd voor de API Management-portal voor ontwikkelaars.

![App Id URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>De Web-API importeren in API Management
API's worden geconfigureerd in de API-publicatieportal, die wordt geopend via de Azure-Portal. Als u wilt openen, klikt u op **publicatieportal** van de werkbalk van uw API Management-service. Als u nog geen exemplaar van API Management-service hebt gemaakt, raadpleegt u [API Management service-exemplaar maken] [ Create an API Management service instance] in de [uw eerste API beheren] [ Manage your first API] zelfstudie.

![Publicatieportal][api-management-management-console]

Bewerkingen kunnen worden [toegevoegd aan de API's handmatig](api-management-howto-add-operations.md), of ze kunnen worden geïmporteerd. In deze video worden bewerkingen in Swagger-indeling vanaf 6:40 geïmporteerd.

Maak een bestand met de naam `calcapi.json` met de volgende inhoud en sla deze op uw computer. Zorg ervoor dat de `host` punten kenmerk back-end van uw Web-API. In dit voorbeeld `"host": "apimaaddemo.azurewebsites.net"` wordt gebruikt.

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

Klik voor het importeren van de rekenmachine-API op **API's** in het menu **API Management** aan de linkerkant en klik vervolgens op **API importeren**.

![Knop API importeren][api-management-import-api]

Voer de volgende stappen uit voor het configureren van de Rekenmachine-API.

1. Klik op **uit bestand**, blader naar de `calculator.json` bestand dat u hebt opgeslagen en op de **Swagger** keuzerondje.
2. Type **calc** in de **achtervoegsel URL Web-API** textbox.
3. Klik in het vak **Producten (optioneel)** en kies **Starter**.
4. Klik op **Opslaan** om de API te importeren.

![Nieuwe API toevoegen][api-management-import-new-api]

Zodra de API is geïmporteerd, wordt de overzichtspagina voor de API weergegeven in de publicatieportal.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>De API succes aanroepen vanuit de portal voor ontwikkelaars
Op dit moment de API is geïmporteerd in API Management, maar kan nog worden aangeroepen met succes vanuit de ontwikkelaarsportal omdat de back-endservice is beveiligd met Azure AD-verificatie. Dit wordt geïllustreerd in de video vanaf 7:40 met behulp van de volgende stappen uit.

Klik op **ontwikkelaarsportal** uit de rechts bovenaan kant van de publicatieportal.

![ontwikkelaarsportal][api-management-developer-portal-menu]

Klik op **API's** en klik op de **Rekenmachine** API.

![ontwikkelaarsportal][api-management-dev-portal-apis]

Klik op **Try it**.

![Probeer het nu][api-management-dev-portal-try-it]

Klik op **verzenden** en noteer de antwoordstatus van **401 niet geautoriseerd**.

![Verzenden][api-management-dev-portal-send-401]

De aanvraag is niet geautoriseerd, omdat de back-end API is beveiligd door Azure Active Directory. Voordat het is de API aanroept de ontwikkelaar moet de portal worden geconfigureerd voor het autoriseren van ontwikkelaars met behulp van OAuth 2.0. Dit proces wordt beschreven in de volgende secties.

## <a name="register-the-developer-portal-as-an-aad-application"></a>De portal voor ontwikkelaars registreren als een AAD-toepassing
De eerste stap bij het configureren van de portal voor ontwikkelaars om te verifiëren met behulp van OAuth 2.0 ontwikkelaars is de portal voor ontwikkelaars registreren als een AAD-toepassing. Dit wordt geïllustreerd 8:27 in de video wordt gestart.

Navigeer naar de Azure AD-tenant van de eerste stap in deze video in dit voorbeeld **APIMDemo** en navigeer naar de **toepassingen** tabblad.

![nieuwe toepassing][api-management-aad-new-application-devportal]

Klik op de **toevoegen** klikken om een nieuwe Azure Active Directory-toepassing maken en kies **mijn organisatie ontwikkelt toepassing toevoegen**.

![nieuwe toepassing][api-management-new-aad-application-menu]

Kies **Web-toepassing en/of Web-API**, voer een naam in en klik op de pijl Volgende. In dit voorbeeld **APIMDeveloperPortal** wordt gebruikt.

![nieuwe toepassing][api-management-aad-new-application-devportal-1]

Voor **aanmeldings-URL** Voer de URL van uw API Management-service en toevoeg- `/signin`. In dit voorbeeld `https://contoso5.portal.azure-api.net/signin` wordt gebruikt.

Voor **App-Id-URL** Voer de URL van uw API Management-service en het toevoegen van enkele unieke tekens. Deze mag gewenste tekens lang zijn en in dit voorbeeld `https://contoso5.portal.azure-api.net/dp` wordt gebruikt. Wanneer de gewenste **App-eigenschappen** zijn geconfigureerd, klikt u op het vinkje om de toepassing te maken.

![nieuwe toepassing][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Een API Management OAuth 2.0-autorisatie-server configureren
De volgende stap is het configureren van een OAuth 2.0-autorisatie-server in API Management. Deze stap wordt geïllustreerd in de video op 9:43 wordt gestart.

Klik op **beveiliging** in het menu API Management aan de linkerkant, klikt u op **OAuth 2.0**, en klik vervolgens op **toevoegen autorisatie** server.

![Autorisatie-server toevoegen][api-management-add-authorization-server]

Voer een naam en een optionele beschrijving in de **naam** en **beschrijving** velden. Deze velden worden gebruikt voor de server van de autorisatie OAuth 2.0 binnen het exemplaar van API Management-service identificeren. In dit voorbeeld **autorisatie server demo** wordt gebruikt. Later wanneer u een OAuth 2.0-server moet worden gebruikt voor verificatie voor een API opgeeft, selecteert u deze naam.

Voor de **Client de registratie van URL** Voer een aanduidingswaarde van de tijdelijke zoals `http://localhost`.  De **Client de registratie van URL** verwijst naar de pagina die gebruikers gebruiken kunnen voor het maken en hun eigen accounts configureren voor OAuth 2.0-providers die ondersteuning bieden voor gebruikersbeheer van accounts. In dit voorbeeld gebruikers niet maken en hun eigen accounts configureren zodat een tijdelijke aanduiding wordt gebruikt.

![Autorisatie-server toevoegen][api-management-add-authorization-server-1]

Geef vervolgens **autorisatie eindpunt-URL** en **Token eindpunt-URL**.

![Autorisatie-server][api-management-add-authorization-server-1a]

Deze waarden kunnen worden opgehaald uit de **App eindpunten** pagina van de AAD-toepassing die u hebt gemaakt voor de portal voor ontwikkelaars. Voor toegang tot de eindpunten, navigeer naar de **configureren** tabblad voor de AAD-toepassing en klik op **eindpunten weergeven**.

![Toepassing][api-management-aad-devportal-application]

![Eindpunten weergeven][api-management-aad-view-endpoints]

Kopieer de **OAuth 2.0-eindpunt voor autorisatie** en plak deze in de **autorisatie eindpunt-URL** textbox.

![Autorisatie-server toevoegen][api-management-add-authorization-server-2]

Kopieer de **OAuth 2.0-tokeneindpunt** en plak deze in de **Token eindpunt-URL** textbox.

![Autorisatie-server toevoegen][api-management-add-authorization-server-2a]

Naast plakken in het token eindpunt toevoegen een extra inhoudparameter met de naam **resource** en voor het gebruik van de waarde de **App Id URI** van de AAD-toepassing voor de back endservice die is gemaakt wanneer de Visual Studio-project is gepubliceerd.

![App Id URI][api-management-aad-sso-uri]

Geef vervolgens de referenties van de client. Dit zijn de referenties voor de resource die u wilt openen, in dit geval de portal voor ontwikkelaars.

![Clientreferenties][api-management-client-credentials]

Ophalen van de **Client-Id**, gaat u naar de **configureren** tabblad van de AAD-toepassing voor het portal voor ontwikkelaars en kopieer de **Client-Id**.

Ophalen van de **Clientgeheim** klikt u op de **Selecteer duur** omlaag in de **sleutels** sectie en geeft u een interval. In dit voorbeeld wordt 1 jaar gebruikt.

![Client-ID][api-management-aad-client-id]

Klik op **opslaan** de configuratie op te slaan en de sleutel wordt weergegeven. 

> [!IMPORTANT]
> Noteer deze sleutel. Wanneer u het venster Azure Active Directory-configuratie hebt gesloten, wordt de sleutel kan niet opnieuw weergegeven.
> 
> 

De sleutel naar het Klembord kopiëren, gaat u terug naar de publicatieportal, plak de sleutel in de **Clientgeheim** tekstvak en klik op **opslaan**.

![Autorisatie-server toevoegen][api-management-add-authorization-server-3]

Direct na de referenties van de client is een code machtiging grant. Kopieer deze autorisatiecode en keert u terug naar uw Azure AD portal developer-toepassing-pagina configureren en plak de machtiging grant in de **antwoord-URL** veld en klikt u op **opslaan** opnieuw.

![Antwoord-URL][api-management-aad-reply-url]

De volgende stap is om de machtigingen voor de portal voor ontwikkelaars AAD-toepassing te configureren. Klik op **Toepassingsmachtigingen** en schakel het selectievakje voor **mapgegevens lezen**. Klik op **opslaan** deze wijziging opslaat, en klik vervolgens op **toepassing toevoegen**.

![Machtigingen toevoegen][api-management-add-devportal-permissions]

Klik op het zoekpictogram type **APIM** selecteren in de start met het selectievakje **APIMAADDemo**, en klik op het vinkje om op te slaan.

![Machtigingen toevoegen][api-management-aad-add-app-permissions]

Klik op **gedelegeerde machtigingen** voor **APIMAADDemo** en schakel het selectievakje voor **toegang APIMAADDemo**, en klik op **opslaan**. Hierdoor kan de ontwikkelaar van de bedrijfsportal-App voor toegang tot de back-endservice.

![Machtigingen toevoegen][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Inschakelen van OAuth 2.0-gebruikersautorisatie voor de basisrekenmachine-API
Nu dat het OAuth 2.0-server is geconfigureerd, kunt u deze kunt opgeven in de beveiligingsinstellingen voor uw API. Deze stap wordt geïllustreerd in de video vanaf 14:30.

Klik op **API's** in het menu aan de linkerkant en klik op **Rekenmachine** weergeven en configureren van de instellingen ervan.

![Rekenmachine-API][api-management-calc-api]

Navigeer naar de **beveiliging** tabblad controle de **OAuth 2.0** selectievakje, selecteer de gewenste autorisatie-server in de **autorisatie server** vervolgkeuzelijst en klik op  **Sla**.

![Rekenmachine-API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Is de basisrekenmachine-API aanroepen vanuit de portal voor ontwikkelaars
Nu dat het OAuth 2.0-autorisatie is geconfigureerd op de API, kunnen bewerkingen met succes worden aangeroepen vanuit het developer center. Deze stap wordt geïllustreerd in de video om 15:00 uur wordt gestart.

Ga terug naar de **twee gehele getallen toevoegen** bewerking van de Rekenmachine-service in de portal voor ontwikkelaars en klik op **Try it**. Let op het nieuwe item in de **autorisatie** sectie overeenkomt met de autorisatie-server die u zojuist hebt toegevoegd.

![Rekenmachine-API][api-management-calc-authorization-server]

Selecteer **autorisatiecode** uit de vervolgkeuzelijst van de autorisatie en voer de referenties van de account moet worden gebruikt. Als u bent al aangemeld bij het account dat u niet gevraagd.

![Rekenmachine-API][api-management-devportal-authorization-code]

Klik op **verzenden** en noteer de **antwoordstatus** van **200 OK** en de resultaten van de bewerking in de antwoordinhoud.

![Rekenmachine-API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Configureer een bureaubladtoepassing de API aan te roepen
De volgende procedure in de video begint bij 16:30 en configureert u een eenvoudige bureaubladtoepassing de API aan te roepen. De eerste stap is het registreren van de bureaubladtoepassing in Azure AD en toegang geven tot de map en de back-endservice. 18:25 is er een demonstratie van de bureaubladtoepassing een bewerking op de Rekenmachine-API aanroepen.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configureer een beleid voor de validatie van JWT vooraf om aanvragen te verifiëren
De laatste procedure in de video begint bij 20:48 en leest u hoe u de [JWT valideren](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) beleid voor het autoriseren vooraf aanvragen door het valideren van de toegangstokens voor elke inkomende aanvraag. Als de aanvraag is niet gevalideerd door het valideren van JWT-beleid, wordt de aanvraag wordt geblokkeerd door de API Management en niet wordt doorgestuurd naar de back-end.

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

Zie voor een andere demonstratie van configureren en gebruiken van dit beleid [Cloud hebben betrekking op aflevering 177: meer API Management-functies](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit tot 13:50. Snel doorsturen naar 15:00 voor een overzicht van het beleid dat is geconfigureerd in de beleidseditor en vervolgens naar 18:50 voor een demonstratie van een bewerking aanroepen vanuit de ontwikkelaarsportal zowel met als zonder de vereiste verificatietoken.

## <a name="next-steps"></a>Volgende stappen
* Bekijk meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over API Management.
* Zie voor andere manieren voor het beveiligen van uw back-endservice [wederzijdse certificaatverificatie](api-management-howto-mutual-certificates.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Manage your first API]: api-management-get-started.md
