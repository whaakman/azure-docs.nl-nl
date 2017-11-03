---
title: Schema-updates 1 juni 2016 - Azure Logic Apps | Microsoft Docs
description: JSON-definities voor Azure Logic Apps maken met schemaversie 2016-06-01
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 43df04d6478e44c82c88b17d916cfc9fe4afc03e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Schema-updates voor Azure Logic Apps - 1 juni 2016

Deze nieuwe schema en de API-versie voor Azure Logic Apps bevat belangrijke verbeteringen die logische apps betrouwbaarder en eenvoudiger te gebruiken:

* [Scopes](#scopes) kunt u een groep of acties nesten als een verzameling van acties.
* [Voorwaarden en lussen](#conditions-loops) zijn nu klas acties.
* Nauwkeurigere ordening voor het uitvoeren van acties met de `runAfter` eigenschap vervangen`dependsOn`

Upgrade van uw logische apps van het schema van de preview 1 augustus 2015 aan het schema 1 juni 2016 [Raadpleeg de sectie upgrade](##upgrade-your-schema).

<a name="scopes"></a>
## <a name="scopes"></a>Scopes

Dit schema bevat bereiken, waarmee u groep samen of nest acties in de andere. Een voorwaarde kan bijvoorbeeld een voorwaarde bevatten. Meer informatie over [bereik syntaxis](../logic-apps/logic-apps-loops-and-scopes.md), of in dit voorbeeld basic bereik bekijken:

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

Zijn parameters die zijn gekoppeld aan één actie in het schema van vorige versies, de voorwaarden en lussen. Dit schema liftonderhoud deze beperking, zodat de voorwaarden en lussen nu worden weergegeven als actietypen. Meer informatie over [lussen en -scopes](../logic-apps/logic-apps-loops-and-scopes.md), of dit eenvoudige voorbeeld voor een actie voorwaarde bekijken:

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

De `runAfter` eigenschap vervangt `dependsOn`, mits nauwkeuriger wanneer u de volgorde voor acties opgeven op basis van de status van de vorige acties.

De `dependsOn` eigenschap is gelijk aan 'de actie is uitgevoerd en is geslaagd', niet van belang hoe vaak u wilt uitvoeren van een actie, op basis van of de vorige bewerking geslaagd is, mislukt of overgeslagen. De `runAfter` eigenschap die flexibel als een object dat Hiermee geeft u alle actienamen waarna het object wordt uitgevoerd. Deze eigenschap bepaalt ook een matrix van statussen die geaccepteerd als triggers worden. Bijvoorbeeld, als u uitvoeren wilt nadat de stap voor stap een kan worden uitgevoerd en ook na stap B is gelukt of mislukt, u samenstellen dit `runAfter` eigenschap:

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

Een upgrade naar het nieuwe schema duurt slechts een paar stappen uitvoeren. Het upgradeproces bevat het upgrade-script uitgevoerd op te slaan als een nieuwe logische app, en als u wilt, mogelijk wordt de vorige logische app overschreven.

1. Open uw logische app in de Azure-portal.

2. Ga naar **overzicht**. Kies op de werkbalk van de app logica **Schema bijwerken**.
   
    ![Kies Schema bijwerken][1]
   
    De bijgewerkte definitie wordt geretourneerd, die u kunt kopiëren en plakken in de resourcedefinitie van een, indien nodig. 
    Maar we **aangeraden** u **OpslaanAls** om ervoor te zorgen dat alle verbinding verwijzingen geldig in de bijgewerkte logische app zijn.

3. Kies in de werkbalk upgrade blade **OpslaanAls**.

4. Voer de naam van de logica en status. Als u wilt uw bijgewerkte logische app implementeren, kies **maken**.

5. Controleer of uw bijgewerkte logische app werkt zoals verwacht.
   
   > [!NOTE]
   > Als u van een trigger handmatig of aanvraag gebruikmaakt, wordt de callback-URL gewijzigd in de nieuwe logische app. Test de nieuwe URL om te controleren of de end-to-end-ervaring werkt. Als u wilt behouden vorige URL's, kunt u via uw bestaande logische app klonen.

6. *Optionele* kiezen als u wilt uw vorige logische app overschrijven met de nieuwe schemaversie op de werkbalk **kloon**naast **Schema bijwerken**. Deze stap is alleen nodig als u wilt behouden dezelfde resource-ID of de aanvraag-URL van uw logische app trigger.

### <a name="upgrade-tool-notes"></a>Opmerkingen bij de upgrade hulpprogramma

#### <a name="mapping-conditions"></a>Toewijzing van voorwaarden

In de bijgewerkte definitie maakt het hulpprogramma een zo goed mogelijke poging op true en false vertakking acties groeperen als bereik. In het bijzonder de ontwerpfunctie patroon van `@equals(actions('a').status, 'Skipped')` moet worden weergegeven als een `else` in te grijpen. Als het hulpprogramma onherkenbare patronen detecteert, kan het hulpprogramma echter afzonderlijke voorwaarden voor zowel de true als de vertakking ONWAAR maken. U kunt acties opnieuw toewijzen na de upgrade, indien nodig.

#### <a name="foreach-loop-with-condition"></a>'foreach' lus met voorwaarde

In het nieuwe schema, kunt u de filteractie voor replicatie van het patroon van een `foreach` lus met een voorwaarde per object, maar deze wijziging automatisch moet gebeuren wanneer u een upgrade uitvoert. De voorwaarde wordt een filteractie voordat de foreach-lus voor het retourneren van een reeks items die overeenkomen met de voorwaarde en deze reeks wordt doorgegeven in de foreach-actie. Zie voor een voorbeeld [lussen en -scopes](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Resourcetags

Na de upgrade, worden resourcetags verwijderd, zodat u ze voor de bijgewerkte workflow moet opnieuw instellen.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="renamed-manual-trigger-to-request-trigger"></a>Nieuwe naam 'manual' trigger 'aanvraag' activeren

De `manual` triggertype is afgeschaft en gewijzigd in `request` met type `http`. Deze wijziging meer van de consistentie van het soort patroon maakt die de trigger wordt gebruikt om samen te stellen.

### <a name="new-filter-action"></a>Nieuwe 'filter' actie

Voor het filteren van een grote matrix naar beneden op een kleiner aantal items, de nieuwe `filter` type accepteert een matrix en een voorwaarde, de voorwaarde voor elk item wordt geëvalueerd en resulteert in een matrix met items die voldoen aan de voorwaarde.

### <a name="restrictions-for-foreach-and-until-actions"></a>Beperkingen voor 'foreach' en 'tot' acties

De `foreach` en `until` lus zijn beperkt tot één actie.

### <a name="new-trackedproperties-for-actions"></a>Nieuwe 'trackedProperties' voor acties

Acties hebt nu een extra eigenschap met de naam `trackedProperties`, namelijk op hetzelfde niveau naar de `runAfter` en `type` eigenschappen. Dit object geeft een bepaalde actie in- of uitgangen die u opnemen in de Azure diagnostische telemetrie wilt, verzonden als onderdeel van een werkstroom. Bijvoorbeeld:

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
* [Werkstroomdefinities voor logic apps maken](../logic-apps/logic-apps-author-definitions.md)
* [Van implementatiesjablonen maken voor logische apps](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
