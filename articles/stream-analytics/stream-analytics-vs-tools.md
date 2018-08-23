---
title: Azure Stream Analytics-taken weergeven in Visual Studio
description: In dit artikel wordt beschreven hoe u Stream Analytics-taken weergeven in Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 0df7c7680137be9a4b2c7c3cedb46330f4d3e5bd
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42054533"
---
# <a name="use-visual-studio-to-view-azure-stream-analytics-jobs"></a>Visual Studio gebruiken om Azure Stream Analytics-taken weer te geven

Azure Stream Analytics-hulpprogramma's voor Visual Studio maakt het eenvoudig voor ontwikkelaars voor het beheren van de Stream Analytics-taken rechtstreeks vanuit de IDE. Met Azure Stream Analytics-hulpprogramma's, kunt u het volgende doen:
- [Nieuwe taken maken](stream-analytics-quick-create-vs.md)
- Starten, stoppen, en [taken controleren](stream-analytics-monitor-jobs-use-vs.md)
- De resultaten van de taak
- Bestaande taken exporteren naar een project
- Invoer- en -verbindingen testen
- [Lokaal uitvoeren van query 's](stream-analytics-vs-tools-local-run.md)

Meer informatie over het [Azure Stream Analytics-hulpprogramma's voor Visual Studio installeren](stream-analytics-tools-for-visual-studio-install.md).

## <a name="explore-the-job-view"></a>Verken de taakweergave

U kunt de Project-weergave gebruiken om te communiceren met Azure Stream Analytics-taken vanuit Visual Studio.

### <a name="open-the-job-view"></a>De taakweergave openen

1. In **Server Explorer**, selecteer **Stream Analytics-taken** en selecteer vervolgens **vernieuwen**. Uw taak moet worden weergegeven onder **Stream Analytics-taken**.

    ![Lijst met Stream Analytics server explorer](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-list-jobs-01.png)



2. Vouw het taakknooppunt van uw uit en dubbelklik op de **taakweergave** knooppunt om de taakweergave van een te openen.
    
   ![Uitgebreide taakknooppunt](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-job-view-01.png)

### <a name="start-and-stop-jobs"></a>Starten en stoppen van taken

Azure Stream Analytics-taken kunnen volledig worden beheerd vanuit de taakweergave in Visual Studio. Gebruik de besturingselementen om te starten, stoppen of verwijderen van een taak.
    
   ![Stream Analytics-taak besturingselementen](./media/stream-analytics-vs-tools/azure-stream-analytics-job-view-controls.png)


## <a name="check-job-results"></a>De resultaten van de taak

Stream Analytics-hulpprogramma's voor Visual Studio ondersteunt momenteel Uitvoervoorbeeld voor Azure Data Lake Storage Storage en blob-opslag. Als u wilt weergeven resultaat, eenvoudigweg dubbelklikt u op het knooppunt van de uitvoer van het taakdiagram in **taakweergave** en voer de juiste referenties.

   ![Stream Analytics-taakuitvoer blob](./media/stream-analytics-vs-tools/stream-analytics-blob-preview.png)


## <a name="export-jobs-to-a-project"></a>Taken exporteren naar een project

Er zijn twee manieren kunt u een bestaande taak exporteren naar een project.

1. In **Server Explorer**, onder het knooppunt Stream Analytics-taken met de rechtermuisknop op het taakknooppunt. Selecteer **exporteren naar nieuwe Stream Analytics-Project**.
    
   ![Taak exporteren naar project](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-01.png)
    
    Het gegenereerde project wordt weergegeven in **Solution Explorer**.
    
   ![Solution explorer](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-02.png)

2. Selecteer in de taakweergave **Project genereren**.
    
   ![Project genereren van de taak weergeven](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="test-connections"></a>Verbindingen testen

Invoer- en -verbindingen kunnen worden getest in de **taakweergave** door een optie van de **testverbinding** vervolgkeuzelijst.

   ![Vervolgkeuzelijst van de verbinding testen](./media/stream-analytics-vs-tools/stream-analytics-test-connection-dropdown.png)

De **testverbinding** resultaten worden weergegeven in de **uitvoer** venster.

   ![De resultaten van verbinding](./media/stream-analytics-vs-tools/stream-analytics-test-connection-results.png)

## <a name="next-steps"></a>Volgende stappen

* [Controleren en beheren van Azure Stream Analytics-taken met behulp van Visual Studio](stream-analytics-monitor-jobs-use-vs.md)
* [Snelstart: Een Stream Analytics-taak met behulp van Visual Studio maken](stream-analytics-quick-create-vs.md)
* [Zelfstudie: Een Azure Stream Analytics-taak implementeren met CI/CD met VSTS](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Continue integratie en ontwikkeling met Stream Analytics-hulpprogramma’s](stream-analytics-tools-for-visual-studio-cicd.md)