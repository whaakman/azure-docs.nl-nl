---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: e128f3c67a41322d9c25a8d6941e937729760bf4
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227463"
---
In deze stap maakt u een firewall-regel om de testpoort voor het eindpunt met load balancing (zoals eerder opgegeven 59999) te openen en een andere regel om te openen, de poort beschikbaarheidsgroeplistener. Omdat u het met load balancing-eindpunt op de virtuele machines die replica's van beschikbaarheidsgroepen bevatten gemaakt, moet u de testpoort en de listener-poort op de desbetreffende VM's te openen.

1. Start op virtuele machines die als host fungeren van replica's, **Windows Firewall met geavanceerde beveiliging**.

2. Met de rechtermuisknop op **regels voor binnenkomende verbindingen**, en klik vervolgens op **nieuwe regel**.

3. Op de **regeltype** weergeeft, schakelt **poort**, en klik vervolgens op **volgende**.

4. Op de **protocollen en poorten** weergeeft, schakelt **TCP**, type **59999** in de **specifieke lokale poorten** vak en klik vervolgens op  **Volgende**.

5. Op de **actie** pagina, bewaart **de verbinding toestaan** geselecteerd en klik vervolgens op **volgende**.

6. Op de **profiel** pagina, accepteer de standaardinstellingen en klik vervolgens op **volgende**.

7. Op de **naam** pagina, in de **naam** tekst, geeft u de regelnaam van een, zoals **altijd op Listener-test poort**, en klik vervolgens op **voltooien**.

8. Herhaal de voorgaande stappen voor de poort beschikbaarheidsgroeplistener (zoals is opgegeven als eerder in de parameter $EndpointPort van het script) en geef vervolgens een naam voor de desbetreffende regel, zoals **altijd op Listener-poort**.

