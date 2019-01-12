---
title: Maken of lid van parallelle vertakkingen - Azure Logic Apps | Microsoft Docs
description: Maken of koppelen van parallelle vertakkingen voor werkstromen in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: 2e1c155a371fa96e4f772f632a9585948b012e54
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232240"
---
# <a name="create-or-join-parallel-branches-for-workflow-actions-in-azure-logic-apps"></a>Maken of lid van parallelle vertakkingen voor werkstroomacties in Azure Logic Apps

Standaard wordt uw acties in logic app workflows worden opeenvolgend uitgevoerd. Als u wilt onafhankelijke acties uitvoeren op hetzelfde moment, kunt u [parallelle vertakkingen](#parallel-branches), en vervolgens [deelnemen aan die vertakkingen](#join-branches) verderop in de stroom. 

> [!TIP] 
> Als u een trigger die een matrix ontvangt en een werkstroom voor elk matrixitem wilt uitvoeren, kunt u *debatch* matrix met de [ **SplitOn** activeren van de eigenschap](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="parallel-branches"></a>

## <a name="add-parallel-branch"></a>Parallelle vertakking toevoegen

Onafhankelijke stappen op hetzelfde moment uitgevoerd, kunt u parallelle vertakkingen naast een bestaande stap toevoegen. 

![Werk parallel worden uitgevoerd](media/logic-apps-control-flow-branches/parallel.png)

Uw logische app wacht tot alle vertakkingen te voltooien voordat u doorgaat werkstroom. Parallelle vertakkingen uitvoeren alleen wanneer hun `runAfter` eigenschapswaarden overeenkomen met de status van de voltooide bovenliggende stap. Bijvoorbeeld, beide `branchAction1` en `branchAction2` zijn ingesteld om uit te voeren alleen wanneer de `parentAction` is voltooid met `Succeeded` status.

> [!NOTE]
> Voordat u begint, moet uw logische app al een stap waarin u parallelle vertakkingen kunt toevoegen.

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u uw logische app in Logic App Designer.

1. Plaats de muisaanwijzer boven de pijl boven de stap waarin u wilt toevoegen van parallelle vertakkingen. Kies de **plus** teken (**+**) die wordt weergegeven, en kies vervolgens **een parallelle vertakking toevoegen**. 

   ![Parallelle vertakking toevoegen](media/logic-apps-control-flow-branches/add-parallel-branch.png)

1. Zoek in het zoekvak en selecteer de actie die u wilt.

   ![Zoek en selecteer de actie die u wilt](media/logic-apps-control-flow-branches/find-select-parallel-action.png)

   Nu de geselecteerde actie wordt bijvoorbeeld weergegeven in de parallelle vertakking:

   ![Zoek en selecteer de actie die u wilt](media/logic-apps-control-flow-branches/added-parallel-branch.png)

1. Voeg nu de stappen die u wilt dat toe in elke parallelle vertakking. Nog een actie toevoegen aan een vertakking, de aanwijzer onder de actie waar u wilt een sequentiële actie toevoegen. Kies de **plus** (**+**) teken dat wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

   ![Sequentiële actie aan parallelle vertakking toevoegen](media/logic-apps-control-flow-branches/add-sequential-action.png)

1. Zoek in het zoekvak en selecteer de actie die u wilt.

   ![Zoek en selecteer opeenvolgende actie](media/logic-apps-control-flow-branches/find-select-sequential-action.png)

   Nu de geselecteerde actie wordt bijvoorbeeld weergegeven in de huidige vertakking:

   ![Zoek en selecteer opeenvolgende actie](media/logic-apps-control-flow-branches/added-sequential-action.png)

Samen takken terug samenvoegen [deelnemen aan uw parallelle vertakkingen](#join-branches). 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Parallelle vertakking definitie (JSON)

Als u in de codeweergave werkt, kunt u definiëren de parallelle structuur in JSON-definitie van uw logische app in plaats daarvan, bijvoorbeeld:

``` json
{
  "triggers": {
    "myTrigger": {}
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": {},
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": {},
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": {},
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

## <a name="join-parallel-branches"></a>Deelnemen aan parallelle vertakkingen

Om parallelle vertakkingen samenvoegen, moet u alleen een stap toevoegen aan de onderkant onder alle vertakkingen. Deze stap wordt uitgevoerd nadat alle de einddatum van de parallelle vertakkingen uitgevoerd.

![Deelnemen aan parallelle vertakkingen](media/logic-apps-control-flow-branches/join.png)

1. In de [Azure-portal](https://portal.azure.com), zoeken en openen van uw logische app in Logic App Designer. 

1. Kies onder de parallelle vertakkingen die u wilt deelnemen, **nieuwe stap**. 

   ![Stap om toe te voegen toevoegen](media/logic-apps-control-flow-branches/add-join-step.png)

1. In het zoekvak, zoek en selecteer de actie die u wilt de stap die lid wordt van de branches.

   ![Zoek en selecteer de actie die lid wordt van parallelle vertakkingen](media/logic-apps-control-flow-branches/join-steps.png)

   Uw parallelle vertakkingen zijn nu samengevoegd.

   ![Gekoppelde vertakkingen](media/logic-apps-control-flow-branches/joined-branches.png)

<a name="join-json"></a>

## <a name="join-definition-json"></a>Lid worden van definitie (JSON)

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
* Als u wilt indienen of hierop stemmen op de functies en suggesties, gaat u naar de [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch-instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Voer stappen uit op basis van gegroepeerde Actiestatus (bereiken)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
