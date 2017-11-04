---
title: Beheren van uw StorSimple-back-upcatalogus | Microsoft Docs
description: Wordt uitgelegd hoe u de pagina StorSimple Apparaatbeheer back-upcatalogus wilt weergeven, selecteert en back-upsets verwijderen.
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
ms.openlocfilehash: ac2577c6cd350d6d437d55e61ec73d954cb24893
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van uw back-upcatalogus
## <a name="overview"></a>Overzicht
De service Manager voor StorSimple-apparaat **back-upcatalogus** blade wordt weergegeven voor de back-upsets die worden gemaakt wanneer de handmatige of geplande back-ups worden gemaakt. U kunt deze pagina gebruiken alle-ups voor een back-upbeleid of een volume, selecteer weergeven of verwijderen van back-ups, of een back-up gebruiken om te zetten of te klonen van een volume.

Deze zelfstudie wordt uitgelegd hoe u de optie en verwijderen van een back-upset. Als u wilt weten hoe u uw apparaat van de back-up herstellen, gaat u naar [uw apparaat terugzetten vanaf een back-upset](storsimple-8000-restore-from-backup-set-u2.md). Voor informatie over het klonen van een volume, gaat u naar [klonen van een StorSimple-volume](storsimple-8000-clone-volume-u2.md).

![Back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

De **back-upcatalogus** blade biedt een query voor het verfijnen van uw back-up selectie instellen. U kunt de back-upsets die zijn opgehaald, op basis van de volgende parameters op te filteren:

* **Apparaat** : het apparaat waarop de back-upset is gemaakt.
* **Back-upbeleid of Volume** – de back-upbeleid of het volume dat is gekoppeld aan deze back-upset.
* **Van en naar** – het bereik datum en tijd wanneer de back-upset is gemaakt.

De gefilterde back-upsets worden vervolgens in een tabel weergegeven op basis van de volgende kenmerken:

* **Naam** : de naam van de back-upbeleid of het volume dat is gekoppeld aan de back-upset.
* **De grootte van** : de werkelijke grootte van de back-upset.
* **Gemaakt op** – de datum en tijd waarop de back-ups zijn gemaakt. 
* **Type** – sets van back-up kunnen worden lokale momentopnamen of cloud worden opgeslagen. Een lokale momentopname is een back-up van alle uw worden lokaal opgeslagen volumegegevens op het apparaat dat een cloudmomentopname verwijst naar de back-up van de volumegegevens die zich in de cloud. Lokale momentopnamen bieden sneller toegang terwijl cloudmomentopnamen voor gegevenstolerantie worden gekozen.
* **Gestart door** – de back-ups automatisch kunnen worden geïnitieerd door een schema of handmatig door een gebruiker. U kunt een back-upbeleid back-ups plannen. U kunt ook de **back-up maken** optie voor het uitvoeren van een handmatige back-up.

## <a name="list-backup-sets-for-a-backup-policy"></a>Lijst met back-up wordt ingesteld voor een back-upbeleid
De volgende stappen als u alle de back-ups voor een back-upbeleid wilt weergeven.

#### <a name="to-list-backup-sets"></a>Aan de lijst met back-upsets
1. Ga naar uw StorSimple-Apparaatbeheer service en klik op **back-upcatalogus**.

2. Filter de selecties als volgt:
   
   1. Geef het tijdsbereik.
   2. Selecteer het juiste apparaat.
   3. Filteren op **back-up maken van beleid** om weer te geven van de bijbehorende de back-ups.
   3. Kies in de vervolgkeuzelijst back-upbeleid **alle** de back-ups op het geselecteerde apparaat weergeven.
   4. Klik op **toepassen** deze query uit te voeren.
      
      De back-ups die zijn gekoppeld aan de geselecteerde back-upbeleid moeten worden weergegeven in de lijst met back-upsets.

      ![Ga naar de back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Selecteer een back-upset
De volgende stappen om te selecteren van een back-upset voor een volume of de back-upbeleid.

#### <a name="to-select-a-backup-set"></a>Een back-upset selecteren
1. Ga naar uw StorSimple-Apparaatbeheer service en klik op **back-upcatalogus**.
2. Filter de selecties als volgt:
   
   1. Geef het tijdsbereik. 
   2. Selecteer het juiste apparaat. 
   3. Filteren op een volume of back-up beleid voor de back-up die u wilt selecteren.
   4. Klik op **toepassen** deze query uit te voeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.

      ![Ga naar de back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Selecteer en breid uit een back-upset. U ziet nu de back-upsets onderverdeeld op basis van de volumes die deze bevat. De **herstellen** en **verwijderen** opties zijn beschikbaar via het snelmenu (Klik met de rechtermuisknop) voor de back-upset. U kunt een van deze acties uitvoeren op de back-upset die u hebt geselecteerd.

    ![Ga naar de back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Een back-upset verwijderen
Een back-up verwijderen als u niet langer wilt bewaren van de gegevens die zijn gekoppeld. Voer de volgende stappen uit als u wilt verwijderen van een back-upset.

#### <a name="to-delete-a-backup-set"></a>Een back-upset verwijderen
 Ga naar uw StorSimple-Apparaatbeheer service en klik op **back-upcatalogus**.
2. Filter de selecties als volgt:
   
   1. Geef het tijdsbereik. 
   2. Selecteer het juiste apparaat. 
   3. Filteren op een volume of back-up beleid voor de back-up die u wilt selecteren.
   4. Klik op **toepassen** deze query uit te voeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.

      ![Ga naar de back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Selecteer en breid uit een back-upset. U ziet nu de back-upsets onderverdeeld op basis van de volumes die deze bevat. De **herstellen** en **verwijderen** opties zijn beschikbaar via het snelmenu (Klik met de rechtermuisknop) voor de back-upset. Met de rechtermuisknop op de geselecteerde back-upset en selecteer in het contextmenu **verwijderen**.

    ![Ga naar de back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. Als u wordt gevraagd om bevestiging, Controleer de weergegeven informatie en op **verwijderen**. De geselecteerde back-up wordt permanent verwijderd.

    ![Ga naar de back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. U wordt gewaarschuwd wanneer het verwijderen is uitgevoerd en wanneer het met succes is voltooid. Nadat de verwijdering is voltooid, kunt u de query op deze pagina vernieuwen. De verwijderde back-upset wordt niet meer weergegeven in de lijst met back-upsets.

    ![Ga naar de back-upcatalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [gebruik van de back-upcatalogus uw apparaat terugzetten vanaf een back-upset](storsimple-8000-restore-from-backup-set-u2.md).
* Meer informatie over hoe [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

