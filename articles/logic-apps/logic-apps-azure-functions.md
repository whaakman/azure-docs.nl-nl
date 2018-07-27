---
title: Toevoegen en aangepaste code wordt uitgevoerd in Azure Logic Apps met Azure Functions | Microsoft Docs
description: Meer informatie over het toevoegen en aangepaste codefragmenten uitvoeren in Azure Logic Apps met Azure Functions
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/25/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 20ad738541554279ff9fd6dd6babe90a38676c00
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263187"
---
# <a name="add-and-run-custom-code-snippets-in-azure-logic-apps-with-azure-functions"></a>Toevoegen en aangepaste codefragmenten uitvoeren in Azure Logic Apps met Azure Functions

Als u wilt maken en alleen voldoende code uitvoert die gericht is op een specifiek probleem in uw logische apps, kunt u uw eigen functies maken met behulp van [Azure Functions](../azure-functions/functions-overview.md). Deze service biedt de mogelijkheid voor het maken en uitvoeren van aangepaste codefragmenten die zijn geschreven met Node.js of C# in uw logische apps zonder u zorgen te maken over het maken van een volledige app of de infrastructuur voor het uitvoeren van uw code. Azure Functions biedt serverloze computing in de cloud en is handig voor het uitvoeren van taken, zoals deze voorbeelden:

* Gedrag van uw logische app uitbreiden met functies worden ondersteund door de Node.js- of C#.
* Uitvoeren van berekeningen in uw werkstroom voor logische Apps.
* Geavanceerde opmaak toepassen of compute-velden in uw logische apps.

U kunt ook [logische apps aanroepen in Azure functions](#call-logic-app).

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen in dit artikel, zijn dit de items die u nodig hebt:

* Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>. 

* De logische app waar u de functie toevoegen

  Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) als de eerste stap in uw logische app 

  Voordat u de acties voor het uitvoeren van functies toevoegen kunt, wordt uw logische app moet beginnen met een trigger.

* Een Azure-functie-app, dat een container voor Azure functions en uw Azure-functie is. Als u een functie-app niet hebt, moet u [maakt u eerst uw functie-app](../azure-functions/functions-create-first-azure-function.md). U kunt de functie vervolgens ofwel maken [afzonderlijk buiten uw logische app](#create-function-external), of [van binnen uw logische app](#create-function-designer) in Logic App Designer.

  Zowel nieuwe en bestaande Azure-functie-apps en functies hebben dezelfde vereisten voor het werken met uw logische apps:

  * Uw functie-app moet behoren tot hetzelfde Azure-abonnement als uw logische app.

  * De functie moet gebruiken de **generieke webhook** functiesjabloon voor **JavaScript** of **C#**. Deze sjabloon kunt accepteren inhoud waarvoor `application/json` type van uw logische app. Deze sjablonen kunt u ook de Logic App Designer zoeken en weergeven van de aangepaste functies die u met deze sjablonen maakt wanneer u deze functies aan uw logische apps toevoegt.

  * Controleer of uw functiesjabloon **modus** eigenschap is ingesteld op **Webhook** en de **webhooktype** eigenschap is ingesteld op **generieke JSON**.

    1. Meld u aan bij <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.
    2. Selecteer in het hoofdmenu van Azure **functie-Apps**. 
    3. In de **functie-Apps** lijst, selecteer uw functie-app, vouw de functie uit en selecteer **integreren**. 
    4. Controleren van uw sjabloon **modus** eigenschap is ingesteld op **Webhook** en dat de **webhooktype** eigenschap is ingesteld op **generieke JSON**. 

  * Als uw functie heeft een [API-definitie](../azure-functions/functions-openapi-definition.md), voorheen bekend als een [Swagger-bestand](http://swagger.io/), de ontwerper van logische Apps biedt een uitgebreidere ervaring voor het werken met de parameters van de functie. 
  Voordat uw logische app kunt vinden en toegang tot functies waarvoor Swagger beschrijvingen, [instellen van uw functie-app door de volgende stappen](#function-swagger).

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>Functies buiten logische apps maken

In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, maken van uw Azure-functie-app, die moet zijn van hetzelfde Azure-abonnement als uw logische app en maak vervolgens uw Azure-functie. Als u geen ervaring met Azure Functions, krijgt u informatie over het [uw eerste functie maken in Azure portal](../azure-functions/functions-create-first-azure-function.md), maar houd er rekening mee deze vereisten voor het maken van Azure-functies die u kunt toevoegen en aanroepen vanuit logic apps.

* Zorg ervoor dat u selecteert de **generieke webhook** functiesjabloon voor **JavaScript** of **C#**.

  ![Generieke webhook - JavaScript of C#](./media/logic-apps-azure-functions/generic-webhook.png)

* Nadat u uw Azure-functie maakt, controleert u die van de sjabloon **modus** en **webhooktype** eigenschappen juist zijn ingesteld.

  1. In de **functie-Apps** lijst, vouw de functie uit en selecteer **integreren**. 

  2. Controleer of van uw sjabloon **modus** eigenschap is ingesteld op **Webhook** en dat de **webhooktype** eigenschap is ingesteld op **generieke JSON**. 

     ![Eigenschappen van uw functiesjabloon "Integreren"](./media/logic-apps-azure-functions/function-integrate-properties.png)

<a name="function-swagger"></a>

* (Optioneel) als u [een API-definitie genereren](../azure-functions/functions-openapi-definition.md), voorheen bekend als een [Swagger-bestand](http://swagger.io/), voor uw functie, krijgt u een rijkere ervaring wanneer u met parameters van de functie in de ontwerper van logische Apps werkt. Uw functie-app instellen zodat uw logische app kan vinden en toegang tot functies die Swagger beschrijvingen hebben:

  * Zorg ervoor dat uw functie-app actief wordt uitgevoerd.

  * In uw functie-app instellen [Cross-Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) zodat alle oorsprongen zijn toegestaan:

    1. Vanaf de **functie-Apps** , selecteert u de functie-app > **platformfuncties** > **CORS**.

       ![Selecteer uw functie-app > 'Platformfuncties' > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

    2. Onder **CORS**, voeg de `*` jokertekens teken, maar alle andere oorsprongen verwijderen in de lijst en kies **opslaan**.

       ![Selecteer uw functie-app > 'Platformfuncties' > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

### <a name="access-property-values-inside-http-requests"></a>Toegang tot eigenschapswaarden in HTTP-aanvragen

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

2. De functie hebben nu toegang tot de `date` eigenschap via de `data` variabele en converteren naar die eigenschapswaarde van het type datum/tijd naar DateString type door het aanroepen van de `ToDateString()` functie. De functie ook retourneert het resultaat via de `body` eigenschap in het antwoord van de functie: 

   ```javascript
   body: data.date.ToDateString();
   ```

Nu dat u uw Azure-functie hebt gemaakt, volgt u de stappen voor het [functies toevoegen aan logische apps](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Functies in logische apps maken

Voordat u een Azure-functie binnen uw logische app in de Logic App Designer vanaf maken kunt, moet u eerst een Azure-functie-app, dat een container voor uw functies is hebben. Als u een functie-app niet hebt, maakt u eerst die functie-app. Zie [uw eerste functie maken in Azure portal](../azure-functions/functions-create-first-azure-function.md). 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u uw logische app in Logic App Designer. 

2. Kies onder de stap waarin u wilt maken en toevoegen van de functie, **nieuwe stap** > **een actie toevoegen**. 

3. Typ 'azure functions' als filter in het zoekvak.
Selecteer in de lijst met acties met deze actie: **kiest u een Azure-functie - Azure Functions** 

   ![Zoek 'Azure functions'](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Selecteer uw functie-app in de functie App-lijst. Nadat de acties weer wordt geopend, selecteert u deze actie: **Azure Functions - nieuwe functie maken**

   ![Selecteer uw functie-app](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. In de editor van de definitie van functie door uw functie te definiëren:

   1. In de **functienaam** Geef een naam voor uw functie. 

   2. In de **Code** Voeg uw functiecode aan te geven aan de sjabloon, met inbegrip van de reactie en u wilt dat de nettolading die wordt geretourneerd aan uw logische app na voltooiing van de functie uitgevoerd wordt. 
   Het context-object in de sjablooncode worden beschreven voor het bericht en de inhoud die uw logische app wordt doorgegeven aan uw functie, bijvoorbeeld:

      ![Uw functie definiëren](./media/logic-apps-azure-functions/function-definition.png)

      Binnen uw functie, kunt u verwijzen naar de eigenschappen in de context-object met behulp van deze syntaxis:

      ```text
      context.<token-name>.<property-name>
      ```
      Hier volgt de syntaxis die u wilt gebruiken voor dit voorbeeld:

      ```text
      context.body.content
      ```

   3. Wanneer u klaar bent, kiest u **Maken**.

6. In de **aanvraagtekst** , geeft u het context-object om door te geven als de invoer van de functie, die moet worden opgemaakt in JavaScript Object Notation (JSON). Wanneer u klikt op de **aanvraagtekst** in het, de lijst met dynamische inhoud wordt geopend zodat u tokens voor beschikbare eigenschappen uit de vorige stappen selecteren kunt. 

   In dit voorbeeld geeft het object in de **hoofdtekst** -token van de e-mailtrigger:  

   !["Aanvraagtekst" voorbeeld - context-object nettolading](./media/logic-apps-azure-functions/function-request-body-example.png)

   Op basis van de inhoud in de context-object, genereert de ontwerper van logische App een functiesjabloon die u kunt vervolgens inline bewerken. 
   Logic Apps maakt ook variabelen op basis van de invoer context-object.

   In dit voorbeeld wordt het context-object is niet cast-conversie uitvoeren als een tekenreeks, zodat de inhoud wordt direct toegevoegd aan de JSON-nettolading. 
   Als het object is niet een JSON-token een tekenreeks, een JSON-object of een JSON-matrix moet, krijgt u echter een fout. 
   Als u wilt het context-object als een tekenreeks cast-conversie uitvoeren, moet u dubbele aanhalingstekens, bijvoorbeeld toevoegen:

   ![CAST-object als tekenreeks](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. Geef andere details, zoals de methode voor het gebruik, aanvraagheaders of queryparameters, kies **geavanceerde opties weergeven**.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Bestaande functies toevoegen aan logische apps

U kunt voor het aanroepen van bestaande Azure-functies van uw logische apps, Azure functions, zoals een andere actie toevoegen in de Logic App Designer. 

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u uw logische app in Logic App Designer. 

2. Kies onder de stap waarin u wilt toevoegen van de functie, **nieuwe stap** > **een actie toevoegen**. 

3. Typ 'azure functions' als filter in het zoekvak.
Selecteer in de lijst met acties met deze actie: **kiest u een Azure-functie - Azure Functions** 

   ![Zoek 'Azure functions'](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Selecteer uw functie-app in de functie App-lijst. Nadat de functielijst met wordt weergegeven, selecteert u uw functie. 

   ![Selecteer uw functie-app en de Azure-functie](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Voor functies waarvoor API-definities (Swagger beschrijvingen) en die zijn [instellen zodat uw logische app kan vinden en toegang deze functies tot](#function-swagger), kunt u **Swagger-acties**:

   ![Selecteer uw functie-app, 'Swagger-acties' ', en uw Azure-functie](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. In de **aanvraagtekst** , geeft u het context-object om door te geven als de invoer van de functie, die moet worden opgemaakt in JavaScript Object Notation (JSON). Deze context-object beschrijving van het bericht en de inhoud die uw logische app naar uw functie verzendt. 

   Wanneer u klikt op de **aanvraagtekst** in het, de lijst met dynamische inhoud wordt geopend zodat u tokens voor beschikbare eigenschappen uit de vorige stappen selecteren kunt. 
   In dit voorbeeld geeft het object in de **hoofdtekst** -token van de e-mailtrigger:

   !["Aanvraagtekst" voorbeeld - context-object nettolading](./media/logic-apps-azure-functions/function-request-body-example.png)

   In dit voorbeeld wordt het context-object is niet cast-conversie uitvoeren als een tekenreeks, zodat de inhoud wordt direct toegevoegd aan de JSON-nettolading. 
   Als het object is niet een JSON-token een tekenreeks, een JSON-object of een JSON-matrix moet, krijgt u echter een fout. 
   Als u wilt het context-object als een tekenreeks cast-conversie uitvoeren, moet u dubbele aanhalingstekens, bijvoorbeeld toevoegen:

   ![CAST-object als tekenreeks](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. Geef andere details, zoals de methode voor het gebruik, aanvraagheaders of queryparameters, kies **geavanceerde opties weergeven**.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Logische apps aanroepen van functies

Voor het activeren van een logische app uit in een Azure-functie, moet deze logische app een aanroepbare eindpunten zijn, of meer specifiek, een **aanvragen** trigger. Van binnen uw functie, verzend vervolgens een HTTP POST-aanvraag naar de URL voor dat **aanvraag** activeren en het opnemen van de nettolading die u wilt dat deze logische app te verwerken. Zie voor meer informatie, [aanroepen, trigger of nesten van logische apps](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logic Apps-connectors](../connectors/apis-list.md)
