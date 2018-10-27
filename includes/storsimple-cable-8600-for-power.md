---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8e6db54853efcba4d648c1d3bc793a9d1ce57441
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166155"
---
<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a>Op uw apparaat voor power bekabelen
> [!NOTE]
> Beide behuizingen op uw StorSimple-apparaat bevatten redundante PCMs. Voor elke behuizing, moeten de PCMs worden geïnstalleerd en die zijn verbonden met verschillende energiebronnen om hoge beschikbaarheid te garanderen.
> 
> 

1. Zorg ervoor dat de power-switches op alle PCMs in de positie OFF zijn.
2. Op de primaire behuizing verbinden met de stroomkabels beide PCMs. De stroomkabels worden aangeduid in het rood in het diagram van de power-bekabeling hieronder.
3. Zorg ervoor dat de twee PCMs op de primaire behuizing afzonderlijke energiebronnen.
4. Koppel de stroomkabels tot de macht op het rek distributie-eenheden zoals wordt weergegeven in het diagram bekabeling power.
5. Herhaal stappen 2 tot en met 4 voor de EBOD-behuizing.
6. De behuizing EBOD inschakelen door het spiegelen van de switch power op elke PCM op de positie aan.
7. Controleer of dat de behuizing EBOD is ingeschakeld door te controleren dat de groene LED's op de achterkant van het EBOD-controller zijn ingeschakeld.
8. Schakel op de primaire behuizing door elke PCM-switch op de positie van spiegelen.
9. Controleer of het systeem op door ervoor te zorgen dat de apparaatcontroller die LED 's hebben ingeschakeld.
10. Zorg ervoor dat de verbinding tussen de EBOD-controller en de apparaatcontroller actief is door te controleren dat de vier LED's naast de SAS-poort op de EBOD-controller groen zijn.
    
    > [!IMPORTANT]
    > Voor hoge beschikbaarheid voor uw systeem, wordt u aangeraden dat u alleen voldoen aan de kracht bekabeling schema in het volgende diagram wordt weergegeven.
    > 
    > 
    
    ![Uw apparaat 4U voor power bekabelen](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Energiebeheer-bekabeling**
    
    | Label | Beschrijving |
    |:--- |:--- |
    | 1 |Primaire behuizing |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Controller 0 |
    | 5 |Controller 1 |
    | 6 |EBOD-controller 0 |
    | 7 |EBOD-controller 1 |
    | 8 |EBOD behuizing |
    | 9 |PDU 's |

