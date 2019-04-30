---
title: Opmerkingen bij de release van de StorSimple 8000 serie Update 5 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 serie Update 5.
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
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: 462cbd6261723aa91bbfd23292611e758a800ed2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844088"
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>Opmerkingen bij de release van de StorSimple 8000 serie Update 5

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven de nieuwe functies en de kritieke open problemen identificeren voor StorSimple 8000 serie Update 5. Ze bevatten ook een lijst van de StorSimple-software-updates in deze release is opgenomen.

Update 5 kan worden toegepast op alle StorSimple-apparaat met Update 0.1 tot en met 4 van de Update. De versie van het apparaat dat is gekoppeld met Update 5 is 6.3.9600.17845.

Lees de informatie die is opgenomen in de opmerkingen bij de release voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Update 5 is een verplichte update en onmiddellijk moet worden geïnstalleerd. Zie voor meer informatie over het [toepassen Update 5](storsimple-8000-install-update-5.md).
> * Update 5 heeft device-software, schijffirmware, OS-beveiliging en andere updates van het besturingssysteem. Het duurt ongeveer 4 uur om deze update te installeren. Update van de schijffirmware is een update verstorend en resulteert in een uitvaltijd voor uw apparaat. Het is raadzaam dat u Update 5 om uw apparaat up-to-date houden van toepassing zijn.
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk dat er een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze updates snel weer beschikbaar is.

## <a name="whats-new-in-update-5"></a>Wat is er nieuw in Update 5

De volgende belangrijke verbeteringen en oplossingen voor problemen zijn aangebracht in Update 5.

* **Gebruik van Azure Active Directory (AAD) om te verifiëren met StorSimple Device Manager-service** – vanaf Update 5 en hoger, Azure Active Directory wordt gebruikt voor verificatie met de StorSimple Device Manager-service. Het oude verificatiemechanisme worden per December 2017 afgeschaft. Alle gebruikers moeten de nieuwe verificatie-URL's opnemen in hun firewall-regels. Ga voor meer informatie naar [verificatie URL's die worden vermeld in de netwerkvereisten voor uw StorSimple-apparaat](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal).

    Als de verificatie-URL niet in de firewall-regels opgenomen is, zien de gebruikers een kritieke waarschuwing dat het StorSimple-apparaat kan niet worden geverifieerd met de service. Als de gebruikers deze waarschuwing zien, moeten ze de nieuwe verificatie-URL. Ga voor meer informatie naar [StorSimple netwerken waarschuwingen](storsimple-8000-manage-alerts.md#networking-alerts).

* **Nieuwe versie van StorSimple Snapshot Manager** -een nieuwe versie van StorSimple Snapshot Manager met Update 5 wordt vrijgegeven en is compatibel met alle StorSimple-apparaten met Update 4 of hoger. We bevelen aan dat u in deze versie. De vorige versie van StorSimple Snapshot Manager wordt gebruikt voor de StorSimple-apparaten met Update 3 of lager. [Download de juiste versie van StorSimple Snapshot Manager](https://www.microsoft.com/en-us/download/details.aspx?id=44220) en verwijzen naar [StorSimple Snapshot Manager implementeren](storsimple-snapshot-manager-deployment.md).


## <a name="issues-fixed-in-update-5"></a>Problemen opgelost met Update 5

De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Update 5.

| Nee | Functie | Probleem | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Windows PowerShell voor externe toegang |In de vorige release ontvangt een gebruiker een fout opgetreden tijdens het tot stand brengen van een externe verbinding met de StorSimple-Cloudapparaat via Windows PowerShell. Dit probleem is veroorzaakt met een basis- en opgelost in deze release. |Nee |Ja |
| 2 |Bandbreedtesjablonen |Er is een probleem met bandbreedtesjablonen dat heeft geresulteerd in een lagere bandbreedte dan wat het apparaat is geconfigureerd voor in eerdere versie. Dit probleem wordt opgelost in deze release. |Ja |Ja |
| 3 |Failover |In de vorige versie, wanneer een apparaat met een groot aantal volumes is een failover naar een ander apparaat met Update 4, mislukt het proces tijdens het toepassen van de access control records. Dit probleem is opgelost in deze release. |Ja |Ja |



## <a name="known-issues-in-update-5-from-previous-releases"></a>Bekende problemen in Update 5 uit vorige versies

Er zijn geen nieuwe bekende problemen in Update 5. Voor een lijst met problemen die worden meegenomen naar een Update 5 uit vorige versies, gaat u naar [opmerkingen bij de release van Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Serial attached SCSI (SAS)-controller en firmware-updates in Update 5

Deze release bevat SAS-controller en LSI-stuurprogramma en firmware-updates. Zie voor meer informatie over het installeren van deze updates [Update 5 installeren](storsimple-8000-install-update-5.md) op uw StorSimple-apparaat.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>StorSimple-Cloudapparaat-updates in Update 5

Deze update kan niet worden toegepast op het StorSimple-Cloudapparaat (ook wel bekend als het virtuele apparaat). Nieuwe cloudapparaten moeten worden gemaakt met behulp van de Update 5. Ga voor informatie over het maken van een StorSimple Cloud Appliance naar [implementeren en beheren van een StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Volgende stap

Meer informatie over het [Update 5 installeren](storsimple-8000-install-update-5.md) op uw StorSimple-apparaat.

