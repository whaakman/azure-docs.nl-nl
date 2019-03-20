---
title: Een volume herstellen vanuit back-up op een StorSimple 8000-serie | Microsoft Docs
description: Wordt uitgelegd hoe u de StorSimple Device Manager-service back-catalogus gebruiken om een StorSimple-volume herstellen vanuit een back-upset.
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 6a2e022697ced90d968075b7a4abe4163be7a539
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58082803"
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Een StorSimple-volume herstellen vanuit een back-upset

## <a name="overview"></a>Overzicht

Deze zelfstudie beschrijft de herstelbewerking opnieuw uitgevoerd op een StorSimple 8000-apparaat met behulp van een bestaande back-upset. Gebruik de **back-upcatalogus** blade voor het terugzetten een volume van een lokale of cloudback-up. De **back-upcatalogus** blade wordt weergegeven voor de back-upsets die worden gemaakt wanneer er een handmatige of geautomatiseerde back-ups worden gemaakt. De herstelbewerking vanuit een back-upset wordt het volume online onmiddellijk terwijl gegevens worden gedownload op de achtergrond.

Een alternatieve methode om te beginnen herstellen is naar **apparaten > [uw apparaat] > Volumes**. In de **Volumes** blade, selecteert u een volume, met de rechtermuisknop om het aanroepen van het contextmenu en selecteer vervolgens **herstellen**.

## <a name="before-you-restore"></a>Voordat u herstellen

Voordat u een herstelbewerking start, controleert u het volgende voorbehoud:

* **U moet het volume offline halen** : plaats het volume offline op de host en het apparaat voordat u de herstelbewerking opnieuw starten. Hoewel de herstelbewerking wordt automatisch het volume online op het apparaat brengt, moet u handmatig het apparaat online brengen op de host. Als het volume op het apparaat online is, kunt u het volume online brengen op de host. (U hoeft niet te wachten tot de herstelbewerking is voltooid.) Voor procedures gaat u naar [een volume offline halen](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline).

* **Volumetype na herstel** – verwijderde volumes worden teruggezet op basis van het type in de momentopname; dat wil zeggen, volumes die lokaal zijn vastgemaakt als lokaal vastgemaakte volumes worden teruggezet en volumes die zijn lagen worden hersteld als gelaagde volumes.

    Voor bestaande volumes, de huidige gebruikstype van het volume heeft voorrang boven het type dat is opgeslagen in de momentopname. Bijvoorbeeld, als u een volume herstellen vanuit een momentopname die is uitgevoerd wanneer het volumetype is gelaagd en volumetype is nu lokaal vastgemaakt (als gevolg van een conversiebewerking die werd uitgevoerd), wordt klikt u vervolgens het volume hersteld als een lokaal vastgemaakt volume. Op dezelfde manier als een bestaand lokaal vastgemaakt volume is uitgebreid en vervolgens worden hersteld vanuit een momentopname van een oudere uitgevoerd wanneer het volume kleiner is, behoudt het herstelde volume de huidige uitgebreide grootte.

    U kunt een volume niet converteren van een gelaagd volume naar een lokaal vastgemaakt volume of van een lokaal vastgemaakt volume naar een gelaagd volume, terwijl het volume wordt hersteld. Wacht totdat de herstelbewerking is voltooid en klikt u vervolgens u het volume naar een ander type converteren kunt. Voor informatie over het converteren van een volume, gaat u naar [wijzigen van het volumetype](storsimple-8000-manage-volumes-u2.md#change-the-volume-type). 

* **Grootte van het volume wordt weergegeven in het herstelde volume** : dit is een belangrijk aandachtspunt als u een lokaal vastgemaakt volume verwijderd (omdat de lokaal vastgemaakte volumes worden volledig is ingericht) wilt herstellen. Zorg ervoor dat u voldoende ruimte hebt voordat u probeert te herstellen van een lokaal vastgemaakt volume die eerder is verwijderd.

* **U kunt een volume niet uitbreiden wanneer deze wordt hersteld** : wacht totdat de herstelbewerking is voltooid voordat u probeert om uit te breiden het volume. Voor informatie over het uitbreiden van een volume, gaat u naar [wijzigen van een volume](storsimple-8000-manage-volumes-u2.md#modify-a-volume).

* **U kunt een back-up uitvoeren terwijl u een lokaal volume herstellen** – voor procedures gaat u naar [de StorSimple Device Manager-service gebruiken voor back-upbeleid beheren](storsimple-8000-manage-backup-policies-u2.md).

* **U kunt een restore-bewerking annuleren** : als u de hersteltaak annuleert en vervolgens het volume wordt teruggedraaid naar de staat waarin deze zich bevond voordat u de herstelbewerking opnieuw gestart. Voor procedures gaat u naar [een taak annuleren](storsimple-8000-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Hoe werk wordt hersteld

Voor apparaten met Update 4 of hoger kunt wordt herstellen op basis van een heatmap geïmplementeerd. Als de host aanvragen voor toegang tot gegevens tot het apparaat, deze aanvragen worden bijgehouden en een heatmap wordt gemaakt. Hoge aanvraagsnelheid resulteert in gegevenssegmenten met hogere heatmap terwijl de snelheid van aanvragen voor lagere wordt omgezet in segmenten met lagere heatmap. U moet toegang tot de gegevens ten minste twee keer om te worden gemarkeerd als _hot_. Een bestand dat wordt gewijzigd, is ook gemarkeerd als _hot_. Nadat u de herstelbewerking start, treedt er proactieve hydration van gegevens op op basis van de heatmap. Voor versies wordt ouder dan Update 4, de gegevens gedownload tijdens het terugzetten op basis van alleen toegang.

Het volgende voorbehoud van toepassing op op basis van een heatmap herstelt:

* Heatmap bijhouden is ingeschakeld voor gelaagde volumes en lokaal vastgemaakte volumes worden niet ondersteund.

* Herstel op basis van een Heatmap wordt niet ondersteund bij het klonen van een volume naar een ander apparaat. 

* Als er een in-place herstellen en een lokale momentopname voor het volume kunnen worden hersteld op het apparaat bestaat, doen klikt u vervolgens we niet rehydrate (zoals de gegevens zijn al lokaal beschikbaar). 

* Standaard, wanneer u herstelt, worden de rehydratatie-taken gestart die proactief gegevens op basis van de heatmap rehydrate. 

In de Update 4, kan Windows PowerShell-cmdlets worden gebruikt om query rehydratatie-taken die worden uitgevoerd, een rehydratatie taak annuleren en de status van de taak rehydratatie ophalen.

* `Get-HcsRehydrationJob` -Met deze cmdlet wordt de status van de taak rehydratatie opgehaald. Een taak één rehydratatie is geactiveerd voor één volume.

* `Set-HcsRehydrationJob` -Met deze cmdlet kunt u onderbreken, stoppen, hervat de taak rehydratatie wanneer de reactivering uitgevoerd is.

Voor meer informatie over rehydratatie cmdlets, gaat u naar [naslaginformatie over Windows PowerShell-cmdlets voor StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

Met automatische rehydratatie doorgaans hoger tijdelijke leesprestaties verwacht. De werkelijke grootte van verbeteringen, is afhankelijk van diverse factoren, zoals toegangspatroon, gegevensverloop, en het gegevenstype. 

Als u wilt een taak rehydratatie annuleren, kunt u de PowerShell-cmdlet. Als u wilt permanent uitgeschakeld rehydratatie taken voor alle toekomstige herstelbewerkingen [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md).

## <a name="how-to-use-the-backup-catalog"></a>Het gebruik van de back-catalogus

De **back-catalogus** blade biedt een query die u helpt bij het verfijnen van uw back-up selectie instellen. U kunt filteren, de back-upsets die worden opgehaald op basis van de volgende parameters:

* **Tijdsbereik** : het bereik datum en tijd waarop de back-upset is gemaakt.
* **Apparaat** : het apparaat waarop de back-upset is gemaakt.
* **Back-upbeleid** of **Volume** : de back-upbeleid of het volume dat is gekoppeld aan deze back-upset.

De gefilterde back-upsets worden vervolgens de tabelindeling op basis van de volgende kenmerken:

* **Naam** – de naam van de back-upbeleid of het volume dat is gekoppeld aan de back-upset.
* **Type** – sets van back-up kunnen worden lokale momentopnamen of momentopnamen in de cloud. Een lokale momentopname is een back-up van alle uw volumegegevens lokaal opgeslagen op het apparaat, terwijl een cloud-momentopname verwijst naar de back-up van volumegegevens die zich bevinden in de cloud. Lokale momentopnamen bieden snellere toegang tot dat cloudmomentopnamen worden gekozen voor gegevenstolerantie.
* **Grootte** : de werkelijke grootte van de back-upset.
* **Gemaakt op** – de datum en tijd waarop de back-ups zijn gemaakt. 
* **Volumes** -het aantal volumes die zijn gekoppeld aan de back-upset.
* **Gestart** : de back-ups automatisch kunnen worden gestart volgens een schema of handmatig door de gebruiker. (U kunt een back-upbeleid kunt gebruiken voor het plannen van back-ups. U kunt ook kunt u de **back-up maken** optie een interactieve of on-demand back-up.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Uw StorSimple-volume herstellen vanuit een back-up

U kunt de **back-catalogus** blade om uw StorSimple-volume herstellen vanaf een specifieke back-up. Houd er rekening mee, echter, het volume op de status was toen de back-up werd gemaakt dat het herstellen van een volume wordt hersteld. Alle gegevens die zijn toegevoegd na de back-upbewerking, gaan verloren.

> [!WARNING]
> Terugzetten vanuit een back-up wordt vervangen door de bestaande volumes van de back-up. Hierdoor kan het verlies van gegevens die is gemaakt nadat de back-up is gemaakt.


### <a name="to-restore-your-volume"></a>Het volume herstellen
1. Ga naar uw StorSimple Device Manager-service en klik vervolgens op **back-upcatalogus**.

2. Selecteer een back-upset als volgt:
   
   1. De tijdsperiode opgeven.
   2. Selecteer het juiste apparaat.
   3. Kies het beleid dat volume of de back-up voor de back-up die u wilt selecteren in de vervolgkeuzelijst.
   4. Klik op **toepassen** deze query uit te voeren.

      De back-ups die zijn gekoppeld aan het geselecteerde volume of back-upbeleid moet worden weergegeven in de lijst met back-upsets.
   
      ![Lijst met back-upset](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. Vouw de back-upset om de gekoppelde volumes weer te geven. Deze volumes moeten op de host en het apparaat offline worden gezet voordat u ze kunt herstellen. Toegang krijgen tot de volumes op de **Volumes** blade van uw apparaat, en volg de stappen in [een volume offline halen](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) offline te nemen ze.
   
   > [!IMPORTANT]
   > Zorg ervoor dat u hebt ondernomen de volumes offline op de host eerst voordat u de volumes offline op het apparaat nemen. Als u de volumes offline op de host niet uitvoert, moet dit kan leiden tot beschadigde gegevens.
   
4. Ga terug naar de **back-catalogus** tabblad en selecteert u een back-upset. Met de rechtermuisknop op en selecteer vervolgens in het contextmenu **herstellen**.

    ![Lijst met back-upset](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. U wordt gevraagd om bevestiging. Controleer de gegevens herstellen en selecteer vervolgens het selectievakje voor de bevestiging.
   
    ![Bevestigingspagina](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7. Klik op **herstellen**. Hiermee initieert een hersteltaak die u door de toegang tot bekijken kunt de **taken** pagina.

   ![Bevestigingspagina](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. Nadat het herstel voltooid is, controleert u of dat de inhoud van uw volumes worden vervangen door de volumes van de back-up.


## <a name="if-the-restore-fails"></a>Als het herstellen is mislukt

U ontvangt een waarschuwing als de herstelbewerking opnieuw om welke reden dan ook mislukt. Als dit het geval is, moet u de back-lijst om te bevestigen dat de back-up nog geldig vernieuwen. Als de back-up geldig is en u vanuit de cloud herstellen wilt, klikt u vervolgens problemen met de netwerkverbinding mogelijk oorzaak van het probleem.

Voor het voltooien van de herstelbewerking, het volume offline nemen op de host en voer de herstelbewerking opnieuw uit. Houd er rekening mee dat alle wijzigingen in de volumegegevens die zijn uitgevoerd tijdens het herstelproces, gaan verloren.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [beheren StorSimple-volumes](storsimple-8000-manage-volumes-u2.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

