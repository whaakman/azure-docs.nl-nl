---
title: Beheer van Microsoft Azure StorSimple Manager virtuele matrix | Microsoft Docs
description: Informatie over het beheren van uw StorSimple lokale virtuele-matrix met behulp van de service Manager voor StorSimple-apparaat in de Azure-portal.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 958244a5-f9f5-455e-b7ef-71a65558872e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: a74a160eae88a2d03460a1346479c333d8f9d524
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-virtual-array"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van uw virtuele StorSimple-matrix
![Processtroom](./media/storsimple-virtual-array-manager-service-administration/manage4.png)

## <a name="overview"></a>Overzicht
In dit artikel worden Apparaatbeheer StorSimple service interface, met inbegrip van hoe u verbinding maakt en de verschillende opties die beschikbaar zijn, beschreven en vindt u koppelingen naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface.

Lees dit artikel en u weet hoe:

* Verbinding maken met de service Manager voor StorSimple-apparaat
* De StorSimple-apparaat Manager UI navigeren
* Beheren van uw virtuele StorSimple-matrix via de service Manager voor StorSimple-apparaat

> [!NOTE]
> Als u wilt de beheeropties die beschikbaar zijn voor de StorSimple 8000 series apparaat weergeven, gaat u naar [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).
> 
> 

## <a name="connect-to-the-storsimple-device-manager-service"></a>Verbinding maken met de service Manager voor StorSimple-apparaat
De StorSimple-apparaat Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere virtuele StorSimple-matrices. U een centrale Microsoft Azure-portal uitgevoerd in een browser gebruiken om deze apparaten te beheren. Voor verbinding met de service Manager voor StorSimple-apparaat, het volgende te doen.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service
1. Ga naar [https://ms.portal.azure.com](https://ms.portal.azure.com).
2. Met de referenties van uw Microsoft-account, meld u aan bij de Microsoft Azure-portal (te vinden op de rechterbovenhoek van het deelvenster).
3. Navigeren naar Bladeren--> 'Filter' op het StorSimple-apparaat Managers om weer te geven van alle beheerders van uw apparaat in een bepaald abonnement.

## <a name="use-the-storsimple-device-manager-service-to-perform-management-tasks"></a>De service Manager voor StorSimple-apparaat gebruiken voor beheertaken uitvoeren
De volgende tabel bevat een overzicht van de algemene beheertaken en complexe werkstromen die kunnen worden uitgevoerd binnen de samenvatting blade voor Apparaatbeheer van StorSimple-service. Deze taken zijn ingedeeld op basis van de blades waarop ze worden geïnitieerd.

Klik op de toepasselijke procedure in de tabel voor meer informatie over elke werkstroom.

#### <a name="storsimple-device-manager-workflows"></a>Apparaatbeheer StorSimple-werkstromen
| Als u dit wilt doen... | Gebruik deze procedure |
| --- | --- | --- |
| Een service maken</br>Een service verwijderen</br>De serviceregistratiesleutel ophalen</br>Opnieuw genereren van de serviceregistratiesleutel |[De service Manager voor StorSimple-apparaat implementeren](storsimple-virtual-array-manage-service.md) |
| Bekijk de activiteitenlogboeken |[Het overzicht van StorSimple-service gebruiken](storsimple-virtual-array-service-summary.md) |
| Een virtuele-matrix deactiveren</br>Verwijderen van een virtuele-matrix |[Deactiveren of verwijderen van een virtuele-matrix](storsimple-virtual-array-deactivate-and-delete-device.md) |
| Disaster recovery en apparaat-failover</br>Vereisten voor failover</br>Herstel van zakelijke continuïteit na noodgevallen (BCDR)</br>Fouten tijdens herstel na noodgevallen |[Disaster recovery en apparaat failover voor uw virtuele StorSimple-matrix](storsimple-virtual-array-failover-dr.md) |
| Back-up-shares en -volumes</br>Maak een handmatige back-up</br>De back-upschema wijzigen</br>Bestaande back-ups weergeven |[Back-up van uw virtuele StorSimple-matrix](storsimple-virtual-array-backup.md) |
| Kloon-shares vanuit een back-upset</br>Volumes klonen van een back-upset</br>Herstel op itemniveau (bestandsserver) |[Klonen van een back-up van uw virtuele StorSimple-matrix](storsimple-virtual-array-clone.md) |
| Over de storage-accounts</br>Een opslagaccount toevoegen</br>Storage-account bewerken</br>Een opslagaccount verwijderen |[Storage-accounts beheren voor de virtuele StorSimple-matrix](storsimple-virtual-array-manage-storage-accounts.md) |
| Over access control records</br>Toevoegen of wijzigen van een record voor toegangscontrole </br>Een record voor toegangscontrole verwijderen |[Access control records voor de virtuele StorSimple-matrix beheren](storsimple-virtual-array-manage-acrs.md) |
| Taakdetails weergeven |[Virtuele StorSimple-matrix taken beheren](storsimple-virtual-array-manage-jobs.md) |
| Instellingen voor waarschuwingen configureren</br>Waarschuwingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen weergeven |[Waarschuwingen weergeven en beheren voor de virtuele StorSimple-matrix](storsimple-virtual-array-manage-alerts.md) |
| Het beheerderswachtwoord van het apparaat wijzigen |[Het beheerderswachtwoord van het virtuele StorSimple-matrix apparaat wijzigen](storsimple-virtual-array-change-device-admin-password.md) |
| Software-updates installeren |[Bijwerken van uw virtuele matrix](storsimple-virtual-array-install-update.md) |

> [!NOTE]
> Moet u de [lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md) voor de volgende taken:
> 
> * [Ophalen van de gegevensversleutelingssleutel van service](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
> * [Een ondersteuningspakket maken](storsimple-ova-web-ui-admin.md#generate-a-log-package)
> * [Stoppen en opnieuw starten van een virtuele-matrix](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)
> 
> 

## <a name="next-steps"></a>Volgende stappen
Voor informatie over de webgebruikersinterface en het gebruik ervan, gaat u naar [de StorSimple-webgebruikersinterface gebruiken voor het beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

