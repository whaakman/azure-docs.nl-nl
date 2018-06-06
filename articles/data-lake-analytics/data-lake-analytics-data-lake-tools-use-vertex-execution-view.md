---
title: Gebruik de Vertex Execution View in Data Lake Tools voor Visual Studio
description: Dit artikel wordt beschreven hoe u de Vertex Execution View naar examen Data Lake Analytics-taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: mumian
ms.author: jgao
manager: kfile
editor: jasonwhowell
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.topic: conceptual
ms.date: 10/13/2016
ms.openlocfilehash: af15bb9fd1131f598dc87f13c4af481b63d023e3
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34735438"
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Gebruik de Vertex Execution View in Data Lake Tools voor Visual Studio
Informatie over het gebruik van de Vertex Execution View naar examen Data Lake Analytics-taken.


## <a name="open-the-vertex-execution-view"></a>Open de Vertex Execution View
Open een U-SQL-taak in de Data Lake Tools voor Visual Studio. Klik op **Vertex Execution View** in de linkerbenedenhoek. U wordt mogelijk gevraagd eerst profielen laden en het kan even duren, afhankelijk van uw netwerkverbinding.

![Data Lake Analytics Tools Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Vertex Execution View begrijpen
De Vertex Execution View bestaat uit drie delen:

![Data Lake Analytics Tools Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

De **hoekpunt selector** op de links kunt u hoekpunten selecteren door functies (zoals top 10 gegevens lezen, of kies fase). Een van de filters voor het meest gebruikt om te zien is de **hoekpunten op kritieke pad**. De **kritieke pad** is de langste keten van hoekpunten van een U-SQL-taak. Inzicht in het kritieke pad is nuttig voor het optimaliseren van uw taken door te controleren welke hoekpunt het langste tijd in beslag neemt.
  
![Data Lake Analytics Tools Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

In het deelvenster ziet u boven de **verwerkingsstatus van de hoekpunten**.
  
![Data Lake Analytics Tools Vertex Execution View](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Het deelvenster onderaan center bevat informatie over elk hoekpunt:
* Procesnaam: De naam van het hoekpunt-exemplaar. Bestaat uit verschillende onderdelen in StageName | VertexName | VertexRunInstance. Het SV7_Split [62] .v1 hoekpunt staat bijvoorbeeld voor de tweede actieve instantie (.v1, index die begint bij 0) van hoekpunt getal 62 in fase SV7_Split.
* Totale gegevens lezen/schriftelijke: De gegevens zijn gelezen of weggeschreven door dit hoekpunt.
* Status/afsluitstatus: De laatste status wanneer het hoekpunt wordt beëindigd.
* Exit Code/mislukte Type: De fout bij het hoekpunt is mislukt.
* Het maken van reden: Waarom het hoekpunt is gemaakt.
* Resource latentie/proces latentie/PN wachtrij latentie: de gebruikte tijd voor het hoekpunt moet worden gewacht voor bronnen om gegevens te verwerken en om te blijven in de wachtrij.
* GUID van proces/Maker: GUID voor de persoon die of het huidige actieve hoekpunt.
* Versie: de N de instantie van het actieve hoekpunt (het systeem kunt plannen nieuwe exemplaren van een hoekpunt voor een groot aantal redenen, bijvoorbeeld failover compute redundantie, enz.)
* Versie wanneer gemaakt.
* Verwerken maken Start tijd/proces in de wachtrij geplaatst tijd/proces Start tijd/proces voltooid tijd: wanneer het proces hoekpunt maken start; Wanneer het hoekpunt-proces wordt gestart in de wachtrij; Wanneer het bepaalde hoekpunt-proces wordt gestart; Wanneer het bepaalde hoekpunt is voltooid.

## <a name="next-steps"></a>Volgende stappen
* Zie [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Diagnostische logboeken openen voor Azure Data Lake Analytics) voor logboekregistratie van diagnostische informatie.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
* Taakdetails Zie [gebruik taak Browser en weergave van de taak voor Azure Data lake Analytics-taken](data-lake-analytics-data-lake-tools-view-jobs.md)
