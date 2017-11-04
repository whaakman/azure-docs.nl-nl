---
title: MPIO configureren op de host verbonden is met het virtuele StorSimple-matrix | Microsoft Docs
description: Beschrijft hoe u Multipath I/O (MPIO) configureert voor uw virtuele StorSimple-matrix is verbonden met een host waarop Windows Server 2012 R2 wordt uitgevoerd.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: alkohli
ms.openlocfilehash: c75c6ed40754aee964e2b68f4f569dc1422507f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>MPIO configureren op Windows Server-host voor de virtuele StorSimple-matrix
## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe functie Multipath I/O (MPIO) installeert op uw Windows Server-host, specifieke configuratie-instellingen voor alleen-StorSimple-volumes van toepassing en controleer vervolgens MPIO voor StorSimple-volumes. De procedure wordt ervan uitgegaan dat uw virtuele StorSimple-1200-matrix met twee netwerkinterfaces is verbonden met een Windows Server-host met twee netwerkinterfaces. De informatie in dit artikel geldt alleen voor de virtuele-matrix. Voor informatie over apparaten van de StorSimple 8000-serie, gaat u naar [MPIO configureren voor StorSimple host](storsimple-configure-mpio-windows-server.md). 

De MPIO-functie in Windows Server helpt build maximaal beschikbare en fouttolerantie opslagconfiguraties. MPIO maakt gebruik van componenten van fysieke paden redundante: adapters, kabels en switches – om logische paden tussen de server en het opslagapparaat te creëren. Als er een onderdeelfout veroorzaakt door een logische pad mislukken, overgestapt gebruik van meerdere paden op een alternatief pad voor i/o zodat toepassingen steeds toegang hun gegevens tot nog. Bovendien afhankelijk van uw configuratie MPIO kunt ook de prestaties verbeteren door de belasting opnieuw te verdelen over alle deze paden. Zie voor meer informatie [MPIO overzicht](https://technet.microsoft.com/library/cc725907.aspx "MPIO overzicht en kenmerken").

Voor de hoge beschikbaarheid van uw StorSimple-oplossing, moet u MPIO configureren op de Windows Server-hosts die is verbonden met uw virtuele StorSimple-matrix (model 1200). De host-servers kunnen vervolgens een koppeling-, netwerk- of interfacefout tolereren. 

U moet deze stappen MPIO configureren: 

* Configuratievereisten
* Stap 1: Installeer de MPIO op de host Windows Server
* Stap 2: MPIO configureren voor StorSimple-volumes
* Stap 3: Koppelpunt StorSimple-volumes op de host

Elk van de bovenstaande stappen wordt in de volgende secties besproken.

## <a name="prerequisites"></a>Vereisten
In deze sectie beschrijft de configuratievereisten voor Windows Server-host en uw virtuele matrix.

### <a name="on-windows-server-host"></a>Op Windows Server-host
* Zorg ervoor dat uw Windows Server-host 2 netwerkinterfaces ingeschakeld heeft.

### <a name="on-storsimple-virtual-array"></a>Op de virtuele StorSimple-matrix
* De virtuele-matrix moet worden geconfigureerd als een iSCSI-server. Zie voor meer informatie, [virtuele matrix als iSCSI-server instellen](storsimple-virtual-array-deploy3-iscsi-setup.md). Een of meer netwerkinterfaces moeten worden ingeschakeld op de matrix.   
* De netwerkinterfaces op uw virtuele matrix moet bereikbaar is vanaf de Windows Server-host.
* Een of meer volumes moeten worden gemaakt op uw virtuele StorSimple-matrix. Zie voor meer informatie, [toevoegen van een volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume) op uw virtuele StorSimple-matrix. In deze procedure we 3 volumes (1 lokaal vastgemaakte en 2 gelaagde volumes zoals hieronder) gemaakt op de virtuele-matrix.
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardwareconfiguratie voor virtuele StorSimple-matrix
De afbeelding hieronder ziet de hardwareconfiguratie voor hoge beschikbaarheid en taakverdeling gebruik van meerdere paden voor uw Windows Server-host en virtuele StorSimple-matrix in deze procedure gebruikt.

![hardware-configuratie van MPIO](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Zoals u in de voorgaande afbeelding:

* Uw virtuele StorSimple-matrix is ingericht op Hyper-V is een enkel knooppunt active apparaat geconfigureerd als een iSCSI-server.
* Twee virtuele netwerkinterfaces zijn ingeschakeld op uw matrix. In de lokale webgebruikersinterface van uw virtuele matrix, controleert u of twee netwerkinterfaces zijn ingeschakeld door te navigeren naar **netwerkinstellingen** zoals hieronder wordt weergegeven:
  
    ![Netwerkinterfaces die zijn ingeschakeld op 1200](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Noteer de IPv4-adressen van de ingeschakelde netwerkinterfaces (Ethernet, Ethernet 2 standaard) en opslaan voor toekomstig gebruik op de host.
* Twee netwerkinterfaces zijn ingeschakeld op uw Windows Server-host. Als de verbonden interfaces voor de matrix en host zich op hetzelfde subnet bevinden, wordt er 4 paden beschikbaar. Dit is het geval is in deze procedure. Echter, als elke netwerkinterface op de matrix en host-interface op een ander IP-subnet (en niet-routeerbaar), vervolgens alleen 2 paden zijn beschikbaar.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Stap 1: Installeer de MPIO op de host Windows Server
MPIO is een optionele functie in Windows Server en wordt niet standaard geïnstalleerd. Het moet als een functie via Serverbeheer worden geïnstalleerd. Voer de volgende procedure om de installatie van deze functie op de host Windows Server.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Stap 2: MPIO configureren voor StorSimple-volumes
MPIO moet worden geconfigureerd voor het identificeren van StorSimple-volumes. Voor het configureren van MPIO StorSimple-volumes herkent de volgende stappen uitvoeren.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Stap 3: Koppelpunt StorSimple-volumes op de host
Nadat MPIO op Windows Server is geconfigureerd, of meer volumes op de StorSimple-matrix gemaakt kan worden gekoppeld en kan vervolgens profiteren van MPIO voor redundantie. Een volume koppelen, moet u de volgende stappen uitvoeren.

#### <a name="to-mount-volumes-on-the-host"></a>Koppelen van volumes op de host
1. Open de **eigenschappen iSCSI-Initiator** venster op de host Windows Server. Ga naar **Serverbeheer > Dashboard > Extra > iSCSI-Initiator**.
2. In de **eigenschappen iSCSI-Initiator** in het dialoogvenster, klikt u op **detectie**, en klik vervolgens op **doelportaal detecteren**.
3. In de **doelportaal detecteren** dialoogvenster de volgende handelingen uit:
   
    1. Voer het IP-adres van de eerste ingeschakeld voor de netwerkinterface op uw virtuele StorSimple-matrix. Standaard is dit **Ethernet**. 
    2. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
     
    > [!IMPORTANT]
    > Als u van een particulier netwerk voor iSCSI-verbindingen gebruikmaakt, voert u het IP-adres van de poort die is verbonden met het particuliere netwerk.
     
4. Herhaal stap 2-3 voor een tweede netwerkinterface (bijvoorbeeld Ethernet 2) in uw matrix. 
5. Selecteer de **doelen** tabblad de **eigenschappen iSCSI-Initiator** in het dialoogvenster. Voor uw virtuele matrix ziet u elk volume oppervlak als doel onder **gedetecteerde doelen**. In dit geval zou drie doelen (behorend bij drie volumes) worden gedetecteerd.
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Klik op **Connect** tot stand brengen van een iSCSI-sessie met uw virtuele StorSimple-matrix. Een **verbinding maken met doel** dialoogvenster wordt weergegeven. Selecteer de **Multipath inschakelen** selectievakje. Klik op **geavanceerde**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. In de **geavanceerde instellingen** dialoogvenster de volgende handelingen uit:                                        
   
    1. Op de **lokale Adapter** vervolgkeuzelijst, selecteer **Microsoft iSCSI-Initiator**.
    2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres van de host.
    3. Op de **doelportal** IP-vervolgkeuzelijst, selecteert u het IP-adres van de interface van de matrix.
    4. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. Klik op **Eigenschappen**. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. In de **eigenschappen** in het dialoogvenster, klikt u op **sessie toevoegen**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. In de **verbinding maken met doel** selecteert u de **Multipath inschakelen** selectievakje. Klik op **geavanceerde**.
11. In de **geavanceerde instellingen** in het dialoogvenster:                                        
    
    1. Op de **lokale adapter** vervolgkeuzelijst, selecteer Microsoft iSCSI-Initiator.

    2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres overeenkomt met de host. In dit geval u verbinding maakt twee netwerkinterfaces op de matrix met één netwerkinterface op de host. Deze interface is daarom hetzelfde als die opgegeven voor de eerste sessie.

    3. Op de **IP-adres van doel Portal** vervolgkeuzelijst, selecteer het IP-adres voor de tweede gegevens interface is ingeschakeld op de matrix.

    4. Klik op **OK** om terug te keren naar het dialoogvenster voor iSCSI-Initiator eigenschappen. U kunt een tweede sessie hebt toegevoegd aan het doel.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. Na het toevoegen van de gewenste sessies (paden) in de **eigenschappen iSCSI-Initiator** dialoogvenster vak, selecteert u het doel en klik op **eigenschappen**. Op het tabblad sessies van de **eigenschappen** in het dialoogvenster, notitie de vier sessie-id's die met het aantal permutaties van mogelijke pad overeenkomen. Annuleren van een sessie, schakel het selectievakje in naast een sessie-id en klik vervolgens op **Disconnect**.

    6. Als apparaten wilt weergeven die zijn gepresenteerd in sessies, selecteer de **apparaten** tabblad. Om de MPIO-beleid voor een geselecteerd apparaat configureren, klikt u op **MPIO**.

    7. De **Details** dialoogvenster wordt weergegeven. Op de **MPIO** tabblad kunt u de juiste **taakverdelingsbeleid** instellingen. U kunt ook bekijken de **Active** of **stand-by** padtype.
12. Herhaal stap 8-11 extra sessies (paden) toevoegen aan het doel. U kunt een totaal van vier sessies voor elk doel toevoegen met twee interfaces op de host en twee gegevensbronnen op de virtuele-matrix. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. U moet deze stappen herhalen voor elk volume (verwerkingsinformatie als doel).
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Open **Computerbeheer** door te navigeren naar **Serverbeheer > Dashboard > Computerbeheer**. Klik in het linkerdeelvenster op **opslag > Schijfbeheer**. De volumes op de virtuele StorSimple-matrix gemaakt die zichtbaar zijn voor deze host wordt weergegeven onder **Schijfbeheer** als nieuwe schijf of schijven.
15. Initialiseer de schijf en maak een nieuw volume. Tijdens het proces indeling selecteert u een clustergrootte (AUS) van 64 KB. Het proces herhalen voor alle beschikbare volumes.
    
    ![Schijfbeheer](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. Onder **Schijfbeheer**, met de rechtermuisknop op de **schijf** en selecteer **eigenschappen**.
17. In de **multipath schijf apparaateigenschappen** in het dialoogvenster, klikt u op de **MPIO** tabblad.
    
    ![Schijfeigenschappen](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. In de **DSM-naam** sectie, klikt u op **Details** en controleer of dat de parameters zijn ingesteld op de standaardparameters. De standaardparameters zijn:
    
    * Pad controleren periode = 30
    * Aantal nieuwe pogingen = 3
    * PDO periode verwijdert = 20
    * Interval voor nieuwe pogingen = 1
    * Pad controleren ingeschakeld = niet ingeschakeld.
    
    > [!NOTE]
    > **De standaardparameters mag niet worden gewijzigd.**
   
## <a name="next-steps"></a>Volgende stappen
Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw virtuele StorSimple-matrix](storsimple-virtual-array-manager-service-administration.md).

