---
title: Back-upbeleid van StorSimple 8000-serie beheren | Microsoft Docs
description: Wordt uitgelegd hoe u de service StorSimple Device Manager maken en beheren van handmatige back-ups, back-upschema's en back-upretentie op een StorSimple 8000-apparaat kunt gebruiken.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 569dbfdeb7dcd526cb5a54b487ea1bfb59b13cc6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38681580"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-manage-backup-policies"></a>De StorSimple Device Manager-service in Azure portal gebruiken voor back-upbeleid beheren


## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u de service StorSimple Device Manager **back-upbeleid** blade voor het beheren van back-processen en back-upretentie voor uw StorSimple-volumes. Ook wordt beschreven hoe u een handmatige back-up.

Wanneer u back-up van een volume, kunt u kiezen om een momentopname van een lokale of een cloud-momentopname te maken. Als u een back-up een lokaal vastgemaakt volume, wordt u aangeraden dat u opgeeft dat een cloud-momentopname. Een groot aantal lokale momentopnamen van een lokaal vastgemaakt volume is gekoppeld aan een gegevensset die een groot aantal verloop heeft resulteert in een situatie waarin u snel geen lokale schijfruimte meer uitvoeren kunt. Als u kiest voor lokale momentopnamen worden gemaakt, raden wij u minder dagelijkse momentopnamen maakt u een back-up van de meest recente status behouden voor een dag duren en verwijder deze.

Wanneer u een cloudmomentopname van een lokaal vastgemaakt volume maakt, kunt u alleen de gewijzigde gegevens kopiëren naar de cloud, waar deze is ontdubbeld en gecomprimeerd.

## <a name="the-backup-policy-blade"></a>De blade back-upbeleid

De **back-upbeleid** blade voor uw StorSimple-apparaat kunt u back-upbeleid beheren en plannen van lokale en cloud-momentopnamen. Back-upbeleid worden gebruikt om back-upschema's en back-upretentie voor een verzameling van volumes te configureren. Back-upbeleid kunnen u een momentopname van meerdere volumes tegelijkertijd. Dit betekent dat de back-ups die zijn gemaakt door een back-upbeleid crash-consistente kopieën zijn.

De lijst in tabelvorm back-upbeleid kunt u filteren op de bestaande back-upbeleid door een of meer van de volgende velden:

* **De naam van beleid** – de naam die is gekoppeld aan het beleid. De verschillende soorten beleid zijn onder andere:

  * Geplande beleidsregels, die expliciet zijn gemaakt door de gebruiker.
  * Geïmporteerde beleidsregels, die zijn gemaakt in de StorSimple Snapshot Manager. Deze moeten een label dat de beschrijving van de StorSimple Snapshot Manager-host die de beleidsregels zijn geïmporteerd uit.

  > [!NOTE]
  > Automatische of standaard back-upbeleid zijn niet meer ingeschakeld op het moment van volume maken.

* **Laatste geslaagde back-up** – de datum en tijd van de laatste geslaagde back-up die is gemaakt met dit beleid.

* **Volgende back-up** – de datum en tijd van de volgende geplande back-up die door dit beleid wordt gestart.

* **Volumes** – de volumes die zijn gekoppeld aan het beleid. Alle volumes die zijn gekoppeld aan een back-upbeleid zijn gegroepeerd wanneer back-ups worden gemaakt.

* **Schema's** : het aantal planningen die zijn gekoppeld aan het back-upbeleid.

De gebruikte bewerkingen die u voor back-upbeleid uitvoeren kunt zijn:

* Een back-upbeleid toevoegen
* Toevoegen of wijzigen van een planning
* Toevoegen of verwijderen van een volume
* Verwijderen van een back-upbeleid
* Een handmatige back-up maken

## <a name="add-a-backup-policy"></a>Een back-upbeleid toevoegen

Een back-upbeleid automatisch uw back-ups plannen toevoegen. Wanneer u een volume voor het eerst maakt, is er geen standaard back-upbeleid dat is gekoppeld aan het volume. U wilt toevoegen en toewijzen van een back-upbeleid voor het beveiligen van volumegegevens.

De volgende stappen uitvoeren in Azure portal om een back-upbeleid voor uw StorSimple-apparaat toevoegen. Nadat u het beleid hebt toegevoegd, kunt u een schema definiëren (Zie [toevoegen of wijzigen van een schema](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Toevoegen of wijzigen van een planning

U kunt toevoegen of wijzigen van een schema dat is gekoppeld aan een bestaande back-upbeleid op uw StorSimple-apparaat. De volgende stappen uitvoeren in Azure portal toevoegen of wijzigen van een schema.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Toevoegen of verwijderen van een volume

U kunt toevoegen of verwijderen van een volume dat is toegewezen aan een back-upbeleid op uw StorSimple-apparaat. Voer de volgende stappen uit in de Azure-portal toevoegen of verwijderen van een volume.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>Verwijderen van een back-upbeleid

De volgende stappen uitvoeren in Azure portal om een back-upbeleid op uw StorSimple-apparaat verwijderen.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Een handmatige back-up maken

De volgende stappen uitvoeren in Azure portal om te maken van een back-up op-aanvraag (handmatig) voor één volume.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

