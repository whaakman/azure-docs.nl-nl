---
title: Bewaken van uw StorSimple 8000 series apparaat | Microsoft Docs
description: Beschrijft hoe de service Manager voor StorSimple-apparaat gebruiken om te controleren van informatie over het gebruik, i/o-prestaties en gebruik van capaciteit.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/17/2017
ms.author: alkohli
ms.openlocfilehash: 679c1fc8775ad4481bc99c9aea79fe16e9bcac8f
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-your-storsimple-device"></a>De service Manager voor StorSimple-apparaat gebruiken voor het bewaken van uw StorSimple-apparaat

## <a name="overview"></a>Overzicht
U kunt de service Manager voor StorSimple-apparaat gebruiken voor het bewaken van specifieke apparaten binnen uw StorSimple-oplossing. U kunt aangepaste grafieken op basis van i/o-prestaties, gebruik van capaciteit, netwerkdoorvoer en maatstaven voor prestaties apparaat maken en deze naar het dashboard vastmaken. Ga voor meer informatie naar [portal dashboard aanpassen](../azure-portal/azure-portal-dashboards.md).

Als u de bewaking informatie voor een specifiek apparaat in de Azure portal, selecteer de Apparaatbeheer StorSimple-service. Selecteer het apparaat uit de lijst met apparaten en gaat u naar **Monitor**. Vervolgens ziet u de **capaciteit**, **gebruik**, en **prestaties** grafieken voor het geselecteerde apparaat.

## <a name="capacity"></a>Capaciteit
**Capaciteit** houdt de ingerichte ruimte en de schijfruimte op het apparaat berekend. De resterende capaciteit wordt vervolgens weergegeven als lokaal vastgemaakt of lagen.

De capaciteit ingericht en resterende is verder uitgesplitst gelaagde en lokaal vastgemaakte volumes. Voor elk volume wordt de ingerichte capaciteit en de resterende capaciteit op het apparaat weergegeven.

![I/o-capaciteit](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Gebruik
**Gebruik** houdt metrische gegevens die betrekking hebben op de hoeveelheid opslagruimte die wordt gebruikt door de volumes, de volumecontainers of het apparaat. U kunt rapporten op basis van het gebruik van de capaciteit van uw primaire opslag, uw cloud-opslag of uw opslagruimte van het apparaat kunt maken. Capaciteitsverbruik kan op een bepaald volume, een specifieke volumecontainer of alle volumecontainers worden gemeten.
Standaard wordt het gebruik voor de afgelopen 24 uur gerapporteerd. U kunt wijzigen van de duur gedurende welke de informatie over het gebruik wordt gerapporteerd door te selecteren in de grafiek bewerken:
* Afgelopen 24 uur
* Afgelopen 7 dagen
* Afgelopen 30 dagen
* Aantal voorbije 90 dagen
* Afgelopen jaar

Twee belangrijke metrices, groei en bereik worden voor het gebruik van grafieken gemeld. Bereik verwijst naar de maximale waarde en de minimale waarden van het-gebruik gerapporteerd over de geselecteerde duur (voor exemplaar, afgelopen 7 dagen).

Groei verwijst naar de toename van het gebruik van de eerste dag op de laatste dag gedurende de geselecteerde duur. 

Groei en bereik kunnen ook worden weergegeven door de volgende vergelijkingen:

```
Range = {Usage(minimum), Usage(maximum)}

Growth = Usage(Last day) - Usage(first day)

Growth (%) = [{Usage(last day) - Usage(first day)} X 100]/Usage(first day)
```

De primaire, cloud, en lokale opslag gebruikt, kan als volgt worden beschreven:

### <a name="primary-storage-usage"></a>Primaire opslaggebruik
Deze grafieken weergegeven voor de hoeveelheid gegevens geschreven naar de StorSimple-volumes voordat de gegevens worden ontdubbeld en worden gecomprimeerd. U kunt de primaire opslag die wordt gebruikt door alle volumes in een volumecontainer of voor één volume weergeven. De primaire opslag gebruikt verder wordt opgedeeld per primaire gelaagde opslag gebruikt en primaire lokaal vastgemaakt opslag gebruikt.

De volgende grafieken weergegeven primaire opslagruimte wordt gebruikt voor een StorSimple-apparaat voordat en nadat een cloudmomentopname is gemaakt. Omdat dit alleen volumegegevens, moet de primaire opslag niet wijzigen in een cloudmomentopname. Zoals u ziet, bevat het diagram geen verschil tussen de primaire gelaagde of lokaal vastgemaakt opslag gebruikt als gevolg van een momentopname van de cloud. De cloudmomentopname van de is gestart op ongeveer 23:50:00 uur op het apparaat.

![Gebruik van de primaire capaciteit na cloudmomentopname](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

Als u nu IO op de host verbonden met uw StorSimple-apparaat uitvoeren, u een toename van de primaire gelaagde opslag ziet of primaire lokaal opslag gebruikt, afhankelijk vastgemaakt van welke volumes (gelaagd of lokaal vastgemaakt) schrijft u de gegevens naar. Hier volgen de grafieken voor het gebruik van primaire opslag voor een StorSimple-apparaat. Op dit apparaat schrijft de gestart voor de StorSimple-host om ongeveer 2:30 uur op een gelaagd volume op het apparaat. U ziet de piek in de schrijven bytes (s) overeenkomt met de i/o uitgevoerd op de host.

![Prestaties bij i/o waarop volumes gelaagde](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Als u naar de primaire gelaagde opslag gebruikt kijkt, die is geworden van dat de primaire lokaal vastgemaakt gebruik blijft ongewijzigd, omdat er geen schrijfbewerkingen aangeboden aan de lokaal vastgemaakte volumes op het apparaat.

![Gebruik van primaire capaciteit wanneer i/o op gelaagde volumes worden uitgevoerd](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Als u werkt met Update 3 of hoger, u kunt uitsplitsen naar de primaire verbruik van de opslagcapaciteit afzonderlijk volume, alle volumes, alle gelaagde volumes en alle lokaal vastgemaakte volumes zoals hieronder wordt weergegeven. De door alle lokaal vastgemaakte volumes te analyseren, kunt u snel gaan hoeveel van de lokale laag wordt gebruikt.

![Gebruik van de primaire capaciteit voor alle gelaagde volumes](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Gebruik van de primaire capaciteit voor alle lokaal vastgemaakte volumes](./media/storsimple-8000-monitor-device/monitor-usage4.png)

U kunt verder klikt u op elk van de volumes in de lijst en het bijbehorende gebruik Zie.

![Gebruik van de primaire capaciteit voor alle lokaal vastgemaakte volumes](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>Gebruik van cloud-opslag
Deze grafieken weergegeven voor de hoeveelheid cloudopslag die wordt gebruikt. Deze gegevens worden ontdubbeld en gecomprimeerd. Dit bedrag bevat cloudmomentopnamen die mogelijk gegevens bevatten die niet wordt weergegeven in de primaire volumes en bijgehouden voor verouderde of vereist bewaren doeleinden. U kunt vergelijken van de primaire en cloud-verbruik opslagcijfers voor een beter beeld van de frequentie van de gegevens te beperken, maar het nummer is niet exact.

De volgende grafieken weergeven het cloud-opslag-gebruik van een StorSimple-apparaat wanneer een cloudmomentopname is gemaakt.

* De cloudmomentopname gestart op ongeveer 11:50 uur op het apparaat en kunt u zien dat voor de cloud een momentopname, er geen cloud-opslag gebruikt is. 
* Eenmaal de cloudmomentopname is voltooid, het gebruik van de cloud-opslag geschoten up 0.89 GB. 
* De cloudmomentopname werd uitgevoerd, is er ook een bijbehorende piek in de i/o van het apparaat in de cloud.

    ![Gebruik van de cloud-opslag voordat cloudmomentopname](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Gebruik van de cloud-opslag nadat de cloudmomentopname](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![I/o van het apparaat naar cloud tijdens een cloudmomentopname](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Gebruik van lokale opslag
Deze grafieken ziet u het totale gebruik voor het apparaat meer dan primaire opslaggebruik worden omdat hierin de lineaire SSD-laag. Deze laag bevat een hoeveelheid gegevens die ook bestaat op het apparaat de andere laag. De capaciteit in de lineaire SSD-laag volgens zodat als nieuwe gegevens binnenkomt, de oude gegevens worden verplaatst naar de HDD-laag (op dat moment is ontdubbeld en gecomprimeerd) en vervolgens naar de cloud.

Over tijd, primaire opslag verhoogd gebruikt en lokale opslag gebruikt waarschijnlijk samen tot begin van de gegevens naar de in tiers worden verdeeld in de cloud. Op dat moment de lokale opslag gebruikt waarschijnlijk begint af te vlakken, maar de primaire opslag gebruikt naarmate er meer gegevens worden geschreven.

De volgende grafieken weergegeven primaire opslagruimte wordt gebruikt voor een StorSimple-apparaat wanneer een cloudmomentopname is gemaakt. De cloudmomentopname van de om 11:50 am begonnen en de lokale opslag op dat moment verlagen. De lokale opslag gebruikt is gegaan omlaag van 1.445 GB 1,09 GB. Dit geeft aan dat zeer waarschijnlijk de niet-gecomprimeerde gegevens in de lineaire SSD-laag is ontdubbeld, gecomprimeerd en naar de HDD-laag verplaatst. Houd er rekening mee dat als het apparaat al een grote hoeveelheid gegevens in de SSD- en HDD-opslaglagen heeft, u geen deze daling ziet mogelijk. In dit voorbeeld wordt heeft het apparaat een kleine hoeveelheid gegevens.

![Gebruik van de lokale opslag nadat de cloudmomentopname](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Prestaties
**Prestaties** houdt metrische gegevens die betrekking hebben op het aantal lees- en schrijfbewerkingen tussen ofwel de iSCSI-initiator-interfaces op de host-server en het apparaat of het apparaat en de cloud. De prestaties kan worden gemeten voor een specifiek volume, een specifieke volumecontainer of alle volumecontainers. Prestaties tevens CPU-gebruik en de netwerkdoorvoer voor de verschillende netwerkinterfaces op uw apparaat.

### <a name="io-performance-for-initiator-to-device"></a>I/o-prestaties voor initiator op apparaat
Het onderstaande diagram toont de i/o's voor de initiator op uw apparaat voor de volumes voor een productie-apparaat. De metrische gegevens uitgezet worden gelezen en geschreven bytes per seconde. U kunt ook grafiek lezen, schrijven en openstaande i/o, of lezen en schrijven latenties.

![I/o-prestaties voor initiator op apparaat](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-to-cloud"></a>I/o-prestaties voor apparaat in de cloud
De i/o-bewerkingen worden voor hetzelfde apparaat voor de gegevens van het apparaat naar de cloud voor de volumecontainers uitgezet. Op dit apparaat de gegevens zich alleen in de lineaire laag en niets zich in de cloud is verspreid. Er zijn geen lees-/ schrijfbewerkingen optreedt van het apparaat naar de cloud. Daarom zijn de pieken in de grafiek met een interval van 5 minuten dat overeenkomt met de frequentie waarmee de heartbeat tussen het apparaat en de service wordt gecontroleerd.

Voor hetzelfde apparaat, is een cloudmomentopname van een voor volumegegevens starten om 11:50 am genomen. Dit resulteert in gegevens die binnenkomen van het apparaat naar de cloud. Schrijfbewerkingen zijn geleverd in de cloud in deze duur. De i/o-diagram toont een piek in de schrijven Bytes/s overeenkomt met de tijd waarop de momentopname is gemaakt.

![I/o van het apparaat naar cloud tijdens een cloudmomentopname](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Netwerkdoorvoer voor netwerkinterfaces van apparaat
**Netwerkdoorvoer** houdt metrische gegevens die betrekking hebben op de hoeveelheid gegevens overgedragen van de netwerkinterfaces van iSCSI-initiator op de host-server en het apparaat en tussen het apparaat en de cloud. U kunt deze metriek bewaken voor elk van de iSCSI-netwerkinterfaces op uw apparaat.

De volgende diagrammen de netwerkdoorvoer weergeven voor de Data 0, 1-1 GbE-netwerk op uw apparaat, die beide cloud ingeschakeld (standaard) en iSCSI-functionaliteit. Op dit apparaat op 14 juni middags ongeveer 9 gegevens naar de cloud is gelaagd (geen cloudmomentopnamen zijn die op dat moment die wijst naar lagen wordt het mechanisme voor de gegevens worden verplaatst naar de cloud) wat leidde tot i/o worden geleverd aan de cloud. Er is een overeenkomstige piek in de grafiek van de doorvoer netwerk voor hetzelfde moment en het merendeel van het netwerkverkeer is uitgaand naar de cloud.

![Netwerkdoorvoer voor Data 0](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Als we kijken naar de grafiek Data 1 interface doorvoer, 1 GbE een andere netwerkinterface die alleen was ingeschakeld voor iSCSI, wordt er vrijwel geen netwerkverkeer is in deze duur.

![Netwerkdoorvoer voor gegevens-1](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>CPU-gebruik voor apparaat
**CPU-gebruik** houdt gegevens met betrekking tot de CPU-capaciteit op het apparaat worden gebruikt. Het volgende diagram toont de statistieken van de CPU-gebruik voor een apparaat in productie.

![CPU-gebruik voor apparaat](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [gebruiken het Apparaatbeheer StorSimple-apparaat servicedashboard](storsimple-device-dashboard.md).
* Meer informatie over hoe [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

