---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2019
ms.openlocfilehash: e92d1c65d9601c23e7e785f07e2de3e43ea6612b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598910"
---
Query's naar de container in rekening gebracht volgens de prijscategorie van de Azure-resource die wordt gebruikt voor de `<ApiKey>`.

Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met het factuuradres eindpunt voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens aan facturering-eindpunt te allen tijde inschakelen. Cognitive Services-containers verzenden gegevens van de klant (bijvoorbeeld, de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft. 

### <a name="connecting-to-azure"></a>Verbinding maken met Azure

De container moet de facturering argumentwaarden om uit te voeren. Deze waarden kunnen de container verbinding maken met facturering eindpunt. Gebruik de container rapporteert over elke 10 tot 15 minuten. Als de container geen verbinding binnen de toegestane periode naar Azure maken, de container wordt voortgezet, maar wordt niet query's leveren totdat het eindpunt van de facturering is hersteld. De verbinding wordt geprobeerd 10 keer met de dezelfde tijdsinterval van 10 tot 15 minuten. Als er geen verbinding met het eindpunt van de facturering binnen de 10 probeert, stopt de container uitgevoerd. 

### <a name="billing-arguments"></a>Facturering-argumenten

Alle drie van de volgende opties moet worden opgegeven met geldige waarden voor de `docker run` opdracht om te beginnen de container:

| Optie | Description |
|--------|-------------|
| `ApiKey` | De API-sleutel van de resource voor de Cognitive Service gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op een API-sleutel voor de ingerichte resource die is opgegeven `Billing`. |
| `Billing` | Het eindpunt van de resource voor de Cognitive Service gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op de URI van een ingerichte Azure-resource van het eindpunt.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |


