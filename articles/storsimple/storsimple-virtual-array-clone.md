---
title: Klonen van een back-up van de StorSimple Virtual Array | Microsoft Docs
description: Informatie over het klonen van een back-up en herstellen van een bestand van uw StorSimple Virtual Array.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: af6e979c-55e3-477c-b53e-a76a697f80c9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: feffbb634af62d70a840febcf2a04afb7bdeeddd
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496786"
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>Klonen vanaf een back-up van uw StorSimple Virtual Array

## <a name="overview"></a>Overzicht

In dit artikel wordt stapsgewijs beschreven hoe u een back-upset van de shares of volumes op uw Microsoft Azure StorSimple Virtual Array klonen. De gekloonde back-up wordt gebruikt voor het herstellen van een bestand verwijderd of verloren gaan. Het artikel bevat ook gedetailleerde stappen voor een herstel op itemniveau uitvoeren op uw StorSimple Virtual Array geconfigureerd als een bestandsserver.

## <a name="clone-shares-from-a-backup-set"></a>Kloon-shares vanuit een back-upset

**Voordat u probeert te klonen shares, zorgt u ervoor dat er voldoende ruimte op het apparaat om deze bewerking te voltooien.** Om te klonen vanaf een back-up, in de [Azure-portal](https://portal.azure.com/), voer de volgende stappen uit.

#### <a name="to-clone-a-share"></a>Voor het klonen van een share

1. Blader naar **apparaten** blade. Selecteer en klik op het apparaat en klik vervolgens op **Shares**. Selecteer de bestandsshare die u wilt klonen, met de rechtermuisknop op de share voor het aanroepen van het contextmenu. Selecteer **Klonen**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. In de **kloon** blade, klikt u op **back-up > Selecteer** en vervolgens het volgende doen: 
   
   a.    Een back-up op dit apparaat op basis van het tijdsbereik filteren. U kunt kiezen uit **afgelopen 7 dagen**, **afgelopen 30 dagen**, en **afgelopen jaar**.
   
   b.    In de lijst met gefilterde back-ups weergegeven, selecteert u een back-up van klonen.
   
   c.    Klik op **OK**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. In de **kloon** blade, klikt u op **instellingen gericht** en vervolgens het volgende doen:
   
   a.    Geef een sharenaam. Naam van de share moet 3-127 tekens bevatten.
   
   b.    Geef eventueel een beschrijving voor de gekloonde share.
   
   c.    U kunt het type van de share die u naar wilt herstellen niet wijzigen. Een gelaagde share wordt gekloond als een gelaagde en een lokaal vastgemaakt share als lokaal vastgemaakt.
   
   d.    De capaciteit is ingesteld als gelijk zijn aan de grootte van de share die u uit zijn klonen.
   
   e.    De beheerders voor deze share toewijzen. U kunt zich de eigenschappen van de share via Verkenner wijzigen nadat het klonen voltooid is.
   
   f.    Klik op **OK**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Klik op **kloon** om een kloontaak te starten. Nadat de taak voltooid is, wordt de bewerking klonen wordt gestart en u krijgt een melding. Voor het controleren van de voortgang van de kloon van, gaat u naar de **taken** blade en klik op de taak om de taakdetails van de weer te geven.
5. Nadat het klonen is gemaakt, gaat u terug naar de **Shares** blade op uw apparaat.
6. U kunt nu de nieuwe gekloonde share in de lijst met shares weergeven op uw apparaat. Een gelaagde share is gekloond omdat lagen en een lokaal vastgemaakt share als een lokaal vastgemaakt share.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>Volumes klonen van een back-upset

Als u wilt klonen vanaf een back-up in de Azure-portal, die u moet uitvoeren die vergelijkbaar is met de stappen bij het klonen van een share. De bewerking klonen wordt gekloond van de back-up naar een nieuw volume op het virtuele apparaat dezelfde; u kunt geen klonen naar een ander apparaat.

#### <a name="to-clone-a-volume"></a>Op een volume klonen

1. Blader naar **apparaten** blade. Selecteer en klik op het apparaat en klik vervolgens op **Volumes**. Selecteer het volume dat u wilt klonen, met de rechtermuisknop op het volume voor het aanroepen van het contextmenu. Selecteer **Klonen**.
   
   ![Een volume klonen](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. In de **kloon** blade, klikt u op **back-up** en vervolgens het volgende doen: 
   
   a.    Een back-up op dit apparaat op basis van het tijdsbereik filteren. U kunt kiezen uit **afgelopen 7 dagen**, **afgelopen 30 dagen**, en **afgelopen jaar**. 
   
   b.    In de lijst met gefilterde back-ups weergegeven, selecteert u een back-up van klonen.
   
   c.    Klik op **OK**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. In de **kloon** blade, klikt u op **volume-instellingen als doel** en voer de volgende::
   
   a. Naam van het apparaat wordt automatisch ingevuld.
   
   b. Geef de naam van een volume voor de **gekloond volume**. Naam van het volume moet 3 tot en met 127 tekens bevatten.
   
   c. Het volumetype is automatisch ingesteld op het oorspronkelijke volume. Een gelaagd volume is gekloond omdat lagen en een lokaal vastgemaakt volume als lokaal vastgemaakt.
   
   d. Voor de **verbonden hosts**, klikt u op **Selecteer**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. In de **verbonden hosts** blade, selecteert u in een bestaande ACR of Voeg een nieuwe ACR toe. Om toe te voegen een nieuwe ACR, moet u een ACR-naam en de IQN-host te geven. Klik op **Selecteren**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Klik op **kloon** een kloontaak starten.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Nadat de kloontaak is gemaakt, wordt het klonen gestart. Nadat het klonen is gemaakt, wordt deze weergegeven op de blade Volumes op uw apparaat. Houd er rekening mee dat een gelaagd volume is gekloond omdat lagen en een lokaal vastgemaakt volume als een lokaal vastgemaakt volume is gekloond.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. Wanneer het volume wordt weergegeven in de lijst van volumes online, is het volume is beschikbaar voor gebruik. Vernieuw de lijst met doelen in het eigenschappenvenster van iSCSI-initiator op de iSCSI-initiator-host. Een nieuw doel met de naam van de gekloonde volume moet worden weergegeven als 'inactive' onder de statuskolom.
8. Selecteer het doel en klik op **Connect**. Nadat de initiator is verbonden met het doel, de status moet wijzigen naar **verbonden**.
9. In de **Schijfbeheer** venster gekoppelde volumes weergegeven zoals in de volgende afbeelding. Klik met de rechtermuisknop op het gedetecteerde volume (klik op de naam van de schijf) en klik vervolgens op **Online**.

> [!IMPORTANT]
> Bij het klonen van een volume of een bestandsshare vanaf een back-up instellen, als de taak klonen mislukt, een doelvolume of share nog steeds kan worden gemaakt in de portal. Het is belangrijk dat u dit doelvolume verwijderen of de share in de portal voor het minimaliseren van toekomstige problemen die ontstaan van dit element.
> 
> 

## <a name="item-level-recovery-ilr"></a>Herstel op itemniveau (ILR)

Deze versie introduceert het herstel op itemniveau (ILR) op een StorSimple Virtual Array geconfigureerd als een bestandsserver. De functie kunt u gedetailleerde herstel van bestanden en mappen van een cloudback-up van alle shares op het StorSimple-apparaat. U kunt verwijderde bestanden ophalen van recente back-ups met een selfservice-model.

Elke share heeft een *.backups* map met de meest recente back-ups. U kunt navigeren naar de gewenste back-up, relevante bestanden en mappen te kopiëren vanuit de back-up en herstellen. Deze functie wordt voorkomen dat aanroepen voor beheerders voor het herstellen van bestanden uit back-ups.

1. Bij het uitvoeren van de ILR, kunt u de back-ups met Bestandenverkenner kunt weergeven. Klik op de specifieke delen die u wilt kijken naar de back-up voor. U ziet een *.backups* map gemaakt op basis van de share waarin de back-ups. Vouw de *.backups* map om de back-ups weer te geven. De map bevat de geëxplodeerde weergave van de volledige back-hiërarchie. In deze weergave wordt gemaakt op aanvraag en duurt meestal slechts een paar seconden om te maken.
   
   De laatste vijf back-ups op deze manier worden weergegeven en kunnen worden gebruikt om een herstel op itemniveau uitvoeren. De vijf recente back-ups zijn zowel de standaardwaarde is gepland en de handmatige back-ups.
   
   * **Geplande back-ups** met de naam als &lt;apparaatnaam&gt;instelling DailySchedule JJJJMMDD-HHMMSS UTC.
   * **Handmatige back-ups** met de naam als de Ad-hoc-JJJJMMDD-HHMMSS-UTC.
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Identificeer de back-up die de meest recente versie van het verwijderde bestand. Hoewel de mapnaam van de een UTC-timestamp in elk van de bovenstaande gevallen bevat, is het tijdstip waarop de map is gemaakt de tijd op het werkelijke apparaat waarop de back-up is gestart. Gebruik de tijdstempel van de map te vinden en identificeren van de back-ups.

3. Ga naar de map of het bestand dat u wilt herstellen in de back-up die u in de vorige stap hebt geïdentificeerd. Opmerking: u kunt alleen de bestanden of mappen die u hebt machtigingen voor weergeven. Als u geen toegang bepaalde bestanden of mappen tot, neem dan contact op met de beheerder van een share. De beheerder kan File Explorer gebruiken om te bewerken van de sharemachtigingen en krijgt u toegang tot het specifieke bestand of map. Het is een aanbevolen procedure dat de beheerder van de share een groep in plaats van één gebruiker is.

4. Kopieer het bestand of de map naar de juiste share op uw StorSimple-bestandsserver.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple Virtual Array met behulp van de lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md).

