---
title: Maak en registreer de SOAP-connectors - Azure Logic Apps | Microsoft Docs
description: SOAP-connectors instellen voor gebruik in Azure Logic Apps
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
ms.date: 10/24/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 031762e5639fc52e0b0a6a5bf8d12db25da25e12
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Maak en registreer de SOAP-connectors in Azure Logic Apps

U kunt SOAP-services integreren in de werkstromen van uw logische apps door een aangepaste SOAP-connector (Simple Object Access Protocol) te maken en registreren door gebruik te maken van de WSDL-taal (Web Services Description Language), waarmee u de SOAP-service kunt beschrijven. De SOAP-connectors werken zoals vooraf gedefinieerde connectors. U kunt ze dus op dezelfde manier als andere connectors gebruiken in logische apps.


## <a name="prerequisites"></a>Vereisten

Deze items zijn vereist om de SOAP-connector te registreren:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u beginnen met een [gratis Azure-account](https://azure.microsoft.com/free/). U kunt ook kiezen voor een [Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Een van deze items:
  * Een URL naar een WSDL die de SOAP-service en de API's definieert
  * Een WSDL-bestand dat de SOAP-service en de API's definieert

  Voor deze zelfstudie kunt u ons voorbeeld [Orders SOAP Service](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl) gebruiken.

* Optioneel: een afbeelding die u als een pictogram wilt gebruiken voor uw aangepaste connector


## <a name="1-create-your-connector"></a>1. De connector maken

1. Kies **Nieuw** in het Azure-hoofdmenu in Azure Portal. Typ 'logic apps connector' als filter in het zoekvak en druk op Enter.

   ![Zoek naar 'logic apps connector'](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Kies in de lijst met resultaten **Logic Apps-connector** > **Maken**.

   ![Een Logic Apps-connector maken](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Geef details op voor het registreren van de connector, zoals wordt beschreven in de tabel. Als u klaar bent, kiest u **Vastmaken aan dashboard** > **Maken**.

   ![Details van aangepaste Logic App-connector](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Eigenschap | Voorgestelde waarde | Beschrijving | 
   | -------- | --------------- | ----------- | 
   | **Naam** | *naam-soap-connector* | Geef een naam op voor de connector. | 
   | **Abonnement** | *naam-Azure-abonnement* | Selecteer uw Azure-abonnement. | 
   | **Resourcegroep** | *naam-Azure-resourcegroep* | Maak of selecteer een Azure-groep voor het ordenen van uw Azure-resources. | 
   | **Locatie** | *implementatie-regio* | Selecteer een implementatieregio voor de connector. | 
   |||| 

   Nadat Azure de connector heeft geïmplementeerd, wordt automatisch het menu van de Logic Apps-connector geopend. 
   Als dat niet het geval is, kiest u de SOAP-connector in het Azure-dashboard.

## <a name="2-define-your-connector"></a>2. De connector definiëren

Geef nu het WSDL-bestand of de URL op voor het maken van de connector, de verificatie die door de connector wordt gebruikt, en de acties en triggers die de SOAP-connector ondersteunt.


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Het WSDL-bestand of de URL voor de connector opgeven

1. Kies in het menu van de connector **Logic Apps-connector** als u dat nog niet hebt gedaan. Kies **Bewerken** op de werkbalk.

   ![Aangepaste connector bewerken](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Kies **Algemeen** zodat u de gegevens uit deze tabellen kunt opgeven om de acties en triggers voor de SOAP-connector te maken, beveiligen en definiëren.

   1. Selecteer bij **Aangepaste connectoren** het keuzerondje **SOAP** voor het **API-eindpunt** , zodat u het WSDL-bestand kunt opgeven dat een beschrijving van uw API bevat.

      ![WSDL-bestand opgeven voor uw API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Optie | Indeling |Beschrijving | 
      | ------ | ------ | ----------- | 
      | **WSDL uploaden uit bestand** | *WSDL-bestand* | Blader naar de locatie van het WSDL-bestand en selecteer dat bestand. | 
      | **WSDL-bestand uploaden via URL** | http://*pad-naar-wsdl-bestand* | Geef de URL op voor het WSDL-bestand van uw service. | 
      | **SOAP naar REST** |   | API's in de SOAP-service transformeren naar REST-API's. | 
      |||| 

   2. In de sectie **Algemene informatie** kunt u een pictogram voor de connector uploaden. 
   De velden **Beschrijving**, **Host** en **Basis-URL** worden meestal automatisch ingevuld op basis van de gegevens in het WSDL-bestand. 
   Als dat niet zo is, voegt u deze informatie toe aan de hand van de onderstaande tabel en kiest u vervolgens **Doorgaan**. 

      ![Connectorgegevens](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Optie of instelling | Indeling | Beschrijving | 
      | ----------------- | ------ | ----------- | 
      | **Connectorpictogram uploaden** | *png-of-jpg-bestand-kleiner-dan-1-MB* | Een pictogram dat de connector voorstelt. <p>Kleur: bij voorkeur een wit logo tegen een gekleurde achtergrond. <p>Afmetingen: een logo van ~ 160 pixels in een vierkant van 230 pixels. | 
      | **Achtergrondkleur van het pictogram** | *hexadecimale-code-kleur-pictogram* | <p>De kleur achter het pictogram die overeenkomt met de achtergrondkleur in uw pictogrambestand. <p>Indeling: hexadecimaal. #007ee5 vertegenwoordigt bijvoorbeeld de kleur blauw. | 
      | **Beschrijving** | *beschrijving-connector* | Geef een korte beschrijving op voor de connector. | 
      | **Host** | *connector-host* | Geef het hostdomein voor de SOAP-service op. | 
      | **Basis-URL** | *basis-URL-connector* | Geef de basis-URL voor de SOAP-service op. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. De verificatie beschrijven die de connector gebruikt

1. Kies nu **Beveiliging** zodat u de verificatie kunt controleren of beschrijven die door de connector wordt gebruikt. Verificatie zorgt ervoor dat de id's van uw gebruikers op de juiste wijze worden uitgewisseld tussen uw service en eventuele clients.

   De optie **Verificatietype** van een connector is standaard ingesteld op **Geen verificatie**.
   
   ![Verificatietype](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Als u het verificatietype wilt wijzigen, kiest u **Bewerken**. U kunt vervolgens **Basisverificatie** selecteren. Als u niet de standaardwaarden voor de parameterlabels wilt gebruiken, wijzigt u de waarden onder **Parameterlabel**.

   ![Basisverificatie](./media/logic-apps-soap-connector-create-register/security.png)

   
2. U kunt de connector opslaan na het invoeren van de beveiligingsgegevens door boven aan de pagina eerst **Connector bijwerken** te kiezen en vervolgens **Doorgaan**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Acties en triggers voor uw connector controleren, bijwerken of definiëren

1. Kies nu **Definitie** zodat u acties en triggers kunt controleren, bewerken of definiëren die gebruikers aan hun werkstromen kunnen toevoegen.

   Acties en triggers worden gebaseerd op de bewerkingen die zijn gedefinieerd in het WSDL-bestand. Aan de hand van deze bewerkingen worden bepaalde waarden op de pagina **Definitie** automatisch ingevuld, zoals in de secties Aanvraag en Antwoord. Dus als de vereiste bewerkingen hier al worden weergegeven, kunt u verdergaan met de volgende stap in het registratieproces zonder wijzigingen aan te brengen op deze pagina.

   ![Definitie van connector](./media/logic-apps-soap-connector-create-register/definition.png)

2. Eventueel, als u wilt bewerken, bestaande acties en triggers of nieuwe toevoegen, [doorgaan met deze stappen](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Connector voltooien

Als u klaar bent, kiest u **Connector bijwerken** zodat u de connector kunt implementeren. 

Gefeliciteerd! Als u vanaf nu een logische app gaat maken, kunt u de connector vinden in de ontwerpfunctie voor logische apps en toevoegen aan uw logische app.

![Connector zoeken in ontwerpfunctie voor logische apps](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Connector delen met andere Logic Apps-gebruikers

Aangepaste connectors die zijn geregistreerd maar niet zijn gecertificeerd, werken zoals door Microsoft beheerde connectors, maar zijn *alleen* zichtbaar en beschikbaar voor de maker van de connector en voor gebruikers met dezelfde Azure Active Directory-tenant en hetzelfde Azure-abonnement voor logische apps in de regio waarin die apps worden geïmplementeerd. Hoewel delen optioneel is, zijn er scenario's mogelijk waarin u uw connectors moet delen met andere gebruikers. 

> [!IMPORTANT]
> Als u een connector deelt, kunnen anderen afhankelijk zijn van de connector. 
> ***Wanneer u de connector verwijdert, worden alle verbindingen met die connector ook verwijderd.***
 
De connector met externe gebruikers buiten deze grenzen bijvoorbeeld delen met alle gebruikers van Logic Apps, [uw connector voor Microsoft-certificeringsinstantie indienen](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Veelgestelde vragen

**V:** Is de SOAP-connector algemeen beschikbaar (GA)? </br>
**A:** De SOAP-connector bevindt zich in **preview**, en is nog geen GA-service.

**V:** Zijn er beperkingen en bekende problemen voor de SOAP-connector? </br>
**A:** Ja, zie de [beperkingen en bekende problemen voor de SOAP-connector](../api-management/api-management-api-import-restrictions.md#wsdl).

**V:** Gelden er limieten voor aangepaste connectors? </br>
**A:** Ja, die worden [hier](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits) beschreven.

## <a name="get-support"></a>Ondersteuning krijgen

* Voor ondersteuning met ontwikkeling en onboarding, of om functies aan te vragen die niet beschikbaar zijn in de registratiewizard, neemt u contact op met [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). Microsoft controleert dit account op vragen en problemen van ontwikkelaars en stuurt deze door naar het juiste team.

* Ga naar het [forum voor Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

* Ter verbetering van Logic Apps kunt u stemmen op ideeën of ideeën indienen op de [site voor gebruikersfeedback van Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Volgende stappen

* [Optioneel: Certificeren uw connector](../logic-apps/custom-connector-submit-certification.md)
* [Veelgestelde vragen over aangepaste connectors voor Azure Logic Apps, Microsoft Flow en PowerApps](../logic-apps/custom-connector-faq.md)