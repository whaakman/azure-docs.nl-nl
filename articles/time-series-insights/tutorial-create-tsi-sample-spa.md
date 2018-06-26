---
title: Een web-app van Azure Time Series Insights met één pagina maken
description: Lees hoe u een webtoepassing met één pagina maakt waarmee gegevens in een TSI-omgeving worden opgevraagd en weergegeven.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/14/2018
ms.author: bryanla
ms.openlocfilehash: 4442a724cf3e37d5e7271d9c29f99138ab1faa5f
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295827"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Zelfstudie: Een web-app van Azure Time Series Insights met één pagina maken

In deze zelfstudie wordt beschreven hoe u uw eigen webtoepassing met één pagina (SPA) maakt voor het opvragen van TSI gegevens, gemodelleerd naar [deze TSI-voorbeeldtoepassing (Time Series Insights)](https://insights.timeseries.azure.com/clientsample). In deze zelfstudie wordt aandacht besteed aan:

> [!div class="checklist"]
> * Het ontwerp van de toepassing
> * Registreren van de toepassing bij Azure Active Directory (AD)
> * Maken, publiceren en testen van de webtoepassing 

## <a name="prerequisites"></a>Vereisten

Meld u aan voor een [gratis Azure-abonnement](https://azure.microsoft.com/free/) als u nog geen abonnement hebt. 

U moet ook Visual Studio installeren als u dat nog niet hebt gedaan. Voor deze zelfstudie kunt u [de gratis Community-versie downloaden/installeren of een gratis proefversie](https://www.visualstudio.com/downloads/).

## <a name="application-design-overview"></a>Overzicht van ontwerp van de toepassing

Zoals gezegd, vormt de TSI-voorbeeldtoepassing de basis voor het ontwerp en de code die worden gebruikt in deze zelfstudie. Voor de code wordt de JavaScript-bibliotheek voor de TSI-client gebruikt. De TSI-clientbibliotheek biedt een abstractie voor twee API-hoofdcategorieën:

- **Wrapper-methoden voor het aanroepen van de query-API's van TSI** - REST-API's waarmee u met behulp van expressies op basis van JSON query's kunt uitvoeren op TSI-gegevens. De methoden zijn georganiseerd onder de `TsiClient.server`-naamruimte van de bibliotheek.
- **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen**: methoden die worden gebruikt om de TSI-gegevens te visualiseren op een webpagina. De methoden zijn georganiseerd onder de `TsiClient.ux`-naamruimte van de bibliotheek.

In deze zelfstudie worden ook de gegevens uit TSI-omgeving van de voorbeeldtoepassing gebruikt. Zie de zelfstudie [JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken](tutorial-explore-js-client-lib.md) voor meer informatie over de structuur van de TSI-voorbeeldtoepassing en het gebruik van de TSI-clientbibliotheek.

## <a name="register-the-application-with-azure-ad"></a>Toepassing registreren bij Azure AD 

Voordat u de toepassing maakt, moet u deze registreren bij Azure AD. Het registratieproces zorgt ervoor dat de identiteit voor een toepassing wordt geconfigureerd, zodat de toepassing OAuth-ondersteuning kan gebruiken voor eenmalige aanmelding. OAuth vereist dat SPA's de 'impliciete' autorisatietoekenning gebruiken, die u gaat bijwerken in het manifest van de toepassing. Een toepassingsmanifest is een JSON-weergave van de identiteitsconfiguratie van de toepassing. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met uw Azure-abonnementsaccount.  
2. Selecteer in het linkerdeelvenster de resource **Azure Active Directory** en daarna **App-registraties** en **+ Nieuwe toepassing registreren**:  
   
   ![Azure AD-toepassing registreren in Azure Portal](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)

3. Vul op de pagina **Maken** de vereiste parameters in:
   
   Parameter|Beschrijving
   ---|---
   **Naam** | Geef een zinvolle naam op voor de registratie.  
   **Toepassingstype** | Omdat u een SPA-webtoepassing maakt, kunt u Web-app/-API laten staan.
   **Aanmeldings-URL** | Voer de URL in voor de startpagina/aanmeldingspagina van de toepassing. Omdat de toepassing zal worden gehost in Azure App Service (later), moet u een URL binnen het domein 'https://azurewebsites.net' gebruiken. In dit voorbeeld is de naam gebaseerd op de naam van de registratie.

   Als u klaar bent, klikt u op **Maken** om de registratie van de nieuwe toepassing te voltooien.

   ![Registratie van Azure AD-toepassing in Azure Portal voltooien](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)

4. Resourcetoepassingen bieden REST-API's voor gebruik door andere toepassingen en zijn ook geregistreerd bij Azure AD. API's bieden nauwkeurige/beveiligde toegang tot clienttoepassingen, door het beschikbaar stellen van 'scopes'. Omdat uw toepassing de API 'Azure Time Series Insights' gaat aanroepen, moet u de API en de scope opgeven, waarvoor dan tijdens runtime een machtiging wordt aangevraagd/verleend. Selecteer **Instellingen** en vervolgens **Vereiste machtigingen** en **+ Toevoegen**:

   ![Azure AD-machtigingen toevoegen in Azure Portal](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)

5. Klik op de pagina **API-toegang toevoegen** op **1 Een API selecteren** om de API van TSI op te geven. Typ op de pagina **Een API selecteren** 'azure time' in het zoekveld. Selecteer vervolgens de API 'Azure Time Series Insights' in de lijst met resultaten en klik op **Selecteren**: 

   ![Azure AD-machtigingen toevoegen in Azure Portal - API](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)

6. Geef nu een scope op voor de API. Klik terug op de pagina **API-toegang toevoegen** op **2 Machtigingen selecteren**. Selecteer op de pagina **Toegang inschakelen** de scope 'Access Azure Time Series Insights service'. Klik op **Selecteren**, waarna u terugkeert naar de pagina **API-toegang toevoegen**. Klik daar op **Gereed**:

   ![Azure AD-machtigingen toevoegen in Azure Portal - scope](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)

7. Als u terugkeert naar de pagina **Vereiste machtigingen**, ziet u dat daar nu de API 'Azure Time Series inzicht' wordt vermeld. U moet de toepassing ook vooraf toegang geven tot de API en scope, voor alle gebruikers. Klik bovenaan op de knop **Machtigingen verlenen** en selecteer **Ja**:

   ![Vereiste Azure AD-machtigingen in Azure Portal - toestemming](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)

8. Zoals eerder vermeld, moet u ook het manifest van de toepassing bijwerken. Klik op de naam van de toepassing in de breadcrumbs om terug te keren naar de pagina **Geregistreerde app**. Selecteer **Manifest**, wijzig de eigenschap `oauth2AllowImplicitFlow` in `true` en klik op **Opslaan**:

   ![Manifest van Azure AD bijwerken in Azure Portal](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)

9. Klik tot slot op de breadcrumbs om weer terug te keren naar de pagina **Geregistreerde app** en kopieer de URL voor de **startpagina** en de **toepassings-id** voor uw toepassing. U gaat deze eigenschappen in een latere stap gebruiken:

   ![Azure AD-eigenschappen in Azure Portal](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)

## <a name="build-and-publish-the-web-application"></a>Webtoepassing bouwen en publiceren

1. Maak een map voor het opslaan van de projectbestanden van uw toepassing. Blader vervolgens naar elk van de volgende URL's, klik met de rechtermuisknop op de koppeling Raw in de rechterbovenhoek van de pagina en gebruik Opslaan als om het bestand op te slaan in de projectmap:

   > [!NOTE]
   > Afhankelijk van de browser moet u de bestandsextensie misschien wijzigen (in HTML of CSS) voordat u het bestand opslaat.

   - **index.html** HTML en JavaScript voor de pagina https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html
   - **sampleStyles.css:** CSS-opmaakmodel: https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css
    
2. Meld u aan bij Visual Studio om een project voor de webtoepassing te maken. Selecteer in het menu **File** de optie **Open** en dan **Web Site**. Selecteer in het dialoogvenster **Open Web Site** de werkmap waarin u de HTML- en CSS-bestanden hebt opgeslagen en klik vervolgens op **Open**:

   ![VS - File kiezen en dan Open Web Site](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)

3. Open **Solution Explorer** via het menu **View** van Visual Studio. U ziet nu uw nieuwe oplossing, met een websiteproject (pictogram van wereldbol) met daarin de HTML- en CSS-bestanden:

   ![VS - Nieuwe oplossing in Solution Explorer](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)

4. Voordat u de toepassing kunt publiceren, moet u delen van de JavaScript-code in **index.html** bijwerken: 

   a. Wijzig eerst de paden voor de verwijzing naar de JavaScript-bestanden en opmaakmodellen in het element `<head>`. Open het bestand **index.html** in uw Visual Studio-oplossing en zoek de volgende regels JavaScript-code. Verwijder het commentaarteken (!) aan het begin van de eerste regel onder 'PROD RESOURCE LINKS' en voeg het commentaarteken toe aan de eerste regel onder 'DEV RESOURCE LINKS':
   
      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      De gewijzigde code moet er nu ongeveer uitzien zoals in het volgende voorbeeld:

      ```javascript
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="sampleStyles.css"></link>
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css"></link>

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="pages/samples/sampleStyles.css"></link>
      <script src="dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="dist/tsiclient.css"></link> -->
      ```

   b. Wijzig vervolgens de logica voor het toegangstoken voor het gebruik van de nieuwe bij Azure AD geregistreerde toepassing. Wijzig de volgende variabelen `clientID` en `postLogoutRedirectUri` in respectievelijk de toepassings-id en de URL van de startpagina die u hebt gekopieerd in stap 9 van de sectie [Toepassing registreren bij Azure AD](#register-the-application-with-azure-ad):

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      De gewijzigde code moet er nu ongeveer uitzien zoals in het volgende voorbeeld:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ``` 

   c. Sla het bestand **index.html** op als u klaar bent met bewerken.

5. Publiceer de webtoepassing nu als een Azure App Service in uw Azure-abonnement:  

   > [!NOTE]
   > Verschillende velden in de volgende dialoogvensters worden automatisch ingevuld met gegevens uit uw Azure-abonnement. Hierdoor kan het een paar seconden duren voordat een dialoogvenster volledig is laden en u verder kunt gaan.  

   a. Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt van de website en selecteer **Publish Web App**:  

      ![VS - Web-app publiceren via Solution Explorer](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)

   b. Selecteer **Microsoft Azure App Service** om het doel te maken waarnaar u wilt publiceren:  

      ![VS - Publicatieprofiel](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)  

   c. Selecteer het abonnement dat u wilt gebruiken voor het publiceren van de toepassing en klik vervolgens op New: 

      ![VS - Publicatieprofiel- app service](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)  

   d. Wacht even totdat het dialoogvenster **Create App Service** volledig is geladen en wijzig vervolgens de volgende velden:
   
      Veld | Beschrijving
      ---|---
      **Naam van app** | Gebruik hier naam waaronder u de Azure AD-toepassing in stap 3 hebt geregistreerd, in [Toepassing registreren bij Azure AD](#register-the-application-with-azure-ad). 
      **Resourcegroep** | Gebruik de knop **New...** om hier de waarde uit het veld **App Name** weer te geven.
      **App Service Plan** | Gebruik de knop **New...** om hier de waarde uit het veld **App Name** weer te geven.

      Klik op **Create** als u klaar bent. De knop **Export** in de linkerbenedenhoek wordt vervangen door 'Deploying'. Deze melding blijft enkele seconden zichtbaar, terwijl de Azure-resources worden gemaakt. Als de resources zijn gemaakt, gaat u terug naar het vorige dialoogvenster. 

      ![VS - Publicatieprofiel- nieuwe app service toevoegen](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service-create.png)  

   e. Als u terug bent in het dialoogvenster **Publish**, controleert u of **Publish method** is ingesteld op 'Web Deploy'. Wijzig ook de waarde `http` van **Destination URL** in `https`, zodat deze overeenkomt met de registratie van de Azure AD-toepassing. Klik vervolgens op Publish om de webtoepassing te implementeren:  

      ![VS - Web-app publiceren - app service publiceren](media/tutorial-create-tsi-sample-spa/vs-publish-publish.png)  

   f. U ziet een bericht in het venster **Outlook** van Visual Studio dat de publicatie is gelukt. Als de implementatie is voltooid, wordt de webtoepassing ook in Visual Studio geopend op een browsertabblad, met het verzoek om u aan te melden. Als u dat hebt gedaan, ziet u dat alle TSI-besturingselementen zijn gevuld met gegevens:  

      [![VS - Web-app publiceren - uitvoer van publicatielogboek](media/tutorial-create-tsi-sample-spa/vs-publish-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-output.png#lightbox)

      ![TSI SPA-app - aanmelden](media/tutorial-create-tsi-sample-spa/tsispaapp-azurewebsites-net.png)  

## <a name="troubleshooting"></a>Problemen oplossen  

Foutcode/-conditie | Beschrijving
---------------------| -----------
*AADSTS50011: No reply address is registered for the application.* | De eigenschap Antwoord-URL ontbreekt voor de Azure AD-registratie. Ga naar de pagina **Instellingen** / **Antwoord-URL's** voor de registratie van uw Azure AD-toepassing. Controleer of de URL voor **Aanmelding** die is opgegeven in stap 3 van [Toepassing registreren bij Azure AD](#register-the-application-with-azure-ad) aanwezig is. 
*AADSTS50011: The reply url specified in the request does not match the reply urls configured for the application: '<Application ID GUID>'.* | De `postLogoutRedirectUri` die is opgegeven in stap 4.b van [Webtoepassing bouwen en publiceren](#build-and-publish-the-web-application) moet overeenkomen met de waarde die is opgegeven onder de eigenschap **Instellingen** / **Antwoord-URL's** in de registratie van uw Azure AD-toepassing. Vergeet ook niet de waarde voor **Destination URL** te wijzigen in`https`, overeenkomstig stap 5.e van [Webtoepassing bouwen en publiceren](#build-and-publish-the-web-application).
De webtoepassing wordt geladen, maar heeft een aanmeldingspagina met alleen tekst zonder opmaak, met een witte achtergrond. | Controleer of de paden uit stap 4.a van [Webtoepassing bouwen en publiceren](#build-and-publish-the-web-application) juist zijn. Als de webtoepassing de CSS-bestanden niet kan vinden, wordt de pagina niet goed opgemaakt.

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie worden verschillende actieve Azure-services gemaakt. Als u niet van plan bent om deze reeks zelfstudies verder uit te voeren, raden we u aan alle resources te verwijderen, om onnodige kosten te voorkomen. 

In het linkermenu van de Azure-portal:

1. Klik op het pictogram **Resourcegroepen** en selecteer de resourcegroep die u hebt gemaakt voor de TSI-omgeving. Klik bovenaan de pagina op **Resourcegroep verwijderen**, voer de naam van de resourcegroep in en klik op **Verwijderen**. 
2. Klik op het pictogram **Resourcegroepen** en selecteer de resourcegroep die is gemaakt met de oplossingsverbetering voor apparaatsimulatie. Klik bovenaan de pagina op **Resourcegroep verwijderen**, voer de naam van de resourcegroep in en klik op **Verwijderen**. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het ontwerp van de toepassing
> * Registreren van de toepassing bij Azure Active Directory (AD)
> * Maken, publiceren en testen van de webtoepassing 

Tijdens deze zelfstudie wordt Azure AD gebruikt, waarbij een toegangstoken wordt verkregen met behulp van de id van de aangemelde gebruiker. Ga naar deze zelfstudie als u wilt weten hoe u toegang tot de TSI-API krijgt met behulp van de identiteit van een service/daemon-toepassing:

> [!div class="nextstepaction"]
> [Verificatie en autorisatie voor Azure Time Series Insights-API](time-series-insights-authentication-and-authorization.md)
