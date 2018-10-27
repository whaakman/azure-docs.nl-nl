---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 792589f4aa2a80c05378224ffe4e4d1dad2f935c
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166426"
---
<!--author=alkohli last changed:02/22/16-->

#### <a name="to-attach-the-sas-cables"></a>De SAS-kabels koppelen
1. Identificeer de primaire en de EBOD-behuizingen. De twee bijlagen kunnen worden geïdentificeerd door hun respectieve back vlakken kijken. Zie de volgende afbeelding voor hulp. 
   
    ![Back-ups maken vlak van primaire en EBOD-behuizingen](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Back-weergave van de primaire en EBOD-behuizingen**
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Primaire behuizing |
   | 2 |EBOD behuizing |
2. Ga naar de seriële nummers waaraan op de primaire en de EBOD-behuizingen. De sticker serienummer is aangebracht in het vorige oor van elke behuizing. De seriële nummers waaraan moet identiek zijn op beide behuizingen. [Neem contact op met Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) onmiddellijk als de seriële nummers waaraan niet overeenkomen. Zie de volgende afbeelding om te vinden van de seriële nummers waaraan.
   
    ![Weergave van de achterzijde van behuizing met van serienummer](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Locatie van het serienummer sticker**
   
   | Label | Beschrijving |
   |:--- |:--- |
   | 1 |Wissen van de behuizing |
3. Gebruik de opgegeven SAS-kabels verbinden met de behuizing EBOD de primaire behuizing als volgt:
   
   1. Identificeer de vier SAS-poorten op de primaire behuizing en de EBOD-behuizing. De SAS-poorten zijn gelabeld als EBOD op de primaire behuizing en komen overeen met een poort op de behuizing EBOD, zoals wordt weergegeven in de SAS-bekabeling afbeelding hieronder.
   2. De opgegeven SAS-kabels gebruiken voor verbinding van de poort naar poort EBOD A.
   3. De EBOD-poort op controller 0 moet zijn verbonden met de poort A op EBOD-controller 0. De EBOD-poort op controller 1 moet worden verbonden met de poort A op EBOD-controller 1. Zie de volgende afbeelding voor hulp. 
      
      ![SAS-bekabeling voor uw apparaat](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS-bekabeling**
      
      | Label | Beschrijving |
      |:--- |:--- |
      | A |Primaire behuizing |
      | B |EBOD behuizing |
      | 1 |Controller 0 |
      | 2 |Controller 1 |
      | 3 |EBOD-Controller 0 |
      | 4 |EBOD-Controller 1 |
      | 5, 6 |SAS-poorten op de primaire behuizing (EBOD gelabelde) |
      | 7, 8 |SAS-poorten op EBOD behuizing (poort A) |

