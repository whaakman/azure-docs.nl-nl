---
title: De StorSimple 8000-apparaat controleren | Microsoft Docs
description: Beschrijft hoe u de service StorSimple Device Manager gebruiken voor het bewaken van gebruik, i/o-prestaties en verbruik van de opslagcapaciteit.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/17/2017
ms.author: alkohli
ms.openlocfilehash: 602514df69977891167f341db0ab20913bcacc9f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60634468"
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-your-storsimple-device"></a>De StorSimple Device Manager-service gebruiken voor het bewaken van uw StorSimple-apparaat

## <a name="overview"></a>Overzicht
U kunt de service StorSimple Device Manager gebruiken voor het bewaken van specifieke apparaten binnen uw StorSimple-oplossing. U kunt aangepaste diagrammen op basis van i/o-prestaties, gebruik van capaciteit, netwerkdoorvoer en maatstaven voor prestaties van apparaat maken en die aan het dashboard vastmaken. Ga voor meer informatie naar [aanpassen van uw portal-dashboard](../azure-portal/azure-portal-dashboards.md).

Als u wilt de controlegegevens voor een specifiek apparaat weergeven in Azure portal, selecteert u de service StorSimple Device Manager. Selecteer uw apparaat uit de lijst met apparaten en gaat u naar **Monitor**. Vervolgens ziet u de **capaciteit**, **gebruik**, en **prestaties** grafieken voor het geselecteerde apparaat.

## <a name="capacity"></a>Capaciteit
**Capaciteit** houdt de ingerichte ruimte en de schijfruimte op het apparaat. De resterende capaciteit wordt vervolgens weergegeven zoals lokaal vastgemaakt of lagen.

De ingerichte en resterende capaciteit is verder opgedeeld per gelaagde en lokaal vastgemaakte volumes. Voor elk volume wordt de ingerichte capaciteit en de resterende capaciteit op het apparaat weergegeven.

![I/o-capaciteit](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Gebruik
**Gebruik** metrische gegevens met betrekking tot de hoeveelheid opslagruimte die wordt gebruikt door de volumes, de volumecontainers of het apparaat wordt bijgehouden. U kunt rapporten op basis van het gebruik van de capaciteit van uw primaire opslag, uw cloudopslag of de apparaatopslag van uw kunt maken. Gebruik van capaciteit kan worden gemeten op een bepaald volume, de container voor een bepaald volume of alle volumecontainers.
Standaard wordt het gebruik voor de afgelopen 24 uur vermeld. U kunt de grafiek om te wijzigen van de duur die het gebruik wordt gerapporteerd door te selecteren in bewerken:
* Afgelopen 24 uur
* Afgelopen 7 dagen
* Afgelopen 30 dagen
* Afgelopen 90 dagen
* Afgelopen jaar

Twee belangrijke metrices, groei en bereik worden gerapporteerd voor het gebruik van grafieken. Bereik verwijst naar de maximale waarde en de minimale waarde van het gebruik gerapporteerd over de geselecteerde periode (fo exemplaar, afgelopen 7 dagen).

Groei verwijst naar de toename in gebruik van de eerste dag tot de laatste dag gedurende de geselecteerde periode. 

Groei en bereik kunnen ook worden voorgesteld door de volgende vergelijkingen:

```
Range = {Usage(minimum), Usage(maximum)}

Growth = Usage(Last day) - Usage(first day)

Growth (%) = [{Usage(last day) - Usage(first day)} X 100]/Usage(first day)
```

De primaire, cloud en lokale opslag die wordt gebruikt, kan als volgt worden beschreven:

### <a name="primary-storage-usage"></a>Gebruik van primaire opslag
Deze diagrammen tonen de hoeveelheid gegevens geschreven naar de StorSimple-volumes voordat de gegevens worden ontdubbeld en gecomprimeerd. U kunt de primaire opslag die wordt gebruikt door alle volumes in een volumecontainer of voor één volume weergeven. De gebruikte primaire opslag verder wordt onderverdeeld op basis van primair gelaagde opslag gebruikt en primaire lokaal vastgemaakte opslag gebruikt.

De volgende diagrammen tonen de primaire opslag gebruikt voor een StorSimple-apparaat voordat en nadat u een cloudmomentopname werd gemaakt. Omdat deze alleen de gegevens van het volume, moet de primaire opslag niet wijzigen in een cloud-momentopname. Zoals u ziet, bevat het diagram geen verschil in de primaire gelaagde of lokaal vastgemaakte opslag gebruikt als gevolg van het maken van een cloudmomentopname. De cloudmomentopname aan de slag om ongeveer 11:50 uur op het apparaat.

![Primaire capaciteitsverbruik na cloud-momentopname](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

Als u i/o nu op de host verbonden is met uw StorSimple-apparaat uitvoeren, u een toename van primair gelaagde opslag ziet of primaire lokaal opslag die wordt gebruikt, afhankelijk vastgemaakte van welke volumes (gelaagd of lokaal vastgemaakt) schrijven u welke gegevens moeten worden. Hier volgen de grafieken voor het gebruik van primaire opslag voor een StorSimple-apparaat. Op dit apparaat voor de StorSimple-host aan de slag voor schrijfbewerkingen in ongeveer 14:30 uur op een gelaagd volume op het apparaat. Hier ziet u de piek in de geschreven bytes per seconde overeenkomt met de i/o die wordt uitgevoerd op de host.

![Prestaties bij i/o die wordt uitgevoerd op volumes gelaagde](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Als u naar de primair gelaagde opslag gebruikt kijkt, die is geworden van dat de primaire lokaal vastgemaakte gebruik blijft ongewijzigd, omdat er geen schrijfbewerkingen naar de lokaal vastgemaakte volumes op het apparaat verzonden.

![Primaire capaciteitsverbruik bij het uitvoeren van i/o op gelaagde volumes](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Als u werkt met Update 3 of hoger, u kunt uitsplitsen naar de primaire verbruik van de opslagcapaciteit afzonderlijk volume, alle volumes, alle gelaagde volumes en alle lokaal vastgemaakte volumes zoals hieronder wordt weergegeven. Afbreken door alle lokaal vastgemaakte volumes kunt u snel nagaan welk deel van de lokale laag is verbruikt.

![Gebruik van de primaire capaciteit voor alle gelaagde volumes](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Gebruik van de primaire capaciteit voor alle lokaal vastgemaakte volumes](./media/storsimple-8000-monitor-device/monitor-usage4.png)

U kunt verder klikt u op elk van de volumes in de lijst en Zie het bijbehorende gebruik.

![Gebruik van de primaire capaciteit voor alle lokaal vastgemaakte volumes](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>Gebruik van cloud-opslag
Deze diagrammen tonen de hoeveelheid opslag in de cloud gebruikt. Deze gegevens worden ontdubbeld en gecomprimeerd. Dit bedrag omvat de cloudmomentopnamen die mogelijk gegevens bevatten die niet wordt weergegeven in een primaire volume en wordt opgeslagen voor bewaren van verouderde of vereist. U kunt de primaire vergelijken en cloud storage verbruik cijfers voor een beter beeld van de frequentie van gegevens beperken, hoewel het nummer niet exact.

De volgende diagrammen tonen het gebruik van de cloud-opslag van een StorSimple-apparaat wanneer een cloudmomentopname werd gemaakt.

* De cloudmomentopname gestart om ongeveer 11:50 uur op het apparaat en kunt u zien dat voor de cloud momentopname, er geen cloudopslag die wordt gebruikt is. 
* Nadat de cloudmomentopname is voltooid, het gebruik van de opslag cloud agenda van 0.89 GB. 
* Terwijl de cloudmomentopname werd uitgevoerd, is er ook een bijbehorende piek in de i/o van het apparaat naar de cloud.

    ![Gebruik van de cloud-opslag voor cloud-momentopname](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Gebruik van de opslag na cloud-momentopname in de cloud](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![I/o van het apparaat naar cloud tijdens een cloud-momentopname](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Gebruik van lokale opslag
Het totale gebruik voor het apparaat dat meer dan primaire opslaggebruik is omdat het de lineaire SSD-laag omvat weergeven deze grafieken Deze laag bevat een hoeveelheid gegevens die ook bestaat op het apparaat de andere lagen. De capaciteit van de SSD-laag voor lineaire volgens zodat wanneer er nieuwe gegevens, wordt de oude gegevens worden verplaatst naar de HDD-laag (op dat moment is ontdubbeld en gecomprimeerd) en vervolgens naar de cloud.

Over tijd, primaire opslag zullen gebruikt en lokale opslag die wordt gebruikt waarschijnlijk toenemen samen totdat de gegevens begint met het naar de cloud in tiers worden verdeeld. Op dat moment wordt de lokale opslag gebruikt zullen waarschijnlijk af te vlakken, maar de gebruikte primaire opslag naarmate er meer gegevens worden geschreven.

De volgende diagrammen tonen de primaire opslag voor een StorSimple-apparaat gebruikt wanneer een cloudmomentopname werd gemaakt. De cloudmomentopname die is begonnen om 11:50 uur en de lokale opslag op dat moment verlagen. De gebruikte lokale opslag is verkeerd omlaag van 1.445 GB tot 1,09 GB. Dit geeft aan dat waarschijnlijk de niet-gecomprimeerde gegevens in de lineaire SSD-laag is ontdubbeld, gecomprimeerd en naar de HDD-laag verplaatst. Houd er rekening mee dat als het apparaat al een grote hoeveelheid gegevens in lagen van de SSD en HDD heeft, u niet deze verkleinen ziet mogelijk. In dit voorbeeld heeft het apparaat een kleine hoeveelheid gegevens.

![Gebruik van de lokale opslag nadat cloud-momentopname](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Prestaties
**Prestaties** houdt metrische gegevens met betrekking tot het aantal lees- en schrijfbewerkingen tussen een de iSCSI-initiator-interfaces op de host-server en het apparaat of het apparaat en de cloud. Deze prestaties kan worden gemeten voor een bepaald volume, de container voor een bepaald volume of alle volumecontainers. Prestaties bevat ook de CPU-gebruik en de netwerkdoorvoer voor de verschillende netwerkinterfaces op uw apparaat.

### <a name="io-performance-for-initiator-to-device"></a>I/o-prestaties voor de initiator naar apparaat
Het onderstaande diagram ziet u de i/o voor de initiator op uw apparaat voor alle volumes voor een productie-apparaat. De metrische gegevens uitgezet worden gelezen en geschreven bytes per seconde. U kunt ook lezen, schrijven en openstaande i/o-, grafiek of lezen en latentie voor schrijfbewerkingen.

![I/o-prestaties voor de initiator naar apparaat](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-to-cloud"></a>I/o-prestaties voor apparaat naar de cloud
Voor hetzelfde apparaat, worden de i/o-bewerkingen voor de gegevens van het apparaat naar de cloud voor alle volumecontainers getekend. Op dit apparaat, de gegevens is alleen in de lineaire laag en niets heeft zich verspreid in de cloud. Er zijn geen lezen / schrijven-bewerkingen plaatsvinden van apparaat naar de cloud. Daarom zijn de pieken in de grafiek op een interval van 5 minuten die overeenkomt met de frequentie waarmee de heartbeat tussen het apparaat en de service wordt gecontroleerd.

Bij hetzelfde apparaat, is een cloud-momentopname voor volumegegevens beginnen om 11:50 uur uitgevoerd. Dit heeft geresulteerd in gegevensstromen van het apparaat naar de cloud. Schrijfbewerkingen zijn geleverd in de cloud in deze duur. De i/o-grafiek bevat een piek in de geschreven Bytes per seconde overeenkomt met de tijd waarop de momentopname is gemaakt.

![I/o van het apparaat naar cloud tijdens een cloud-momentopname](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Netwerkdoorvoer van netwerkinterfaces van apparaat
**De netwerkdoorvoer** metrische gegevens met betrekking tot de hoeveelheid gegevens die worden overgedragen van de netwerkinterfaces van de iSCSI-initiator op de host-server en het apparaat en tussen het apparaat en de cloud worden bijgehouden. U kunt deze metrische gegevens controleren voor elk van de iSCSI-netwerkinterfaces op uw apparaat.

De volgende diagrammen tonen de netwerkdoorvoer voor de Data 0, 1-1 GbE-netwerk op uw apparaat, die beide ingeschakeld voor de cloud (standaard) en iSCSI-zijn ingeschakeld. Op dit apparaat op 14 juni om ongeveer 9 uur, gelaagde gegevens is in de cloud (geen cloudmomentopnamen zijn uitgevoerd op dat moment die verwijst naar meerdere lagen wordt het mechanisme voor het verplaatsen van de gegevens naar de cloud) wat leidde tot i/o naar de cloud wordt geleverd. Er is een bijbehorende piek in de grafiek van netwerk doorvoer voor hetzelfde moment en de meeste van het netwerkverkeer is uitgaand naar de cloud.

![Netwerkdoorvoer voor Data 0](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Als we kijken naar de grafiek Data 1 interface doorvoer, een andere 1 GbE netwerkinterface die is alleen ingeschakeld voor iSCSI, wordt er vrijwel geen netwerkverkeer is in deze duur.

![Netwerkdoorvoer voor gegevens-1](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>CPU-gebruik voor apparaat
**CPU-gebruik** metrische gegevens met betrekking tot de CPU gebruikt op uw apparaat wordt bijgehouden. Het volgende diagram toont de statistieken van de CPU-gebruik voor een apparaat in productie.

![CPU-gebruik voor apparaat](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [het hulpprogramma voor het apparaat van de StorSimple Device Manager-servicedashboard](storsimple-device-dashboard.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

