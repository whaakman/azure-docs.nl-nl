---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 67de9042af11a2b17c4b65f8225ecd0580b95466
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263601"
---
| Poort niet.| In- of uitschalen | Poort-bereik| Vereist|   Opmerkingen |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Uit|WAN |Nee|Uitgaande poort wordt gebruikt voor toegang tot internet om op te halen van updates. <br>De webproxy uitgaande is gebruiker worden geconfigureerd. |
| TCP 443 (HTTPS)|Uit|WAN|Ja|Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud.<br>De webproxy uitgaande is gebruiker worden geconfigureerd.|   
| UDP 53 (DNS)|Uit|WAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u een DNS-server voor clientbeheer op Internet.<br>We raden u aan met behulp van een lokale DNS-server. |
| UDP 123 (NTP)|Uit|WAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u een internet-gebaseerde NTP-server.  |
| UDP 67 (DHCP)|Uit|WAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u een DHCP-server.  |
| TCP 80 (HTTP)|In|LAN|Ja|Dit is de binnenkomende poort voor lokale UI op het apparaat voor lokaal beheer. <br>Toegang tot de gebruikersinterface voor het lokale via HTTP worden automatisch omgeleid naar HTTPS.  |
| TCP 443 (HTTPS)|In|LAN|Ja|Dit is de binnenkomende poort voor lokale UI op het apparaat voor lokaal beheer. |