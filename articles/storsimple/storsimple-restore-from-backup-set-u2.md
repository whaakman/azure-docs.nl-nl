---
title: Een StorSimple-volume voor back-up terugzetten | Microsoft Docs
description: Legt uit hoe de StorSimple Manager-servicepagina back-upcatalogus gebruiken om te zetten van een StorSimple-volume vanuit een back-upset.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
ms.openlocfilehash: 99b76e3bc2939c65654cbf606fda6f8a45e0c44b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Een StorSimple-volume herstelt vanuit een back-upset (Update 2)
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Overzicht
De **back-upcatalogus** pagina wordt weergegeven voor de back-upsets die worden gemaakt wanneer de handmatige of automatische back-ups worden gemaakt. Gebruik deze pagina weergeven en beheren van back-ups, terugzetten vanaf een back-upset of klonen van een volume.

 ![Back-pagina van de catalogus](./media/storsimple-restore-from-backup-set-u2/restore.png)

Deze zelfstudie wordt uitgelegd hoe u de **back-upcatalogus** pagina om uw apparaat terugzetten vanaf een back-upset.

U kunt een volume van een lokale of de cloud back-up herstellen. In beide gevallen worden de herstelbewerking het volume online onmiddellijk tijdens het downloaden van gegevens op de achtergrond. 

## <a name="before-you-restore"></a>Voordat u herstellen
Voordat u een herstelbewerking start, moet u rekening houden met het volgende voorbehoud zijn:

* **Het volume offline halen** – het volume offline op de host en het apparaat nemen voordat u de herstelbewerking opnieuw starten. Hoewel de herstelbewerking om automatisch het volume online brengen op het apparaat, moet u handmatig het apparaat online brengen op de host. U kunt het volume online zetten op de host als het volume online op het apparaat. (U hoeft niet te wachten tot de herstelbewerking is voltooid.) Voor procedures gaat u naar [offline zetten van een volume](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Na het terugzetten van het volumetype** – verwijderde volumes zijn hersteld op basis van het type in de momentopname. Volumes die lokaal zijn vastgemaakt worden teruggezet als lokaal vastgemaakte volumes en volumes die zijn lagen worden teruggezet als gelaagde volumes.
  
    Voor bestaande volumes overschrijft het huidige gebruikstype informatie over het van het volume het type dat is opgeslagen in de momentopname. Bijvoorbeeld, als u een volume herstellen vanuit een momentopname die is uitgevoerd wanneer het volumetype is gelaagd en volumetype nu lokaal (als gevolg van een conversiebewerking vastgemaakt is), wordt klikt u vervolgens het volume teruggezet als een lokaal vastgemaakt volume. Op dezelfde manier als een bestaand lokaal vastgemaakt volume is uitgebreid en vervolgens wordt teruggezet vanuit een momentopname van een oudere uitgevoerd wanneer het volume kleiner is, behoudt de herstelde volume de huidige grootte van de uitgevouwen.
  
    U kunt een volume van een gelaagd volume converteren naar een lokaal vastgemaakt volume of _omgekeerd_ terwijl het volume wordt hersteld. Wacht totdat de herstelbewerking is voltooid en vervolgens u het volume naar een ander type converteren kunt. Voor informatie over het converteren van een volume, gaat u naar [wijzigen van het volumetype](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **Grootte van het volume wordt weergegeven in het herstelde volume** – dit is een belangrijk aandachtspunt als u een lokaal vastgemaakt volume dat is verwijderd (omdat lokaal vastgemaakte volumes zijn volledig is ingericht) wilt herstellen. Zorg ervoor dat u over voldoende ruimte beschikken voordat u probeert te herstellen van een lokaal vastgemaakt volume die eerder is verwijderd. 
* **U kunt een volume niet uitbreiden wanneer deze wordt hersteld** : wacht totdat de herstelbewerking is voltooid voordat u het volume uitbreiden. Voor informatie over het uitbreiden van een volume, gaat u naar [wijzigen van een volume](storsimple-manage-volumes-u2.md#modify-a-volume).
* **U kunt een back-up uitvoeren terwijl u een lokaal volume herstelt** – voor procedures gaat u naar [de StorSimple Manager-service gebruiken voor het beheren van back-upbeleid](storsimple-manage-backup-policies.md).
* **U kunt een herstelbewerking annuleren** : als u de hersteltaak en vervolgens het volume wordt teruggedraaid naar de staat waarin deze zich bevond voordat u de herstelbewerking geïnitieerd annuleren. Voor procedures gaat u naar [een taak annuleren](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Hoe werk herstellen
Voor apparaten met Update 4 of hoger, is heatmap-herstelfuncties geïmplementeerd. Als de host-aanvragen voor toegang tot gegevens tot het apparaat, deze aanvragen worden bijgehouden en een heatmap wordt gemaakt. Hoog percentage resulteert in gegevenssegmenten met hogere hitte dat lagere frequentie van aanvragen wordt omgezet in segmenten met lagere hitte. U moet toegang tot de gegevens ten minste tweemaal moet worden gemarkeerd als _hot_. Een bestand dat wordt gewijzigd ook is gemarkeerd als _hot_. Zodra u het herstel start, treedt er proactieve hydratie van gegevens op op basis van de heatmap. Voor versies is eerder dan Update 4, de gegevens gedownload tijdens het terugzetten op basis van alleen toegang. 

Op basis van Heatmap bijhouden is ingeschakeld, alleen voor volumes gelaagde en lokaal vastgemaakte volumes worden niet ondersteund. Heatmap-herstelfuncties wordt ook niet ondersteund bij het klonen van een volume op een ander apparaat. Als er een in-place herstellen en een lokale momentopname voor het volume moet worden hersteld op het apparaat bestaat, komen klikt u vervolgens we niet rehydrate (zoals gegevens al lokaal beschikbaar is). Standaard, wanneer u herstelt, zijn rehydratering taken gestart die gegevens op basis van de heatmap proactief rehydrate. In Update 4, Windows PowerShell-cmdlets kunnen worden gebruikt om op te vragen rehydratering taken die worden uitgevoerd, een rehydratering taak annuleren en de status van de taak rehydratering ophalen.

* `Get-HcsRehydrationJob`-Deze cmdlet haalt de status van de taak rehydratering. Een taak één rehydratering wordt geactiveerd voor één volume.
* `Set-HcsRehydrationJob`-Deze cmdlet kunt u onderbreken, stoppen, hervat de taak rehydratering wanneer de rehydratering uitgevoerd wordt. 

Ga voor meer informatie over cmdlets rehydratering naar [naslaginformatie over Windows PowerShell-cmdlets voor StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

Met automatische rehdyration wordt doorgaans hogere prestaties voor de tijdelijke lezen verwacht. De werkelijke magniutde van verbeteringen is afhankelijk van verschillende factoren zoals toegangstijden en gegevensverloop gegevenstype. Als u wilt een rehydratering taak annuleren, kunt u de PowerShell-cmdlet. Als u rehydratering-taken voor alle toekomstige herstelbewerkingen permanent uitschakelen wilt, moet u contact op met Microsoft Support.

## <a name="how-to-use-the-backup-catalog"></a>Het gebruik van de back-upcatalogus
De **back-upcatalogus** pagina vindt u een query die u helpt bij het verfijnen van uw back-up selectie instellen. U kunt de back-upsets die worden opgehaald op basis van de volgende parameters op te filteren:

* **Apparaat** : het apparaat waarop de back-upset is gemaakt.
* **Back-up maken van beleid** of **volume** – de back-upbeleid of het volume dat is gekoppeld aan deze back-upset.
* **Van** en **naar** – het bereik datum en tijd wanneer de back-upset is gemaakt.

De gefilterde back-upsets worden vervolgens in een tabel weergegeven op basis van de volgende kenmerken:

* **Naam** : de naam van de back-upbeleid of het volume dat is gekoppeld aan de back-upset.
* **De grootte van** : de werkelijke grootte van de back-upset.
* **Gemaakt op** – de datum en tijd waarop de back-ups zijn gemaakt. 
* **Type** – sets van back-up kunnen worden lokale momentopnamen of cloud worden opgeslagen. Een lokale momentopname is een back-up van alle uw worden lokaal opgeslagen volumegegevens op het apparaat. Een cloudmomentopname verwijst naar de back-up van de volumegegevens die zich in de cloud. Lokale momentopnamen bieden sneller toegang terwijl cloudmomentopnamen voor gegevenstolerantie worden gekozen.
* **Gestart door** – de back-ups automatisch kunnen worden gestart volgens een schema of handmatig door u. (U kunt een back-upbeleid back-ups plannen. U kunt ook de **back-up maken** optie voor een interactieve back-up te maken.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Het terugzetten van uw StorSimple-volume van een back-up
U kunt de **back-upcatalogus** pagina uw StorSimple-volume van een specifieke back-up herstellen. Houd er echter rekening mee, dat het herstellen van een volume wordt teruggezet het volume op de status was toen de back-up is gemaakt. Alle gegevens die zijn toegevoegd na de back-upbewerking wordt verbroken.

> [!WARNING]
> Terugzetten vanuit een back-up vervangt de bestaande volumes van de back-up. Dit kan ertoe leiden dat het verlies van gegevens die is geschreven nadat de back-up is gemaakt.
> 
> 

### <a name="to-restore-your-volume"></a>Het volume te herstellen
1. Klik op de pagina StorSimple Manager-service op de **back-upcatalogus** tabblad.
   
    ![Back-upcatalogus](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Selecteer een back-up als volgt instellen:
   
   1. Selecteer het juiste apparaat.
   2. Kies in de vervolgkeuzelijst het volume of back-up-beleid voor de back-up die u wilt selecteren.
   3. Geef het tijdsbereik.
   4. Klik op het vinkje ![vinkje](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) Deze query uitvoeren.
      
      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.
3. Vouw in de back-upset om de gekoppelde volumes weer te geven. Deze volumes moeten op de host en het apparaat offline worden gezet voordat u ze kunt herstellen. Toegang krijgen tot de volumes op de **Volumecontainers** pagina en volg de stappen in [offline zetten van een volume](storsimple-manage-volumes-u2.md#take-a-volume-offline) offline te nemen ze.
   
   > [!IMPORTANT]
   > Zorg ervoor dat u hebt ondernomen volumes offline op de host eerst voordat u de volumes offline op het apparaat nemen. Als u niet de volumes offline op de host nemen, kan dit leiden tot beschadiging van gegevens.
   > 
   > 
4. Ga terug naar de **back-upcatalogus** tabblad en selecteert u een back-upset.
5. Klik op **herstellen** aan de onderkant van de pagina.
6. U wordt gevraagd om bevestiging. Controleer de gegevens herstellen en selecteer vervolgens het selectievakje bevestiging.
   
    ![Bevestigingspagina](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Klik op het vinkje ![vinkje](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Een hersteltaak begint. U kunt de taak weergeven door het openen van de **taken** pagina. 
8. Nadat het herstel voltooid is, kunt u controleren dat de inhoud van uw volumes worden vervangen door de volumes van de back-up.

![Video beschikbaar](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video beschikbaar**

Als u wilt een video die u laat zien hoe u gebruikt voor het klonen en functies in StorSimple voor het herstellen van verwijderde bestanden herstellen, klikt u op [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Als het herstel mislukt
U ontvangt een waarschuwing als de herstelbewerking voor een of andere reden mislukt. Als dit het geval is, moet u de back-lijst om te bevestigen dat de back-up nog geldig vernieuwen. Als de back-up geldig is en u vanuit de cloud herstellen wilt, vervolgens verbindingsproblemen mogelijk oorzaak van het probleem. 

Voor het voltooien van de herstelbewerking, het volume offline nemen op de host en probeer de herstelbewerking opnieuw. Wijzigingen in de volumegegevens die zijn uitgevoerd tijdens het herstelproces gaan verloren.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [beheren StorSimple-volumes](storsimple-manage-volumes-u2.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

