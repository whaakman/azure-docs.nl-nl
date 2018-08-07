---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 08/06/2018
ms.author: ramamill
ms.openlocfilehash: 81390d38b4c0c38b7ac6883ae2bf18c64542fa00
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582870"
---
De stappen om de registratie van een processerver ongedaan te maken, verschillen afhankelijk van de status van de verbinding met de configuratieserver.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>De registratie ongedaan maken van een processerver die is verbonden

1. Meld u extern bij de processerver aan als beheerder.
2. Start het **Configuratiescherm** en open **Programma's > Een programma verwijderen**
3. Verwijder het programma met de naam **Microsoft Azure Site Recovery-configuratie-/processerver**
4. Nadat stap 3 is voltooid, verwijdert u **Microsoft Azure Site Recovery-configuratie-/processerverafhankelijkheden**

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>De registratie ongedaan maken van een processerver die niet is verbonden

> [!WARNING]
> Volg onderstaande stappen als het niet lukt om de virtuele machine te reactiveren waarop de processerver is geïnstalleerd.

1. Meld u bij uw configuratieserver aan als beheerder.
2. Open een beheeropdrachtprompt en blader naar de map `%ProgramData%\ASR\home\svsystems\bin`.
3. Voer nu de opdracht uit.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. De bovenstaande opdracht geeft de lijst van de processerver (s) (kan worden meer dan één, in het geval van dubbele vermeldingen) seriële number(S.No), IP-adres (IP), naam van de virtuele machine op welke processerver is geïmplementeerd (naam), hartslag van de virtuele machine (Heartbeat) zoals hieronder wordt weergegeven.
    ![De registratie ongedaan maken-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Voer nu het serienummer van de processerver die u wilt voor het voor het opheffen van de registratie.
6. Hiermee worden de details van de processerver verwijderd uit het systeem en wordt het bericht weergegeven: **registratie is opgeheven servernaam > (server-IP-adres)**

