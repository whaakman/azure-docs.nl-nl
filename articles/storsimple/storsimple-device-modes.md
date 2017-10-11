---
title: De StorSimple-apparaat-modus wijzigen | Microsoft Docs
description: Beschrijft de modus voor de StorSimple-apparaat en wordt uitgelegd hoe u Windows PowerShell voor StorSimple om de apparatuurmodus te wijzigen.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>Wijzig de apparatuurmodus op uw StorSimple-apparaat
Dit artikel bevat een korte beschrijving van de verschillende modi waarin uw StorSimple-apparaat kan worden uitgevoerd. Uw StorSimple-apparaat kan worden gebruikt in de drie beschikbare modi: standaard, onderhoud en herstel. 

Na het lezen van dit artikel wordt u het volgende weten:

* Welke de StorSimple-apparaat-modi
* Hoe om te achterhalen welke modus de StorSimple-apparaat bevindt zich in
* Het wijzigen van de normale naar de onderhoudsmodus en *omgekeerd*

De bovenstaande beheertaken kunnen alleen worden uitgevoerd via de Windows PowerShell-interface van uw StorSimple-apparaat.

## <a name="about-storsimple-device-modes"></a>Over de modi van StorSimple-apparaat
Uw StorSimple-apparaat kan werken in de modus Normaal, onderhoud of herstellen. Elk van deze modi wordt kort hieronder beschreven.

### <a name="normal-mode"></a>Normale modus
Dit is gedefinieerd als de normale operationele modus voor volledig geconfigureerde StorSimple-apparaat. Standaard moet uw apparaat in de normale modus.

### <a name="maintenance-mode"></a>Onderhoudsmodus
Soms moet het StorSimple-apparaat kan worden geplaatst in de onderhoudsmodus. Deze modus kunt u onderhoud uitvoeren op het apparaat en verstoren updates, zoals die betrekking hebben op firmware van de schijf installeren.

U kunt het systeem plaatsen in de onderhoudsmodus alleen via de Windows PowerShell voor StorSimple. Alle i/o-aanvragen zijn in deze modus onderbroken. Services zoals niet-vluchtige RAM-geheugen (NVRAM) of de clusteringservice ook worden gestopt. Beide domeincontrollers worden opnieuw opgestart wanneer u in of uit deze modus. Wanneer u de onderhoudsmodus afsluit, worden alle services wordt hervat en moeten in orde. Dit kan enkele minuten duren.

> [!NOTE]
> **Onderhoudsmodus wordt alleen ondersteund op een apparaat naar behoren werkt. Dit wordt niet ondersteund op een apparaat waarop een of beide van de domeincontrollers niet functioneren.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Modus voor herstel
Herstelmodus kan als 'Veilige modus voor Windows met netwerkondersteuning' worden beschreven. Herstelmodus stelt het team van Microsoft Support en kan ze voor het uitvoeren van diagnostische gegevens op het systeem. Het voornaamste doel van de herstelmodus is voor het ophalen van het systeemlogboek in Logboeken.

Als uw systeem in de herstelmodus overgaat wordt, neemt u contact op met Microsoft Support voor volgende stappen. Ga voor meer informatie naar [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).

> [!NOTE]
> **U kunt het apparaat in herstelmodus plaatsen. Als het apparaat in orde is, probeert op te halen van het apparaat in een status waarin medewerkers van Microsoft Support Bekijk deze herstelmodus.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>Modus van StorSimple-apparaat bepalen
#### <a name="to-determine-the-current-device-mode"></a>Om te bepalen van de huidige apparatuurmodus
1. Meld u aan bij de seriële console van het apparaat door de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Bekijk het bannerbericht aangegeven in het menu van de seriële console van het apparaat. Dit bericht expliciet geeft aan of het apparaat in de modus voor onderhoud of herstel. Als het bericht geen specifieke gegevens met betrekking tot de systeem-modus bevat, is het apparaat in de normale modus.

## <a name="change-the-storsimple-device-mode"></a>De modus StorSimple-apparaat wijzigen
U kunt het StorSimple-apparaat plaatsen in de onderhoudsmodus (van de normale modus) voor het uitvoeren van onderhoud of onderhoud modus updates installeren. Voer de volgende procedures om in of uit de onderhoudsmodus.

> [!IMPORTANT]
> Voordat u de onderhoudsmodus invoert, of beide apparaatcontrollers in orde zijn met het openen van de **hardwarestatus** op de **onderhoud** pagina in de klassieke Azure portal. Als een of beide domeincontrollers niet in orde, neem dan contact op met Microsoft Support voor de volgende stappen. Ga voor meer informatie naar [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).
> 
> 

#### <a name="to-enter-maintenance-mode"></a>Onderhoudsmodus invoeren
1. Meld u aan bij de seriële console van het apparaat door de stappen in [PuTTY gebruiken om verbinding maken met de seriële console van het apparaat](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**. Geef desgevraagd de **wachtwoord apparaatbeheerder**. Is het standaardwachtwoord: `Password1`.
3. Typ het volgende achter de opdrachtprompt 
   
    `Enter-HcsMaintenanceMode`
4. Hier ziet u een waarschuwingsbericht weergegeven waarin staat dat onderhoudsmodus wordt verstoord alle i/o-aanvragen en de verbinding met de klassieke Azure portal-server en wordt u gevraagd om bevestiging. Type **Y** onderhoudsmodus invoeren.
5. Beide domeincontrollers wordt opnieuw opgestart. Wanneer het opnieuw opstarten voltooid is, verschijnt er een ander bericht waarmee wordt aangegeven dat het apparaat in de onderhoudsmodus.

#### <a name="to-exit-maintenance-mode"></a>Om af te sluiten van onderhoudsmodus
1. Meld u bij de seriële console van het apparaat. Controleer de van het bannerbericht aangegeven dat uw apparaat in de onderhoudsmodus.
2. Typ het volgende achter de opdrachtprompt:
   
    `Exit-HcsMaintenanceMode`
3. Een waarschuwing en een bevestigingsbericht wordt weergegeven. Type **Y** om af te sluiten van de onderhoudsmodus.
4. Beide domeincontrollers wordt opnieuw opgestart. Wanneer het opnieuw opstarten voltooid is, verschijnt er een ander bericht waarmee wordt aangegeven dat het apparaat in de normale modus.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [normaal en onderhoud modus updates toepassen](storsimple-update-device.md) op uw StorSimple-apparaat.

