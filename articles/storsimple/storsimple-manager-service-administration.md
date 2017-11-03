---
title: StorSimple Manager-servicebeheer | Microsoft Docs
description: Informatie over het beheren van uw StorSimple-apparaat met behulp van de StorSimple Manager-service in de klassieke Azure portal.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2586582e-d85c-42e1-afb3-be734c1c0461
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 22924da07434a06f4c822d97a2afd02ea982e0e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>De StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat
## <a name="overview"></a>Overzicht
In dit artikel beschrijft de StorSimple Manager service interface, waaronder verbinding maken met het, verschillende opties en koppelingen uit naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface. In deze richtlijnen is van toepassing op beide; de fysieke StorSimple en het virtuele apparaat.

Na het lezen van dit artikel leert u hoe:

* Verbinding maken met de StorSimple Manager-service
* De StorSimple Manager UI navigeren
* Beheren van uw StorSimple-apparaat via de StorSimple Manager-service

## <a name="connect-to-storsimple-manager-service"></a>Verbinding maken met de StorSimple Manager-service
De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. U kunt een centrale Microsoft Azure classic portal wordt uitgevoerd in een browser gebruiken om deze apparaten te beheren. Maak verbinding met de StorSimple Manager-service door het volgende te doen.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service
1. Navigeer naar [https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. Met de referenties van uw Microsoft-account, meld u aan bij de klassieke portal (te vinden op de rechterbovenhoek van het deelvenster) van Microsoft Azure.
3. Schuif omlaag in het navigatiedeelvenster links toegang tot de StorSimple Manager-service.

## <a name="navigate-storsimple-manager-service-ui"></a>Navigeer gebruikersinterface van de StorSimple Manager-service
De navigatie hiërarchie voor de StorSimple Manager-service de gebruikersinterface wordt weergegeven in de volgende tabel.

* **StorSimple Manager** startpagina, gaat u naar de gebruikersinterface serviceniveau's die van toepassing op alle apparaten in een service.
* **Apparaten** pagina gaat u naar de van toepassing op een specifiek apparaat apparaatniveau – UI's.
* **Volumecontainers** pagina gaat u naar de pagina volume waarin alle volumes die zijn gekoppeld aan een apparaat.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple Manager service navigeerbare hiërarchie
| Startpagina | Serviceniveau-pagina 's | Pagina's op apparaatniveau | Pagina's op apparaatniveau |
| --- | --- | --- | --- |
| StorSimple Manager-service |Servicedashboard |Apparaat-dashboard | |
| Apparaten → |Bewaken | | |
| Back-upcatalogus |Volume containers→ |Volumes | |
| (Service) configureren |Back-upbeleid | | |
| Taken |(Apparaat) configureren | | |
| Waarschuwingen |Onderhoud | | |

![Video beschikbaar](./media/storsimple-manager-service-administration/Video_icon.png) **Video beschikbaar**

Als u wilt een video die u bij de gebruikersinterface van de StorSimple Manager-service helpt, klikt u op [hier](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>StorSimple-apparaat met behulp van de StorSimple Manager-service beheren
De volgende tabel bevat een overzicht van de algemene beheertaken en complexe werkstromen die kunnen worden uitgevoerd in de gebruikersinterface van de StorSimple Manager-service. Deze taken zijn ingedeeld op basis van de UI-pagina's waarop ze worden geïnitieerd.

Klik op de toepasselijke procedure in de tabel voor meer informatie over elke werkstroom.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager-werkstromen
| Als u dit wilt doen... | Ga naar deze pagina... | Gebruik deze procedure. |
| --- | --- | --- |
| Een service maken</br>Een service verwijderen</br>Serviceregistratiesleutel ophalen</br>Opnieuw genereren serviceregistratiesleutel |StorSimple Manager-service |[Een StorSimple Manager-service implementeren](storsimple-manage-service.md) |
| De gegevensversleutelingssleutel van service wijzigen</br>Bekijk de bewerkingslogboeken |Dashboard van de StorSimple Manager service → |[Gebruik het servicedashboard StorSimple Manager](storsimple-service-dashboard.md) |
| Een apparaat deactiveren</br>Een apparaat verwijderen |Apparaten voor StorSimple Manager-service → |[Deactiveren of een apparaat verwijderen](storsimple-deactivate-and-delete-device.md) |
| Meer informatie over disaster recovery en apparaat-failover</br>Failover naar een fysiek apparaat</br>Failover naar een virtueel apparaat</br>Herstel van zakelijke continuïteit na noodgevallen (BCDR) |Apparaten voor StorSimple Manager-service → |[Failover en herstel na noodgevallen voor uw StorSimple-apparaat](storsimple-device-failover-disaster-recovery.md) |
| Lijst met back-ups voor een volume</br>Selecteer een back-upset</br>Een back-upset verwijderen |StorSimple Manager service → back-upcatalogus |[Back-ups beheren](storsimple-manage-backup-catalog.md) |
| Een volume klonen |StorSimple Manager service → back-upcatalogus |[Een volume klonen](storsimple-clone-volume.md) |
| Een back-upset terugzetten |StorSimple Manager service → back-upcatalogus |[Een back-upset terugzetten](storsimple-restore-from-backup-set.md) |
| Over de storage-accounts</br>Een opslagaccount toevoegen</br>Storage-account bewerken</br>Een opslagaccount verwijderen</br>Sleutel rotatie van de storage-accounts |Configureren van de StorSimple Manager service → |[Opslagaccounts beheren](storsimple-manage-storage-accounts.md) |
| Bandbreedte-sjablonen</br>Voeg een bandbreedtesjabloon</br>Een bandbreedtesjabloon bewerken</br>Een bandbreedtesjabloon verwijderen</br>Een standaardsjabloon voor bandbreedte</br>Een hele bandbreedtesjabloon maken die bij een opgegeven periode begint |Configureren van de StorSimple Manager service → |[Bandbreedtesjablonen beheren](storsimple-manage-bandwidth-templates.md) |
| Over access control records</br>Een record voor toegangscontrole maken</br>Een record voor toegangscontrole bewerken</br>Een record voor toegangscontrole verwijderen |Configureren van de StorSimple Manager service → |[Access control records beheren](storsimple-manage-acrs.md) |
| Taakdetails weergeven</br>Een taak annuleren |Taken voor StorSimple Manager-service → |[Taken beheren](storsimple-manage-jobs.md) |
| Waarschuwingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen weergeven |StorSimple Manager-Servicewaarschuwingen → |[StorSimple-waarschuwingen weergeven en beheren](storsimple-manage-alerts.md) |
| Verbonden initiators weergeven</br>Het serienummer van het apparaat vinden</br>Het doel IQN vinden |StorSimple Manager service → apparaten → Dashboard |[Gebruik het dashboard StorSimple-apparaat](storsimple-device-dashboard.md) |
| Bewaking grafieken maken |StorSimple Manager service → apparaten bewaken → |[Bewaken van uw StorSimple-apparaat](storsimple-monitor-device.md) |
| Een volumecontainer toevoegen</br>Een volumecontainer wijzigen</br>Een volumecontainer verwijderen |Volumecontainers van StorSimple Manager service → apparaten → |[Volumecontainers beheren](storsimple-manage-volume-containers.md) |
| Een volume toevoegen</br>Een volume wijzigen</br>Een volume offline zetten</br>Een volume verwijderen</br>Een volume bewaken |Volumes van de StorSimple Manager service → apparaten → Volumecontainers → |[Volumes beheren](storsimple-manage-volumes.md) |
| Apparaatinstellingen wijzigen</br>Tijdinstellingen wijzigen</br>DNS.md instellingen wijzigen</br>Netwerkinterfaces configureren |StorSimple Manager service → apparaten → configureren |[Apparaatconfiguratie voor uw StorSimple-apparaat wijzigen](storsimple-modify-device-config.md) |
| Web proxy-instellingen weergeven |StorSimple Manager service → apparaten → configureren |[Webproxy voor uw apparaat configureren](storsimple-configure-web-proxy.md) |
| Het beheerderswachtwoord apparaat wijzigen</br>Wachtwoord StorSimple Snapshot Manager wijzigen |StorSimple Manager service → apparaten → configureren |[StorSimple-wachtwoorden wijzigen](storsimple-change-passwords.md) |
| Extern beheer configureren |StorSimple Manager service → apparaten → configureren |[Extern verbinding maken met uw StorSimple-apparaat](storsimple-remote-connect.md) |
| Instellingen voor waarschuwingen configureren |StorSimple Manager service → apparaten → configureren |[StorSimple-waarschuwingen weergeven en beheren](storsimple-manage-alerts.md) |
| CHAP configureren voor uw StorSimple-apparaat |StorSimple Manager service → apparaten → configureren |[CHAP configureren voor uw StorSimple-apparaat](storsimple-configure-chap.md) |
| Een back-upbeleid toevoegen</br>Toevoegen of wijzigen van een planning</br>Een back-upbeleid te verwijderen</br>Maak een handmatige back-up</br>Een aangepaste back-upbeleid maken met meerdere volumes en schema 's |StorSimple Manager service → apparaten → back-upbeleid |[Back-upbeleid beheren](storsimple-manage-backup-policies.md) |
| Apparaatcontrollers stoppen</br>Apparaatcontrollers starten</br>Apparaatcontrollers afsluiten</br>Uw apparaat terugzetten op fabrieksinstellingen</br>(Hiervoor zijn voor on-premises alleen apparaat) |StorSimple Manager service → apparaten → onderhoud |[StorSimple-apparaat domeincontroller beheren](storsimple-manage-device-controller.md) |
| Meer informatie over hardware-onderdelen voor StorSimple</br>De status van de monitor-hardware</br>(Hiervoor zijn voor on-premises alleen apparaat) |StorSimple Manager service → apparaten → onderhoud |[Monitor voor hardware-onderdelen](storsimple-monitor-hardware-status.md) |
| Een ondersteuningspakket maken |StorSimple Manager service → apparaten → onderhoud |[Maken en beheren van een ondersteuningspakket](storsimple-create-manage-support-package.md) |
| Software-updates installeren |StorSimple Manager service → apparaten → onderhoud |[Uw apparaat bijwerken](storsimple-update-device.md) |

## <a name="next-steps"></a>Volgende stappen
Als u problemen met het dagelijkse beheer van uw StorSimple-apparaat of met een van de hardware-onderdelen optreden, Raadpleeg:

* [Een operationele apparaat oplossen](storsimple-troubleshoot-operational-device.md)
* [StorSimple indicatielampjes bewaking gebruiken](storsimple-monitoring-indicators.md)

Als u de problemen kan niet worden omgezet en moet u een serviceaanvraag maken, raadpleegt u [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).

