---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 8f7eae06947a40117f3952a0a5624c22df750b34
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165545"
---
* Geef in **vCenter toevoegen** een beschrijvende naam op voor de vSphere-host of vCenter-server, en geef vervolgens het IP-adres of de FQDN op voor de server. Behoud poort 443 tenzij de VMware-servers zijn geconfigureerd om te luisteren naar aanvragen van een andere poort. Selecteer het account dat is verbonden met de VMware vCenter-of vSphere ESXi-server. Klik op **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Als u de VMware vCenter-server of VMware vSphere-host toevoegt met een account zonder beheerdersbevoegdheden voor de vCenter- of hostserver, moet u ervoor zorgen dat voor het account deze bevoegdheden zijn ingeschakeld: Datacenter, Gegevensopslag, Map, Host, Netwerk, Resource, Virtuele machine en Gedistribueerde vSphere-switch. Daarnaast moet voor de VMware vCenter-server de bevoegdheid Opslagweergaven zijn ingeschakeld.
