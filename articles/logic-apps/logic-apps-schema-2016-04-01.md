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
ms.openlocfilehash: 43fd52dd04e679b9756c07e8c6e260323469026a
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126199"
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

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
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

Zijn parameters die zijn gekoppeld aan één actie in het schema van vorige versies, voorwaarden en lussen. Dit schema omhoog worden getild deze beperking, zodat de voorwaarden en lussen worden weergegeven als actietypen. Meer informatie over [lussen en scopes](../logic-apps/logic-apps-loops-and-scopes.md), of dit eenvoudige voorbeeld voor een actie voorwaarde controleren:

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>

## <a name="runafter-property"></a>de eigenschap 'runAfter'

De `runAfter` vervangt de eigenschap `dependsOn`, hiervoor bieden meer nauwkeurigheid wanneer u de volgorde voor acties op basis van de status van vorige acties.

De `dependsOn` eigenschap is gelijk aan 'in de actie is uitgevoerd en het is voltooid', niet van belang hoe vaak u wilt uitvoeren van een actie, afhankelijk van of de vorige bewerking geslaagd is, mislukt of overgeslagen. De `runAfter` eigenschap die flexibel als een object dat Hiermee geeft u alle van de actienamen waarna het object wordt uitgevoerd. Deze eigenschap ook een matrix van statussen die geaccepteerd als triggers zijn gedefinieerd. Bijvoorbeeld, als u uitvoeren wilt nadat u stap voor stap een is geslaagd en ook na stap B is geslaagd of mislukt, u maakt dit `runAfter` eigenschap:

```
{
    "...",
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
    Echter, we **wordt ten zeerste aangeraden** u **OpslaanAls** om ervoor te zorgen dat alle verbindingsreferenties geldig in de bijgewerkte logische app zijn.

3. Kies in de werkbalk upgrade blade **OpslaanAls**.

4. Voer de naam van de logica en status. Voor het implementeren van uw bijgewerkte logische app, kies **maken**.

5. Controleer of uw bijgewerkte logische app werkt zoals verwacht.
   
   > [!NOTE]
   > Als u een aanvraag of handmatige trigger gebruikt, verandert de callback-URL in de nieuwe logische app. Test de nieuwe URL om te controleren of de end-to-end-ervaring werkt. Als u wilt behouden vorige URL's, kunt u via uw bestaande logische app klonen.

6. *Optionele* kiezen als u wilt uw vorige logische app te overschrijven met de nieuwe schemaversie op de werkbalk **kloon**, naast **Schema bijwerken**. Deze stap is alleen nodig als u wilt behouden dezelfde resource-ID of aanvraag-URL van de trigger van uw logische app.

### <a name="upgrade-tool-notes"></a>Opmerkingen bij de upgrade hulpprogramma

#### <a name="mapping-conditions"></a>Toewijzen van voorwaarden

In de definitie van de upgrade is uitgevoerd maakt het hulpprogramma een best-effort op true en false vertakking acties groeperen als een bereik. Met name de ontwerpfunctie patroon van `@equals(actions('a').status, 'Skipped')` moet worden weergegeven als een `else` actie. Als het hulpprogramma onherkenbare patronen detecteert, kan het hulpprogramma echter verschillende voorwaarden voor zowel de true als de waarde false vertakking maken. U kunt acties opnieuw toewijzen na de upgrade, indien nodig.

#### <a name="foreach-loop-with-condition"></a>'foreach-lus met voorwaarde

In het nieuwe schema, kunt u de filteractie gebruiken voor het repliceren van het patroon van een `foreach` lus met een voorwaarde per item, maar deze wijziging automatisch moet gebeuren wanneer u een upgrade uitvoert. De voorwaarde wordt een filteractie voordat de foreach-lus voor het retourneren van alleen een matrix met items die overeenkomen met de voorwaarde en deze reeks wordt doorgegeven in de foreach-actie. Zie voor een voorbeeld [lussen en scopes](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Resourcetags

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

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
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
