---
title: Werk stromen met HTTP-eind punten aanroepen, activeren of nesten-Azure Logic Apps
description: HTTP-eind punten instellen om werk stromen voor Azure Logic Apps aan te roepen, te activeren of te nesten
services: logic-apps
ms.service: logic-apps
ms.workload: integration
author: ecfan
ms.author: klam
ms.reviewer: jehollan, klam, LADocs
manager: carmonm
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.openlocfilehash: f93e90ef442740e4fb17f166023fbe3d5f0bae66
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875957"
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-azure-logic-apps"></a>Aanroepen, activeren of nesten van werk stromen met HTTP-eind punten in Azure Logic Apps

U kunt synchrone HTTP-eind punten in de oorspronkelijke stand weer geven als triggers op Logic apps, zodat u logische apps kunt activeren of aanroepen via een URL. U kunt ook werk stromen nesten in uw logische apps met behulp van een patroon van aanroep bare eind punten.

Als u HTTP-eind punten wilt maken, kunt u deze triggers toevoegen zodat uw Logic apps binnenkomende aanvragen kunnen ontvangen:

* [Aanvraag](../connectors/connectors-native-reqres.md)

* [Webhook van API-verbinding](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnection-trigger)

* [HTTP-webhook](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Hoewel deze voor beelden de **aanvraag** trigger gebruiken, kunt u een van de vermelde http-triggers gebruiken en alle principes zijn identiek van toepassing op de andere trigger typen.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Een HTTP-eind punt instellen voor uw logische app

Als u een HTTP-eind punt wilt maken, voegt u een trigger toe die binnenkomende aanvragen kan ontvangen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). Ga naar uw logische app en open Logic app Designer.

2. Voeg een trigger toe waarmee uw logische app binnenkomende aanvragen kan ontvangen. Voeg bijvoorbeeld de **aanvraag** trigger toe aan uw logische app.

3.  Onder het JSON-schema van de **aanvraag tekst**kunt u optioneel een JSON-schema invoeren voor de payload (gegevens) die u verwacht de trigger te ontvangen.

    De ontwerp functie maakt gebruik van dit schema voor het genereren van tokens die door uw logische app kunnen worden gebruikt om gegevens van de trigger via uw werk stroom te gebruiken, te parseren en door te geven. 
    Meer informatie over de tokens die zijn [gegenereerd op basis van JSON-schema's](#generated-tokens).

    Voor dit voor beeld voert u het schema in dat wordt weer gegeven in de ontwerp functie:

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

    ![De aanvraag actie toevoegen][1]

    > [!TIP]
    > 
    > U kunt een schema genereren voor een JSON-voorbeeld payload van een hulp programma zoals [jsonschema.net](https://jsonschema.net/)of in de trigger voor **aanvragen** door voor **beeld van nettolading gebruiken te kiezen voor het genereren**van het schema. 
    > Voer uw voor beeld-nettolading in en kies **gereed**.

    Dit is bijvoorbeeld een voor beeld van een nettolading:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    Hiermee wordt dit schema gegenereerd:

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

4.  Sla uw logische app op. Onder **http post naar deze URL**moet u nu een gegenereerde call back-URL vinden, zoals in dit voor beeld:

    ![Gegenereerde call back-URL voor eind punt](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Deze URL bevat een Shared Access Signature SAS-sleutel in de query parameters die worden gebruikt voor verificatie. 
    U kunt de URL van het HTTP-eind punt ook ophalen uit het overzicht van de logische app in de Azure Portal. Onder **trigger geschiedenis**selecteert u de trigger:

    ![URL voor HTTP-eind punt ophalen uit Azure Portal][2]

    U kunt ook de URL ophalen door de volgende aanroep te nemen:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a>De HTTP-methode voor de trigger wijzigen

De **aanvraag** trigger verwacht standaard een HTTP POST-aanvraag, maar u kunt een andere HTTP-methode gebruiken. 

> [!NOTE]
> U kunt slechts één methode type opgeven.

1. Kies op de **aanvraag** trigger **Geavanceerde opties weer geven**.

2. Open de lijst met **methoden** . Selecteer voor dit voor beeld **Get** zodat u de URL van uw http-eind punt later kunt testen.

    > [!NOTE]
    > U kunt een wille keurige andere HTTP-methode selecteren of een aangepaste methode voor uw eigen logische app opgeven.

    ![HTTP-methode wijzigen](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>Para meters accepteren via uw HTTP-eind punt-URL

Als u wilt dat de URL van het HTTP-eind punt para meters accepteert, past u het relatieve pad van de trigger aan.

1. Kies op de **aanvraag** trigger **Geavanceerde opties weer geven**. 

2. Geef bij **methode**de HTTP-methode op die u wilt gebruiken voor uw aanvraag. Voor dit voor beeld selecteert u de methode **Get** , als u dat nog niet hebt gedaan, zodat u de URL van het http-eind punt kunt testen.

      > [!NOTE]
      > Wanneer u een relatief pad voor de trigger opgeeft, moet u ook expliciet een HTTP-methode opgeven voor de trigger.

3. Geef onder **relatief pad**het relatieve pad op voor de para meter die uw URL moet accepteren, bijvoorbeeld `customers/{customerID}`.

    ![Geef de HTTP-methode en het relatieve pad op voor de para meter](./media/logic-apps-http-endpoint/relativeurl.png)

4. Als u de para meter wilt gebruiken, voegt u een **reactie** actie toe aan uw logische app. (Kies **nieuwe stap** > **een actie** > **antwoord**toevoegen onder de trigger) 

5. Neem in de **hoofd tekst**van uw antwoord het token op voor de para meter die u hebt opgegeven in het relatieve pad van uw trigger.

    Als u bijvoorbeeld wilt terugkeren `Hello {customerID}`, moet u de **hoofd tekst** van `Hello {customerID token}`het antwoord bijwerken met. 
    De lijst met dynamische inhoud moet worden weer gegeven `customerID` en het token weer geven dat u wilt selecteren.

    ![Para meter toevoegen aan antwoord tekst](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    Uw **hoofd tekst** moet er als volgt uitzien:

    ![Antwoord tekst met para meter](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Sla uw logische app op. 

    De URL van uw HTTP-eind punt bevat nu het relatieve pad, bijvoorbeeld: 

    https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. Als u uw http-eind punt wilt testen, kopieert en plakt u de bijgewerkte URL in een `{customerID}` ander `123456`browser venster, maar vervangt u door en drukt u op ENTER.

    Deze tekst wordt weer gegeven in de browser: 

    `Hello 123456`

<a name="generated-tokens"></a>

### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Tokens die zijn gegenereerd op basis van JSON-schema's voor uw logische app

Wanneer u een JSON-schema in uw **aanvraag** trigger opgeeft, worden in de ontwerp functie voor logische apps tokens voor eigenschappen in dat schema gegenereerd. U kunt vervolgens deze tokens gebruiken voor het door geven van gegevens via de werk stroom van uw logische app.

Als u in dit voor beeld de `title` eigenschappen en `name` toevoegt aan het JSON-schema, zijn de tokens nu beschikbaar voor gebruik in latere werk stroom stappen. 

Dit is het volledige JSON-schema:

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

## <a name="create-nested-workflows-for-logic-apps"></a>Geneste werk stromen maken voor logische apps

U kunt werk stromen nesten in uw logische app door andere logische apps toe te voegen die aanvragen kunnen ontvangen. Als u deze Logic Apps wilt toevoegen, voegt u de **Azure Logic apps-Kies een Logic apps werk stroom** actie toe aan uw trigger. Vervolgens kunt u kiezen uit in aanmerking komende Logic apps.

![Een andere logische app toevoegen](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>Logische apps aanroepen of activeren via HTTP-eind punten

Nadat u een http-eind punt hebt gemaakt, kunt u uw logische app `POST` activeren via een methode voor de volledige URL. Logic apps hebben ingebouwde ondersteuning voor directe toegangs punten.

> [!NOTE] 
> Als u een logische app op elk gewenst moment hand matig wilt uitvoeren, klikt u op de werk balk van de Logic app Designer of Logic app code weergave op **uitvoeren**.

## <a name="reference-content-from-an-incoming-request"></a>Verwijzen naar inhoud van een binnenkomende aanvraag

Als het type van de inhoud `application/json`is, kunt u naar eigenschappen van de inkomende aanvraag verwijzen. Anders wordt inhoud behandeld als één binaire eenheid die u kunt door geven aan andere Api's. Als u naar deze inhoud in de werk stroom wilt verwijzen, moet u die inhoud converteren. Als u bijvoorbeeld inhoud doorgeeft `application/xml` , kunt u gebruiken `@xpath()` voor een XPath-extractie of `@json()` voor het converteren van XML naar JSON. Meer informatie over het [werken met inhouds typen](../logic-apps/logic-apps-content-type.md).

Als u de uitvoer van een binnenkomende aanvraag wilt ophalen, kunt u `@triggerOutputs()` de functie gebruiken. De uitvoer kan er als volgt uitzien:

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

Voor toegang tot `body` de eigenschap kunt u de `@triggerBody()` snelkoppeling gebruiken. 

## <a name="respond-to-requests"></a>Reageren op aanvragen

Mogelijk wilt u reageren op bepaalde aanvragen die een logische app starten door inhoud te retour neren aan de aanroeper. Als u de status code, koptekst en hoofd tekst voor uw antwoord wilt maken, kunt u de **reactie** actie gebruiken. Deze actie kan overal in uw logische app worden weer gegeven, niet alleen aan het einde van uw werk stroom.

> [!NOTE] 
> Als uw logische app geen **antwoord**bevat, reageert het http-eind punt *onmiddellijk* met een geaccepteerde status van **202** . Voor de oorspronkelijke aanvraag om het antwoord op te halen, moeten alle stappen die vereist zijn voor het antwoord binnen de [time-outlimiet](./logic-apps-limits-and-config.md) van de aanvraag worden voltooid, tenzij u de werk stroom aanroept als een geneste logische app. Als er niet binnen deze limiet een reactie plaatsvindt, wordt de inkomende aanvraag een time-out ontvangen en ontvangt de HTTP-respons **408-timeout**van de client. Voor geneste logische apps blijft de bovenliggende logische app wachten op een reactie voordat deze is voltooid, ongeacht de hoeveelheid tijd die nodig is.

### <a name="construct-the-response"></a>Het antwoord maken

U kunt meer dan één header en elk type inhoud in de hoofd tekst van het antwoord toevoegen. In het voorbeeld antwoord geeft de header aan dat het antwoord inhouds type `application/json`heeft. en de hoofd tekst `title` bevat `name`en, op basis van het JSON-schema, eerder voor de **aanvraag** trigger bijgewerkt.

![HTTP-antwoord actie][3]

Antwoorden hebben de volgende eigenschappen:

| Eigenschap | Description |
| --- | --- |
| Status code |Hiermee geeft u de HTTP-status code op die reageert op de binnenkomende aanvraag. Deze code kan een geldige status code zijn die begint met 2xx, 4xx of 5xx. 3xx-status codes zijn echter niet toegestaan. |
| Koppen |Hiermee definieert u een wille keurig aantal kopteksten dat in het antwoord moet worden meegenomen. |
| Organen |Hiermee geeft u een body-object op dat een teken reeks, een JSON-object of zelfs binaire inhoud bevat waarnaar wordt verwezen vanuit een vorige stap. |

Het JSON-schema ziet er nu uit als de **reactie** actie:

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
> Als u de volledige JSON-definitie voor uw logische app wilt weer geven, kiest u in de ontwerp functie voor logische apps de optie **code weergave**.

## <a name="q--a"></a>Vragen en antwoorden

#### <a name="q-what-about-url-security"></a>V: Hoe zit het met URL-beveiliging?

A: Met Azure kunt u op een veilige manier Url's voor terugbellen voor logische apps genereren met een Shared Access Signature (SAS). Deze hand tekening wordt door gegeven als een query parameter en moet worden gevalideerd voordat de logische app kan worden geactiveerd. Azure genereert de hand tekening met een unieke combi natie van een geheime sleutel per logische app, de naam van de trigger en de bewerking die wordt uitgevoerd. Tenzij iemand toegang heeft tot de sleutel van de geheime logische app, kan er dus geen geldige hand tekening worden gegenereerd.

   > [!IMPORTANT]
   > Voor productie en beveiligde systemen raden wij u ten zeerste aan om uw logische app rechtstreeks vanuit de browser aan te roepen omdat:
   > 
   > * De gedeelde toegangs sleutel wordt weer gegeven in de URL.
   > * U kunt geen beleids regels voor beveiligde inhoud beheren vanwege gedeelde domeinen van logische app-klanten.

#### <a name="q-can-i-configure-http-endpoints-further"></a>V: Kan ik HTTP-eind punten verder configureren?

A: Ja, HTTP-eind punten ondersteunen meer geavanceerde configuratie via [**API Management**](../api-management/api-management-key-concepts.md). Deze service biedt ook de mogelijkheid om alle Api's consistent te beheren, met inbegrip van Logic apps, het instellen van aangepaste domein namen, het gebruiken van meer verificatie methoden en meer, bijvoorbeeld:

* [De aanvraag methode wijzigen](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [De URL-segmenten van de aanvraag wijzigen](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* Stel uw API Management domeinen in het [Azure Portal]in(https://portal.azure.com/ "Azure Portal")
* Beleid instellen om te controleren op basis verificatie

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a>V: Wat is er gewijzigd wanneer het schema is gemigreerd vanaf 1 december 2014 preview?

A: Hier volgt een overzicht van deze wijzigingen:

| 1 december 2014 preview | 1 juni 2016 |
| --- | --- |
| Klik op **HTTP listener API-** app |Klik op **hand matige trigger** (geen API-app vereist) |
| HTTP-listener-instelling '*antwoord automatisch verzenden*' |Neem een **reactie** actie op of niet in de definitie van de werk stroom |
| Basis-of OAuth-verificatie configureren |via API Management |
| HTTP-methode configureren |Kies onder **Geavanceerde opties weer geven**een HTTP-methode |
| Relatief pad configureren |Voeg onder **Geavanceerde opties weer geven**een relatief pad toe |
| Verwijzen naar de binnenkomende instantie via`@triggerOutputs().body.Content` |Verwijzing door`@triggerOutputs().body` |
| **HTTP-antwoord actie verzenden** naar de HTTP-listener |Klik op **reageren op HTTP-aanvraag** (er is geen API-app vereist) |

## <a name="get-help"></a>Help opvragen

Ga naar het [Forum voor Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Azure Logic Apps en connectors kunt u stemmen op ideeën of ideeën indien op de [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Definities voor logische apps maken](./logic-apps-author-definitions.md)
* [Fouten en uitzonderingen verwerken](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png