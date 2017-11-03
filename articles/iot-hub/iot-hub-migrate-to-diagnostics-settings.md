---
title: Azure IoT Hub migreren naar de diagnostische instellingen | Microsoft Docs
description: Het bijwerken van Azure IoT Hub Azure diagnostics om instellingen te gebruiken in plaats van bewerkingen voor het controleren van de status van bewerkingen voor uw IoT-hub in real-time bewaking.
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 2716f397ad0e7abfdcd397340da8fa8116a172db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migreren van uw IoT-Hub van bewaking aan de diagnostische instellingen bewerkingen

Klanten die gebruikmaken van [operations bewaking] [ lnk-opsmon] voor het bijhouden van de status van bewerkingen in IoT-Hub kunnen migreren die werkstroom [Azure diagnostische instellingen] [ lnk-diagnostics-settings], een functie van Azure-Monitor. Diagnostische instellingen opgeven voor diagnostische gegevens van de resource-niveau voor vele Azure-services.

De bewerkingen controlefunctionaliteit van IoT Hub is afgeschaft en wordt in de toekomst verwijderd. Dit artikel bevat stappen om uw werkbelastingen verplaatsen van bewerkingen bewaking aan de diagnostische instellingen. Zie voor meer informatie over de tijdlijn afschaffing [bewaken van uw Azure-IoT-oplossingen met Azure Monitor en Azure resourcestatus][lnk-blog-announcement].

## <a name="update-iot-hub"></a>Bijwerken van IoT-Hub

Voor het bijwerken van uw IoT-Hub in de Azure portal, schakelt u eerst de diagnostische instellingen vervolgens bewerkingen bewaking uitschakelen.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Bewerkingen bewaking uitschakelen

Zodra u de nieuwe instellingen voor diagnostische gegevens over uw werkstroom getest hebt, kunt u de bewaking van de functie operations uitschakelen. 

1. Selecteer in uw IoT Hub-menu **Operations bewaking**.
1. Selecteer onder elke categorie bewaking **geen**.
1. Sla de wijzigingen controleren bewerkingen.

## <a name="update-applications-that-use-operations-monitoring"></a>Bijwerken van toepassingen die gebruikmaken van bewerkingen controleren

De schema's voor bewerkingen controle en diagnostische instellingen afwijken enigszins. Het is belangrijk dat u de toepassingen die gebruikmaken van bewaking vandaag bewerkingen toewijzen aan het schema dat wordt gebruikt door de diagnostische instellingen bijwerken. 

Ook diagnostische instellingen aanbiedingen bijhouden in vijf nieuwe categorieën. Na het bijwerken van toepassingen voor het bestaande schema toevoegen nieuwe categorieën:

- Cloud-naar-apparaat twin bewerkingen
- Apparaat-naar-cloud-twin bewerkingen
- Dubbele query 's
- Taakbewerkingen
- Rechtstreekse methoden

Zie voor de specifieke schemastructuren, [inzicht in het schema voor de diagnostische instellingen][lnk-diagnostics-schema].

## <a name="next-steps"></a>Volgende stappen

- [De status van Azure IoT Hub bewaken en problemen snel onderzoeken][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health
[lnk-monitor]: iot-hub-monitor-resource-health.md
