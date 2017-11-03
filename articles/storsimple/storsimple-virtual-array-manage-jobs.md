---
title: Weergeven en beheren van virtuele StorSimple-matrix taken | Microsoft Docs
description: Beschrijft de pagina StorSimple Apparaatbeheer taken en het gebruik van het bijhouden van recente en de huidige taken voor het virtuele StorSimple-matrix.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: 3fd1c262a8ce94d8e98f2b066a8028d974b15b1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>De service Manager voor StorSimple-apparaat gebruiken om taken weergeven voor de virtuele StorSimple-matrix
## <a name="overview"></a>Overzicht
De **taken** blade biedt één centrale portal voor het weergeven en beheren van taken die zijn gestart op de virtuele matrices die zijn verbonden met uw StorSimple-apparaat Manager-service. U kunt actieve voltooide en mislukte taken voor meerdere virtuele apparaten weergeven. Resultaten zijn in tabelvorm gepresenteerd.

![Blade taken](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

U kunt de taken die u bent geïnteresseerd door te filteren op velden, zoals snel zoeken:

* **Tijdsbereik** – taken kunnen worden gefilterd op basis van het bereik van de datum en tijd.
* **Apparaten** – taken zijn gestart op een specifiek apparaat is verbonden met uw service. De gefilterde taken worden vervolgens in een tabel weergegeven op basis van de volgende kenmerken:
  
  * **Naam** : de taaknaam kan worden **alle**, **back-up**, **kloon**, **failover**, **updatesdownloaden**, of **updates installeren**.
  * **Status** – taken kunnen worden **alle**, **Bezig**, **geslaagd**, of **mislukt**, of **geannuleerd**.
  * **Entiteit** – de taken kunnen worden gekoppeld aan een volume, share of apparaat.
  * **Apparaat** : de naam van het apparaat waarop de taak is gestart.
  * **Gestart op** – de tijd waarop de taak is gestart.
  * **Duur** : de duur voor de taak is uitgevoerd.
* **Status** – u kunt zoeken naar alle, uitgevoerd, is voltooid of mislukte taken.
* **Taaktype** – het taaktype worden all, back-up, herstel, failover, downloaden van updates of updates installeren.

De lijst met taken wordt elke 30 seconden vernieuwd.

## <a name="view-job-details"></a>Taakdetails weergeven
De volgende stappen uit om de details van elke taak uitvoeren.

#### <a name="to-view-job-details"></a>Taakdetails weergeven
1. Op de **taken** blade weer van de taak of taken die u door het uitvoeren van een query met de juiste filters bent geïnteresseerd. U kunt zoeken naar is voltooid of actieve taken.
2. Selecteer een taak in de lijst in tabelvorm van taken.
   
    ![Blade taak](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. Klik onder aan de pagina op **Details**.
4. In de **Details** in het dialoogvenster vindt u de status, details en statistieken. De volgende afbeelding toont een voorbeeld van de **back-up taakdetails** in het dialoogvenster.
   
    ![Taakdetails](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Taakfouten wanneer de virtuele machine in de hypervisor is onderbroken
Wanneer een taak wordt uitgevoerd op uw virtuele StorSimple-matrix en het apparaat (virtuele machine is ingericht in de hypervisor) voor meer dan 15 minuten is onderbroken, de taak is mislukt. Dit is vanwege de tijd van uw virtuele StorSimple-matrix wordt niet gesynchroniseerd met de Microsoft Azure-tijd. 

Ziet u de volgende fout: "de tijd op het apparaat is niet gesynchroniseerd met de Microsoft Azure-tijd door meer dan 15 minuten. Zorg ervoor dat de hypervisor en de servertijden zijn gesynchroniseerd met een clientverzoeken NTP-apparaat. Controleer of er geen verbindingsproblemen zijn. Voor het oplossen van problemen met de netwerkverbinding diagnostische tests uitvoeren vanaf de lokale webgebruikersinterface van uw virtuele apparaat."

Deze fouten van toepassing op back-up, herstel, bijwerken en failover-taken. Als uw virtuele machine is ingericht in Hyper-V, synchroniseert de machine tijd uiteindelijk met de hypervisor. Wanneer dit gebeurt, kunt u de taak opnieuw te starten.

## <a name="next-steps"></a>Volgende stappen
[Informatie over het gebruik van de lokale webgebruikersinterface voor het beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

