---
title: Bestanden kopiëren van meerdere containers met Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van een oplossingssjabloon om bestanden te kopiëren uit meerdere containers met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/1/2018
ms.openlocfilehash: aa5f32594c295ab6a8e60af8359370f64f75a72d
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967202"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Bestanden kopiëren van meerdere containers met Azure Data Factory

De sjabloon van de oplossing wordt beschreven in dit artikel helpt u bij het kopiëren van bestanden uit meerdere bestanden, containers of buckets tussen bestand. Bijvoorbeeld, misschien wilt u migreren van uw data lake van AWS S3 naar Azure Data Lake Store. Of misschien wilt u alles van de ene Azure Blob Storage-account te repliceren naar een andere Azure Blob Storage-account. Deze sjabloon is ontworpen voor deze gevallen gebruikt.

Als u kopiëren van bestanden vanaf een enkele container of bucket wilt, is het efficiënter gebruik van de **hulpprogramma voor kopiëren-gegevens** een pijplijn maken met een één kopieeractiviteit. Deze sjabloon is meer dan u nodig hebt voor dit eenvoudige use-case.

## <a name="about-this-solution-template"></a>Over deze oplossingssjabloon

Deze sjabloon worden de containers in uw bron opslag store opgesomd en kopieën van de containers van de bronopslagruimte slaat u vervolgens naar de doel-store. 

De sjabloon bevat drie activiteiten:
-   Een **GetMetadata** activiteit voor het scannen van uw opslag gegevensbronopslag en ophalen van de containerlijst.
-   Een **ForEach** activiteit container lijst weer van de **GetMetadata** activiteit en de lijst met herhalen en elke container doorgeven aan de Copy-activiteit.
-   Een **kopie** activiteit elke container uit het archief van de opslag bron naar het doelarchief kopiëren.

De sjabloon definieert twee parameters:
-   De parameter *SourceFilePath* is het pad van de brongegevensopslag, waar u een lijst van de containers of buckets kan krijgen. In de meeste gevallen is het pad naar de hoofdmap waarin meerdere mappen van de container. De standaardwaarde van deze parameter is `/`.
-   De parameter *DestinationFilePath* is het pad waar de bestanden in uw doelarchief worden gekopieerd. De standaardwaarde van deze parameter is `/`.

## <a name="how-to-use-this-solution-template"></a>Het gebruik van deze oplossingssjabloon

1. Ga naar de sjabloon **meerdere bestanden containers tussen bestand kopiëren**, en maak een **nieuwe verbinding** naar uw bron-opslag-store. De bron-opslag-store is de plek waar u wilt kopiëren van bestanden uit meerdere containers of buckets.

    ![Maak een nieuwe verbinding met de bron](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Maak een **nieuwe verbinding** aan uw doelarchief voor opslag.

    ![Maak een nieuwe verbinding met het doel](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Klik op **Gebruik deze sjabloon**.

    ![Deze sjabloon gebruiken](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. U ziet de pijplijn die beschikbaar zijn in het deelvenster, zoals wordt weergegeven in het volgende voorbeeld:

    ![De pijplijn weergeven](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Klik op **Debug**, voer **Parameters** en klikt u op **voltooien**.

    ![De pijplijn uitvoeren](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Bekijk het resultaat.

    ![Bekijk het resultaat](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Data Factory](introduction.md)
