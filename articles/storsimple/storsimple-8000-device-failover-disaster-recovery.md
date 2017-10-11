---
title: StorSimple-failover, herstel na noodgevallen voor apparaten 8000 serie | Microsoft Docs
description: Leer uw StorSimple-apparaat zelf, een ander fysiek apparaat of een apparaat cloud failover.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 5dc4a98bf889d38c62c76364289c2d58c14d771e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>Failover en herstel na noodgevallen voor uw StorSimple 8000 series apparaat

## <a name="overview"></a>Overzicht

In dit artikel beschrijft de functie van de failover-apparaat voor de StorSimple 8000 series apparaten en hoe deze functie kan worden gebruikt voor het herstellen van StorSimple-apparaten als een noodsituatie voordoet. Apparaat failover StorSimple gebruikt voor het migreren van de gegevens van een Bronapparaat in het datacenter naar een ander doelapparaat. De instructies in dit artikel is van toepassing op StorSimple 8000 series fysieke apparaten en apparaten van cloud met versies van de software Update 3 en hoger.

Maakt gebruik van StorSimple de **apparaten** blade starten van de functie van de failover-apparaat in het geval van een noodgeval. Deze blade bevat alle StorSimple-apparaten die worden verbonden met uw StorSimple-apparaat Manager-service.

![Blade voor apparaten](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Noodherstel (DR) en failover-apparaat

In een (DR) noodherstelscenario, het primaire apparaat niet meer werkt. StorSimple maakt gebruik van het primaire apparaat als _bron_ en de gekoppelde cloudgegevens verplaatst naar een andere _doel_ apparaat. Dit proces wordt aangeduid als de *failover*. De volgende afbeelding illustreert het proces van failover.

![Wat gebeurt er in failover-apparaat?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

Het doelapparaat voor een failover kan niet een fysiek apparaat of zelfs een cloud-apparaat. Het doelapparaat kan zich bevinden in dezelfde of een andere geografische locatie dan het bronapparaat.

Tijdens de failover selecteert u volumecontainers voor migratie. Het eigendom van deze volumecontainers wijzigingen van het bronapparaat StorSimple vervolgens naar het doelapparaat. Zodra de volumecontainers eigendom wijzigt, worden deze containers in StorSimple verwijderd van het bronapparaat. Nadat de verwijdering voltooid is, u kunt een failback uit op het doelapparaat. _Failback_ het eigendom overdraagt terug naar de oorspronkelijke Bronapparaat.

### <a name="cloud-snapshot-used-during-device-failover"></a>Cloudmomentopname gebruikt tijdens de failover van apparaat

Een DR moet de meest recente cloud back-up worden de gegevens herstellen naar het doelapparaat. Zie voor meer informatie over cloudmomentopnamen [de service Manager voor StorSimple-apparaat gebruiken om een handmatige back-up maken](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

Op een serie StorSimple 8000 zijn back-upbeleid gekoppeld aan back-ups. Als er meerdere back-upbeleid voor hetzelfde volume, selecteert StorSimple vervolgens het back-upbeleid met het grootste aantal volumes. De meest recente back-up van de geselecteerde back-upbeleid StorSimple vervolgens gebruikt om de gegevens op het doelapparaat te herstellen.

Stel dat er zijn twee back-upbeleid, *defaultPol* en *customPol*:

* *defaultPol*: één volume *vol1*, dagelijkse begint bij 10:30 uur uitgevoerd.
* *customPol*: vier volumes *vol1*, *vol2*, *vol3*, *vol4*, dagelijkse begint bij 10:00 uur uitgevoerd.

In dit geval StorSimple bepaalt de volgorde voor crashconsistentie en maakt gebruik van *customPol* omdat deze meerdere volumes bevat. De meest recente back-up van dit beleid wordt gebruikt om gegevens te herstellen. Ga voor meer informatie over het maken en beheren van back-upbeleid naar [de service Manager voor StorSimple-apparaat gebruiken voor het beheren van back-upbeleid](storsimple-8000-manage-backup-policies-u2.md).

## <a name="common-considerations-for-device-failover"></a>Algemene overwegingen voor het apparaat failover

Voordat u een apparaat een failover, controleert u de volgende informatie:

* Voordat een apparaat failover wordt gestart, moeten alle volumes in de volumecontainers offline zijn. In een niet-geplande failover gaat StotSimple volumes automatisch offline. Maar als u een geplande failover (voor het testen DR) uitvoert, moet u alle volumes offline halen.
* Het volumecontainers die een bijbehorende cloudmomentopname worden vermeld voor herstel na Noodgevallen. Er moet ten minste één volumecontainer met een momentopname van de gekoppelde cloud om gegevens te herstellen.
* Als er cloudmomentopnamen die meerdere volumecontainers overbruggen, wordt StorSimple overgenomen door deze volumecontainers als een set. In een zeldzame exemplaar als lokale momentopnamen die meerdere volumecontainers overbruggen, maar momentopnamen van de gekoppelde cloud niet doet, StorSimple failover van de lokale momentopnamen en de lokale gegevens verloren na Noodherstel.
* De beschikbare doelapparaten voor herstel na Noodgevallen zijn apparaten die voldoende ruimte voor de geselecteerde volumecontainers hebben. Alle apparaten die niet over voldoende ruimte beschikken, worden niet vermeld als doelapparaten.
* Na een DR (voor een bepaalde duur), kan de prestaties van de toegang tot gegevens worden beïnvloed aanzienlijk, als het apparaat nodig heeft voor toegang tot de gegevens vanuit de cloud en lokaal opslaan.

#### <a name="device-failover-across-software-versions"></a>Apparaat failover tussen softwareversies

Een StorSimple-apparaat Manager-service in een implementatie met meerdere apparaten, fysieke kan hebben en de cloud, alle actieve andere softwareversies.

Gebruik de volgende tabel om te bepalen of u kunt een failover of failback naar een ander apparaat met een ander software-versie en de werking van de volumetypen tijdens DR.

#### <a name="failover-and-failback-across-software-versions"></a>Failover en failback over softwareversies

| Failover / failback van | Fysiek apparaat | Cloudapparaat |
| --- | --- | --- |
| Update 3 voor het bijwerken van 4 |Gelaagde volumes dan gelaagde mislukken. <br></br>Lokaal vastgemaakte volumes failover uitgevoerd als lokaal vastgemaakt. <br></br> Na een failover wanneer u een momentopname op het apparaat met Update 4 [bijhouden op basis van heatmap](storsimple-update4-release-notes.md#whats-new-in-update-4) in gang. |Lokaal vastgemaakt failover als gelaagde volumes. |
| Update 4 voor het bijwerken van 3 |Gelaagde volumes dan gelaagde mislukken. <br></br>Lokaal vastgemaakte volumes failover uitgevoerd als lokaal vastgemaakt. <br></br> Back-ups gebruikt om te zetten behouden heatmap metagegevens. <br></br>Bijhouden op basis van Heatmap is niet beschikbaar in Update 3 na een failback. |Lokaal vastgemaakt failover als gelaagde volumes. |


## <a name="device-failover-scenarios"></a>Apparaat failover-scenario 's

Als er een ramp, kunt u failover voor uw StorSimple-apparaat:

* [Naar een fysiek apparaat](storsimple-8000-device-failover-physical-device.md).
* [Naar zichzelf](storsimple-8000-device-failover-same-device.md).
* [Op een cloud-toestel](storsimple-8000-device-failover-cloud-appliance.md).

De voorgaande artikelen bieden gedetailleerde stappen voor elk van de bovenstaande gevallen voor failover.


## <a name="failback"></a>Failback

Update 3 en hoger ondersteuning StorSimple ook voor failback. Failback is alleen de achterzijde van failover, het doel wordt de bron en de oorspronkelijke Bronapparaat tijdens de failover nu is het doelapparaat. 

Tijdens de failback, de gegevens terug naar de primaire locatie, stopt de i/o en activiteit van de toepassing opnieuw worden gesynchroniseerd met StorSimple en overgangen terug naar de oorspronkelijke locatie.

Nadat een failover voltooid is, voert StorSimple uit de volgende acties:

* StorSimple schoongemaakt de volumecontainers die worden overgenomen van het bronapparaat.
* StorSimple initieert een achtergrondtaak per volumecontainer (failover) op het bronapparaat. Als u probeert te doen terwijl de bezig is mislukt, ontvangt u een melding in die zin. Wacht totdat de taak is voltooid de failback starten.
* De tijd om het verwijderen van volumecontainers te voltooien, is afhankelijk van verschillende factoren zoals de hoeveelheid gegevens, de leeftijd van de gegevens, het aantal back-ups en de beschikbare netwerkbandbreedte voor de bewerking.

Als u van plan bent testfailovers of failbacks te testen, kunt u het beste testen volumecontainers met minder gegevens (GB). Meestal kunt u beginnen de failback 24 uur na de failover voltooid is.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Q. **Wat gebeurt er als de DR mislukt of gedeeltelijk geslaagd is?**

A. Als de DR mislukt, wordt u aangeraden dat u het opnieuw proberen. De tweede apparaat failover-taak is op de hoogte van de voortgang van de eerste taak en start vanaf dat moment en hoger.

Q. **Kan ik een apparaat verwijderen terwijl de apparaat-failover uitgevoerd wordt?**

A. U kunt een apparaat niet verwijderen, terwijl een DR uitgevoerd wordt. U kunt alleen het apparaat verwijderen nadat de DR voltooid is. U kunt de voortgang apparaat failover-taak in de **taken** blade.

Q. **Wanneer de garbagecollection begint op het bronapparaat zodat de lokale gegevens op het bronvolume is verwijderd?**

A. Garbagecollection is ingeschakeld op de Bronapparaat alleen als het apparaat is volledig opgeschoond. Het opruimen van de omvat het opruimen van objecten die zijn mislukt via van het bronapparaat zoals volumes, back-objecten (geen gegevens), volumecontainers en beleidsregels.

Q. **Wat gebeurt er als de delete-taak die is gekoppeld aan de volumecontainers in het bronapparaat mislukt?**

A.  Als de taak verwijderen is mislukt, kunt u de volumecontainers handmatig verwijderen. In de **apparaten** blade, selecteer uw Bronapparaat en klik op **volumecontainers**. Selecteer de volumecontainers die u niet over en in de onderaan de blade, klik op **verwijderen**. Nadat u alle hebt verwijderd de mislukte via volumecontainers op het bronapparaat, kunt u beginnen met de failback. Ga voor meer informatie naar [verwijderen van een volumecontainer](storsimple-8000-manage-volume-containers.md#delete-a-volume-container).

## <a name="business-continuity-disaster-recovery-bcdr"></a>Herstel van zakelijke continuïteit na noodgevallen (BCDR)

Een zakelijke continuïteit (BCDR) noodherstelscenario treedt op wanneer de volledige Azure-datacenter niet meer werkt. Dit scenario kan van invloed op uw StorSimple-apparaat Manager-service en de bijbehorende StorSimple-apparaten.

Als een StorSimple-apparaat is geregistreerd, net voordat een noodgeval is opgetreden, kan dit apparaat moet ondergaan fabrieksinstellingen terug te zetten. Na de sitedatabase, het StorSimple-apparaat wordt weergegeven in de Azure-portal als offline. Dit apparaat moet worden verwijderd uit de portal. Het apparaat opnieuw instellen op de standaardwaarden en opnieuw te registreren bij de service.

## <a name="next-steps"></a>Volgende stappen

Als u klaar bent voor het uitvoeren van de failover van een apparaat, kies een van de volgende scenario's voor gedetailleerde instructies:

- [Failover naar een andere fysieke apparaat](storsimple-8000-device-failover-physical-device.md)
- [Failover op hetzelfde apparaat](storsimple-8000-device-failover-same-device.md)
- [Failover naar de Cloud StorSimple-apparaat](storsimple-8000-device-failover-cloud-appliance.md)

Als u uw apparaat hebt failover, kies een van de volgende opties:

* [Deactiveren of verwijderen van uw StorSimple-apparaat](storsimple-8000-deactivate-and-delete-device.md).
* [De service Manager voor StorSimple-apparaat gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

