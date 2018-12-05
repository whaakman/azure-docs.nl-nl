---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: eb1fe7f83ed83efe078be0aadf26cc7db6f498e2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52886326"
---
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| [Web-/ werkrollen per implementatie](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Exemplaar van de invoer-eindpunten](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) per implementatie |25 |25 |
| [Voer de eindpunten](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) per implementatie |25 |25 |
| [Interne eindpunten](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) per implementatie |25 |25 |
| [Service-certificaten die worden gehost](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) per implementatie |199 |199 |

<sup>1</sup>voor elke Cloudservice met Web/Worker-rollen kan hebben twee implementaties, één voor productie en één voor fasering. Houd er ook rekening mee dat deze limiet naar het aantal afzonderlijke rollen (configuratie) en niet het aantal instanties per rol (vergroten/verkleinen verwijst).

