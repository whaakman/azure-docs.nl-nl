---
title: StorSimple 8000 Series Update 4 release-opmerkingen | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 Series Update 4.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 23f1bbb066c5b6481988ee841ad8979d78abf084
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>Opmerkingen bij de release van de StorSimple 8000 Series Update 4

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven de nieuwe functies en problemen met de kritieke open voor StorSimple 8000 Series Update 4. Ze bevatten ook een lijst van de software-updates van het StorSimple opgenomen in deze release. 

Update 4 kan worden toegepast op een StorSimple-apparaat met Release (GA) of Update 0.1 via Update 3.1. De versie van het apparaat dat is gekoppeld met Update 4 is 6.3.9600.17820.

Controleer de informatie in de release-opmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Update 4 heeft device-software, USM firmware, stuurprogramma LSI en firmware, schijf-firmware, Storport en Spaceport, beveiligingsupdates en andere updates OS. Het duurt ongeveer 4 uur deze update te installeren. Schijf firmware-update is een update verstoren en resulteert in een uitvaltijd voor uw apparaat. U wordt aangeraden Update 4 up-to-date te houden uw apparaat toe te passen. 
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze is binnenkort beschikbaar.

## <a name="whats-new-in-update-4"></a>Wat is er nieuw in Update 4

De volgende belangrijke verbeteringen en correcties zijn aangebracht in Update 4.

* **Slimmer geautomatiseerde ruimte vrijmaken algoritmen** – In Update 4, de geautomatiseerde ruimte wordt vrijgemaakt algoritmen zijn verbeterd om aan te passen de ruimte vrijmaken cycli op basis van de verwachte geregenereerde beschikbare ruimte in de cloud. 

* **Prestatieverbeteringen voor lokaal vastgemaakte volumes** – Update 4 de prestaties van lokaal vastgemaakte volumes in scenario's waarvoor hoge gegevensopname (gegevens vergelijkbaar is met de grootte van volume) is verbeterd.

* **Heatmap-herstelfuncties** -In de eerdere versies, na een noodherstel (DR), de gegevens is hersteld vanuit de cloud op basis van de toegangspatronen leiden tot een trage prestaties. 

    Een nieuwe functie is geïmplementeerd in Update 4 dat nummers gegevens veelgebruikte voor het maken van een heatmap wanneer het apparaat gebruikt voor Noodherstel wordt (meestgebruikte gegevenssegmenten hoge hitte hebben dat kleiner segmenten gebruikt lage hitte hebt). Na DR, StorSimple de heatmap gebruikt om automatisch te herstellen en rehydrate van de gegevens vanuit de cloud. 

    Alle herstelbewerkingen zijn nu gebaseerd heatmap herstelt. Voor meer informatie over het opvragen en heatmap op basis van terugzetten en rehydratering taken annuleren, gaat u naar [Windows PowerShell voor StorSimple cmdlet-verwijzing](https://technet.microsoft.com/library/dn688168.aspx).

* **Diagnostische StorSimple** : In Update 4, een StorSimple-diagnostische wordt vrijgegeven om toe te staan voor eenvoudig opsporen en oplossen van problemen met betrekking tot de status van systeem, netwerk, prestaties en hardware-onderdeel. Dit hulpprogramma wordt uitgevoerd via de Windows PowerShell voor StorSimple. Ga voor meer informatie naar [oplossen met behulp van StorSimple diagnostische hulpprogramma](storsimple-8000-diagnostics.md).

* **Hulpprogramma voor migratie van de StorSimple van gebruikersinterface gebaseerde** -vóór deze release van migratie van gegevens van de 5000-en 7000-serie vereiste de gebruikers uit te voeren van een onderdeel van de Migratiewerkstroom met behulp van de Azure PowerShell-interface. In deze release is een eenvoudig te gebruiken op basis van een gebruikersinterface StorSimple Migratiehulpmiddel beschikbaar gesteld voor ondersteuning van dezelfde Migratiewerkstroom vergemakkelijken. Dit hulpprogramma wordt ook de mogelijkheid voor de consolidatie van herstel buckets. 

* **FIPS-gerelateerde wijzigingen** - deze release en hoger, FIPS is standaard ingeschakeld op alle apparaten uit de StorSimple 8000 serie voor de Microsoft Azure Government en de Azure openbare cloud accounts.

* **Wijzigingen doorvoeren** -In deze release fouten die betrekking hebben op het bijwerken van de fouten zijn gecorrigeerd.

* **Waarschuwing voor schijffouten** -een nieuwe waarschuwing die door de gebruiker van een aanstaande schijffouten waarschuwt in deze release is toegevoegd. Als u deze waarschuwing tegenkomt, neem dan contact op met Microsoft Support om te verzenden van een vervangende schijf. Ga voor meer informatie naar [hardware waarschuwingen op het StorSimple-apparaat](storsimple-manage-alerts.md#hardware-alerts).

* **Controller vervanging wijzigingen** -een cmdlet waarmee de gebruiker de status van het proces van de vervangende domeincontroller opvragen in deze release is toegevoegd. Ga voor meer informatie naar de [cmdlet query controller vervanging status](https://technet.microsoft.com/library/dn688168.aspx).


## <a name="issues-fixed-in-update-4"></a>Problemen opgelost in Update 4

De volgende tabel bevat een samenvatting van problemen die in Update 4 zijn opgelost.    

| Nee | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Failover |In de eerdere versie wordt na de failover er is een probleem op te schonen waargenomen bij de klant. Dit probleem is opgelost in deze release. |Ja |Ja |
| 2 |Lokaal vastgemaakte volumes |In de vorige release moet u er een probleem voor verwante volume maken voor lokaal vastgemaakte volumes die tot fouten bij het maken van volume leiden zou is. Dit probleem is veroorzaakt met een hoofdmap en opgelost in deze release. |Ja |Nee |
| 3 |Ondersteuningspakket |In de vorige release, er zijn problemen met betrekking tot ondersteuning-pakket dat zou leiden tot een uitzondering System.OutOfMemory of andere fouten waardoor een ondersteuning pakket maken is mislukt. Deze fouten worden opgelost in deze release. |Ja |Ja |
| 4 |Bewaking |In de vorige release vastgemaakte er een probleem dat is gerelateerd aan de bewaking van grafieken voor lokaal volumes waar verbruik is weergegeven in EB. Deze fout is opgelost in deze release. |Ja |Ja |
| 5 |Migratie |In eerdere versie gebruikt zijn er verschillende problemen met betrekking tot de betrouwbaarheid van de migratie van 5000 7000 series naar apparaten 8000 serie. Deze problemen zijn gericht in deze release. |Ja |Ja |
| 6 |Update |In eerdere versies, als er een update-fout is opgetreden, de controllers in de herstelmodus vallen zou en daarom de gebruiker kan niet doorgaan met de update en moet contact opnemen met Microsoft Support. <br> Dit gedrag is gewijzigd in deze release. Als de gebruiker een update mislukt heeft nadat beide domeincontrollers dezelfde versie (Update 4) worden uitgevoerd, gaan de controllers niet in de herstelmodus overgaat. Als de gebruiker deze fout wordt aangetroffen, raden wij aan dat ze een bits wacht en probeer vervolgens de update. De nieuwe poging is mislukt. Als de nieuwe poging is mislukt, moeten ze contact met Microsoft Support. |Ja |Ja |


## <a name="known-issues-in-update-4-from-previous-releases"></a>Bekende problemen in Update 4 uit eerdere versies

Er zijn geen nieuwe bekende problemen in Update 4. Voor een lijst met problemen die worden overgedragen aan Update 4 uit vorige versies, gaat u naar [opmerkingen bij de release van Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Serial attached SCSI (SAS)-controller en firmware-updates in Update 4

Deze release heeft SAS-controller en LSI stuurprogramma en firmware-updates. Zie voor meer informatie over het installeren van deze updates [Update 4 installeren](storsimple-install-update-4.md) op uw StorSimple-apparaat.

## <a name="virtual-device-updates-in-update-4"></a>Virtueel apparaat-updates in Update 4

Deze update kan niet worden toegepast op het StorSimple-Cloud-apparaat (ook wel bekend als het virtuele apparaat). Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap

Meer informatie over hoe [Update 4 installeren](storsimple-install-update-4.md) op uw StorSimple-apparaat.

