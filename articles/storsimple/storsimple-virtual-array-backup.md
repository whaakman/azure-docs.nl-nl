---
title: Microsoft Azure StorSimple Virtual Array back-zelfstudie | Microsoft Docs
description: Beschrijft hoe u back-up van StorSimple Virtual Array shares en volumes.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a61dcca1f78b6ba444a2deefcf6b8bb4fd5c5087
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58121975"
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>Back-up shares of volumes op uw StorSimple Virtual Array

## <a name="overview"></a>Overzicht

De StorSimple Virtual Array is een hybride cloud storage on-premises virtuele apparaat die kan worden geconfigureerd als een bestandsserver of een iSCSI-server. De virtuele matrix kan de gebruiker te maken van geplande en handmatige back-ups van alle shares of volumes op het apparaat. Wanneer geconfigureerd als een bestandsserver, kunt u ook herstel op itemniveau. Deze zelfstudie wordt beschreven hoe u geplande en handmatige back-ups en herstel op itemniveau voor het herstellen van een verwijderd bestand op uw virtuele array uitvoert.

In deze zelfstudie is van toepassing op de virtuele StorSimple-matrices alleen. Voor informatie over de 8000-serie, gaat u naar [maken van een back-up voor 8000-serie-apparaat](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Back-up van bestandsshares en volumes

Back-ups point-in-time-beveiliging bieden, de herstelmogelijkheden te verbeteren en hersteltijden voor shares en volumes te minimaliseren. U kunt een back-up van een share of een volume maken op uw StorSimple-apparaat op twee manieren: **Geplande** of **handmatig**. Elk van de methoden wordt in de volgende secties besproken.

## <a name="change-the-backup-start-time"></a>Wijzigen van de begintijd voor back-up

> [!NOTE]
> In deze release, worden geplande back-ups gemaakt door een standaardbeleid dat wordt uitgevoerd op een bepaald tijdstip dagelijks en back-ups van alle shares of volumes op het apparaat. Het is niet mogelijk te maken van aangepaste beleidsregels voor geplande back-ups op dit moment.


Uw StorSimple Virtual Array is een standaard back-upbeleid dat begint op een bepaald tijdstip van de dag (22:30) en back-ups van alle shares of volumes op het apparaat eenmaal per dag. U kunt de tijd waarop de back-up wordt gestart, maar de frequentie en de bewaarperiode (die Hiermee geeft u het aantal back-ups wilt behouden) kunnen niet worden gewijzigd. Het hele virtuele apparaat is tijdens deze back-ups back-ups. Dit kan mogelijk van invloed zijn op de prestaties van het apparaat en van invloed zijn op de werkbelastingen die zijn geïmplementeerd op het apparaat. Daarom raden we u aan deze back-ups te plannen voor daluren.

 Voer de volgende stappen uit als u wilt wijzigen van de begintijd van de standaard-back-up, het [Azure-portal](https://portal.azure.com/).

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>De begintijd voor het standaard back-upbeleid wijzigen

1. Ga naar **apparaten**. De lijst met apparaten die zijn geregistreerd met uw StorSimple Device Manager-service wordt weergegeven. 
   
    ![Navigeer naar apparaten](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Selecteer en klik op het apparaat. De **instellingen** blade wordt weergegeven. Ga naar **beheren > back-upbeleid**.
   
    ![Selecteer uw apparaat](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. In de **back-upbeleid** blade, de begintijd van de standaardwaarde is 22:30. U kunt de nieuwe begintijd voor de dagelijkse planning opgeven in de tijdzone apparaat.
   
    ![Navigeer naar de back-upbeleid](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. Klik op **Opslaan**.

### <a name="take-a-manual-backup"></a>Een handmatige back-up maken

Naast de geplande back-ups, kunt u uitvoeren (op aanvraag) van een handmatige back-ups van apparaatgegevens op elk gewenst moment.

#### <a name="to-create-a-manual-backup"></a>Een handmatige back-up maken

1. Ga naar **apparaten**. Selecteer uw apparaat en met de rechtermuisknop op **...**  aan de rechterkant in de geselecteerde rij. Selecteer in het contextmenu **back-up maken**.
   
    ![Ga voor het back-up maken](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. In de **back-up maken** blade, klikt u op **back-up maken**. Hiermee worden back-up alle shares op de bestandsserver of alle volumes op de iSCSI-server. 
   
    ![back-up begint](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    Een on-demand back-up wordt gestart en u ziet dat er een back-uptaak is gestart.
   
    ![back-up begint](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    Zodra de taak is voltooid, bent u opnieuw bericht. Het back-upproces vervolgens wordt gestart.
   
    ![back-uptaak gemaakt](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. De voortgang van de back-ups volgen en bekijk de taakdetails, klikt u op de melding. Hiermee gaat u naar **taakgegevens**.
   
     ![Details van back-uptaak weergeven](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Nadat de back-up voltooid is, gaat u naar **beheren > back-upcatalogus**. U ziet een cloud-momentopname van alle shares (of volumes) op uw apparaat.
   
    ![Voltooide back-up](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Bestaande back-ups weergeven
Als u de bestaande back-ups, moet u de volgende stappen uitvoeren in Azure portal.

#### <a name="to-view-existing-backups"></a>Om weer te geven van de bestaande back-ups

1. Ga naar **apparaten** blade. Selecteer en klik op het apparaat. In de **instellingen** blade, Ga naar **beheren > back-upcatalogus**.
   
    ![Navigeer naar de back-catalogus](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Geef de volgende criteria om te worden gebruikt om te filteren:
   
   - **Tijdsbereik** – kan **afgelopen 1 uur**, **afgelopen 24 uur**, **afgelopen 7 dagen**, **afgelopen 30 dagen**, **afgelopen jaar** , en **aangepaste datum**.
    
   - **Apparaten** : Selecteer in de lijst van bestandsservers of iSCSI-servers die zijn geregistreerd bij uw StorSimple Device Manager-service.
   
   - **Gestart** – automatisch kunnen worden **geplande** (door een back-upbeleid) of **handmatig** gestart (door u).
   
     ![Back-ups filteren](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. Klik op **Toepassen**. De gefilterde lijst met back-ups wordt weergegeven in de **back-upcatalogus** blade. Houd er rekening mee slechts 100 back-elementen kunnen worden weergegeven op een bepaald moment.
   
    ![Bijgewerkte back-catalogus](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

