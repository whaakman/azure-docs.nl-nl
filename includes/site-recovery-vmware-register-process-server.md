---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: cf39baf34096691144181332566cf567ebc02310
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925618"
---
1. Tot stand brengen op een externe bureaubladverbinding met de machine met de processerver. 
2. Voer cspsconfigtool.exe voor het starten van het configuratieprogramma van Azure Site Recovery voor de processerver.
    - Het hulpprogramma wordt automatisch gestart en de eerste keer dat u zich bij de processerver aanmeldt.
    - Als deze niet automatisch wordt geopend, klikt u op de snelkoppeling op het bureaublad.

3. In **configuratieserver FQDN-naam of IP-**, geef de naam of IP-adres van de configuratieserver waarmee de processerver te registreren.
4. In **configuratie-serverpoort**, Controleer of 443 is opgegeven. Dit is de poort waarop de configuratieserver naar aanvragen luistert.
5. In **wachtwoordzin voor verbinding met**, geeft u de wachtwoordzin die u hebt opgegeven bij het instellen van de configuratieserver. Zoek de wachtwoordzin:
    -  Op de configuratieserver, gaat u naar de installatiemap van Site Recovery **\home\svssystems\bin\**. 
    - Voer deze opdracht uit: **genpassphrase.exe.n**. Hiermee geeft u de locatie van de wachtwoordzin die u kunt vervolgens Houd er rekening mee.

6. In **Gegevensoverdrachtpoort**, laat de standaardwaarde staan, tenzij u een aangepaste poort hebt opgegeven.

7. Klik op **opslaan** Sla de instellingen op en registreer de processerver.

    
    ![Registreer de processerver](./media/site-recovery-vmware-register-process-server/register-ps.png)
