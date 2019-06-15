---
title: Overzichtsblade van de service StorSimple Virtual Array | Microsoft Docs
description: Beschrijft de overzichtsblade van de service voor StorSimple Device Manager en wordt uitgelegd hoe u deze gebruiken voor het bewaken van de status van uw StorSimple Virtual Array.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 8a2b9a84-b0e6-48b9-b366-d16f004241a5
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9c05bddaeb3c34400db1ec75c624ef00a85d9444
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720730"
---
# <a name="use-the-service-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>Gebruik de overzichtsblade van de service voor StorSimple Device Manager is verbonden met StorSimple Virtual Array
## <a name="overview"></a>Overzicht
De overzichtsblade van de service voor de StorSimple Device Manager geeft een overzicht van de virtuele StorSimple-matrices (ook wel bekend als StorSimple on-premises virtuele apparaten of virtuele apparaten) die zijn verbonden met uw service, die een systeem moet markeren beheerder van uw aandacht. In deze zelfstudie introduceert de overzichtsblade van de service, wordt de inhoud en de functie uitgelegd en beschrijft de taken die u vanaf deze blade uitvoeren kunt.

![Servicedashboard](./media/storsimple-virtual-array-service-summary/service-blade.png)

## <a name="management-commands-and-essentials"></a>Opdrachten voor het beheer- en essentials
In de overzichtsblade van StorSimple ziet u de opties voor het beheren van uw StorSimple Device Manager-service, evenals de virtuele matrices die zijn geregistreerd bij deze service. Aan de bovenkant van de blade en aan de linkerkant ziet u de opdrachten voor het beheer.

Gebruik deze opties verschillende bewerkingen uit te voeren, zoals bestandsshares of volumes toevoegen, of een monitor die de verschillende taken die worden uitgevoerd op de virtuele matrices.

Het gebied essentials bevat enkele belangrijke eigenschappen, zoals de resourcegroep, de locatie en het abonnement waarin uw StorSimple Device Manager is gemaakt.

## <a name="storsimple-device-manager-service-summary"></a>Serviceoverzicht van de StorSimple Device Manager
* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen voor alle virtuele apparaten, gegroepeerd op ernst van waarschuwing. Hiermee opent u de tegel te klikken op de **waarschuwingen** blade waar u kunt klikken op een afzonderlijke waarschuwing om weer te geven aanvullende informatie over deze waarschuwing, inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.
* De **capaciteit** tegel geeft ziet u de primaire opslag die is ingericht en resterende voor alle virtuele apparaten ten opzichte van de totale beschikbare opslag voor alle virtuele apparaten. **Ingericht** verwijst naar de hoeveelheid opslag die is voorbereid en is toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op alle virtuele apparaten. De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** is de resterende capaciteit op de schijven die zijn gekoppeld aan de virtuele matrices.
* In de **gebruik** diagram ziet u de relevante metrische gegevens voor uw virtuele apparaten. U kunt de primaire opslag die wordt gebruikt voor alle virtuele apparaten, evenals de cloudopslag die wordt gebruikt door virtuele apparaten gedurende de afgelopen 7 dagen, de standaard periode bekijken. Gebruik de **bewerken** optie in de rechterbovenhoek van de grafiek naar een ander tijdstip schaal kiezen.
* De **apparaten** tegel bevat een samenvatting van het aantal virtuele matrices in uw StorSimple-Apparaatbeheerfunctie gegroepeerd op de status van apparaat. Klik op deze tegel te openen de **apparaten** blade lijst en klik vervolgens op een afzonderlijk apparaat om te zoomen op de samenvatting voor apparaten die specifiek zijn voor het apparaat. U kunt ook specifieke apparaatacties uitvoeren vanaf de overzichtsblade van een bepaald apparaat. Voor meer informatie over de overzichtsblade van het apparaat, gaat u naar [de overzichtsblade van Device](storsimple-virtual-array-device-summary.md).

## <a name="view-the-activity-logs"></a>De activiteitenlogboeken bekijken
Als u de verschillende bewerkingen uitgevoerd binnen uw StorSimple Device Manager, klikt u op de **activiteitenlogboeken** koppelen aan de linkerkant van de overzichtsblade van uw StorSimple-service. Hiermee gaat u naar de **activiteitenlogboeken** blade waar u een overzicht van de recente bewerkingen uitgevoerd kunt zien.

![Activiteitenlogboeken](./media/storsimple-virtual-array-service-summary/activity-log.png)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [gebruiken van de lokale webgebruikersinterface voor het beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

