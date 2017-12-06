---
title: Klonen van een volume op StorSimple 8000-serie | Microsoft Docs
description: Beschrijft de kloon van de verschillende typen en het gebruik en wordt uitgelegd hoe u een back-upset voor het klonen van een afzonderlijke volume op een StorSimple 8000 series apparaat kunt gebruiken.
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
ms.date: 12/05/2017
ms.author: alkohli
ms.openlocfilehash: d3965c74d51996b3e4144130fad01fae9e265897
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-clone-a-volume"></a>De service Manager van StorSimple-apparaat in Azure-portal gebruiken voor het klonen van een volume

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt beschreven hoe u kunt een back-upset voor het klonen van een afzonderlijke volume via de **back-upcatalogus** blade. Ook wordt uitgelegd hoe het verschil tussen *tijdelijke* en *permanente* klonen. De instructies in deze zelfstudie geldt voor alle de StorSimple 8000 series apparaat met Update 3 of hoger.

De service Manager voor StorSimple-apparaat **back-upcatalogus** blade wordt weergegeven voor de back-upsets die worden gemaakt wanneer de handmatige of automatische back-ups worden gemaakt. Vervolgens kunt u een volume selecteren in een back-upset voor het klonen.

 ![Back-upset lijst](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Overwegingen voor het klonen van een volume

Houd rekening met de volgende informatie bij het klonen van een volume.

- Een kloon gedraagt zich op dezelfde manier als een reguliere-volume. Een bewerking die mogelijk is op een volume is beschikbaar voor de kloon.

- Bewaking en het standaard back-up worden automatisch uitgeschakeld op een gekloonde volume. U moet een gekloonde volume voor een back-ups configureren.

- Een lokaal vastgemaakt volume is als een gelaagd volume gekloond. Als u het gekloonde volume lokaal worden vastgemaakt moet, kunt u de kloon converteren naar een lokaal vastgemaakt volume nadat de kopieerbewerking is voltooid. Ga naar voor informatie over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume [wijzigen van het volumetype](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).

- Als u probeert te converteren van een gekloonde volume van lagen naar lokaal vastgemaakt onmiddellijk na het klonen (wanneer deze nog steeds een tijdelijke kloon wordt), de conversie is mislukt met het volgende foutbericht weergegeven:

    `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.`

    Deze fout wordt alleen als u het doorsturen naar een ander apparaat worden klonen ontvangen. U kunt het volume lokaal vastgemaakt als u eerst de tijdelijke kloon converteren naar een permanente kloon correct worden geconverteerd. Een cloud momentopname van de tijdelijke kloon te converteren naar een permanente kloon.

## <a name="create-a-clone-of-a-volume"></a>Maak een kloon van een volume

U kunt een kloon maken op hetzelfde apparaat, een ander apparaat of zelfs een cloud-apparaat met behulp van een lokale of cloudmomentopname van de.

De onderstaande procedure beschrijft hoe een kloon van de back-upcatalogus maakt.  Een alternatieve methode voor het starten van de kloon wordt naar **Volumes**, selecteert u een volume en klik vervolgens met de rechtermuisknop om het contextmenu aanroepen en selecteer **kloon**.

De volgende stappen voor het maken van een kloon van het volume van de back-upcatalogus uitvoeren.

#### <a name="to-clone-a-volume"></a>Een volume klonen

1. Ga naar uw StorSimple-apparaat Manager-service en klik vervolgens op **back-upcatalogus**.

2. Selecteer een back-up als volgt instellen:
   
   1. Selecteer het juiste apparaat.
   2. Kies in de vervolgkeuzelijst het volume of back-up-beleid voor de back-up die u wilt selecteren.
   3. Geef het tijdsbereik.
   4. Klik op **toepassen** deze query uit te voeren.

    De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.
   
    ![Back-upset lijst](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. Vouw in de back-upset weergeven van het gekoppelde volume en selecteert u een volume in een back-upset. Met de rechtermuisknop en selecteer vervolgens in het contextmenu **kloon**.

   ![Back-upset lijst](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. In de **kloon** blade, moet u de volgende stappen uitvoeren:
   
    1. Identificeer een doelapparaat. Dit is de locatie waar de kloon wordt gemaakt. U kunt kiezen hetzelfde apparaat of geef een ander apparaat.

      > [!NOTE]
      > Zorg ervoor dat de vereiste capaciteit voor de kloon lager dan de capaciteit die beschikbaar is op het doelapparaat is.
       
    2. Geef een unieke volumenaam voor de kloon. De naam moet tussen 3 en 127 tekens bevatten.
      
        > [!NOTE]
        > De **kloon Volume als** veld **tiers verdeelde** zelfs als u bij het klonen van een lokaal vastgemaakt volume. U kunt deze instelling; niet wijzigen echter, als u het gekloonde volume worden ook lokaal vastgemaakt moet, kunt u omzetten de kloon een lokaal vastgemaakt volume nadat u de kloon is gemaakt. Ga naar voor informatie over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume [wijzigen van het volumetype](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).
          
    3. Onder **verbonden hosts**, Geef een record voor toegangscontrole (ACR) voor de kloon. U kunt een nieuwe ACR toevoegen of u kunt kiezen uit de bestaande lijst. De ACR wordt bepaald welke hosts hebben toegang tot de kloon.
      
        ![Back-upset lijst](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. Klik op **kloon** om de bewerking te voltooien.

4. Een taak klonen wordt gestart en u wordt gewaarschuwd wanneer het klonen is gemaakt. Klik op de melding van de taak of Ga naar **taken** blade voor het bewaken van de kloon-taak.

    ![Back-upset lijst](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Nadat het klonen voltooid is, gaat u naar het apparaat en klik vervolgens op **Volumes**. U ziet in de lijst van volumes, de kloon die is gemaakt in dezelfde volumecontainer met het bronvolume.

    ![Back-upset lijst](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Een kloon dat is gemaakt op deze manier is een tijdelijke kloon. Zie voor meer informatie over de typen kloon [tijdelijke versus permanente klonen](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Tijdelijke versus permanente klonen
Tijdelijke klonen worden alleen gemaakt wanneer u op een ander apparaat klonen. U kunt een specifiek volume van een back-upset naar een ander apparaat beheerd door de StorSimple-Apparaatbeheer klonen. De tijdelijke kloon verwijzingen naar de gegevens in het oorspronkelijke volume gebruikt, en die gegevens te lezen en schrijven lokaal op het doelapparaat.

Nadat u een cloud momentopname van een kloon van tijdelijke, de resulterende kloon is een *permanente* kloon. Tijdens dit proces wordt een kopie van de gegevens in de cloud is gemaakt en de tijd om deze gegevens te kopiëren wordt bepaald door de grootte van de gegevens en de Azure latenties (dit is een exemplaar van Azure naar Azure). Dit proces kan dagen tot weken duren. De tijdelijke kloon wordt een permanente kloon en hoeft niet alle verwijzingen naar de oorspronkelijke volumegegevens dat deze is gekloond uit.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenario's voor tijdelijke en permanente klonen
De volgende secties beschrijven voorbeeld situaties waarin tijdelijke en permanente klonen kunnen worden gebruikt.

### <a name="item-level-recovery-with-a-transient-clone"></a>Herstel op itemniveau met een tijdelijke kloon
U moet één jaar oude Microsoft PowerPoint-presentatie-bestand te herstellen. Uw IT-beheerder de specifieke back-up vanaf dat moment identificeert en vervolgens gefilterd het volume. De beheerder vervolgens kloont het volume, zoekt naar het bestand dat u zoekt, en stelt deze voor u. In dit scenario wordt wordt een tijdelijke kloon gebruikt.

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>In de productieomgeving met een permanente kloon testen
U moet controleren of een testen fout in de productieomgeving. U maakt een kloon van het volume in de productieomgeving en vervolgens een momentopname cloud van de kloon een onafhankelijke gekloonde volume maken. In dit scenario wordt wordt een permanente kloon gebruikt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [een StorSimple-volume herstelt vanuit een back-upset](storsimple-8000-restore-from-backup-set-u2.md).
* Meer informatie over hoe [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

