---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/11/2019
ms.author: alkohli
ms.openlocfilehash: 783ae29e9ca0c9a609d1d1d283525a952c2f7f33
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118687"
---
| Poort niet.| In- of uitschalen | Poort-bereik| Vereist|   Opmerkingen |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Out In|WAN  |Nee|Uitgaande poort wordt gebruikt voor toegang tot internet om op te halen van updates. <br>De webproxy uitgaande is gebruiker worden geconfigureerd. |
| TCP 443 (HTTPS)|Out In|WAN|Ja|Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud.<br>De webproxy uitgaande is gebruiker worden geconfigureerd.|
| UDP 123 (NTP)|Uit|WAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u een internet-gebaseerde NTP-server.  |   
| UDP 53 (DNS)|Uit|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u een DNS-server voor clientbeheer op Internet.<br>We raden u aan met behulp van een lokale DNS-server. |
| TCP 5985 (WinRM)|Out In|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is vereist voor het verbinding maken met het apparaat via externe PowerShell via HTTP.  |
| UDP 67 (DHCP)|Uit|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u een lokale DHCP-server.  |
| TCP 80 (HTTP)|Out In|LAN|Ja|Dit is de binnenkomende poort voor lokale UI op het apparaat voor lokaal beheer. <br>Toegang tot de gebruikersinterface voor het lokale via HTTP worden automatisch omgeleid naar HTTPS.  |
| TCP 443 (HTTPS)|Out In|LAN|Ja|Dit is de binnenkomende poort voor lokale UI op het apparaat voor lokaal beheer. |
| TCP 445 (SMB)|In|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u verbinding via SMB maakt. |
| TCP 2049 (NFS)|In|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u verbinding via NFS maakt. |

