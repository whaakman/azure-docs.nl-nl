---
title: Klonen van een volume op StorSimple 8000-serie | Microsoft Docs
description: Beschrijving van de kloon van verschillende typen en het gebruik, en wordt uitgelegd hoe u een back-upset voor het klonen van een afzonderlijke volume op een StorSimple 8000-apparaat kunt gebruiken.
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
ms.date: 12/05/2017
ms.author: alkohli
ms.openlocfilehash: 84734aefb72a3330d99c5707b461de2cd5e30484
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58097232"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-clone-a-volume"></a>De StorSimple Device Manager-service in Azure portal gebruiken voor een volume klonen

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt beschreven hoe u kunt een back-upset voor het klonen van een afzonderlijke volume via de **back-upcatalogus** blade. Ook wordt uitgelegd hoe het verschil tussen *tijdelijke* en *permanente* klonen. De instructies in deze zelfstudie is van toepassing op alle de StorSimple 8000-apparaat met Update 3 of hoger.

De service StorSimple Device Manager **back-upcatalogus** blade wordt weergegeven voor de back-upsets die worden gemaakt wanneer er een handmatige of geautomatiseerde back-ups worden gemaakt. Vervolgens kunt u een volume selecteren in een back-upset om te klonen.

 ![Lijst met back-upset](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Overwegingen voor een volume klonen

Houd rekening met de volgende informatie op bij het klonen van een volume.

- Een kloon gedraagt zich op dezelfde manier als een reguliere-volume. Elke bewerking die is toegestaan op een volume is beschikbaar voor de kloon.

- Controle en het standaard back-up automatisch voor een gekloond volume worden uitgeschakeld. U moet een gekloond volume voor een back-ups configureren.

- Een lokaal vastgemaakt volume is gekloond als een gelaagd volume. Als u het gekloonde worden lokaal vastgemaakt volume nodig hebt, kunt u de kloon converteren naar een lokaal vastgemaakt volume nadat de kloonbewerking is voltooid. Voor informatie over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume, gaat u naar [wijzigen van het volumetype](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).

- Als u probeert te converteren van een gekloond volume van gelaagde in lokaal vastgemaakt onmiddellijk na het kopiëren (als deze nog steeds een tijdelijke kloon is), de conversie is mislukt met de volgende strekking weergegeven:

    `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.`

    Deze fout wordt alleen ontvangen als u bij een ander apparaat worden klonen. U kunt het volume lokaal vastgemaakt als u de tijdelijke kloon eerst converteren naar een permanente kloon is converteren. Maakt u een cloudmomentopname van de tijdelijke kloon te converteren naar een permanente kloon.

## <a name="create-a-clone-of-a-volume"></a>Maak een kloon van een volume

U kunt een kloon op hetzelfde apparaat, een ander apparaat of zelfs een cloudapparaat maken met behulp van een lokale of cloudmomentopname van de.

De volgende procedure wordt beschreven hoe u een kloon maken van de back-catalogus.  Een alternatieve methode voor het starten van de kloon wordt naar **Volumes**, selecteert u een volume en klik vervolgens met de rechtermuisknop op voor het aanroepen van het contextmenu en selecteer **kloon**.

Voer de volgende stappen uit voor het maken van een kloon van het volume van de back-catalogus.

#### <a name="to-clone-a-volume"></a>Op een volume klonen

1. Ga naar uw StorSimple Device Manager-service en klik vervolgens op **back-upcatalogus**.

2. Selecteer een back-upset als volgt:
   
   1. Selecteer het juiste apparaat.
   2. Kies het beleid dat volume of de back-up voor de back-up die u wilt selecteren in de vervolgkeuzelijst.
   3. De tijdsperiode opgeven.
   4. Klik op **toepassen** deze query uit te voeren.

      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.
   
      ![Lijst met back-upset](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. Vouw de back-up die is ingesteld op het gekoppelde volume weergeven en selecteren van een volume in een back-upset. Met de rechtermuisknop op en selecteer vervolgens in het contextmenu **kloon**.

   ![Lijst met back-upset](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. In de **kloon** blade de volgende stappen uit:
   
   1. Identificeer een doelapparaat. Dit is de locatie waar de kloon wordt gemaakt. U kunt hetzelfde apparaat kiezen of een ander apparaat opgeven.

      > [!NOTE]
      > Zorg ervoor dat de vereiste capaciteit voor de kloon lager dan de capaciteit die beschikbaar is op het doelapparaat is.
       
   2. Geef de naam van een unieke volume voor de kloon. De naam moet tussen 3 en maximaal 127 tekens bevatten.
      
       > [!NOTE]
       > De **Volume klonen als** pole bude **gelaagd** , zelfs als u bij het klonen van een lokaal vastgemaakt volume. U kunt deze instelling; niet wijzigen echter, als u de gekloonde worden ook lokaal vastgemaakt volume, u kunt converteren de kloon naar een lokaal vastgemaakt volume nadat u de kloon is gemaakt. Voor informatie over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume, gaat u naar [wijzigen van het volumetype](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).
          
   3. Onder **verbonden hosts**, Geef een Acces controlerecord (ACR) voor de kloon. U kunt een nieuwe ACR toevoegen of kies uit de bestaande lijst. De ACR, bepaalt welke hosts toegang krijgen tot de kloon.
      
       ![Lijst met back-upset](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

   4. Klik op **kloon** om de bewerking te voltooien.

4. Een taak klonen wordt gestart en u krijgt een melding wanneer het klonen is gemaakt. Klik op de melding van de taak of Ga naar **taken** blade voor het bewaken van de taak klonen.

    ![Lijst met back-upset](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Wanneer het klonen voltooid is, gaat u naar uw apparaat en klik vervolgens op **Volumes**. In de lijst van volumes ziet u de kloon die zojuist is gemaakt in dezelfde volumecontainer waarvoor het bronvolume.

    ![Lijst met back-upset](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Een kloon die is gemaakt op deze manier is een tijdelijke kloon. Zie voor meer informatie over de typen van de kloon [tijdelijke en permanente klonen](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Tijdelijke en permanente klonen
Tijdelijke klonen worden alleen gemaakt wanneer u naar een ander apparaat kloont. U kunt een bepaald volume van een back-up die is ingesteld op een ander apparaat beheerd door de StorSimple Device Manager klonen. De tijdelijke kloon verwijzingen naar de gegevens in het oorspronkelijke volume heeft en maakt gebruik van die gegevens te lezen en schrijven lokaal op het doelapparaat.

Nadat u een cloudmomentopname van een tijdelijke kloon, de resulterende kloon is een *permanente* klonen. Tijdens dit proces wordt een kopie van de gegevens wordt gemaakt in de cloud en de tijd om deze gegevens te kopiëren wordt bepaald door de grootte van de gegevens en de Azure latenties (dit is een exemplaar van Azure naar Azure). Dit proces kan dagen tot weken duren. De tijdelijke kloon wordt een permanente kloon en alle verwijzingen naar de oorspronkelijke gegevens op het volume dat deze is gekloond vanuit beschikt niet over.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenario's voor tijdelijke en permanente klonen
De volgende secties beschrijven voorbeeld situaties waarin tijdelijke en permanente klonen kunnen worden gebruikt.

### <a name="item-level-recovery-with-a-transient-clone"></a>Herstel op itemniveau met een tijdelijke kloon
U moet een bestand met één jaar oude Microsoft PowerPoint-presentatie te herstellen. Uw IT-beheerder de specifieke back-up vanaf dat moment identificeert en filtert vervolgens het volume. De beheerder vervolgens kloont het volume, wordt gezocht naar het bestand dat u zoekt, en biedt dit aan u. In dit scenario wordt wordt een tijdelijke kloon gebruikt.

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testen in de productie-omgeving met een permanente kloon
U moet controleren of een bug testen in de productieomgeving. U maakt een kloon van het volume in de productie-omgeving en vervolgens maakt u een cloudmomentopname van de kloon. het maken van een onafhankelijke gekloonde volume. In dit scenario wordt wordt een permanente kloon gebruikt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [een StorSimple-volume herstellen vanuit een back-upset](storsimple-8000-restore-from-backup-set-u2.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

