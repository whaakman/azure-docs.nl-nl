---
title: Gebruik StorSimple 8000 series apparaatoverzicht | Microsoft Docs
description: Beschrijving van de samenvatting blade voor StorSimple-service en wordt uitgelegd hoe u hiermee de status van uw StorSimple-oplossing.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: d987a4ae170f21532a886552cbe1eb5a0d25fc3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a>De service samenvatting blade voor StorSimple 8000 series apparaat gebruiken

## <a name="overview"></a>Overzicht

De samenvatting blade voor Apparaatbeheer van StorSimple-service geeft een overzicht van de apparaten die zijn verbonden met de Apparaatbeheer StorSimple-service, die apparaten die aandacht vereisen een systeembeheerder is gemarkeerd. Deze zelfstudie maakt u kennis met de service samenvatting blade, worden de Dashboardinhoud en de functie en beschrijft de taken die u op deze pagina uitvoeren kunt.

![Serviceoverzicht](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a>Opdrachten voor beheer

In de StorSimple-service samenvatting blade ziet u de opties voor het beheren van uw StorSimple-apparaat Manager-service en de apparaten van het StorSimple 8000 serie geregistreerd bij deze service. Ziet u de opdrachten voor het beheer aan de bovenkant van de blade en aan de linkerkant.

![Opdrachtbalk](./media/storsimple-8000-service-dashboard/service-summary2.png)

Gebruik deze opties verschillende bewerkingen uitvoeren zoals shares of volumes of monitor voor de verschillende taken die worden uitgevoerd op het StorSimple-apparaten toevoegen.


## <a name="essentials"></a>Essentials

Het gebied essentials worden enkele belangrijke eigenschappen zoals de resourcegroep, de locatie en het abonnement waarin uw StorSimple-apparaat Manager is gemaakt.

![Essentials](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a>Serviceoverzicht StorSimple-Apparaatbeheer

* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen op alle apparaten, gegroepeerd op de ernst van waarschuwing.

    ![Tegel waarschuwingen](./media/storsimple-8000-service-dashboard/service-summary4.png)

    Op de tegel klikt, wordt de **waarschuwingen** blade waar u kunt klikken op een afzonderlijke waarschuwing om aanvullende informatie over deze waarschuwing weer te geven inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.

    ![Klik op de tegel waarschuwingen](./media/storsimple-8000-service-dashboard/service-summary8.png)

* De **capaciteit** tegel geeft de primaire opslag die is ingericht en resterende op alle apparaten ten opzichte van de totale opslagruimte beschikbaar op alle apparaten bevat. **Ingericht** verwijst naar de hoeveelheid opslagruimte die is voorbereid en toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op alle apparaten.

    ![Capaciteit tegel](./media/storsimple-8000-service-dashboard/service-summary6.png)

    De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** is de resterende capaciteit op de schijven die zijn gekoppeld aan apparaten uit de StorSimple 8000 serie.


* In de **gebruik** grafiek, kunt u de relevante metrische gegevens voor uw apparaten bekijken. U kunt de primaire opslagruimte gebruikt op alle apparaten en de cloudopslag die wordt gebruikt door apparaten gedurende de afgelopen 7 dagen, de standaardwaarde periode weergeven. 

    ![Tegel gebruik](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    Als u een ander tijdstip schaal, gebruiken de **bewerken** optie in de rechterbovenhoek van de grafiek.

     ![Klik op de tegel gebruik](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![De grafiekgegevens exporteren](./media/storsimple-8000-service-dashboard/service-summary11.png)

* De **apparaten** tegel bevat een samenvatting van het aantal StorSimple 8000 series apparaten in uw Apparaatbeheer van StorSimple gegroepeerd op de apparaatstatus. 

    ![Apparaten tegel](./media/storsimple-8000-service-dashboard/service-summary5.png)

    Klik op deze tegel openen de **apparaten** blade lijst en klik vervolgens op een afzonderlijk apparaat om te zoomen op de samenvatting voor apparaten die specifiek zijn voor het apparaat. U kunt ook apparaatspecifieke acties uitvoeren vanaf een bepaald apparaat samenvatting blade. Voor meer informatie over de samenvatting blade apparaat, gaat u naar [apparaat samenvatting blade](storsimple-8000-device-dashboard.md).

    ![Klik op de tegel apparaten](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a>Bekijk de activiteitenlogboeken

Als u wilt weergeven van de verschillende bewerkingen uitgevoerd binnen uw StorSimple-Apparaatbeheer de **activiteitenlogboeken** koppeling aan de linkerkant van de blade voor een overzicht van het StorSimple-service. Hiermee gaat u naar de **activiteitenlogboeken** blade waar u een van de recente bewerkingen uitgevoerd overzicht.

![Activiteitenlogboeken](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

