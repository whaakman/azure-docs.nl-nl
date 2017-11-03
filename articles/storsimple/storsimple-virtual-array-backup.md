---
title: Microsoft Azure StorSimple virtuele matrix back-zelfstudie | Microsoft Docs
description: Beschrijft hoe u back-up van virtuele StorSimple-matrix-shares en -volumes.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c926f0c80ce56cac3106ad97ec3ec2e18a8e2cc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>Back-up shares of volumes op uw virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

De virtuele StorSimple-matrix is een hybride cloud-opslag on-premises virtuele apparaat die kan worden geconfigureerd als een bestandsserver of een iSCSI-server. De virtuele matrix kan de gebruiker geplande en handmatige back-ups van de shares of volumes maken op het apparaat. Wanneer geconfigureerd als een bestandsserver, kunt u ook herstel op itemniveau. Deze zelfstudie wordt beschreven hoe geplande en handmatige back-ups maken en herstel op itemniveau voor het herstellen van een verwijderd bestand op uw virtuele matrix uitvoeren.

Deze zelfstudie geldt voor de StorSimple virtuele matrices alleen. Voor informatie over 8000-serie, gaat u naar [een back-up voor 8000 series apparaat maken](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Back-up-shares en -volumes

Back-ups punt in tijd beveiliging bieden, verbeteren de herstelmogelijkheden en hersteltijden voor shares en -volumes te minimaliseren. U kunt back-up een share of het volume op uw StorSimple-apparaat op twee manieren: **geplande** of **handmatige**. Elk van de methoden wordt in de volgende secties besproken.

## <a name="change-the-backup-start-time"></a>De begintijd voor back-ups wijzigen

> [!NOTE]
> In deze release worden geplande back-ups gemaakt door een standaardbeleid dat wordt dagelijks wordt uitgevoerd op een opgegeven periode zijn uitgevoerd en een back-up alle shares of volumes op het apparaat. Het is niet mogelijk te maken van aangepaste beleidsregels voor geplande back-ups op dit moment.


Uw virtuele StorSimple-matrix heeft een standaard back-upbeleid dat begint op een bepaald tijdstip van de dag (22.30) en een back-up alle shares of volumes op het apparaat eenmaal per dag. U kunt de tijd waarop de back-up wordt gestart, maar de frequentie en de bewaarperiode (die geeft het aantal back-ups wilt behouden) kunnen niet worden gewijzigd. Tijdens deze back-ups, het virtuele apparaat voor volledige back-up. Dit kan mogelijk invloed op de prestaties van het apparaat en van invloed zijn op de werkbelastingen die worden geïmplementeerd op het apparaat. Daarom raden we u aan deze back-ups te plannen voor daluren.

 Om te wijzigen van de standaard back-begintijd, voer de volgende stappen in de [Azure-portal](https://portal.azure.com/).

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>De begintijd voor het standaard back-upbeleid wijzigen

1. Ga naar **apparaten**. De lijst met apparaten die zijn geregistreerd bij uw StorSimple-apparaat Manager-service wordt weergegeven. 
   
    ![Navigeer naar apparaten](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Selecteer en klikt u op uw apparaat. De **instellingen** blade wordt weergegeven. Ga naar **beheren > back-upbeleid**.
   
    ![Selecteer het apparaat](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. In de **back-upbeleid** blade de begintijd van de standaardwaarde is 22:30. U kunt de nieuwe begintijd voor de dagelijkse planning opgeven in de tijdzone apparaat.
   
    ![Navigeer naar de back-upbeleid](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. Klik op **Opslaan**.

### <a name="take-a-manual-backup"></a>Maak een handmatige back-up

Naast de geplande back-ups, kunt u nemen (op aanvraag) van een handmatige back-up van apparaatgegevens op elk gewenst moment.

#### <a name="to-create-a-manual-backup"></a>Een handmatige back-up maken

1. Ga naar **apparaten**. Selecteer uw apparaat en met de rechtermuisknop op **...**  aan de rechterkant in de geselecteerde rij. Selecteer in het contextmenu **back-up maken**.
   
    ![Navigeer om de back-up maken](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. In de **back-up maken** blade, klikt u op **back-up maken**. Dit wordt back-up alle shares op de bestandsserver of alle volumes op uw iSCSI-server. 
   
    ![back-up wordt gestart](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    Een op aanvraag back-up wordt gestart en u ziet dat er een back-uptaak is gestart.
   
    ![back-up wordt gestart](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    Zodra de taak is voltooid, wordt u nogmaals gewaarschuwd. Het back-upproces vervolgens wordt gestart.
   
    ![back-uptaak gemaakt](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. De voortgang van de back-ups volgen en bekijk de taakdetails, klikt u op de melding. Hiermee gaat u naar **taakgegevens**.
   
     ![details van de back-uptaak](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Nadat de back-up voltooid is, gaat u naar **Management > back-upcatalogus**. U ziet een cloudmomentopname van alle shares (of volumes) op uw apparaat.
   
    ![Voltooide back-up](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Bestaande back-ups weergeven
Als u wilt weergeven van de bestaande back-ups, moet u de volgende stappen uitvoeren in de Azure portal.

#### <a name="to-view-existing-backups"></a>Bestaande back-ups weergeven

1. Ga naar **apparaten** blade. Selecteer en klikt u op uw apparaat. In de **instellingen** blade, gaat u naar **Management > back-upcatalogus**.
   
    ![Navigeer naar de back-upcatalogus](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Geef de volgende criteria moet worden gebruikt voor het filteren van:
   
    - **Tijdsbereik** – kan **afgelopen 1 uur**, **afgelopen 24 uur**, **afgelopen 7 dagen**, **afgelopen 30 dagen**, **afgelopen jaar**, en **aangepaste datum**.
    
    - **Apparaten** – Selecteer in de lijst met bestandsservers of iSCSI-servers geregistreerd bij uw StorSimple-apparaat Manager-service.
   
    - **Geïnitieerd** – automatisch kunnen worden **geplande** (door een back-upbeleid) of **handmatig** gestart (door u).
   
    ![Back-ups filteren](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. Klik op **Toepassen**. De gefilterde lijst met back-ups wordt weergegeven in de **back-upcatalogus** blade. Opmerking alleen 100 back-elementen kunnen worden weergegeven op een bepaald moment.
   
    ![Bijgewerkte back-upcatalogus](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beheer van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

