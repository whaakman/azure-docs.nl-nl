---
title: Virtuele StorSimple-matrix apparaat samenvatting blade | Microsoft Docs
description: Beschrijft de samenvatting blade apparaat voor StorSimple Apparaatbeheer en wordt uitgelegd hoe u hiermee de status van uw virtuele StorSimple-matrix.
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: a13c1ea7-6428-4234-84a6-0ebf51670a85
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: manuaery
ms.openlocfilehash: 35413d597c3b6b1c7600241a78572b63f982d175
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-device-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>Gebruik de samenvatting blade apparaat voor StorSimple Apparaatbeheer verbonden met virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

De blade Apparaatbeheer StorSimple-apparaat wordt een samenvatting weergegeven van een virtueel StorSimple-matrix die is geregistreerd met een bepaalde StorSimple Apparaatbeheer markeren die apparaat-problemen die aandacht vereisen een systeembeheerder. In deze zelfstudie maakt u kennis met de samenvatting blade apparaat, wordt de inhoud en de functie wordt uitgelegd en beschrijft de taken die u vanuit deze blade uitvoeren kunt.

De samenvatting apparaat-blade bevat de volgende informatie:

![Apparaat-dashboard](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a>Beheer

In de blade StorSimple-apparaat ziet u de opties voor het beheren van uw StorSimple-apparaat. Ziet u de opdrachten voor het beheer aan de bovenkant van de blade en aan de linkerkant. Gebruik deze opties shares of volumes, toevoegen of bijwerken of failover van uw virtuele matrix.

Het gebied essentials bevat enkele belangrijke eigenschappen zoals de status, model, versie, evenals een koppeling naar de **Webgebruikersinterface** van de matrix. Als u zich op een intern netwerk, kunt u rechtstreeks starten de [lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md) voor het beheren van uw virtuele matrix.

![Apparaat essentials](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a>Samenvatting StorSimple-apparaat

* De **waarschuwingen** tegel biedt een momentopname van de actieve waarschuwingen voor uw virtuele matrix gegroepeerd op de ernst van waarschuwing. Klik op de tegel openen de **waarschuwingen** blade en klik vervolgens op een afzonderlijke waarschuwing u meer details over deze waarschuwing, inclusief alle aanbevolen acties. U kunt ook de waarschuwing wissen als het probleem is opgelost.

* De **capaciteit** tegel worden weergegeven voor de primaire opslag die is ingericht en resterende in het virtuele apparaat ten opzichte van de totale opslag beschikbaar voor dezelfde. **Ingericht** verwijst naar de hoeveelheid opslagruimte die is voorbereid en toegewezen voor gebruik, **resterend** verwijst naar de resterende capaciteit die kan worden ingericht op dit apparaat. De **resterende gelaagde** capaciteit is de beschikbare capaciteit die kan worden ingericht met inbegrip van de cloud, terwijl de **resterende lokale** is de resterende capaciteit op de schijven die aan deze virtuele matrix is gekoppeld.

* In de **gebruik** grafiek, vindt u de primaire opslag in uw virtuele matrix gebruikt, evenals de cloudopslag verbruikt gedurende de afgelopen 7 dagen, de standaard time-outperiode. Gebruik de **bewerken** optie in de rechterbovenhoek van de grafiek naar een ander tijdstip schaal kiezen.

* De **Shares** of **Volumes** tegel bevat een samenvatting van het aantal shares of volumes op uw apparaat op status gegroepeerd. Klik op de tegel openen de **Shares** of **Volumes** blade lijst en klik vervolgens op een afzonderlijke share of het volume weergeven of wijzigen van de eigenschappen ervan. Zie voor meer informatie hoe [shares kunt beheren](storsimple-virtual-array-manage-shares.md) of [volumes beheren](storsimple-virtual-array-manage-volumes.md).

## <a name="next-steps"></a>Volgende stappen
Leer hoe u het volgende doet:
- [Shares op een virtueel StorSimple-matrix beheren](storsimple-virtual-array-manage-shares.md)
    
- [Beheren van volumes op een virtueel StorSimple-matrix](storsimple-virtual-array-manage-volumes.md)

