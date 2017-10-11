---
title: Beheren van uw StorSimple-back-upcatalogus | Microsoft Docs
description: Wordt uitgelegd hoe u met de StorSimple Manager-servicepagina back-upcatalogus lijst, selecteert en back-upsets voor een volume verwijderen.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 5ee9855e1428c7a2d871d9c215d302c5c3b7101a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>De StorSimple Manager-service gebruiken voor het beheren van uw back-upcatalogus
## <a name="overview"></a>Overzicht
De StorSimple Manager-service **back-upcatalogus** pagina wordt weergegeven voor de back-upsets die worden gemaakt wanneer de handmatige of geplande back-ups worden gemaakt. U kunt deze pagina gebruiken alle-ups voor een back-upbeleid of een volume, selecteer weergeven of verwijderen van back-ups, of een back-up gebruiken om te zetten of te klonen van een volume.

Deze zelfstudie wordt uitgelegd hoe u de optie en verwijderen van een back-upset. Als u wilt weten hoe u uw apparaat van de back-up herstellen, gaat u naar [uw apparaat terugzetten vanaf een back-upset](storsimple-restore-from-backup-set.md). Voor informatie over het klonen van een volume, gaat u naar [klonen van een StorSimple-volume](storsimple-clone-volume.md).

![Back-upcatalogus](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

De **back-upcatalogus** pagina vindt u een query voor het verfijnen van uw back-up selectie instellen. U kunt de back-upsets die zijn opgehaald, op basis van de volgende parameters op te filteren:

* **Apparaat** : het apparaat waarop de back-upset is gemaakt.
* **Back-up maken van beleid of Volume** – de back-upbeleid of het volume dat is gekoppeld aan deze back-upset.
* **Van en naar** – het bereik datum en tijd wanneer de back-upset is gemaakt.

De gefilterde back-upsets worden vervolgens in een tabel weergegeven op basis van de volgende kenmerken:

* **Naam** : de naam van de back-upbeleid of het volume dat is gekoppeld aan de back-upset.
* **De grootte van** : de werkelijke grootte van de back-upset.
* **Gemaakt op** – de datum en tijd waarop de back-ups zijn gemaakt. 
* **Type** – sets van back-up kunnen worden lokale momentopnamen of cloud worden opgeslagen. Een lokale momentopname is een back-up van alle uw worden lokaal opgeslagen volumegegevens op het apparaat dat een cloudmomentopname verwijst naar de back-up van de volumegegevens die zich in de cloud. Lokale momentopnamen bieden sneller toegang terwijl cloudmomentopnamen voor gegevenstolerantie worden gekozen.
* **Gestart door** – de back-ups automatisch kunnen worden geïnitieerd door een schema of handmatig door een gebruiker. U kunt een back-upbeleid back-ups plannen. U kunt ook de **back-up maken** optie voor het uitvoeren van een handmatige back-up.

## <a name="list-backup-sets-for-a-volume"></a>Lijst met back-up wordt ingesteld voor een volume
De volgende stappen als u alle de back-ups voor een volume wilt weergeven.

#### <a name="to-list-backup-sets"></a>Aan de lijst met back-upsets
1. Klik op de pagina StorSimple Manager-service op de **back-upcatalogus** tabblad.
2. Filter de selecties als volgt:
   
   1. Selecteer het juiste apparaat.
   2. Kies een volume om de bijbehorende de back-ups weer te geven in de vervolgkeuzelijst.
   3. Geef het tijdsbereik.
   4. Klik op het vinkje ![Vinkje](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Deze query uitvoeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume worden weergegeven in de lijst met back-upsets.

## <a name="select-a-backup-set"></a>Selecteer een back-upset
De volgende stappen om te selecteren van een back-upset voor een volume of de back-upbeleid.

#### <a name="to-select-a-backup-set"></a>Een back-upset selecteren
1. Klik op de pagina StorSimple Manager-service op de **back-upcatalogus** tabblad.
2. Filter de selecties als volgt:
   
   1. Selecteer het juiste apparaat.
   2. Kies in de vervolgkeuzelijst het volume of back-up-beleid voor de back-up die u wilt selecteren.
   3. Geef het tijdsbereik.
   4. Klik op het vinkje ![Vinkje](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Deze query uitvoeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.
3. Selecteer en breid uit een back-upset. De **herstellen** en **verwijderen** opties worden weergegeven onder aan de pagina. U kunt een van deze acties uitvoeren op de back-upset die u hebt geselecteerd.

## <a name="delete-a-backup-set"></a>Een back-upset verwijderen
Een back-up verwijderen als u niet langer wilt bewaren van de gegevens die zijn gekoppeld. Voer de volgende stappen uit als u wilt verwijderen van een back-upset.

#### <a name="to-delete-a-backup-set"></a>Een back-upset verwijderen
1. Klik op de pagina StorSimple Manager-service op de **back-upcatalogus tabblad**.
2. Filter de selecties als volgt:
   
   1. Selecteer het juiste apparaat.
   2. Kies in de vervolgkeuzelijst het volume of back-up-beleid voor de back-up die u wilt selecteren.
   3. Geef het tijdsbereik.
   4. Klik op het vinkje ![Vinkje](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Deze query uitvoeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.
3. Selecteer en breid uit een back-upset. De **herstellen** en **verwijderen** opties worden weergegeven onder aan de pagina. Klik op **Verwijderen**.
4. U wordt gewaarschuwd wanneer het verwijderen is uitgevoerd en wanneer het met succes is voltooid. Nadat de verwijdering is voltooid, kunt u de query op deze pagina vernieuwen. De verwijderde back-upset wordt niet meer weergegeven in de lijst met back-upsets.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [gebruik van de back-upcatalogus uw apparaat terugzetten vanaf een back-upset](storsimple-restore-from-backup-set.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

