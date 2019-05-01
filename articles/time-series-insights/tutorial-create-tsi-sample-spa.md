---
title: 'Zelfstudie: Een web-app van Azure Time Series Insights met één pagina maken | Microsoft Docs'
description: Lees hoe u een webtoepassing met één pagina maakt waarmee gegevens in een TSI-omgeving worden opgevraagd en weergegeven.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 18f5c14a9427f4d7e34a802b2bcc0612a51a804a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64573094"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Zelfstudie: Een web-app van Azure Time Series Insights met één pagina maken

In deze zelfstudie leidt u door het proces voor het maken van uw eigen één pagina web-App voor toegang tot gegevens van de Time Series Insights. Specifiek, vindt u meer over:

> [!div class="checklist"]
> * Het ontwerp van de toepassing
> * Registreren van de toepassing bij Azure Active Directory (AD)
> * Maken, publiceren en testen van de webtoepassing

> [!NOTE]
> * De broncode voor deze zelfstudie is beschikbaar op [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * De Time Series Insights [client-voorbeeldapp](https://insights.timeseries.azure.com/clientsample) om weer te geven van de voltooide app die wordt gebruikt in deze zelfstudie wordt gehost.

## <a name="prerequisites"></a>Vereisten

* Meld u aan voor een [gratis Azure-abonnement](https://azure.microsoft.com/free/) als u nog geen abonnement hebt.

* U moet ook een gratis exemplaar van Visual Studio. Download de [Community-versie 2017 of 2019](https://www.visualstudio.com/downloads/) aan de slag.

* U moet ook de **IIS Express**, **Web Deploy**, en **core-hulpprogramma's voor Azure Cloud Services** onderdelen voor Visual Studio. Voeg deze door het wijzigen van de installatie van Visual Studio.

## <a name="application-design-overview"></a>Overzicht van ontwerp van de toepassing

De Time Series Insights één pagina voorbeeld-app vormt de basis voor het ontwerp en de code die wordt gebruikt in deze zelfstudie. Voor de code wordt de JavaScript-bibliotheek voor de TSI-client gebruikt. De TSI-clientbibliotheek biedt een abstractie voor twee API-hoofdcategorieën:

- **Wrapper-methoden voor het aanroepen van de Query-API's voor TSI**: REST-API's waarmee u met behulp van JSON-expressies query's kunt uitvoeren op TSI-gegevens. De methoden zijn georganiseerd onder de `TsiClient.server`-naamruimte van de bibliotheek.

- **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen**: Methoden die worden gebruikt om de TSI-gegevens te visualiseren op een webpagina. De methoden zijn georganiseerd onder de `TsiClient.ux`-naamruimte van de bibliotheek.

In deze zelfstudie worden ook de gegevens uit TSI-omgeving van de voorbeeldtoepassing gebruikt. Zie de zelfstudie [JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken](tutorial-explore-js-client-lib.md) voor meer informatie over de structuur van de TSI-voorbeeldtoepassing en het gebruik van de TSI-clientbibliotheek.

## <a name="register-the-application-with-azure-ad"></a>Toepassing registreren bij Azure AD

Voordat u de toepassing maakt, moet u deze registreren bij Azure AD. Het registratieproces zorgt ervoor dat de identiteit voor een toepassing wordt geconfigureerd, zodat de toepassing OAuth-ondersteuning kan gebruiken voor eenmalige aanmelding. OAuth vereist dat SPA's de 'impliciete' autorisatietoekenning gebruiken, die u gaat bijwerken in het manifest van de toepassing. Een toepassingsmanifest is een JSON-weergave van de identiteitsconfiguratie van de toepassing.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met uw Azure-abonnementsaccount.  
1. Selecteer de **Azure Active Directory** resource in het linkerdeelvenster vervolgens **App-registraties**, klikt u vervolgens **+ nieuwe toepassing registreren**.

   [![Azure-portal Azure AD-toepassing registreren](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png#lightbox)

1. Op de **maken** pagina, vult u de vereiste parameters.

   Parameter|Description
   ---|---
   **Naam** | Geef een zinvolle naam op voor de registratie.  
   **Toepassingstype** | Omdat u een SPA-webtoepassing maakt, kunt u Web-app/-API laten staan.
   **Aanmeldings-URL** | Voer de URL in voor de startpagina/aanmeldingspagina van de toepassing. Omdat de toepassing zal worden gehost in Azure App Service (hoger), moet u een URL in de ' https:\//azurewebsites.net "domein. In dit voorbeeld is de naam gebaseerd op de naam van de registratie.

   Als u klaar bent, klikt u op **Maken** om de registratie van de nieuwe toepassing te voltooien.

   [![Registratie in Azure portal Azure AD-toepassing - maken](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png#lightbox)

1. Resourcetoepassingen bieden REST-API's voor gebruik door andere toepassingen en zijn ook geregistreerd bij Azure AD. API's bieden nauwkeurige/beveiligde toegang tot clienttoepassingen, door het beschikbaar stellen van 'scopes'. Omdat uw toepassing de API 'Azure Time Series Insights' gaat aanroepen, moet u de API en de scope opgeven, waarvoor dan tijdens runtime een machtiging wordt aangevraagd/verleend. Selecteer **instellingen**, klikt u vervolgens **vereiste machtigingen**, klikt u vervolgens **+ toevoegen**.

   [![Azure-portal Azure AD machtigingen toevoegen](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png#lightbox)

1. Klik op de pagina **API-toegang toevoegen** op **1 Een API selecteren** om de API van TSI op te geven. Typ op de pagina **Een API selecteren** 'azure time' in het zoekveld. Vervolgens selecteert u de 'Azure Time Series Insights' API in de lijst met resultaten en klikt u op **Selecteer**.

   [![Machtigingen - API van Azure portal Azure AD toevoegen](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png#lightbox)

1. Geef nu een scope op voor de API. Klik terug op de pagina **API-toegang toevoegen** op **2 Machtigingen selecteren**. Selecteer op de pagina **Toegang inschakelen** de scope 'Access Azure Time Series Insights service'. Klik op **Selecteer**, waarmee u wordt geretourneerd de **API-toegang toevoegen** pagina, waarbij u klikken op **gedaan**.

   [![Azure-portal Azure AD machtigingen - bereik toevoegen](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png#lightbox)

1. Als u terugkeert naar de pagina **Vereiste machtigingen**, ziet u dat daar nu de API 'Azure Time Series inzicht' wordt vermeld. U moet de toepassing ook vooraf toegang geven tot de API en scope, voor alle gebruikers. Klik op de **machtigingen verlenen** aan de bovenkant en selecteer **Ja**.

   [![Azure-portal Azure AD - machtigingen voor toestemming vereist](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png#lightbox)

1. Zoals eerder vermeld, moet u ook het manifest van de toepassing bijwerken. Klik op de naam van de toepassing in de breadcrumbs om terug te keren naar de pagina **Geregistreerde app**. Selecteer **Manifest**, wijzigt de `oauth2AllowImplicitFlow` eigenschap `true`, klikt u vervolgens op **opslaan**.

   [![Manifest van de Azure-portal Azure AD-update](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png#lightbox)

1. Klik tot slot op de breadcrumbs om weer terug te keren naar de pagina **Geregistreerde app** en kopieer de URL voor de **startpagina** en de **toepassings-id** voor uw toepassing. U gebruikt deze eigenschappen in een latere stap.

   [![Azure-portal Azure AD-eigenschappen](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png#lightbox)

## <a name="build-and-publish-the-web-application"></a>Webtoepassing bouwen en publiceren

1. Maak een map voor het opslaan van de projectbestanden van uw toepassing. Blader vervolgens naar elk van de volgende URL's, met de rechtermuisknop op de koppeling 'Raw' in het bovenste juiste gebied van de pagina en 'Opslaan als' in de directory van uw project.

   > [!NOTE]
   > Afhankelijk van de browser moet u de bestandsextensie misschien wijzigen (in HTML of CSS) voordat u het bestand opslaat.

   - [**index.html**](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): HTML en JavaScript voor de pagina.
   - [**sampleStyles.css**]( https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): Opmaakmodel.

1. Controleer of u Visual Studio heeft de vereiste onderdelen geïnstalleerd.

    [![VS - geïnstalleerde onderdelen wijzigen](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    * U moet de **IIS Express**, **Web Deploy**, en **core-hulpprogramma's voor Azure Cloud Services** onderdelen voor Visual Studio.

    > [!NOTE]
    > Uw Visual Studio-ervaring kan enigszins afwijken van de beschreven voorbeelden, afhankelijk van de versie en configuratie-instellingen.

1. Meld u aan bij Visual Studio om een project voor de webtoepassing te maken. Selecteer in het menu **File** de optie **Open** en dan **Web Site**.

    [![Visual Studio - nieuwe oplossing maken](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. Op de **Open Web Site** dialoogvenster, selecteert u de werkmap waarin u de HTML en CSS-bestanden hebt opgeslagen en klik vervolgens op **Open**.

   [![Visual Studio - website van bestand openen](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. Open **Solution Explorer** via het menu **View** van Visual Studio. U ziet uw nieuwe oplossing met een website-project (pictogram van globe), die de HTML en CSS-bestanden bevat.

   [![Visual Studio - nieuwe oplossing voor solution explorer](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Voordat u uw app publiceert, moet u de configuratie-instellingen in alter **index.html**.

   a. Overschakelen van de afhankelijkheden van ontwikkeling tot productie door de drie regels onder de opmerking uncommenting `"PROD RESOURCE LINKS"`. Een opmerking bij de drie regels onder de opmerking `"DEV RESOURCE LINKS"`.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      De afhankelijkheden moeten overeenkomstig worden toegelicht:

      ```HTML
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css">

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="../../dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="../../dist/tsiclient.css"> -->
      ```

   b. Configureer vervolgens de app voor het gebruik van uw Azure Active Directory-app-registratie-ID. Wijzig de `clientID` en `postLogoutRedirectUri` velden moeten worden gebruikt de **toepassings-ID** en **URL van startpagina** u gekopieerd **stap 9** van [registreren de toepassingen met Azure AD](#register-the-application-with-azure-ad).

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Bijvoorbeeld:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   c. Sla **index.html** wanneer u klaar bent met deze wijzigingen worden aangebracht.

1. Nu, publiceer de webtoepassing in uw Azure-abonnement als een Azure App Service:  

   > [!NOTE]
   > Verschillende velden in de volgende dialoogvensters worden automatisch ingevuld met gegevens uit uw Azure-abonnement. Hierdoor kan het een paar seconden duren voordat een dialoogvenster volledig is laden en u verder kunt gaan.  

   a. Klik met de rechtermuisknop op het projectknooppunt website in **Solution Explorer**, en selecteer **Web-App publiceren**.  

      [![VS - solution explorer web-app publiceren](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   b. Selecteer **Start** om te beginnen met uw app publiceren.

      [![VS - publicatieprofiel](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   c. Selecteer het abonnement dat u gebruiken wilt voor het publiceren van de toepassing. Selecteer de **TsiSpaApp** project. Vervolgens **OK**:

      [![VS - profiel - appservice publiceren](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   d. Klik op **publiceren** om de webtoepassing te implementeren.

      [![VS - Web-app publiceren - uitvoer van publicatielogboek](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   e. U ziet een bericht in het venster **Outlook** van Visual Studio dat de publicatie is gelukt. Als de implementatie is voltooid, wordt de webtoepassing ook in Visual Studio geopend op een browsertabblad, met het verzoek om u aan te melden. Na geslaagde aanmelding ziet u alle TSI-besturingselementen die worden ingevuld met gegevens.

## <a name="troubleshooting"></a>Problemen oplossen  

Foutcode/-conditie | Description
---------------------| -----------
*AADSTS50011: No reply address is registered for the application.* | De Azure AD-registratie ontbreekt de **antwoord-URL** eigenschap. Ga naar de pagina **Instellingen** / **Antwoord-URL's** voor de registratie van uw Azure AD-toepassing. Controleer de **Sign-on** -URL opgeven **stap 3** van [de toepassing registreren bij Azure AD](#register-the-application-with-azure-ad) aanwezig is.
*AADSTS50011: Het antwoord op de url die is opgegeven in de aanvraag komt niet overeen met de antwoord-URL's geconfigureerd voor de toepassing: '\<Application ID GUID >'.* | De `postLogoutRedirectUri` die is opgegeven in stap 4.b van [Webtoepassing bouwen en publiceren](#build-and-publish-the-web-application) moet overeenkomen met de waarde die is opgegeven onder de eigenschap **Instellingen** / **Antwoord-URL's** in de registratie van uw Azure AD-toepassing. Zorg ervoor dat u ook wijzigen **doel-URL** gebruiken `https`, per **stap 5** van [bouwen en publiceer de webtoepassing](#build-and-publish-the-web-application).
De webtoepassing wordt geladen, maar heeft een aanmeldingspagina met alleen tekst zonder opmaak, met een witte achtergrond. | Controleer of de paden die worden beschreven **stap 4** van [bouwen en publiceer de webtoepassing](#build-and-publish-the-web-application) juist zijn. Als de webtoepassing de CSS-bestanden niet kan vinden, wordt de pagina niet goed opgemaakt.

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie worden verschillende actieve Azure-services gemaakt. Als u niet van plan bent om deze reeks zelfstudies verder uit te voeren, raden we u aan alle resources te verwijderen, om onnodige kosten te voorkomen.

In het linkermenu van de Azure-portal:

1. Selecteer **resourcegroepen**, selecteer vervolgens de resourcegroep die u hebt gemaakt voor de TSI-omgeving. Klik bovenaan de pagina op **Resourcegroep verwijderen**, voer de naam van de resourcegroep in en klik op **Verwijderen**.
1. Selecteer **resourcegroepen**, selecteer vervolgens de resourcegroep die is gemaakt door de oplossingsversneller voor simulatie van apparaat. Klik bovenaan de pagina op **Resourcegroep verwijderen**, voer de naam van de resourcegroep in en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het ontwerp van de toepassing
> * Registreren van de toepassing bij Azure Active Directory (AD)
> * Maken, publiceren en testen van de webtoepassing

Tijdens deze zelfstudie wordt Azure AD gebruikt, waarbij een toegangstoken wordt verkregen met behulp van de id van de aangemelde gebruiker. Ga naar deze zelfstudie als u wilt weten hoe u toegang tot de TSI-API krijgt met behulp van de identiteit van een service/daemon-toepassing:

> [!div class="nextstepaction"]
> [Verificatie en autorisatie voor Azure Time Series Insights-API](time-series-insights-authentication-and-authorization.md)