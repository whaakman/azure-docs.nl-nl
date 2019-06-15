---
title: StorSimple 8000-serie apparaat gebruiken samenvatting | Microsoft Docs
description: Beschrijving van het overzicht StorSimple Device Manager serviceapparaat en hoe u kunt metrische gegevens van storage en verbonden initiators weergeven en het serienummer en de IQN vinden.
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 1d88af2c0739c30b2562bad7660015b890e8159c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60578219"
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a>De apparaat-overzicht in StorSimple Device Manager-service gebruiken

## <a name="overview"></a>Overzicht
De overzichtsblade van de StorSimple-apparaat biedt u een overzicht van informatie voor een specifiek StorSimple-apparaat, in tegenstelling tot de overzichtsblade service, waardoor u informatie over de apparaten die zijn opgenomen in de Microsoft Azure StorSimple-oplossing.

De blade samenvatting geeft een overzicht van een StorSimple 8000-apparaat dat is geregistreerd met een opgegeven StorSimple Device Manager markeren die apparaat-problemen die aandacht vereist een systeembeheerder. In deze zelfstudie introduceert de overzichtsblade van het apparaat, wordt de inhoud en de functie uitgelegd en beschrijft de taken die u vanaf deze blade uitvoeren kunt.

De blade samenvatting bevat de volgende informatie:

![De overzichtsblade van apparaat](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Management-opdrachtbalk

In de blade van de StorSimple-apparaat ziet u de opties voor het beheren van uw StorSimple-apparaat. Aan de bovenkant van de blade en aan de linkerkant ziet u de opdrachten voor het beheer. Gebruik deze opties shares of volumes, toevoegen of bijwerken of failover van uw apparaat.

![Management-opdrachtbalk](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Essentials

Het gebied essentials bevat enkele van de belangrijke eigenschappen zoals de status, model, IQN van het doel en versie van de software. 

![Apparaat essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>Bewaking

* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen voor uw apparaat, gegroepeerd op ernst van waarschuwing.

    ![Waarschuwing tegel](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Klik op de tegel te openen de **waarschuwingen** blade en klik vervolgens op een afzonderlijke waarschuwing om weer te geven aanvullende informatie over deze waarschuwing, inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.

    ![Klik op de tegel waarschuwingen](./media/storsimple-8000-device-dashboard/device-summary10.png)

* De **Status en integriteit** tegel geeft inzicht in de status van de hardware-onderdeel voor een apparaat met inbegrip van de status van het apparaat. Status van het apparaat is mogelijk offline, online, gedeactiveerde of klaar om in te stellen.

    ![Tegel status en gezondheid](./media/storsimple-8000-device-dashboard/device-summary5.png)

* De **Volumes** tegel bevat een samenvatting van het aantal volumes op uw apparaat op status gegroepeerd.

    ![Tegel volumes](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Klik op de tegel te openen de **Volumes** blade lijst en klik vervolgens op een afzonderlijke volume weergeven of wijzigen van de eigenschappen ervan.
    
    ![Klik op de tegel volumes](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Zie voor meer informatie over het [volumes beheren](storsimple-8000-manage-volumes-u2.md).

* In de **gebruik** grafiek, vindt u de primaire opslag die wordt gebruikt op uw apparaat en de opslag in de cloud gebruikt gedurende de afgelopen 7 dagen, de standaard time-outperiode.

     ![Gebruikstegel](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     Als u de schaal van een ander tijdstip, gebruikt u de **bewerken** optie in de rechterbovenhoek van de grafiek.

     ![Gebruiksdiagram bewerken](./media/storsimple-8000-device-dashboard/device-summary12.png)

     In deze grafiek, kunt u metrische gegevens voor de totale primaire opslag (de hoeveelheid gegevens die zijn geschreven door hosts met het apparaat) en de totale cloudopslag die door uw apparaat gedurende een bepaalde periode bekijken.
  
     In deze context *primaire opslag* verwijst naar de totale hoeveelheid gegevens die door de host wordt geschreven en kunnen worden onderverdeeld op basis van volumetype: *primair gelaagde opslag* zowel lokaal opgeslagen gegevens en gelaagde in de cloud. *Primaire lokaal vastgemaakte opslag* bevat alleen gegevens die lokaal zijn opgeslagen. *Cloudopslag*, daarentegen, is een meting van de totale hoeveelheid gegevens die zijn opgeslagen in de cloud. Deze opslag omvat gelaagde gegevens en back-ups. De gegevens die zijn opgeslagen in de cloud is ontdubbeld en gecomprimeerd, terwijl primaire opslag de hoeveelheid opslag die wordt gebruikt geeft voordat de gegevens worden ontdubbeld en gecomprimeerd. (U kunt deze twee getallen om een beter beeld van de snelheid met compressie kunt vergelijken.) Voor zowel de primaire en opslag in de cloud, de bedragen die zijn gebaseerd op de tracerings-frequentie die u configureert. Als u ervoor een frequentie van één week kiest, klikt u vervolgens ziet de grafiek u bijvoorbeeld gegevens voor elke dag in de vorige week.

     Als u wilt zien van de hoeveelheid opslag in de cloud die na verloop van tijd worden gebruikt, selecteert u de **CLOUD-opslag gebruikt** optie. Als u wilt zien van de totale opslag die is geschreven door de host, selecteer de **PRIMAIR GELAAGDE opslag gebruikt** en **primaire lokaal VASTGEMAAKTE opslag gebruikt** opties. 
     Zie voor meer informatie, [de StorSimple Device Manager-service gebruiken voor het bewaken van uw StorSimple-apparaat](storsimple-monitor-device.md).


* De **capaciteit** tegel toont de primaire opslag die is ingericht en resterende voor het apparaat ten opzichte van de totale beschikbare opslag voor dezelfde. **Ingericht** verwijst naar de hoeveelheid opslag die is voorbereid en is toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op dit apparaat. 

    ![Gebruikstegel](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Klik op deze tegel om te bekijken hoe de capaciteit is ingericht voor gelaagde en lokaal vastgemaakte volumes. De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** is de resterende capaciteit op de schijven die zijn gekoppeld aan dit apparaat.

    ![Klik op de grafiek van het gebruik](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [overzichtsblade van de service StorSimple](storsimple-8000-service-dashboard.md).
* Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

