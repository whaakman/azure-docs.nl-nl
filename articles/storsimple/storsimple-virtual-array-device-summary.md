---
title: StorSimple Virtual Array apparaat overzichtsblade | Microsoft Docs
description: Beschrijving van de blade samenvatting voor StorSimple Device Manager en wordt uitgelegd hoe u deze gebruiken voor het bewaken van de status van uw StorSimple Virtual Array.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: a13c1ea7-6428-4234-84a6-0ebf51670a85
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: manuaery
ms.openlocfilehash: 9edc0b552f5c2f38e646bc4b44dd8df5c16b0457
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408479"
---
# <a name="use-the-device-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>Gebruik de blade samenvatting voor StorSimple Device Manager is verbonden met StorSimple Virtual Array

## <a name="overview"></a>Overzicht

De blade StorSimple Device Manager geeft een overzicht van een StorSimple Virtual Array die is geregistreerd met een opgegeven StorSimple Device Manager markeren die apparaat-problemen die aandacht vereist een systeembeheerder. In deze zelfstudie introduceert de overzichtsblade van het apparaat, wordt de inhoud en de functie uitgelegd en beschrijft de taken die u vanaf deze blade uitvoeren kunt.

De blade samenvatting bevat de volgende informatie:

![Apparaatdashboard](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a>Beheer

In de blade van de StorSimple-apparaat ziet u de opties voor het beheren van uw StorSimple-apparaat. Aan de bovenkant van de blade en aan de linkerkant ziet u de opdrachten voor het beheer. Gebruik deze opties shares of volumes, toevoegen of bijwerken of failover van uw virtuele matrix.

Het gebied essentials bevat enkele belangrijke eigenschappen, zoals de status, model, softwareversie, evenals een koppeling naar de **Webgebruikersinterface** van de matrix. Als u van een intern netwerk gebruikmaakt, kunt u rechtstreeks starten de [lokale web-UI](storsimple-ova-web-ui-admin.md) voor het beheren van uw virtuele matrix.

![Apparaat essentials](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a>Overzicht StorSimple-apparaat

* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen voor uw virtuele array, gegroepeerd op ernst van waarschuwing. Klik op de tegel te openen de **waarschuwingen** blade en klik vervolgens op een afzonderlijke waarschuwing om weer te geven aanvullende informatie over deze waarschuwing, inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.

* De **capaciteit** tegel toont de primaire opslag die is ingericht en resterende binnen het virtuele apparaat ten opzichte van de totale beschikbare opslag voor dezelfde. **Ingericht** verwijst naar de hoeveelheid opslag die is voorbereid en is toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op dit apparaat. De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** de resterende capaciteit op de schijven die zijn gekoppeld aan deze virtuele matrix is.

* In de **gebruik** grafiek, u kunt weergeven die worden gebruikt in uw virtuele array primaire opslag, evenals de opslag in de cloud gebruikt gedurende de afgelopen 7 dagen, de standaard time-outperiode. Gebruik de **bewerken** optie in de rechterbovenhoek van de grafiek naar een ander tijdstip schaal kiezen.

* De **Shares** of **Volumes** tegel bevat een samenvatting van het aantal shares of volumes op uw apparaat op status gegroepeerd. Klik op de tegel te openen de **Shares** of **Volumes** blade lijst en klik vervolgens op een afzonderlijke share of een volume weergeven of wijzigen van de eigenschappen ervan. Zie voor meer informatie over het [beheren van bestandsshares](storsimple-virtual-array-manage-shares.md) of [volumes beheren](storsimple-virtual-array-manage-volumes.md).

## <a name="next-steps"></a>Volgende stappen
Leer hoe u het volgende doet:
- [Beheren van bestandsshares op een StorSimple Virtual Array](storsimple-virtual-array-manage-shares.md)
    
- [Beheren van volumes op een StorSimple Virtual Array](storsimple-virtual-array-manage-volumes.md)

