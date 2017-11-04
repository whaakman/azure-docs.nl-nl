---
title: Klonen van uw StorSimple-volume | Microsoft Docs
description: Beschrijft de kloon van de verschillende typen en wanneer u ze gebruikt en wordt uitgelegd hoe u een back-upset voor het klonen van een afzonderlijke volume kunt gebruiken.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: cf11a369549b887f79a81c19780048d31e56beae
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>De StorSimple Manager-service gebruiken voor het klonen van een volume
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Overzicht
De StorSimple Manager-service **back-upcatalogus** pagina wordt weergegeven voor de back-upsets die worden gemaakt wanneer de handmatige of automatische back-ups worden gemaakt. U kunt deze pagina gebruiken alle-ups voor een back-upbeleid of een volume, selecteer weergeven of verwijderen van back-ups, of een back-up gebruiken om te zetten of te klonen van een volume.

![Back-upcatalogus pagina](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Deze zelfstudie wordt beschreven hoe u een back-upset voor het klonen van een afzonderlijke volume kunt gebruiken. Ook wordt uitgelegd hoe het verschil tussen *tijdelijke* en *permanente* klonen. 

## <a name="create-a-clone-of-a-volume"></a>Maak een kloon van een volume
U kunt een kloon op hetzelfde apparaat, een ander apparaat of zelfs een virtuele machine maken met behulp van een lokale of een cloudmomentopname van de.

#### <a name="to-clone-a-volume"></a>Een volume klonen
1. Klik op de pagina StorSimple Manager-service op de **back-upcatalogus** tabblad en selecteert u een back-upset.
2. Vouw in de back-upset om de gekoppelde volumes weer te geven. Klik op en selecteert u een volume van de back-upset.
   
     ![Een volume klonen](./media/storsimple-clone-volume/HCS_Clone.png) 
3. Klik op **kloon** om te beginnen met het geselecteerde volume klonen.
4. In de wizard kloon Volume onder **naam en locatie opgeven**:
   
   1. Identificeer een doelapparaat. Dit is de locatie waar de kloon wordt gemaakt. U kunt kiezen hetzelfde apparaat of geef een ander apparaat. Als u een volume dat is gekoppeld aan andere cloudserviceproviders (geen Azure), de vervolgkeuzelijst voor het doelapparaat wordt alleen weergegeven voor fysieke apparaten. Een volume dat is gekoppeld aan andere cloudserviceproviders op een virtueel apparaat kan niet worden gekloond.
      
      > [!NOTE]
      > Zorg ervoor dat de vereiste capaciteit voor de kloon lager dan de capaciteit die beschikbaar is op het doelapparaat is.
      > 
      > 
   2. Geef een unieke volumenaam voor de kloon. De naam moet tussen 3 en 127 tekens bevatten.
   3. Klik op het pijlpictogram ![pijltje](./media/storsimple-clone-volume/HCS_ArrowIcon.png) om door te gaan naar de volgende pagina.
5. Onder **Geef hosts die dit volume kunnen gebruiken**:
   
   1. Geef een record voor toegangscontrole (ACR) voor de kloon. U kunt een nieuwe ACR toevoegen of u kunt kiezen uit de bestaande lijst.
   2. Klik op het vinkje ![vinkje](./media/storsimple-clone-volume/HCS_CheckIcon.png)de uit te voeren.
6. Een taak klonen wordt gestart en u wordt gewaarschuwd als de kloon is gemaakt. Klik op **taak weergeven** voor het bewaken van de kloon-taak op de **taken** pagina.
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
Tijdelijke en permanente klonen worden alleen gemaakt wanneer u het doorsturen naar een ander apparaat worden klonen. U kunt een specifiek volume van een back-upset naar een ander apparaat klonen. Een kloon gemaakt op deze manier is een *tijdelijke* kloon. De tijdelijke kloon verwijzingen naar het oorspronkelijke volume zijn en dat volume gebruikt om te lezen tijdens het schrijven van lokaal. 

Nadat u een cloud momentopname van een kloon van tijdelijke, worden de resulterende kloon een *permanente* kloon. De permanente kloon is onafhankelijk en hoeft niet alle verwijzingen naar het oorspronkelijke volume dat deze is gekloond uit.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenario's voor tijdelijke en permanente klonen
De volgende secties beschrijven voorbeeld situaties waarin tijdelijke en permanente klonen kunnen worden gebruikt.

### <a name="item-level-recovery-with-a-transient-clone"></a>Herstel op itemniveau met een tijdelijke kloon
U moet één jaar oude Microsoft PowerPoint-presentatie-bestand te herstellen. Uw IT-beheerder identificeert de specifieke back-up van die periode wordt voltooid en vervolgens gefilterd het volume. De beheerder vervolgens kloont het volume, zoekt naar het bestand dat u zoekt, en stelt deze voor u. In dit scenario wordt wordt een tijdelijke kloon gebruikt. 

![Video beschikbaar](./media/storsimple-clone-volume/Video_icon.png) **Video beschikbaar**

Als u wilt een video die u laat zien hoe u gebruikt voor het klonen en functies in StorSimple voor het herstellen van verwijderde bestanden herstellen, klikt u op [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>In de productieomgeving met een permanente kloon testen
U moet controleren of een testen fout in de productieomgeving. Maakt u een kloon van het volume in de productieomgeving door het maken van een cloudmomentopname van de kloon. Het gekloonde volume is nu onafhankelijk. In dit scenario wordt wordt een permanente kloon gebruikt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [een StorSimple-volume herstelt vanuit een back-upset](storsimple-restore-from-backup-set.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

