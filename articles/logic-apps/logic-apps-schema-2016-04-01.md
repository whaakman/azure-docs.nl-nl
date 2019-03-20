---
title: Schema-updates 1 juni 2016 - Azure Logic Apps | Microsoft Docs
description: Bijgewerkte schemaversie 2016-06-01 voor definities voor logische apps in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.topic: article
ms.date: 07/25/2016
ms.openlocfilehash: 6df29543df2b7b2609582f7e8dd9a0629182760c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57849773"
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Schema-updates voor Azure Logic Apps - 1 juni 2016

De [schema bijgewerkt](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json) en API-versie voor Azure Logic Apps bevat belangrijke verbeteringen die logische apps nog betrouwbaar en eenvoudiger te gebruiken:

* [Scopes](#scopes) kunt u groeperen of acties worden genest als een verzameling van acties.
* [Voorwaarden en lussen](#conditions-loops) zijn nu eersteklas acties.
* Exacte volgorde voor het uitvoeren van acties met de `runAfter` eigenschap vervangen `dependsOn`

Upgraden van uw logische apps van het schema van de Preview-versie 1 augustus 2015 naar het schema op 1 juni 2016 [Ga naar de sectie upgrade](#upgrade-your-schema).

<a name="scopes"></a>

## <a name="scopes"></a>Bereiken

Dit schema bevat bereiken, waarmee u groep samen of geneste acties binnen elkaar worden verbonden kunnen. Een voorwaarde kan bijvoorbeeld een voorwaarde bevatten. Meer informatie over [syntaxis van de scope](../logic-apps/logic-apps-loops-and-scopes.md), of bekijk dit voorbeeld basic bereik:

```json
{
   "actions": {
      "Scope": {
         "type": "Scope",
         "actions": {                
            "Http": {
               "inputs": {
                   "method": "GET",
                   "uri": "https://www.bing.com"
               },
               "runAfter": {},
               "type": "Http"
            }
         }
      }
   }
}
```

<a name="conditions-loops"></a>

## <a name="conditions-and-loops-changes"></a>Voorwaarden en lussen wijzigingen

Zijn parameters die zijn gekoppeld aan één actie in het schema van vorige versies, voorwaarden en lussen. Dit schema omhoog worden getild deze beperking, zodat de voorwaarden en lussen nu beschikbaar als actietypen zijn. Meer informatie over [lussen en scopes](../logic-apps/logic-apps-loops-and-scopes.md), [voorwaarden](../logic-apps/logic-apps-control-flow-conditional-statement.md), of bekijk dit eenvoudige voorbeeld waarin een voorwaarde-actie:

```json
{
   "Condition - If trigger is some trigger": {
      "type": "If",
      "expression": "@equals(triggerBody(), '<trigger-name>')",
      "runAfter": {},
      "actions": {
         "Http_2": {
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com"
            },
            "runAfter": {},
            "type": "Http"
         }
      },
      "else": 
      {
         "Condition - If trigger is another trigger": {}
      }  
   }
}
```

<a name="run-after"></a>

## <a name="runafter-property"></a>de eigenschap 'runAfter'

De `runAfter` vervangt de eigenschap `dependsOn`, hiervoor bieden meer nauwkeurigheid wanneer u de volgorde voor acties op basis van de status van vorige acties. De `dependsOn` eigenschap wordt aangegeven of 'de actie is uitgevoerd en is voltooid', op basis van de vorige actie is geslaagd, mislukt of overgeslagen - niet het aantal keren dat u wilt uitvoeren. De `runAfter` eigenschap biedt flexibiliteit als een object dat Hiermee geeft u alle actie namen nadat het object wordt uitgevoerd. Deze eigenschap ook een matrix van statussen die geaccepteerd als triggers zijn gedefinieerd. Bijvoorbeeld, als u een actie uit te voeren wilt nadat een actie is geslaagd en ook na actie B is geslaagd of mislukt, instellen van dit `runAfter` eigenschap:

```json
{
   // Other parts in action definition
   "runAfter": {
      "A": ["Succeeded"],
      "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Uw schema bijwerken

Upgrade uitvoeren naar de [meest recente schema](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json), hoeft u slechts een paar stappen te ondernemen. Het upgradeproces bevat waarop de upgrade-script wordt uitgevoerd als een nieuwe logische app opslaan en als u wilt, mogelijk overschrijven van de vorige logische app.

1. Open uw logische app in Azure portal.

2. Ga naar **overzicht**. Kies op de werkbalk van de logische app **Schema bijwerken**.
   
   ![Kies Schema bijwerken][1]
   
   De definitie van de bijgewerkte wordt geretourneerd, die u kunt kopiëren en plakken in de resourcedefinitie van een, indien nodig. 

   > [!IMPORTANT]
   > *Zorg ervoor dat* u **OpslaanAls** zodat alle verbindingsreferenties geldig in de bijgewerkte logische app blijven.

3. Kies in de werkbalk upgrade blade **OpslaanAls**.

4. Voer de naam van de logica en status. Voor het implementeren van uw bijgewerkte logische app, kies **maken**.

5. Controleer of uw bijgewerkte logische app werkt zoals verwacht.
   
   > [!NOTE]
   > Als u een aanvraag of handmatige trigger gebruikt, verandert de callback-URL in de nieuwe logische app. Test de nieuwe URL om te controleren of de end-to-end-ervaring werkt. Als u wilt behouden vorige URL's, kunt u via uw bestaande logische app klonen.

6. *Optionele* kiezen als u wilt uw vorige logische app te overschrijven met de nieuwe schemaversie op de werkbalk **kloon**, naast **Schema bijwerken**. Deze stap is alleen nodig als u wilt behouden dezelfde resource-ID of aanvraag-URL van de trigger van uw logische app.

## <a name="upgrade-tool-notes"></a>Opmerkingen bij de upgrade hulpprogramma

### <a name="mapping-conditions"></a>Toewijzen van voorwaarden

In de definitie van de upgrade is uitgevoerd maakt het hulpprogramma de best-effort op true en false vertakking acties groeperen als een bereik. Met name de ontwerpfunctie patroon van `@equals(actions('a').status, 'Skipped')` wordt weergegeven als een `else` actie. Als het hulpprogramma onherkenbare patronen detecteert, kan het hulpprogramma echter verschillende voorwaarden voor zowel de true als de waarde false vertakking maken. U kunt acties opnieuw toewijzen na de upgrade, indien nodig.

#### <a name="foreach-loop-with-condition"></a>'foreach-lus met voorwaarde

In het nieuwe schema, kunt u de filteractie gebruiken voor het repliceren van het patroon die gebruikmaakt van een **voor elk** lus met één voorwaarde per item. De wijziging gebeurt echter automatisch wanneer u een upgrade uitvoert. De voorwaarde wordt een actie die wordt weergegeven vóór de **voor elk** lus retourneert alleen een matrix met items die overeenkomen met de voorwaarde, en door te geven die matrix **voor elk** actie. Zie voor een voorbeeld [lussen en scopes](../logic-apps/logic-apps-loops-and-scopes.md).

### <a name="resource-tags"></a>Resourcetags

Na de upgrade, worden resourcetags verwijderd, zodat u ze voor de bijgewerkte werkstroom moet opnieuw ingesteld.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="renamed-manual-trigger-to-request-trigger"></a>Hernoemd 'manual' trigger ' aanvraagtrigger '

De `manual` triggertype is afgeschaft en gewijzigd in `request` met het type `http`. Deze wijziging meer consistentie van het type patroon maakt die de trigger wordt gebruikt om te bouwen.

### <a name="new-filter-action"></a>Nieuwe actie voor 'filter'

Voor het filteren van een grote matrix omlaag naar een kleiner aantal items, de nieuwe `filter` type accepteert een matrix en een voorwaarde wordt geëvalueerd als de voorwaarde voor elk item en retourneert een matrix met items die voldoen aan de voorwaarde.

### <a name="restrictions-for-foreach-and-until-actions"></a>Beperkingen voor 'foreach' en 'tot'-acties

De `foreach` en `until` lus zijn beperkt tot één actie.

### <a name="new-trackedproperties-for-actions"></a>Nieuwe 'trackedProperties' voor acties

Acties hebt nu een extra eigenschap, genaamd `trackedProperties`, die op hetzelfde niveau aan is de `runAfter` en `type` eigenschappen. Dit object bevat een bepaalde actie-invoer of uitvoer die u opnemen in de Azure diagnostische telemetrie wilt, verzonden als onderdeel van een werkstroom. Bijvoorbeeld:

``` json
{
   "Http": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com"
      },
      "runAfter": {},
      "type": "Http",
      "trackedProperties": {
         "responseCode": "@action().outputs.statusCode",
         "uri": "@action().inputs.uri"
      }
   }
}
```

## <a name="next-steps"></a>Volgende stappen
* [Werkstroomdefinities voor logische apps maken](../logic-apps/logic-apps-author-definitions.md)
* [Sjablonen voor implementatie voor logische apps maken](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
