---
title: Virtuele StorSimple-matrix service samenvatting blade | Microsoft Docs
description: Beschrijft de samenvatting blade service voor StorSimple Apparaatbeheer en wordt uitgelegd hoe u hiermee de status van uw virtuele StorSimple-matrix.
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 8a2b9a84-b0e6-48b9-b366-d16f004241a5
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 284e404c44505a98d9e0ed5abe87cd945415af56
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-service-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>Gebruik de service samenvatting blade voor StorSimple Apparaatbeheer verbonden met virtuele StorSimple-matrix
## <a name="overview"></a>Overzicht
De service samenvatting blade voor de StorSimple-Apparaatbeheer geeft een overzicht van de StorSimple virtuele matrices (ook wel bekend als StorSimple lokale virtuele apparaten of virtuele apparaten) die zijn verbonden met uw service markeren die aandacht vereisen een systeembeheerder. In deze zelfstudie maakt u kennis met de service samenvatting blade, wordt de inhoud en de functie uitgelegd en beschrijft de taken die u vanuit deze blade uitvoeren kunt.

![Servicedashboard](./media/storsimple-virtual-array-service-summary/service-blade.png)

## <a name="management-commands-and-essentials"></a>Opdrachten voor het beheer en essentials
In de blade van de StorSimple-overzicht ziet u de opties voor het beheren van uw StorSimple-apparaat Manager-service, evenals de virtuele matrices die zijn geregistreerd bij deze service. Ziet u de opdrachten voor het beheer aan de bovenkant van de blade en aan de linkerkant.

Gebruik deze opties om verschillende bewerkingen uitvoeren, zoals bestandsshares of volumes toevoegen, of de monitor de verschillende taken die worden uitgevoerd op de virtuele-matrices.

Het gebied essentials worden enkele belangrijke eigenschappen zoals de resourcegroep, de locatie en het abonnement waarin uw StorSimple-apparaat Manager is gemaakt.

## <a name="storsimple-device-manager-service-summary"></a>Serviceoverzicht StorSimple-Apparaatbeheer
* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen voor alle virtuele apparaten gegroepeerd op de ernst van waarschuwing. Op de tegel klikt, wordt de **waarschuwingen** blade waar u kunt klikken op een afzonderlijke waarschuwing om aanvullende informatie over deze waarschuwing weer te geven inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.
* De **capaciteit** tegel geeft de primaire opslag die is ingericht en resterende op alle virtuele apparaten ten opzichte van de totale opslagruimte beschikbaar op alle virtuele apparaten bevat. **Ingericht** verwijst naar de hoeveelheid opslagruimte die is voorbereid en toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op alle virtuele apparaten. De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** is de resterende capaciteit op de schijven die zijn gekoppeld aan de virtuele-matrices.
* In de **gebruik** grafiek, kunt u de relevante metrische gegevens voor uw virtuele apparaten bekijken. U kunt de primaire opslagruimte gebruikt op alle virtuele apparaten, evenals de cloudopslag die wordt gebruikt door virtuele apparaten gedurende de afgelopen 7 dagen, de standaardwaarde periode bekijken. Gebruik de **bewerken** optie in de rechterbovenhoek van de grafiek naar een ander tijdstip schaal kiezen.
* De **apparaten** tegel bevat een samenvatting van het aantal virtuele matrices in uw Apparaatbeheer van StorSimple gegroepeerd op de apparaatstatus. Klik op deze tegel openen de **apparaten** blade lijst en klik vervolgens op een afzonderlijk apparaat om te zoomen op de samenvatting voor apparaten die specifiek zijn voor het apparaat. U kunt specifieke apparaatacties ook uitvoeren vanaf een bepaald apparaat samenvatting blade. Voor meer informatie over de samenvatting blade apparaat, gaat u naar [apparaat samenvatting blade](storsimple-virtual-array-device-summary.md).

## <a name="view-the-activity-logs"></a>Bekijk de activiteitenlogboeken
Als u wilt weergeven van de verschillende bewerkingen uitgevoerd binnen uw StorSimple-Apparaatbeheer de **activiteitenlogboeken** koppeling aan de linkerkant van de blade voor een overzicht van het StorSimple-service. Hiermee gaat u naar de **activiteitenlogboeken** blade waar u een van de recente bewerkingen uitgevoerd overzicht.

![Activiteitenlogboeken](./media/storsimple-virtual-array-service-summary/activity-log.png)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [gebruiken van de lokale webgebruikersinterface voor het beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

