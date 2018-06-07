---
title: Een Power BI Desktop-bestand importeren in Azure Analysis Services | Microsoft Docs
description: Beschrijft hoe een Power BI Desktop-bestand (pbix) importeren met behulp van Azure portal.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: aea6f3efcf3740527c43b75a30caadf6b2a8b623
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601068"
---
# <a name="import-a-power-bi-desktop-file"></a>Een Power BI Desktop-bestand importeren

U kunt een gegevensmodel in een Power BI Desktop-bestand (pbix) importeren in Azure Analysis Services. Metagegevens van het model en gegevens in de cache datasource-verbindingen worden geïmporteerd. Rapporten en visualisaties zijn niet geïmporteerd. De gegevens modellen van Power BI Desktop op het compatibiliteitsniveau 1400 zijn geïmporteerd.

**Beperkingen**   
- Het model pbix verbinding kan maken met **Azure SQL Database** en **Azure SQL Data Warehouse** alleen gegevensbronnen. 
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
