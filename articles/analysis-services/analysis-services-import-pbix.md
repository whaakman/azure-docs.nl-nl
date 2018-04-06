---
title: Een Power BI Desktop-bestand importeren in Azure Analysis Services | Microsoft Docs
description: Beschrijft hoe een Power BI Desktop-bestand (pbix) importeren met behulp van Azure portal.
services: analysis-services
documentationcenter: ''
author: minewiskan
manager: kfile
editor: ''
tags: ''
ms.assetid: ''
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 04/03/2018
ms.author: owend
ms.openlocfilehash: 2ba9bc0e4b9a55312875fe120ee179800aeefb23
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Een Power BI Desktop-bestand importeren

U kunt een nieuw model maken in Azure als door het importeren van een Power BI Desktop-bestand (pbix). Metagegevens van het model en gegevens in de cache datasource-verbindingen worden geïmporteerd. Rapporten en visualisaties zijn niet geïmporteerd.

**Beperkingen**   
- Het model pbix kunt verbinden met Azure SQL Database- en Azure SQL Data Warehouse-gegevensbronnen alleen. 
- De pbix-model kan niet live hebben of DirectQuery-verbindingen. 
- Importeren mislukt mogelijk als de pbix-gegevensmodel niet ondersteund in Analysis Services-metagegevens bevat.

## <a name="to-import-from-pbix"></a>Om te importeren uit pbix

1. In de server **overzicht** > **webdesigner**, klikt u op **Open**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. In **webdesigner** > **modellen**, klikt u op **+ toevoegen**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. In **nieuw model**, typ de modelnaam van een en selecteer vervolgens de Power BI Desktop-bestand.

    ![Dialoogvenster voor nieuwe model in Azure-portal](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. In **importeren**, zoekt en selecteert u het bestand.

     ![Verbinding maken met het dialoogvenster in Azure-portal](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Zie ook

[Een model maken in Azure portal](analysis-services-create-model-portal.md)   
[Verbinding maken met Azure analyseservices](analysis-services-connect.md)  
