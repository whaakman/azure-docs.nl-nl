---
title: Storage-accounts, abonnementen voor uw StorSimple Device Manager-service migreren | Microsoft Docs
description: Informatie over het migreren van abonnementen, storage-accounts voor uw StorSimple Device Manager-service8000.
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
ms.date: 06/08/2018
ms.author: alkohli
ms.openlocfilehash: 5a0da47b854e625f2f4a2fcf4c95ec566ba63093
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646445"
---
# <a name="migrate-subscriptions-and-storage-accounts-associated-with-storsimple-device-manager-service"></a>Migreren van abonnementen en opslagaccounts die zijn gekoppeld met StorSimple Device Manager-service

Mogelijk moet u uw StorSimple-service verplaatsen naar een nieuwe inschrijving of naar een nieuw abonnement. Deze scenario's voor migratie zijn Accountwijzigingen of datacenter-wijzigingen. Gebruik de volgende tabel om te begrijpen welke van deze scenario's worden ondersteund met inbegrip van de gedetailleerde stappen voor het verplaatsen.

## <a name="account-changes"></a>Accountwijzigingen

| U kunt verplaatsen...| Ondersteund| Downtime| Azure ondersteuningsproces| Methode|
|-----|-----|-----|-----|-----|
| Een volledige abonnement (met inbegrip van StorSimple-service en storage-accounts) op een andere registratie? | Ja       | Nee       | **Overdracht van inschrijving**<br>U kunt gebruiken:<li>Wanneer u een nieuwe toezegging voor Azure kopen onder een nieuwe overeenkomst.</li><li>U wilt migreren van alle accounts en abonnementen van de oude registratie naar de nieuwe. Dit omvat alle Azure-services onder de oude abonnement.</li> | **Stap 1: Een Azure Enterprise bewerking-ondersteuningsticket openen.**<li>Ga naar [http://aka.ms/AzureEntSupport](http://aka.ms/AzureEntSupport).</li><li> Selecteer **inschrijving beheer** en selecteer vervolgens **overzetten van een inschrijving naar een nieuwe inschrijving**.<br>**Stap 2: Geef de gevraagde informatie**<br>Zijn onder andere:<li>bron-inschrijvingsnummer</li><li> doel-inschrijvingsnummer</li><li>de ingangsdatum overdracht|
| StorSimple-service van een bestaand account naar een nieuwe inschrijving?    | Ja       | Nee       | **Accountoverdracht**<br>U kunt gebruiken:<li>Als u wilt niet een volledige inschrijving-overdracht.</li><li>U wilt dat alleen bepaalde accounts verplaatsen naar een nieuwe inschrijving.</li>| **Stap 1: Een Azure Enterprise bewerking-ondersteuningsticket openen.**<li>Ga naar [http://aka.ms/AzureEntSupport](http://aka.ms/AzureEntSupport).</li><li>Selecteer **inschrijving beheer** en selecteer vervolgens **een EA-Account overdragen naar een nieuwe inschrijving**.<br>**Stap 2: Geef de gevraagde informatie**<br>Zijn onder andere:<li>bron-inschrijvingsnummer</li><li> doel-inschrijvingsnummer</li><li>de ingangsdatum overdracht|
| StorSimple-service van één abonnement naar een ander abonnement?      | Nee        |    Ja         | None, handmatig proces|<li>Migreren van gegevens uit de StorSimple-apparaat.</li><li>Fabrieksinstellingen terugzetten van het apparaat uitvoeren, Hiermee verwijdert u alle lokale gegevens op het apparaat.</li><li>Registreer het apparaat met het nieuwe abonnement op een StorSimple Device Manager-service.</li><li>Migreren van de gegevens terug naar het apparaat.|
  |Kan ik de eigendom van een Azure-abonnement overbrengen naar een andere directory? | Ja       | Nee       | Een bestaand abonnement aan uw Azure AD-directory koppelen | Raadpleeg [om te koppelen van een bestaand abonnement aan uw Azure AD-directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md). Het kan maximaal 10 minuten duren voordat alles goed wordt weergegeven.|
| StorSimple-apparaat bij een StorSimple Device Manager-service op een andere service in een andere regio?      | Nee        | Ja            | None, handmatig proces |Hetzelfde als hierboven.|
| Storage-account aan een nieuw abonnement of resourcegroep groep?     | Ja        | Nee             |Storage-account verplaatsen naar andere abonnement of de resource-groep |Na de verplaatsing als de opslagaccountsleutels zijn bijgewerkt, moet de gebruiker de toegangssleutel voor de gemigreerde storage-account via de StorSimple Device Manager-service handmatig configureren.|
| Klassieke storage-account naar een Azure Resource Manager-opslagaccount      | Ja        | Nee             |Migreren van klassiek naar Azure Resource Manager |<li>Voor gedetailleerde instructies over het migreren van een opslagaccount van klassiek naar Azure Resource Manager, gaat u naar [migreren van een klassiek opslagaccount](../virtual-machines/windows/migration-classic-resource-manager-ps.md#step-62-migrate-a-storage-account).</li><li> Als de toegangssleutels voor opslag account na de migratie zijn bijgewerkt, moet de gebruiker om te synchroniseren van de toegangssleutels voor de gemigreerde storage-account via de StorSimple Device Manager-service. Dit is om te controleren of de StorSimple-apparaten blijven normaal functioneren en kunnen gegevens in lagen primaire/back-up in Azure. Voor gedetailleerde instructies voor het synchroniseren van de toegang tot sleutels, gaat u naar [rotatie werkstroom](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts).</li><li> In het geval van een StorSimple-Cloudapparaat als het klassieke storage-account wordt gemigreerd, maar de onderliggende virtuele machine nog steeds in de klassieke versie, blijft moet het apparaat correct functioneren. Als de onderliggende virtuele machine voor het cloudapparaat wordt gemigreerd, en vervolgens de functionaliteit deactiveren en verwijderen niet werken.</li><li> U moet een nieuwe StorSimple-Cloudapparaten maken in Azure portal en vervolgens een failover van de oudere cloud appliances. U kunt een StorSimple-Cloudapparaat in de nieuwe Azure-portal met behulp van een klassiek opslagaccount maken, moeten een Azure Resource Manager-opslagaccount hebben. Ga voor meer informatie naar [implementeren en beheren van een StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</li>|Hetzelfde als hierboven.|

## <a name="datacenter-changes"></a>Datacenter-wijzigingen

| U kunt verplaatsen...| Ondersteund|Downtime| Azure ondersteuningsproces| Methode|
|-----|-----|-----|-----|-----|
| Een StorSimple-service vanuit een Azure-datacenter naar de andere? | Nee | Ja |None, handmatig proces  |<li>Migreren van gegevens uit de StorSimple-apparaat.</li><li>Fabrieksinstellingen terugzetten van het apparaat uitvoeren, Hiermee verwijdert u alle lokale gegevens op het apparaat.</li><li>Registreer het apparaat met het nieuwe abonnement naar een nieuwe StorSimple Device Manager-service.</li><li>Migreren van de gegevens terug naar het apparaat.|
| Een opslagaccount van het ene Azure-datacenter naar de andere? | Nee |Ja  |None, handmatig proces  | Hetzelfde als hierboven.|

## <a name="next-steps"></a>Volgende stappen

* [StorSimple Device Manager-service implementeren](storsimple-8000-manage-service.md)
* [StorSimple 8000-apparaat in Azure portal implementeren](storsimple-8000-deployment-walkthrough-u2.md)
