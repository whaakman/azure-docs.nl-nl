---
title: Weergeven en beheren van StorSimple taken | Microsoft Docs
description: Beschrijving van de StorSimple Manager-servicepagina taken en het gebruik van het bijhouden van recente, huidige en geplande back-uptaken.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 426fd3ec76157670c9d192f007adacc4284abb42
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Met de StorSimple Manager-service kunt weergeven en beheren van StorSimple-taken
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Overzicht
De **taken** pagina biedt één centrale portal voor het weergeven en beheren van taken die zijn gestart op apparaten die zijn verbonden met uw StorSimple Manager-service. U kunt geplande, actieve voltooide en mislukte taken voor meerdere apparaten weergeven. Resultaten zijn in tabelvorm gepresenteerd.

![Pagina taken](./media/storsimple-manage-jobs/HCS_JobsPage.png)

U kunt de taken die u bent geïnteresseerd door te filteren op velden, zoals snel zoeken:

* **Status** – taken kunnen worden uitgevoerd, geplande, is mislukt, voltooide, annuleren of geannuleerd.
* **Type** – taken kunnen worden gemaakt als gevolg van een geplande of een op aanvraag back-up (**duren back-**), klonen, een apparaat te herstellen of update-bewerking.
* **Apparaten** – taken zijn gestart op een bepaald apparaat verbonden met uw service.
* **Van en naar** – taken kunnen worden gefilterd op basis van het bereik van de datum en tijd.

De gefilterde taken worden vervolgens in een tabel weergegeven op basis van de volgende kenmerken:

* **Type** : back-up, kloon, terugzetten, failover of update.
* **Status** – uitgevoerd, gepland, is mislukt, voltooid, geannuleerd of geannuleerd.
* **Entiteit** – de taken kunnen worden gekoppeld aan een volume, een back-upbeleid of een apparaat. De taak van een kloon is gekoppeld aan een volume dat een geplande back-uptaak gekoppeld aan een back-upbeleid is. Een apparaat-taak is gemaakt als gevolg van een noodherstel (DR) of een herstelbewerking.
* **Apparaat** : de naam van het apparaat waarop de taak is gestart.
* **Gestart op** – de tijd waarop de taak is gestart.
* **Voortgang** – het percentage voltooien van een actieve taak. Dit moet altijd 100% zijn voor een voltooide taak.

De lijst met taken wordt elke 30 seconden vernieuwd.

U kunt de volgende taak-gerelateerde acties uitvoeren op deze pagina:

* Taakdetails weergeven
* Een taak annuleren

## <a name="view-job-details"></a>Taakdetails weergeven
De volgende stappen uit om de details van elke taak uitvoeren.

#### <a name="to-view-job-details"></a>Taakdetails weergeven
1. Op de **taken** pagina, wordt de taak of taken die u bent geïnteresseerd door het uitvoeren van een query met de juiste filters weergegeven. U kunt zoeken voor voltooid, uitvoeren of taken geannuleerd.
2. Selecteer een taak.
3. Klik onder aan de pagina op **Details**.
4. In de **back-up taakdetails** in het dialoogvenster kunt u de status, details, tijdstatistieken en statistieken van de gegevens bekijken.

## <a name="cancel-a-job"></a>Een taak annuleren
De volgende stappen uit om te annuleren een actieve taak uitvoeren.

### <a name="to-cancel-a-job"></a>Een taak annuleren
1. Op de **taken** pagina, wordt de actieve taak of taken die u Annuleren wilt door het uitvoeren van een query met de juiste filters weergegeven.
2. Selecteer de taak.
3. Klik onder aan de pagina op **annuleren**.
4. Klik op **Ja** als u om bevestiging wordt gevraagd.

Deze taak is nu geannuleerd.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [beheren van uw StorSimple-back-upbeleid](storsimple-manage-backup-policies.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

