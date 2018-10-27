---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: e18d0a6a01a86f844edc213fc95003cf4f4b46c9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165367"
---
* Maak verbinding met de virtuele machine van de processerver via Verbinding met extern bureaublad.
* U kunt cspsconfigtool.exe openen door te klikken op de snelkoppeling op het bureaublad. (Het hulpprogramma wordt automatisch gestart als dit de eerste keer dat u zich bij de processerver).
  - De volledig gekwalificeerde naam (FQDN) of het IP-adres van de configuratieserver
  - De poort waarop de configuratieserver luistert. De waarde hiervoor moet 443 zijn
  - De wachtwoordzin om verbinding te maken met de configuratieserver.
  - De gegevensoverdrachtpoort die voor deze processerver moet worden geconfigureerd. Wijzig de standaardwaarde niet tenzij u deze in uw omgeving in een ander poortnummer hebt gewijzigd.

    ![Processerver registreren](./media/site-recovery-vmware-register-process-server/register-ps.png)
* Klik op Opslaan om de configuratie op te slaan en de processerver te registreren.
