---
title: Azure Stream Analytics-query's met Visual Studio lokaal testen
description: Dit artikel wordt beschreven hoe u voor het testen van query's lokaal met Azure Stream Analytics-hulpprogramma's voor Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: cc4372a8a8a8f39bf4f77f637b459c5281b8852d
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299522"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio"></a>Stream Analytics-query's met Visual Studio lokaal testen

U kunt Azure Stream Analytics-hulpprogramma's voor Visual Studio gebruiken voor het testen van uw Stream Analytics-taken lokaal met voorbeeldgegevens.

Gebruik deze [snelstartgids](stream-analytics-quick-create-vs.md) voor informatie over het maken van een Stream Analytics-taak met behulp van Visual Studio.

## <a name="test-your-query"></a>De query testen

Dubbelklik in het project Azure Stream Analytics op **Script.asaql** het script in de editor te openen. U kunt de query om te zien of er syntaxisfouten zijn compileren. De query-editor ondersteunt IntelliSense, syntaxis kleur en de markering van een fout.

![Query-editor](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="add-local-input"></a>Lokale invoer toevoegen

Voor het valideren van de query op de lokale statische gegevens, met de rechtermuisknop op de invoer en selecteer **lokale invoer toevoegen**.
   
![Lokale invoer toevoegen](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-01.png)
   
Selecteer in het pop-upvenster voorbeeldgegevens uit het lokale pad en **opslaan**.
   
![Lokale invoer toevoegen](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-02.png)
   
Een bestand met de naam **local_EntryStream.json** wordt automatisch toegevoegd aan de map invoer.
   
![Lijst met bestanden van lokale invoermap](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-03.png)
   
Selecteer **lokaal uitvoeren** in de query-editor. Of u kunt op F5 drukt.
   
![Lokaal uitvoeren](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-01.png)
   
De uitvoer kan worden weergegeven in de vorm van een tabel rechtstreeks vanuit Visual Studio.

![Uitvoer in tabelvorm](./media/stream-analytics-vs-tools-local-run/stream-analytics-for-vs-local-result.png)

Hier vindt u het uitvoerpad van de console-uitvoer. Op een willekeurige toets om de resultaat-map te openen.
   
![Lokaal uitvoeren](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-02.png)
   
Controleer de resultaten in de lokale map.
   
![Resultaat van de lokale map](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-03.png)
   

### <a name="sample-input"></a>Van Voorbeeldinvoer
U kunt ook voorbeeldinvoergegevens verzamelen uit uw invoer bronnen naar een lokaal bestand. Met de rechtermuisknop op het configuratiebestand van de invoer en selecteer **voorbeeldgegevens**. 

![Voorbeeldgegevens](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-01.png)

U kunt alleen voorbeeldgegevens streamen vanuit Event Hubs of IoT-Hubs. Andere invoerbronnen worden niet ondersteund. Vul in het lokale pad naar de voorbeeld-gegevens opslaan en selecteer in het pop-updialoogvenster **voorbeeld**.

![In de configuratie van gegevens](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-02.png)
 
U ziet de voortgang in de **uitvoer** venster. 

![Voorbeelduitvoer van gegevens](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-03.png)

## <a name="next-steps"></a>Volgende stappen

* [Visual Studio gebruiken om Azure Stream Analytics-taken weer te geven](stream-analytics-vs-tools.md)
* [Snelstart: Een Stream Analytics-taak met behulp van Visual Studio maken](stream-analytics-quick-create-vs.md)
* [Zelfstudie: Een Azure Stream Analytics-taak met CI/CD met behulp van Azure DevOps implementeren](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Continue integratie en ontwikkeling met Stream Analytics-hulpprogramma’s](stream-analytics-tools-for-visual-studio-cicd.md)