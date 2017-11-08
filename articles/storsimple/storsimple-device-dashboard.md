---
title: Het dashboard StorSimple Manager-apparaat gebruiken | Microsoft Docs
description: Beschrijving van het dashboard StorSimple Manager service apparaat en hoe u met metrische gegevens storage en verbonden initiators weergeven en het serienummer en de IQN vinden.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c213969-a385-461f-b698-78ef5b8a79cc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2209790b54f5dd18a413f2e08ddc6ffd6aef662
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-device-dashboard-in-storsimple-manager-service"></a>Gebruik het dashboard van het apparaat in StorSimple Manager-service  
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. De versie van dit artikel voor de nieuwe Azure portal, Ga naar [gebruik van het dashboard van het apparaat in de StorSimple Manager-service ](storsimple-8000-device-dashboard.md). Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).


## <a name="overview"></a>Overzicht
Het dashboard van het apparaat StorSimple Manager biedt u een overzicht van informatie voor een specifiek StorSimple-apparaat, in tegenstelling tot het servicedashboard waarmee u informatie over alle apparaten die deel uitmaken van uw Microsoft Azure StorSimple-oplossing.

![De dashboardpagina van apparaat](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Het dashboard bevat de volgende informatie:

* **Grafiekgebied** – ziet u de relevante opslag metrische gegevens in het grafiekgebied aan de bovenkant van het dashboard. U kunt metrische gegevens voor de totale primaire opslag (de hoeveelheid gegevens die zijn geschreven door hosts op uw apparaat) en de totale cloudopslag verbruikt door uw apparaat gedurende een periode weergeven in deze grafiek.
  
     In deze context *primaire opslag* verwijst naar de totale hoeveelheid gegevens die zijn geschreven door de host en door volumetype kunnen worden onderverdeeld: *primaire gelaagde opslag* bevat zowel lokaal opgeslagen gegevens en lagen naar de cloud; *primaire lokaal vastgemaakt opslag* bevat alleen gegevens die lokaal zijn opgeslagen. *Cloudopslag*, aan de andere kant is een meting van de totale hoeveelheid gegevens die zijn opgeslagen in de cloud. Dit omvat gelaagde gegevens en back-ups. Houd er rekening mee dat de gegevens die zijn opgeslagen in de cloud is ontdubbeld en gecomprimeerd, terwijl primaire opslag de hoeveelheid opslagruimte gebruikt geeft voordat de gegevens worden ontdubbeld en worden gecomprimeerd. (U kunt deze twee getallen om een beter beeld van de snelheid met compressie vergelijken.) Voor zowel de primaire en cloud-opslag, de bedragen die zijn gebaseerd op de bijhouden frequentie die u configureert. Bijvoorbeeld, als u een frequentie van één week kiest, klikt u vervolgens de grafiek toont gegevens voor elke dag in de vorige week.
  
     U kunt de grafiek als volgt configureren:
  
  * Overzicht van de hoeveelheid verbruikte gedurende een periode van cloud-opslag, selecteer de **CLOUD-opslag gebruikt** optie. Overzicht van de totale opslag die is geschreven door de host, selecteert u de **primaire GELAAGDE opslag gebruikt** en **primaire lokaal VASTGEMAAKT opslag gebruikt** opties. In de afbeelding zijn beide opties geselecteerd. de grafiek bevat daarom opslag bedragen voor zowel cloud als primaire opslag. Merk op dat alle primaire opslag voordat u Update 2 installeert, wordt vertegenwoordigd door de **primaire GELAAGDE opslag gebruikt** regel.
  * Gebruik de vervolgkeuzelijst in de rechterbovenhoek van de grafiek om op te geven van een periode van 1 week, 1 maand, 3 maanden of 1 jaar. Houd er rekening mee dat op het hoogste niveau van de grafiek vernieuwd slechts één keer per dag wordt en daarom totalen in de vorige dag wordt weergegeven.
    
    Zie voor meer informatie [de StorSimple Manager-service gebruiken voor het bewaken van uw StorSimple-apparaat](storsimple-monitor-device.md).
* **Overzicht gebruik** : In de **overzicht gebruik** gebied, ziet u de hoeveelheid primaire opslag gebruikt, de hoeveelheid ingerichte opslag en de maximale capaciteit voor uw apparaat. Door het vergelijken van deze gebruiksstatistieken voor de maximale hoeveelheid opslag die beschikbaar is, ziet u in één oogopslag als u wilt verkrijgen van extra opslagruimte. Houd er rekening mee dat dit overzicht wordt bijgewerkt om de 15 minuten en, vanwege het verschil in updatefrequentie kan tonen verschillende aantallen dan die wordt weergegeven in het grafiekgebied bovenstaande wordt dagelijks bijgewerkt. Zie voor meer informatie [de StorSimple Manager-service gebruiken voor het bewaken van uw StorSimple-apparaat](storsimple-monitor-device.md).
* **Waarschuwingen** -het **waarschuwingen** gebied bevat een overzicht van de waarschuwingen voor uw apparaat. Waarschuwingen worden gegroepeerd op ernst en een telling van het aantal waarschuwingen op elk ernstniveau is opgegeven. Ernst van de waarschuwing te klikken op een begrensde weergave van het tabblad waarschuwingen om weer te geven u alleen de waarschuwingen van dat ernstniveau worden weergegeven voor dit apparaat worden geopend.
* **Taken** -het **taken** gebied toont de uitkomst van recente activiteit van de taak. Hiermee kunt u zorgen dat het systeem werkt zoals verwacht, of u weet kunt dat u nodig hebt om corrigerende actie te ondernemen. Als u meer informatie over recentelijk uitgevoerde taken, klikt u op **taken is voltooid in de afgelopen 24 uur**.
* De **snelle weergave** gebied aan de rechterkant van het dashboard biedt nuttige informatie zoals Apparaatmodel, serienummer, status, beschrijving en het aantal volumes.

U kunt ook failover configureren en weergeven van verbonden initiators vanuit het dashboard van het apparaat.

De algemene taken die kunnen worden uitgevoerd op deze pagina zijn:

* Verbonden initiators weergeven
* Het serienummer van het apparaat vinden
* Het apparaat doel IQN vinden

## <a name="view-connected-initiators"></a>Verbonden initiators weergeven
Vindt u de iSCSI-initiators die zijn verbonden met uw apparaat door te klikken op de **weergave verbonden initiators** koppeling in de **snelle weergave** gebied van uw apparaat-dashboard. Deze pagina bevat een lijst in tabelvorm van de initiators die zijn verbonden met uw apparaat. Voor elke initiator kunt u het volgende zien:

* De iSCSI Qualified Name (IQN) van de verbonden initiator.
* De naam van de record voor toegangscontrole (ACR) waarmee deze verbonden initiator.
* Het IP-adres van de verbonden initiator.
* De netwerkinterfaces die de initiator is verbonden met op uw opslagapparaat. Dit kunnen variëren van DATA 0 tot en met 5 van de gegevens.
* Alle volumes die de verbonden initiator voor toegang tot volgens de huidige ACR-configuratie is toegestaan.

Als u onverwachte initiators in deze lijst wordt weergegeven of de verwachte resultaten niet ziet, raadpleegt u uw ACR-configuratie. Maximaal 512 initiators kunt aansluiten op uw apparaat.

## <a name="find-the-device-serial-number"></a>Het serienummer van het apparaat vinden
Mogelijk moet u het serienummer van het apparaat wanneer u Microsoft Multipath I/O (MPIO) op het apparaat configureert. De volgende stappen uitvoeren om te zoeken naar het serienummer van het apparaat.

#### <a name="to-find-the-device-serial-number"></a>Het serienummer van het apparaat vinden
1. Navigeer naar **apparaten** > **Dashboard**.
2. Zoek in het rechterdeelvenster van het dashboard, het **snelle weergave** gebied.
3. Schuif naar beneden en zoek het serienummer wordt geverifieerd.

## <a name="find-the-device-target-iqn"></a>Het apparaat doel IQN vinden
Mogelijk moet u het apparaat doel IQN wanneer u de Challenge Handshake Authentication Protocol (CHAP) op uw StorSimple-apparaat configureren. De volgende stappen uitvoeren om te zoeken naar het apparaat doel IQN.

### <a name="to-find-the-device-target-iqn"></a>Het apparaat doel IQN vinden
1. Navigeer naar **apparaten** > **Dashboard**.
2. Zoek in het rechterdeelvenster van het dashboard, het **snelle weergave** gebied.
3. Schuif naar beneden en het doel IQN vinden.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [StorSimple Manager servicedashboard](storsimple-service-dashboard.md).
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

