---
title: Uw StorSimple-apparaat bijwerken | Microsoft Docs
description: Wordt uitgelegd hoe u met de functie StorSimple update regular en onderhoud modus updates en hotfixes installeren.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 8490110942741b049b6d44ac93697303cef40e8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="update-your-storsimple-8000-series-device"></a>Uw StorSimple 8000-serie-apparaat bijwerken
## <a name="overview"></a>Overzicht
De StorSimple-updates-functies kunt u eenvoudig uw StorSimple-apparaat om up-to-date te houden. Afhankelijk van het updatetype kunt u updates toepassen op het apparaat via de klassieke Azure portal of via de Windows PowerShell-interface. Deze zelfstudie wordt de update-typen en het installeren van elk van deze beschreven.

U kunt twee soorten apparaatupdates toepassen: 

* Reguliere (of de normale modus) updates
* Onderhoud modus updates

U kunt ook regelmatige updates via de klassieke Azure-portal of de Windows PowerShell; installeren u moet Windows PowerShell echter gebruiken om onderhoud modus updates te installeren. 

Elk updatetype wordt afzonderlijk hieronder beschreven.

### <a name="regular-updates"></a>Regelmatig updates
Regelmatig updates zijn ononderbroken updates die kunnen worden geïnstalleerd wanneer het apparaat zich in de normale modus. Deze updates worden toegepast via de website Microsoft Update op elke domeincontroller apparaat. 

> [!IMPORTANT]
> Een failover van de domeincontroller kan optreden tijdens het updateproces. Dit wordt echter niet gewijzigd beschikbaarheid van het systeem of de bewerking.
> 
> 

* Zie voor meer informatie over het installeren van regelmatige updates via de klassieke Azure portal [regelmatig updates via de klassieke Azure-portal installeren](#install-regular-updates-via-the-azure-classic-portal).
* U kunt ook regelmatige updates via Windows PowerShell voor StorSimple installeren. Zie voor meer informatie [regelmatig updates via Windows PowerShell voor StorSimple installeren](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Onderhoud modus updates
Onderhoud modus updates zijn verstoren updates zoals upgrades van de schijf. Deze updates vereisen dat het apparaat in de onderhoudsmodus moet worden geplaatst. Zie voor meer informatie [stap 2: Voer onderhoudsmodus](#step2). U kunt de klassieke Azure portal niet gebruiken om onderhoud modus updates te installeren. In plaats daarvan moet u Windows PowerShell voor StorSimple. 

Zie voor meer informatie over het installeren van updates voor onderhoud modus [installeren onderhoud modus updates via Windows PowerShell voor StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!IMPORTANT]
> Onderhoud modus updates moeten afzonderlijk worden toegepast op elke domeincontroller. 
> 
> 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Regelmatig updates via de klassieke Azure-portal installeren
Updates toepassen op uw StorSimple-apparaat kunt u de klassieke Azure portal.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Installeer regelmatig updates via Windows PowerShell voor StorSimple
U kunt ook Windows PowerShell voor StorSimple regular (normale modus)-updates toe te passen.

> [!IMPORTANT]
> Hoewel u regelmatig updates met Windows PowerShell voor StorSimple installeren kunt, wordt aangeraden dat u regelmatig updates via de klassieke Azure portal installeert. Vanaf Update 1, worden eerste controles uitgevoerd voordat u updates installeert vanuit de portal. Deze controles vooraf wordt hebben voorrang op fouten en zorg ervoor dat een soepeler ervaring. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Onderhoud modus updates via Windows PowerShell voor StorSimple installeren
U kunt Windows PowerShell voor StorSimple gebruiken onderhoud modus updates toepassen op uw StorSimple-apparaat. Alle i/o-aanvragen zijn in deze modus onderbroken. Services zoals niet-vluchtige RAM-geheugen (NVRAM) of de clusteringservice ook worden gestopt. Beide domeincontrollers worden opgestart wanneer u in of uit deze modus. Wanneer u deze modus afsluit, worden alle services wordt hervat en moeten in orde. (Dit kan enkele minuten duren.)

Als u onderhoud modus updates toe te passen wilt, ontvangt u een waarschuwing via de klassieke Azure portal dat er updates die moeten worden geïnstalleerd. Deze waarschuwing bevat instructies voor het gebruik van Windows PowerShell voor StorSimple om de updates te installeren. Nadat u uw apparaat bijwerkt, gebruikt u dezelfde procedure om te wijzigen van het apparaat naar de normale modus. Zie voor stapsgewijze instructies [stap 4: afsluiten onderhoudsmodus](#step4).

> [!IMPORTANT]
> * Controleer of beide apparaatcontrollers zijn in orde door te controleren voordat u de onderhoudsmodus invoert, de **hardwarestatus** op de **onderhoud** pagina in de klassieke Azure portal. Als de domeincontroller niet in orde is, moet u contact op met Microsoft Support voor de volgende stappen. Ga voor meer informatie naar contact opnemen met Microsoft ondersteuning. 
> * Wanneer u in de onderhoudsmodus bevindt bent, moet u eerst de update toepassen op een domeincontroller en klik vervolgens op de andere controller.
> 
> 

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Stap 1: Verbinding maken met de seriële console<a name="step1">
Eerst een toepassing zoals PuTTY gebruiken voor toegang tot de seriële console. De volgende procedure wordt uitgelegd hoe u met PuTTY verbinding maken met de seriële console.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Stap 2: Geef de onderhoudsmodus<a name="step2">
Nadat u verbinding met de console, moet u bepalen of er updates voor het installeren en voer de onderhoudsmodus voor de installatie van deze zijn.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Stap 3: Uw updates installeren<a name="step3">
Vervolgens installeert u de updates.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Stap 4: Afsluiten onderhoudsmodus<a name="step4">
Onderhoudsmodus ten slotte af te sluiten.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Installeren van hotfixes via Windows PowerShell voor StorSimple
In tegenstelling tot de updates voor Microsoft Azure StorSimple zijn-hotfixes geïnstalleerd vanuit een gedeelde map. Net als bij updates, zijn er twee soorten hotfixes: 

* Reguliere hotfixes 
* Onderhoud modus hotfixes  

De volgende procedures wordt uitgelegd hoe u Windows PowerShell voor StorSimple gebruikt regular en onderhoud modus hotfixes installeren.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Wat gebeurt er met updates als u de fabrieksinstellingen van het apparaat uitvoeren?
Als een apparaat is standaardinstellingen herstellen, zijn alle updates gaan verloren. Nadat het apparaat de fabrieksinstellingen is geregistreerd en geconfigureerd, moet u handmatig installeren van updates via de klassieke Azure-portal en/of de Windows PowerShell voor StorSimple. Zie voor meer informatie over de fabrieksinstellingen [opnieuw instellen van het apparaat fabrieksinstellingen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Windows PowerShell voor StorSimple gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-windows-powershell-administration.md).
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

