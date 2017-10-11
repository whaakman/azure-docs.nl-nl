---
title: StorSimple 8000 series volume klonen | Microsoft Docs
description: Beschrijft de kloon van de verschillende typen en wanneer u ze gebruikt en wordt uitgelegd hoe u een back-upset voor het klonen van een afzonderlijke volume kunt gebruiken.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 070ac53e-7388-4c48-b8a5-8ed7f9108b2c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 2b627250df62bbe3299869ef3812947afbb8f29f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>De StorSimple Manager-service gebruiken voor het klonen van een volume (Update 2)
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Overzicht
De StorSimple Manager-service **back-upcatalogus** pagina wordt weergegeven voor de back-upsets die worden gemaakt wanneer de handmatige of automatische back-ups worden gemaakt. U kunt deze pagina gebruiken alle-ups voor een back-upbeleid of een volume, selecteer weergeven of verwijderen van back-ups, of een back-up gebruiken om te zetten of te klonen van een volume.

![Back-upcatalogus pagina](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Deze zelfstudie wordt beschreven hoe u een back-upset voor het klonen van een afzonderlijke volume kunt gebruiken. Ook wordt uitgelegd hoe het verschil tussen *tijdelijke* en *permanente* klonen.

> [!NOTE]
> Een lokaal vastgemaakt volume wordt als een gelaagd volume worden gekloond. Als u het gekloonde volume lokaal worden vastgemaakt moet, kunt u de kloon converteren naar een lokaal vastgemaakt volume nadat de kopieerbewerking is voltooid. Ga naar voor informatie over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume [wijzigen van het volumetype](storsimple-manage-volumes-u2.md#change-the-volume-type).
> 
> Als u probeert te converteren van een gekloonde volume van mislukt lagen naar lokaal vastgemaakt onmiddellijk na het klonen (wanneer deze nog steeds een tijdelijke kloon wordt), de conversie met het volgende foutbericht weergegeven:
> 
> `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
> 
> Deze fout wordt alleen als u het doorsturen naar een ander apparaat worden klonen ontvangen. U kunt het volume lokaal vastgemaakt als u eerst de tijdelijke kloon converteren naar een permanente kloon correct worden geconverteerd. Als u wilt de tijdelijke kloon omzetten in een permanente kloon, een momentopname cloud in ervan.
> 
> 

## <a name="create-a-clone-of-a-volume"></a>Maak een kloon van een volume
U kunt een kloon op hetzelfde apparaat, een ander apparaat of zelfs een virtuele machine maken met behulp van een lokale of cloudmomentopname van de.

#### <a name="to-clone-a-volume"></a>Een volume klonen
1. Klik op de pagina StorSimple Manager-service op de **back-upcatalogus** tabblad en selecteert u een back-upset.
2. Vouw in de back-upset om de gekoppelde volumes weer te geven. Klik op en selecteert u een volume van de back-upset.
   
     ![Een volume klonen](./media/storsimple-clone-volume-u2/CloneVol.png) 
3. Klik op **kloon** om te beginnen met het geselecteerde volume klonen.
4. In de wizard kloon Volume onder **naam en locatie opgeven**:
   
   1. Identificeer een doelapparaat. Dit is de locatie waar de kloon wordt gemaakt. U kunt kiezen hetzelfde apparaat of geef een ander apparaat. Als u een volume dat is gekoppeld aan andere cloudserviceproviders (geen Azure), de vervolgkeuzelijst voor het doelapparaat wordt alleen weergegeven voor fysieke apparaten. Een volume dat is gekoppeld aan andere cloudserviceproviders op een virtueel apparaat kan niet worden gekloond.
      
      > [!NOTE]
      > Zorg ervoor dat de vereiste capaciteit voor de kloon lager dan de capaciteit die beschikbaar is op het doelapparaat is.
      > 
      > 
   2. Geef een unieke volumenaam voor de kloon. De naam moet tussen 3 en 127 tekens bevatten. 
      
      > [!NOTE]
      > De **kloon Volume als** veld **tiers verdeelde** zelfs als u bij het klonen van een lokaal vastgemaakt volume. U kunt deze instelling; niet wijzigen echter, als u het gekloonde volume worden ook lokaal vastgemaakt moet, kunt u omzetten de kloon een lokaal vastgemaakt volume nadat u de kloon is gemaakt. Ga naar voor informatie over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume [wijzigen van het volumetype](storsimple-manage-volumes-u2.md#change-the-volume-type).
      > 
      > 
      
        ![Kloon wizard 1](./media/storsimple-clone-volume-u2/clone1.png) 
   3. Klik op het pijlpictogram ![pijltje](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) om door te gaan naar de volgende pagina.
5. Onder **Geef hosts die dit volume kunnen gebruiken**:
   
   1. Geef een record voor toegangscontrole (ACR) voor de kloon. U kunt een nieuwe ACR toevoegen of u kunt kiezen uit de bestaande lijst.
      
        ![Kloon wizard 2](./media/storsimple-clone-volume-u2/clone2.png) 
   2. Klik op het vinkje ![vinkje](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)de uit te voeren.
6. Een taak klonen wordt gestart en u wordt gewaarschuwd als de kloon is gemaakt. Klik op **taak weergeven** voor het bewaken van de kloon-taak op de **taken** pagina. U ziet het volgende bericht wanneer de taak van de kloon is voltooid:
   
    ![Kloon-bericht](./media/storsimple-clone-volume-u2/CloneMsg.png) 
7. Nadat de taak van de kloon is voltooid:
   
   1. Ga naar de **apparaten** pagina en selecteer de **Volumecontainers** tabblad. 
   2. Selecteer de volumecontainer die is gekoppeld aan het bronvolume die u gekloond. U ziet in de lijst van volumes, de kloon die is gemaakt.

> [!NOTE]
> Bewaking en het standaard back-up worden automatisch uitgeschakeld op een gekloonde volume.
> 
> 

Een kloon dat is gemaakt op deze manier is een tijdelijke kloon. Zie voor meer informatie over de typen kloon [tijdelijke versus permanente klonen](#transient-vs-permanent-clones).

De kloon is nu een gewone volume en bewerking die mogelijk is op een volume, zijn beschikbaar voor de kloon. U moet dit volume voor een back-ups configureren.

## <a name="transient-vs-permanent-clones"></a>Tijdelijke versus permanente klonen
Tijdelijke klonen worden alleen gemaakt wanneer u naar een ander apparaat klonen. U kunt een specifiek volume van een back-upset naar een ander apparaat beheerd door de StorSimple Manager klonen. De tijdelijke kloon zijn verwijzingen naar de gegevens in het oorspronkelijke volume en deze gegevens gebruiken om te lezen en schrijven lokaal op het doelapparaat. 

Nadat u een cloud momentopname van een kloon van tijdelijke, worden de resulterende kloon een *permanente* kloon. Tijdens dit proces wordt een kopie van de gegevens in de cloud is gemaakt en de tijd om deze gegevens te kopiëren wordt bepaald door de grootte van de gegevens en de Azure latenties (dit is een exemplaar van Azure naar Azure). Dit proces kan dagen tot weken duren. De tijdelijke kloon wordt een permanente kloon op deze manier en hoeft niet alle verwijzingen naar de oorspronkelijke volumegegevens dat deze is gekloond uit. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenario's voor tijdelijke en permanente klonen
De volgende secties beschrijven voorbeeld situaties waarin tijdelijke en permanente klonen kunnen worden gebruikt.

### <a name="item-level-recovery-with-a-transient-clone"></a>Herstel op itemniveau met een tijdelijke kloon
U moet één jaar oude Microsoft PowerPoint-presentatie-bestand te herstellen. Uw IT-beheerder identificeert de specifieke back-up van die periode wordt voltooid en vervolgens gefilterd het volume. De beheerder vervolgens kloont het volume, zoekt naar het bestand dat u zoekt, en stelt deze voor u. In dit scenario wordt wordt een tijdelijke kloon gebruikt. 

![Video beschikbaar](./media/storsimple-clone-volume-u2/Video_icon.png) **Video beschikbaar**

Als u wilt een video die u laat zien hoe u gebruikt voor het klonen en functies in StorSimple voor het herstellen van verwijderde bestanden herstellen, klikt u op [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>In de productieomgeving met een permanente kloon testen
U moet controleren of een testen fout in de productieomgeving. U maakt een kloon van het volume in de productieomgeving en vervolgens een momentopname cloud van de kloon een onafhankelijke gekloonde volume maken. In dit scenario wordt wordt een permanente kloon gebruikt.  

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [een StorSimple-volume herstelt vanuit een back-upset](storsimple-restore-from-backup-set-u2.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

