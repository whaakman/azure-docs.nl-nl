---
title: Weergeven en beheren van taken voor StorSimple 8000-serie | Microsoft Docs
description: Hierin wordt beschreven in de blade taken van StorSimple Device Manager-service en deze gebruiken voor het bijhouden van recente, huidige en geplande back-uptaken.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 462f8dafdffa7ee01e6ccf7945a1abfdff90db42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720688"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-jobs-update-3-and-later"></a>De StorSimple Device Manager-service gebruiken voor het weergeven en beheren van taken (Update 3 en hoger)

## <a name="overview"></a>Overzicht
De **taken** blade biedt één centrale portal voor het weergeven en beheren van taken die zijn gestart op apparaten die zijn verbonden met uw StorSimple Device Manager-service. U kunt geplande, actieve, voltooid, geannuleerd en mislukte taken voor meerdere apparaten weergeven. Resultaten worden weergegeven in tabelvorm.

![Blade taken](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

U vindt snel de taken die u geïnteresseerd bent door te filteren op velden, zoals:

* **Status** -taken kunnen worden uitgevoerd, is voltooid, geannuleerd, is mislukt, annuleren of voltooid met fouten.
* **Tijdsbereik** -taken kunnen worden gefilterd op basis van het bereik van de datum en tijd. De bereiken ligt die zijn afgelopen uur, afgelopen 24 uur, afgelopen 7 dagen, afgelopen 30 dagen, afgelopen jaar of aangepaste datum.
* **Type** : het taaktype kan worden geplande back-up, handmatige back-up, herstel back-up, volume klonen, failover volumecontainers, lokaal vastgemaakt volume maken, wijzigen van een volume, updates installeren, voor Ondersteuningslogboeken te verzamelen en cloudapparaat maken.
* **Apparaten** – taken worden gestart op een bepaald apparaat verbonden met uw service.
  
De gefilterde taken worden vervolgens de tabelindeling op basis van de volgende kenmerken:
  
* **Naam** : geplande back-up, handmatige back-up en herstel back-up, kloon volume, failover volumecontainers, lokaal vastgemaakt volume maken, wijzigen van een volume, updates installeren, voor Ondersteuningslogboeken te verzamelen of cloudapparaat maken.
* **Status** : wordt uitgevoerd, is voltooid, geannuleerd, is mislukt, annuleren of voltooid met fouten.
* **Entiteit** – de taken kunnen worden gekoppeld aan een volume, een back-upbeleid of een apparaat. Bijvoorbeeld, is een kloontaak gekoppeld aan een volume dat een geplande back-uptaak gekoppeld aan een back-upbeleid is. Er wordt een apparaattaak gemaakt als gevolg van een herstel na noodgevallen (DR) of een herstelbewerking uit.
* **Apparaat** : de naam van het apparaat waarop de taak is gestart.
* **Aan de slag op** : de tijd waarop de taak is gestart.
* **Duur** – de tijd die nodig is om de taak te voltooien.

De lijst met taken wordt elke 30 seconden vernieuwd.

U kunt de volgende taakgerelateerde acties uitvoeren op deze pagina:

* Taakdetails weergeven
* Een taak annuleren

## <a name="view-job-details"></a>Taakdetails weergeven
Voer de volgende stappen uit om de details van elke taak weer te geven.

#### <a name="to-view-job-details"></a>Taakdetails weergeven
1. Ga naar uw StorSimple Device Manager-service en klik vervolgens op **taken**.

2. In de **taken** blade u geïnteresseerd bent door een query uitvoeren met de juiste filters of meer taken worden weergegeven. U kunt zoeken naar voltooid, wordt uitgevoerd, of taken geannuleerd.

    ![Taakblade](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Selecteer en klik op een taak.

    ![Taakblade](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. Op de taakblade, kunt u de status, gegevens, statistieken en statistische gegevens weergeven.
   
    ![Taakdetails](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Een taak annuleren
Voer de volgende stappen uit als u wilt een actieve taak annuleren.

> [!NOTE]
> Sommige taken, zoals het wijzigen van een volume als u wilt wijzigen van het volumetype of uitbreiden van een volume kunnen niet worden geannuleerd.


### <a name="to-cancel-a-job"></a>Een taak annuleren
1. Op de **taken** pagina, de actieve taak/taken die u Annuleren wilt door het uitvoeren van een query met de juiste filters weer te geven. Selecteer de taak.

2. Met de rechtermuisknop op de geselecteerde taak voor het aanroepen van het contextmenu en klik op **annuleren**.

    ![Taakdetails](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Klik op **Ja** als u om bevestiging wordt gevraagd. Nu is deze taak geannuleerd.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [uw StorSimple-back-upbeleid beheren](storsimple-8000-manage-backup-policies-u2.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

