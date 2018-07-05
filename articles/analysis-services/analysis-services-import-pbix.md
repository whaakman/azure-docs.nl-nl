---
title: Een Power BI Desktop-bestand importeren in Azure Analysis Services | Microsoft Docs
description: Beschrijft hoe u een Power BI Desktop-bestand (pbix) importeren met behulp van Azure portal.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3dd90fc862e64812c0ba17bef74818d18788f4b5
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440977"
---
# <a name="import-a-power-bi-desktop-file"></a>Een Power BI Desktop-bestand importeren

U kunt een gegevensmodel in Power BI Desktop-bestand (pbix) importeren in Azure Analysis Services. Model metagegevens, gegevens in de cache en gegevensbronverbindingen worden geïmporteerd. Rapporten en visualisaties zijn niet geïmporteerd. Geïmporteerde gegevens modellen vanuit Power BI Desktop op het compatibiliteitsniveau 1400 zijn.

**Beperkingen**   
- Het pbix-model verbinding kan maken met **Azure SQL Database** en **Azure SQL Data Warehouse** worden alleen de gegevensbronnen. 
- Het pbix-model kan niet live hebben of DirectQuery-verbindingen. 
- Importeren mislukt mogelijk als uw pbix-gegevensmodel metagegevens niet ondersteund in Analysis Services bevat.

## <a name="to-import-from-pbix"></a>Om te importeren uit pbix

1. In van uw server **overzicht** > **webdesigner**, klikt u op **Open**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. In **webdesigner** > **modellen**, klikt u op **+ toevoegen**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. In **nieuw model**, typ de modelnaam van een en selecteer vervolgens Power BI Desktop-bestand.

    ![Dialoogvenster voor nieuw model in Azure portal](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. In **importeren**, zoekt en selecteert u het bestand.

     ![Verbinding maken met het dialoogvenster in Azure portal](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Zie ook

[Een model maken in Azure portal](analysis-services-create-model-portal.md)   
[Verbinding maken met Azure analyseservices](analysis-services-connect.md)  
