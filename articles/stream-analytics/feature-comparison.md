---
title: Vergelijking van Azure Stream Analytics
description: In dit artikel vergelijkt de functies die worden ondersteund voor de cloud van Azure Stream Analytics en IoT Edge-taken in de Azure portal, Visual Studio en Visual Studio Code.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 22d7ef90ee0cf4d09467516b7bb0664327b7dabe
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509779"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Vergelijking van Azure Stream Analytics

Met Azure Stream Analytics kunt u streamingoplossingen kunt maken in de cloud en op de IoT Edge met behulp van [Azure-portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md), en [Visual Studio Code](quick-create-vs-code.md). De tabellen in dit artikel ziet welke functies worden ondersteund door elk platform voor beide taaktypen.

## <a name="cloud-job-features"></a>Functies van cloud-taak


|Functie  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Platformoverschrijdend     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Script ontwerpen     |Ja         |Ja         |Ja         |
|Script Intellisense     |Syntaxismarkering         |Syntaxismarkering</br>Code uitvoeren</br>Fout-markering         |Syntaxismarkering</br>Code uitvoeren</br>Fout-markering         |
|Invoer, uitvoer en taak configuraties definiëren     |Ja         |Ja         |Ja         |
|Partitioneren van de BLOB-uitvoer     |Ja         |Ja         |Ja         |
|Power BI als uitvoer     |Ja         |Ja         |Nee         |
|Referentiegegevens voor SQL database     |Ja         |Ja         |Ja         |
|Eigenschappen van aangepaste berichten     |Ja         |Nee         |Nee         |
|Invoer en uitvoer delen tussen meerdere query 's     |Nee         |Ja         |Ja         |
|JavaScript UDF en UDA     |Ja         |Ja         |Alleen Windows         |
|Machine Learning bijschriften     |Ja, maar de query kan niet worden getest        |Ja, maar kan niet lokaal worden getest         |Nee         |
|Compatibiliteitsniveau     |1.0</br>1.1</br>1.2         |1.0</br>1.1</br>1.2          |1.0</br>1.1</br>1.2          |
|Ingebouwde detectie van afwijkingen op basis van een ML-functies     |Ja         |Ja         |Ja         |
|Ingebouwde georuimtelijke functies     |Ja         |Ja         |Ja         |
|Query's testen met een voorbeeldbestand     |Ja         |Ja         |Ja         |
|Live gegevens lokaal testen     |Nee         |Ja         |Nee         |
|Lijst met taken en weergeven van de taak entiteiten     |Ja         |Ja         |Ja         |
|Een taak exporteren naar een lokale project     |Nee         |Ja         |Ja         |
|Indienen, starten en stoppen van taken     |Ja         |Ja         |Ja         |
|Broncodebeheer     |Nee         |Ja         |Ja         |
|CI/CD-ondersteuning     |Gedeeltelijke         |Ja         |Ja         |
|Metrische gegevens weergeven-taak en diagram     |Ja         |Ja         |Openen in portal         |
|Taak-runtime-fouten weergeven     |Ja         |Ja         |Nee         |
|Diagnostische logboeken     |Ja         |Nee         |Nee         |


## <a name="iot-edge-job-features"></a>Functies van IoT Edge-taak

|Functie  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Taak schrijven     |Ja         |Ja         |Nee         |
|Broncodebeheer     |Nee         |Ja         |Nee         |
|Een taak exporteren naar een lokale project     |Nee         |Ja         |Nee         |
|Query's testen met een voorbeeldbestand     |Ja         |Ja         |Nee         |
|Invoer en uitvoer delen tussen meerdere query 's     |Nee         |Ja         |Nee         |
|C# UDF     |Nee         |Ja         |Nee         |
|Indienen, starten en stoppen van taken     |Ja         |Ja         |Nee         |
|Lijst met taken en weergeven van de taak entiteiten     |Ja         |Ja         |Nee         |
|Metrische gegevens weergeven-taak en diagram     |Ja         |Gedeeltelijke         |Nee         |
|Taak-runtime-fouten weergeven     |Ja         |Gedeeltelijke         |Nee         |
|CI/CD-ondersteuning     |Nee         |Nee         |Nee         |


## <a name="next-steps"></a>Volgende stappen

* [Azure Stream Analytics op IoT Edge](stream-analytics-edge.md)
* [Zelfstudie: Schrijf een C# door de gebruiker gedefinieerde functie voor Azure Stream Analytics IoT Edge-taak (Preview)](stream-analytics-edge-csharp-udf.md)
* [Stream Analytics IoT Edge-taken met behulp van Visual Studio-hulpprogramma's ontwikkelen](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Visual Studio gebruiken om Azure Stream Analytics-taken weer te geven](stream-analytics-vs-tools.md)
* [Verken Azure Stream Analytics met Visual Studio Code (Preview)](vscode-explore-jobs.md)


