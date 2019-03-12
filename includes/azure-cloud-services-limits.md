---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: acf5a7e8d7feebaac3d82629c5b561a22112de0f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553414"
---
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| [Web- of werkrollen rollen per implementatie](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Exemplaar invoer eindpunten](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) per implementatie |25 |25 |
| [Voer de eindpunten](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) per implementatie |25 |25 |
| [Interne eindpunten](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) per implementatie |25 |25 |
| [Service-certificaten die worden gehost](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) per implementatie |199 |199 |

<sup>1</sup>voor elke Azure-Cloudservice met web-of werkrollen kan hebben twee implementaties, één voor productie en één voor fasering. Deze limiet verwijst naar het aantal afzonderlijke rollen, dat wil zeggen, de configuratie. Deze limiet verwijst niet naar het aantal instanties per rol, dat wil zeggen, schalen.

