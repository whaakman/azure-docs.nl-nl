---
title: Aangepaste API's en -connectors beschrijven met Postman - Azure Logic Apps | Microsoft Docs
description: Postman verzamelingen om te beschrijven, groep en indelen van uw aangepaste API's en -connectors maken
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
ms.openlocfilehash: 072d544e5d29c4abb3d69651e53cfb33d5e0c9e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="describe-custom-apis-and-custom-connectors-with-postman"></a>Aangepaste API's en aangepaste connectors met Postman beschrijven

Om ervoor te ontwikkelen [aangepaste API's](../logic-apps/logic-apps-create-api-app.md) en [aangepaste connectors](../logic-apps/custom-connector-overview.md) sneller en eenvoudiger, kunt u [Postman](https://www.getpostman.com/) verzamelingen, in plaats van OpenAPI documenten, voor het beschrijven van uw API's en -connectors. Postman verzamelingen helpen u te organiseren en groep gerelateerd API-aanvragen. U kunt bijvoorbeeld Postman gebruiken bij het maken van connectors voor Azure Logic Apps, Microsoft Flow of Microsoft PowerApps. 

Deze zelfstudie laat zien hoe u maakt een [Postman verzameling](https://www.getpostman.com/docs/postman/collections/creating_collections) met behulp van de [detecteren taal API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) in [Azure cognitieve Services Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/) als in het voorbeeld. Deze API identificeert de taal, gevoel en in de tekst die u aan de API doorgeeft-sleutel zinnen.

## <a name="prerequisites"></a>Vereisten

* Als u geen ervaring met Postman, [installeren de app Postman](https://www.getpostman.com/apps).

* Een Azure-abonnement. Als u geen een abonnement hebt, kunt u beginnen met een [gratis Azure-account](https://azure.microsoft.com/free/). Anders zich aanmelden voor een [abonnement met betalen naar gebruik](https://azure.microsoft.com/pricing/purchase-options/).

* Als u een Azure-abonnement hebt, aanmelden voor de tekst Analytics API's door te voeren [taak 1 hier](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-signup.md). 

## <a name="create-a-postman-collection"></a>Een verzameling Postman maken

Voordat u een verzameling maken kunt, maakt u een HTTP-aanvraag voor uw API-eindpunt. 

### <a name="create-an-http-request-for-your-api"></a>Maken van een HTTP-aanvraag voor uw API

1. Open de app Postman zodat kunt u een [HTTP-aanvraag](https://www.getpostman.com/docs/postman/sending_api_requests/requests) voor uw API-eindpunt. Zie voor meer informatie de Postman [aanvragen documentatie](https://www.getpostman.com/docs/postman/sending_api_requests/requests).

   1. Op de **Builder** tabblad, selecteer de HTTP-methode, voer de aanvraag-URL voor uw API-eindpunt en selecteer een (authorization protocol), indien van toepassing. 
   Als u klaar bent, kiest u **Params**.

      Voor deze zelfstudie kunt u deze instellingen in dit voorbeeld:

      ![Aanvraag maken: 'HTTP-methode', 'Aanvraag-URL', 'Autorisatie'](./media/custom-connector-api-postman-collection/01-create-api-http-request.png)

      | Parameter | Voorgestelde waarde | 
      | --------- | --------------- | 
      | **HTTP-methode** | VERZENDEN | 
      | **Aanvraag-URL** | `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages` | | 
      | **Autorisatie** | 'Er is geen Auth' | | 
      ||| 

   2. U kunt nu de sleutel-waardeparen moet worden gebruikt als parameters voor query of het pad in de aanvraag-URL invoeren. Postman worden deze items bij elkaar gecombineerd tot een queryreeks.
   Als u bent klaar, kiest u **Headers**.

      ![Aanvraag voortgezet: 'Parameters'](./media/custom-connector-api-postman-collection/02-create-api-http-request-params.png)

      | Parameter | Voorgestelde waarde | 
      | --------- | --------------- | 
      | **Parameters voor** | **Sleutel**: 'numberOfLanguagesToDetect' </br>**Waarde**: '1' | 
      ||| 

   3. Sleutel-waardeparen voor de aanvraagheader invoeren. 
   Voer een willekeurige tekenreeks die u wilt dat voor de naam van de header. Veelvoorkomende HTTP-headers, kunt u in de vervolgkeuzelijst. Als u bent klaar, kiest u **hoofdtekst**. 
   
      ![Aanvraag voortgezet: 'Headers'](./media/custom-connector-api-postman-collection/03-create-api-http-request-header.png)

      | Parameter | Waarde | 
      | --------- | ----- | 
      | **Headers** | **Sleutel**: 'Ocp-Apim-Subscription-Key' </br>**Waarde**: *uw-API-abonnement-key*, kunt u vinden in uw cognitieve Services-account <p>**Sleutel**: 'Content-Type' </br> **Waarde**: ' application/json' | 
      ||| 

   4. Voer de inhoud die u wilt verzenden in de aanvraagtekst. 
   Om te controleren dat de aanvraag door een antwoord terug werkt, kies **verzenden**. 
   
      ![Aanvraag voortgezet: 'Hoofdtekst'](./media/custom-connector-api-postman-collection/04-create-api-http-request-body.png)

      | Parameter | Voorgestelde waarde | 
      | --------- | --------------- |    
      | **Hoofdtekst** | ```{"documents": [{ "id": "1", "text": "Hello World"}]}``` | 
      ||| 

      Het veld met de reactie bevat het volledige antwoord van de API, met inbegrip van het resultaat of een fout optreedt, indien van toepassing is opgetreden.

      ![Reactie op aanvragen ophalen](./media/custom-connector-api-postman-collection/05-create-api-http-request-response.png)

2. Nadat u hebt gecontroleerd dat uw werkt aanvraag opslaan van uw aanvraag voor een verzameling Postman. 

   1. Kies **Opslaan**. 
      
      !['Opslaan' kiezen](./media/custom-connector-api-postman-collection/06a-save-request.png)
 
   2. Onder **opslaan aanvragen**, bieden een **Aanvraagnaam** en desgewenst een **beschrijving aanvragen**. 

      > [!NOTE]
      > Uw aangepaste connector gebruikt deze waarden later. Zorg er dus voor dat u de dezelfde waarden die u later voor uw aangepaste API bewerking samenvatting en beschrijving gebruiken opgeven.

   3. Kies **+ maken verzameling** en geef een verzamelingsnaam. 
   Kiest u het selectievakje waarmee een verzamelingsmap maakt, vervolgens **opslaan naar *uw Postman-verzameling-naam***.

      ![Aanvraag opslaan](./media/custom-connector-api-postman-collection/06b-save-request.png)

### <a name="save-the-request-response"></a>Opslaan van de reactie op aanvragen

Nadat u uw aanvraag hebt opgeslagen, kunt u het antwoord opslaan. Op die manier het antwoord wordt weergegeven als voorbeeld wanneer u de aanvraag later laden.

1. Kies boven het venster antwoord **opslaan antwoord**. 

   ![Antwoord opslaan](./media/custom-connector-api-postman-collection/07-create-api-http-request-save-response.png)

   Aangepaste connectors ondersteuning voor slechts één antwoord per aanvraag. 
   Als u meerdere antwoorden per aanvraag opslaat, worden alleen de eerste die wordt gebruikt.

2. Geef een naam voor uw voorbeeld en kies **opslaan voorbeeld**.

3. Doorgaan met het bouwen van uw verzameling Postman met eventuele aanvullende aanvragen en antwoorden.

### <a name="export-your-postman-collection"></a>Uw collectie Postman exporteren

1. Wanneer u bent klaar, kunt u uw collectie exporteren naar een JSON-bestand.

   ![Verzameling exporteren](./media/custom-connector-api-postman-collection/08-export-http-request.png)

2. Kies de **verzameling v1** exportindeling en blader naar de locatie waar u het JSON-bestand.

   > [!NOTE]
   > Op dit moment werkt alleen V1 voor aangepaste connectors.

   ![Kies de exportindeling: 'Verzameling v1'](./media/custom-connector-api-postman-collection/09-export-format.png)
   
U kunt deze verzameling Postman nu gebruiken voor het maken van aangepaste API's en -connectors. Nadat u de verzameling hebt geëxporteerd, kunt u het JSON-bestand importeren in Logic Apps, stroom of PowerApps.

> [!IMPORTANT]
> Als u een aangepaste connector uit een verzameling Postman maakt, controleert u of u verwijdert de `Content-type` header van acties en triggers. De doelservice, bijvoorbeeld Flow, voegt automatisch deze header. Verwijder bovendien verificatieheaders in de `securityDefintions` sectie van de acties en triggers.

## <a name="next-steps"></a>Volgende stappen

* [Logic Apps: Aangepaste connectors registreren](../logic-apps/logic-apps-custom-connector-register.md)
* [Stroom: Registreer uw connector](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Registreer uw connector](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)
* [Veelgestelde vragen over aangepaste connector](../logic-apps/custom-connector-faq.md)
