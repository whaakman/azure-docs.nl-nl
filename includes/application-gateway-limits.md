---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 9a8d925e5d72500817028b913a80255a4339255c
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148413"
---
| Resource | Standaard-/ maximumlimiet | Opmerking |
| --- | --- | --- |
| Azure Application Gateway |1000 per abonnement | |
| Front-end-IP-configuraties |2 |1 openbaar en 1 privé |
| Front-endpoorten |100<sup>1</sup> | |
| Back-end-adresgroepen |100<sup>1</sup> | |
| Back-endservers per groep |1,200 | |
| HTTP-listeners |100<sup>1</sup> | |
| HTTP-taakverdelingsregels |100<sup>1</sup> | |
| Back-end-HTTP-instellingen |100<sup>1</sup> | |
| Exemplaren per gateway |32 | |
| SSL-certificaten |100<sup>1</sup> |1 per HTTP-listeners |
| Maximumgrootte voor SSL-certificaat |V1-SKU - 10 KB<br>V2-SKU - 16 KB| |
| Certificaten voor verificatie |100 | |
| Vertrouwde basiscertificaten |100 | |
| Minimale time-out van aanvraag |1 seconde | |
| Maximale time-out van aanvraag |24 uur | |
| Aantal sites |100<sup>1</sup> |1 per HTTP-listeners |
| URL-toewijzingen per listener |1 | |
| Maximale op pad gebaseerde regels per URL-kaart|100||
| Omleidings-configuraties |100<sup>1</sup>| |
| Gelijktijdige WebSocket-verbindingen |Gemiddeld gateways 20k<br> Grote gateways 50k| |
| Maximale URL-lengte|8,000||
| Maximale bestandsgrootte voor uploaden, Standard |2 GB | |
| Maximale upload grootte WAF |Gemiddeld WAF-gateways, 100 MB<br>Grote WAF-gateways, 500 MB| |
| WAF hoofdtekst van de maximale grootte, zonder bestanden|128 KB||
|Maximum aantal aangepaste regels voor WAF|100||

<sup>1</sup> in het geval van WAF-functionaliteit SKU's, het is raadzaam dat u het aantal resources tot 40 voor optimale prestaties beperken.
