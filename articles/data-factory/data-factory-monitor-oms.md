---
title: Azure Data Factory met OMS bewaken | Microsoft Docs
description: Informatie over het bewaken van Azure Data Factory met routering gegevens naar de Operations Management Suite (OMS) voor analyse.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: douglasl
ms.openlocfilehash: 4275a4ddcee51d58949b5bd83e4a898cb3dbb389
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304421"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Monitor voor Azure Data Factory met Operations Management Suite (OMS)

U kunt Azure Data Factory-integratie met Azure-bewaking gegevens routeren naar Operations Management Suite (OMS). Deze integratie is handig in de volgende scenario's:

1.  Wilt u het schrijven van complexe query's op een groot aantal metrische gegevens die door Data Factory met OMS is gepubliceerd. U kunt ook aangepaste waarschuwingen maken op deze query's via OMS.

2.  U wilt bewaken voor data Factory. U kunt gegevens uit meerdere data Factory versturen naar een enkele OMS-werkruimte.

## <a name="get-started"></a>Aan de slag

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Diagnostische instellingen en OMS-werkruimte configureren

Schakel diagnostische instellingen voor uw gegevensfactory.

1.  Selecteer **Azure Monitor** -> **diagnostische instellingen** -> Selecteer de gegevensfactory -> Schakel diagnostische gegevens.

    ![monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Geef de diagnostische instellingen zoals de configuratie van de OMS-werkruimte.

    ![monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Azure Data Factory Analytics OMS pack installeren vanuit Azure Marketplace

![monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Klik op **maken** en selecteer de OMS-werkruimte en de OMS-werkruimte-instellingen.

![monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>Azure Data Factory metrische gegevens met behulp van OMS controleren

Installeren van de **Azure Data Factory Analytics** OMS pack maakt een standaardset weergaven waarmee de volgende metrische gegevens:

- 1-ADF wordt uitgevoerd) pijplijn wordt uitgevoerd door Data Factory

- 2-ADF wordt uitgevoerd) activiteit wordt uitgevoerd door Data Factory

- 3-ADF wordt uitgevoerd) Trigger wordt uitgevoerd door Data Factory

- ADF fouten-1) Top 10 pijplijn fouten door Data Factory

- 2-ADF fouten) Top 10 activiteit wordt uitgevoerd door Data Factory

- 3-ADF fouten) Top 10 Trigger fouten door Data Factory

- ADF statistieken-1) activiteiten bij uitvoering op Type

- 2-ADF statistieken) Trigger wordt uitgevoerd door het Type

- 3-ADF statistieken) Max pijplijn uitgevoerd duur

![monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

U kunt de bovenstaande metrische gegevens te visualiseren, kijken naar de query's achter deze metrische gegevens, de query's te bewerken, waarschuwingen maken, enzovoort.

![monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Volgende stappen

Zie [bewaken en beheren van pijplijnen programmatisch](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) voor meer informatie over het controleren en beheren van pijplijnen door het uitvoeren van scripts.
