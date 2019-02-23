---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 6f7772eb7f2c500bbb58c391b1bc4b7a73141699
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56675752"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Logboekregistratie inschakelen met de diagnostische instellingen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-hub.

2. Selecteer **diagnostische instellingen**.

3. Selecteer **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens inschakelen](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. De diagnostische instellingen voor een naam geven.

5. Kies waar u om de logboeken te verzenden. U kunt een willekeurige combinatie van de drie opties selecteren:

   * Archiveren naar een opslagaccount
   * Streamen naar een Event Hub
   * Verzenden naar Log Analytics

6. Kiezen welke bewerkingen u wilt bewaken, en schakel logboeken voor deze bewerkingen. Er zijn de bewerkingen die diagnostische instellingen kunnen worden weergegeven:

   * Verbindingen
   * Telemetrie van apparaten
   * Cloud-naar-apparaat-berichten
   * Bewerkingen voor apparaat-id
   * Uploaden van bestanden
   * Berichtroutering
   * Cloud-naar-apparaat dubbele bewerkingen
   * Dubbele apparaat-naar-cloud-bewerkingen
   * Dubbele bewerkingen
   * Taakbewerkingen
   * Directe methoden  
   * Gedistribueerde tracering (preview)
   * Configuraties
   * Apparaat-streams
   * Metrische gegevens van apparaat

6. De nieuwe instellingen opslaan. 

Als u inschakelen op de instellingen voor diagnostische gegevens met PowerShell wilt, gebruikt u de volgende code:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Nieuwe instellingen van kracht in ongeveer 10 minuten. Hierna logboeken worden weergegeven in de geconfigureerde archivering doel op de **diagnostische instellingen** blade. Zie voor meer informatie over het configureren van diagnostische gegevens [verzamelen en gebruiken van logboekgegevens van uw azure-resources](../articles/azure-monitor/platform/diagnostic-logs-overview.md).