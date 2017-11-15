---
title: Opmerkingen bij de release van de StorSimple 8000 Series Update 5 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 Series Update 5.
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
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: 672757e82bcf645b705f46a9975e09c9dc5eef92
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>Opmerkingen bij de release van de StorSimple 8000 Series Update 5

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen beschrijven de nieuwe functies en problemen met de kritieke open voor StorSimple 8000 Series Update 5. Ze bevatten ook een lijst van de software-updates van het StorSimple opgenomen in deze release.

Update 5 kan worden toegepast op een StorSimple-apparaat met Update 0.1 tot en met 4 van de Update. De versie van de apparaten die zijn gekoppeld met Update 5 is 6.3.9600.17845.

Bekijk de informatie in de release-opmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Update 5 is een verplichte update en onmiddellijk moet worden geïnstalleerd. Zie voor meer informatie hoe [toepassen Update 5](storsimple-8000-install-update-5.md).
> * Update 5 heeft device-software, firmware van de schijf, OS-beveiliging en andere updates voor het besturingssysteem. Het duurt ongeveer 4 uur deze update te installeren. Schijf firmware-update is een update verstoren en resulteert in een uitvaltijd voor uw apparaat. U wordt aangeraden Update 5 up-to-date te houden uw apparaat toe te passen.
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates. Wacht een paar dagen, en vervolgens zoeken naar updates opnieuw als deze updates worden binnenkort beschikbaar.

## <a name="whats-new-in-update-5"></a>Wat is er nieuw in Update 5

De volgende belangrijke verbeteringen en correcties zijn aangebracht in Update 5.

* **Gebruik van Azure Active Directory (AAD) voor verificatie met service voor Apparaatbeheer StorSimple** – van Update 5 en hoger, Azure Active Directory wordt gebruikt voor verificatie met de service Manager voor StorSimple-apparaat. Het oude verificatiemechanisme wordt afgeschaft per December 2017. Alle gebruikers moeten de verificatie-URL's opnemen in hun firewallregels. Ga voor meer informatie naar [verificatie URL's die worden vermeld in de netwerkvereisten voor uw StorSimple-apparaat](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal).

    Als de verificatie-URL niet in de firewall-regels opgenomen is, ziet de gebruiker een kritieke waarschuwing voor die hun StorSimple-apparaat kan niet worden geverifieerd met de service. Als gebruikers deze waarschuwing wordt weergegeven, moeten deze de nieuwe URL voor webverificatie opnemen. Ga voor meer informatie naar [StorSimple networking waarschuwingen](storsimple-8000-manage-alerts.md#networking-alerts).

* **Nieuwe versie van StorSimple Snapshot Manager** -een nieuwe versie van StorSimple Snapshot Manager met Update 5 wordt vrijgegeven en is compatibel met alle StorSimple-apparaten met Update 4 of hoger. Het wordt aangeraden om bij te werken naar deze versie. De vorige versie van StorSimple Snapshot Manager is gebruikt voor de StorSimple-apparaten met Update 3 of eerder. [Download de juiste versie van StorSimple Snapshot Manager](https://www.microsoft.com/en-us/download/details.aspx?id=44220) en verwijzen naar [StorSimple Snapshot Manager implementeren](storsimple-snapshot-manager-deployment.md).


## <a name="issues-fixed-in-update-5"></a>Problemen opgelost met Update 5

De volgende tabel bevat een samenvatting van problemen die in Update 5 zijn opgelost.

| Nee | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Windows PowerShell op afstand |In de vorige release ontvangt een gebruiker zou een fout opgetreden tijdens een poging tot stand brengen van een externe verbinding met het toestel StorSimple Cloud via Windows PowerShell. Dit probleem is veroorzaakt met een hoofdmap en opgelost in deze release. |Nee |Ja |
| 2 |Bandbreedte-sjablonen |In eerdere versie is er een probleem met de bandbreedte-sjablonen dat heeft geresulteerd in lagere bandbreedte dan wat het apparaat is geconfigureerd. Dit probleem wordt opgelost in deze release. |Ja |Ja |
| 3 |Failover |In de vorige release wanneer een apparaat met een groot aantal volumes is een failover naar een ander apparaat met Update 4 mislukken het proces bij het toepassen van de access control records. Dit probleem is opgelost in deze release. |Ja |Ja |



## <a name="known-issues-in-update-5-from-previous-releases"></a>Bekende problemen in Update 5 uit eerdere versies

Er zijn geen nieuwe bekende problemen in Update 5. Voor een lijst met problemen die worden overgedragen aan Update 5 uit vorige versies, gaat u naar [opmerkingen bij de release van Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Serial attached SCSI (SAS)-controller en firmware-updates in Update 5

Deze release heeft SAS-controller en LSI stuurprogramma en firmware-updates. Zie voor meer informatie over het installeren van deze updates [installeren Update 5](storsimple-8000-install-update-5.md) op uw StorSimple-apparaat.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>StorSimple Cloud toestel-updates in de Update 5

Deze update kan niet worden toegepast op het StorSimple-Cloud-apparaat (ook wel bekend als het virtuele apparaat). Nieuwe cloud-apparaten moeten worden gemaakt met behulp van de Update 5-afbeelding. Ga voor informatie over het maken van een StorSimple-apparaat met Cloud naar [implementeren en beheren van een StorSimple-apparaat met Cloud](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Volgende stap

Meer informatie over hoe [installeren Update 5](storsimple-8000-install-update-5.md) op uw StorSimple-apparaat.

