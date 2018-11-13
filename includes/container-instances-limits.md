---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: 44bdaec78e1fad574f29a5945b07041b588aaff8
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572029"
---
| Resource | Standaardlimiet |
| --- | :--- |
| Containergroepen per [abonnement](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| Aantal containers per containergroep | 60 |
| Aantal volumes per containergroep | 20 |
| Poorten per IP-adres | 5 |
| Aangemaakte containers per uur |300<sup>1</sup> |
| Aangemaakte containers per 5 minuten | 100<sup>1</sup> |
| Verwijderde containers per uur | 300<sup>1</sup> |
| Verwijderde containers per 5 minuten | 100<sup>1</sup> |
| Meerdere containers per containergroep | Alleen voor Linux<sup>2</sup> |
| Azure Files-volumes | Alleen voor Linux<sup>2</sup> |
| GitRepo-volumes | Alleen voor Linux<sup>2</sup> |
| Geheime volumes | Alleen voor Linux<sup>2</sup> |

<sup>1</sup> Verstuur een [Azure ondersteuningsaanvraag][azure-support] om een hogere limiet aan te vragen.<br />
<sup>2</sup> Windows-ondersteuning voor deze functie is gepland.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
