---
title: StorSimple-apparaat servicebeheer | Microsoft Docs
description: Informatie over het beheren van uw StorSimple-apparaat met behulp van de service Manager voor StorSimple-apparaat in de Azure-portal.
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
ms.date: 07/12/2017
ms.author: alkohli
ms.openlocfilehash: 0e7d7f44a70278a7777ba6c32c8e546074953fdc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-device"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van uw StorSimple-apparaat

## <a name="overview"></a>Overzicht

In dit artikel beschrijft de StorSimple-apparaat Manager service interface, waaronder verbinding maken met het, verschillende opties en koppelingen uit naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface. In deze richtlijnen is van toepassing op beide; het fysieke StorSimple-apparaat en het toestel cloud.

Na het lezen van dit artikel leert u hoe:

* Verbinding maken met de service Manager voor StorSimple-apparaat
* Beheren van uw StorSimple-apparaat via de service Manager voor StorSimple-apparaat

## <a name="connect-to-storsimple-device-manager-service"></a>Verbinding maken met de service Manager voor StorSimple-apparaat

De StorSimple-apparaat Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. U een centrale Microsoft Azure-portal uitgevoerd in een browser gebruiken om deze apparaten te beheren. Voor verbinding met de service Manager voor StorSimple-apparaat, het volgende te doen.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service
1. Navigeer naar [https://portal.azure.com/](https://portal.azure.com/).
2. Met de referenties van uw Microsoft-account, meld u aan bij de Microsoft Azure-portal (te vinden op de rechterbovenhoek van het deelvenster).
3. Schuif omlaag in het navigatiedeelvenster links toegang tot de service Manager voor StorSimple-apparaat.


## <a name="administer-storsimple-device-using-storsimple-device-manager-service"></a>StorSimple-apparaat met behulp van Apparaatbeheer StorSimple-service beheren

De volgende tabel bevat een overzicht van de algemene beheertaken en complexe werkstromen die kunnen worden uitgevoerd in de service Manager voor StorSimple-apparaat gebruikersinterface. Deze taken zijn ingedeeld op basis van de UI-blades waarop ze worden geïnitieerd.

Klik op de toepasselijke procedure in de tabel voor meer informatie over elke werkstroom.

#### <a name="storsimple-device-manager-workflows"></a>Apparaatbeheer StorSimple-werkstromen

| Als u dit wilt doen... | Gebruik deze procedure. |
| --- | --- |
| Een service maken</br>Een service verwijderen</br>Serviceregistratiesleutel ophalen</br>Opnieuw genereren serviceregistratiesleutel |[Een service Manager voor StorSimple-apparaat implementeren](storsimple-8000-manage-service.md) |
| Bekijk de activiteitenlogboeken |[Het Serviceoverzicht Manager voor StorSimple-apparaat gebruiken](storsimple-8000-service-dashboard.md) |
| De gegevensversleutelingssleutel van service wijzigen</br>Bekijk de bewerkingslogboeken |[Gebruik het servicedashboard Manager voor StorSimple-apparaat](storsimple-8000-service-dashboard.md) |
| Een apparaat deactiveren</br>Een apparaat verwijderen |[Deactiveren of een apparaat verwijderen](storsimple-8000-deactivate-and-delete-device.md) |
| Meer informatie over disaster recovery en apparaat-failover</br>Failover naar een fysiek apparaat</br>Failover naar een virtueel apparaat</br>Herstel van zakelijke continuïteit na noodgevallen (BCDR) |[Failover en herstel na noodgevallen voor uw StorSimple-apparaat](storsimple-8000-device-failover-disaster-recovery.md) |
| Lijst met back-ups voor een volume</br>Selecteer een back-upset</br>Een back-upset verwijderen |[Back-ups beheren](storsimple-8000-manage-backup-catalog.md) |
| Een volume klonen |[Een volume klonen](storsimple-8000-clone-volume-u2.md) |
| Een back-upset terugzetten |[Een back-upset terugzetten](storsimple-8000-restore-from-backup-set-u2.md) |
| Over de storage-accounts</br>Een opslagaccount toevoegen</br>Storage-account bewerken</br>Een opslagaccount verwijderen</br>Sleutel rotatie van de storage-accounts |[Opslagaccounts beheren](storsimple-8000-manage-storage-accounts.md) |
| Bandbreedte-sjablonen</br>Voeg een bandbreedtesjabloon</br>Een bandbreedtesjabloon bewerken</br>Een bandbreedtesjabloon verwijderen</br>Een standaardsjabloon voor bandbreedte</br>Een hele bandbreedtesjabloon maken die bij een opgegeven periode begint |[Bandbreedtesjablonen beheren](storsimple-8000-manage-bandwidth-templates.md) |
| Over access control records</br>Een record voor toegangscontrole maken</br>Een record voor toegangscontrole bewerken</br>Een record voor toegangscontrole verwijderen |[Access control records beheren](storsimple-8000-manage-acrs.md) |
| Taakdetails weergeven</br>Een taak annuleren |[Taken beheren](storsimple-8000-manage-jobs-u2.md) |
| Waarschuwingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen weergeven |[StorSimple-waarschuwingen weergeven en beheren](storsimple-8000-manage-alerts.md) |
| Bewaking grafieken maken |[Bewaken van uw StorSimple-apparaat](storsimple-monitor-device.md) |
| Een volumecontainer toevoegen</br>Een volumecontainer wijzigen</br>Een volumecontainer verwijderen |[Volumecontainers beheren](storsimple-8000-manage-volume-containers.md) |
| Een volume toevoegen</br>Een volume wijzigen</br>Een volume offline zetten</br>Een volume verwijderen</br>Een volume bewaken |[Volumes beheren](storsimple-8000-manage-volumes-u2.md) |
| Apparaatinstellingen wijzigen</br>Tijdinstellingen wijzigen</br>DNS.md instellingen wijzigen</br>Netwerkinterfaces configureren |[Apparaatconfiguratie voor uw StorSimple-apparaat wijzigen](storsimple-8000-modify-device-config.md) |
| Web proxy-instellingen weergeven |[Webproxy voor uw apparaat configureren](storsimple-8000-configure-web-proxy.md) |
| Het beheerderswachtwoord apparaat wijzigen</br>Wachtwoord StorSimple Snapshot Manager wijzigen |[StorSimple-wachtwoorden wijzigen](storsimple-8000-change-passwords.md) |
| Extern beheer configureren |[Extern verbinding maken met uw StorSimple-apparaat](storsimple-8000-remote-connect.md) |
| Instellingen voor waarschuwingen configureren |[StorSimple-waarschuwingen weergeven en beheren](storsimple-8000-manage-alerts.md) |
| CHAP configureren voor uw StorSimple-apparaat |[CHAP configureren voor uw StorSimple-apparaat](storsimple-configure-chap.md) |
| Een back-upbeleid toevoegen</br>Toevoegen of wijzigen van een planning</br>Een back-upbeleid te verwijderen</br>Maak een handmatige back-up</br>Een aangepaste back-upbeleid maken met meerdere volumes en schema 's |[Back-upbeleid beheren](storsimple-8000-manage-backup-policies-u2.md) |
| Apparaatcontrollers stoppen</br>Apparaatcontrollers starten</br>Apparaatcontrollers afsluiten</br>Uw apparaat terugzetten op fabrieksinstellingen</br>(Hiervoor zijn voor on-premises alleen apparaat) |[StorSimple-apparaat domeincontroller beheren](storsimple-8000-manage-device-controller.md) |
| Meer informatie over hardware-onderdelen voor StorSimple</br>De status van de monitor-hardware</br>(Hiervoor zijn voor on-premises alleen apparaat) |[Monitor voor hardware-onderdelen](storsimple-8000-monitor-hardware-status.md) |
| Een ondersteuningspakket maken |[Maken en beheren van een ondersteuningspakket](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple) |
| Software-updates installeren |[Uw apparaat bijwerken](storsimple-update-device.md) |

## <a name="next-steps"></a>Volgende stappen

Als u problemen met het dagelijkse beheer van uw StorSimple-apparaat of met een van de hardware-onderdelen optreden, Raadpleeg:

* [Problemen oplossen met het hulpprogramma voor diagnostische gegevens](storsimple-8000-diagnostics.md)
* [StorSimple indicatielampjes bewaking gebruiken](storsimple-monitoring-indicators.md)

Als u de problemen kan niet worden omgezet en moet u een serviceaanvraag maken, raadpleegt u [contact opnemen met Microsoft ondersteuning](storsimple-8000-contact-microsoft-support.md).

