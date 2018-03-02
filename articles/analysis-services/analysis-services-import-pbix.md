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
ms.openlocfilehash: 43eab587a1e5209069a248f1e2e1f57af158a2b8
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Een Power BI Desktop-bestand importeren

U kunt een nieuw model maken in Azure als door het importeren van een Power BI Desktop-bestand (pbix). Metagegevens van het model en gegevens in de cache datasource-verbindingen worden geïmporteerd. Rapporten en visualisaties zijn niet geïmporteerd. In de server kunnen wijzigingen in het gegevensmodel één keer worden gemaakt door bijwerken en de pbix opnieuw te importeren met behulp van de functie web designer (preview) in de portal of met behulp van SQL Server Management Studio (SSMS). Geïmporteerde modellen kunnen niet worden geopend of geëxporteerd naar Visual Studio.

> [!NOTE]
> Als uw model pbix met on-premises gegevensbronnen verbindt, een [lokale gateway](analysis-services-gateway.md) moet worden geconfigureerd voor uw server.

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
