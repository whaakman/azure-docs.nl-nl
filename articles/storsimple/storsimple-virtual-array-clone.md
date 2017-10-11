---
title: Klonen van een virtueel StorSimple-matrix back-up | Microsoft Docs
description: Informatie over het klonen van een back-up en herstellen van een bestand van uw virtuele StorSimple-matrix.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: af6e979c-55e3-477c-b53e-a76a697f80c9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 768c9a1c906999f4690c9c8f7d075743ab1678ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>Klonen van een back-up van uw virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

In dit artikel wordt stapsgewijs beschreven hoe u een back-upset van uw shares of volumes op uw Microsoft Azure StorSimple virtuele matrix klonen. De gekloonde back-up wordt gebruikt voor het herstellen van een bestand verwijderd of verloren. Het artikel bevat tevens gedetailleerde stappen voor het uitvoeren van een herstel op itemniveau op uw virtuele StorSimple-matrix geconfigureerd als een bestandsserver.

## <a name="clone-shares-from-a-backup-set"></a>Kloon-shares vanuit een back-upset

**Voordat u probeert te klonen shares, zorg ervoor dat er voldoende ruimte op het apparaat om deze bewerking te voltooien.** Voor het klonen van een back-up, in de [Azure-portal](https://portal.azure.com/), de volgende stappen uitvoeren.

#### <a name="to-clone-a-share"></a>Voor het klonen van een share

1. Blader naar **apparaten** blade. Selecteer en klik op het apparaat en klik vervolgens op **Shares**. Selecteer de share die u wilt klonen, met de rechtermuisknop op de share om aan te roepen het contextmenu. Selecteer **kloon**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. In de **kloon** blade, klikt u op **back-up > Selecteer** en doe vervolgens het volgende: 
   
   a.    Een back-up op dit apparaat op basis van het tijdsbereik filteren. U kunt kiezen uit **afgelopen 7 dagen**, **afgelopen 30 dagen**, en **afgelopen jaar**.
   
   b.    Selecteer in de lijst met gefilterde back-ups weergegeven, een back-up om van te klonen.
   
   c.    Klik op **OK**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. In de **kloon** blade, klikt u op **doel instellingen** en doe vervolgens het volgende:
   
   a.    Geef een sharenaam. De share-naam moet 3-127 tekens bevatten.
   
   b.    Geef optioneel een beschrijving voor de gekloonde share.
   
   c.    U kunt de share die u naar herstellen wilt het type niet wijzigen. Een gelaagde share wordt gekloond als een gelaagde en een lokaal vastgemaakt share als lokaal vastgemaakt.
   
   d.    De capaciteit is ingesteld als gelijk is aan de grootte van de share die u klonen uit.
   
   e.    Wijs de beheerders voor deze share. U kunt zich de eigenschappen van de share via Verkenner wijzigen nadat het klonen voltooid is.
   
   f.    Klik op **OK**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Klik op **kloon** om de taak van een kloon te starten. Nadat de taak voltooid is, wordt de bewerking klonen wordt gestart en u wordt gewaarschuwd. Om de voortgang van de kloon van gaat u naar de **taken** blade en klik op de taak om taakgegevens weer te geven.
5. Nadat het klonen is gemaakt, gaat u terug naar de **Shares** blade op uw apparaat.
6. U kunt nu de nieuwe share gekloonde weergeven in de lijst met shares op uw apparaat. Een gelaagde share is gekloond omdat lagen en een lokaal vastgemaakt share als een lokaal vastgemaakt share.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>Volumes klonen van een back-upset

Net als bij de stappen uitvoeren bij het klonen van een share klonen van een back-up in de Azure portal hebt. De kopieerbewerking wordt gekloond van de back-up naar een nieuw volume op dezelfde virtuele apparaat; u kunt geen klonen naar een ander apparaat.

#### <a name="to-clone-a-volume"></a>Een volume klonen

1. Blader naar **apparaten** blade. Selecteer en klik op het apparaat en klik vervolgens op **Volumes**. Het volume dat u klonen wilt, selec met de rechtermuisknop op het volume aan te roepen het contextmenu. Selecteer **kloon**.
   
   ![Een volume klonen](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. In de **kloon** blade, klikt u op **back-up** en doe vervolgens het volgende: 
   
   a.    Een back-up op dit apparaat op basis van het tijdsbereik filteren. U kunt kiezen uit **afgelopen 7 dagen**, **afgelopen 30 dagen**, en **afgelopen jaar**. 
   
   b.    Selecteer in de lijst met gefilterde back-ups weergegeven, een back-up om van te klonen.
   
   c.    Klik op **OK**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. In de **kloon** blade, klikt u op **volume-instellingen als doel** en voer de volgende::
   
   a. Naam van het apparaat wordt automatisch gevuld.
   
   b. Geef de naam van een volume voor de **gekloond volume**. Naam van het volume moet 3 tot en met 127 tekens bevatten.
   
   c. Het volumetype wordt automatisch ingesteld op het oorspronkelijke volume. Een gelaagd volume is gekloond omdat lagen en een lokaal vastgemaakt volume als lokaal vastgemaakt.
   
   d. Voor de **verbonden hosts**, klikt u op **Selecteer**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. In de **verbonden hosts** blade uit een bestaande ACR selecteren of een nieuwe ACR toevoegen. Als u wilt een nieuwe ACR toevoegen, moet u een ACR-naam en de host IQN opgeven. Klik op **Selecteren**.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Klik op **kloon** starten van een taak voor de kloon.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Nadat de kloon-taak is gemaakt, wordt klonen gestart. Als de kloon wordt gemaakt, wordt deze weergegeven op de blade Volumes op uw apparaat. Houd er rekening mee dat een gelaagd volume is gekloond omdat lagen en een lokaal vastgemaakt volume als een lokaal vastgemaakt volume wordt gekloond.
   
   ![Klonen van een back-up](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. Als het volume wordt weergegeven in de lijst van volumes online, is het volume is beschikbaar voor gebruik. Vernieuw de lijst met doelen in het eigenschappenvenster van iSCSI-initiator op de iSCSI-initiator-host. Een nieuw doel met de naam van de gekloonde volume moet worden weergegeven als 'inactive' onder de statuskolom.
8. Selecteer het doel en klik op **Connect**. Nadat de initiator is verbonden met het doel, de status moet worden gewijzigd in **verbonden**.
9. In de **Schijfbeheer** venster de gekoppelde volumes weergegeven zoals in de volgende afbeelding. Klik met de rechtermuisknop op het gedetecteerde volume (klik op de naam van de schijf) en klik vervolgens op **Online**.

> [!IMPORTANT]
> Bij het klonen van een volume of een share van een back-up wordt ingesteld als de taak voor het klonen mislukt, een doelvolume of share kan nog steeds in de portal worden gemaakt. Het is belangrijk dat u dit doelvolume verwijderen of in de portal delen om toekomstige problemen die voortkomen uit dit element.
> 
> 

## <a name="item-level-recovery-ilr"></a>Herstel op itemniveau (ILR)

Deze release bevat het herstel op itemniveau (ILR) op een virtueel StorSimple-matrix geconfigureerd als een bestandsserver. De functie kunt u gedetailleerde bestanden en mappen herstellen van een cloud back-up van alle shares op het StorSimple-apparaat. U kunt verwijderde bestanden ophalen van recente back-ups met behulp van een selfservice-model.

Elke share heeft een *.backups* map met de meest recente back-ups. U kunt gaat u naar de gewenste back-up, relevante bestanden en mappen kopiëren vanuit de back-up en herstel deze. Deze functie wordt voorkomen dat beheerders aanroepen voor het herstellen van bestanden vanuit back-ups.

1. Bij het uitvoeren van de ILR, kunt u de back-ups via Verkenner kunt weergeven. Klik op de specifieke delen die u wilt kijken naar de back-up voor. U ziet een *.backups* map gemaakt onder de share die de back-ups worden opgeslagen. Vouw de *.backups* map om weer te geven van de back-ups. De map bevat de geëxplodeerde weergave van de volledige back-hiërarchie. Deze weergave wordt gemaakt op aanvraag en gewoonlijk duurt slechts enkele seconden maken.
   
   De laatste vijf back-ups op deze manier worden weergegeven en kunnen worden gebruikt een herstel op itemniveau uit te voeren. De vijf recente back-ups bevatten zowel de standaardwaarde is gepland als de handmatige back-ups.
   
   * **Geplande back-ups** met de naam als &lt;apparaatnaam&gt;DailySchedule JJJJMMDD-UUMMSS UTC.
   * **Handmatige back-ups** met de naam als Ad-hoc-JJJJMMDD-UUMMSS-UTC.
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Identificeer de back-up die de meest recente versie van het verwijderde bestand. Hoewel de naam van de map bevat een UTC-timestamp in elk van de bovenstaande gevallen, het tijdstip waarop de map is gemaakt is de daadwerkelijk apparaat waarop de back-up is gestart. Gebruik de map tijdstempel vinden en identificeren van de back-ups.

3. Ga naar de map of het bestand dat u wilt herstellen in de back-up die u hebt geïdentificeerd in de vorige stap. Opmerking: u kunt alleen de bestanden of mappen die u machtigingen hebt voor weergeven. Als u geen toegang bepaalde bestanden of mappen tot, moet u contact op met de beheerder van een share. De beheerder kan Verkenner gebruiken om te bewerken van machtigingen voor de share en krijgt u toegang tot het specifieke bestand of map. Het is een aanbevolen procedure dat de beheerder van de share een gebruikersgroep in plaats van een enkele gebruiker is.

4. Kopieer het bestand of de map naar de juiste share op uw StorSimple-bestandsserver.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw virtuele StorSimple-matrix met de lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md).

