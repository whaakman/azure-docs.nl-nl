---
title: StorSimple 8000 serie back-upbeleid beheren | Microsoft Docs
description: Legt uit hoe u kunt de service Manager voor StorSimple-apparaat maken en beheren van handmatige back-ups en back-upschema's bewaren van back-up op een apparaat StorSimple 8000-serie.
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
ms.workload: TBD
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 569dbfdeb7dcd526cb5a54b487ea1bfb59b13cc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-manage-backup-policies"></a>De service Manager voor StorSimple-apparaat in Azure-portal gebruiken voor het beheren van back-upbeleid


## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u de service Manager voor StorSimple-apparaat **back-up maken van beleid** blade om back-processen en bewaren van back-up voor uw StorSimple-volumes te beheren. Ook wordt beschreven hoe u een handmatige back-up uitvoeren.

Wanneer u back-up van een volume, kunt u kiezen voor het maken van een momentopname van een lokale of een cloudmomentopname van de. Als u een back-up van een lokaal vastgemaakt volume, raden wij aan dat u opgeeft dat een cloudmomentopname. Maken van een groot aantal lokale momentopnamen van een lokaal vastgemaakt volume samen met een gegevensset die een groot aantal verloop heeft resulteert in een situatie waarin u kan snel worden uitgevoerd buiten het lokale ruimte. Als u kiest voor het lokale momentopnamen, raden wij u momentopnamen minder dagelijkse back-up van de meest recente status behouden voor een dag en verwijder deze.

Wanneer u een cloudmomentopname van een lokaal vastgemaakt volume maakt, kunt u alleen de gewijzigde gegevens kopiëren naar de cloud, waar het ontdubbeld en gecomprimeerd.

## <a name="the-backup-policy-blade"></a>De blade back-upbeleid

De **back-up maken van beleid** blade voor uw StorSimple-apparaat kunt u back-upbeleid beheren en plannen van lokale en cloud worden opgeslagen. Back-beleidsregels worden gebruikt om back-upschema en de back-up van de bewaarperiode voor een verzameling van volumes te configureren. Back-upbeleid kunnen u een momentopname van meerdere volumes tegelijkertijd. Dit betekent dat de back-ups gemaakt door een back-upbeleid crashconsistent exemplaren.

De back-upbeleid in tabelvorm aanbieding kunt u het bestaande back-upbeleid door een of meer van de volgende velden te filteren:

* **De naam van beleid** : de naam gekoppeld aan het beleid. De verschillende soorten beleid zijn:

  * Geplande beleidsregels die expliciet zijn gemaakt door de gebruiker.
  * Geïmporteerde beleid, StorSimple Snapshot Manager oorspronkelijk zijn gemaakt. Deze hebben een code die de StorSimple Snapshot Manager-host die de beleidsregels zijn geïmporteerd beschrijft uit.

  > [!NOTE]
  > Automatische of standaard back-upbeleid zijn niet meer ingeschakeld op het moment van volume maken.

* **Laatste geslaagde back-up** – de datum en tijd van de laatste goede back-up die is uitgevoerd met dit beleid.

* **Volgende back-up** – de datum en tijd van de volgende geplande back-up die door dit beleid wordt gestart.

* **Volumes** – de volumes die zijn gekoppeld aan het beleid. Alle volumes die zijn gekoppeld aan een back-upbeleid worden gegroepeerd wanneer back-ups worden gemaakt.

* **Planningen** – het nummer van schema's die zijn gekoppeld aan de back-upbeleid.

De veelgebruikte bewerkingen die u voor back-upbeleid uitvoeren kunt zijn:

* Een back-upbeleid toevoegen
* Toevoegen of wijzigen van een planning
* Toevoegen of verwijderen van een volume
* Een back-upbeleid te verwijderen
* Maak een handmatige back-up

## <a name="add-a-backup-policy"></a>Een back-upbeleid toevoegen

Toevoegen van een back-upbeleid automatisch uw back-ups plannen. Wanneer u een volume maakt, is er geen back-standaardbeleid die zijn gekoppeld aan het volume. U wilt toevoegen en toewijzen van een back-upbeleid volumegegevens te beveiligen.

De volgende stappen uitvoeren in Azure portal voor het toevoegen van een back-upbeleid voor uw StorSimple-apparaat. Nadat u het beleid hebt toegevoegd, kunt u een schema definiëren (Zie [toevoegen of wijzigen van een planning](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Toevoegen of wijzigen van een planning

U kunt toevoegen of wijzigen van een schema dat is gekoppeld aan een bestaande back-upbeleid op uw StorSimple-apparaat. Voer de volgende stappen uit in de Azure-portal toevoegen of wijzigen van een planning.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Toevoegen of verwijderen van een volume

U kunt toevoegen of verwijderen van een volume dat is toegewezen aan een back-upbeleid op uw StorSimple-apparaat. Voer de volgende stappen uit in de Azure portal toevoegen of verwijderen van een volume.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>Een back-upbeleid te verwijderen

De volgende stappen uitvoeren in de Azure portal een back-upbeleid op uw StorSimple-apparaat verwijderen.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Maak een handmatige back-up

Voer de volgende stappen uit in de Azure-portal voor het maken van een back-up-op-verzoek (handmatig) voor één volume.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

