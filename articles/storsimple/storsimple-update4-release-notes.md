---
title: Opmerkingen bij de release van de StorSimple 8000 serie Update 4 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 serie Update 4.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: ef95ca7b9f94690b607e37fbf5d9378c2f2bcfda
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60530979"
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>Opmerkingen bij de release van de StorSimple 8000 serie Update 4

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven de nieuwe functies en de kritieke open problemen identificeren voor StorSimple 8000 serie Update 4. Ze bevatten ook een lijst van de StorSimple-software-updates in deze release is opgenomen. 

Update 4 kan worden toegepast op alle StorSimple-apparaat met Release (GA) of Update 0.1 via Update 3.1. De versie van het apparaat dat is gekoppeld aan de Update 4 wordt 6.3.9600.17820.

Lees de informatie in de opmerkingen bij de release voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Update 4 heeft de software voor apparaten, voor Gebruikersbeheer firmware, LSI-stuurprogramma en firmware, schijffirmware, Storport en Spaceport, beveiliging en andere updates van het besturingssysteem. Het duurt ongeveer 4 uur om deze update te installeren. Update van de schijffirmware is een update verstorend en resulteert in een uitvaltijd voor uw apparaat. Het is raadzaam dat u Update 4 om uw apparaat up-to-date houden van toepassing zijn. 
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk dat er een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze snel weer beschikbaar is.

## <a name="whats-new-in-update-4"></a>Wat is er nieuw in Update 4

De volgende belangrijke verbeteringen en oplossingen voor problemen zijn aangebracht in Update 4.

* **Slimmer geautomatiseerde ruimte vrijmaken algoritmen** – In Update 4, de geautomatiseerde ruimte vrijmaken algoritmen worden verbeterd, zodat het aanpassen van de ruimte vrijmaken cycli op basis van de verwachte geregenereerde beschikbare ruimte in de cloud. 

* **Prestatieverbeteringen voor lokaal vastgemaakte volumes** – Update 4 de prestaties van lokaal vastgemaakte volumes in scenario's met hoge gegevensopname (gegevens die vergelijkbaar is met de grootte van volume) is verbeterd.

* **Herstel op basis van een Heatmap** -In de eerdere releases, na een noodgeval (DR), de gegevens zijn hersteld vanuit de cloud op basis van de toegangspatronen leidt tot een trage prestaties. 

    Een nieuwe functie in Update 4 wordt geïmplementeerd dat nummers gegevens veelgebruikte voor het maken van een heatmap wanneer het apparaat wordt gebruikt voor herstel na Noodgevallen (meestgebruikte gegevenssegmenten hoge heatmap hebben, terwijl minder segmenten gebruikt lage heatmap hebben). Na het herstel na Noodgevallen, StorSimple de heatmap gebruikt om automatisch te herstellen en rehydrate van de gegevens vanuit de cloud. 

    Alle herstelbewerkingen zijn nu heatmap op basis van herstelbewerkingen. Voor meer informatie over hoe u query's uitvoeren en heatmap op basis van terugzetten en rehydratatie taken annuleren, gaat u naar [Windows PowerShell voor StorSimple-cmdlet-verwijzing](https://technet.microsoft.com/library/dn688168.aspx).

* **Hulpprogramma voor diagnostische gegevens voor StorSimple** – In Update 4, een hulpprogramma voor diagnostische gegevens over StorSimple wordt uitgebracht om toe te staan voor het eenvoudig vaststellen en oplossen van problemen met betrekking tot de status van system, het netwerk, de prestaties en de hardware-onderdeel. Dit hulpprogramma wordt uitgevoerd via de Windows PowerShell voor StorSimple. Ga voor meer informatie naar [problemen oplossen met behulp van StorSimple diagnostische](storsimple-8000-diagnostics.md).

* **Hulpprogramma voor migratie van StorSimple van gebruikersinterface gebaseerde** -migratie van gegevens uit de 5000-7000 serie vereist vóór deze release is de gebruikers voor het uitvoeren van een onderdeel van de Migratiewerkstroom met behulp van de Azure PowerShell-interface. In deze release is een eenvoudig te gebruiken op basis van een gebruikersinterface StorSimple Migratiehulpmiddel beschikbaar gesteld voor ondersteuning van dezelfde Migratiewerkstroom vergemakkelijken. Dit hulpprogramma ook zorgen voor de consolidatie van herstel buckets. 

* **FIPS-gerelateerde wijzigingen** : met deze release en hoger, FIPS is standaard ingeschakeld op alle apparaten uit de StorSimple 8000-serie voor zowel de Microsoft Azure Government en Azure openbare cloud die accounts.

* **Wijzigingen doorvoeren** -In deze release, fouten met betrekking tot het bijwerken van de fouten zijn opgelost.

* **Waarschuwing voor schijffouten** -een nieuwe waarschuwing waarschuwing van de gebruiker van de aanstaande schijffouten te voorkomen dat in deze release is toegevoegd. Als u deze waarschuwing tegenkomt, neem dan contact op met Microsoft Support om te verzenden van een vervangende schijf. Ga voor meer informatie naar [hardware-meldingen op uw StorSimple-apparaat](storsimple-8000-manage-alerts.md#hardware-alerts).

* **Controller vervanging wijzigingen** -een cmdlet waarmee de gebruiker de status van het proces voor het vervangen van controller opvragen in deze release is toegevoegd. Voor meer informatie gaat u naar de [cmdlet in query-controller Vervangingsstatus](https://technet.microsoft.com/library/dn688168.aspx).


## <a name="issues-fixed-in-update-4"></a>Problemen opgelost in Update 4

De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Update 4.    

| Nee | Functie | Probleem | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Failover |In de eerdere versie, na de failover, er is een probleem met betrekking tot opschonen waargenomen op de locatie van de klant. Dit probleem is opgelost in deze release. |Ja |Ja |
| 2 |Lokaal vastgemaakte volumes |Er is een probleem te maken van een bijbehorende volume voor lokaal vastgemaakte volumes die in fouten bij het maken van volume resulteren zouden in de vorige versie. Dit probleem is veroorzaakt met een basis- en opgelost in deze release. |Ja |Nee |
| 3 |Ondersteuningspakket |In de vorige versie, er zijn problemen met betrekking tot ondersteuning-pakket dat leiden tot zou een uitzondering System.OutOfMemory of andere fouten die leidt tot een ondersteuning pakket maken is mislukt. Deze fouten zijn verholpen in deze release. |Ja |Ja |
| 4 |Bewaking |In de vorige versie vastgemaakte er een probleem met betrekking tot controlegrafieken voor lokaal volumes waar verbruik in EB werd weergegeven. Deze fout is opgelost in deze release. |Ja |Ja |
| 5 |Migratie |In eerdere versie gebruikt zijn er verschillende problemen met betrekking tot de betrouwbaarheid van de migratie van 5000-7000-serie 8000-serie-apparaten. Deze problemen zijn opgelost in deze release. |Ja |Ja |
| 6 |Update |In eerdere versies, als er een fout tijdens het bijwerken is, de domeincontrollers in de herstelmodus gebruik en kan daarom de gebruiker kan niet doorgaan met de update en moet er contact op met Microsoft Support. <br> Dit gedrag is gewijzigd in deze release. Als de gebruiker een fout tijdens het bijwerken heeft wanneer beide controllers dezelfde versie (Update 4) worden uitgevoerd, gaan de controllers niet in de herstelmodus. Als de gebruiker deze fout wordt aangetroffen, raden wij dat ze op een bits wachten en voer de update vervolgens opnieuw uit. De nieuwe poging kan slagen. Als de nieuwe poging is mislukt, moeten ze contact met Microsoft Support. |Ja |Ja |


## <a name="known-issues-in-update-4-from-previous-releases"></a>Bekende problemen in Update 4 uit vorige versies

Er zijn geen nieuwe bekende problemen in Update 4. Voor een lijst met problemen die worden overgedragen naar Update 4 uit vorige versies, gaat u naar [opmerkingen bij de release van Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Serial attached SCSI (SAS)-controller en firmware-updates in Update 4

Deze release bevat SAS-controller en LSI-stuurprogramma en firmware-updates. Zie voor meer informatie over het installeren van deze updates [Update 4 installeren](storsimple-install-update-4.md) op uw StorSimple-apparaat.

## <a name="virtual-device-updates-in-update-4"></a>Updates van de virtuele apparaat in Update 4

Deze update kan niet worden toegepast op het StorSimple-Cloudapparaat (ook wel bekend als het virtuele apparaat). Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap

Meer informatie over het [Update 4 installeren](storsimple-install-update-4.md) op uw StorSimple-apparaat.

