---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/09/2018
ms.author: estfan
ms.openlocfilehash: 3fa71085d649ace95aa24ac87c8714a7268f5386
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269682"
---
Voor een meer nauwkeurige verbruikskosten schatting, houd rekening met het aantal berichten of gebeurtenissen die mogelijk op een willekeurige dag binnenkomen, in plaats van uw berekeningen baseren op alleen het polling-interval. Wanneer een gebeurtenis of een bericht voldoet aan de criteria van de trigger, wordt er onmiddellijk veel triggers probeert te lezen en alle andere wachten gebeurtenissen of de berichten die voldoen aan de criteria. Dit gedrag betekent dat zelfs als u een langer polling-interval, de trigger wordt geactiveerd op basis van het aantal gebeurtenissen van de wachtrij of berichten die in aanmerking komen voor het starten van werkstromen. Triggers die dit gedrag volgen zijn Azure Service Bus en Azure Event Hub.

Dus Stel bijvoorbeeld dat u de trigger waarmee wordt gecontroleerd of een eindpunt voor elke dag instellen. Wanneer de trigger controleert of het eindpunt en 15 gebeurtenissen die voldoen aan de criteria wordt gevonden, wordt de trigger wordt geactiveerd en de bijbehorende werkstroom 15 keer uitvoert. Logic Apps-meters alle acties die deze 15 werkstromen, met inbegrip van de trigger-aanvragen uitvoert. Voor het berekenen van de mogelijke kosten, probeert de [prijscalculator van Azure](https://azure.microsoft.com/pricing/calculator/).