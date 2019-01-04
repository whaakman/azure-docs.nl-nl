---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: 31a0acbfd0c9714bf2cbdf20f9f1f82edb07f05a
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977347"
---
De `ApplicationInsights` instelling kunt u om toe te voegen [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container. Application Insights biedt uitgebreide bewaking van de container. U kunt de container voor beschikbaarheid, prestaties en gebruik eenvoudig bewaken. U kunt ook snel identificeren en opsporen van fouten in de container.

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `ApplicationInsights` sectie.

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Nee| `InstrumentationKey` | Reeks | De instrumentatiesleutel van de Application Insights-exemplaar aan welke telemetrie gegevens voor de container is verzonden. Zie voor meer informatie, [Application Insights voor ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Voorbeeld:<br>`InstrumentationKey=123456789`|

