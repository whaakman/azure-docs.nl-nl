---
title: Registreren van aangepaste connectors - Azure Logic Apps | Microsoft Docs
description: Aangepaste connectors voor gebruik in Azure Logic Apps instellen
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 722b82aabe796367d906e9338355b83a1a8b1e1c
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2018
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Aangepaste connectors registreren in Azure Logic Apps

Nadat u uw REST API bouwen, verificatie instellen en u uw definitiebestand OpenAPI of een verzameling Postman, kunt u de connector te registreren. 

## <a name="prerequisites"></a>Vereisten

Voor het registreren van uw aangepaste connector, moet u deze items:

* Details voor het registreren van de connector in Azure, zoals de naam, de Azure-abonnement, de Azure-resourcegroep en de locatie die u wilt gebruiken

* Een bestand OpenAPI (Swagger) of een verzameling Postman waarmee uw API wordt beschreven

  Voor deze zelfstudie kunt u de [Azure-Resources Manager OpenAPI voorbeeldbestand](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

* Een pictogram dat de connector voorstelt.

* Een korte beschrijving voor de connector

* De locatie van de host voor uw API

## <a name="1-create-your-connector"></a>1. De connector maken

1. Kies in de Azure-portal in de Azure hoofdmenu **maken van een resource**. Typ 'logic apps connector' als filter in het zoekvak en druk op Enter.

   ![Zoek naar 'logic apps connector'](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. Kies in de lijst met resultaten **Logic Apps-connector** > **Maken**.

   ![Een Logic Apps-connector maken](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Geef details op voor het registreren van de connector, zoals wordt beschreven in de tabel. Als u klaar bent, kiest u **Vastmaken aan dashboard** > **Maken**.

   ![Details van aangepaste Logic App-connector](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Eigenschap | Voorgestelde waarde | Beschrijving | 
   | -------- | --------------- | ----------- | 
   | **Naam** | *naam-aangepaste-connector* | Geef een naam op voor de connector. | 
   | **Abonnement** | *naam-Azure-abonnement* | Selecteer uw Azure-abonnement. | 
   | **Resourcegroep** | *naam-Azure-resourcegroep* | Maak of selecteer een Azure-groep voor het ordenen van uw Azure-resources. | 
   | **Locatie** | *implementatie-regio* | Selecteer een implementatieregio voor de connector. | 
   |||| 

   Nadat Azure de connector heeft geïmplementeerd, wordt automatisch het menu van de aangepaste connector geopend. 
   Als dat niet het geval is, kiest u de aangepaste connector in het Azure-dashboard.

## <a name="2-define-your-connector"></a>2. De connector definiëren

Nu de OpenAPI bestand of de verzameling Postman voor het maken van de connector, de verificatie die gebruikmaakt van de connector, de acties en triggers die uw aangepaste connector biedt en parameters waarmee acties en triggers kunnen opgeven.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. OpenAPI bestand of Postman verzameling voor de connector opgeven

1. Kies in het menu van de connector **Logic Apps-connector** als u dat nog niet hebt gedaan. Kies **Bewerken** op de werkbalk.

   ![Aangepaste connector bewerken](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Kies **algemene** zodat u de informatie in deze tabellen voor het maken opgeven kunt, beveiligen en de acties en wordt geactiveerd voor uw aangepaste connector te definiëren.

   1. Voor **aangepaste connectors**, een optie te selecteren, zodat u de OpenAPI (Swagger)-bestand met een beschrijving van uw API kan leveren.

      ![Geef het OpenAPI-bestand voor uw API](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Optie | Indeling |Beschrijving | 
      | ------ | ------ | ----------- | 
      | **Een OpenAPI-bestand uploaden** | *OpenAPI (Swagger)-json-file* | Blader naar de locatie voor uw OpenAPI-bestand en selecteer dat bestand. | 
      | **Een URL OpenAPI gebruiken** | http://*path-to-swagger-json-file* | Geef de URL voor uw API OpenAPI bestand. | 
      | **Postman verzameling V1 uploaden** | *exported-Postman-collection-V1-file* | Blader naar de locatie voor de verzameling van een geëxporteerde Postman V1-indeling. | 
      |||| 

   2. In de sectie **Algemene informatie** kunt u een pictogram voor de connector uploaden. 
   Normaal gesproken de **beschrijving**, **Host**, en **basis-URL** velden worden automatisch ingevuld uit het bestand OpenAPI. 
   Als dat niet zo is, voegt u deze informatie toe aan de hand van de onderstaande tabel en kiest u vervolgens **Doorgaan**. 

      ![Connectorgegevens](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Optie of instelling | Indeling | Beschrijving | 
      | ----------------- | ------ | ----------- | 
      | **Connectorpictogram uploaden** | *png-of-jpg-bestand-kleiner-dan-1-MB* | Een pictogram dat de connector voorstelt. <p>Kleur: bij voorkeur een wit logo tegen een gekleurde achtergrond. <p>Afmetingen: een logo van ~ 160 pixels in een vierkant van 230 pixels. | 
      | **Achtergrondkleur van het pictogram** | *hexadecimale-code-kleur-pictogram* | <p>De kleur achter het pictogram die overeenkomt met de achtergrondkleur in uw pictogrambestand. <p>Indeling: hexadecimaal. #007ee5 vertegenwoordigt bijvoorbeeld de kleur blauw. | 
      | **Beschrijving** | *beschrijving-connector* | Geef een korte beschrijving op voor de connector. | 
      | **Host** | *connector-host* | Geef het hostdomein voor uw Web-API. | 
      | **Basis-URL** | *basis-URL-connector* | Geef de basis-URL voor uw Web-API. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. De verificatie beschrijven die de connector gebruikt

1. Kies nu **Beveiliging** zodat u de verificatie kunt controleren of beschrijven die door de connector wordt gebruikt. Verificatie zorgt ervoor dat de id's van uw gebruikers op de juiste wijze worden uitgewisseld tussen uw service en eventuele clients.

   ![Verificatietype](./media/logic-apps-custom-connector-register/security.png)

   * Als u een OpenAPI-bestand uploadt, detecteert de registratiewizard automatisch het verificatietype dat uw Web-API gebruikt, en wordt automatisch gevuld de **beveiliging** sectie in de wizard, op basis van de `securityDefinitions` object dat in het bestand. Dit is bijvoorbeeld een sectie waarin OAuth2.0 met:

     ``` json
     "securityDefinitions": {
       "AAD": {
       "type": "oauth2",
       "flow": "accessCode",
       "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
       "tokenUrl": "https://login.windows.net/common/oauth2/token",
       "scopes": {}
       }
     },
     ```

   * Als het bestand OpenAPI niet het verificatietype en eigenschappen vullen, kiest u **bewerken** zodat u deze informatie kunt toevoegen. 
   
     Bijvoorbeeld, als u eerder hebt [instellen van Azure AD-verificatie in deze zelfstudie](../logic-apps/custom-connector-azure-active-directory-authentication.md), u Azure AD-apps voor het beveiligen van uw Web-API en de connector hebt gemaakt. 
     U kunt nu de toepassing-ID en de client de sleutel die u eerder hebt opgeslagen opgeven:
    
     | Instelling | Voorgestelde waarde | Beschrijving | 
     | ------- | --------------- | ----------- | 
     | **Verificatietype** | OAuth 2.0 | | 
     | **Identity Provider** | Azure Active Directory | | 
     | **client-id** | *application-ID-for-connector-Azure-AD-app* | De toepassings-ID die u eerder hebt opgeslagen voor de connector Azure AD-app | 
     | **Clientgeheim** | *client-key-for-connector-Azure-AD-app* | De sleutel van de client voor de connector Azure AD-app | 
     | **Aanmeldings-URL** | `https://login.windows.net` | | 
     | **Bron-URL** | `https://management.core.windows.net/` | Zorg ervoor dat u de URL precies zoals opgegeven, met inbegrip van de afsluitende slash toevoegt. | 
     |||| 

   * Een verzameling Postman, die automatisch voor u een bestand OpenAPI genereert, wordt automatisch gevuld met het verificatietype *alleen* wanneer u de typen van de ondersteunde verificatie, zoals OAuth 2.0- of Basic gebruikt.

2. U kunt de connector opslaan na het invoeren van de beveiligingsgegevens door boven aan de pagina eerst **Connector bijwerken** te kiezen en vervolgens **Doorgaan**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Acties en triggers voor uw connector controleren, bijwerken of definiëren

1. Kies nu **Definitie** zodat u acties en triggers kunt controleren, bewerken of definiëren die gebruikers aan hun werkstromen kunnen toevoegen.

   Acties en triggers zijn gebaseerd op de bewerkingen gedefinieerd in uw bestand OpenAPI of Postman verzameling, waardoor automatisch invullen de **definitie** pagina en de aanvraag- en -waarden bevatten. Dus als de vereiste bewerkingen hier al worden weergegeven, kunt u verdergaan met de volgende stap in het registratieproces zonder wijzigingen aan te brengen op deze pagina.

   ![Definitie van connector](./media/logic-apps-custom-connector-register/definition.png)

2. Eventueel, als u wilt bewerken, bestaande acties en triggers of nieuwe toevoegen, doorgaan met deze stappen.

<a name="add-action-or-trigger"></a> 

#### <a name="edit-or-add-actions-for-your-connector"></a>Bewerken of acties voor de connector toevoegen

1. Een bestaande actie bewerken, kiest u het nummer voor deze actie. Toevoegen van een actie die niet voorkomt in uw bestand OpenAPI of Postman verzameling, onder **acties**, kies **nieuwe actie**.

2. Onder **algemene**, opgeven of bewerken van deze informatie voor de actie:
   
   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Samenvatting** | *naam van bewerking* | De titel voor deze actie | 
   | **Beschrijving** | *operation-description* | De beschrijving voor deze actie. <p>**Tip**: Zorg ervoor dat de beschrijving wordt beëindigd met een punt. |
   | **Bewerkings-id** | *operation-identifier* | Een unieke naam voor het identificeren van deze actie. Gebruik kamelen, bijvoorbeeld: 'GetPullRequest' | 
   |**Zichtbaarheid**| **geen**, **geavanceerde**, **interne**, of **belangrijk** | De zichtbaarheid van de gebruiker gerichte voor deze actie. Zie voor meer informatie [OpenAPI extensies](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   |||| 

3. Nu de aanvraag voor de actie definiëren.
  
   1. In de **aanvragen** sectie **Import van voorbeeld**. 

   2. Op de **Import van voorbeeld** pagina, plakt u een voorbeeld van een aanvraag. 

      Meestal steekproef aanvragen zijn beschikbaar in de API-documentatie waar vind u informatie voor de **term**, **URL**, **Headers**, en **hoofdtekst**velden. Zie bijvoorbeeld de [Text Analytics API-documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Voorbeeld van een aanvraag importeren](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Als u een connector uit een verzameling Postman maken, controleert u of u die u verwijdert de `Content-type` header van acties en triggers. Logic Apps voegt automatisch deze header. Verwijder bovendien verificatieheaders die u hebt gedefinieerd in de `Security` sectie van de acties en triggers.

      Zie voor geavanceerde OpenAPI functionaliteit [OpenAPI uitbreidingen voor aangepaste connectors](../logic-apps/custom-connector-openapi-extensions.md).

   3. Kies voor het voltooien van de definitie van aanvraag **importeren**.

4. Nu de reactie voor de actie definiëren.

   1. Onder **antwoord**, kunt u een standaardantwoord. 
   Kies **Standaardantwoord toevoegen**.

   2. Op de **Import van voorbeeld** pagina, plakt u een voorbeeldantwoord en kies vervolgens **importeren**.

5. Ten slotte onder **validatie**, Controleer en corrigeer eventuele problemen geïdentificeerd voor de actie.

#### <a name="edit-or-add-triggers-for-your-connector"></a>Bewerken of triggers voor uw connector toevoegen

1. Een bestaande trigger bewerken, kiest u het nummer voor deze trigger. Toevoegen van een trigger die niet voorkomt in uw bestand OpenAPI of Postman verzameling, onder **Triggers**, kies **nieuwe trigger**.

2. Onder **algemene**, opgeven of bewerken van deze informatie voor de trigger:

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Samenvatting** | *naam van bewerking* | De titel voor deze trigger | 
   | **Beschrijving** | *operation-description* | De beschrijving voor deze trigger. <p>**Tip**: Zorg ervoor dat de beschrijving wordt beëindigd met een punt. | 
   | **Bewerkings-id** | *operation-identifier* | Een unieke naam voor het identificeren van deze trigger. Gebruik kamelen, bijvoorbeeld: 'TriggerOnGitHubPushEvent' | 
   |**Zichtbaarheid**| **geen**, **geavanceerde**, **interne**, of **belangrijk** | De zichtbaarheid van de gebruiker gerichte voor deze trigger. Zie voor meer informatie [OpenAPI extensies](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   | **Triggertype** | **Webhook** of **Polling** | Het type voor deze trigger. Bijvoorbeeld, wacht een trigger webhook voor een bepaalde gebeurtenis gebeurt vóór het starten. Een polling-trigger controleert regelmatig een service-eindpunt op basis van een opgegeven interval en frequentie. <p>Zie voor meer informatie over webhook en trigger patronen polling [maken van aangepaste API's](../logic-apps/logic-apps-create-api-app.md). | 
   |||| 

3. De aanvraag die de trigger maakt nu definiëren. 

   1. In de **aanvragen** sectie **Import van voorbeeld**.

   2. Op de **Import van voorbeeld** pagina, plakt u een voorbeeld van een aanvraag. 

      Meestal steekproef aanvragen zijn beschikbaar in de API-documentatie waar vind u informatie voor de **term**, **URL**, **Headers**, en **hoofdtekst**velden. Zie bijvoorbeeld de [Text Analytics API-documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Voorbeeld van een aanvraag importeren](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Als u een connector uit een verzameling Postman maken, controleert u of u die u verwijdert de `Content-type` header van acties en triggers. Logic Apps voegt automatisch deze header. Verwijder bovendien verificatieheaders die u hebt gedefinieerd in de `Security` sectie van de acties en triggers.

      Zie voor geavanceerde OpenAPI functionaliteit [OpenAPI uitbreidingen voor aangepaste connectors](../logic-apps/custom-connector-openapi-extensions.md).

   3. Kies voor het voltooien van de definitie van aanvraag **importeren**. 

4. Nu definiëren van de trigger-antwoord. In de **antwoord** sectie, op basis van de trigger-type als volgt te werk:

   **Webhook-trigger**
   1. In **Webhook antwoord**, kies **Import van voorbeeld**. 

   2. Op de **Import van voorbeeld** pagina, plakt u een voorbeeldantwoord en kies vervolgens **importeren**. Een voorbeeld van een antwoord, Zie de [GitHub-API-referentiemateriaal voor het maken van een webhook](https://developer.github.com/v3/repos/hooks/#create-a-hook).

   3. Onder **Trigger configuratie**, selecteert u een parameter moet worden gebruikt voor een aanvraag voor het maken van de webhook. Deze parameterwaarde Logic Apps gebruikt voor het invullen van de callback-URL die door uw service gebruikt om te communiceren met de trigger.

   **Polling-trigger**
   1. Onder **antwoord**, kunt u een standaardantwoord. 
   Kies **Standaardantwoord toevoegen**.

   2. Op de **Import van voorbeeld** pagina, plakt u een voorbeeldantwoord en kies vervolgens **importeren**.

   3. Onder **Trigger configuratie**, geef de queryparameter, de waarde voor de parameter en een *trigger hint* die aangeeft dat een juiste polling-interval voor de volgende aanvraag.

5. Ten slotte onder **validatie**, Controleer en corrigeer eventuele problemen geïdentificeerd voor de trigger.

#### <a name="review-reference-definitions-for-your-connector"></a>Bekijk verwijzing definities voor de connector

De **verwijzingen** sectie automatisch gevuld van uw API-beschrijving en beschrijft elke parametervelden die acties en triggers kunnen verwijzen naar. 

1. Onder **verwijzingen**, kiest u het nummer voor de referentie-definitie die u wilt bekijken.

2. Onder **naam**, controleren of bijwerken van de referentienaam definitie.

3. Onder **validatie**, Controleer en corrigeer eventuele problemen geïdentificeerd voor de referentie-definitie.

## <a name="3-finish-creating-your-connector"></a>3. Connector voltooien

Als u klaar bent, kiest u **maken** zodat u de connector kunt implementeren. Als u een bestaande connector bijwerkt, kiest u **connector bijwerken**.

Gefeliciteerd! Als u vanaf nu een logische app gaat maken, kunt u de connector vinden in de ontwerpfunctie voor logische apps en toevoegen aan uw logische app.

![Connector zoeken in ontwerpfunctie voor logische apps](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Connector delen met andere Logic Apps-gebruikers

Aangepaste connectors die zijn geregistreerd maar niet zijn gecertificeerd, werken zoals door Microsoft beheerde connectors, maar zijn *alleen* zichtbaar en beschikbaar voor de maker van de connector en voor gebruikers met dezelfde Azure Active Directory-tenant en hetzelfde Azure-abonnement voor logische apps in de regio waarin die apps worden geïmplementeerd. Hoewel delen optioneel is, zijn er scenario's mogelijk waarin u uw connectors moet delen met andere gebruikers. 

> [!IMPORTANT]
> Als u een connector deelt, kunnen anderen afhankelijk zijn van de connector. 
> ***Wanneer u de connector verwijdert, worden alle verbindingen met die connector ook verwijderd.***
 
De connector met externe gebruikers buiten deze grenzen bijvoorbeeld delen met gebruikers alle Logic Apps, stroom en PowerApps, [uw connector voor Microsoft-certificeringsinstantie indienen](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Veelgestelde vragen

**V:** Gelden er limieten voor aangepaste connectors? </br>
**A:** Ja, die worden [hier](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits) beschreven.

## <a name="get-support"></a>Ondersteuning krijgen

* Voor ondersteuning met ontwikkeling en onboarding, of om functies aan te vragen die niet beschikbaar zijn in de registratiewizard, neemt u contact op met [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). Microsoft controleert dit account op vragen en problemen van ontwikkelaars en stuurt deze door naar het juiste team.

* Ga naar het [forum voor Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

* Ter verbetering van Logic Apps kunt u stemmen op ideeën of ideeën indienen op de [site voor gebruikersfeedback van Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Volgende stappen

* [Optioneel: Certificeren uw connector](../logic-apps/custom-connector-submit-certification.md)
* [Veelgestelde vragen over aangepaste connectors voor Azure Logic Apps, Microsoft Flow en PowerApps](../logic-apps/custom-connector-faq.md)