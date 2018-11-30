---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 3d66d825306c5183bdd8d8e611d98904eef2022a
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440097"
---
| Resource | Standaardlimiet | Opmerking |
| --- | --- | --- |
| Application Gateway |1000 per abonnement | |
| Front-end-IP-configuraties |2 |1 openbaar en 1 privé |
| Front-endpoorten |40 | |
| Back-endadresgroepen |40 | |
| Back-endservers per groep |1200 | |
| HTTP-listeners |40 | |
| HTTP-taakverdelingsregels |400 |Totaal aantal HTTP-Listeners * n |
| Back-end-HTTP-instellingen |40 | |
| Exemplaren per gateway |75 | |
| SSL-certificaten |40 |1 per HTTP-listener |
| Certificaten voor verificatie |40 | |
| Time-out min aanvragen |1 seconde | |
| Maximale time-out aanvraag |24 uur | |
| Aantal sites |40 |1 per HTTP-listener |
| URL Maps per listener |1 | |
| Maximale op pad gebaseerde regels per URL-kaart|100|
| Omleidings-configuraties |40| |
| Gelijktijdige WebSocket-verbindingen |5000| |
|Maximale URL-lengte|8000|
| Maximale bestandsgrootte voor uploaden Standard |2 GB | |
| Maximale upload grootte WAF |Gemiddeld WAF-Gateways, 100 MB<br>Grote WAF Gateways - 500 MB| |
|WAF hoofdtekst van de maximale grootte (zonder-bestanden)|128 KB|
