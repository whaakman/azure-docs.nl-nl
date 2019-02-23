---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 002/08/2019
ms.openlocfilehash: 12c86ca71b7421678b68684cccca86411d604d61
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56740715"
---
Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft. Gebruik de container rapporteert over elke 10 tot 15 minuten.

De `docker run` maakt gebruik van de volgende argumenten voor factureringsdoeleinden bepalen:

| Optie | Description |
|--------|-------------|
| `ApiKey` | De API-sleutel van de resource voor de Cognitive Service gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op een API-sleutel voor de ingerichte resource die is opgegeven `Billing`. |
| `Billing` | Het eindpunt van de resource voor de Cognitive Service gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op de URI van een ingerichte LUIS Azure-resource van het eindpunt.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |

> [!IMPORTANT]
> Alle drie de opties met geldige waarden moeten worden opgegeven of de container start niet.
