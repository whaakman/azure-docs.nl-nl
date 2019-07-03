---
title: 'Zelfstudie: Een web-app van Azure Time Series Insights met één pagina maken | Microsoft Docs'
description: Informatie over het maken van een webtoepassing van één pagina waarmee query's en gegevens uit een Azure Time Series Insights-omgeving wordt weergegeven.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/29/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: e415c681ae5a35de6e8ff76e09cfef8cc8cc98f8
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544070"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Zelfstudie: Een web-app van Azure Time Series Insights met één pagina maken

In deze zelfstudie leidt u door het proces voor het maken van uw eigen webtoepassing één pagina (SPA) voor toegang tot Azure Time Series Insights-gegevens.

In deze zelfstudie wordt aandacht besteed aan:

> [!div class="checklist"]
> * Het ontwerp van de toepassing
> * Informatie over het registreren van uw toepassing met Azure Active Directory (Azure AD)
> * Maken, publiceren en testen van de webtoepassing

> [!NOTE]
> * De broncode voor deze zelfstudie is beschikbaar op [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * De Time Series Insights [client-voorbeeldapp](https://insights.timeseries.azure.com/clientsample) om weer te geven van de voltooide app die wordt gebruikt in deze zelfstudie wordt gehost.

## <a name="prerequisites"></a>Vereisten

* Zich aanmelden voor een [gratis Azure-abonnement](https://azure.microsoft.com/free/) als u er nog geen hebt.

* Een gratis exemplaar van Visual Studio. Download de [2017 of 2019 Community versies](https://www.visualstudio.com/downloads/) aan de slag.

* De IIS Express, Web Deploy en Azure Cloud Services-hulpprogramma's basisonderdelen voor Visual Studio. De onderdelen toevoegen door het wijzigen van de installatie van Visual Studio.

## <a name="application-design"></a>Toepassingsontwerp

De Time Series Insights-voorbeeld beveiligd-WACHTWOORDVERIFICATIE vormt de basis voor het ontwerp en de code die wordt gebruikt in deze zelfstudie. De code maakt gebruik van de Time Series Insights JavaScript-clientbibliotheek. De clientbibliotheek van Time Series Insights biedt een abstractie van twee API-hoofdcategorieën:

- **Wrapper methoden voor het aanroepen van de Time Series Insights query API's**: REST API's die kunt u gegevens op te vragen voor Time Series Insights met behulp van JSON-expressies. De methoden zijn ingedeeld in de naamruimte TsiClient.server van de bibliotheek.

- **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen**: Methoden die u gebruiken kunt om Time Series Insights-gegevens in een webpagina te visualiseren. De methoden zijn ingedeeld in de naamruimte TsiClient.ux van de bibliotheek.

In deze zelfstudie wordt ook gegevens uit de voorbeeldtoepassing Time Series Insights-omgeving. Zie de zelfstudie voor meer informatie over de structuur van de Time Series Insights-voorbeeldtoepassing en hoe deze maakt gebruik van de Time Series Insights-clientbibliotheek [verkennen van de Azure Time Series Insights JavaScript-clientbibliotheek](tutorial-explore-js-client-lib.md).

## <a name="register-the-application-with-azure-ad"></a>Toepassing registreren bij Azure AD

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

## <a name="build-and-publish-the-web-application"></a>Webtoepassing bouwen en publiceren

1. Maak een map voor het opslaan van de projectbestanden van uw toepassing. Vervolgens gaat u naar elk van de volgende URL's. Met de rechtermuisknop op de **Raw** koppelen in de rechterbovenhoek van de pagina en selecteer vervolgens **opslaan als** om op te slaan van de bestanden in uw projectmap.

   - [*index.HTML*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): de HTML en JavaScript voor de pagina
   - [*sampleStyles.css*]( https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): het opmaakmodel

   > [!NOTE]
   > Afhankelijk van de browser moet u mogelijk de bestandsextensies .html of CSS wijzigen voordat u het bestand opslaat.

1. Controleer of de vereiste onderdelen zijn geïnstalleerd in Visual Studio. De IIS Express, Web Deploy en Azure Cloud Services core extra onderdelen voor Visual Studio moeten worden geïnstalleerd.

    [![Visual Studio - wijzigen van de geïnstalleerde onderdelen](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    > [!NOTE]
    > Uw ervaring met Visual Studio kan enigszins afwijken van de beschreven voorbeelden, afhankelijk van uw versie en configuratie-instellingen.

1. Open Visual Studio en meld u aan. Een project voor de web-App maken op de **bestand** in het menu **Open** > **website**.

    [![Visual Studio - een nieuwe oplossing maken](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. In de **Open Web Site** deelvenster, selecteert u de werkmap waarin u de HTML en CSS-bestanden hebt opgeslagen, en selecteer vervolgens **Open**.

   [![Visual Studio - het menu bestand met de Open- en website-opties](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. In de Visual Studio **weergave** in het menu **Solution Explorer**. Hiermee opent u de nieuwe oplossing. Een website-project (pictogram van globe), waarin de HTML en CSS-bestanden bevat.

   [![Visual Studio - de nieuwe oplossing in Solution Explorer](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Voordat u uw app publiceert, moet u de configuratie-instellingen in alter *index.html*.

   1. Verwijder de opmerkingen in de drie regels onder de opmerking `"PROD RESOURCE LINKS"` om over te schakelen van de afhankelijkheden van ontwikkeling naar productie. Een opmerking bij de drie regels onder de opmerking `"DEV RESOURCE LINKS"`.

      [!code-html[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      De afhankelijkheden moeten waaraan opmerkingen worden toegevoegd als in het volgende voorbeeld:

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

   1. Voor het configureren van de app voor het gebruik van uw Azure AD-app-registratie-ID wijzigt de `clientID` waarde gebruiken de de **toepassings-ID** u gekopieerd **stap 3** wanneer u [geregistreerd van de toepassing Gebruik Azure AD](#register-the-application-with-azure-ad). Als u hebt gemaakt een **afmeldings-URL van** in Azure AD, stelt u die waarde als de `postLogoutRedirectUri` waarde.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Bijvoorbeeld:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   1. Wanneer u klaar wijzigingen worden aangebracht bent, slaat *index.html*.

1. Publiceer de webtoepassing in uw Azure-abonnement als een Azure App Service.  

   > [!NOTE]
   > Verschillende opties in de schermafbeeldingen die worden weergegeven in de volgende stappen worden automatisch ingevuld met gegevens uit uw Azure-abonnement. Het duurt een paar seconden voor elk deelvenster volledig is geladen.  

   1. Klik in Solution Explorer met de rechtermuisknop op de website projectknooppunt en selecteer vervolgens **Web-App publiceren**.  

      [![Visual Studio - Selecteer de optie Solution Explorer publiceren Web-App](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   1. Selecteer **Start** om te beginnen met uw app publiceren.

      [![Visual Studio - het publiceren profiel deelvenster](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   1. Selecteer het abonnement dat u wilt gebruiken voor het publiceren van de toepassing. Selecteer de **TsiSpaApp** project. Selecteer vervolgens **OK**.

      [![Visual Studio - het publicatieprofiel deelvenster van de App Service](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   1. Selecteer **publiceren** om de webtoepassing te implementeren.

      [![Visual Studio - de optie publiceren en de logboekuitvoer publiceren](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   1. Een geslaagde publiceren logboek wordt weergegeven in de Visual Studio **uitvoer** deelvenster. Wanneer de implementatie is voltooid, wordt Visual Studio wordt de web-App op een browsertabblad geopend en wordt gevraagd voor aanmelden. De Time Series Insights-besturingselementen worden na geslaagde aanmelding wordt gevuld met gegevens.

## <a name="troubleshoot"></a>Problemen oplossen  

Foutcode/-conditie | Description
---------------------| -----------
*AADSTS50011: No reply address is registered for the application.* | De Azure AD-registratie ontbreekt de **antwoord-URL** eigenschap. Ga naar **instellingen** > **antwoord-URL's** voor de registratie van uw Azure AD-toepassing. Controleer de **omleidings-URI** moest u de optie om op te geven **stap 2** wanneer u [geregistreerd van de toepassing voor het gebruik van Azure AD](#register-the-application-with-azure-ad) aanwezig is.
*AADSTS50011: Het antwoord op de url die is opgegeven in de aanvraag komt niet overeen met de antwoord-URL's geconfigureerd voor de toepassing: '\<Application ID GUID >'.* | De `postLogoutRedirectUri` opgegeven in **stap 6** in [bouwen en publiceer de webtoepassing](#build-and-publish-the-web-application) moet overeenkomen met de waarde die is opgegeven onder **instellingen**  >  **Antwoord-URL's** in de registratie van uw Azure AD-toepassing. Zorg ervoor dat u ook Wijzig de waarde voor **doel-URL** gebruiken *https* per **stap 5** in [bouwen en publiceer de webtoepassing](#build-and-publish-the-web-application).
De web-App wordt geladen, maar heeft een opgemaakte, alleen tekst aanmeldingspagina, met een witte achtergrond. | Controleer of de paden die worden beschreven **stap 4** in [bouwen en publiceer de webtoepassing](#build-and-publish-the-web-application) juist zijn. Als de webtoepassing de CSS-bestanden niet kan vinden, wordt de pagina niet goed opgemaakt.

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie worden verschillende actieve Azure-services gemaakt. Als u niet van plan bent om te voltooien in deze reeks zelfstudies, wordt u aangeraden dat u alle resources om te voorkomen dat er onnodige kosten in rekening verwijderen.

In Azure portal menu aan de linkerkant:

1. Selecteer **resourcegroepen**, en selecteer vervolgens de resourcegroep die u hebt gemaakt voor de Time Series Insights-omgeving. Aan de bovenkant van de pagina, selecteer **resourcegroep verwijderen**, voer de naam van de resourcegroep en selecteer vervolgens **verwijderen**.
1. Selecteer **resourcegroepen**, en selecteer vervolgens de resourcegroep die is gemaakt door de oplossingsversneller voor simulatie van apparaat. Aan de bovenkant van de pagina, selecteer **resourcegroep verwijderen**, voer de naam van de resourcegroep en selecteer vervolgens **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over:

> [!div class="checklist"]
> * Het ontwerp van de toepassing
> * Informatie over het registreren van uw toepassing met Azure AD
> * Maken, publiceren en testen van de webtoepassing

In deze zelfstudie kan worden geïntegreerd met Azure AD en maakt gebruik van de identiteit van de gebruiker die is aangemeld bij een toegangstoken verkrijgen. Zie voor meer informatie over toegang tot de Time Series Insights-API met behulp van de identiteit van een service of -daemon-toepassing, in dit artikel:

> [!div class="nextstepaction"]
> [Verificatie en autorisatie voor Azure Time Series Insights-API](time-series-insights-authentication-and-authorization.md)
