---
title: Aanroepen, trigger of nesten van werkstromen met HTTP-eindpunten - Azure Logic Apps | Microsoft Docs
description: HTTP-eindpunten kunnen worden gebeld, trigger of nesten van werkstromen voor Azure Logic Apps instellen
services: logic-apps
keywords: werkstromen, HTTP-eindpunten
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: dab336da4e010d0a78de9a2bdd62536d8fdd9bf1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a>Bel, trigger of nesten van werkstromen met HTTP-eindpunten in logic apps

U kunt systeemeigen synchrone HTTP-eindpunten weergeven als triggers voor logic apps, zodat u kunt activeren of aanroepen van uw logische apps via een andere URL. U kunt ook de werkstromen in uw logische apps nesten met behulp van een patroon van callable-eindpunten.

U kunt deze triggers voor het maken van HTTP-eindpunten toevoegen zodat uw logische apps inkomende aanvragen kunnen ontvangen:

* [Aanvraag](../connectors/connectors-native-reqres.md)

* [API-verbinding Webhook](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [HTTP-webhook](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Hoewel onze voorbeelden gebruiken de **aanvragen** worden geactiveerd, kunt u een van de vermelde HTTP-triggers en alle principles identiek toepassen op de andere triggertypen.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Een HTTP-eindpunt voor uw logische app instellen

Voeg een trigger die inkomende aanvragen kan ontvangen voor het maken van een HTTP-eindpunt.

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). Ga naar uw logische app en open Logic App-ontwerper.

2. Toevoegen van een trigger waarmee uw logische app inkomende aanvragen worden ontvangen. Bijvoorbeeld, voeg de **aanvragen** trigger aan uw logische app.

3.  Onder **aanvraag hoofdtekst JSON-Schema**, kunt u eventueel een JSON-schema voor de nettolading (gegevens) die u verwacht dat de trigger voor het ontvangen van invoeren.

    De ontwerpfunctie voor dit schema gebruikt voor het genereren van tokens die uw logische app gebruiken kunt om te gebruiken, analyseren en doorsturen van gegevens van de trigger via uw werkstroom. 
    Informatie over [tokens die zijn gegenereerd op basis van de JSON-schema's](#generated-tokens).

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

    ![De actie aanvraag toevoegen][1]

    > [!TIP]
    > 
    > U kunt een schema voor een voorbeeld JSON-nettolading genereren vanuit een hulpprogramma zoals [jsonschema.net](http://jsonschema.net/), of in de **aanvragen** trigger door te kiezen **gebruik voorbeeld nettolading voor het genereren van schema**. 
    > Voer uw payload voorbeeld en kies **gedaan**.

    Bijvoorbeeld, de nettolading van dit voorbeeld:

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

4.  Sla uw logische app. Onder **HTTP POST naar deze URL**, u ziet nu een gegenereerde retouraanroep-URL, zoals in dit voorbeeld:

    ![URL van de gegenereerde retouraanroep voor het eindpunt](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Deze URL bevat een Shared Access Signature (SAS)-sleutel in de queryparameters die worden gebruikt voor verificatie. 
    U kunt ook de HTTP-eindpunt-URL ophalen uit het overzicht van de app logica in de Azure portal. Onder **Trigger geschiedenis**, selecteer uw trigger:

    ![HTTP-eindpunt-URL ophalen uit Azure-portal][2]

    Of u kunt de URL ophalen door deze oproep te plaatsen:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a>De HTTP-methode voor de trigger wijzigen

Standaard de **aanvraag** trigger een HTTP POST-aanvraag verwacht, maar u kunt een andere HTTP-methode. 

> [!NOTE]
> U kunt slechts één methodetype opgeven.

1. Op uw **aanvragen** activeren, kiest u **geavanceerde opties weergeven**.

2. Open de **methode** lijst. Selecteer voor dit voorbeeld **ophalen** zodat u kunt uw HTTP-eindpunt-URL later testen.

    > [!NOTE]
    > U kunt selecteren van een andere HTTP-methode of een aangepaste methode voor uw eigen logische app opgeven.

    ![HTTP-methode wijzigen](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>Accepteren parameters via uw HTTP-eindpunt-URL

Als u wilt dat uw HTTP-eindpunt-URL parameters accepteert, aanpassen van uw trigger relatief pad.

1. Op uw **aanvragen** activeren, kiest u **geavanceerde opties weergeven**. 

2. Onder **methode**, geef de HTTP-methode die u wilt dat uw aanvraag om te gebruiken. Selecteer voor dit voorbeeld wordt de **ophalen** methode, als u nog niet gedaan, hebt zodat u kunt uw HTTP-eindpunt-URL testen.

      > [!NOTE]
      > Wanneer u een relatief pad voor de trigger opgeeft, kunt u een HTTP-methode moet ook expliciet opgeven voor de trigger.

3. Onder **relatief pad**, geef het relatieve pad voor de parameter die de URL, bijvoorbeeld accepteren moet `customers/{customerID}`.

    ![Geef de HTTP-methode en het relatieve pad voor de parameter](./media/logic-apps-http-endpoint/relativeurl.png)

4. Toevoegen voor het gebruik van de parameter een **antwoord** aan uw logische app in te grijpen. (Kies onder de trigger **nieuwe stap** > **een actie toevoegen** > **antwoord**) 

5. In uw antwoord **hoofdtekst**, het token voor de parameter die u hebt opgegeven in de trigger relatief pad zijn.

    Om bijvoorbeeld te retourneren `Hello {customerID}`, uw antwoord bijwerken **hoofdtekst** met `Hello {customerID token}`. 
    De lijst met dynamische inhoud moet worden weergegeven en geven de `customerID` token voor selectie.

    ![Parameter toevoegen aan de hoofdtekst van de reactie](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    Uw **hoofdtekst** moet eruitzien als in dit voorbeeld:

    ![Antwoordtekst met de parameter](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Sla uw logische app. 

    De HTTP-eindpunt-URL bevat nu het relatieve pad, bijvoorbeeld: 

    HTTPS & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. Als u wilt testen van uw HTTP-eindpunt, kopieer en plak de bijgewerkte URL in een ander browservenster, maar vervang `{customerID}` met `123456`, en druk op Enter.

    Deze tekst moet worden weergegeven in uw browser: 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Tokens die zijn gegenereerd op basis van de JSON-schema's voor uw logische app

Wanneer het bieden van een JSON-schema in uw **aanvragen** worden geactiveerd, de Logic App-ontwerper genereert tokens voor eigenschappen in dat het schema. U kunt deze tokens vervolgens gebruiken voor het doorgeven van gegevens via uw logische app-werkstroom.

Voor dit voorbeeld als u de `title` en `name` eigenschappen voor uw JSON-schema, hun tokens zijn nu beschikbaar voor gebruik in latere werkstroomstappen. 

Dit is het volledig JSON-schema:

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

## <a name="create-nested-workflows-for-logic-apps"></a>Geneste werkstromen voor logic apps maken

U kunt werkstromen in uw logische app nesten door toe te voegen andere logic apps die aanvragen kunnen ontvangen. Toevoegen als u wilt opnemen in deze logic apps, de **Azure Logic Apps - kiest u een werkstroom Logic Apps** actie kan de trigger. U kunt selecteren van in aanmerking komende logic apps.

![Een andere logische app toevoegen](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>Aanroepen of activeren van logische apps via HTTP-eindpunten

Nadat u uw HTTP-eindpunt hebt gemaakt, kunt u uw logische app via activeren een `POST` methode voor de volledige URL. Logische apps hebben een ingebouwde ondersteuning voor directe toegang eindpunten.

## <a name="reference-content-from-an-incoming-request"></a>Referentie-inhoud van een binnenkomende aanvraag

Als de inhoud type `application/json`, kunt u verwijzen naar eigenschappen van de binnenkomende aanvraag. Anders wordt inhoud behandeld als één eenheid binaire die u aan andere API's doorgeven kunt. Om te verwijzen naar deze inhoud in de werkstroom, moet u die inhoud te converteren. Bijvoorbeeld, als u doorgeeft `application/xml` inhoud, kunt u `@xpath()` voor een extractie XPath of `@json()` voor XML converteren naar JSON. Meer informatie over [werken met inhoudstypen](../logic-apps/logic-apps-content-type.md).

Als u de uitvoer van een inkomende aanvraag, kunt u de `@triggerOutputs()` functie. De uitvoer kan er als volgt uitzien:

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

Voor toegang tot de `body` eigenschap specifiek, kunt u de `@triggerBody()` snelkoppeling. 

## <a name="respond-to-requests"></a>Reageren op aanvragen

Het is raadzaam om te reageren op bepaalde verzoeken die een logische app starten door de inhoud wordt teruggezonden naar de aanroeper. Kan de statuscode, koptekst en hoofdtekst voor uw antwoord, kunt u de **antwoord** in te grijpen. Deze actie kan overal voorkomen in uw logische app, niet alleen aan het einde van uw werkstroom.

> [!NOTE] 
> Als uw logische app geen een **antwoord**, het HTTP-eindpunt reageert *onmiddellijk* met een **202 geaccepteerde** status. Ook voor de oorspronkelijke aanvraag voor het ophalen van het antwoord, alle stappen die nodig zijn voor het antwoord moeten worden voltooid binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md) tenzij u de werkstroom als een geneste logische app aanroepen. Als er geen reactie binnen deze limiet gebeurt, de binnenkomende aanvraag een time-out optreedt en ontvangt van het HTTP-antwoord **408 time-out van de Client**. Voor geneste logic apps blijft de bovenliggende logische app moet worden gewacht op een antwoord pas voltooid, ongeacht hoeveel tijd vereist is.

### <a name="construct-the-response"></a>Het antwoord maken

U kunt meer dan één header en elk type inhoud in de hoofdtekst van antwoord opnemen. In ons voorbeeld-antwoord bevat de header dat het antwoord type inhoud heeft `application/json`. en de hoofdtekst bevat `title` en `name`, op basis van de JSON-schema eerder bijgewerkt in verband met de **aanvragen** trigger.

![HTTP-antwoord actie][3]

Antwoorden hebben deze eigenschappen:

| Eigenschap | Beschrijving |
| --- | --- |
| statusCode |Hiermee geeft u de HTTP-statuscode voor het reageren op de binnenkomende aanvraag. Deze code kan geldige statuscode die met 2xx, 4xx of 5xx begint zijn. Statuscodes 3xx zijn echter niet toegestaan. |
| Headers |Hiermee definieert u een willekeurig aantal kopteksten in het antwoord op te nemen. |
| Hoofdtekst |Hiermee geeft u een instantie-object dat kan bestaan uit een tekenreeks, een JSON-object of zelfs binaire inhoud waarnaar wordt verwezen vanuit de vorige stap. |

Hier ziet u hoe de JSON-schema eruit ziet nu voor de **antwoord** actie:

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
> U kunt de volledige JSON-definitie voor uw logische app bekijken op de logische App Designer **codeweergave**.

## <a name="q--a"></a>Vragen en antwoorden

#### <a name="q-what-about-url-security"></a>V: hoe zit URL beveiliging?

A: azure genereert veilig logic app retouraanroep-URL's met behulp van een Shared Access Signature (SAS). Deze handtekening als een queryparameter passeert en moet worden gevalideerd voordat uw logische app kan worden gestart. Azure genereert de handtekening met een unieke combinatie van een geheime sleutel per logische app, de naam van de trigger en de bewerking die wordt uitgevoerd. Dus tenzij iemand toegang tot de geheime logic app-sleutel heeft, deze een geldige handtekening niet genereren kunnen.

   > [!IMPORTANT]
   > Voor productie en beveiligde systemen kunt beter geen aanroepen van uw logische app rechtstreeks vanuit de browser, omdat:
   > 
   > * De gedeelde toegangssleutel wordt weergegeven in de URL.
   > * U beheren geen beveiligde inhoud beleidsregels vanwege gedeelde domeinen voor klanten is logische App.

#### <a name="q-can-i-configure-http-endpoints-further"></a>V: kan ik meer HTTP-eindpunten configureren?

A: Ja, HTTP-eindpunten ondersteuning voor meer geavanceerde configuratie via [ **API Management**](../api-management/api-management-key-concepts.md). Deze service biedt ook de mogelijkheid om u consistent alle uw API's, met inbegrip van logische apps beheren, instellen van aangepaste domeinnamen, gebruik meer verificatiemethoden en meer, zoals:

* [De aanvraagmethode wijzigen](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [De URL-segmenten van de aanvraag wijzigen](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* Instellen van uw API Management-domeinen in de [Azure-portal](https://portal.azure.com/ "Azure-portal")
* Beleid instellen om te controleren voor basisverificatie

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a>V: wat gewijzigd wanneer het schema van de preview 1 December 2014 gemigreerd?

A: Hier volgt een samenvatting over deze wijzigingen:

| Voorbeeld 1 december 2014 | 1 juni 2016 |
| --- | --- |
| Klik op **HTTP-Listener** API-App |Klik op **handmatige trigger** (Er is geen API App vereist) |
| HTTP-Listener-instelling '*automatisch antwoord verzendt*' |Beide omvatten een **antwoord** actie of niet in de werkstroomdefinitie |
| Basisverificatie of OAuth-verificatie configureren |via API-beheer |
| Configureren van HTTP-methode |Onder **geavanceerde opties weergeven**, kiest u een HTTP-methode |
| Relatief pad configureren |Onder **geavanceerde opties weergeven**, voegt u een relatief pad |
| Verwijst naar de hoofdtekst van het binnenkomende via`@triggerOutputs().body.Content` |Verwijzing via`@triggerOutputs().body` |
| **HTTP-antwoord verzonden** actie op de HTTP-Listener |Klik op **reageert op HTTP-aanvraag** (Er is geen API App vereist) |

## <a name="get-help"></a>Help opvragen

Ga naar het [Forum voor Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Azure Logic Apps en connectors kunt u stemmen op ideeën of ideeën indien op de [site voor gebruikersfeedback van Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Definities voor logische apps maken](./logic-apps-author-definitions.md)
* [Fouten en uitzonderingen verwerken](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
