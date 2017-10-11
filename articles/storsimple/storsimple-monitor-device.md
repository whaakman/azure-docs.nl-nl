---
title: Bewaken van uw StorSimple-apparaat | Microsoft Docs
description: Beschrijft hoe de StorSimple Manager-service gebruiken voor het bewaken van i/o-prestaties, gebruik van capaciteit, netwerkdoorvoer en prestaties van een apparaat.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: bd4f7704-4f6f-47d0-927a-b1c91eabc453
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: d45bb37c8417785db0ea38be4375a998b6d9f109
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>De StorSimple Manager-service gebruiken voor het bewaken van uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
U kunt de StorSimple Manager-service gebruiken voor het bewaken van specifieke apparaten binnen uw StorSimple-oplossing. U kunt aangepaste grafieken op basis van i/o-prestaties, gebruik van capaciteit, netwerkdoorvoer en maatstaven voor prestaties apparaat maken. 

Als u de bewaking informatie voor een specifiek apparaat in de klassieke Azure portal, selecteer de StorSimple Manager-service. Klik op de **Monitor** tabblad en selecteer vervolgens in de lijst met apparaten. De **Monitor** pagina bevat de volgende informatie.

## <a name="io-performance"></a>I/o-prestaties
**I/o-prestaties** houdt metrische gegevens die betrekking hebben op het aantal lees- en schrijfbewerkingen tussen ofwel de iSCSI-initiator-interfaces op de host-server en het apparaat of het apparaat en de cloud. De prestaties kan worden gemeten voor een specifiek volume, een specifieke volumecontainer of alle volumecontainers.

Het onderstaande diagram toont de i/o's voor de initiator op uw apparaat voor de volumes voor een productie-apparaat. De metrische gegevens uitgezet worden gelezen en geschreven bytes per seconde, lezen en schrijven van i/o-bewerkingen per seconde en lezen en schrijven latenties.

![I/o-prestaties van de initiator op apparaat](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

De i/o-bewerkingen worden voor hetzelfde apparaat voor de gegevens van het apparaat naar de cloud voor de volumecontainers uitgezet. Op dit apparaat de gegevens zich alleen in de lineaire laag en niets zich in de cloud is verspreid. Er zijn geen lees-/ schrijfbewerkingen optreedt van het apparaat naar de cloud. Daarom zijn de pieken in de grafiek met een interval van 5 minuten dat overeenkomt met de frequentie waarmee de heartbeat tussen het apparaat en de service wordt gecontroleerd. 

![I/o-prestaties van het apparaat in de cloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)

Voor hetzelfde apparaat, is een cloudmomentopname van een worden genomen voor volumegegevens vanaf 14:00 uur. Dit resulteert in gegevens die binnenkomen van het apparaat naar de cloud. Lezen / schrijven zijn geleverd in de cloud in deze duur. De i/o-diagram toont een piek in de verschillende metrische gegevens die overeenkomt met de tijd waarop de momentopname is gemaakt. 

![I/o-prestaties voor apparaat in de cloud nadat de cloudmomentopname](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)

## <a name="capacity-utilization"></a>Capaciteitsverbruik
**Capaciteitsverbruik** houdt metrische gegevens die betrekking hebben op de hoeveelheid opslagruimte die wordt gebruikt door de volumes, de volumecontainers of het apparaat. U kunt rapporten op basis van het gebruik van de capaciteit van uw primaire opslag, uw cloud-opslag of uw opslagruimte van het apparaat kunt maken. Capaciteitsverbruik kan op een bepaald volume, een specifieke volumecontainer of alle volumecontainers worden gemeten.

De primaire, cloud, en de opslagcapaciteit apparaat kan als volgt worden beschreven:

### <a name="primary-storage-capacity-utilization"></a>Primaire verbruik van de opslagcapaciteit
Deze grafieken weergegeven voor de hoeveelheid gegevens geschreven naar de StorSimple-volumes voordat de gegevens worden ontdubbeld en worden gecomprimeerd. U kunt het gebruik van de primaire opslag weergeven door alle volumes of voor één volume.

Wanneer u de primaire opslag volume capaciteit gebruik grafieken voor alle volumes ten opzichte van elk van de afzonderlijke volumes weergeven en totaliseren van de primaire gegevens in beide gevallen deze, kan er een niet-overeenkomend tussen de twee getallen. De totale primaire gegevens op alle volumes kan niet tot de totale som van de primaire gegevens van de afzonderlijke volumes toevoegen. Dit wordt mogelijk veroorzaakt door een van de volgende:

* **Momentopnamegegevens opgenomen voor alle volumes**: dit gedrag wordt gezien alleen als u versie ouder is dan Update 3. De primaire gegevens weergegeven voor alle volumes is de som van de primaire gegevens voor elk volume en de momentopname. De primaire gegevens weergegeven voor een bepaald volume komt overeen met de hoeveelheid gegevens die zijn toegewezen op het volume (en omvat niet de bijbehorende gegevens van de volume-momentopname).
  
    Dit kan ook worden verklaard door de volgende vergelijking:
  
    *Primaire gegevens (alle volumes) = de som van (primaire gegevens (volume i) + grootte van de momentopnamegegevens (volume i))*
  
    *Wanneer primaire gegevens (volume i) = grootte van de primaire gegevens die zijn toegewezen aan volume i*
  
    Als de momentopnamen worden verwijderd via de service, wordt de verwijdering asynchroon uitgevoerd op de achtergrond. Het kan even duren voor de grootte van het volume gegevens na het verwijderen van een momentopname worden bijgewerkt. 
  
    Als met Update 3 of hoger, klikt u vervolgens de momentopnamegegevens niet opgenomen in de volumegegevens. En het gebruik van de primaire als volgt berekend:
  
    * Primaire gegevens (alle volumes) = de som van (primaire gegevens (volume i)
  
    *Wanneer primaire gegevens (volume i) = grootte van de primaire gegevens die zijn toegewezen aan volume i*
* **Volumes met bewaking uitgeschakeld opgenomen in alle volumes**: als u volumes op het apparaat waarvoor bewaking is uitgeschakeld hebt, de bewakingsgegevens voor de afzonderlijke volumes niet meer beschikbaar in de grafieken. De gegevens voor alle volumes in de grafiek bevat echter de volumes waarvoor bewaking is uitgeschakeld. 
* **Volumes met een live gekoppelde back-ups opgenomen voor alle volumes verwijderd**: als volumes met momentopnamegegevens worden verwijderd, maar de bijbehorende momentopnamen, nog bestaan, wordt u een niet-overeenkomend ziet mogelijk.
* **Volumes die zijn opgenomen voor alle volumes verwijderd**: In sommige gevallen oude volumes mogelijk bestaat, zelfs als deze zijn verwijderd. Het effect van verwijdering niet te zien en het apparaat kan tonen lagere beschikbare capaciteit. Moet u contact opnemen met Microsoft Support deze volumes verwijderen.

De volgende grafieken weergeven in de primaire verbruik van de opslagcapaciteit van een StorSimple-apparaat vóór en na een cloudmomentopname werd gemaakt. Omdat dit alleen volumegegevens, moet de primaire opslag niet wijzigen in een cloudmomentopname. Zoals u ziet, bevat het diagram geen verschil tussen het gebruik van de capaciteit van de primaire als gevolg van een momentopname van de cloud. De cloudmomentopname van de is gestart op ongeveer 14:00 uur op het apparaat.

![Gebruik van de primaire capaciteit voordat cloudmomentopname](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Gebruik van de primaire capaciteit na cloudmomentopname](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Als u werkt met Update 2 of hoger, u kunt uitsplitsen naar de primaire verbruik van de opslagcapaciteit afzonderlijk volume, alle volumes, alle gelaagde volumes en alle lokaal vastgemaakte volumes zoals hieronder wordt weergegeven. De door alle lokaal vastgemaakte volumes te analyseren, kunt u snel gaan hoeveel van de lokale laag wordt gebruikt.

![Gebruik van de primaire capaciteit voor alle lokaal vastgemaakte volumes](./media/storsimple-monitor-device/localvolumes.png)

### <a name="cloud-storage-capacity-utilization"></a>Gebruik van cloud-opslag-capaciteit
Deze grafieken weergegeven voor de hoeveelheid cloudopslag die wordt gebruikt. Deze gegevens worden ontdubbeld en gecomprimeerd. Dit bedrag bevat cloudmomentopnamen die mogelijk gegevens bevatten die niet wordt weergegeven in de primaire volumes en bijgehouden voor verouderde of vereist bewaren doeleinden. U kunt vergelijken van de primaire en cloud-verbruik opslagcijfers voor een beter beeld van de frequentie van de gegevens te beperken, maar het nummer is niet exact. De volgende diagrammen geven de cloud verbruik van de opslagcapaciteit van een StorSimple-apparaat voordat en nadat een cloudmomentopname is gemaakt. De cloudmomentopname gestart op ongeveer 14:00 uur op het apparaat en ziet u het gebruik van de capaciteit van de cloud maken van op hetzelfde moment van 5.73 MB versie 4.04 GB verhogen.

![Gebruik van cloud-capaciteit voordat cloudmomentopname](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Gebruik van capaciteit cloud nadat de cloudmomentopname](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)

### <a name="device-storage-capacity-utilization"></a>Verbruik van de opslagcapaciteit apparaat
Deze grafieken ziet u het totale gebruik voor het apparaat meer dan primaire opslaggebruik worden omdat hierin de lineaire SSD-laag. Deze laag bevat een hoeveelheid gegevens die ook bestaat op het apparaat de andere laag. De capaciteit in de lineaire SSD-laag volgens zodat als nieuwe gegevens binnenkomt, de oude gegevens worden verplaatst naar de HDD-laag (op dat moment is ontdubbeld en gecomprimeerd) en vervolgens naar de cloud.

Na verloop van tijd verhoogd gebruik van primaire capaciteit en gebruik van de capaciteit apparaat waarschijnlijk samen tot begin van de gegevens naar de in tiers worden verdeeld in de cloud. Op dat moment wordt het gebruik van de capaciteit apparaat waarschijnlijk begint af te vlakken, maar het gebruik van de primaire capaciteit naarmate er meer gegevens worden geschreven.

De volgende grafieken weergeven in de primaire verbruik van de opslagcapaciteit van een StorSimple-apparaat vóór en na een cloudmomentopname werd gemaakt. De cloudmomentopname van de begonnen op 14:00 uur en het gebruik van de capaciteit apparaat op dat moment verlagen. Het apparaat verbruik van de opslagcapaciteit is gegaan omlaag van 11.58 GB 7.48 GB. Dit geeft aan dat zeer waarschijnlijk de niet-gecomprimeerde gegevens in de lineaire SSD-laag is ontdubbeld, gecomprimeerd en naar de HDD-laag verplaatst. Houd er rekening mee dat als het apparaat al een grote hoeveelheid gegevens in de SSD- en HDD-opslaglagen heeft, u geen deze daling ziet mogelijk. In dit voorbeeld wordt heeft het apparaat een kleine hoeveelheid gegevens.

![Gebruik van capaciteit apparaat voordat cloudmomentopname](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Gebruik van capaciteit apparaat na cloudmomentopname](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)

## <a name="network-throughput"></a>Netwerkdoorvoer
**Netwerkdoorvoer** houdt metrische gegevens die betrekking hebben op de hoeveelheid gegevens overgedragen van de netwerkinterfaces van iSCSI-initiator op de host-server en het apparaat en tussen het apparaat en de cloud. U kunt deze metriek bewaken voor elk van de iSCSI-netwerkinterfaces op uw apparaat.

De volgende diagrammen geven de netwerkdoorvoer voor de Data 0 en 4, gegevens, zowel 1 GbE-netwerkinterfaces op uw apparaat. In dit exemplaar is Data 0 ingeschakeld voor de cloud terwijl Data 4 iSCSI-functionaliteit. U ziet het inkomende en uitgaande verkeer voor uw StorSimple-apparaat. De platte regel in de grafiek vanaf 3:24 uur is ten gevolge van het feit dat we verzamelen van gegevens alleen om de 5 minuten en dient te worden genegeerd. 

![Netwerkdoorvoer voor Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Netwerkdoorvoer voor Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)

## <a name="device-performance"></a>Prestaties van een apparaat
**Prestaties van een apparaat** houdt metrische gegevens die betrekking hebben op de prestaties van uw apparaat. Het volgende diagram toont de statistieken van de CPU-gebruik voor een apparaat in productie.

![CPU-gebruik voor apparaat](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [gebruik het dashboard StorSimple Manager service apparaat](storsimple-device-dashboard.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

