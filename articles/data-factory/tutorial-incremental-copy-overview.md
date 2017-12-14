---
title: "Stapsgewijs gegevens kopiëren met behulp van Azure Data Factory | Microsoft Docs"
description: "Deze zelfstudies tonen hoe u incrementeel gegevens kunt kopiëren van een brongegevensarchief naar een doelgegevensarchief. De eerste kopieert gegevens uit één tabel. "
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/05/2017
ms.author: shlo
ms.openlocfilehash: 2ae6cb42685dfb227bd75f83e73dfdf646ab909f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Incrementeel laden van gegevens van een brongegevensarchief naar een doelgegevensarchief

In een oplossing voor gegevensintegratie is incrementeel (of delta) laden van gegevens na een eerste volledige laadhandeling een veelgebruikt scenario. De zelfstudies in deze sectie tonen u de verschillende manieren van het incrementeel laden van gegevens met behulp van Azure Data Factory versie 2.

## <a name="delta-data-loading-using-a-watermark"></a>Delta-gegevens laden met behulp van een watermerk
In dit geval definieert u een watermerk in de brondatabase. Een watermerk is een kolom die het laatst bijgewerkte tijdstempel of een ophogende sleutel heeft. Bij delta-laden wordt de gewijzigde data tussen een oud watermerk en een nieuw watermerk geladen. De werkstroom voor deze benadering wordt verduidelijkt in het volgende diagram: 

![Werkstroom voor het gebruik van een watermerk](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Zie de volgende zelfstudies voor stapsgewijze instructies: 

- [Stapsgewijs gegevens uit een enkele tabel in de Azure SQL Database laden in Azure Blob Storage](tutorial-incremental-copy-powershell.md)
- [Incrementeel gegevens uit meerdere tabellen in een lokale SQL Server naar een Azure SQL Database kopiëren](tutorial-incremental-copy-multiple-tables-powershell.md)


## <a name="delta-data-loading-using-the-change-tracking-technology"></a>Delta-gegevens laden met behulp van de technologie voor wijzigingen bijhouden
Technologie voor het bijhouden van wijzigingen is een lichtgewicht oplossing in SQL Server en Azure SQL Database waarmee een efficiënt mechanisme wordt geboden voor bijhouden van wijzigingen in applicaties. Hiermee kan een toepassing eenvoudig gegevens herkennen die zijn toegevoegd, bijgewerkt of verwijderd. 

De werkstroom voor deze benadering wordt verduidelijkt in het volgende diagram:

![Werkstroom voor het gebruik van Wijzigingen bijhouden](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Zie de volgende zelfstudies voor stapsgewijze instructies: <br/>
[Incrementeel gegevens kopiëren van Azure SQL Database naar Azure Blob Storage met behulp van technologie voor wijzigingen bijhouden](tutorial-incremental-copy-change-tracking-feature-powershell.md)


## <a name="next-steps"></a>Volgende stappen
Ga naar de volgende zelfstudie: 

> [!div class="nextstepaction"]
>[Stapsgewijs gegevens uit een enkele tabel in de Azure SQL Database laden in Azure Blob Storage](tutorial-incremental-copy-powershell.md)
