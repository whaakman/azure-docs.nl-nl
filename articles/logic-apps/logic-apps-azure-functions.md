---
title: Toevoegen en Azure functions aanroepen vanuit Azure Logic Apps
description: Toevoegen en Azure functions uitvoeren vanuit logische apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.topic: article
ms.date: 06/04/2019
ms.reviewer: klam, LADocs
ms.openlocfilehash: 524b927ec0966199c51cdee93e920d7b847139ae
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66495114"
---
# <a name="call-azure-functions-from-azure-logic-apps"></a>Azure functions aanroepen vanuit Azure Logic Apps

Wanneer u uitvoeren van code dat een specifieke taak in uw logische apps wilt uitvoert, kunt u uw eigen functie maken met behulp van [Azure Functions](../azure-functions/functions-overview.md). Deze service helpt u bij het maken van Node.js, C#, en F# functies, zodat u hoeft te maken van een volledige app of infrastructuur code uit te voeren. U kunt ook [logische apps aanroepen in Azure functions](#call-logic-app). Azure Functions biedt serverloze computing in de cloud en is handig voor het uitvoeren van taken, zoals deze voorbeelden:

* Breid uw logische app gedrag met Node.js of C#-functies.
* Uitvoeren van berekeningen in uw werkstroom voor logische Apps.
* Geavanceerde opmaak toepassen of compute-velden in uw logische apps.

Als u wilt codefragmenten uitvoeren zonder te maken van Azure functions, informatie over hoe u [toevoegen en uitvoeren van Inlinecode](../logic-apps/logic-apps-add-run-inline-code.md).

> [!NOTE]
> Integratie tussen Azure-functies en Logic Apps op dit moment werkt niet met sleuven ingeschakeld.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een Azure-functie-app, dat een container voor Azure functions, samen met uw Azure-functie is. Als u een functie-app geen [maakt u eerst uw functie-app](../azure-functions/functions-create-first-azure-function.md). U kunt vervolgens maken uw functie buiten uw logische app in de Azure-portal of [van binnen uw logische app](#create-function-designer) in Logic App Designer.

* Als u werkt met logic apps, dezelfde vereisten zijn van toepassing met de functie-apps en functies ongeacht of deze bestaande of nieuwe:

  * Uw functie-app en de logische app moeten hetzelfde Azure-abonnement gebruiken.

  * Nieuwe functie-apps moeten de .NET- of JavaScript gebruiken als de runtimestack. Wanneer u een nieuwe functie aan bestaande functie-apps toevoegen, kunt u een selecteren C# of JavaScript.

  * Uw functie maakt gebruik van de **HTTP-trigger** sjabloon.

    De HTTP-trigger-sjabloon kunt accepteren inhoud waarvoor `application/json` type van uw logische app. Wanneer u een Azure-functie aan uw logische app toevoegt, toont de ontwerper van logische App aangepaste functies die zijn gemaakt met deze sjabloon binnen uw Azure-abonnement.

  * De functie aangepaste routes niet gebruiken tenzij u hebt gedefinieerd een [OpenAPI-definitie](../azure-functions/functions-openapi-definition.md) (voorheen bekend als een [Swagger-bestand](https://swagger.io/)).

  * Hebt u een OpenAPI-definitie voor uw functie, de ontwerper van logische Apps biedt een uitgebreidere ervaring wanneer u werkt met parameters van de functie. Voordat uw logische app kunt vinden en toegang tot functies met OpenAPI-definities [instellen van uw functie-app door de volgende stappen](#function-swagger).

* De logische app waaraan u wilt toevoegen van de functie, met inbegrip van een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) als de eerste stap in uw logische app

  Voordat u de acties die worden uitgevoerd van functies toevoegen kunt, wordt uw logische app moet beginnen met een trigger. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="function-swagger"></a>

## <a name="find-functions-that-have-openapi-descriptions"></a>Functies met OpenAPI beschrijvingen vinden

Voor een rijkere ervaring wanneer u met parameters van de functie in de ontwerper van logische Apps werkt [een OpenAPI-definitie genereren](../azure-functions/functions-openapi-definition.md), voorheen bekend als een [Swagger-bestand](https://swagger.io/), voor uw functie. Voor het instellen van uw functie-app, zodat uw logische app kunt vinden en gebruiken van functies met Swagger beschrijvingen, als volgt te werk:

1. Zorg ervoor dat uw functie-app actief wordt uitgevoerd.

1. In uw functie-app instellen [Cross-Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) zodat alle oorsprongen zijn toegestaan door de volgende stappen:

   1. Uit de **functie-Apps** , selecteert u de functie-app. Selecteer in het rechter deelvenster **platformfuncties** > **CORS**.

      ![Selecteer uw functie-app > 'Platformfuncties' > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

   1. Onder **CORS**, toevoegen van het sterretje ( **`*`** ) jokertekens teken, maar alle andere oorsprongen verwijderen in de lijst en kies **opslaan**.

      ![Stel ' CORS * voor het jokerteken ' * '](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

## <a name="access-property-values-inside-http-requests"></a>Toegang tot eigenschapswaarden in HTTP-aanvragen

Webhook-functies kunnen accepteren van HTTP-aanvragen als invoer en deze aanvragen doorsturen naar andere functies. Bijvoorbeeld, hoewel de Logic Apps heeft [functies waarmee u datum-/ tijdwaarden converteren](../logic-apps/workflow-definition-language-functions-reference.md), in dit eenvoudige voorbeeld JavaScript-functie laat zien hoe u toegang hebt tot een eigenschap in een Aanvraagobject voor de die wordt doorgegeven aan de functie en bewerkingen uitvoeren op de waarde van die eigenschap. Voor toegang tot de eigenschappen in objecten, in dit voorbeeld wordt de [punt (.)](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors):

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

Dit is wat er gebeurt binnen deze functie:

1. Hiermee maakt u de functie een `data` variabele en wijst de `body` object binnen de `request` object naar die variabele. De functie maakt gebruik van de punt (.) om te verwijzen naar de `body` object binnen de `request` object:

   ```javascript
   var data = request.body;
   ```

1. De functie hebben nu toegang tot de `date` eigenschap via de `data` variabele en converteren naar die eigenschapswaarde van het type datum/tijd naar DateString type door het aanroepen van de `ToDateString()` functie. De functie ook retourneert het resultaat via de `body` eigenschap in het antwoord van de functie:

   ```javascript
   body: data.date.ToDateString();
   ```

Nu dat u uw Azure-functie hebt gemaakt, volgt u de stappen voor het [functies toevoegen aan logische apps](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Functies in logische apps maken

Voordat u een Azure-functie binnen uw logische app met behulp van de Logic App Designer vanaf maken kunt, moet u eerst een Azure-functie-app, dat een container voor uw functies is hebben. Als u een functie-app niet hebt, maakt u eerst die functie-app. Zie [uw eerste functie maken in Azure portal](../azure-functions/functions-create-first-azure-function.md).

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

1. Het maken en uw functie toevoegen, volg de stappen die van toepassing op uw scenario is:

   * Kies onder de laatste stap in uw logic app-werkstroom, **nieuwe stap**.

   * Tussen bestaande stappen in uw logic app-werkstroom Beweeg de muis over de pijl, kies de plusknop (+) Meld u aan en selecteer vervolgens **een actie toevoegen**.

1. Typ 'azure functions' als filter in het zoekvak. Selecteer deze actie uit de lijst met acties: **Een Azure-functie kiezen - Azure Functions**

   ![Zoek 'Azure functions'](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. Selecteer uw functie-app in de functie App-lijst. Nadat de lijst met acties wordt geopend, selecteert u deze actie: **Azure Functions - maken nieuwe functie**

   ![Selecteer uw functie-app](./media/logic-apps-azure-functions/select-function-app-create-function.png)

1. In de editor van de definitie van functie door uw functie te definiëren:

   1. In de **functienaam** Geef een naam voor uw functie.

   1. In de **Code** Voeg uw code wordt de functiesjabloon voor de, inclusief de respons en de nettolading die u wilt dat aan uw logische app geretourneerd nadat de functie is voltooid.

      ![Uw functie definiëren](./media/logic-apps-azure-functions/function-definition.png)

      In de code van de sjabloon wordt de  *`context` object* verwijst naar het bericht dat uw logische app via verzendt de **aanvraagtekst** veld in een latere stap. Toegang krijgen tot de `context` van het object eigenschappen uit binnen uw functie, gebruik de volgende syntaxis:

      `context.body.<property-name>`

      Bijvoorbeeld, om te verwijzen naar de `content` eigenschap binnen de `context` object, gebruik de volgende syntaxis: 

      `context.body.content`

      De sjablooncode bevat ook een `input` -variabele, waarmee de waarde van slaat de `data` parameter zodat uw functie bewerkingen op deze waarde kunt uitvoeren. In JavaScript-functies, de `data` variabele is ook een snelkoppeling voor `context.body`.

      > [!NOTE]
      > De `body` hier van toepassing op de `context` object en is niet hetzelfde als de **hoofdtekst** -token van een actie-uitvoer, die u mogelijk ook worden doorgegeven in uw functie.

   1. Wanneer u klaar bent, kiest u **Maken**.

1. In de **aanvraagtekst** geeft u de functie de invoer van, die moet worden opgemaakt als een JavaScript Object Notation (JSON)-object.

   Deze invoer is de *context-object* of het bericht dat uw logische app naar uw functie verzendt. Wanneer u klikt op de **aanvraagtekst** veld, de lijst met dynamische inhoud wordt weergegeven, zodat u tokens voor uitvoer uit de vorige stappen selecteren kunt. In dit voorbeeld geeft aan dat de nettolading van de context een eigenschap met de naam bevat `content` waarvoor de **van** token de waarde van de e-mailtrigger:

   !["Aanvraagtekst" voorbeeld - context-object nettolading](./media/logic-apps-azure-functions/function-request-body-example.png)

   Hier het context-object is niet cast-conversie uitvoeren als een tekenreeks, zodat de inhoud van het object rechtstreeks naar de JSON-nettolading wordt toegevoegd. Als het context-object niet is een JSON-token dat wordt doorgegeven van een tekenreeks, een JSON-object of een JSON-matrix, krijgt u echter een fout. Als dit voorbeeld gebruikt de **ontvangen** token in plaats daarvan, u kunt casten het context-object als een tekenreeks door dubbele aanhalingstekens toe te voegen:  

   ![CAST-object als tekenreeks](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. Geef andere details, zoals de methode voor het gebruik, aanvraagheaders of queryparameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de opties die u wilt gebruiken.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Bestaande functies toevoegen aan logische apps

U kunt voor het aanroepen van bestaande Azure-functies van uw logische apps, Azure functions, zoals een andere actie toevoegen in de Logic App Designer.

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

1. Kies onder de stap waarin u wilt toevoegen van de functie, **nieuwe stap**, en selecteer **een actie toevoegen**.

1. Typ 'azure functions' als filter in het zoekvak. Selecteer deze actie uit de lijst met acties: **Een Azure-functie kiezen - Azure Functions**

   ![Zoek 'Azure functions'](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. Selecteer uw functie-app in de functie App-lijst. Nadat de functielijst met wordt weergegeven, selecteert u uw functie.

   ![Selecteer uw functie-app en de Azure-functie](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Voor functies die API-definities (Swagger beschrijvingen) hebben en [instellen zodat uw logische app kan vinden en toegang deze functies tot](#function-swagger), kunt u **Swagger-acties**:

   ![Selecteer uw functie-app, 'Swagger-acties' ', en uw Azure-functie](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

1. In de **aanvraagtekst** geeft u de functie de invoer van, die moet worden opgemaakt als een JavaScript Object Notation (JSON)-object.

   Deze invoer is de *context-object* of het bericht dat uw logische app naar uw functie verzendt. Wanneer u klikt op de **aanvraagtekst** veld, de lijst met dynamische inhoud wordt weergegeven, zodat u tokens voor uitvoer uit de vorige stappen selecteren kunt. In dit voorbeeld geeft aan dat de nettolading van de context een eigenschap met de naam bevat `content` waarvoor de **van** token de waarde van de e-mailtrigger:

   !["Aanvraagtekst" voorbeeld - context-object nettolading](./media/logic-apps-azure-functions/function-request-body-example.png)

   Hier het context-object is niet cast-conversie uitvoeren als een tekenreeks, zodat de inhoud van het object rechtstreeks naar de JSON-nettolading wordt toegevoegd. Als het context-object niet is een JSON-token dat wordt doorgegeven van een tekenreeks, een JSON-object of een JSON-matrix, krijgt u echter een fout. Als dit voorbeeld gebruikt de **ontvangen** token in plaats daarvan, u kunt casten het context-object als een tekenreeks door dubbele aanhalingstekens toe te voegen:

   ![CAST-object als tekenreeks](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. Geef andere details, zoals de methode voor het gebruik, aanvraagheaders of queryparameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de opties die u wilt gebruiken.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-azure-functions"></a>Logische apps aanroepen vanuit Azure functions

Als u wilt voor het activeren van een logische app uit in een Azure-functie, worden de logische app moet beginnen met een trigger die een aanroepbare eindpunt biedt. Bijvoorbeeld, kun u de logische app met de **HTTP**, **aanvragen**, **Azure Queues**, of **Event Grid** trigger. Binnen de functie een HTTP POST-aanvraag verzenden naar de URL van de trigger en bevatten de nettolading die u wilt dat deze logische app te verwerken. Zie voor meer informatie, [aanroepen, trigger of nesten van logische apps](../logic-apps/logic-apps-http-endpoint.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logic Apps-connectors](../connectors/apis-list.md)
