---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2019
ms.openlocfilehash: b20ee2df1f468323c3298854371c97950c017e49
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57788830"
---
Query's naar de container in rekening gebracht volgens de prijscategorie van de Azure-resource die wordt gebruikt voor de `<ApiKey>`.

Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft. Gebruik de container rapporteert over elke 10 tot 15 minuten.

De `docker run` maakt gebruik van de volgende argumenten voor factureringsdoeleinden bepalen:

| Optie | Description |
|--------|-------------|
| `ApiKey` | De API-sleutel van de resource voor de Cognitive Service gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op een API-sleutel voor de ingerichte resource die is opgegeven `Billing`. |
| `Billing` | Het eindpunt van de resource voor de Cognitive Service gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op de URI van een ingerichte LUIS Azure-resource van het eindpunt.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |

> [!IMPORTANT]
> Alle drie de opties met geldige waarden moeten worden opgegeven of de container start niet.
