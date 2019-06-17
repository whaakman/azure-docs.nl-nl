---
title: Verken Azure Stream Analytics-taken met Visual Studio Code (Preview)
description: In dit artikel wordt beschreven hoe u een Azure Stream Analytics-taak exporteren naar een lokale-project, lijst met taken en entiteiten van de taak weergeven.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: 8674d478646c8f9be6b32521c6624752ac6df052
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827798"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Verken Azure Stream Analytics met Visual Studio Code (Preview)

De Azure Stream Analytics voor Visual Studio Code-extensie biedt ontwikkelaars een eenvoudige ervaring voor het beheren van de Stream Analytics-taken. Het kan worden gebruikt op Windows, Mac en Linux. Met de extensie Azure Stream Analytics kunt u het volgende doen:

- [Maak](quick-create-vs-code.md), starten en stoppen van taken
- Bestaande taken exporteren naar een lokale project
- Lijst met taken en weergeven van de taak entiteiten

## <a name="export-a-job-to-a-local-project"></a>Een taak exporteren naar een lokale project

Als u wilt een taak exporteren naar een lokaal project, zoekt u de taak die u exporteren wilt de **Stream Analytics Explorer** in Visual Studio Code. Selecteer een map voor uw project. Het project is geëxporteerd naar de map die u selecteert en kunt u doorgaan met het beheren van de taak van Visual Studio Code. Zie voor meer informatie over het gebruik van Visual Studio Code voor het beheren van Stream Analytics-taken, de Visual Studio Code [snelstartgids](quick-create-vs-code.md).

![Exporteren van de ASA-taak in Visual Studio Code](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>Lijst van de taak en entiteiten van de taak weergeven

U kunt de Project-weergave gebruiken om te communiceren met Azure Stream Analytics-taken vanuit Visual Studio.


1. Klik op de **Azure** pictogram in de Activiteitenbalk van Visual Studio Code en vouw vervolgens **knooppunt Stream Analytics**. Uw taken moeten worden weergegeven onder uw abonnementen.

   ![Open Stream Analytics Explorer](./media/vscode-explore-jobs/open-explorer.png)

2. Vouw het taakknooppunt van uw, kunt u openen en weergeven van de taakquery, configuratie, invoer, uitvoer en functies. 

3. Klik met de rechtermuisknop op uw taakknooppunt en kies de **taakweergave openen in Portal** knooppunt om te openen van de taakweergave in Azure portal.

   ![Open taak weergeven in portal](./media/vscode-explore-jobs/open-job-view.png)

## <a name="next-steps"></a>Volgende stappen

* [Een taak in de cloud Azure Stream Analytics maken in Visual Studio Code (Preview)](quick-create-vs-code.md)
