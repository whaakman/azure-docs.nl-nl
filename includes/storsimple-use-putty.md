---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 50a5c8d515e27db7c2c65b484cdecad8ff00baf8
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166023"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-connect-through-the-serial-console"></a>Verbinding maken via de seriële console
1. Sluit de seriële kabel aan op het apparaat (rechtstreeks of via een USB-seriële adapter).
2. Open het **Configuratiescherm** en open vervolgens **Apparaatbeheer**.
3. Kijk waar de COM-poort zich bevindt, zoals weergegeven in de volgende afbeelding.
   
     ![Verbinding maken via een seriële console](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Start PuTTY. 
5. Wijzig in het rechterdeelvenster **Verbindingstype** in **Serieel**.
6. Typ in het rechterdeelvenster de juiste COM-poort. Zorg ervoor dat de seriële configuratieparameters als volgt zijn ingesteld:
   
   * Snelheid: 115.200
   * Gegevensbits: 8
   * Stopbits: 1
   * Pariteit: Geen
   * Datatransportbesturing: Geen
     
     Deze instellingen worden weergegeven in de volgende afbeelding.
     
     ![Instellingen voor PuTTY](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Als de standaardinstelling voor datatransportbesturing niet werkt, kunt u de datatransportbesturing instellen op XON/XOFF.
     > 
     > 
7. Klik op **Openen** om een seriële sessie te starten.

