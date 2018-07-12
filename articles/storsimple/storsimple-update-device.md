---
title: Uw StorSimple-apparaat bijwerken | Microsoft Docs
description: Wordt uitgelegd hoe u de functie StorSimple update gebruiken voor het installeren van hotfixes en updates voor de normale en onderhoud.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/23/2018
ms.author: v-sharos
ms.openlocfilehash: 412978d2c343394f295e336690ec72153dda4b79
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452614"
---
# <a name="update-your-storsimple-8000-series-device"></a>Uw StorSimple 8000-serie-apparaat bijwerken
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. De StorSimple-apparaatbeheerfuncties worden automatisch volgens schema verplaatst naar de nieuwe Azure Portal. U ontvangt hierover een e-mailbericht en een portalmelding. Dit document wordt binnenkort ook ingetrokken. Zie [FAQ: Move to Azure portal](storsimple-8000-move-azure-portal-faq.md) (Veelgestelde vragen: verplaatsing naar Azure Portal) voor vragen met betrekking tot de verplaatsing.

## <a name="overview"></a>Overzicht
De StorSimple-updates-functies kunnen u eenvoudig uw StorSimple-apparaat om up-to-date te houden. Afhankelijk van het updatetype, kunt u updates toepassen op het apparaat via de klassieke Azure portal of via de Windows PowerShell-interface. Deze zelfstudie wordt de update-typen en hoe u het installeren van elk van beide beschreven.

U kunt twee soorten apparaatupdates toepassen: 

* Reguliere (of de normale modus) updates
* Updates voor de onderhoudsmodus

U kunt reguliere updates via de klassieke Azure-portal of de Windows PowerShell; installeren echter, moet u Windows PowerShell gebruiken voor het installeren van updates voor de onderhoudsmodus. 

Elk updatetype wordt afzonderlijk, hieronder beschreven.

### <a name="regular-updates"></a>Regelmatige updates
Regelmatige updates zijn niet-storende updates die kunnen worden geïnstalleerd wanneer het apparaat zich in de normale modus. Deze updates worden toegepast via de website Microsoft Update op elke apparaatcontroller. 

> [!IMPORTANT]
> Een failover van de controller kan zich voordoen tijdens het updateproces. Dit heeft echter geen gevolgen beschikbaarheid van het systeem of de bewerking.
> 
> 

* Zie voor meer informatie over het installeren van regelmatige updates via de klassieke Azure portal [regelmatige updates installeren via de klassieke Azure portal](#install-regular-updates-via-the-azure-classic-portal).
* U kunt ook regelmatige updates via Windows PowerShell voor StorSimple installeren. Zie voor meer informatie, [regelmatige updates installeren via Windows PowerShell voor StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Updates voor de onderhoudsmodus
Updates voor de onderhoudsmodus zijn updates waarvoor de computer, zoals upgrades van de schijf. Deze updates vereisen dat het apparaat in de onderhoudsmodus worden geplaatst. Zie voor meer informatie, [stap 2: Voer onderhoudsmodus](#step2). U kunt de klassieke Azure-portal niet gebruiken voor het installeren van updates voor de onderhoudsmodus. In plaats daarvan moet u Windows PowerShell voor StorSimple. 

Zie voor meer informatie over het installeren van updates voor de onderhoudsmodus [updates voor de onderhoudsmodus installeren via Windows PowerShell voor StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!IMPORTANT]
> Updates voor de onderhoudsmodus moeten afzonderlijk worden toegepast op elke domeincontroller. 
> 
> 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Regelmatige updates installeren via de klassieke Azure portal
Updates toepassen op uw StorSimple-apparaat kunt u de klassieke Azure portal.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Regelmatige updates installeren via Windows PowerShell voor StorSimple
U kunt ook Windows PowerShell voor StorSimple gebruiken normaal (normale modus)-updates toe te passen.

> [!IMPORTANT]
> Hoewel u regelmatig updates met behulp van Windows PowerShell voor StorSimple installeren kunt, wordt aangeraden dat u regelmatig updates via de klassieke Azure-portal installeert. Vanaf Update 1, worden controles van systeemtabel uitgevoerd voordat u updates installeert vanuit de portal. Deze controles van systeemtabel hebben voorrang op fouten en zorg ervoor dat een soepeler ervaring. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Installeren van updates voor de onderhoudsmodus via Windows PowerShell voor StorSimple
Windows PowerShell voor StorSimple kunt u updates voor de onderhoudsmodus toepassen op uw StorSimple-apparaat. Alle i/o-aanvragen zijn in deze modus onderbroken. Services, zoals niet-vluchtige RAM-geheugen (NVRAM) of de clustering-service worden ook gestopt. Beide controllers zijn opnieuw gestart wanneer u opgeven of deze modus sluit. Wanneer u deze modus afsluit, worden alle services wordt hervat en moeten in orde. (Dit kan enkele minuten duren.)

Als u toepassen van updates voor de onderhoudsmodus wilt, ontvangt u een waarschuwing via de klassieke Azure portal dat er updates die moeten worden geïnstalleerd. Deze waarschuwing bevat instructies voor het gebruik van Windows PowerShell voor StorSimple om de updates te installeren. Nadat u uw apparaat bijwerkt, gebruikt u dezelfde procedure om te wijzigen van het apparaat in de normale modus. Zie voor stapsgewijze instructies [stap 4: onderhoudsmodus afsluiten](#step4).

> [!IMPORTANT]
> * Controleer of beide apparaatcontrollers in orde zijn door te controleren voordat u de onderhoudsmodus invoert, de **hardwarestatus** op de **onderhoud** pagina in de klassieke Azure portal. Als de controller niet in orde is, contact op met Microsoft Support voor de volgende stappen. Ga voor meer informatie naar contact opnemen met Microsoft ondersteuning. 
> * Wanneer u in de onderhoudsmodus bevindt bent, moet u eerst de update van toepassing op een domeincontroller en klik vervolgens op de andere controller.
> 
> 

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Stap 1: Verbinding maken met de seriële console <a name="step1">
Gebruik een toepassing, zoals PuTTY eerst toegang tot de seriële console. De volgende procedure wordt uitgelegd hoe u PuTTY gebruiken om verbinding maken met de seriële console.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Stap 2: Voer de onderhoudsmodus <a name="step2">
Nadat u verbinding met de console maakt, moet u bepalen of er updates beschikbaar zijn voor installeren en voer de onderhoudsmodus voor het installeren van deze.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Stap 3: Uw updates installeren <a name="step3">
Vervolgens installeert u de updates.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Stap 4: Afsluiten in onderhoudsmodus <a name="step4">
Ten slotte de onderhoudsmodus afsluit.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Installeren van hotfixes via Windows PowerShell voor StorSimple
In tegenstelling tot de updates voor Microsoft Azure StorSimple, zijn hotfixes geïnstalleerd vanuit een gedeelde map. Net als bij updates, zijn er twee soorten hotfixes: 

* Reguliere hotfixes 
* Hotfixes in de onderhoudsmodus  

De volgende procedures wordt uitgelegd hoe u Windows PowerShell voor StorSimple gebruiken voor het installeren van normale en hotfixes in de onderhoudsmodus.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Wat gebeurt er met updates als u de fabrieksinstellingen van het apparaat uitvoeren?
Als een apparaat opnieuw wordt ingesteld op de fabrieksinstellingen, zijn alle updates gaan verloren. Nadat het apparaat teruggezet naar de fabrieksinstellingen is geregistreerd en geconfigureerd, moet u handmatig installeren van updates via de klassieke Azure-portal en/of de Windows PowerShell voor StorSimple. Zie voor meer informatie over de fabrieksinstellingen terug te zetten, [het apparaat terugzetten op fabrieksinstellingen](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Windows PowerShell voor StorSimple gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-windows-powershell-administration.md).
* Meer informatie over [met behulp van de StorSimple Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

