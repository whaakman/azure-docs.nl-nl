---
title: Een Power BI Desktop-bestand importeren in Azure Analysis Services | Microsoft Docs
description: Beschrijft hoe een Power BI Desktop-bestand (pbix) importeren met behulp van Azure portal.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/26/2018
ms.author: owend
ms.openlocfilehash: e0be0c69b501d7e93c65bcf23d4dd1b6bfa89caf
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Een Power BI Desktop-bestand importeren

U kunt een nieuw model maken in Azure als door het importeren van een Power BI Desktop-bestand (pbix). Metagegevens van het model en gegevens in de cache datasource-verbindingen worden geïmporteerd. Rapporten en visualisaties zijn niet geïmporteerd.

**Beperkingen**   
- Het model pbix moet verbinding maken met [ondersteunde gegevensbronnen voor Analysis Services](analysis-services-datasource.md) alleen. 
- De pbix-model kan niet live hebben of DirectQuery-verbindingen. 
- Als het model pbix met on-premises gegevensbronnen verbindt, een [On-premises gegevensgateway](analysis-services-gateway.md) moet worden geconfigureerd voor de Analysis Services-server.
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
