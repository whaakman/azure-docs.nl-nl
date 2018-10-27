---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 5ba55e339db4c33d1b0d759e4682481e20318938
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164627"
---
### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Een push-installatie voorbereiden op een Linux-server

1. Zorg ervoor dat er een netwerkverbinding tussen de Linux-computer en de processerver is.
1. Maak een account dat op de processerver kan worden gebruikt voor toegang tot de computer. Het account moet een **rootgebruiker** zijn op de Linux-bronserver. Gebruik dit account alleen voor de push-installatie en voor updates.
1. Controleer of het bestand /etc/hosts op de Linux-bronserver vermeldingen bevat die de lokale hostnaam toewijzen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
1. Installeer de meest recente openssh, openssh-server en openssl-pakketten op de computer die u wilt repliceren.
1. Zorg ervoor dat SSH (Secure Shell) is ingeschakeld en wordt uitgevoerd op poort 22.
1. SFTP-subsysteem en wachtwoordverificatie verificatie in het bestand sshd_config inschakelen. Volg deze stappen:

    a. Meld u aan als **rootgebruiker**.

    b. In de **/etc/ssh/sshd_config** bestand, zoek de regel die met begint **PasswordAuthentication**.

    c. Verwijder opmerkingen bij de regel en wijzig de waarde in **Ja**.

    d. Zoek de regel die met begint **subsysteem**, en verwijder opmerkingen bij de regel.

      ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

    e. Start de service **sshd** opnieuw.

1. Voeg het account toe dat u hebt gemaakt in CSPSConfigtool. Volg deze stappen:

    a. Meld u aan bij de configuratieserver.

    b. Open **cspsconfigtool.exe**. Het is beschikbaar als snelkoppeling op het bureaublad en in de map %ProgramData%\home\svsystems\bin.

    c. Op de **Accounts beheren** tabblad **-Account toevoegen**.

    d. Voeg het account toe dat u hebt gemaakt.

    d. Voer de referenties in die u gebruikt wanneer u replicatie voor een computer inschakelt.
