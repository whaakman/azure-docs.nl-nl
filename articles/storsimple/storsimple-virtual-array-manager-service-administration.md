---
title: Virtuele matrix van Microsoft Azure StorSimple Manager-beheer | Microsoft Docs
description: Informatie over het beheren van uw on-premises StorSimple Virtual Array met behulp van de StorSimple Device Manager-service in Azure portal.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 958244a5-f9f5-455e-b7ef-71a65558872e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: bb6bb491ca71e5ced5aecc8137e9e1cbd950e80b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58091865"
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-virtual-array"></a>De StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple Virtual Array
![Processtroom voor Setup](./media/storsimple-virtual-array-manager-service-administration/manage4.png)

## <a name="overview"></a>Overzicht
In dit artikel beschrijft de StorSimple Device Manager service-interface, met inbegrip van hoe u verbinding maakt en de verschillende opties die beschikbaar is, en vindt u koppelingen naar de specifieke werkstromen die kunnen worden uitgevoerd via deze gebruikersinterface.

Na het lezen van dit artikel, weet u hoe u:

* Verbinding maken met de StorSimple Device Manager-service
* De StorSimple-Apparaatbeheerfunctie gebruikersinterface navigeren
* Uw StorSimple Virtual Array via de service StorSimple Device Manager beheren

> [!NOTE]
> Als u wilt weergeven van de opties beschikbaar voor de StorSimple 8000-apparaat, gaat u naar [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).
> 
> 

## <a name="connect-to-the-storsimple-device-manager-service"></a>Verbinding maken met de StorSimple Device Manager-service
De StorSimple Device Manager-service wordt uitgevoerd in Microsoft Azure en verbindt met meerdere virtuele StorSimple-matrices. U een centrale Microsoft Azure portal wordt uitgevoerd in een browser gebruiken om deze apparaten te beheren. Voor verbinding met de StorSimple Device Manager-service, het volgende te doen.

#### <a name="to-connect-to-the-service"></a>Verbinding maken met de service
1. Ga naar [https://ms.portal.azure.com](https://ms.portal.azure.com).
2. Met behulp van de referenties van uw Microsoft-account, meld u aan bij de Microsoft Azure-portal (te vinden in de rechterbovenhoek van het deelvenster).
3. Navigeren naar Bladeren--> 'Filter' op de StorSimple Device Manager-services om uw apparaatbeheerfuncties in een bepaald abonnement weer te geven.

## <a name="use-the-storsimple-device-manager-service-to-perform-management-tasks"></a>De StorSimple Device Manager-service gebruiken voor beheertaken uitvoeren
De volgende tabel bevat een overzicht van de algemene beheertaken en complexe werkstromen die kunnen worden uitgevoerd in de overzichtsblade van de StorSimple Device Manager-service. Deze taken zijn ingedeeld op basis van de blades waarop ze worden gestart.

Klik op de desbetreffende procedure in de tabel voor meer informatie over elke werkstroom.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager-werkstromen
| Als u dit wilt doen... | Gebruik deze procedure |
| --- | --- |
| Een service maken</br>Een service verwijderen</br>De serviceregistratiesleutel ophalen</br>De serviceregistratiesleutel opnieuw genereren |[De StorSimple Device Manager-service implementeren](storsimple-virtual-array-manage-service.md) |
| De activiteitenlogboeken bekijken |[Het overzicht van de service StorSimple gebruiken](storsimple-virtual-array-service-summary.md) |
| Een virtuele matrix deactiveren</br>Verwijderen van een virtuele matrix |[Deactiveren of verwijderen van een virtuele matrix](storsimple-virtual-array-deactivate-and-delete-device.md) |
| Disaster recovery en apparaat-failover</br>Vereisten voor failover</br>Zakelijke continuïteit noodherstel (BCDR)</br>Fouten tijdens het herstel na noodgevallen |[Disaster recovery en apparaat failover voor de StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md) |
| Back-up van bestandsshares en volumes</br>Een handmatige back-up maken</br>De back-upschema wijzigen</br>Bestaande back-ups weergeven |[Back-up van uw StorSimple Virtual Array](storsimple-virtual-array-backup.md) |
| Kloon-shares vanuit een back-upset</br>Volumes klonen van een back-upset</br>Herstel op itemniveau (bestandsserver) |[Klonen vanaf een back-up van uw StorSimple Virtual Array](storsimple-virtual-array-clone.md) |
| Over storage-accounts</br>Een storage-account toevoegen</br>Een storage-account bewerken</br>Een opslagaccount verwijderen |[Storage-accounts beheren voor de StorSimple Virtual Array](storsimple-virtual-array-manage-storage-accounts.md) |
| Over access control records</br>Toevoegen of wijzigen van een Acces controlerecord </br>Verwijderen van een access controlerecord |[Access control records voor de StorSimple Virtual Array beheren](storsimple-virtual-array-manage-acrs.md) |
| Taakdetails weergeven |[StorSimple Virtual Array taken beheren](storsimple-virtual-array-manage-jobs.md) |
| Instellingen voor waarschuwingen configureren</br>Waarschuwingen ontvangen</br>Waarschuwingen beheren</br>Waarschuwingen weergeven |[Waarschuwingen weergeven en beheren voor de StorSimple Virtual Array](storsimple-virtual-array-manage-alerts.md) |
| Het beheerderswachtwoord voor het apparaat wijzigen |[Wijzig het beheerderswachtwoord van de StorSimple Virtual Array](storsimple-virtual-array-change-device-admin-password.md) |
| Software-updates installeren |[Bijwerken van uw virtuele Array](storsimple-virtual-array-install-update.md) |

> [!NOTE]
> Moet u de [lokale web-UI](storsimple-ova-web-ui-admin.md) voor de volgende taken:
> 
> * [De versleutelingssleutel voor servicegegevens ophalen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
> * [Een ondersteuningspakket maken](storsimple-ova-web-ui-admin.md#generate-a-log-package)
> * [Stoppen en opnieuw starten van een virtuele matrix](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)
> 
> 

## <a name="next-steps"></a>Volgende stappen
Voor informatie over de web-UI en om deze te gebruiken, gaat u naar [de webgebruikersinterface van StorSimple gebruiken voor het beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

