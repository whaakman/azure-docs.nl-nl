---
title: Azure Security Center voor IoT-vereisten | Microsoft Docs
description: Meer informatie over alles wat u nodig hebt om aan de slag te gaan met Azure Security Center voor IoT-service vereisten.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: mlottner
ms.openlocfilehash: cc2dc3a190e3ad06bdc048f2a5770eae2a6990ec
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596828"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Azure Security Center voor IoT-vereisten

Dit artikel bevat een uitleg van de verschillende bouw stenen van de Azure Security Center (ASC) voor de IoT-service, wat u moet beginnen en legt de basis concepten uit om inzicht te krijgen in de service. 

## <a name="minimum-requirements"></a>Minimale vereisten

- IoT Hub Standard-laag
    - Bevoegdheden van het niveau van de RBAC-rol **eigenaar** 
- [Log Analytics Workspace](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (aanbevolen)
    - Het gebruik van Azure Security Center is een aanbeveling en geen vereiste. Zonder Azure Security Center kunt u uw andere Azure-resources niet weer geven binnen IoT Hub. 
 
## <a name="working-with-asc-for-iot-service"></a>Werken met ASC voor IoT-service

ASC voor IoT Insights en rapportage zijn beschikbaar via Azure IoT Hub en Azure Security Center. Als u ASC wilt inschakelen voor IoT op uw Azure-IoT Hub, is een account met bevoegdheden op **eigenaars** niveau vereist. Nadat u ASC voor IoT hebt ingeschakeld in uw IoT Hub, worden de **beveiligings** functies in azure IOT hub en als **IOT** in azure Security Center weer gegeven. 

## <a name="supported-service-regions"></a>Ondersteunde service regio's 

ASC voor IoT wordt momenteel ondersteund voor IoT-hubs in de volgende Azure-regio's:
  - US - centraal  
  - East US 
  - US - oost 2
  - US - west-centraal
  - US - west
  - VS - west 2
  - VS Zuid-Centraal
  - US - noord-centraal
  - Canada - midden
  - Canada - oost 
  - Europa - noord    
  - Brazilië - zuid
  - Frankrijk - centraal  
  - Verenigd Koninkrijk West 
  - Verenigd Koninkrijk Zuid
  - Europa -west 
  - Noord-Europa 
  - Japan - west  
  - Japan - oost  
  - Australië - zuidoost
  - Australië - oost
  - Azië - oost   
  - Azië - zuidoost
  - Korea - centraal
  - Korea - zuid 
  - India - centraal
  - India - zuid
  
## <a name="wheres-my-iot-hub"></a>Waar is mijn IoT Hub?

Controleer de locatie van uw IoT Hub om de beschik baarheid van de service te controleren voordat u begint. 

1. Open uw IoT Hub. 
2. Klik op **Overzicht**. 
3. Controleer of de weer gegeven locatie overeenkomt met een van de [ondersteunde service regio's](#supported-service-regions). 


## <a name="supported-platforms-for-agents"></a>Ondersteunde platforms voor agents 

ASC voor IoT-agents biedt ondersteuning voor een groeiende lijst met apparaten en platformen. Zie de [lijst met ondersteunde platforms](how-to-deploy-agent.md) om uw bestaande of geplande Apparaatbeheer te controleren.  

## <a name="next-steps"></a>Volgende stappen
- Lees het [overzicht](overview.md) van Azure IOT Security
- Meer informatie over [het inschakelen van de service](quickstart-onboard-iot-hub.md)
- Lees de [Veelgestelde vragen over Azure Security Center voor IOT](resources-frequently-asked-questions.md)
- Ontdek hoe u [Azure Security Center begrijpt voor IOT-waarschuwingen](concept-security-alerts.md)
