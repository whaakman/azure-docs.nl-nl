---
title: Logische apps met Hiermee mock-gegevens - Azure Logic Apps testen
description: Statische resultaten voor het testen van logische apps met Hiermee mock-gegevens zonder die betrekking hebben op de productie-omgevingen instellen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 23cce4d846cdf183f41b25663ba21d3bf1d27013
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57790997"
---
# <a name="test-logic-apps-with-mock-data-by-setting-up-static-results"></a>Logische apps met Hiermee mock-gegevens door het instellen van statische resultaten testen

Bij het testen van uw logische apps, kunt u mogelijk niet daadwerkelijk aanroepen of toegang tot apps, services en systemen om verschillende redenen. In deze scenario's mogelijk u meestal aan andere voorwaarde paden worden uitgevoerd, fouten forceren, bieden specifieke antwoord de berichttekst of zelfs Probeer enkele stappen wordt overgeslagen. Door het instellen van statische resultaten voor een actie in uw logische app, kunt u uitvoergegevens van deze actie model. Inschakelen van statische resultaten op een actie die de actie kan niet worden uitgevoerd, maar in plaats daarvan de Hiermee mock-gegevens geretourneerd.

Bijvoorbeeld, als u statische resultaten voor de Outlook 365 actie e-mail verzenden, de Logic Apps-engine retourneert alleen de mock gegevens die u hebt opgegeven als statische resultaten, in plaats van bij het aanroepen van Outlook en een e-mail verzenden.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u het instellen van statische resultaten

<a name="set-up-static-results"></a>

## <a name="set-up-static-results"></a>Statische resultaten instellen

1. Als u dat nog niet gedaan hebt, in de [Azure-portal](https://portal.azure.com), opent u uw logische app in de ontwerper van logische Apps.

1. Op de actie waar u het instellen van statische resultaten, de volgende stappen uit: 

   1. In de rechterbovenhoek van de actie, kiest u de weglatingstekens (*...* ) en selecteer **statische resultaat**, bijvoorbeeld:

      ![Selecteer 'Statische resultaat' > "Statische resultaat inschakelen"](./media/test-logic-apps-mock-data-static-results/select-static-result.png)

   1. Kies **inschakelen statische resultaat**. Geef de mock uitvoerwaarden die u wilt ophalen voor de reactie van de actie voor de eigenschappen vereist (*).

      Dit zijn bijvoorbeeld de vereiste eigenschappen voor de HTTP-actie:

      | Eigenschap | Description |
      |----------|-------------|
      | **Status** | De status van de actie moeten worden geretourneerd |
      | **Statuscode** | De specifieke statuscode om terug te keren |
      | **Headers** | De inhoud van de koptekst om terug te keren |
      |||

      ![Selecteer 'Statische resultaat inschakelen'](./media/test-logic-apps-mock-data-static-results/enable-static-result.png)

      Als u wilt de mock-gegevens in JavaScript Object Notation (JSON)-indeling invoeren, kies **overschakelen naar de JSON-modus** (![kiezen 'Overschakelen naar JSON-modus'](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)).

   1. Voor de optionele eigenschappen, opent u de **optionele velden selecteren** lijst en selecteer de eigenschappen die u wilt model.

      ![Selecteer de optionele eigenschappen](./media/test-logic-apps-mock-data-static-results/optional-properties.png)

1. Wanneer u klaar om op te slaan bent, kies **gedaan**.

   In de rechterbovenhoek van de actie klikken, de titelbalk wordt nu een pictogram van een test bekerglas (![pictogram voor statische resultaten](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)), wat aangeeft dat u statische resultaten hebt ingeschakeld.

   ![Pictogram weergeven statische resultaten ingeschakeld](./media/test-logic-apps-mock-data-static-results/static-results-enabled.png)

   Nadat uw logische app wordt uitgevoerd, in de uitvoeringsgeschiedenis van uw logische app, de **statische resultaten** kolom wordt weergegeven of een specifieke uitvoering is inbegrepen bij acties waarvoor statische resultaten die zijn ingeschakeld, bijvoorbeeld:

   ![Uitvoeringsgeschiedenis - statische resultatenkolom](./media/test-logic-apps-mock-data-static-results/run-history.png)

<a name="reuse-sample-outputs"></a>

## <a name="reuse-previous-outputs"></a>Hergebruik van vorige uitvoer

Als uw logische app een vorige is kunt uitvoeren met uitvoer die u opnieuw als mock-uitvoer gebruiken kunt, u kopiëren en plakken de uitvoer van die worden uitgevoerd.

1. Als u dat nog niet gedaan hebt, in de [Azure-portal](https://portal.azure.com), opent u uw logische app in de ontwerper van logische Apps.

1. Selecteer in het hoofdmenu van uw logische app, **overzicht**. 

1. In de **geschiedenis van uitvoeringen** sectie, selecteer de logische app die u wilt.

1. Zoek in de werkstroom van uw logische app, en vouw de actie die de uitvoer die u wilt.

1. Kies de **onbewerkte uitvoer weergeven** koppeling.

1. Kopieer de volledige JavaScript Object Notation (JSON)-object of de specifieke subsectie die u gebruiken wilt, bijvoorbeeld, de uitvoersectie of zelfs alleen de headers-sectie.

1. Volg de stappen voor het openen de **statische resultaat** vak voor de actie in [instellen op statisch resultaten](#set-up-static-results).

1. Na de **statische resultaat** vak wordt geopend, kiest u een stap:

   * Als u een volledige JSON-object, kies **overschakelen naar de JSON-modus** (![kiezen 'Overschakelen naar JSON-modus'](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)):

     !['Switch voor JSON-modus' voor volledig object kiezen](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-complete.png)

   * Als u alleen een sectie van het JSON, naast die sectielabel, kies **overschakelen naar de JSON-modus** voor die sectie, bijvoorbeeld:

     !['Switch voor JSON-modus' voor uitvoer kiezen](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-outputs.png)

1. Plak de eerder gekopieerde JSON in de JSON-editor.

   ![JSON-modus](./media/test-logic-apps-mock-data-static-results/json-editing-mode.png)

1. Wanneer u klaar bent, kiest u **Gereed**. Of, als u wilt terugkeren naar de ontwerpfunctie, kiezen **Switch bewerkingsmodus** (![kiezen "Bewerkingsmodus Switch"](./media/test-logic-apps-mock-data-static-results/switch-editor-mode-button.png)).

## <a name="disable-static-results"></a>Statische resultaten uitschakelen

Het uitschakelen van statische resultaten weggooien niet de waarden van de laatste installatie. Dus wanneer u de volgende keer statische resultaten inschakelt, kunt u doorgaan met behulp van uw vorige waarden.

1. De actie vinden waar u wilt uitschakelen statische uitvoer. In de rechterbovenhoek van de actie, kies het pictogram van de bekerglas test (![pictogram voor statische resultaten](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)).

   ![Statische resultaten uitschakelen](./media/test-logic-apps-mock-data-static-results/disable-static-results.png)

1. Kies **uitschakelen statische resultaat** > **gedaan**.

   ![Statische resultaten uitschakelen](./media/test-logic-apps-mock-data-static-results/disable-static-results-button.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Logic Apps](../logic-apps/logic-apps-overview.md)