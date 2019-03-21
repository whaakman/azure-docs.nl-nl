---
title: U kunt MPIO configureren voor uw StorSimple-apparaat | Microsoft Docs
description: Beschrijft hoe u Multipath I/O (MPIO) configureren voor uw StorSimple-apparaat dat is verbonden met een host met Windows Server 2012 R2.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: alkohli
ms.openlocfilehash: eda134257edb851eea076459b44e02fc59028f46
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58078139"
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>Multipath I/O voor uw StorSimple-apparaat configureren

In deze zelfstudie beschrijft de stappen die u moet volgen om te installeren en gebruiken van de functie Multipath I/O (MPIO) op een host waarop Windows Server 2012 R2 wordt uitgevoerd en die zijn verbonden met een fysiek StorSimple-apparaat. De instructies in dit artikel is van toepassing op StorSimple 8000-serie alleen fysieke apparaten. MPIO wordt momenteel niet ondersteund op een StorSimple Cloud Appliance.

Nu Microsoft ondersteuning voor de functie Multipath I/O (MPIO) in Windows Server kunt opbouwen van maximaal beschikbare en fouttolerante iSCSI-netwerkconfiguraties ingebouwd. MPIO maakt gebruik van componenten van redundante fysieke paden – adapters, kabels en switches: te maken van logische paden tussen de server en het opslagapparaat. Als er een onderdeel optreedt, veroorzaakt door een logische pad op naar het mislukken, Multipath-logica wordt gebruikgemaakt van een alternatief pad voor i/o zodat toepassingen nog steeds toegang hun gegevens tot kunnen. Bovendien afhankelijk van uw configuratie MPIO kan de prestaties ook verbeteren door het herverdelen van de belasting over alle deze paden. Zie voor meer informatie, [MPIO overzicht](https://technet.microsoft.com/library/cc725907.aspx "MPIO overzicht en onderdelen").

Voor de hoge beschikbaarheid van uw StorSimple-oplossing, moet de MPIO op uw StorSimple-apparaat worden geconfigureerd. Wanneer MPIO is geïnstalleerd op uw hostservers waarop Windows Server 2012 R2 wordt uitgevoerd, worden de servers kunnen vervolgens tolereren een koppeling-, netwerk- of Interfacefout.

## <a name="mpio-configuration-summary"></a>Samenvatting configuratie van MPIO

MPIO is een optionele functie in Windows Server en wordt niet standaard geïnstalleerd. Het moet als een functie via Serverbeheer worden geïnstalleerd.

Volg deze stappen voor het configureren van MPIO op uw StorSimple-apparaat:

* Stap 1: Installeren van MPIO op de host Windows Server
* Stap 2: U kunt MPIO configureren voor StorSimple-volumes
* Stap 3: Koppelpunt StorSimple-volumes op de host
* Stap 4: U kunt MPIO configureren voor hoge beschikbaarheid en taakverdeling

Elk van de voorgaande stappen wordt in de volgende secties besproken.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Stap 1: Installeren van MPIO op de host Windows Server

Voer de volgende procedure voor het installeren van deze functie op uw Windows Server-host.

#### <a name="to-install-mpio-on-the-host"></a>Het installeren van MPIO op de host

1. Open Serverbeheer op uw Windows Server-host. Serverbeheer wordt standaard gestart wanneer een lid van de groep Administrators zich aanmeldt bij een computer waarop Windows Server 2012 R2 of Windows Server 2012 wordt uitgevoerd. Als de Server Manager nog niet is geopend, klikt u op **Start > Serverbeheer**.
   
   ![Server Manager](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Klik op **Serverbeheer > Dashboard > functies en onderdelen toevoegen**. Hiermee start u de **functies en onderdelen toevoegen** wizard.
   
   ![Toevoegen van de Wizard functies en onderdelen 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. In de **functies en onderdelen toevoegen** wizard de volgende stappen uitvoeren:
   
   1. Op de **voordat u begint met** pagina, klikt u op **volgende**.
   2. Op de **installatietype selecteren** pagina, accepteer de standaardinstelling van **op basis van rollen of op basis van functie** installatie. Klik op **volgende**.
   
       ![Wizard functies en onderdelen 2 toevoegen](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. Op de **Selecteer doelserver** pagina, kies **Selecteer een server uit de servergroep**. Uw host-server moet automatisch worden gedetecteerd. Klik op **volgende**.
   4. Op de **Serverfuncties selecteren** pagina, klikt u op **volgende**.
   5. Op de **functies selecteren** pagina, selecteert u **Multipath i/o**, en klikt u op **volgende**.
   
       ![Wizard functies en onderdelen 5 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. Op de **installatie-instellingen bevestigen** pagina, Controleer of de selectie en selecteer vervolgens **de doelserver automatisch opnieuw opstarten indien nodig**, zoals hieronder weergegeven. Klik op **Install**.
   
       ![Wizard functies en onderdelen 8 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. U wordt gewaarschuwd wanneer de installatie voltooid is. Klik op **Sluiten** om de wizard te sluiten.
   
       ![Wizard functies en onderdelen 9 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Stap 2: U kunt MPIO configureren voor StorSimple-volumes

MPIO moet worden geconfigureerd voor het identificeren van StorSimple-volumes. Voor het configureren van MPIO voor het herkennen van StorSimple-volumes, moet u de volgende stappen uitvoeren.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>MPIO configureren voor StorSimple-volumes

1. Open de **MPIO-configuratie**. Klik op **Serverbeheer > Dashboard > Tools > MPIO**.
2. In de **eigenschappen voor MPIO** in het dialoogvenster, selecteer de **detecteren met meerdere paden** tabblad.
3. Selecteer **is ondersteuning toegevoegd voor iSCSI-apparaten**, en klik vervolgens op **toevoegen**.  
   ![Eigenschappen voor MPIO detecteren meerdere paden](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. Start opnieuw op de server wanneer u hierom wordt gevraagd.
5. In de **eigenschappen voor MPIO** in het dialoogvenster, klikt u op de **MPIO apparaten** tabblad. Klik op **Toevoegen**.
    </br>![MPIO eigenschappen MPIO-apparaten](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. In de **MPIO-ondersteuning toevoegen** dialoogvenster onder **Hardware-ID apparaat**, voer het serienummer van uw apparaat. Als u het serienummer van apparaat, toegang krijgen tot uw StorSimple Device Manager-service. Navigeer naar **apparaten > Dashboard**. Het serienummer van het apparaat wordt weergegeven in het rechter **snel in een oogopslag** deelvenster van het apparaatdashboard.
    </br>
    ![Ondersteuning voor MPIO toevoegen](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Start opnieuw op de server wanneer u hierom wordt gevraagd.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Stap 3: Koppelpunt StorSimple-volumes op de host

Nadat MPIO op Windows Server is geconfigureerd, worden de volumes die zijn gemaakt op het StorSimple-apparaat kunnen worden gekoppeld en kunnen vervolgens profiteren van MPIO voor redundantie. Als u wilt een volume koppelen, moet u de volgende stappen uitvoeren.

#### <a name="to-mount-volumes-on-the-host"></a>Koppelen van volumes op de host

1. Open de **eigenschappen iSCSI-Initiator** -venster op de Windows Server-host. Klik op **Serverbeheer > Dashboard > Tools > iSCSI-Initiator**.
2. In de **eigenschappen iSCSI-Initiator** in het dialoogvenster, klikt u op het tabblad detectie en klik vervolgens op **doelportaal detecteren**.
3. In de **doelportaal detecteren** dialoogvenster vak, voer de volgende stappen uit:
   
   1. Geef het IP-adres van de poort van de gegevens van uw StorSimple-apparaat (bijvoorbeeld enter DATA 0).
   2. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
     
      > [!IMPORTANT]
      > **Als u van een particulier netwerk voor iSCSI-verbindingen gebruikmaakt, voert u het IP-adres van de poort die is verbonden met het particuliere netwerk.**
    
4. Herhaal stappen 2-3 voor een tweede netwerkinterface (bijvoorbeeld, gegevens-1) op uw apparaat. Houd er rekening mee dat deze interfaces moeten worden ingeschakeld voor iSCSI. Zie voor meer informatie, [netwerkinterfaces wijzigen](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Selecteer de **doelen** tabblad de **eigenschappen iSCSI-Initiator** in het dialoogvenster. U ziet het StorSimple-apparaat als doel IQN onder **gedetecteerde doelen**.

   ![iSCSI-Initiator eigenschappen doelen tabblad](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Klik op **Connect** tot stand brengen van een iSCSI-sessie met uw StorSimple-apparaat. Een **verbinding maken met doel** in het dialoogvenster wordt weergegeven.
7. In de **verbinding maken met doel** in het dialoogvenster, selecteer de **inschakelen Multipath** selectievakje. Klik op **Advanced**.
8. In de **geavanceerde instellingen** dialoogvenster vak, voer de volgende stappen uit:
   
   1. Op de **lokale Adapter** vervolgkeuzelijst, selecteer **Microsoft iSCSI-Initiator**.
   2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres van de host.
   3. Op de **doelportal** IP-vervolgkeuzelijst, selecteert u het IP-adres van apparaatinterface.
   4. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
9. Klik op **Eigenschappen**. In de **eigenschappen** in het dialoogvenster, klikt u op **sessie toevoegen**.
10. In de **verbinding maken met doel** in het dialoogvenster, selecteer de **inschakelen Multipath** selectievakje. Klik op **Advanced**.
11. In de **geavanceerde instellingen** in het dialoogvenster:

    1. Op de **lokale adapter** vervolgkeuzelijst, selecteer Microsoft iSCSI-Initiator.
    2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres overeenkomt met de host. In dit geval verbinding u twee netwerkinterfaces op het apparaat met één netwerkinterface op de host. Deze interface is daarom hetzelfde als die voor de eerste sessie.
    3. Op de **doel Portal IP** vervolgkeuzelijst, selecteer het IP-adres voor de tweede gegevens interface is ingeschakeld op het apparaat.
    4. Klik op **OK** om terug te keren naar het dialoogvenster voor iSCSI-Initiator-eigenschappen. U hebt een tweede sessie toegevoegd aan het doel.
12. Open **Computerbeheer** door te navigeren naar **Serverbeheer > Dashboard > Computerbeheer**. Klik in het linkerdeelvenster op **opslag > Schijfbeheer**. Het volume dat is gemaakt op het StorSimple-apparaat die zichtbaar zijn voor deze host wordt weergegeven onder **Schijfbeheer** als nieuwe schijf of schijven.
13. Initialiseer de schijf en maak een nieuw volume. Tijdens de opmaak, selecteer een blokgrootte van 64 KB.
    
    ![Schijfbeheer](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. Onder **Schijfbeheer**, met de rechtermuisknop op de **schijf** en selecteer **eigenschappen**.
15. In het StorSimple Model ### **multipath schijf apparaateigenschappen** in het dialoogvenster, klikt u op de **MPIO** tabblad.
    
    ![StorSimple 8100 Multi-Path Disk DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. In de **DSM-naam** sectie, klikt u op **Details** en controleer of dat de parameters zijn ingesteld op de standaardparameters. De standaardparameters zijn:
    
    * Pad controleren periode = 30
    * Aantal nieuwe pogingen = 3
    * PDO periode verwijdert = 20
    * Interval voor opnieuw proberen = 1
    * Pad controleren ingeschakeld = uitgeschakeld.

> [!NOTE]
> **De standaardparameters mag niet worden gewijzigd.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Stap 4: U kunt MPIO configureren voor hoge beschikbaarheid en taakverdeling

Voor Multipath op basis van hoge beschikbaarheid en taakverdeling, moeten de meerdere sessies handmatig worden toegevoegd om aan te geven van de verschillende paden die beschikbaar zijn. Bijvoorbeeld, als de host heeft twee interfaces met iSCSI-netwerk verbonden en het apparaat heeft twee interfaces zijn verbonden met iSCSI-netwerk, moet u vier sessies die zijn geconfigureerd met het juiste pad permutaties (slechts twee sessies is vereist als elke DATA interface en hostinterface is in een ander IP-subnet en waarde niet routeerbaar is).

**Het is raadzaam dat u ten minste 8 parallelle actieve sessies tussen het apparaat en uw toepassingshost hebt.** Dit kan worden bereikt door in te schakelen 4-netwerkinterfaces op uw Windows Server-systeem. Fysieke netwerkinterfaces of virtuele interfaces via netwerk-technologieën voor virtualisatie gebruiken op het niveau van de hardware of besturingssysteem op uw Windows Server-host. Met de twee netwerkinterfaces op het apparaat leidt deze configuratie 8 actieve sessies. Deze configuratie kunt u de doorvoer van het apparaat en cloud optimaliseren.

> [!IMPORTANT]
> **Het is raadzaam om niet 1 GbE en 10 GbE-netwerkinterfaces te combineren. Als u twee netwerkinterfaces, moeten beide interfaces van een identieke type.**

De volgende procedure wordt beschreven hoe u sessies wanneer een StorSimple-apparaat met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces toevoegen. Hierdoor kunt u alleen 4 sessies. Gebruik dezelfde procedure met een StorSimple-apparaat met twee netwerkinterfaces die zijn verbonden met een host met vier netwerkinterfaces. U moet configureren 8 in plaats van de 4-sessies die hier worden beschreven.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>U kunt MPIO configureren voor hoge beschikbaarheid en taakverdeling

1. Uitvoeren van een detectie van het doel: in de **eigenschappen iSCSI-Initiator** dialoogvenster op de **detectie** tabblad **Portal detecteren**.
2. In de **verbinding maken met doel** dialoogvenster vak, voer het IP-adres van een van de apparaat-netwerkinterfaces.
3. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster.
4. In de **eigenschappen iSCSI-Initiator** in het dialoogvenster, selecteer de **doelen** tabblad, markeert u de gedetecteerde doel en klik vervolgens op **Connect**. De **verbinding maken met doel** in het dialoogvenster wordt weergegeven.
5. In de **verbinding maken met doel** in het dialoogvenster:
   
   1. Laat de standaard geselecteerde doel-instelling voor **toevoegen van deze verbinding** aan de lijst met favoriete doelen. Dit maakt het apparaat automatisch proberen opnieuw te starten van de verbinding telkens als deze computer opnieuw wordt opgestart.
   2. Selecteer de **inschakelen Multipath** selectievakje.
   3. Klik op **Advanced**.
6. In de **geavanceerde instellingen** in het dialoogvenster:
   
   1. Op de **lokale Adapter** vervolgkeuzelijst, selecteer **Microsoft iSCSI-Initiator**.
   2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres overeenkomt met de eerste interface op de host (iSCSI-interface).
   3. Op de **doel Portal IP** vervolgkeuzelijst, selecteer het IP-adres voor de eerste gegevens interface is ingeschakeld op het apparaat.
   4. Klik op **OK** om terug te keren naar het dialoogvenster voor iSCSI-Initiator-eigenschappen.
7. Klik op **eigenschappen**, en klik in de **eigenschappen** in het dialoogvenster, klikt u op **sessie toevoegen**.
8. In de **verbinding maken met doel** in het dialoogvenster, selecteer de **inschakelen Multipath** selectievakje en klik vervolgens op **Geavanceerd**.
9. In de **geavanceerde instellingen** in het dialoogvenster:
   
   1. Op de **lokale adapter** vervolgkeuzelijst, selecteer **Microsoft iSCSI-Initiator**.
   2. Op de **Initiator IP** vervolgkeuzelijst, selecteert u het IP-adres overeenkomt met de tweede iSCSI-interface op de host.
   3. Op de **doel Portal IP** vervolgkeuzelijst, selecteer het IP-adres voor de tweede gegevens interface is ingeschakeld op het apparaat.
   4. Klik op **OK** om terug te keren naar de **eigenschappen iSCSI-Initiator** in het dialoogvenster. U hebt nu een tweede sessie toegevoegd aan het doel.
10. Herhaal stap 8-10 extra sessies (paden) toevoegen aan het doel. U kunt een totaal van vier sessies toevoegen met twee interfaces op de host en twee gegevensbronnen op het apparaat.
11. Na het toevoegen van de gewenste sessies (paden) in de **eigenschappen iSCSI-Initiator** dialoogvenster vak, selecteert u het doel en klikt u op **eigenschappen**. Op het tabblad sessies van de **eigenschappen** in het dialoogvenster, houd er rekening mee de vier sessie-id's die met de mogelijke pad permutaties corresponderen. Een sessie te annuleren, selecteert u het selectievakje in naast een sessie-id en klik vervolgens op **verbinding verbreken**.
12. Als u wilt weergeven van apparaten binnen sessies weergegeven, selecteert u de **apparaten** tabblad. Om de MPIO-beleid voor een geselecteerd apparaat configureren, klikt u op **MPIO**. De **Apparaatdetails** in het dialoogvenster wordt weergegeven. Op de **MPIO** tabblad kunt u de juiste **beleid voor taakverdeling** instellingen. U kunt ook zien de **Active** of **stand-by** padtype.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [met de StorSimple Device Manager-service voor het wijzigen van de configuratie van uw StorSimple-apparaat](storsimple-8000-modify-device-config.md).

