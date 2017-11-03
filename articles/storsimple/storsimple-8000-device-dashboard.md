---
title: Gebruik StorSimple 8000 series apparaatoverzicht | Microsoft Docs
description: Beschrijft de Apparaatbeheer StorSimple-apparaat samenvatting en hoe u met metrische gegevens storage en verbonden initiators weergeven en het serienummer en de IQN vinden.
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 784d3ce9d8f926b00ac1c6fbf48a05c0b04f900a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a>Het apparaat in Apparaatbeheer StorSimple-service samenvatting gebruiken

## <a name="overview"></a>Overzicht
De samenvatting blade voor StorSimple-apparaat biedt u een overzicht van informatie voor een specifiek StorSimple-apparaat, in tegenstelling tot de samenvatting blade service, waarmee u informatie over de apparaten die zijn opgenomen in de Microsoft Azure StorSimple-oplossing.

De samenvatting blade apparaat geeft een overzicht van een StorSimple 8000 serie-apparaat dat is geregistreerd bij een gegeven StorSimple apparaat Manager markeren die apparaat-problemen die aandacht vereisen een systeembeheerder. In deze zelfstudie maakt u kennis met de samenvatting blade apparaat, wordt de inhoud en de functie wordt uitgelegd en beschrijft de taken die u vanuit deze blade uitvoeren kunt.

De samenvatting apparaat-blade bevat de volgende informatie:

![Samenvatting blade apparaat](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Opdrachtbalk Management

In de blade StorSimple-apparaat ziet u de opties voor het beheren van uw StorSimple-apparaat. Ziet u de opdrachten voor het beheer aan de bovenkant van de blade en aan de linkerkant. Gebruik deze opties shares of volumes, toevoegen of bijwerken of failover van uw apparaat.

![Opdrachtbalk Management](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Essentials

Het gebied essentials bevat enkele van de belangrijke eigenschappen zoals, de status, model, doel IQN en versie van de software. 

![Apparaat essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>Bewaking

* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen voor uw apparaat, gegroepeerd op de ernst van waarschuwing.

    ![Waarschuwing tegel](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Klik op de tegel openen de **waarschuwingen** blade en klik vervolgens op een afzonderlijke waarschuwing u meer details over deze waarschuwing, inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.

    ![Klik op de tegel waarschuwingen](./media/storsimple-8000-device-dashboard/device-summary10.png)

* De **Status en gezondheid** tegel biedt inzicht in de status van de hardware-onderdeel voor een apparaat met inbegrip van de status van het apparaat. Status van het apparaat is mogelijk offline, online, gedeactiveerd of klaar om te stellen.

    ![Tegel status en gezondheid](./media/storsimple-8000-device-dashboard/device-summary5.png)

* De **Volumes** tegel bevat een samenvatting van het aantal volumes in uw apparaat op status gegroepeerd.

    ![Tegel volumes](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Klik op de tegel openen de **Volumes** blade lijst en klik vervolgens op een afzonderlijke volume weergeven of wijzigen van de eigenschappen ervan.
    
    ![Klik op de tegel volumes](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Zie voor meer informatie hoe [volumes beheren](storsimple-8000-manage-volumes-u2.md).

* In de **gebruik** grafiek, vindt u de primaire opslag in uw apparaat gebruikt en de cloudopslag verbruikt gedurende de afgelopen 7 dagen, de standaard time-outperiode.

     ![Tegel gebruik](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     Als u een ander tijdstip schaal, gebruiken de **bewerken** optie in de rechterbovenhoek van de grafiek.

     ![Gebruiksgrafiek bewerken](./media/storsimple-8000-device-dashboard/device-summary12.png)

     U kunt metrische gegevens voor de totale primaire opslag (de hoeveelheid gegevens die zijn geschreven door hosts op uw apparaat) en de totale cloudopslag verbruikt door uw apparaat gedurende een periode weergeven in deze grafiek.
  
     In deze context *primaire opslag* verwijst naar de totale hoeveelheid gegevens die zijn geschreven door de host en door volumetype kunnen worden onderverdeeld: *primaire gelaagde opslag* bevat zowel lokaal opgeslagen gegevens en lagen naar de cloud. *Primaire lokaal vastgemaakt opslag* bevat alleen gegevens die lokaal zijn opgeslagen. *Cloudopslag*, aan de andere kant is een meting van de totale hoeveelheid gegevens die zijn opgeslagen in de cloud. Deze opslag bevat gelaagde gegevens en back-ups. De gegevens die zijn opgeslagen in de cloud is ontdubbeld en gecomprimeerd, terwijl primaire opslag de hoeveelheid opslagruimte gebruikt geeft voordat de gegevens worden ontdubbeld en worden gecomprimeerd. (U kunt deze twee getallen om een beter beeld van de snelheid met compressie vergelijken.) Voor zowel de primaire en cloud-opslag, de bedragen die zijn gebaseerd op de bijhouden frequentie die u configureert. Als u een frequentie van één week kiest, klikt u vervolgens ziet het diagram u bijvoorbeeld gegevens voor elke dag in de vorige week.

     Overzicht van de hoeveelheid verbruikte gedurende een periode van cloud-opslag, selecteer de **CLOUD-opslag gebruikt** optie. Overzicht van de totale opslag die is geschreven door de host, selecteert u de **primaire GELAAGDE opslag gebruikt** en **primaire lokaal VASTGEMAAKT opslag gebruikt** opties. 
     Zie voor meer informatie [de Apparaatbeheer StorSimple-service gebruiken voor het bewaken van uw StorSimple-apparaat](storsimple-monitor-device.md).


* De **capaciteit** tegel worden weergegeven voor de primaire opslag die is ingericht en resterende op het apparaat ten opzichte van de totale opslag beschikbaar voor dezelfde. **Ingericht** verwijst naar de hoeveelheid opslagruimte die is voorbereid en toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op dit apparaat. 

    ![Tegel gebruik](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Klik op deze tegel om weer te geven hoe de capaciteit is ingericht op de gelaagde en lokaal vastgemaakte volumes. De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** is de resterende capaciteit op de schijven die zijn gekoppeld aan dit apparaat.

    ![Klik op de gebruiksgrafiek](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [StorSimple-service samenvatting blade](storsimple-8000-service-dashboard.md).
* Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

