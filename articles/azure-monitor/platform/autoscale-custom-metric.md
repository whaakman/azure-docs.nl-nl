---
title: Automatisch schalen in Azure met behulp van een aangepaste meetwaarde
description: Leer hoe u uw resource schalen door aangepaste metrische gegevens in Azure.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 275c954ac54aab5ab4b41f4d6f6610a01573483f
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54319066"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Aan de slag met automatisch schalen door aangepaste metrische gegevens in Azure
In dit artikel wordt beschreven hoe u uw resource schalen door een aangepaste metrische gegevens in Azure portal.

Automatisch schalen van Azure Monitor is alleen bedoeld voor [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/), en [API Management-services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="lets-get-started"></a>Hiermee kunnen aan de slag
In dit artikel wordt ervan uitgegaan dat u een web-app met application insights is geconfigureerd. Als u er nog geen hebt, kunt u [Application Insights instellen voor uw ASP.NET-website][1]

- Open [Azure-portal][2]
- Klik op het pictogram in het navigatiedeelvenster links in Azure Monitor.
  ![Starten met Azure Monitor][3]
- Klik op de instelling voor automatisch schalen om alle resources voor welke automatisch schalen van toepassing, samen met de huidige status van de functie voor automatisch schalen is weer te geven ![detecteren, automatisch schalen in Azure monitor][4]
- Open 'voor automatisch schalen-blade in Azure Monitor en selecteer een resource die u wilt schalen
> Opmerking: De onderstaande stappen gebruiken een app service-plan dat is gekoppeld aan een web-app met app insights is geconfigureerd.
- U ziet dat het huidige aantal instanties 1 is in de blade van de instelling schaal voor de resource. Klik op 'Inschakelen voor automatisch schalen'.
  ![Instelling voor de nieuwe web-app schalen][5]
- Geef een naam voor de schaalinstelling van de en het klikken op 'Een regel toevoegen'. U ziet de schaalopties regel die wordt geopend in een deelvenster context in de rechterkant. Standaard wordt de optie voor het schalen van uw aantal instanties met 1 als het CPU-percentage van de resource groter is dan 70%. De bron van metrische gegevens aan de bovenkant wijzigen in 'Application Insights', selecteert u de app insights-resource in de vervolgkeuzelijst 'Resource' en selecteer vervolgens de aangepaste metrische gegevens op basis van die u wilt schalen.
  ![Schalen door aangepaste metrische gegevens][6]
- Net als bij de stap hierboven, een scale-regel die wordt geschaald en de schaalaanpassingsaantal verlagen met 1 als de aangepaste metrische gegevens lager dan een drempel is toevoegen.
  ![Schaal op basis van cpu][7]
- Stel u instantielimieten. Bijvoorbeeld, als u schalen tussen 2-5-exemplaren, afhankelijk van de aangepaste metrische fluctuaties wilt, ingesteld 'minimale' op '2', 'maximale' op '5' en 'default' op ' 2'
> Opmerking: In het geval er een probleem opgetreden bij het lezen van de metrische gegevens voor resources en de huidige capaciteit lager dan de standaardcapaciteit is, wordt klikt u vervolgens voor de beschikbaarheid van de resource voor automatisch schalen uitgeschaald op de standaardwaarde. Als de huidige capaciteit al hoger is dan de standaardcapaciteit, wordt niet automatisch schalen geschaald.
- Klik op 'Opslaan'

Gefeliciteerd! U nu de instelling voor automatisch schalen van uw web-app op basis van een aangepaste meetwaarde schaalset is gemaakt.

> Opmerking: Dezelfde stappen zijn van toepassing aan de slag met een VMSS of cloud service-rol.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/autoscale-custom-metric/azure-monitor-launch.png
[4]: ./media/autoscale-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/autoscale-custom-metric/scale-setting-new-web-app.png
[6]: ./media/autoscale-custom-metric/scale-by-custom-metric.png
[7]: ./media/autoscale-custom-metric/autoscale-setting-custom-metrics-ai.png
