---
title: StorSimple Device Manager-servicebeheer | Microsoft Docs
description: Informatie over het beheren van uw StorSimple-apparaat met behulp van de StorSimple Device Manager-service in Azure portal.
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
ms.date: 07/12/2017
ms.author: alkohli
ms.openlocfilehash: b5490c4e79ee1458b498f539c0db2cc189fce7f7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60723303"
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-device"></a>De StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat

## <a name="overview"></a>Overzicht

Dit artikel beschrijft de StorSimple Device Manager service-interface, met inbegrip van hoe u verbinding maakt, de verschillende opties beschikbaar, en koppelingen naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface. Deze handleiding is van toepassing op beide; de fysieke StorSimple-apparaat en het cloudapparaat.

Na het lezen van dit artikel leert u hoe u:

* Verbinding maken met StorSimple Device Manager-service
* Uw StorSimple-apparaat via de service StorSimple Device Manager beheren

## <a name="connect-to-storsimple-device-manager-service"></a>Verbinding maken met StorSimple Device Manager-service

De StorSimple Device Manager-service wordt uitgevoerd in Microsoft Azure en verbindt met meerdere StorSimple-apparaten. U een centrale Microsoft Azure portal wordt uitgevoerd in een browser gebruiken om deze apparaten te beheren. Voor verbinding met de StorSimple Device Manager-service, het volgende te doen.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service
1. Navigeer naar [https://portal.azure.com/](https://portal.azure.com/).
2. Met behulp van de referenties van uw Microsoft-account, meld u aan bij de Microsoft Azure-portal (te vinden in de rechterbovenhoek van het deelvenster).
3. Schuif omlaag in het navigatiedeelvenster links in voor toegang tot de service StorSimple Device Manager.


## <a name="administer-storsimple-device-using-storsimple-device-manager-service"></a>StorSimple-apparaat met behulp van StorSimple Device Manager-service beheren

De volgende tabel bevat een overzicht van de algemene beheertaken en complexe werkstromen die kunnen worden uitgevoerd in de gebruikersinterface van de StorSimple Device Manager-service. Deze taken zijn ingedeeld op basis van de UI-blades waarop ze worden gestart.

Klik op de desbetreffende procedure in de tabel voor meer informatie over elke werkstroom.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager-werkstromen

| Als u dit wilt doen... | Gebruik deze procedure. |
| --- | --- |
| Een service maken</br>Een service verwijderen</br>Serviceregistratiesleutel ophalen</br>Serviceregistratiesleutel opnieuw genereren |[Een StorSimple Device Manager-service implementeren](storsimple-8000-manage-service.md) |
| De activiteitenlogboeken bekijken |[Het Serviceoverzicht StorSimple Device Manager gebruiken](storsimple-8000-service-dashboard.md) |
| Wijzigen van de versleutelingssleutel voor servicegegevens</br>De logboeken voor bewerkingen weergeven |[Gebruik het dashboard van de StorSimple Device Manager](storsimple-8000-service-dashboard.md) |
| Een apparaat deactiveren</br>Een apparaat verwijderen |[Deactiveren of verwijderen van een apparaat](storsimple-8000-deactivate-and-delete-device.md) |
| Meer informatie over disaster recovery en apparaat-failover</br>Failover naar een fysiek apparaat</br>Failover naar een virtueel apparaat</br>Zakelijke continuïteit noodherstel (BCDR) |[Failover en herstel na noodgevallen voor uw StorSimple-apparaat](storsimple-8000-device-failover-disaster-recovery.md) |
| Lijst met back-ups voor een volume</br>Een back-upset selecteren</br>Een back-upset verwijderen |[Back-ups beheren](storsimple-8000-manage-backup-catalog.md) |
| Een volume klonen |[Een volume klonen](storsimple-8000-clone-volume-u2.md) |
| Een back-upset terugzetten |[Een back-upset terugzetten](storsimple-8000-restore-from-backup-set-u2.md) |
| Over storage-accounts</br>Een storage-account toevoegen</br>Een storage-account bewerken</br>Een opslagaccount verwijderen</br>Sleutelroulatie van storage-accounts |[Opslagaccounts beheren](storsimple-8000-manage-storage-accounts.md) |
| Over bandbreedtesjablonen</br>Een bandbreedtesjabloon toevoegen</br>Een van de bandbreedtesjabloon bewerken</br>Een bandbreedtesjabloon verwijderen</br>Een standaardsjabloon voor bandbreedte</br>Maken van een hele dag duurt bandbreedtesjabloon die bij een opgegeven periode begint |[Bandbreedtesjablonen beheren](storsimple-8000-manage-bandwidth-templates.md) |
| Over access control records</br>Een Acces controlerecord maken</br>Een access controlerecord bewerken</br>Verwijderen van een access controlerecord |[Access control records beheren](storsimple-8000-manage-acrs.md) |
| Taakdetails weergeven</br>Een taak annuleren |[Taken beheren](storsimple-8000-manage-jobs-u2.md) |
| Waarschuwingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen weergeven |[StorSimple-waarschuwingen weergeven en beheren](storsimple-8000-manage-alerts.md) |
| Bewakingsgrafieken maken |[Uw StorSimple-apparaat controleren](storsimple-monitor-device.md) |
| Een volumecontainer toevoegen</br>Een volumecontainer aanpassen</br>Een volumecontainer |[Volumecontainers beheren](storsimple-8000-manage-volume-containers.md) |
| Een volume toevoegen</br>Een volume wijzigen</br>Een volume offline halen</br>Een volume verwijderen</br>Een volume bewaken |[Volumes beheren](storsimple-8000-manage-volumes-u2.md) |
| Instellingen voor apparaten wijzigen</br>Tijdinstellingen wijzigen</br>DNS.md instellingen wijzigen</br>Netwerkinterfaces configureren |[Apparaatconfiguratie voor uw StorSimple-apparaat wijzigen](storsimple-8000-modify-device-config.md) |
| Webproxy-instellingen weergeven |[Webproxy voor uw apparaat configureren](storsimple-8000-configure-web-proxy.md) |
| Beheerderswachtwoord voor het apparaat wijzigen</br>StorSimple Snapshot Manager-wachtwoord wijzigen |[StorSimple-wachtwoorden wijzigen](storsimple-8000-change-passwords.md) |
| Extern beheer configureren |[Extern verbinding maken met uw StorSimple-apparaat](storsimple-8000-remote-connect.md) |
| Instellingen voor waarschuwingen configureren |[StorSimple-waarschuwingen weergeven en beheren](storsimple-8000-manage-alerts.md) |
| CHAP configureren voor uw StorSimple-apparaat |[CHAP configureren voor uw StorSimple-apparaat](storsimple-configure-chap.md) |
| Een back-upbeleid toevoegen</br>Toevoegen of wijzigen van een planning</br>Verwijderen van een back-upbeleid</br>Een handmatige back-up maken</br>Een aangepaste back-upbeleid maken met meerdere volumes en schema 's |[Back-upbeleid beheren](storsimple-8000-manage-backup-policies-u2.md) |
| Apparaatcontrollers stoppen</br>Apparaatcontrollers opnieuw starten</br>Apparaatcontrollers afsluiten</br>Uw apparaat terugzetten op fabrieksinstellingen</br>(Hiervoor zijn voor on-premises apparaat alleen) |[StorSimple-apparaatcontroller beheren](storsimple-8000-manage-device-controller.md) |
| Meer informatie over hardware-onderdelen voor StorSimple</br>Hardware-bewakingsstatus</br>(Hiervoor zijn voor on-premises apparaat alleen) |[Monitor voor hardware-onderdelen](storsimple-8000-monitor-hardware-status.md) |
| Een ondersteuningspakket maken |[Maken en beheren van een ondersteuningspakket](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple) |
| Software-updates installeren |[Uw apparaat bijwerken](storsimple-update-device.md) |

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt met het dagelijkse gebruik van uw StorSimple-apparaat of een van de hardware-onderdelen, kunt u verwijzen naar:

* [Problemen oplossen met het hulpprogramma Diagnostics](storsimple-8000-diagnostics.md)
* [StorSimple controle-LED's gebruiken](storsimple-monitoring-indicators.md)

Als u niet kunt van problemen oplossen en moet u een serviceaanvraag maken, raadpleegt u [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md).

