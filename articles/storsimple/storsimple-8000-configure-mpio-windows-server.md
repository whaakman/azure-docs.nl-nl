---
title: MPIO configureren voor uw StorSimple-apparaat | Microsoft Docs
description: Beschrijft hoe u Multipath I/O (MPIO) configureert voor uw StorSimple-apparaat is verbonden met een host waarop Windows Server 2012 R2 wordt uitgevoerd.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 9fe3fa3a2df63d111de742ecb48b1469aad543cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>MPIO configureren voor uw StorSimple-apparaat

Deze zelfstudie bevat de stappen die u moet volgen om te installeren en gebruiken van de functie Multipath I/O (MPIO) op een host waarop Windows Server 2012 R2 en verbonden met een fysiek StorSimple-apparaat. De instructies in dit artikel is van toepassing op StorSimple 8000-serie alleen fysieke apparaten. MPIO wordt momenteel niet ondersteund op een StorSimple-apparaat met Cloud.

Microsoft ondersteuning voor de functie Multipath I/O (MPIO) in Windows Server naar Help-informatie samenstellen maximaal beschikbare en fouttolerantie SAN configuraties gebouwd. MPIO maakt gebruik van componenten van fysieke paden redundante: adapters, kabels en switches – om logische paden tussen de server en het opslagapparaat te creëren. Als er een onderdeelfout veroorzaakt door een logische pad mislukken, overgestapt gebruik van meerdere paden op een alternatief pad voor i/o zodat toepassingen steeds toegang hun gegevens tot nog. Bovendien afhankelijk van uw configuratie MPIO kunt ook de prestaties verbeteren door de belasting herverdeling over alle deze paden. Zie voor meer informatie [MPIO overzicht](https://technet.microsoft.com/library/cc725907.aspx "MPIO overzicht en kenmerken").

Voor de hoge beschikbaarheid van uw StorSimple-oplossing, moet de MPIO op uw StorSimple-apparaat worden geconfigureerd. Wanneer de MPIO op de hostservers met Windows Server 2012 R2 is geïnstalleerd, worden de servers kunnen vervolgens tolereren een koppeling-, netwerk- of Interfacefout.

## <a name="mpio-configuration-summary"></a>Samenvatting configuratie van MPIO

MPIO is een optionele functie in Windows Server en wordt niet standaard geïnstalleerd. Het moet als een functie via Serverbeheer worden geïnstalleerd.

Volg deze stappen voor het configureren van MPIO op uw StorSimple-apparaat:

* Stap 1: Installeer de MPIO op de host Windows Server
* Stap 2: MPIO configureren voor StorSimple-volumes
* Stap 3: Koppelpunt StorSimple-volumes op de host
* Stap 4: MPIO configureren voor hoge beschikbaarheid en taakverdeling

Elk van de voorgaande stappen wordt in de volgende secties besproken.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Stap 1: Installeer de MPIO op de host Windows Server

Voer de volgende procedure om de installatie van deze functie op de host Windows Server.

#### <a name="to-install-mpio-on-the-host"></a>Voor het installeren van MPIO op de host

1. Open Serverbeheer op uw Windows Server-host. Serverbeheer wordt standaard gestart wanneer een lid van de groep Administrators zich aanmeldt bij een computer waarop Windows Server 2012 R2 of Windows Server 2012 wordt uitgevoerd. Als de Server Manager nog niet is geopend, klikt u op **Start > Serverbeheer**.
   
   ![Serverbeheer](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Klik op **Serverbeheer > Dashboard > functies en onderdelen toevoegen**. Hiermee start u de **functies en onderdelen toevoegen** wizard.
   
   ![Wizard functies en onderdelen 1 toevoegen](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. In de **functies en onderdelen toevoegen** wizard de volgende stappen uitvoeren:
   
   1. Op de **voordat u begint met** pagina, klikt u op **volgende**.
   2. Op de **installatietype selecteren** pagina, accepteer de standaardinstelling van **op basis van functie of onderdeel gebaseerde** installatie. Klik op **Volgende**.
   
       ![Wizard functies en onderdelen 2 toevoegen](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. Op de **Select doelserver** pagina **Selecteer een server uit de servergroep**. De hostserver moet automatisch worden gedetecteerd. Klik op **Volgende**.
   4. Op de **Serverfuncties selecteren** pagina, klikt u op **volgende**.
   5. Op de **functies selecteren** pagina **Multipath i/o**, en klik op **volgende**.
   
       ![Wizard functies en onderdelen 5 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. Op de **installatieselecties bevestigen** pagina, bevestig de selectie en selecteer vervolgens **de doelserver automatisch opnieuw opstarten indien nodig**, zoals hieronder weergegeven. Klik op **Install**.
   
       ![Wizard functies en onderdelen 8 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. U wordt gewaarschuwd wanneer de installatie voltooid is. Klik op **Sluiten** om de wizard te sluiten.
   
       ![Wizard functies en onderdelen 9 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Stap 2: MPIO configureren voor StorSimple-volumes

MPIO moet worden geconfigureerd voor het identificeren van StorSimple-volumes. Voor het configureren van MPIO StorSimple-volumes herkent de volgende stappen uitvoeren.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>MPIO configureren voor StorSimple-volumes

1. Open de **MPIO-configuratie**. Klik op **Serverbeheer > Dashboard > Extra > MPIO**.
2. In de **eigenschappen voor MPIO** selecteert u de **detecteren met meerdere paden** tabblad.
3. Selecteer **ondersteuning voor iSCSI-apparaten toevoegen**, en klik vervolgens op **toevoegen**.  
   ![Eigenschappen voor MPIO detecteren meerdere paden](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. Start de server wanneer u wordt gevraagd.
5. In de **eigenschappen voor MPIO** in het dialoogvenster, klikt u op de **MPIO apparaten** tabblad. Klik op **Add**.
    </br>![MPIO eigenschappen MPIO-apparaten](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. In de **MPIO-ondersteuning toevoegen** dialoogvenster onder **Hardware-ID apparaat**, voer het serienummer van uw apparaat. Als u het serienummer van het apparaat, toegang krijgen tot uw StorSimple-apparaat Manager-service. Navigeer naar **apparaten > Dashboard**. Het serienummer van het apparaat wordt weergegeven in het recht **snel in één oogopslag** deelvenster van het dashboard van het apparaat.
    </br>
    ![Ondersteuning voor MPIO toevoegen](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Start de server wanneer u wordt gevraagd.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Stap 3: Koppelpunt StorSimple-volumes op de host

Nadat MPIO op Windows Server is geconfigureerd, wordt volumes die zijn gemaakt op het StorSimple-apparaat kan worden gekoppeld en kunnen vervolgens profiteren van MPIO voor redundantie. Een volume koppelen, moet u de volgende stappen uitvoeren.

#### <a name="to-mount-volumes-on-the-host"></a>Koppelen van volumes op de host

1. Open de **eigenschappen iSCSI-Initiator** venster op de host Windows Server. Klik op **Serverbeheer > Dashboard > Extra > iSCSI-Initiator**.
2. In de **eigenschappen iSCSI-Initiator** in het dialoogvenster, klikt u op het tabblad detectie en klik vervolgens op **doelportaal detecteren**.
3. In de **doelportaal detecteren** dialoogvenster vak, voert u de volgende stappen uit:
   
   1. Geef het IP-adres voor de poort van de gegevens van uw StorSimple-apparaat (bijvoorbeeld enter DATA 0).
   2. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
     
     > [!IMPORTANT]
     > **Als u van een particulier netwerk voor iSCSI-verbindingen gebruikmaakt, voert u het IP-adres van de poort die is verbonden met het particuliere netwerk.**
    
4. Herhaal stappen 2-3 voor een tweede netwerkinterface (bijvoorbeeld, gegevens-1) op uw apparaat. Houd er rekening mee dat deze interfaces moeten worden ingeschakeld voor iSCSI. Zie voor meer informatie [netwerkinterfaces wijzigen](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Selecteer de **doelen** tabblad de **eigenschappen iSCSI-Initiator** in het dialoogvenster. U ziet het StorSimple-apparaat zijn gericht IQN onder **gedetecteerde doelen**.

   ![iSCSI-Initiator eigenschappen doelen tabblad](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Klik op **Connect** tot stand brengen van een iSCSI-sessie met uw StorSimple-apparaat. Een **verbinding maken met doel** dialoogvenster wordt weergegeven.
7. In de **verbinding maken met doel** selecteert u de **Multipath inschakelen** selectievakje. Klik op **geavanceerde**.
8. In de **geavanceerde instellingen** dialoogvenster vak, voert u de volgende stappen uit:
   
   1. Op de **lokale Adapter** vervolgkeuzelijst, selecteer **Microsoft iSCSI-Initiator**.
   2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres van de host.
   3. Op de **doelportal** IP-vervolgkeuzelijst, selecteert u het IP-adres van apparaatinterface.
   4. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
9. Klik op **Eigenschappen**. In de **eigenschappen** in het dialoogvenster, klikt u op **sessie toevoegen**.
10. In de **verbinding maken met doel** selecteert u de **Multipath inschakelen** selectievakje. Klik op **geavanceerde**.
11. In de **geavanceerde instellingen** in het dialoogvenster:

    1. Op de **lokale adapter** vervolgkeuzelijst, selecteer Microsoft iSCSI-Initiator.
    2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres overeenkomt met de host. In dit geval u verbinding maakt twee netwerkinterfaces op het apparaat met één netwerkinterface op de host. Deze interface is daarom hetzelfde als die opgegeven voor de eerste sessie.
    3. Op de **IP-adres van doel Portal** vervolgkeuzelijst, selecteer het IP-adres voor de tweede gegevens interface is ingeschakeld op het apparaat.
    4. Klik op **OK** om terug te keren naar het dialoogvenster voor iSCSI-Initiator eigenschappen. U kunt een tweede sessie hebt toegevoegd aan het doel.
12. Open **Computerbeheer** door te navigeren naar **Serverbeheer > Dashboard > Computerbeheer**. Klik in het linkerdeelvenster op **opslag > Schijfbeheer**. Het volume dat is gemaakt op het StorSimple-apparaat die zichtbaar zijn voor deze host wordt weergegeven onder **Schijfbeheer** als nieuwe schijf of schijven.
13. Initialiseer de schijf en maak een nieuw volume. Tijdens het proces indeling selecteert een blokgrootte van 64 KB.
    
    ![Schijfbeheer](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. Onder **Schijfbeheer**, met de rechtermuisknop op de **schijf** en selecteer **eigenschappen**.
15. In het StorSimple Model ### **apparaateigenschappen multipath-schijf** in het dialoogvenster, klikt u op de **MPIO** tabblad.
    
    ![StorSimple 8100 multipath-schijf DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. In de **DSM-naam** sectie, klikt u op **Details** en controleer of dat de parameters zijn ingesteld op de standaardparameters. De standaardparameters zijn:
    
    * Pad controleren periode = 30
    * Aantal nieuwe pogingen = 3
    * PDO periode verwijdert = 20
    * Interval voor nieuwe pogingen = 1
    * Pad controleren ingeschakeld = niet ingeschakeld.

> [!NOTE]
> **De standaardparameters mag niet worden gewijzigd.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Stap 4: MPIO configureren voor hoge beschikbaarheid en taakverdeling

Voor Multipath op basis van hoge beschikbaarheid en taakverdeling, moeten meerdere sessies handmatig worden toegevoegd aan het declareren van de verschillende paden die beschikbaar zijn. Bijvoorbeeld, als de host heeft twee interfaces zijn verbonden met een SAN en het apparaat heeft twee interfaces zijn verbonden met een SAN, moet u vier sessies die zijn geconfigureerd met het juiste pad permutaties (slechts twee sessies worden als elke interface van de gegevens en de hostinterface op een ander IP-subnet en kan niet worden omgeleid).

**We raden u aan ten minste 8 parallelle actieve sessies tussen het apparaat en de toepassingshost van uw.** Dit kan worden bereikt door het inschakelen van 4-netwerkinterfaces op uw Windows Server-systeem. Fysieke netwerkinterfaces of virtuele interfaces via network-technologieën voor virtualisatie gebruiken op het niveau van hardware of besturingssysteem op uw Windows Server-host. Met twee netwerkinterfaces op het apparaat, worden deze configuratie zou leiden tot 8 actieve sessies. Deze configuratie helpt om het apparaat en cloud-doorvoer te optimaliseren.

> [!IMPORTANT]
> **Het is raadzaam dat u niet door elkaar 1 GbE- en 10 GbE-netwerkinterfaces. Als u twee netwerkinterfaces gebruikt, moet beide interfaces het type identiek zijn.**

De volgende procedure wordt beschreven hoe om toe te voegen sessies wanneer een StorSimple-apparaat met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces. U kunt alleen 4 sessies. Gebruik dezelfde procedure met een StorSimple-apparaat met twee netwerkinterfaces verbonden met een host met vier netwerkinterfaces. U moet 8 in plaats van de hier beschreven 4-sessies te configureren.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Voor MPIO configureren voor hoge beschikbaarheid en taakverdeling

1. Het doel te doorzoeken: in de **eigenschappen iSCSI-Initiator** in het dialoogvenster op de **detectie** tabblad **Portal detecteren**.
2. In de **verbinding maken met doel** dialoogvenster en voer het IP-adres van een van de netwerkinterfaces van het apparaat.
3. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
4. In de **eigenschappen iSCSI-Initiator** selecteert u de **doelen** tabblad, markeer het gedetecteerde doel en klik vervolgens op **Connect**. De **verbinding maken met doel** dialoogvenster wordt weergegeven.
5. In de **verbinding maken met doel** in het dialoogvenster:
   
   1. Laat de standaard geselecteerde doel-instelling voor **toevoegen van deze verbinding** aan de lijst met favoriete doelen. Hierdoor kunt u het apparaat automatisch geprobeerd de verbinding opnieuw te starten elke keer dat deze computer opnieuw wordt opgestart.
   2. Selecteer de **Multipath inschakelen** selectievakje.
   3. Klik op **geavanceerde**.
6. In de **geavanceerde instellingen** in het dialoogvenster:
   
   1. Op de **lokale Adapter** vervolgkeuzelijst, selecteer **Microsoft iSCSI-Initiator**.
   2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres van de host.
   3. Op de **IP-adres van doel Portal** vervolgkeuzelijst, selecteer het IP-adres van de interface van de gegevens die zijn ingeschakeld op het apparaat.
   4. Klik op **OK** om terug te keren naar het dialoogvenster voor iSCSI-Initiator eigenschappen.
7. Klik op **eigenschappen**, en in de **eigenschappen** in het dialoogvenster, klikt u op **sessie toevoegen**.
8. In de **verbinding maken met doel** selecteert u de **Multipath inschakelen** selectievakje en klik vervolgens op **Geavanceerd**.
9. In de **geavanceerde instellingen** in het dialoogvenster:
   
   1. Op de **lokale adapter** vervolgkeuzelijst, selecteer **Microsoft iSCSI-Initiator**.
   2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres overeenkomt met de tweede interface op de host.
   3. Op de **IP-adres van doel Portal** vervolgkeuzelijst, selecteer het IP-adres voor de tweede gegevens interface is ingeschakeld op het apparaat.
   4. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster. U hebt nu een tweede sessie toegevoegd aan het doel.
10. Herhaal stap 8-10 extra sessies (paden) toevoegen aan het doel. U kunt een totaal van vier sessies toevoegen met twee interfaces op de host en twee gegevensbronnen op het apparaat.
11. Na het toevoegen van de gewenste sessies (paden) in de **eigenschappen iSCSI-Initiator** dialoogvenster vak, selecteert u het doel en klik op **eigenschappen**. Op het tabblad sessies van de **eigenschappen** in het dialoogvenster, notitie de vier sessie-id's die met het aantal permutaties van mogelijke pad overeenkomen. Annuleren van een sessie, schakel het selectievakje in naast een sessie-id en klik vervolgens op **Disconnect**.
12. Als apparaten wilt weergeven die zijn gepresenteerd in sessies, selecteer de **apparaten** tabblad. Om de MPIO-beleid voor een geselecteerd apparaat configureren, klikt u op **MPIO**. De **Apparaatdetails** dialoogvenster wordt weergegeven. Op de **MPIO** tabblad kunt u de juiste **taakverdelingsbeleid** instellingen. U kunt ook bekijken de **Active** of **stand-by** padtype.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [de StorSimple-apparaat Manager-service gebruikt om te wijzigen van de configuratie van uw StorSimple-apparaat](storsimple-8000-modify-device-config.md).

