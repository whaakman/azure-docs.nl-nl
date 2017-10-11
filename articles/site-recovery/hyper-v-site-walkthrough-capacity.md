---
title: Plannen van capaciteit en de schaalbaarheid van de machine van Hyper-V-replicatie (zonder VMM) naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel voor plan capaciteit en de schaal gebruiken als Hyper-V-machines repliceren naar Azure met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: c7891c188c2cecbbf056fa79672a13bb16fa7fcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-to-azure-replication"></a>Stap 3: Plannen van capaciteit en schalen voor Hyper-V naar Azure replicatie

In dit artikel gebruiken om te achterhalen plannen van capaciteit en de schaling bij het lokale Hyper-V-machines (zonder de System Center VMM) repliceren naar Azure met [Azure Site Recovery](site-recovery-overview.md).

Na het lezen van dit artikel, eventuele opmerkingen posten onder of technische vragen op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="how-do-i-start-capacity-planning"></a>Hoe start capaciteitsplanning?


U informatie verzamelen over uw replicatieomgeving, en vervolgens plannen van capaciteit met behulp van deze informatie, samen met de overwegingen gemarkeerd in dit artikel.


## <a name="gather-information"></a>Informatie verzamelen

1. Informatie verzamelen over uw replicatieomgeving, waaronder informatie over de virtuele machines, het aantal schijven per virtuele machine en de opslag per schijf.
2. De snelheid van uw dagelijkse wijzigen (verloop) voor gerepliceerde gegevens identificeren. Download de [Hyper-V-hulpprogramma voor capaciteitsplanning](https://www.microsoft.com/download/details.aspx?id=39057) de wijzigingssnelheid ophalen. U wordt aangeraden dat u dit hulpprogramma uitvoeren via een week om vast te leggen gemiddelden.
 

## <a name="figure-out-capacity"></a>Capaciteit te achterhalen

Op basis van de informatie verzamelen die u hebt, voer de [Site Recovery-Capaciteitsplanner](http://aka.ms/asr-capacity-planner-excel) schatten bandbreedte behoeften en serverbronnen voor de bronlocatie en de resources (virtuele machines en opslag, enzovoort), die u nodig hebt in de doellocatie voor het analyseren van uw bronomgeving en werkbelastingen. U kunt het hulpprogramma uitvoeren in een aantal modi:

- Snelle planning: Voer het hulpprogramma in deze modus netwerk- en projecties ophalen op basis van een gemiddelde aantal virtuele machines, schijven, opslag en wijzigingssnelheid.
- Detail te plannen: het hulpprogramma uitvoeren in deze modus en Geef details op van elke werkbelasting op het niveau van de virtuele machine. VM-compatibiliteit analyseren en netwerk- en projecties ophalen.

Nu u het hulpprogramma uitvoert:

1. Download de [hulpprogramma](http://aka.ms/asr-capacity-planner-excel)
2. Ga als volgt de snelle planner voeren, [deze instructies](site-recovery-capacity-planner.md#run-the-quick-planner), en selecteert u het scenario **Hyper-V naar Azure**.
3. Ga als volgt de gedetailleerde planner voeren, [deze instructies](site-recovery-capacity-planner.md#run-the-detailed-planner), en selecteert u het scenario **Hyper-V naar Azure**.

## <a name="control-network-bandwidth"></a>Netwerkbandbreedte

Nadat u de bandbreedte die u nodig hebt berekend, hebt u een aantal opties voor het beheren van de hoeveelheid bandbreedte die wordt gebruikt voor replicatie:

* **Bandbreedte beperken**: Hyper-V-verkeer dat wordt gerepliceerd naar Azure verloopt via een specifieke Hyper-V-host. U kunt de bandbreedte op de hostserver beperken.
* **Netwerkbandbreedte beïnvloeden**: U kunt de bandbreedte die wordt gebruikt voor replicatie via diverse registersleutels beïnvloeden.

### <a name="throttle-bandwidth"></a>Bandbreedte beperken
1. Open de Microsoft Azure Backup MMC-module op de Hyper-V-hostserver. Standaard is er een snelkoppeling naar Microsoft Azure Backup beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik in de module op **Eigenschappen wijzigen**.
3. Selecteer op het tabblad **Beperken** de optie **Internetbandbreedtebeperking inschakelen voor back-upbewerkingen** en stel de limieten in voor werktijden en voor tijden waarop niet wordt gewerkt. Het geldige bereik ligt tussen 512 Kbps en 102 Mbps.

    ![Bandbreedte beperken](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) gebruiken om een beperking in te stellen. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** geeft aan dat er geen beperking is vereist.

### <a name="influence-network-bandwidth"></a>Netwerkbandbreedte beïnvloeden
1. Navigeer in het register naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Wijzig de waarde ter bepaling van de bandbreedte-verkeer op de replicerende schijf, de **UploadThreadsPerVM**, of maak de sleutel als deze niet bestaat.
   * Wijzig de waarde ter bepaling van de bandbreedte voor failbackverkeer vanuit Azure **DownloadThreadsPerVM**.
2. De standaardwaarde is 4. In netwerken met overprovisioning moeten deze registersleutels zodanig worden gewijzigd dat niet de standaardwaarden worden gebruikt. Het maximum is 32. Houd het verkeer in de gaten om de waarde te optimaliseren.

## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 4: Plan netwerken](hyper-v-site-walkthrough-network.md).
