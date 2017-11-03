---
title: Storage-accounts, abonnementen voor uw StorSimple-apparaat Manager service migreren | Microsoft Docs
description: Informatie over het migreren van storage-accounts voor uw StorSimple-Apparaatbeheer service8000-abonnementen.
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
ms.date: 09/26/2017
ms.author: alkohli
ms.openlocfilehash: edac2635086106f83b30316ab190bead5245f195
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-subscriptions-and-storage-accounts-associated-with-storsimple-device-manager-service"></a>Migreren van abonnementen en opslagaccounts die zijn gekoppeld aan de StorSimple-apparaat Manager-service

Mogelijk moet u uw StorSimple-service verplaatsen naar een nieuwe registratie of naar een nieuw abonnement. Deze scenario's voor migratie zijn wijzigingen-account of datacenter-wijzigingen. Gebruik de volgende tabel om te begrijpen welke van deze scenario's worden ondersteund met inbegrip van de gedetailleerde stappen voor het verplaatsen.

## <a name="account-changes"></a>Wijzigingen in-account

| U kunt verplaatsen...| Ondersteund| Uitvaltijd| Azure Support-proces| Benadering|
|-----|-----|-----|-----|-----|
| Een volledige abonnement (inclusief StorSimple-service en storage-accounts) op een andere registratie? | Ja       | Nee       | **Overdracht van de inschrijving**<br>Gebruik:<li>Wanneer u een nieuwe Azure toezegging aanschaffen onder een nieuwe overeenkomst.</li><li>U wilt migreren van accounts en -abonnementen van de inschrijving van het oude naar het nieuwe. Dit omvat alle Azure-services onder het oude abonnement.</li> | **Stap 1: Open een Azure-ondersteuningsticket Enterprise bewerking.**<li>Ga naar [http://aka.ms/AzureEnt](http://aka.ms/AzureEnt).</li><li> Selecteer **inschrijving beheer** en selecteer vervolgens **van één inschrijving te zetten naar een nieuwe registratie**.<br>**Stap 2: Voer de vereiste gegevens**<br>Zijn onder andere:<li>bron inschrijvingsnummer</li><li> bestemming inschrijvingsnummer</li><li>de ingangsdatum overdracht|
| StorSimple-service van een bestaand account naar een nieuwe registratie?    | Ja       | Nee       | **Account overdracht**<br>Gebruik:<li>Wanneer u niet wilt dat de overdracht van een volledige inschrijving.</li><li>U wilt dat alleen bepaalde accounts verplaatsen naar een nieuwe registratie.</li>| **Stap 1: Open een Azure-ondersteuningsticket Enterprise bewerking.**<li>Ga naar [http://aka.ms/AzureEnt](http://aka.ms/AzureEnt).</li><li>Selecteer **inschrijving beheer** en selecteer vervolgens **een EA Account overdragen naar een nieuwe registratie**.<br>**Stap 2: Voer de vereiste gegevens**<br>Zijn onder andere:<li>bron inschrijvingsnummer</li><li> bestemming inschrijvingsnummer</li><li>de ingangsdatum overdracht|
| StorSimple-service van een abonnement aan een ander abonnement?      | Nee        |    Ja         | None, handmatig proces|<li>Migreren van gegevens uit het StorSimple-apparaat.</li><li>Uitvoeren van de fabrieksinstellingen van het apparaat, hierdoor alle lokale gegevens op het apparaat wordt verwijderd.</li><li>Registreer het apparaat met het nieuwe abonnement op een StorSimple-apparaat Manager-service.</li><li>Migreren van de gegevens terug naar het apparaat.|
| StorSimple-apparaat van de ene Apparaatbeheer StorSimple-service op een andere service in een andere regio?      | Nee        | Ja            | None, handmatig proces |Hetzelfde als hierboven.|
| Storage-account aan een nieuwe abonnement of resourcegroep groep?     | Ja        | Nee             |Storage-account te verplaatsen naar andere abonnement of de resource-groep |Na het verplaatsen als het account toegangssleutels voor opslag zijn bijgewerkt, moet de gebruiker de sneltoetsen voor de gemigreerde storage-account via de service Manager voor StorSimple-apparaat handmatig configureren.|
| Klassieke storage-account met een Azure Resource Manager-opslagaccount      | Ja        | Nee             |Migreren van klassiek naar Azure Resource Manager |<li>Ga voor gedetailleerde instructies voor het migreren van een opslagaccount van klassiek naar Azure Resource Manager naar [migreren van een klassieke opslagaccount](../virtual-machines/windows/migration-classic-resource-manager-ps.md#step-62-migrate-a-storage-account).</li><li> Als de toegangssleutels voor opslag account na de migratie zijn bijgewerkt, moet de gebruiker om te synchroniseren van de toegangssleutels voor de gemigreerde storage-account via de service Manager voor StorSimple-apparaat. Dit is om te controleren of de StorSimple-apparaten blijven normaal functioneren en kunnen laag van primaire/back-up van gegevens naar Azure. Ga voor gedetailleerde instructies voor het synchroniseren van de toegangssleutels naar [rotatie werkstroom](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts).</li><li> In het geval van een apparaat van de Cloud StorSimple als het klassieke storage-account wordt gemigreerd, maar de onderliggende virtuele machine nog steeds in de klassieke, blijft behoren het toestel. Als de onderliggende virtuele machine voor het toestel cloud wordt gemigreerd, dan de deactiveren en verwijderen-functionaliteit niet werkt.</li><li> U moet een nieuwe StorSimple Cloud toestellen maken in de Azure portal en vervolgens een failover uitvoeren van de oudere apparaten van de cloud. U kunt een StorSimple-apparaat met Cloud maken in de nieuwe Azure portal een klassieke opslagaccount gebruikt, moeten een Azure Resource Manager-storage-account hebben. Ga voor meer informatie naar [implementeren en beheren van een StorSimple-apparaat met Cloud](storsimple-8000-cloud-appliance-u2.md).</li>|Hetzelfde als hierboven.|

## <a name="datacenter-changes"></a>Datacenter-wijzigingen

| U kunt verplaatsen...| Ondersteund|Uitvaltijd| Azure Support-proces| Benadering|
|-----|-----|-----|-----|-----|
| Een StorSimple-service van een Azure-datacenter naar de andere? | Nee | Ja |None, handmatig proces  |<li>Migreren van gegevens uit het StorSimple-apparaat.</li><li>Uitvoeren van de fabrieksinstellingen van het apparaat, hierdoor alle lokale gegevens op het apparaat wordt verwijderd.</li><li>Registreer het apparaat met het nieuwe abonnement op een nieuwe StorSimple Device Manager-service.</li><li>Migreren van de gegevens terug naar het apparaat.|
| Een opslagaccount van een Azure-datacenter naar de andere? | Nee |Ja  |None, handmatig proces  | Hetzelfde als hierboven.|

## <a name="next-steps"></a>Volgende stappen

* [Implementeer Apparaatbeheer StorSimple-service](storsimple-8000-manage-service.md)
* [StorSimple 8000 series apparaat in Azure-portal implementeren](storsimple-8000-deployment-walkthrough-u2.md)
