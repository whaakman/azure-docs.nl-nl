---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 4a3925752d1af5e43d5984b06c0a68aa9faa214b
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839740"
---
| Poort niet.| In- of uitschalen | Poort-bereik| Verplicht| Opmerkingen |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|In|LAN|Ja|Deze poort wordt gebruikt voor het verbinding maken met gegevens in het Blog storage REST-API's via HTTP. Als geen verbinding maken met REST-API's, dit automatisch wordt omgeleid naar lokale web-UI meer dan 8443. |
| TCP 443 (HTTPS)|In|LAN|Ja|Deze poort wordt gebruikt via HTTPS verbinding maken met gegevens in BLOB-opslag REST-API's. Als geen verbinding maken met REST-API's, dit automatisch wordt omgeleid naar lokale web-UI meer dan 8443. |
| TCP 8443 (HTTPS-Alt)|In|LAN|Ja|Dit is een alternatieve poort voor HTTPS en wordt gebruikt bij het verbinden met de lokale web-UI voor Apparaatbeheer. |
| TCP 445 (SMB)|Out In|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u verbinding via SMB maakt. |
| TCP 2049 (NFS)|Out In|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u verbinding via NFS maakt. |
| TCP 111 (NFS)|Out In|LAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort wordt gebruikt voor de toewijzing van rpcbind/poort en alleen vereist als u verbinding via NFS maakt.  |
