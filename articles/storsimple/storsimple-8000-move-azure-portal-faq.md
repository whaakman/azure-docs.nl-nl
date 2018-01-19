---
title: Verplaatsen van klassiek naar Azure-portal Veelgestelde vragen | Microsoft Docs
description: Vindt u antwoorden op veelgestelde vragen over het verplaatsen van StorSimple-apparaten van klassiek naar Azure-portal.
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
ms.workload: NA
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: ac57894e4f180f42f80479d2031f4dd5ddfdb1be
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="move-storsimple-device-manager-service-from-classic-to-azure-portal-frequently-asked-questions-faq"></a>StorSimple-apparaat Manager-service verplaatsen van klassiek naar Azure-portal: veelgestelde vragen (FAQ)

## <a name="overview"></a>Overzicht

Hieronder vindt u vragen en antwoorden die u hebt wanneer u verplaatst uw StorSimple-apparaat Manager-service uitgevoerd in de klassieke Azure-portal naar Azure-portal.

Vragen en antwoorden worden gerangschikt in de volgende categorieën:

* Het verplaatsen van service Manager voor StorSimple-apparaat
* Het verplaatsen van de storage-accounts
* Met Azure Resource Manager gebaseerde cmdlets
* Met behulp van StorSimple Data Manager-service
* Diversen

## <a name="moving-storsimple-device-manager-service"></a>Het verplaatsen van service Manager voor StorSimple-apparaat

### <a name="once-i-have-moved-to-azure-portal-can-i-still-create-a-storsimple-manager-service-in-the-classic-portal"></a>Eenmaal ik hebt verplaatst naar Azure-portal, kan ik daar nog steeds een StorSimple Manager-service in de klassieke portal maken?

Nee. Nadat u uw StorSimple Manager-service naar de Azure-portal hebt gemigreerd, kunt u een nieuwe service kan niet maken in de klassieke portal. Bovendien [de klassieke portal is niet beschikbaar is via 8 januari 2018](https://azure.microsoft.com/updates/azure-portal-updates-for-classic-portal-users). 

### <a name="i-have-multiple-storsimple-managers-running-in-the-classic-portal-can-i-choose-which-ones-to-move-to-the-azure-portal"></a>Ik heb meerdere StorSimple Managers uitgevoerd in de klassieke portal. Kan ik kiezen welke u wilt verplaatsen naar de Azure-portal?

Nee. U kiezen niet welke Managers StorSimple verplaatsen naar de Azure-portal. De StorSimple-beheerders in uw abonnement worden verplaatst.


### <a name="i-initiated-the-migration-of-my-service-to-the-new-azure-portal-should-i-delete-the-storsimple-manager-from-the-classic-portal"></a>Ik heb de migratie van mijn-service naar de nieuwe Azure portal hebt gestart. Moet ik de StorSimple Manager verwijderen vanuit de klassieke portal 

Nee. Probeer niet verwijderen van alle services die u hebt verplaatst vanuit de klassieke portal. Wacht totdat de migratie te voltooien, nadat alle StorSimple Managers hebt verplaatst naar de nieuwe portal, zou er geen services verwijderen vanuit de klassieke portal. Uiteindelijk is de klassieke portal afgeschaft.

### <a name="can-i-rename-my-storsimple-device-manager-service-in-the-azure-portal"></a>Kan ik mijn Apparaatbeheer StorSimple-service in de Azure portal wijzigen?

Nee. Naam van de service kan niet worden gewijzigd nadat de service is gemaakt in de Azure-portal. Hetzelfde gedrag geldt ook voor andere entiteiten, zoals apparaten, volumes, volumecontainers en back-upbeleid.

### <a name="can-i-create-the-80108020-storsimple-cloud-appliances-with-azure-resource-manager-deployment-model"></a>Kan ik de 8010/8020 StorSimple Cloud toestellen maken met Azure Resource Manager-implementatiemodel

Ja. U kunt nieuwe 8010/8020 StorSimple Cloud toestellen maken in het Azure Resource Manager-implementatiemodel. De onderliggende virtuele machines voor deze cloud-apparaten zijn ook in het Resource Manager-implementatiemodel.

### <a name="when-we-migrate-subscriptions-to-the-azure-portal-will-the-underlying-vms-for-the-storsimple-cloud-appliances-also-migrate-to-azure-resource-management-deployment-model"></a>Wanneer we abonnementen naar de Azure-portal migreert, wordt de onderliggende virtuele machines voor de Cloud StorSimple toestellen ook migreren naar Azure Resource Management-implementatiemodel?

Het verplaatsen van de StorSimple-service is afhankelijk van het beheer van de virtuele machines voor de StorSimple-Cloud-apparaten. U kunt volledig beheren de virtuele machines voor StorSimple Cloud toestellen in zowel het klassieke en de Azure-portal zelfs vandaag.

### <a name="can-i-manage-existing-classic-80108020-storsimple-cloud-appliance-from-the-azure-portal"></a>Kan ik bestaande klassieke 8010/8020 StorSimple Cloud toestel beheren vanuit de Azure-portal?

Ja. De virtuele machines die zijn gekoppeld aan bestaande 8010/8020 cloud-apparaten kunnen worden beheerd vanaf de Azure-portal.

Als u hebt gemaakt StorSimple Cloud toestellen model 8010/8020 met Update 3.0 en hoger, u niet beïnvloed door uw service verplaatsen naar de nieuwe Azure portal. U moet mogelijk uw cloud-apparaten zonder problemen volledig te beheren. 

Als u cloud toestellen versies vóór de Update 3.0 wordt uitgevoerd in de klassieke portal hebt, klikt beperkt u alleen functionaliteit beschikbaar. Ga voor meer informatie naar de [lijst met niet-ondersteunde bewerkingen voor apparaten met eerdere versies Update 3](storsimple-8000-manage-service.md##supported-operations-on-devices-running-versions-prior-to-update-50).

Een cloudapparaat kan niet worden bijgewerkt. Gebruik de nieuwste versie van software voor het maken van een nieuw cloud toestel en voert vervolgens failover uit via de bestaande volumecontainers op het nieuwe cloud toestel gemaakt. Ga voor meer informatie naar [failover naar de cloud-apparaat](storsimple-8000-cloud-appliance-u2.md#fail-over-to-the-cloud-appliance)


### <a name="my-storsimple-8000-series-device-is-running-update-20-i-migrated-my-service-to-new-azure-portal-my-device-connected-successfully-but-it-seems-that-i-am-not-able-to-fully-manage-my-device-how-do-i-resolve-this-behavior"></a>Mijn apparaat StorSimple 8000-serie kan Update 2.0 wordt uitgevoerd. Ik mijn service gemigreerd naar de nieuwe Azure portal. Mijn apparaat is verbonden, maar het lijkt erop dat ik niet volledig beheren mijn apparaat. Hoe kan ik dit probleem oplossen?

De nieuwe Azure portal wordt alleen ondersteund voor StorSimple-apparaten met Update 3.0 en hoger. Als het apparaat werd Update 2.0 worden uitgevoerd, hebt u alleen beschikbaar voor dit apparaat functionaliteit beperkt. Ga voor meer informatie naar de [lijst met niet-ondersteunde bewerkingen voor apparaten met eerdere versies Update 3](storsimple-8000-manage-service.md##supported-operations-on-devices-running-versions-prior-to-update-50).

Volledig beheer van uw apparaat, de meest recente update te installeren op uw apparaat. Ga voor meer informatie naar [installeren Update 5](storsimple-8000-install-update-5.md).

### <a name="i-just-moved-my-storsimple-manager-service-to-the-azure-portal-i-am-seeing-some-alerts-related-to-my-device-is-this-behavior-related-to-the-move"></a>Ik mijn StorSimple Manager-service Net is verplaatst naar de Azure-portal. Ik ben bepaalde waarschuwingen die betrekking hebben op mijn apparaat te zien. Is dit gedrag gerelateerd aan de verplaatsing?

Nee. De verplaatsing zelf mag niet resulteren in fouten of waarschuwingen. Volg de waarschuwing aanbevelingen voor de waarschuwingen oplost.

### <a name="i-am-using-a-storsimple-50007000-series-device-are-these-also-supported-in-the-azure-portal"></a>Ik gebruik een apparaat StorSimple 5000/7000-serie. Zijn deze ook ondersteund in de Azure portal?

Nee. Apparaten uit de serie StorSimple 5000/7000 worden niet ondersteund in de Azure portal.

### <a name="i-am-planning-to-migrate-data-from-storsimple-50007000-series-device-to-a-storsimple-8000-series-device-how-does-moving-a-service-to-azure-portal-impact-my-data-migration"></a>Ik ben plannen om gegevens te migreren van StorSimple 5000/7000-serie apparaat op een apparaat StorSimple 8000-serie. Hoe het verplaatsen van een service naar de Azure-portal invloed heeft mijn gegevensmigratie? 

U kunt gegevens van uw apparaat StorSimple 5000/7000-serie migreren naar een StorSimple 8000 series apparaat uitgevoerd in Azure-portal. Zodat u voor de gegevensmigratie uit 5000/7000-serie op 8000-serie, moet u [Meld een ondersteuningsticket met Microsoft Support](storsimple-8000-contact-microsoft-support.md).


## <a name="moving-subscriptions-storage-accounts-resource-groups"></a>Abonnementen, opslagaccounts, brongroepen verplaatsen

### <a name="can-i-move-storsimple-device-manager-from-one-subscription-to-another-subscription-in-the-azure-portal"></a>Kan ik StorSimple Apparaatbeheer uit één abonnement verplaatsen naar een ander abonnement in de Azure portal?

Nee. StorSimple-apparaat Manager-service van een abonnement te verplaatsen naar een andere wordt niet ondersteund. U kunt een handmatig proces dat bestaat uit de volgende stappen uitvoeren:

* Migreren van gegevens uit het StorSimple-apparaat.
* Uitvoeren van de fabrieksinstellingen van het apparaat, hierdoor alle lokale gegevens op het apparaat wordt verwijderd.
* Registreer het apparaat met het nieuwe abonnement op een StorSimple-apparaat Manager-service.
* Migreren van de gegevens terug naar het apparaat.

### <a name="do-i-have-to-migrate-the-storage-account-to-azure-resource-manager-deployment-model"></a>Heb ik het storage-account te migreren naar Azure Resource Manager-implementatiemodel?

Nee. Uw klassieke opslagaccounts kunnen volledig worden beheerd vanaf de Azure-portal. Wanneer u uw StorSimple-service naar de Azure-portal hebt verplaatst, blijft uw storage-account functioneren als voorheen.

### <a name="what-happens-to-the-storage-account-after-the-service-is-migrated-to-the-azure-portal"></a>Wat gebeurt er met de storage-account nadat de service is gemigreerd naar de Azure-portal?

De verplaatsing van de service is niet gerelateerd aan het beheer van storage-account. Zowel classic en Azure Resource Manager-opslagaccounts kunnen volledig worden beheerd in de Azure-portal. Zodra u uw service naar de Azure-portal verplaatst, het apparaat moet worden uitgevoerd met dit opslagaccount en moet er niet van invloed op uw gegevens.

### <a name="can-i-migrate-storsimple-device-manager-from-one-resource-group-to-another"></a>Kan ik StorSimple Apparaatbeheer migreren van een resourcegroep naar een andere?

Nee. U kunt een StorSimple-Apparaatbeheer gemaakt met een resourcegroep naar een andere resourcegroep niet verplaatsen.

## <a name="using-azure-resource-manager-based-cmdlets"></a>Met Azure Resource Manager gebaseerde cmdlets

### <a name="i-moved-to-the-new-azure-portal-now-my-scripts-based-on-azure-classic-deployment-model-powershell-cmdlets-are-not-working-what-do-i-need-to-do"></a>Ik verplaatst naar de nieuwe Azure portal. Mijn scripts op basis van de PowerShell-cmdlets voor Azure classic deployment model werkt nu niet? Wat moet ik doen?

De bestaande Azure classic deployment model PowerShell-cmdlets worden niet ondersteund in de Azure portal. Werk de scripts voor het beheren van uw apparaten via de Azure Resource Manager. Voor meer informatie over het bijwerken van uw scripts, gaat u naar [steekproeven voor de nieuwe Azure portal](https://github.com/anoobbacker/storsimpledevicemgmttools).

### <a name="i-just-moved-to-the-azure-portal-and-needed-to-roll-over-the-service-data-encryption-key-where-is-this-option-in-the-azure-portal"></a>Ik zojuist hebt verplaatst naar de Azure-portal en die nodig zijn voor het overschakelen van de gegevensversleutelingssleutel van service. Waar wordt deze optie in de Azure portal?

De optie voor het overschakelen van de gegevensversleutelingssleutel van service is niet in de Azure portal. Voor meer informatie over hoe u deze rollover in Azure-portal, gaat u naar [wijzigen van de gegevensversleutelingssleutel van service](storsimple-8000-manage-service.md#change-the-service-data-encryption-key).

### <a name="i-am-using-windows-powershell-for-storsimple-cmdlets-on-the-storsimple-device-to-perform-operations-such-extract-a-support-package-are-these-cmdlets-affected-when-i-move-to-the-new-azure-portal"></a>Ik gebruik Windows PowerShell voor StorSimple-cmdlets op het StorSimple-apparaat bewerkingen uit te voeren dergelijke extract ondersteuningspakket. Zijn deze cmdlets beïnvloed wanneer ik naar de nieuwe Azure portal verplaatsen?

Nee. Met uw service verplaatsen naar de nieuwe Azure portal, moet er niet van invloed op de Windows PowerShell voor StorSimple-cmdlets die zijn gekoppeld aan de on-premises StorSimple-apparaat (welke zelf wordt niet beïnvloed door de verplaatsing). U kunt blijven deze cmdlets gebruiken om het maken van een ondersteuningspakket zonder problemen zelfs in de Azure-portal. Alleen de klassieke Azure-implementatie model PowerShell-cmdlets zijn beïnvloed door deze verplaatsen.

## <a name="moving-storsimple-data-manager-service"></a>Het verplaatsen van StorSimple Data Manager-service

### <a name="i-am-using-storsimple-data-manager-service-in-classic-azure-portal-how-should-i-proceed-with-this-move"></a>Ik gebruik StorSimple Data Manager-service in de klassieke Azure-portal. Hoe moet ik wilt u doorgaan met deze verplaatsen?

Als u StorSimple Data Manager-service gebruikt, hebt u automatisch is verplaatst naar de Azure-portal.

## <a name="miscellaneous"></a>Diversen

### <a name="i-am-running-storsimple-snapshot-manager-for-my-8000-series-device-is-there-any-impact-on-storsimple-snapshot-manager-when-i-move-to-azure-portal"></a>Ik ben mijn apparaat 8000-serie StorSimple Snapshot Manager uitgevoerd. Is er geen gevolgen voor StorSimple Snapshot Manager wanneer ik naar Azure-portal?

Nee. Er zijn geen gevolgen voor StorSimple Snapshot Manager wanneer u uw service naar Azure-portal verplaatst. Het apparaat en het StorSimple Snapshot Manager blijven functioneren als voorheen.

### <a name="can-i-rename-my-storsimple-device-volume-containers-or-volumes"></a>Kan ik mijn StorSimple-apparaat, volumecontainers of volumes wijzigen?

Nee. U kunt niet hernoemen apparaten, volumes, volumecontainers of back-upbeleid in de Azure-portal.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [bewerkingen op apparaten met eerdere versies Update 5.0 ondersteund](storsimple-8000-manage-service.md#supported-operations-on-devices-running-versions-prior-to-update-50).



