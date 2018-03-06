---
title: Parallelle vertakkingen - Azure Logic Apps | Microsoft Docs
description: Maken of koppelen van parallelle vertakkingen in logic apps
services: logic-apps
keywords: vertakkingen, parallel verwerken
documentationcenter: 
author: ecfan
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 9a836b707a576b9a938f43397ef35c00aeb476bf
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="create-or-join-parallel-branches-in-your-logic-app"></a>Maken of koppelen van parallelle vertakkingen in uw logische app

Standaard acties in een logische app worden opeenvolgend uitgevoerd. Bewerkingen uit te voeren onafhankelijke op hetzelfde moment, kunt u [parallelle vertakkingen](#parallel-branches), en vervolgens [join die vertakkingen](#join-branches) verderop in de stroom. 

> [!TIP] 
> Als u een trigger die een matrix ontvangt en een werkstroom voor elk matrixitem wilt uitvoeren hebt, kunt u *debatch* dat matrix met de [ **SplitOn** activeren eigenschap](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="parallel-branches"></a>

## <a name="add-a-parallel-branch"></a>Een parallelle vertakking toevoegen

Als u wilt uitvoeren onafhankelijk stappen op hetzelfde moment, kunt u parallelle vertakkingen naast een bestaande stap toevoegen. 

![Stappen parallel worden uitgevoerd](media/logic-apps-control-flow-branches/parallel.png)

Uw logische app wacht tot alle takken te voltooien voordat u doorgaat werkstroom.
Parallelle vertakkingen uitvoeren alleen wanneer hun `runAfter` eigenschapswaarden overeenkomen met de status van de voltooide bovenliggende stap. Bijvoorbeeld: beide `branchAction1` en `branchAction2` zijn ingesteld om uit te voeren, alleen wanneer de `parentAction` is voltooid met `Succeded` status.

> [!NOTE]
> Voordat u begint, moet uw logische app al een stap waarin u parallelle vertakkingen kunt toevoegen.

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u uw logische app in Logic App Designer.

2. Beweeg de muis over de pijl boven de stap waar u parallelle vertakkingen wilt.

3. Kies de **plus** teken (**+**), kiest u **toevoegen van een parallelle vertakking**, en selecteer het item dat u wilt toevoegen.

   ![Parallelle vertakking toevoegen](media/logic-apps-control-flow-branches/add-parallel-branch.png)

   Het geselecteerde item wordt nu weergegeven in een parallelle vertakking.

4. Voor elke vertakking parallelle, voegt u de stappen die u wilt. Verplaats voor een opeenvolgende actie toevoegen aan een parallelle vertakking, de muis onder de actie waar u de opeenvolgende actie toevoegen. Kies de **plus** (**+**) aanmelding en de stap die u wilt toevoegen.

   ![Stap aan parallelle vertakking toevoegen](media/logic-apps-control-flow-branches/add-sequential-action-parallel-branch.png)

5. Vertakkingen terug samen samenvoegen [deelnemen aan uw parallelle vertakkingen](#join-branches). 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Parallelle vertakking definitie (JSON)

Als u in de codeweergave werkt, kunt u definiëren de parallelle structuur in JSON-definitie van uw logische app in plaats daarvan, bijvoorbeeld:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<a name="join-branches"></a>

## <a name="join-parallel-branches"></a>Parallelle vertakkingen koppelen

Om parallelle vertakkingen samenvoegen, voegt u toe een stap onderaan onder alle takken. Deze stap wordt uitgevoerd nadat alle de einddatum van de parallelle vertakkingen uitgevoerd.

![Parallelle vertakkingen koppelen](media/logic-apps-control-flow-branches/join.png)

1. In de [Azure-portal](https://portal.azure.com), zoeken en openen van uw logische app in Logic App-ontwerper. 

2. Onder de parallelle vertakkingen die u wilt toevoegen, voeg de stap die u wilt uitvoeren.

   ![Een stap die lid wordt van parallelle vertakkingen toevoegen](media/logic-apps-control-flow-branches/join-steps.png)

   De parallelle vertakkingen zijn nu samengevoegd.

<a name="join-json"></a>

## <a name="join-definition-json"></a>Deelnemen aan de definitie van JSON)

Als u in de codeweergave werkt, kunt u definiëren de joinstructuur in JSON-definitie van uw logische app in plaats daarvan, bijvoorbeeld:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "joinAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "branchAction1": [
          "Succeeded"
        ],
        "branchAction2": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u wilt verzenden of stemmen over de functies en suggesties, gaat u naar de [Azure Logic Apps gebruiker feedback site](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Voer stappen uit op basis van Actiestatus van de gegroepeerde (scopes genoemd)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)