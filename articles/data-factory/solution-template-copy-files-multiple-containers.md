---
title: Bestanden kopiëren van meerdere containers met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van een oplossingssjabloon om bestanden te kopiëren uit meerdere containers met behulp van Azure Data Factory.
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
ms.openlocfilehash: a52729adf8d6df3f4e44e561b45b854db433628c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57543420"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Bestanden kopiëren van meerdere containers met Azure Data Factory

Dit artikel beschrijft een oplossingssjabloon die u gebruiken kunt om bestanden te kopiëren uit meerdere containers tussen bestand. Bijvoorbeeld, kunt u het migreren van uw data lake van AWS S3 naar Azure Data Lake Store. Of u kunt de sjabloon gebruiken voor het repliceren van alles vanuit een Azure Blob storage-account naar een andere.

> [!NOTE]
> Als u kopiëren van bestanden uit een enkele container wilt, is het efficiënter gebruik van de [hulpprogramma voor kopiëren-gegevens](copy-data-tool.md) voor het maken van een pijplijn met een één kopieeractiviteit. De sjabloon in dit artikel is meer dan u nodig hebt voor het betreffende eenvoudige scenario.

## <a name="about-this-solution-template"></a>Over deze oplossingssjabloon

Deze sjabloon worden de containers in uw bron opslag store opgesomd. Vervolgens worden deze containers gekopieerd naar de doel-store.

De sjabloon bevat drie activiteiten:
- **GetMetadata** scant uw gegevensbronopslag voor opslag en haalt de lijst met de container.
- **ForEach** haalt de lijst met de container van de **GetMetadata** activiteit en de lijst te herhalen en wordt elke container doorgegeven aan de Copy-activiteit.
- **Kopie** elke container uit het archief van de opslag bron naar het doelarchief gekopieerd.

De sjabloon definieert twee parameters:
- *SourceFilePath* is het pad van de brongegevensopslag, waar u kunt een lijst van de containers ophalen. In de meeste gevallen is het pad naar de hoofdmap waarin meerdere mappen van de container. De standaardwaarde van deze parameter is `/`.
- *DestinationFilePath* is het pad waar de bestanden worden gekopieerd naar in uw doelarchief. De standaardwaarde van deze parameter is `/`.

## <a name="how-to-use-this-solution-template"></a>Het gebruik van deze oplossingssjabloon

1. Ga naar de **meerdere bestanden containers tussen bestand kopiëren** sjabloon. Maak een **nieuw** verbinding met uw bron-opslag-archief. De bron-opslag-store is waarnaar u wilt kopiëren van bestanden uit meerdere containers uit.

    ![Maak een nieuwe verbinding met de bron](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Maak een **nieuw** verbinding met uw doelarchief voor opslag.

    ![Maak een nieuwe verbinding met het doel](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Selecteer **Gebruik deze sjabloon**.

    ![Deze sjabloon gebruiken](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. U ziet de pijplijn, zoals in het volgende voorbeeld:

    ![De pijplijn weergeven](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Selecteer **Debug**, voer de **Parameters**, en selecteer vervolgens **voltooien**.

    ![De pijplijn uitvoeren](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Bekijk het resultaat.

    ![Bekijk het resultaat](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Volgende stappen

- [Bulksgewijs kopiëren uit een database met behulp van een besturingselement-tabel met Azure Data Factory](solution-template-bulk-copy-with-control-table.md)

- [Bestanden kopiëren van meerdere containers met Azure Data Factory](solution-template-copy-files-multiple-containers.md)