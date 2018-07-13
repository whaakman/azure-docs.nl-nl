---
title: Azure IoT Hub migreren naar diagnostische instellingen | Microsoft Docs
description: Het bijwerken van Azure IoT Hub voor het gebruik van Azure diagnostics-instellingen in plaats van bewerkingen voor het controleren van de status van bewerkingen op uw IoT-hub in realtime controleren.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 85bdb4b4802283c591e4d7a9e8b14ae74fa44e8d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666719"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migreren van uw IoT-Hub van bewerkingen controleren naar diagnostische instellingen

Klanten die gebruikmaken van [bewerkingen controleren] [ lnk-opsmon] om bij te houden van de status van bewerkingen in IoT Hub kunnen migreren die werkstroom [Azure diagnostische instellingen] [ lnk-diagnostics-settings], een functie van Azure Monitor. Instellingen voor diagnostische gegevens leveren resourceniveau diagnostische gegevens voor veel Azure-services.

De bewerkingen bewakingsfunctionaliteit van IoT Hub is verouderd en in de toekomst wordt verwijderd. In dit artikel bevat stappen voor het verplaatsen van uw workloads van bewerkingen controleren naar diagnostische instellingen. Zie voor meer informatie over de tijdlijn afschaffing [bewaken van uw Azure-IoT-oplossingen met Azure Monitor en Azure Resource Health][lnk-blog-announcement].

## <a name="update-iot-hub"></a>IoT Hub bijwerken

Voor het bijwerken van uw IoT-Hub in Azure portal, eerst de diagnostische instellingen inschakelen en uitschakelen van bewerkingen controleren.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Bewerkingen controleren uitschakelen

Nadat u de nieuwe instellingen voor diagnostische gegevens over uw werkstroom getest hebt, kunt u de bewerkingen controleren van functie uitschakelen. 

1. Selecteer in uw IoT Hub-menu **bewerkingen controleren**.
1. Selecteer onder elke categorie bewaking **geen**.
1. Sla de bewerkingen die het bewaken van wijzigingen.

## <a name="update-applications-that-use-operations-monitoring"></a>Bijwerken van toepassingen die gebruikmaken van bewerkingen controleren

De schema's voor bewerkingen controleren en instellingen voor diagnostische gegevens afwijken enigszins. Het is belangrijk dat u de toepassingen die gebruikmaken van bewerkingen controleren vandaag om toe te wijzen aan het schema dat wordt gebruikt door de diagnostische instellingen bijwerken. 

Ook diagnostische instellingen aanbiedingen bijhouden voor vijf nieuwe categorieën. Na het bijwerken van toepassingen voor de bestaande schema's, evenals de nieuwe categorieën worden toegevoegd:

- Cloud-naar-apparaat dubbele bewerkingen
- Dubbele apparaat-naar-cloud-bewerkingen
- Apparaatdubbel-query 's
- Taakbewerkingen
- Directe methoden

Zie voor de specifieke schemastructuren, [inzicht in het schema voor de diagnostische instellingen][lnk-diagnostics-schema].

## <a name="next-steps"></a>Volgende stappen

- [Controleer de status van Azure IoT Hub en snel problemen vaststellen][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/
[lnk-monitor]: iot-hub-monitor-resource-health.md
