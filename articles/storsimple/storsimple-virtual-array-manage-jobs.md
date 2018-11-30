---
title: StorSimple Virtual Array taken weergeven en beheren | Microsoft Docs
description: Hierin wordt beschreven in de StorSimple Device Manager-servicepagina taken en deze gebruiken voor het bijhouden van recente en huidige taken voor de StorSimple Virtual Array.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: dbab2aaab2c12bef07748f54e5864d042f1c982a
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333561"
---
# <a name="use-the-storsimple-device-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Gebruik van de service StorSimple Device Manager-taken weergeven voor de StorSimple Virtual Array
## <a name="overview"></a>Overzicht
De **taken** blade biedt één centrale portal voor het weergeven en beheren van taken die worden gestart op de virtuele matrices die zijn verbonden met uw StorSimple Device Manager-service. U kunt actieve, voltooide en mislukte taken voor meerdere virtuele apparaten weergeven. Resultaten worden weergegeven in tabelvorm.

![Blade taken](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

U vindt snel de taken die u geïnteresseerd bent door te filteren op velden, zoals:

* **Tijdsbereik** -taken kunnen worden gefilterd op basis van het bereik van de datum en tijd.
* **Apparaten** – taken worden gestart op een specifiek apparaat dat is verbonden met uw service. De gefilterde taken worden vervolgens de tabelindeling op basis van de volgende kenmerken:
  
  * **Naam** – naam van de taak kan worden **alle**, **back-up**, **kloon**, **failover**, **updatesdownloaden**, of **updates installeren**.
  * **Status** -taken kunnen worden **alle**, **Bezig**, **geslaagd**, of **mislukt**, of **geannuleerd**.
  * **Entiteit** – de taken kunnen worden gekoppeld aan een volume, share of een apparaat.
  * **Apparaat** : de naam van het apparaat waarop de taak is gestart.
  * **Aan de slag op** : de tijd waarop de taak is gestart.
  * **Duur** : de duur voor op de taak werd uitgevoerd.
* **Status** – u kunt zoeken naar alle actieve, voltooide en mislukte taken.
* **Type taak** – het taaktype worden alle, back-up, herstel, failover, downloaden van updates of updates installeren.

De lijst met taken wordt elke 30 seconden vernieuwd.

## <a name="view-job-details"></a>Taakdetails weergeven
Voer de volgende stappen uit om de details van elke taak weer te geven.

#### <a name="to-view-job-details"></a>Taakdetails weergeven
1. Op de **taken** blade u geïnteresseerd bent door een query uitvoeren met de juiste filters of meer taken worden weergegeven. U kunt zoeken naar de ingevulde of actieve taken.
2. Selecteer een taak in de lijst in tabelvorm van taken.
   
    ![Taakblade](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. Aan de onderkant van de pagina, klikt u op **Details**.
4. In de **Details** in het dialoogvenster kunt u de status, details en statistieken bekijken. De volgende afbeelding toont een voorbeeld van de **back-up taakdetails** in het dialoogvenster.
   
    ![Taakgegevens](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Mislukte wanneer de virtuele machine in de hypervisor wordt onderbroken
Wanneer een taak wordt uitgevoerd op uw StorSimple Virtual Array en het apparaat (virtuele machine is ingericht in een hypervisor) voor meer dan 15 minuten wordt onderbroken, wordt de taak is mislukt. Dit is vanwege de tijd van de StorSimple Virtual Array wordt niet gesynchroniseerd met de Microsoft Azure-tijd. 

Ziet u de volgende fout: "de tijd op het apparaat is gesynchroniseerd met de Microsoft Azure-tijd door meer dan 15 minuten. Zorg ervoor dat de hypervisor en het apparaat servertijden zijn gesynchroniseerd met een NTP-server. Controleer of er zijn geen problemen met de netwerkverbinding. Voor het oplossen van problemen met de netwerkverbinding, Voer diagnostische tests uit via de lokale webgebruikersinterface van uw virtueel apparaat."

Deze fouten van toepassing op back-up, herstel, bijwerken en failover-taken. Als uw virtuele machine in Hyper-V is ingericht, wordt de machine uiteindelijk tijd synchroniseert met de hypervisor. Wanneer dit gebeurt, kunt u de taak opnieuw starten.

## <a name="next-steps"></a>Volgende stappen
[Informatie over het gebruik van de lokale webgebruikersinterface voor het beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

