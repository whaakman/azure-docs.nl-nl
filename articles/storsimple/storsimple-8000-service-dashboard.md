---
title: StorSimple 8000-serie apparaat gebruiken samenvatting | Microsoft Docs
description: Beschrijving van de overzichtsblade van de StorSimple-service en wordt uitgelegd hoe u deze gebruiken voor het bewaken van de status van uw StorSimple-oplossing.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: c174f6ce0fb3d40af953be205a7bfcca60fbfeec
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60632973"
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a>Gebruik de overzichtsblade van de service voor StorSimple 8000-apparaat

## <a name="overview"></a>Overzicht

De overzichtsblade van de StorSimple Device Manager-service wordt een samenvatting weergegeven van alle apparaten die zijn verbonden met de StorSimple Device Manager-service, die apparaten waarvoor aandacht is een systeembeheerder markeren. In deze zelfstudie bevat de overzichtsblade van de service, wordt uitgelegd van het Dashboardinhoud en de functie en beschrijft de taken die u op deze pagina uitvoeren kunt.

![Serviceoverzicht](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a>Opdrachten voor beheer

In de service overzichtsblade van StorSimple ziet u de opties voor het beheren van uw StorSimple Device Manager-service en de StorSimple 8000-serie apparaten die zijn geregistreerd bij deze service. Aan de bovenkant van de blade en aan de linkerkant ziet u de opdrachten voor het beheer.

![Opdrachtbalk](./media/storsimple-8000-service-dashboard/service-summary2.png)

Gebruik deze opties verschillende bewerkingen uit te voeren, zoals bestandsshares of volumes toevoegen, of een monitor die de verschillende taken die worden uitgevoerd op het StorSimple-apparaten.


## <a name="essentials"></a>Essentials

Het gebied essentials bevat enkele belangrijke eigenschappen, zoals de resourcegroep, de locatie en het abonnement waarin uw StorSimple Device Manager is gemaakt.

![Essentials](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a>Serviceoverzicht van de StorSimple Device Manager

* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen voor alle apparaten, gegroepeerd op ernst van waarschuwing.

    ![Tegel waarschuwingen](./media/storsimple-8000-service-dashboard/service-summary4.png)

    Hiermee opent u de tegel te klikken op de **waarschuwingen** blade waar u kunt klikken op een afzonderlijke waarschuwing om weer te geven aanvullende informatie over deze waarschuwing, inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.

    ![Klik op de tegel waarschuwingen](./media/storsimple-8000-service-dashboard/service-summary8.png)

* De **capaciteit** tegel geeft de primaire opslag die is ingericht en resterende voor alle apparaten ten opzichte van de totale beschikbare opslag voor alle apparaten bevat. **Ingericht** verwijst naar de hoeveelheid opslag die is voorbereid en is toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op alle apparaten.

    ![Capaciteit tegel](./media/storsimple-8000-service-dashboard/service-summary6.png)

    De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** is de resterende capaciteit op de schijven die zijn gekoppeld aan de StorSimple 8000 reeks apparaten.


* In de **gebruik** grafiek, kunt u de relevante metrische gegevens voor uw apparaten bekijken. U kunt de primaire opslag die wordt gebruikt voor alle apparaten en de cloudopslag die wordt gebruikt door apparaten gedurende de afgelopen 7 dagen, de standaard periode bekijken. 

    ![Gebruikstegel](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    Als u de schaal van een ander tijdstip, gebruikt u de **bewerken** optie in de rechterbovenhoek van de grafiek.

     ![Klik op de tegel usage](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![Grafiekgegevens exporteren](./media/storsimple-8000-service-dashboard/service-summary11.png)

* De **apparaten** tegel bevat een samenvatting van het aantal apparaten uit StorSimple 8000-serie in uw StorSimple-Apparaatbeheerfunctie gegroepeerd op de status van apparaat. 

    ![Tegel apparaten](./media/storsimple-8000-service-dashboard/service-summary5.png)

    Klik op deze tegel te openen de **apparaten** blade lijst en klik vervolgens op een afzonderlijk apparaat om te zoomen op de samenvatting voor apparaten die specifiek zijn voor het apparaat. U kunt ook de apparaat-specifieke acties uitvoeren vanuit de overzichtsblade van een bepaald apparaat. Voor meer informatie over de overzichtsblade van het apparaat, gaat u naar [de overzichtsblade van Device](storsimple-8000-device-dashboard.md).

    ![Klik op de tegel apparaten](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a>De activiteitenlogboeken bekijken

Als u de verschillende bewerkingen uitgevoerd binnen uw StorSimple Device Manager, klikt u op de **activiteitenlogboeken** koppelen aan de linkerkant van de overzichtsblade van uw StorSimple-service. Hiermee gaat u naar de **activiteitenlogboeken** blade waar u een overzicht van de recente bewerkingen uitgevoerd kunt zien.

![Activiteitenlogboeken](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

