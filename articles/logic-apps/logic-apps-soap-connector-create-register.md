---
title: Maak en registreer de SOAP-connectors - Azure Logic Apps | Microsoft Docs
description: SOAP-connectors voor gebruik in Azure Logic Apps instellen
author: divyaswarnkar
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
ms.author: LADocs; divswa
ms.openlocfilehash: 0323b0f7ee03dce209d5a71c6711988a34ba7633
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Maak en registreer de SOAP-connectors in Azure Logic Apps

U kunt voor het integreren van SOAP-services in uw logische app werkstromen, maken en registreren van een aangepaste SOAP Simple Object Access Protocol ()-connector via beschrijving taal WSDL (Web Services) die worden beschreven SOAP-service. De SOAP-connectors werken zoals vooraf gedefinieerde connectors, zodat u ze op dezelfde manier als andere connectors in logic apps gebruiken kunt.


## <a name="prerequisites"></a>Vereisten

Voor het registreren van de SOAP-connector, moet u deze items:

* Een Azure-abonnement. Als u geen een abonnement hebt, kunt u beginnen met een [gratis Azure-account](https://azure.microsoft.com/free/). Anders zich aanmelden voor een [abonnement met betalen naar gebruik](https://azure.microsoft.com/pricing/purchase-options/).

* Een item hier:
  * Een URL naar een WSDL die de SOAP-service definieert en de API 's
  * Een WSDL-bestand waarin uw SOAP-service en de API 's

  Voor deze zelfstudie kunt u ons voorbeeld [Orders SOAP-Service](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl).

* Optioneel: Een afbeelding gebruiken als een pictogram voor uw aangepaste connector


## <a name="1-create-your-connector"></a>1. De connector maken

1. Kies in de Azure-portal in de Azure hoofdmenu **nieuw**. "Connector logic apps" invoeren als filter in het zoekvak en druk op Enter.

   ![Nieuwe, zoek naar "connector logic apps"](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Kies in de lijst met resultaten **Logic Apps Connector** > **maken**.

   ![Logic Apps-connector maken](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Geef details voor het registreren van de connector, zoals beschreven in de tabel. Als u bent klaar, kiest u **vastmaken aan dashboard** > **maken**.

   ![Gegevens van de aangepaste connector Logic App](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Eigenschap | Voorgestelde waarde | Beschrijving | 
   | -------- | --------------- | ----------- | 
   | **Naam** | *SOAP-connectornaam* | Geef een naam voor de connector. | 
   | **Abonnement** | *Naam van een Azure-abonnement* | Selecteer uw Azure-abonnement. | 
   | **Resourcegroep** | *Azure-resource-group-name* | Maak of Selecteer een Azure-groep voor het ordenen van uw Azure-resources. | 
   | **Locatie** | *implementatie-regio* | Selecteer een regio implementatie voor de connector. | 
   |||| 

   Nadat u Azure implementeert de connector, wordt het menu van logic apps connector automatisch geopend. 
   Als dat niet het geval is, kiest u de soap-connector van de Azure-dashboard.

## <a name="2-define-your-connector"></a>2. Definieer de connector

Nu de WSDL-bestand of de URL opgeven voor het maken van de connector, de verificatie die gebruikmaakt van de connector en de acties en triggers waarmee de soap-connector


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Het WSDL-bestand of de URL voor de connector opgeven

1. De connector Kies in het menu als niet is geselecteerd, **Logic Apps Connector**. Kies in de werkbalk **bewerken**.

   ![Aangepaste connector bewerken](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Kies **algemene** zodat u de informatie in deze tabellen voor het maken opgeven kunt, beveiligen en de acties en wordt geactiveerd voor de SOAP-connector te definiëren.

   1. Voor **aangepaste connectors**, selecteer **SOAP** voor uw **API-eindpunt** zodat u de WSDL-bestand met een beschrijving van uw API kan leveren.

      ![Geef het WSDL-bestand voor uw API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Optie | Indeling |Beschrijving | 
      | ------ | ------ | ----------- | 
      | **WSDL uit bestand uploaden** | *WSDL-bestand* | Blader naar de locatie voor het WSDL-bestand en selecteer dat bestand. | 
      | **Uploaden van WSDL van URL** | http://*pad naar wsdl bestand* | Geef de URL voor uw service-WSDL-bestand. | 
      | **SOAP-voor REST** |   | Transformeren API's in de SOAP-service in REST-API's. | 
      |||| 

   2. Voor **algemene informatie**, upload een pictogram voor de connector. 
   Normaal gesproken de **beschrijving**, **Host**, en **basis-URL** velden worden automatisch ingevuld uit het WSDL-bestand. 
   Maar als ze niet, deze informatie toevoegen, zoals beschreven in de tabel en kies **doorgaan**. 

      ![Details van de connector](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Optie of instelling | Indeling | Beschrijving | 
      | ----------------- | ------ | ----------- | 
      | **Pictogram uploaden** | *PNG-or-jpg-File-under-1-MB* | Een pictogram dat staat voor de connector <p>Kleur: Bij voorkeur een witte logo tegen een achtergrondkleur. <p>Afmetingen: Een ~ 160 pixel logo in een vierkant 230 pixel | 
      | **Pictogram achtergrondkleur** | *pictogram merk-kleur-hexadecimaal-code* | <p>De kleur achter het pictogram dat overeenkomt met de achtergrondkleur die in uw pictogrambestand. <p>Indeling: hexadecimale. #007ee5 vertegenwoordigt bijvoorbeeld blauw. | 
      | **Beschrijving** | *Beschrijving connector* | Geef een korte beschrijving voor de connector. | 
      | **Host** | *Connector-host* | Geef het hostdomein voor de SOAP-service. | 
      | **Basis-URL** | *Connector-basis-URL* | Geef de basis-URL voor de SOAP-service. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. De verificatie die gebruikmaakt van de connector beschrijven

1. Nu kiezen **beveiliging** zodat u kunt controleren of de verificatie die gebruikmaakt van de connector te beschrijven. Verificatie zorgt ervoor dat uw gebruikers-id's op de juiste wijze tussen uw service en clients vloeien.

   Standaard de connector van **verificatietype** is ingesteld op **geen verificatie**.
   
   ![Verificatietype](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Als u het verificatietype wijzigen kiezen **bewerken**. U kunt selecteren **basisverificatie**. Voor het gebruik van parameterlabels dan standaardwaarden, werken ze onder **parameterlabel**.

   ![Basisverificatie](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Kies voor het opslaan van de connector na het invoeren van de beveiligingsgegevens boven aan de pagina **connector bijwerken**, en kies vervolgens **doorgaan**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Controleer, bijwerken of acties en wordt geactiveerd voor uw connector definiëren

1. Nu kiezen **definitie** zodat u bekijken kunt, bewerken of nieuwe acties en triggers die gebruikers aan hun werkstromen toevoegen kunnen definiëren.

   Acties en triggers zijn gebaseerd op de bewerkingen gedefinieerd in de WSDL-bestand, waardoor automatisch invullen de **definitie** pagina en de aanvraag- en -waarden bevatten. Dus als de vereiste bewerkingen al hier weergegeven, kunt u met de volgende stap in het registratieproces gaan zonder de wijzigingen op deze pagina.

   ![De definitie van de connector](./media/logic-apps-soap-connector-create-register/definition.png)

2. Eventueel, als u wilt bewerken, bestaande acties en triggers of nieuwe toevoegen, [doorgaan met deze stappen](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Maken van de connector voltooien

Als u klaar bent, kiest u **Update Connector** zodat u de connector kunt implementeren. 

Gefeliciteerd. Nu wanneer u een logische app maakt, kunt u de connector niet vinden in Logic Apps Designer en die connector toevoegen aan uw logische app.

![Zoeken naar de connector in Logic Apps Designer](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>De connector delen met andere gebruikers Logic Apps

Geregistreerd maar niet-gecertificeerde aangepaste connectors werken zoals Microsoft beheerde connectors, maar zichtbaar zijn en beschikbare *alleen* voor de auteur en gebruikers die werken met de dezelfde Azure Active Directory-tenant en de Azure-abonnement van de connector voor logische apps in de regio waar de apps die zijn geïmplementeerd. Hoewel delen optioneel is, moet u wellicht scenario's waar u uw connectors delen met andere gebruikers. 

> [!IMPORTANT]
> Als u een connector deelt, gaan anderen mogelijk afhankelijk zijn van die connector. 
> ***De connector verwijdert, worden alle verbindingen met die connector.***
 
De connector met externe gebruikers buiten deze grenzen bijvoorbeeld delen met alle gebruikers van Logic Apps, [uw connector voor Microsoft-certificeringsinstantie indienen](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Veelgestelde vragen

**V:** Is de SOAP-connector algemeen beschikbaar (GA)? </br>
**A:** het SOAP-connector bevindt zich in **Preview**, en nog niet een GA-service.

**V:** zijn er beperkingen en bekende problemen voor SOAP-connector? </br>
**A:** Ja, Zie de [SOAP-connector beperkingen en bekende problemen](../api-management/api-management-api-import-restrictions.md#wsdl).

**V:** zijn er beperkingen voor aangepaste connectors? </br>
**A:** Ja, Zie de [aangepaste connector beperkt hier](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Ondersteuning krijgen

* Voor de ondersteuning met een ontwikkelings- en voorbereiding of met functies die niet beschikbaar zijn in de wizard registreren, neem contact op met [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Microsoft controleert deze account voor ontwikkelaars vragen en problemen en doorgestuurd naar het juiste team.

* Vraag of vragen beantwoorden of zien wat anderen Azure Logic Apps doen, gaat u naar de [Azure Logic Apps-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Om te verbeteren van Logic Apps, stem op of dien ideeën op de [Logic Apps gebruiker feedback site](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Volgende stappen

* [Optioneel: Certificeren uw connector](../logic-apps/custom-connector-submit-certification.md)
* [Veelgestelde vragen over aangepaste connector](../logic-apps/custom-connector-faq.md)