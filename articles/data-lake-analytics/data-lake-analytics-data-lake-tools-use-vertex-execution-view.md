---
title: De Vertex Execution View in Data Lake Tools voor Visual Studio gebruiken
description: In dit artikel wordt beschreven hoe u de Vertex Execution View op examen Data Lake Analytics-taken gebruiken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: mumian
ms.author: jgao
ms.reviewer: jasonwhowell
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.topic: conceptual
ms.date: 10/13/2016
ms.openlocfilehash: 9f834d697c0d3fe537bbdb190b0ba0d0f294ac87
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041010"
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>De Vertex Execution View in Data Lake Tools voor Visual Studio gebruiken
Informatie over het gebruik van de Vertex Execution View op examen Data Lake Analytics-taken.


## <a name="open-the-vertex-execution-view"></a>Open de Vertex Execution View
Open een U-SQL-taak in Data Lake Tools voor Visual Studio. Klik op **Vertex Execution View** in de linkerbenedenhoek. U mogelijk gevraagd eerst profielen laden en het kan even duren, afhankelijk van uw netwerkverbinding.

![Data Lake Analytics-hulpprogramma's Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Vertex Execution View begrijpen
De Vertex Execution View bestaat uit drie delen:

![Data Lake Analytics-hulpprogramma's Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

De **hoekpunt selector** op met de links kunt u hoekpunten selecteren door functies (zoals de top 10 gegevens lezen, of kies fase). Een van de meest gebruikte filters om te zien is het **hoekpunten op kritieke pad**. De **kritieke pad** is de langste keten van hoekpunten van een U-SQL-taak. Informatie over het kritieke pad is handig voor het optimaliseren van uw taken door te controleren welke hoekpunt de meeste tijd vergt.
  
![Data Lake Analytics-hulpprogramma's Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Het deelvenster bovenaan in het midden bevat de **met de status van alle hoekpunten**.
  
![Data Lake Analytics-hulpprogramma's Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Het onderste middelste deelvenster toont informatie over elk hoekpunt dat:
* Procesnaam: De naam van het exemplaar van het hoekpunt. Deze bestaat uit verschillende onderdelen in StageName | VertexName | VertexRunInstance. Bijvoorbeeld, het hoekpunt van de [62] .v1 SV7_Split staat voor de tweede instantie uitgevoerd (.v1, index die begint bij 0) van hoekpunt getal 62 in fase SV7_Split.
* Totale gegevens lezen/schriftelijke: De gegevens zijn gelezen of weggeschreven door deze hoekpunt.
* Status/afsluitstatus: De definitieve status wanneer het hoekpunt wordt beëindigd.
* : Afsluiten: / mislukt de fout wanneer het hoekpunt is mislukt.
* Reden voor het maken van: Waarom het hoekpunt is gemaakt.
* Resource latentie/proces latentie/PN wachtrij latentie: de gebruikte tijd voor het hoekpunt na afloop van resources, om gegevens te verwerken en om te blijven in de wachtrij.
* Proces/Creator GUID: GUID voor de persoon die of het huidige actieve hoekpunt.
* Versie: de N-de instantie van de actieve hoekpunt (het systeem kunt plannen nieuwe instanties van een hoekpunt voor verschillende redenen, zoals failover, redundantie, enz.)
* De versie die tijd zijn gemaakt.
* Verwerken maken Start tijd en verwerken in de wachtrij geplaatst tijd/proces Start tijd/proces voltooid tijd: wanneer het proces hoekpunt begint met het maken; Wanneer het hoekpunt-proces wordt gestart in de wachtrij; Wanneer het bepaalde hoekpunt-proces wordt gestart; Wanneer het bepaalde hoekpunt is voltooid.

## <a name="next-steps"></a>Volgende stappen
* Zie [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Diagnostische logboeken openen voor Azure Data Lake Analytics) voor logboekregistratie van diagnostische informatie.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
* Taakdetails Zie [gebruik Job Browser en Job View voor Azure Data lake Analytics-taken](data-lake-analytics-data-lake-tools-view-jobs.md)
