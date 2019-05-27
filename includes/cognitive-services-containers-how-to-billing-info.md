---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2019
ms.openlocfilehash: 05961f8dd2788179a15e6bfe094484cf4770067b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124344"
---
Query's naar de container in rekening gebracht volgens de prijscategorie van de Azure-resource die wordt gebruikt voor de `<ApiKey>`.

Azure Cognitive Services-containers worden niet gelicentieerd voor het uitvoeren zonder verbinding met het factuuradres eindpunt voor het meten. U moet de containers om te communiceren factureringsgegevens met het eindpunt van de facturering te allen tijde inschakelen. Cognitive Services-containers verzenden geen gegevens van de klant, zoals de afbeelding of tekst die wordt geanalyseerd, naar Microsoft. 

### <a name="connect-to-azure"></a>Verbinding maken met Azure

De container moet de facturering argumentwaarden om uit te voeren. Deze waarden kunnen de container verbinding maken met het eindpunt van de facturering. Gebruik de container rapporteert over elke 10 tot 15 minuten. Als de container geen verbinding met Azure binnen de toegestane periode maken, de container blijft om uit te voeren, maar niet query's leveren totdat het eindpunt van de facturering is hersteld. De verbinding wordt geprobeerd 10 keer met de dezelfde tijdsinterval van 10 tot 15 minuten. Als er geen verbinding met het eindpunt van de facturering in het 10 probeert, de container niet meer werkt. 

### <a name="billing-arguments"></a>Facturering-argumenten

Alle drie van de volgende opties moet worden opgegeven met geldige waarden voor de `docker run` opdracht om te beginnen de container.

| Optie | Description |
|--------|-------------|
| `ApiKey` | De API-sleutel van de Cognitive Services-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op een API-sleutel voor de ingerichte resource die is opgegeven `Billing`. |
| `Billing` | Het eindpunt van de Cognitive Services-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op de URI van een ingerichte Azure-resource van het eindpunt.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |


