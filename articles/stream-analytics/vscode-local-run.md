---
title: Azure Stream Analytics-query's met Visual Studio Code (Preview) lokaal testen
description: Dit artikel wordt beschreven hoe u voor het testen van query's lokaal met Azure Stream Analytics-hulpprogramma's voor Visual Studio Code.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: f477a0f99c3eaa82568d8188bfaae03818fb72dc
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827948"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio-code"></a>Stream Analytics-query's met Visual Studio Code lokaal testen

U kunt Azure Stream Analytics-hulpprogramma's voor Visual Studio Code gebruiken voor het testen van uw Stream Analytics-taken lokaal met voorbeeldgegevens.

Gebruik deze [snelstartgids](quick-create-vs-code.md) voor informatie over het maken van een Stream Analytics-taak met behulp van Visual Studio Code.

## <a name="run-queries-locally"></a>Lokaal uitvoeren van query 's

U kunt de Azure Stream Analytics-extensie voor Visual Studio Code gebruiken voor het testen van uw Stream Analytics-taken lokaal met voorbeeldgegevens.

1. Zodra u uw Stream Analytics-taak hebt gemaakt, gebruikt u **Ctrl + Shift + P** het opdrachtenpalet te openen. Typ en selecteer vervolgens **ASA: Invoer toevoegen**.

    ![ASA invoer toevoegen in Visual Studio code](./media/vscode-local-run/add-input.png)

2. Selecteer **lokale invoer**.

    ![Lokale ASA-invoer toevoegen in Visual Studio code](./media/vscode-local-run/add-local-input.png)

3. Selecteer **+ nieuwe lokale invoer**.

    ![Een nieuwe ASA lokale invoer in Visual Studio code toevoegen](./media/vscode-local-run/add-new-local-input.png)

4. Voer dezelfde invoer alias die u hebt gebruikt in uw query.

    ![Een nieuwe ASA lokale invoeralias toevoegen](./media/vscode-local-run/new-local-input-alias.png)

5. In de **LocalInput_DefaultLocalStream.json** bestand, voer het pad waar het lokale bestand zich bevindt.

    ![Lokaal bestandspad invoeren in Visual Studio](./media/vscode-local-run/local-file-path.png)

6. Ga terug naar uw query-editor, en selecteer **lokaal uitvoeren**.

    ![Lokaal uitvoeren selecteren in de query-editor](./media/vscode-local-run/run-locally.png)

## <a name="next-steps"></a>Volgende stappen

* [Een taak in de cloud Azure Stream Analytics maken in Visual Studio Code (Preview)](quick-create-vs-code.md)

* [Verken Azure Stream Analytics-taken met Visual Studio Code (Preview)](vscode-explore-jobs.md)
