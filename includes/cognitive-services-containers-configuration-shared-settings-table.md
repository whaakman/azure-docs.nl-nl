---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2019
ms.openlocfilehash: 1a3de57446c5c92afbc5dd5901e284bc3580f9db
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479008"
---
Deze container heeft de volgende configuratie-instellingen:

|Vereist|Instelling|Doel|
|--|--|--|
|Ja|[ApiKey](#apikey-setting)|Gebruikt voor het bijhouden van informatie over facturering.|
|Nee|[ApplicationInsights](#applicationinsights-setting)|Hiermee kunt u toevoegen [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container.|
|Ja|[Facturering](#billing-setting)|Hiermee geeft u de URI van de bron van de service-eindpunt op Azure.|
|Ja|[Gebruiksrechtovereenkomst](#eula-setting)| Geeft aan dat u de licentie voor de container hebt geaccepteerd.|
|Nee|[Fluentd](#fluentd-settings)|Logboek schrijven en, optioneel, metrische gegevens naar een Fluentd-server.|
|Nee|[HTTP-Proxy](#http-proxy-credentials-settings)|Configureer een HTTP-proxy voor uitgaande aanvragen.|
|Nee|[Logboekregistratie](#logging-settings)|ASP.NET Core logboekregistratie van ondersteuning voor de container biedt. |
|Nee|[Hiermee wordt gekoppeld](#mount-settings)|Gegevens lezen en schrijven van hostcomputer naar de container en naar de container weer met hostcomputer.|
