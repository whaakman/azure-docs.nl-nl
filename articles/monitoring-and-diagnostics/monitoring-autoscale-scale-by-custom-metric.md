---
title: Aan de slag met automatisch schalen door aangepaste metrische gegevens in Azure | Microsoft Docs
description: Informatie over het schalen van uw resource door aangepaste metrische gegevens in Azure.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: 72b6a68d0dbad4639f21aa701ec4865f36409f0a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Aan de slag met automatisch schalen door aangepaste metrische gegevens in Azure
In dit artikel wordt beschreven hoe uw resource schalen door een aangepaste waarde in Azure-portal.

Azure Monitor automatisch schalen geldt alleen voor virtuele Machine Scale Sets (VMSS), cloudservices, app-serviceabonnementen en app service-omgevingen. 

# <a name="lets-get-started"></a>Hiermee kunnen aan de slag
In dit artikel wordt ervan uitgegaan dat u een web-app met application insights is geconfigureerd hebt. Als u nog geen een hebt, kunt u [Application Insights instellen voor uw ASP.NET-website][1]

- Open [Azure-portal][2]
- Klik op het pictogram van de Azure-Monitor in het navigatiedeelvenster links.
  ![Azure Monitor starten][3]
- Klik op de instelling voor automatisch schalen om weer te geven van alle resources waarop automatische schaal van toepassing, samen met de huidige status voor automatisch schalen is ![automatisch schalen in Azure monitor detecteren][4]
- 'Automatisch ' schalen-blade geopend in de Azure-Monitor en selecteer een resource die u wilt schalen
> Opmerking: De onderstaande stappen gebruiken een app service-plan dat is gekoppeld aan een WebApp die app insights geconfigureerd is.
- U ziet dat het huidige aantal exemplaren 1 is in de blade van de instelling schaal voor de resource. Klik op 'Inschakelen voor automatisch schalen'.
  ![Instelling voor de nieuwe web-app schalen][5]
- Geef een naam voor de instelling schalen en de Klik op "Een regel toevoegen". Let op de regel schalingsopties dat wordt geopend in een context deelvenster in de rechterkant. Standaard wordt de optie voor het schalen van het aantal exemplaren op 1 als het CPU-percetage van de resource die groter is dan 70% ingesteld. De metrische bron boven wijzigen in 'Application Insights', selecteert u de app insights-resource in de vervolgkeuzelijst 'Resource' en selecteer vervolgens de aangepaste metrische gegevens op basis van die u wilt schalen.
  ![Schalen door aangepaste metrische gegevens][6]
- Vergelijkbaar met de stap hierboven, een scale-regel die schalen en pas het schaalaanpassingsaantal door 1 als de aangepaste waarde die lager dan een drempelwaarde is toevoegen.
  ![Schalen op basis van cpu][7]
- Stel u limieten-instantie. Bijvoorbeeld, als u wilt schalen tussen 2-5-exemplaren, afhankelijk van de aangepaste metrische schommelingen, ingesteld 'minimale' op '2', maximale '5' en 'default' en ' 2'
> Opmerking: als er een probleem bij het lezen van de metrische gegevens voor resources en de huidige capaciteit lager dan de standaardcapaciteit is, klikt u vervolgens om te controleren of de beschikbaarheid van de bron voor automatisch schalen wordt uitschalen op de standaardwaarde. Als de huidige capaciteit al hoger dan de standaardcapaciteit van de is, wordt niet automatisch schalen schalen.
- Klik op 'Opslaan'

Gefeliciteerd! U nu nu met succes is gemaakt voor uw scale instelling automatisch schalen uw web-app op basis van een aangepaste waarde.

> Opmerking: Er zijn dezelfde stappen van toepassing aan de slag met een VMSS of cloud service-rol.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
