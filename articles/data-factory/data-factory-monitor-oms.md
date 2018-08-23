---
title: Azure Data Factory met OMS controleren | Microsoft Docs
description: Informatie over het bewaken van Azure Data Factory door routeren gegevens naar de Operations Management Suite (OMS) voor analyse.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: douglasl
ms.openlocfilehash: fb5da75d0e42dd0700cad008d348ff846d602409
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42054927"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Monitor voor Azure Data Factory met Operations Management Suite (OMS)

U kunt Azure Data Factory-integratie met Azure Monitor gebruiken het routeren van gegevens naar de Operations Management Suite (OMS). Deze integratie is handig in de volgende scenario's:

1.  Wilt u het schrijven van complexe query's op een uitgebreide set metrische gegevens die door Data Factory met OMS is gepubliceerd. U kunt ook aangepaste waarschuwingen maken op deze query's via OMS.

2.  U wilt controleren in data factory's. U kunt gegevens uit meerdere data factory's versturen aan één OMS-werkruimte.

Bekijk de volgende video voor een inleiding van 7 minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

## <a name="get-started"></a>Aan de slag

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Diagnostische instellingen en OMS-werkruimte configureren

Diagnostische instellingen inschakelen voor uw data factory.

1.  Selecteer **Azure Monitor** -> **diagnostische instellingen** -> Selecteer de data factory -> Schakel diagnostische gegevens.

    ![monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Diagnostische instellingen zoals de configuratie van de OMS-werkruimte kan leveren.

    ![monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Azure Data Factory Analytics OMS pack installeren vanuit Azure Marketplace

![monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Klik op **maken** en selecteert u de OMS-werkruimte en de instellingen voor OMS-werkruimte.

![monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>Azure Data Factory metrische bewaken met OMS

Installeren van de **Azure Data Factory Analytics** OMS pack maakt u een standaardset van weergaven waarmee de volgende metrische gegevens:

- Pijplijnuitvoeringen ADF wordt-1) door Data Factory

- Uitvoeringen van activiteit ADF uitvoeringen-2) door Data Factory

- Triggeruitvoeringen ADF uitvoeringen-3) door Data Factory

- ADF-fouten-1) Top 10 pijplijn fouten door Data Factory

- ADF-fouten-2) Top 10 uitvoeringen van activiteit door Data Factory

- ADF-fouten-3) Top 10 Trigger fouten door Data Factory

- Uitvoeringen van activiteit ADF statistieken-1) per Type

- Triggeruitvoeringen ADF statistieken-2) per Type

- ADF-statistieken-3) Pijplijnuitvoeringen Max duur

![monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

U kunt de bovenstaande metrische gegevens visualiseren, kijken naar de query's achter deze metrische gegevens, de query's bewerken, maken van waarschuwingen, enzovoort.

![monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Volgende stappen

Zie [bewaken en beheren van pijplijnen programmatisch](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) voor meer informatie over het controleren en beheren van pijplijnen door het uitvoeren van scripts.
