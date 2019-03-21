---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: 0d090f43b69b42a07f1c8949d1662e8e720f3cf4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57908532"
---
De stappen om de registratie van een processerver ongedaan te maken, verschillen afhankelijk van de status van de verbinding met de configuratieserver.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>De registratie ongedaan maken van een processerver die is verbonden

1. Meld u extern bij de processerver aan als beheerder.
2. Start de **Configuratiescherm** en open **programma's > een programma verwijderen**.
3. Verwijderen van een programma met de naam **Microsoft Azure Site Recovery Mobility Service/Masterdoelserver**.
4. Verwijderen van een programma met de naam **Microsoft Azure Site Recovery Configuration/processerver**.
5. Wanneer de programma's in stap 3 en 4 zijn verwijderd, kunt u verwijderen **Microsoft Azure Site Recovery Configuration/Processerverafhankelijkheden**.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>De registratie ongedaan maken van een processerver die niet is verbonden

> [!WARNING]
> Gebruik de onderstaande stappen als er geen manier om te terughalen van de virtuele machine waarop de processerver is geïnstalleerd.

1. Meld u bij uw configuratieserver aan als beheerder.
2. Open een beheeropdrachtprompt en blader naar de map `%ProgramData%\ASR\home\svsystems\bin`.
3. Voer nu de opdracht uit.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. De bovenstaande opdracht geeft de lijst van de processerver (s) (kan worden meer dan één, in het geval van dubbele vermeldingen) seriële number(S.No), IP-adres (IP), naam van de virtuele machine op welke processerver is geïmplementeerd (naam), hartslag van de virtuele machine (Heartbeat) zoals hieronder wordt weergegeven.
    ![De registratie ongedaan maken-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Voer nu het serienummer van de processerver die u wilt voor het voor het opheffen van de registratie.
6. Hiermee worden de details van de processerver verwijderd uit het systeem en wordt het bericht weergegeven: **Registratie is opgeheven servernaam > (server-IP-adres)**

