---
title: Aanroepen, triggeren of nesten van werkstromen met HTTP-eindpunten - Azure Logic Apps | Microsoft Docs
description: HTTP-eindpunten aanroepen, trigger of nesten van werkstromen voor Azure Logic Apps instellen
services: logic-apps
keywords: werkstromen, HTTP-eindpunten
author: jeffhollan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: klam; LADocs
ms.openlocfilehash: 7920fee1bacf569ac41c36142fc68080b4de5780
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230485"
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a>Aanroepen, trigger of nesten van werkstromen met HTTP-eindpunten in logische apps

U kunt systeemeigen synchrone HTTP-eindpunten weergeven als triggers voor logic apps, zodat u kunt activeren of uw logische apps via een URL aanroepen. Ook kunt u werkstromen in uw logische apps met behulp van een patroon van aanroepbare eindpunten nesten.

Voor het maken van HTTP-eindpunten, kunt u deze triggers toevoegen zodat uw logische apps inkomende aanvragen kunnen ontvangen:

* [Aanvraag](../connectors/connectors-native-reqres.md)

* [API-verbinding Webhook](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnection-trigger)

* [HTTP-webhook](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Hoewel deze voorbeelden gebruiken de **aanvragen** trigger, kunt u een van de vermelde HTTP-triggers en alle principes identiek toepassen op de andere triggers van het type.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Instellen van een HTTP-eindpunt voor uw logische app

Voeg een trigger die inkomende aanvragen kan ontvangen voor het maken van een HTTP-eindpunt.

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). Ga naar uw logische app en Logic App Designer openen.

2. Een trigger die uw logische app ontvangen van inkomende aanvragen kunt toevoegen. Voeg bijvoorbeeld de **aanvragen** trigger aan uw logische app.

3.  Onder **hoofdtekst van de JSON-Schema aanvragen**, kunt u eventueel een JSON-schema voor de nettolading (gegevens) die u verwacht de trigger dat voor het ontvangen van invoeren.

    De ontwerpfunctie maakt gebruik van dit schema voor het genereren van tokens die uw logische app gebruiken kunt om te gebruiken, parseren en het doorgeven van gegevens van de trigger via uw werkstroom. 
    Meer informatie over [tokens die zijn gegenereerd op basis van JSON-schema](#generated-tokens).

    Geef het schema wordt weergegeven in de ontwerpfunctie voor dit voorbeeld:

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![De aanvraag-actie toevoegen][1]

    > [!TIP]
    > 
    > U kunt een schema voor een voorbeeld-JSON-nettolading genereren op basis van een hulpprogramma zoals [jsonschema.net](http://jsonschema.net/), of in de **aanvragen** trigger door te kiezen **voorbeeldnettolading gebruiken voor het genereren van schema**. 
    > Voer de voorbeeldnettolading in en kies **gedaan**.

    Bijvoorbeeld: in dit voorbeeld van payload:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    Dit schema genereert:

    ```json
    {
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  Sla uw logische app op. Onder **HTTP POST naar deze URL**, vindt u nu een gegenereerde callback-URL, zoals in dit voorbeeld:

    ![Gegenereerde URL voor terugbellen voor het eindpunt](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Deze URL bevat een Shared Access Signature (SAS)-sleutel in de queryparameters die worden gebruikt voor verificatie. 
    U kunt ook de HTTP-eindpunt-URL ophalen uit uw logic app-overzicht in Azure portal. Onder **Triggergeschiedenis**, selecteert u de trigger:

    ![HTTP-eindpunt-URL ophalen uit Azure portal][2]

    Of u kunt de URL krijgen door het maken van deze aanroep:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a>De HTTP-methode voor de trigger wijzigen

Standaard de **aanvraag** trigger wordt verwacht dat een HTTP POST-aanvraag, maar u kunt een andere HTTP-methode. 

> [!NOTE]
> U kunt slechts één methodetype opgeven.

1. Op uw **aanvragen** activeren, kiest u **geavanceerde opties weergeven**.

2. Open de **methode** lijst. Selecteer voor dit voorbeeld **ophalen** zodat u kunt uw HTTP-eindpunt-URL later testen.

    > [!NOTE]
    > U kunt een andere HTTP-methode selecteren of een aangepaste methode opgeven voor uw eigen logica-app.

    ![HTTP-methode wijzigen](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>Accepteren door middel van uw HTTP-eindpunt-URL

Als u wilt dat uw HTTP-eindpunt-URL om te accepteren van parameters, aanpassen van het relatieve pad van de trigger.

1. Op uw **aanvragen** activeren, kiest u **geavanceerde opties weergeven**. 

2. Onder **methode**, geef de HTTP-methode die u wilt dat uw aanvraag voor het gebruik. Selecteer voor dit voorbeeld wordt de **ophalen** methode, als u nog niet gedaan hebt, zodat u uw HTTP-eindpunt-URL kunt testen.

      > [!NOTE]
      > Wanneer u een relatief pad voor de trigger opgeven, moet u een HTTP-methode ook expliciet opgeven voor de trigger.

3. Onder **relatief pad**, geef het relatieve pad voor de parameter die de URL van uw toestemming moet geven, bijvoorbeeld `customers/{customerID}`.

    ![Geef de HTTP-methode en het relatieve pad voor de parameter](./media/logic-apps-http-endpoint/relativeurl.png)

4. Voor het gebruik van de parameter toevoegen een **antwoord** actie aan uw logische app. (Kies onder de trigger **nieuwe stap** > **een actie toevoegen** > **antwoord**) 

5. In van uw antwoord **hoofdtekst**, omvatten het token voor de parameter die u hebt opgegeven in het relatieve pad van de trigger.

    Bijvoorbeeld, om terug te keren `Hello {customerID}`, bijwerken van uw antwoord **hoofdtekst** met `Hello {customerID token}`. 
    De lijst met dynamische inhoud moet worden weergegeven en geven de `customerID` token voor selectie.

    ![Parameter toevoegen aan de hoofdtekst van antwoord](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    Uw **hoofdtekst** moet eruitzien als in dit voorbeeld:

    ![Hoofdtekst van antwoord met de parameter](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Sla uw logische app op. 

    De HTTP-eindpunt-URL bevat nu het relatieve pad, bijvoorbeeld: 

    https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. Als u wilt testen op uw HTTP-eindpunt, kopieer en plak de bijgewerkte URL in een ander browservenster, maar vervang `{customerID}` met `123456`, en druk op Enter.

    Deze tekst moet worden weergegeven in uw browser: 

    `Hello 123456`

<a name="generated-tokens"></a>

### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Tokens die zijn gegenereerd op basis van JSON-schema's voor uw logische app

Wanneer u een JSON-schema in bieden uw **aanvragen** trigger Logic App Designer-tokens voor eigenschappen in dit schema genereert. Vervolgens kunt u deze tokens gebruiken voor het doorgeven van gegevens via de werkstroom van uw logische app.

Voor dit voorbeeld als u de `title` en `name` eigenschappen aan uw JSON-schema, hun tokens zijn nu beschikbaar voor gebruik in latere werkstroomstappen. 

Dit is de volledige JSON-schema:

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a>Geneste werkstromen voor logische apps maken

U kunt werkstromen in uw logische app nesten door toe te voegen andere logische apps die aanvragen kunnen ontvangen. Toevoegen als u wilt opnemen in deze logic apps, de **Azure Logic Apps - een werkstroom voor Logic Apps kiezen** actie op de trigger. U kunt vervolgens selecteren van in aanmerking komende logic apps.

![Toevoegen van een andere logische app](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>Bel of trigger van logische apps via HTTP-eindpunten

Nadat u uw HTTP-eindpunt hebt gemaakt, kunt u uw logische app via activeren een `POST` methode voor de volledige URL. Logische apps hebben ingebouwde ondersteuning voor rechtstreekse toegang eindpunten.

> [!NOTE] 
> Als u wilt een logische app handmatig uitvoeren op elk gewenst moment, op de werkbalk Logic App Designer of codeweergave van logische App kiezen **uitvoeren**.

## <a name="reference-content-from-an-incoming-request"></a>Referentie-inhoud van een binnenkomende aanvraag

Als het type van de inhoud `application/json`, kunt u verwijzen naar eigenschappen van de inkomende aanvraag. Inhoud wordt anders beschouwd als een enkele binaire eenheid die u aan andere API's doorgeven kunt. Als u wilt verwijzen naar deze inhoud binnen de werkstroom, moet u die inhoud te converteren. Bijvoorbeeld, als u doorgeeft `application/xml` inhoud, kunt u `@xpath()` voor een XPath-extractie of `@json()` voor XML converteren naar JSON. Meer informatie over [werken met inhoudstypen](../logic-apps/logic-apps-content-type.md).

Als u wilt de uitvoer van een binnenkomende aanvraag ophalen, kunt u de `@triggerOutputs()` functie. De uitvoer kan er uitzien zoals in dit voorbeeld:

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

Toegang krijgen tot de `body` eigenschap Name, kunt u de `@triggerBody()` snelkoppeling. 

## <a name="respond-to-requests"></a>Reageren op aanvragen

Het is raadzaam om te reageren op bepaalde aanvragen die een logische app te retourneren van inhoud voor de oproepende functie. Kan de statuscode, koptekst en hoofdtekst voor uw antwoord, kunt u de **antwoord** actie. Deze actie kan overal worden weergegeven in uw logische app, niet alleen aan het einde van uw werkstroom.

> [!NOTE] 
> Als uw logische app bevat geen een **antwoord**, het HTTP-eindpunt reageert *onmiddellijk* met een **202 geaccepteerd** status. Ook alle stappen die nodig zijn voor het antwoord voor de oorspronkelijke aanvraag ophalen van het antwoord moeten voltooien binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md) , tenzij u de werkstroom als een geneste logische app aanroepen. Als er geen reactie binnen deze limiet gebeurt, de inkomende aanvraag een time-out optreedt en het HTTP-antwoord ontvangt **408 time-out van de Client**. Voor geneste logic apps blijft de bovenliggende logische app moet worden gewacht op een reactie totdat voltooid, ongeacht hoeveel tijd vereist is.

### <a name="construct-the-response"></a>Het antwoord maken

U kunt meer dan één kop- en elk type inhoud opnemen in de hoofdtekst van de reactie. De header geeft in het voorbeeld van een antwoord, dat het antwoord inhoudstype heeft `application/json`. en de hoofdtekst bevat `title` en `name`, op basis van het JSON-schema eerder bijgewerkt in verband met de **aanvragen** trigger.

![HTTP-reactie][3]

Antwoorden op hebben deze eigenschappen:

| Eigenschap | Beschrijving |
| --- | --- |
| statusCode |Hiermee geeft u de HTTP-statuscode voor het reageren op de inkomende aanvraag. Deze code kan geen geldige statuscode die met 2xx, 4xx of 5xx begint zijn. 3xx statuscodes zijn echter niet toegestaan. |
| Headers |Hiermee definieert u een willekeurig aantal headers om op te nemen in het antwoord. |
| hoofdtekst |Hiermee geeft u een instantie-object dat kan bestaan uit een tekenreeks, een JSON-object of zelfs binaire inhoud waarnaar wordt verwezen in een vorige stap. |

Hier volgt de JSON-schema eruit nu voor de **antwoord** actie:

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> U kunt de volledige JSON-definitie voor uw logische app, in Logic App Designer bekijken **codeweergave**.

## <a name="q--a"></a>Vragen en antwoorden

#### <a name="q-what-about-url-security"></a>V: wat over URL-beveiliging?

A: azure genereert veilig logic app callback-URL's met behulp van een Shared Access Signature (SAS). Deze handtekening wordt doorgegeven via een query-parameter en moet worden gevalideerd voordat uw logische app kan worden gestart. Azure genereert de handtekening met behulp van een unieke combinatie van een geheime sleutel per logische app, de naam van de trigger en de bewerking die wordt uitgevoerd. Als iemand anders toegang tot de geheime logic app-sleutel heeft, kunnen geen ze dus geen geldige handtekening genereren.

   > [!IMPORTANT]
   > Voor productie en beveiligde systemen, wordt aangeraden op basis van uw logische app aanroepen rechtstreeks vanuit de browser, omdat:
   > 
   > * De gedeelde toegangssleutel wordt weergegeven in de URL.
   > * U kunt geen beveiligde inhoud beleidsregels vanwege gedeelde domeinen beheren voor klanten van de logische App.

#### <a name="q-can-i-configure-http-endpoints-further"></a>V: kan ik meer HTTP-eindpunten configureren?

A: Ja, de meer geavanceerde configuratie via voor het ondersteunen van HTTP-eindpunten [ **API Management**](../api-management/api-management-key-concepts.md). Deze service biedt ook de mogelijkheid om u consistent al uw API's, met inbegrip van logische apps beheren, instellen van aangepaste domeinnamen, gebruik meer verificatiemethoden, en meer, bijvoorbeeld:

* [De aanvraagmethode wijzigen](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [De URL-segmenten van de aanvraag wijzigen](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* Instellen van uw API Management-domeinen in de [Azure-portal](https://portal.azure.com/ "Azure-portal")
* Beleid instellen om te controleren voor basisverificatie

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a>V: wat gewijzigd wanneer het schema van de preview 1 December 2014 gemigreerd?

A: Hier volgt een samenvatting over deze wijzigingen:

| Voorbeeld van 1 december 2014 | Op 1 juni 2016 |
| --- | --- |
| Klik op **HTTP-Listener** API-App |Klik op **handmatige trigger** (Er zijn geen API-App vereist) |
| Instelling voor HTTP-Listener '*automatisch antwoord verzendt*" |Beide omvatten een **antwoord** actie of niet in de definitie van de werkstroom |
| Basic- of OAuth-verificatie configureren |via de API Management |
| Configureren van HTTP-methode |Onder **geavanceerde opties weergeven**, kiest u een HTTP-methode |
| Relatief pad configureren |Onder **geavanceerde opties weergeven**, voegt u een relatief pad |
| Verwijzen naar de hoofdtekst van de binnenkomende via `@triggerOutputs().body.Content` |Verwijzing via `@triggerOutputs().body` |
| **HTTP-antwoord verzenden** actie op de HTTP-Listener |Klik op **reageert op HTTP-aanvraag** (Er zijn geen API-App vereist) |

## <a name="get-help"></a>Help opvragen

Ga naar het [Forum voor Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Azure Logic Apps en connectors kunt u stemmen op ideeën of ideeën indien op de [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Definities voor logische apps maken](./logic-apps-author-definitions.md)
* [Fouten en uitzonderingen verwerken](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png