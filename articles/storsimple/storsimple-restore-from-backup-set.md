---
title: Een StorSimple-volume voor back-up terugzetten | Microsoft Docs
description: Legt uit hoe de StorSimple Manager-servicepagina back-upcatalogus gebruiken om te zetten van een StorSimple-volume vanuit een back-upset.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 0ed433ba93cb08742e3f0b09cc7acaa2cd62461e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Een StorSimple-volume herstelt vanuit een back-upset
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Overzicht
De **back-upcatalogus** pagina wordt weergegeven voor de back-upsets die worden gemaakt wanneer de handmatige of automatische back-ups worden gemaakt. U kunt deze pagina gebruiken alle-ups voor een back-upbeleid of een volume, selecteer weergeven of verwijderen van back-ups, of een back-up gebruiken om te zetten of te klonen van een volume.

 ![Back-pagina van de catalogus](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Deze zelfstudie wordt uitgelegd hoe u de **back-upcatalogus** pagina om een volume op uw apparaat terugzetten vanaf een back-upset.

## <a name="how-to-use-the-backup-catalog"></a>Het gebruik van de back-upcatalogus
De **back-upcatalogus** pagina vindt u een query die u helpt bij het verfijnen van uw back-up selectie instellen. U kunt de back-upsets die worden opgehaald op basis van de volgende parameters op te filteren:

* **Apparaat** : het apparaat waarop de back-upset is gemaakt.
* **Back-up maken van beleid** of **volume** – de back-upbeleid of het volume dat is gekoppeld aan deze back-upset.
* **Van** en **naar** – het bereik datum en tijd wanneer de back-upset is gemaakt.

De gefilterde back-upsets worden vervolgens in een tabel weergegeven op basis van de volgende kenmerken:

* **Naam** : de naam van de back-upbeleid of het volume dat is gekoppeld aan de back-upset.
* **De grootte van** : de werkelijke grootte van de back-upset.
* **Gemaakt op** – de datum en tijd waarop de back-ups zijn gemaakt. 
* **Type** – sets van back-up kunnen worden lokale momentopnamen of cloud worden opgeslagen. Een lokale momentopname is een back-up van alle uw worden lokaal opgeslagen volumegegevens op het apparaat dat een cloudmomentopname verwijst naar de back-up van de volumegegevens die zich in de cloud. Lokale momentopnamen bieden sneller toegang terwijl cloudmomentopnamen voor gegevenstolerantie worden gekozen.
* **Gestart door** – de back-ups automatisch kunnen worden gestart volgens een schema of handmatig door een gebruiker. (U kunt een back-upbeleid back-ups plannen. U kunt ook de **back-up maken** optie voor een interactieve back-up te maken.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Het terugzetten van uw StorSimple-volume van een back-up
U kunt de **back-upcatalogus** pagina uw StorSimple-volume van een specifieke back-up herstellen. 

> [!WARNING]
> Terugzetten vanuit een back-up wordt vervangen door de bestaande volumes van de back-up. Dit kan ertoe leiden dat het verlies van gegevens die is geschreven nadat de back-up is gemaakt.
> 
> 

Voordat u een herstelpunt op een volume hebt gestart, zorg ervoor dat het volume offline is. U moet eerst het volume offline nemen op de host en klik vervolgens op het apparaat. Volg de stappen in [offline zetten van een volume](storsimple-manage-volumes.md#take-a-volume-offline). Voer de volgende stappen uit voor het herstellen van een volume van een back-upset.

### <a name="to-restore-from-a-backup-set"></a>Herstel uit vanaf een back-upset
1. Klik op de pagina StorSimple Manager-service op de **back-upcatalogus** tabblad.
   
    ![Back-upcatalogus](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. Selecteer een back-up als volgt instellen:
   
   1. Selecteer het juiste apparaat.
   2. Kies in de vervolgkeuzelijst het volume of back-up-beleid voor de back-up die u wilt selecteren.
   3. Geef het tijdsbereik.
   4. Klik op het vinkje ![vinkje](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) Deze query uitvoeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.
3. Vouw in de back-upset om de gekoppelde volumes weer te geven. Deze volumes moeten op de host en het apparaat offline worden gezet voordat u ze kunt herstellen. Volg de stappen in [offline zetten van een volume](storsimple-manage-volumes.md#take-a-volume-offline).
   
   > [!IMPORTANT]
   > Zorg ervoor dat u hebt ondernomen volumes offline op de host eerst voordat u de volumes offline op het apparaat nemen. Als u niet de volumes offline op de host nemen, kan dit leiden tot beschadiging van gegevens.
   > 
   > 
4. Selecteer een back-upset. Klik op **herstellen** aan de onderkant van de pagina.
5. U wordt gevraagd om bevestiging. 
   
    ![Bevestigingspagina](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. Raadpleeg de informatie herstellen en klik op het vinkje ![vinkje](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Er wordt nu een hersteltaak die u bekijken door het openen van kunt de **taken** pagina. 
7. Nadat het herstel voltooid is, kunt u controleren dat de inhoud van uw volumes worden vervangen door de volumes van de back-up.

![Video beschikbaar](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video beschikbaar**

Als u wilt een video die u laat zien hoe u gebruikt voor het klonen en functies in StorSimple voor het herstellen van verwijderde bestanden herstellen, klikt u op [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [beheren StorSimple-volumes](storsimple-manage-volumes.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

