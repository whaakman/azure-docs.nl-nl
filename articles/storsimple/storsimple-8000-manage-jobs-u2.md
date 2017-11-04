---
title: Weergeven en beheren van taken voor StorSimple 8000-serie | Microsoft Docs
description: Beschrijving van de blade StorSimple Apparaatbeheer service taken en het gebruik van het bijhouden van recente, huidige en geplande back-uptaken.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: bf8038b7171053b75eeb9aed88bff4246e65a8a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-jobs-update-3-and-later"></a>De service Manager van StorSimple-apparaat gebruiken voor het weergeven en beheren van taken (Update 3 en hoger)

## <a name="overview"></a>Overzicht
De **taken** blade biedt één centrale portal voor het weergeven en beheren van taken die zijn gestart op apparaten die zijn verbonden met uw StorSimple-apparaat Manager-service. U kunt geplande, actieve, voltooide, geannuleerde en mislukte taken voor meerdere apparaten weergeven. Resultaten zijn in tabelvorm gepresenteerd.

![Blade taken](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

U kunt de taken die u bent geïnteresseerd door te filteren op velden, zoals snel zoeken:

* **Status** – taken kunnen worden uitgevoerd, is voltooid, geannuleerd, is mislukt, annuleren of voltooid met fouten.
* **Tijdsbereik** – taken kunnen worden gefilterd op basis van het bereik van de datum en tijd. Het bereik is 1 uur, afgelopen 24 uur, afgelopen 7 dagen, na 30 dagen, afgelopen jaar of aangepaste datum uit het verleden.
* **Type** – het taaktype mag geplande back-up, handmatige back-up, herstel back-up, volume klonen, failover volumecontainers, lokaal vastgemaakt volume maakt, volume wijzigen, updates installeren, verzamelen van Ondersteuningslogboeken en cloud toestel maken.
* **Apparaten** – taken zijn gestart op een bepaald apparaat verbonden met uw service.
  
De gefilterde taken worden vervolgens in een tabel weergegeven op basis van de volgende kenmerken:
  
* **Naam** – geplande back-up, handmatige back-up en terugzetten van back-up, kloon volume, failover volumecontainers, lokaal vastgemaakt volume maakt, volume wijzigen, updates installeren, verzamelen van Ondersteuningslogboeken of cloud toestel maken.
* **Status** – uitgevoerd, voltooide, geannuleerde, is mislukt, geannuleerd of voltooid met fouten.
* **Entiteit** – de taken kunnen worden gekoppeld aan een volume, een back-upbeleid of een apparaat. De taak van een kloon is bijvoorbeeld gekoppeld aan een volume dat een geplande back-uptaak gekoppeld aan een back-upbeleid is. Een apparaat-taak is gemaakt als gevolg van een noodherstel (DR) of een herstelbewerking.
* **Apparaat** : de naam van het apparaat waarop de taak is gestart.
* **Gestart op** – de tijd waarop de taak is gestart.
* **Duur** – de tijd die nodig zijn om de taak te voltooien.

De lijst met taken wordt elke 30 seconden vernieuwd.

U kunt de volgende taak-gerelateerde acties uitvoeren op deze pagina:

* Taakdetails weergeven
* Een taak annuleren

## <a name="view-job-details"></a>Taakdetails weergeven
De volgende stappen uit om de details van elke taak uitvoeren.

#### <a name="to-view-job-details"></a>Taakdetails weergeven
1. Ga naar uw StorSimple-apparaat Manager-service en klik vervolgens op **taken**.

2. In de **taken** blade weer van de taak of taken die u door het uitvoeren van een query met de juiste filters bent geïnteresseerd. U kunt zoeken voor voltooid, uitvoeren of taken geannuleerd.

    ![Blade taak](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Selecteer en klik op een taak.

    ![Blade taak](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. U kunt de status, details, tijdstatistieken en statistieken van de gegevens weergeven in de blade taak details.
   
    ![Taakdetails](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Een taak annuleren
De volgende stappen uit om te annuleren een actieve taak uitvoeren.

> [!NOTE]
> Sommige taken, zoals een volume als u wilt wijzigen van het volumetype te wijzigen of het uitbreiden van een volume kunnen niet worden geannuleerd.


### <a name="to-cancel-a-job"></a>Een taak annuleren
1. Op de **taken** pagina, wordt de actieve taak of taken die u Annuleren wilt door het uitvoeren van een query met de juiste filters weergegeven. Selecteer de taak.

2. Met de rechtermuisknop op de geselecteerde taak voor het aanroepen van het contextmenu en klikt u op **annuleren**.

    ![Taakdetails](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Klik op **Ja** als u om bevestiging wordt gevraagd. Deze taak is nu geannuleerd.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [beheren van uw StorSimple-back-upbeleid](storsimple-8000-manage-backup-policies-u2.md).
* Meer informatie over hoe [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

