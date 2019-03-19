---
title: Uw StorSimple-back-upcatalogus beheren | Microsoft Docs
description: Wordt uitgelegd hoe u met het hulpprogramma voor het back-catalogus van de StorSimple Device Manager-servicepagina lijst, selecteer en verwijderen van back-upsets.
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
ms.openlocfilehash: 07d9e03f1631ebce88a7a7c2e33be62f21dda522
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58076782"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a>De StorSimple Device Manager-service gebruiken voor uw back-upcatalogus beheren
## <a name="overview"></a>Overzicht
De service StorSimple Device Manager **back-catalogus** blade wordt weergegeven voor de back-upsets die worden gemaakt wanneer er een handmatige of geplande back-ups worden gemaakt. U kunt deze pagina gebruiken om te lijst van alle de back-ups voor een back-upbeleid of een volume, selecteren of verwijderen van back-ups of gebruik van een back-up wilt terugzetten, of een volume klonen.

In deze zelfstudie wordt uitgelegd hoe u wilt weergeven, selecteert en een back-upset verwijderen. Als u wilt weten hoe u uw apparaat herstellen vanuit back-up, gaat u naar [uw apparaat herstellen vanuit een back-upset](storsimple-8000-restore-from-backup-set-u2.md). Als u wilt weten hoe u een volume klonen, gaat u naar [klonen van een StorSimple-volume](storsimple-8000-clone-volume-u2.md).

![back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

De **back-catalogus** blade biedt een query voor het beperken van de back-up selectie instellen. U kunt filteren, de back-upsets die worden opgehaald, op basis van de volgende parameters:

* **Apparaat** : het apparaat waarop de back-upset is gemaakt.
* **Back-upbeleid of Volume** : de back-upbeleid of het volume dat is gekoppeld aan deze back-upset.
* **Van en naar** : het bereik datum en tijd waarop de back-upset is gemaakt.

De gefilterde back-upsets worden vervolgens de tabelindeling op basis van de volgende kenmerken:

* **Naam** – de naam van de back-upbeleid of het volume dat is gekoppeld aan de back-upset.
* **Grootte** : de werkelijke grootte van de back-upset.
* **Gemaakt op** – de datum en tijd waarop de back-ups zijn gemaakt. 
* **Type** – sets van back-up kunnen worden lokale momentopnamen of momentopnamen in de cloud. Een lokale momentopname is een back-up van alle uw volumegegevens lokaal opgeslagen op het apparaat, terwijl een cloud-momentopname verwijst naar de back-up van volumegegevens die zich bevinden in de cloud. Lokale momentopnamen bieden snellere toegang tot dat cloudmomentopnamen worden gekozen voor gegevenstolerantie.
* **Gestart door** : de back-ups automatisch kunnen worden gestart door een schema of handmatig door de gebruiker. Een back-upbeleid kunt u back-ups plannen. U kunt ook kunt u de **back-up maken** optie een handmatige back-up.

## <a name="list-backup-sets-for-a-backup-policy"></a>Lijst met back-up wordt ingesteld voor een back-upbeleid
De volgende stappen als u wilt weergeven van alle de back-ups voor een back-upbeleid.

#### <a name="to-list-backup-sets"></a>Aan de lijst met back-upsets
1. Ga naar uw StorSimple Device Manager-service en klik op **back-upcatalogus**.

2. Filter de selecties als volgt:
   
   1. De tijdsperiode opgeven.
   2. Selecteer het juiste apparaat.
   3. Filteren op **back-upbeleid** om de bijbehorende de back-ups weer te geven.
   3. Kies in de vervolgkeuzelijst back-upbeleid **alle** om de back-ups op het geselecteerde apparaat weer te geven.
   4. Klik op **toepassen** deze query uit te voeren.
      
      De back-ups die zijn gekoppeld aan de geselecteerde back-upbeleid moeten worden weergegeven in de lijst met back-upsets.

      ![Ga naar de back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Een back-upset selecteren
De volgende stappen om te selecteren van een back-upset voor een volume of de back-upbeleid.

#### <a name="to-select-a-backup-set"></a>Een back-upset selecteren
1. Ga naar uw StorSimple Device Manager-service en klik op **back-upcatalogus**.
2. Filter de selecties als volgt:
   
   1. De tijdsperiode opgeven. 
   2. Selecteer het juiste apparaat. 
   3. Filteren op volume of back-up beleid voor de back-up die u wilt selecteren.
   4. Klik op **toepassen** deze query uit te voeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.

      ![Ga naar de back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Selecteer en uitbreiden van een back-upset. U ziet nu de back-upsets onderverdeeld op basis van de volumes die deze bevat. De **herstellen** en **verwijderen** opties zijn beschikbaar via het snelmenu (klikken met de rechtermuisknop) voor de back-upset. U kunt een van deze acties uitvoeren op de back-upset die u hebt geselecteerd.

    ![Ga naar de back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Een back-upset verwijderen
Een back-up verwijderen wanneer u niet langer wilt bewaren van de gegevens die zijn gekoppeld aan deze. Voer de volgende stappen uit als u wilt verwijderen van een back-upset.

#### <a name="to-delete-a-backup-set"></a>Een back-upset verwijderen
 Ga naar uw StorSimple Device Manager-service en klik op **back-upcatalogus**.
1. Filter de selecties als volgt:
   
   1. De tijdsperiode opgeven. 
   2. Selecteer het juiste apparaat. 
   3. Filteren op volume of back-up beleid voor de back-up die u wilt selecteren.
   4. Klik op **toepassen** deze query uit te voeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.

      ![Ga naar de back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

1. Selecteer en uitbreiden van een back-upset. U ziet nu de back-upsets onderverdeeld op basis van de volumes die deze bevat. De **herstellen** en **verwijderen** opties zijn beschikbaar via het snelmenu (klikken met de rechtermuisknop) voor de back-upset. Met de rechtermuisknop op de geselecteerde back-upset en selecteer in het contextmenu **verwijderen**.

    ![Ga naar de back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

1. Wanneer u wordt gevraagd om bevestiging, controleert u de weergegeven informatie en klikt u op **verwijderen**. De geselecteerde back-up wordt permanent verwijderd.

    ![Ga naar de back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

1. U krijgt bericht wanneer de verwijdering is in uitvoering en wanneer deze met succes voltooid. Nadat de verwijdering is voltooid, vernieuwt u de query op deze pagina. De verwijderde back-upset wordt niet meer weergegeven in de lijst met back-upsets.

    ![Ga naar de back-catalogus](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [de back-catalogus gebruiken om te herstellen van uw apparaat vanuit een back-upset](storsimple-8000-restore-from-backup-set-u2.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

