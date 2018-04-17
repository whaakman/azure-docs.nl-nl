---
title: Een Power BI Desktop-bestand importeren in Azure Analysis Services | Microsoft Docs
description: Beschrijft hoe een Power BI Desktop-bestand (pbix) importeren met behulp van Azure portal.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 35bf2ba85017de43788f802b6244d61ed2bb62df
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
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
