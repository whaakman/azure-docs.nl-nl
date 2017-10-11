---
title: Beheren van uw StorSimple-back-upbeleid | Microsoft Docs
description: Legt uit hoe u kunt de StorSimple Manager-service maken en beheren van handmatige back-ups en back-upschema's bewaren van back-up.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 4a2db707-bbfc-425c-bfeb-bc5c97781873
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 5448247428ab96887470c6b53f7a9b3dcd9238f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>De StorSimple Manager-service gebruiken voor het beheren van back-upbeleid (Update 2)
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe u de StorSimple Manager-service **back-upbeleid** pagina om back-processen en bewaren van back-up voor uw StorSimple-volumes te beheren. Ook wordt beschreven hoe u een handmatige back-up uitvoeren.

Wanneer u back-up van een volume, kunt u kiezen voor het maken van een momentopname van een lokale of een cloudmomentopname van de. Als u een back-up van een lokaal vastgemaakt volume, raden wij aan dat u opgeeft dat een cloudmomentopname. Maken van een groot aantal lokale momentopnamen van een lokaal vastgemaakt volume samen met een gegevensset die een groot aantal verloop heeft resulteert in een situatie waarin u kan snel worden uitgevoerd buiten het lokale ruimte. Als u kiest voor het lokale momentopnamen, raden wij u momentopnamen minder dagelijkse back-up van de meest recente status behouden voor een dag en verwijder deze.

Wanneer u een cloudmomentopname van een lokaal vastgemaakt volume maakt, kunt u alleen de gewijzigde gegevens kopiëren naar de cloud, waar het ontdubbeld en gecomprimeerd. 

## <a name="the-backup-policies-page"></a>De pagina back-upbeleid
De **back-upbeleid** op de pagina kunt u back-upbeleid beheren en plannen van lokale en cloud worden opgeslagen. (Back-upbeleid worden gebruikt om back-upschema en de back-up van de bewaarperiode voor een verzameling van volumes te configureren.) Back-upbeleid kunnen u een momentopname van meerdere volumes tegelijkertijd. Dit betekent dat de back-ups gemaakt door een back-upbeleid crashconsistent exemplaren. De **back-upbeleid** pagina geeft een lijst van de back-upbeleid, de typen, de gekoppelde volumes, het aantal back-ups behouden en de optie voor het inschakelen van deze beleidsregels.

De **back-upbeleid** op de pagina kunt u het bestaande back-upbeleid door een of meer van de volgende velden te filteren:

* **De naam van beleid** : de naam gekoppeld aan het beleid. De verschillende soorten beleid zijn:
  
  * Geplande beleidsregels die expliciet zijn gemaakt door de gebruiker.
  * Automatische beleidsregels die worden gemaakt wanneer de standaardback-up voor deze optie volume is ingeschakeld op het moment van volume maken. Deze beleidsregels worden benoemd als *VolumeName*_Standaard waar *VolumeName* verwijst naar de naam van het StorSimple-volume dat is geconfigureerd door de gebruiker in de klassieke Azure portal. De automatische beleid resulteert in dagelijkse cloudmomentopnamen beginnen bij 22.30 tijd op het apparaat.
  * Geïmporteerde beleid, StorSimple Snapshot Manager oorspronkelijk zijn gemaakt. Deze hebben een code die de StorSimple Snapshot Manager-host die de beleidsregels zijn geïmporteerd beschrijft uit.
* **Volumes** – de volumes die zijn gekoppeld aan het beleid. Alle volumes die zijn gekoppeld aan een back-upbeleid worden gegroepeerd wanneer back-ups worden gemaakt.
* **Laatste geslaagde back-up** – de datum en tijd van de laatste goede back-up die is uitgevoerd met dit beleid.
* **Volgende back-up** – de datum en tijd van de volgende geplande back-up die door dit beleid wordt gestart.
* **Planningen** – het nummer van schema's die zijn gekoppeld aan de back-upbeleid.

De veelgebruikte bewerkingen die u op deze pagina uitvoeren kunt zijn:

* Een back-upbeleid toevoegen 
* Toevoegen of wijzigen van een planning 
* Een back-upbeleid te verwijderen 
* Maak een handmatige back-up 
* Een aangepaste back-upbeleid maken met meerdere volumes en schema 's 

## <a name="add-a-backup-policy"></a>Een back-upbeleid toevoegen
Toevoegen van een back-upbeleid automatisch uw back-ups plannen. Voer de volgende stappen uit in de klassieke Azure portal een back-upbeleid voor uw StorSimple-apparaat toevoegen. Nadat u het beleid hebt toegevoegd, kunt u een schema definiëren (Zie [toevoegen of wijzigen van een planning](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Video beschikbaar](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video beschikbaar**

Als u wilt een video over het maken van een lokale of back-upbeleid cloud bekijken, klikt u op [hier](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).

## <a name="add-or-modify-a-schedule"></a>Toevoegen of wijzigen van een planning
U kunt toevoegen of wijzigen van een schema dat is gekoppeld aan een bestaande back-upbeleid op uw StorSimple-apparaat. Voer de volgende stappen uit in de klassieke Azure portal toevoegen of wijzigen van een planning.

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Een back-upbeleid te verwijderen
Voer de volgende stappen uit in de klassieke Azure portal een back-upbeleid op uw StorSimple-apparaat verwijderen.

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Maak een handmatige back-up
Voer de volgende stappen uit in de klassieke Azure portal voor het maken van een op aanvraag back-up (handmatige) voor één volume.

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Een aangepaste back-upbeleid maken met meerdere volumes en schema 's
De volgende stappen uitvoeren in de klassieke Azure portal voor het maken van een aangepaste back-upbeleid met meerdere volumes en schema's.

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

