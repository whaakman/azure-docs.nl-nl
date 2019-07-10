---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 46376477aae9f94c1c8f6e1dd6bc718d213cc373
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712418"
---
De container heeft de volgende configuratie-instellingen:

|Verplicht|Instelling|Doel|
|--|--|--|
|Ja|[ApiKey](#apikey-configuration-setting)|Nummers factureringsgegevens.|
|Nee|[ApplicationInsights](#applicationinsights-setting)|Toe te voegen kunt [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container.|
|Ja|[Billing](#billing-configuration-setting)|Hiermee wordt de eindpunt-URI van de serviceresource in Azure opgegeven.|
|Ja|[Eula](#eula-setting)| Geeft aan dat u de licentie voor de container hebt geaccepteerd.|
|Nee|[Fluentd](#fluentd-settings)|Schrijfbewerkingen log en, optioneel, metrische gegevens naar een Fluentd-server.|
|Nee|HTTP-Proxy|Hiermee configureert u een HTTP-proxy voor uitgaande aanvragen.|
|Nee|[Logging](#logging-settings)|Biedt ondersteuning voor ASP.NET Core logboekregistratie voor de container. |
|Nee|[Mounts](#mount-settings)|Leest en schrijft gegevens van de computer naar de container en de container terug naar de computer.|
