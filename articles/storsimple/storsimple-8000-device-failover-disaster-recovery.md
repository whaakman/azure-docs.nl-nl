---
title: StorSimple-failover, herstel na noodgevallen voor apparaten uit de 8000-serie | Microsoft Docs
description: Leer hoe u uw StorSimple-apparaat zelf, een ander fysiek apparaat of een cloudapparaat failover.
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 079a2f153f257040d1899a33c9e255d633e526ad
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60576329"
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>Failover en herstel na noodgevallen voor uw StorSimple 8000-apparaat

## <a name="overview"></a>Overzicht

Dit artikel beschrijft de functie apparaat failover voor de apparaten van de StorSimple 8000-serie en hoe deze functie kan worden gebruikt voor het herstellen van StorSimple-apparaten als zich een noodgeval voordoet. StorSimple maakt gebruik van failover van het apparaat naar de gegevens van een Bronapparaat in het datacenter migreren naar een ander doelapparaat. De instructies in dit artikel is van toepassing op StorSimple 8000-serie fysieke apparaten en -cloudapparaten met versies van de software Update 3 en hoger.

StorSimple gebruikt de **apparaten** blade om te beginnen de failoverfunctie van het apparaat in het geval van een noodgeval. Deze blade een lijst met alle van de StorSimple-apparaten die zijn verbonden met uw StorSimple Device Manager-service.

![Blade voor apparaten](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Herstel na noodgeval (DR) en failover van het apparaat

In een scenario voor disaster recovery (DR), het primaire apparaat niet meer werkt. StorSimple maakt gebruik van de primaire als _bron_ en verplaatst u de gekoppelde cloud-gegevens naar een andere _doel_ apparaat. Dit proces wordt aangeduid als de *failover*. De volgende afbeelding illustreert het proces van failover.

![Wat gebeurt er in de failover van het apparaat?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

Het doelapparaat voor een failover mogelijk op een fysiek apparaat of zelfs een cloudapparaat. Het doelapparaat kan zich bevinden in hetzelfde of een andere geografische locatie dan het bronapparaat.

U kunt volumecontainers voor migratie selecteren tijdens de failover. De eigenaar van deze volumecontainers van het bronapparaat StorSimple vervolgens gewijzigd naar het doelapparaat. Zodra de volumecontainers eigendom wijzigen, StorSimple Hiermee verwijdert u deze containers van het bronapparaat. Nadat de verwijdering voltooid is, kunt u het doelapparaat terug mislukken. _Failback_ het eigendom overdraagt naar de oorspronkelijke Bronapparaat.

### <a name="cloud-snapshot-used-during-device-failover"></a>Cloud-momentopname gebruikt tijdens de failover van apparaat

Na een herstel na Noodgevallen, wordt de meest recente cloud back-up gebruikt om de gegevens te herstellen naar het doelapparaat. Zie voor meer informatie over cloudmomentopnamen [gebruik van de StorSimple Device Manager-service een handmatige back-up](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

Op een StorSimple 8000-serie zijn de back-upbeleid gekoppeld aan back-ups. Als er meerdere back-upbeleid voor hetzelfde volume, selecteert de back-upbeleid met het grootste aantal volumes met StorSimple. StorSimple gebruikt vervolgens de meest recente back-up van de geselecteerde back-upbeleid om terug te zetten van de gegevens op het doelapparaat.

Stel dat er zijn twee back-upbeleid, *defaultPol* en *customPol*:

* *defaultPol*: Een volume, *vol1*, wordt uitgevoerd dagelijkse vanaf 10:30 uur.
* *customPol*: Vier volumes, *vol1*, *vol2*, *vol3*, *vol4*, dagelijkse begint bij 10:00 uur uitgevoerd.

In dit geval StorSimple bepaalt de volgorde van voor de crash-consistentie en maakt gebruik van *customPol* omdat er meer volumes. De meest recente back-up van dit beleid wordt gebruikt om gegevens te herstellen. Voor meer informatie over het maken en back-upbeleid beheren, gaat u naar [de StorSimple Device Manager-service gebruiken voor back-upbeleid beheren](storsimple-8000-manage-backup-policies-u2.md).

## <a name="common-considerations-for-device-failover"></a>Algemene overwegingen voor failover van het apparaat

Voordat u een apparaat een failover, controleert u de volgende informatie:

* Voordat de failover van een apparaat wordt gestart, moeten alle volumes in de volumecontainers die offline zijn. In een niet-geplande failover gaat StotSimple volumes automatisch offline. Maar als u een geplande failover (als u wilt testen DR) uitvoert, moet u alle volumes offline uitvoeren.
* Alleen de volumecontainers met een bijbehorende cloud-momentopname voor herstel na Noodgevallen worden vermeld. Er moet ten minste één volumecontainer met een momentopname van een gekoppelde cloud om gegevens te herstellen.
* Als er cloudmomentopnamen die verdeeld over meerdere volumecontainers, failover-schakeling deze volumecontainers StorSimple als een set. In een zeldzame exemplaar als gekoppelde cloud-momentopnamen niet, maar er zijn lokale momentopnamen die verdeeld over meerdere volumecontainers, StorSimple failover van de lokale momentopnamen en de lokale gegevens verloren na het herstel na Noodgevallen.
* De beschikbare doelapparaten voor herstel na Noodgevallen zijn apparaten die u hebt voldoende ruimte voor de geselecteerde volumecontainers. Alle apparaten waarop niet voldoende ruimte worden niet weergegeven als doelapparaten.
* Na een DR (voor een beperkte duur), kan de prestaties van de toegang tot gegevens worden beïnvloed aanzienlijk, terwijl het apparaat moet toegang tot de gegevens vanuit de cloud en lokaal opslaat.

#### <a name="device-failover-across-software-versions"></a>Failover van het apparaat via softwareversies

Een StorSimple Device Manager-service in een implementatie mogelijk op meerdere apparaten, fysieke en de cloud, alle lopende verschillende softwareversies.

Gebruik de volgende tabel om te bepalen of u kunt een failover of een failback uitvoeren naar een ander apparaat met een ander software-versie en de werking van de volumetypen tijdens DR.

#### <a name="failover-and-failback-across-software-versions"></a>Failover en failback tussen versies van software

| Failover / failback van | Fysiek apparaat | Cloudapparaat |
| --- | --- | --- |
| Update 3 voor het bijwerken van 4 |Gelaagde volumes niet meer dan als lagen. <br></br>Lokaal vastgemaakte volumes failover worden uitgevoerd als lokaal vastgemaakt. <br></br> Na een failover wanneer u een momentopname op het apparaat met Update 4 [bijhouden op basis van een heatmap](storsimple-update4-release-notes.md#whats-new-in-update-4) begint. |Lokaal vastgemaakt failover als gelaagde volumes. |
| Update 4 voor het bijwerken van 3 |Gelaagde volumes niet meer dan als lagen. <br></br>Lokaal vastgemaakte volumes failover worden uitgevoerd als lokaal vastgemaakt. <br></br> Back-ups die worden gebruikt om terug te zetten behouden de metagegevens van de heatmap. <br></br>Op basis van een Heatmap tracering is niet beschikbaar in de Update 3 een failback te volgen. |Lokaal vastgemaakt failover als gelaagde volumes. |


## <a name="device-failover-scenarios"></a>Failover-scenario's voor apparaten

Als er een noodgeval, kunt u kunt kiezen voor failover van uw StorSimple-apparaat:

* [Op een fysiek apparaat](storsimple-8000-device-failover-physical-device.md).
* [Aan zichzelf](storsimple-8000-device-failover-same-device.md).
* [Met een cloudapparaat](storsimple-8000-device-failover-cloud-appliance.md).

De voorgaande artikelen bieden gedetailleerde stappen voor elk van de bovenstaande gevallen voor failover.


## <a name="failback"></a>Failback

Update 3 en latere versies wordt ondersteuning StorSimple ook voor failback. Failback wordt alleen de achterzijde van failover, het doel wordt de bron en het oorspronkelijke Bronapparaat tijdens de failover nu wordt het doelapparaat. 

Tijdens een failback wordt StorSimple opnieuw synchroniseert de gegevens terug naar de primaire locatie, stopt de i/o en activiteit van de toepassing en overgangen terug naar de oorspronkelijke locatie.

Na een failover voltooid is, worden de volgende acties uitgevoerd door StorSimple:

* StorSimple wist de volumecontainers waarvoor een failover van het bronapparaat is.
* StorSimple initieert achtergrondtaak per volumecontainer (failover) op het bronapparaat. Als u probeert te mislukken, terwijl de taak uitgevoerd wordt, ontvangt u een melding in die zin. Wacht totdat de taak is voltooid om te beginnen de failback.
* De tijd om het verwijderen van de volumecontainers te voltooien, is afhankelijk van diverse factoren, zoals de hoeveelheid gegevens, leeftijd van de gegevens, het aantal back-ups en de beschikbare netwerkbandbreedte voor de bewerking.

Als u van plan bent testfailovers of failback testen, raden wij u volumecontainers met minder gegevens (in GB's) te testen. Meestal kun u de failback 24 uur nadat de failover voltooid is.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

V. **Wat gebeurt er als het herstel na Noodgeval mislukt of als gedeeltelijk geslaagd heeft?**

A. Als het herstel na Noodgeval mislukt, wordt u aangeraden dat u het opnieuw proberen. Het tweede apparaat failovertaak op de hoogte van de voortgang van de eerste taak en wordt gestart vanaf dat moment en hoger.

V. **Kan ik een apparaat niet verwijderen terwijl de failover van het apparaat uitgevoerd wordt?**

A. U kunt een apparaat niet verwijderen, terwijl een herstel na Noodgeval uitgevoerd wordt. U kunt uw apparaat alleen verwijderen nadat het herstel na Noodgeval voltooid is. U kunt de voortgang apparaat failover-taak in de **taken** blade.

V. **Wanneer de garbagecollection begint op het bronapparaat zodat de lokale gegevens op Bronapparaat wordt verwijderd?**

A. Garbagecollection is ingeschakeld op het bronapparaat alleen als het apparaat volledig is opgeschoond. Het opruimen van de bevat objecten die zijn mislukt gedurende van het bronapparaat zoals volumes, back-objecten (geen gegevens), volumecontainers en beleid opschonen.

V. **Wat gebeurt er als de delete-taak die is gekoppeld aan de volumecontainers op het bronapparaat van de is mislukt?**

A.  Als de taak verwijderen is mislukt, kunt u de volumecontainers handmatig verwijderen. In de **apparaten** blade uw Bronapparaat selecteren en op **volumecontainers**. De volumecontainers die u boven en onder aan de blade niet selecteren, klikt u op **verwijderen**. Nadat u al hebt verwijderd de mislukte via volumecontainers op het bronapparaat, kun u de failback. Ga voor meer informatie naar [verwijderen van een volumecontainer](storsimple-8000-manage-volume-containers.md#delete-a-volume-container).

## <a name="business-continuity-disaster-recovery-bcdr"></a>Zakelijke continuïteit noodherstel (BCDR)

Een zakelijke continuïteit (BCDR) noodherstelscenario treedt op wanneer het hele Azure-datacenter niet meer werkt. In dit scenario kan van invloed op uw StorSimple Device Manager-service en de bijbehorende StorSimple-apparaten.

Als een StorSimple-apparaat is geregistreerd, net voordat een noodgeval is opgetreden, kan dit apparaat moet ondergaan fabrieksinstellingen terug te zetten. Na de ramp wordt het StorSimple-apparaat weergegeven in de Azure-portal als offline. Dit apparaat moet worden verwijderd uit de portal. Het apparaat opnieuw instellen naar de fabrieksinstellingen en registreer deze opnieuw met de service.

## <a name="next-steps"></a>Volgende stappen

Als u klaar voor het uitvoeren van een failover van het apparaat bent, kies een van de volgende scenario's voor gedetailleerde instructies:

- [Failover naar een andere fysieke apparaat](storsimple-8000-device-failover-physical-device.md)
- [Failover naar hetzelfde apparaat](storsimple-8000-device-failover-same-device.md)
- [Failover naar een StorSimple-Cloudapparaat](storsimple-8000-device-failover-cloud-appliance.md)

Als u hebt uw apparaat een failover, kiezen uit een van de volgende opties:

* [Deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).
* [De StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

