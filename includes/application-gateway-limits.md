---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 66dea07a1ff725c6707b19bc6ebdc5563f1b158b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211830"
---
| Resource | Standaardlimiet | Opmerking |
| --- | --- | --- |
| Application Gateway |1000 per abonnement | |
| Front-end-IP-configuraties |2 |1 openbaar en 1 privé |
| Front-endpoorten |100<sup>1</sup> | |
| Back-end-adresgroepen |100<sup>1</sup> | |
| Back-endservers per groep |1200 | |
| HTTP-listeners |100<sup>1</sup> | |
| HTTP-taakverdelingsregels |100<sup>1</sup> | |
| Back-end-HTTP-instellingen |100<sup>1</sup> | |
| Exemplaren per gateway |32 | |
| SSL-certificaten |100<sup>1</sup> |1 per HTTP-listener |
| Certificaten voor verificatie |100 | |
| Vertrouwde basiscertificaten |100 | |
| Time-out min aanvragen |1 seconde | |
| Maximale time-out aanvraag |24 uur | |
| Aantal sites |100<sup>1</sup> |1 per HTTP-listener |
| URL-toewijzingen per listener |1 | |
| Maximale op pad gebaseerde regels per URL-kaart|100||
| Omleidings-configuraties |100<sup>1</sup>| |
| Gelijktijdige WebSocket-verbindingen |5000| |
| Maximale URL-lengte|8000||
| Maximale bestandsgrootte voor uploaden Standard |2 GB | |
| Maximale upload grootte WAF |Gemiddeld WAF-Gateways, 100 MB<br>Grote WAF Gateways - 500 MB| |
| WAF hoofdtekst van de maximale grootte (zonder-bestanden)|128 KB||

<sup>1</sup> in het geval van WAF-functionaliteit SKU's is het aanbevolen dat u het aantal resources tot 40 voor optimale prestaties beperken.
