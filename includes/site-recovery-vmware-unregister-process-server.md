---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 00b0c1b1a40ad16db177916c57dba6e9d5a187a7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925580"
---
Volg de stappen voor uw specifieke omstandigheden.

### <a name="unregister-a-connected-process-server"></a>Registratie van een verbonden processerver ongedaan maken

1. Een externe verbinding maken met de processerver als beheerder.
2. In de **Configuratiescherm**Open **programma's > een programma verwijderen**.
3. Het programma voor verwijderen **Microsoft Azure Site Recovery Mobility Service/Masterdoelserver**.
4. Het programma voor verwijderen **Microsoft Azure Site Recovery Configuration/processerver**.
5. Nadat de programma's in stap 3 en 4 zijn verwijderd, verwijdert u **Microsoft Azure Site Recovery Configuration/Processerverafhankelijkheden**.

### <a name="unregister-a-disconnected-process-server"></a>Registratie van een niet-verbonden processerver ongedaan maken

Volg deze stappen alleen als er geen manier om te terughalen van de machine waarop de processerver is geïnstalleerd.

1. Meld u op de configuratieserver als beheerder.
2. Open een beheeropdrachtprompt en blader naar `%ProgramData%\ASR\home\svsystems\bin`.
3. Voer deze opdracht uit om een lijst van een of meer processervers.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. Nee: het serienummer van het proces.
    - IP/naam: Het IP-adres en de naam van de machine waarop de processerver wordt uitgevoerd.
    - Heartbeat: Laatste heartbeat van de server-machine proces.
    ![De registratie ongedaan maken-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. Geef het serienummer van de processerver die u wilt de registratie ongedaan maken.
5. Registratie van een processerver alle van de details ervan te verwijderen uit het systeem en wordt het bericht weergegeven: **Registratie is opgeheven servernaam > (server-IP-adres)**

