---
title: Verken Azure Stream Analytics met Visual Studio Code (Preview)
description: Dit artikel ziet u hoe u een Azure Stream Analytics-taak exporteren naar een lokale project, lijst met taken en entiteiten van de taak weergeven, en stel een CI/CD-pijplijn voor uw Stream Analytics-taak.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/06/2019
ms.topic: conceptual
ms.openlocfilehash: 640a81fcb94194e2e907e9339f016bd35e279fdd
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079208"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Verken Azure Stream Analytics met Visual Studio Code (Preview)

De Azure Stream Analytics voor Visual Studio Code-extensie biedt ontwikkelaars een eenvoudige ervaring voor het beheren van de Stream Analytics-taken. Met de extensie Azure Stream Analytics kunt u het volgende doen:

- [Maak](quick-create-vs-code.md), starten en stoppen van taken
- Bestaande taken exporteren naar een lokale project
- Lokaal uitvoeren van query 's
- Stel een CI/CD-pijplijn

## <a name="export-a-job-to-a-local-project"></a>Een taak exporteren naar een lokale project

Als u wilt een taak exporteren naar een lokaal project, zoekt u de taak die u exporteren wilt de **Stream Analytics Explorer** in Visual Studio code. Selecteer een map voor uw project. Het project is geëxporteerd naar de map die u selecteert en kunt u doorgaan met het beheren van de taak van Visual Studio Code. Zie voor meer informatie over het gebruik van Visual Studio Code voor het beheren van Stream Analytics-taken, de Visual Studio Code [snelstartgids](quick-create-vs-code.md).

![Exporteren van de ASA-taak in Visual Studio Code](./media/vs-code-how-to/export-job.png)

## <a name="run-queries-locally"></a>Lokaal uitvoeren van query 's

U kunt de Azure Stream Analytics-extensie voor Visual Studio Code gebruiken voor het testen van uw Stream Analytics-taken lokaal met voorbeeldgegevens.

1. Zodra u uw Stream Analytics-taak hebt gemaakt, gebruikt u **Ctrl + Shift + P** het palet opdracht openen. Typ en selecteer vervolgens **ASA: Invoer toevoegen**.

    ![ASA invoer toevoegen in Visual Studio code](./media/vs-code-how-to/add-input.png)

2. Selecteer **lokale invoer**.

    ![Lokale ASA-invoer toevoegen in Visual Studio code](./media/vs-code-how-to/add-local-input.png)

3. Selecteer **+ nieuwe lokale invoer**.

    ![Een nieuwe ASA lokale invoer in Visual Studio code toevoegen](./media/vs-code-how-to/add-new-local-input.png)

4. Voer dezelfde invoer alias die u hebt gebruikt in uw query.

    ![Een nieuwe ASA lokale invoeralias toevoegen](./media/vs-code-how-to/new-local-input-alias.png)

5. In de **LocalInput_DefaultLocalStream.json** bestand, voer het pad waar het lokale bestand zich bevindt.

    ![Lokaal bestandspad invoeren in Visual Studio](./media/vs-code-how-to/local-file-path.png)

6. Ga terug naar uw query-editor, en selecteer **lokaal uitvoeren**.

    ![Lokaal uitvoeren selecteren in de query-editor](./media/vs-code-how-to/run-locally.png)

## <a name="set-up-a-cicd-pipeline"></a>Stel een CI/CD-pijplijn

U kunt inschakelen met continue integratie en implementatie voor Azure Stream Analytics-taken met behulp van de **hulpprogramma's voor asa-cicd** NPM-pakket. De NPM-pakket biedt de hulpprogramma's voor de automatische implementatie van de Stream Analytics Visual Studio Code-projecten. Het kan worden gebruikt in Windows, macOS en Linux zonder dat Visual Studio Code wordt geïnstalleerd.

Als u eenmaal hebt [het pakket hebt gedownload](https://usqldownload.blob.core.windows.net/ext/asa/asa-cicd-0.0.1-preview-beta.tar), gebruik de volgende opdracht om uit te voeren van de Azure Resource Manager-sjablonen. Als de **outputPath** niet is opgegeven, de sjablonen worden geplaatst in de **implementeren** map onder van het project **bin** map.

```powershell
asa-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```

## <a name="next-steps"></a>Volgende stappen

* [Een taak in de cloud Azure Stream Analytics maken in Visual Studio Code (Preview)](quick-create-vs-code.md)