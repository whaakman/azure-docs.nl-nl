---
title: StorSimple Snapshot Manager implementeren | Microsoft Docs
description: Informatie over het downloaden en installeren van het StorSimple Snapshot Manager een MMC-module voor het beheren van StorSimple-beveiliging en back-up gegevensfuncties.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: cde355381b0d726a1ab340bc4230b2dc8f6e2c56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>Implementeer de StorSimple Snapshot Manager MMC-module

## <a name="overview"></a>Overzicht
De StorSimple Snapshot Manager is een module Microsoft Management Console (MMC) die vereenvoudigt de bescherming van gegevens en back-beheer in een Microsoft Azure StorSimple-omgeving. Met StorSimple Snapshot Manager kunt u Microsoft Azure StorSimple lokale beheren en dus processen voor back-up en herstel vereenvoudigen en de kosten te verlagen als een volledig geïntegreerde opslagsysteem cloudopslag. 

Deze zelfstudie wordt beschreven configuratievereisten, evenals de procedures voor het installeren, verwijderen en StorSimple Snapshot Manager upgraden.

> [!NOTE]
> * U kunt StorSimple Snapshot Manager niet gebruiken voor het beheren van virtuele matrices van Microsoft Azure StorSimple (ook wel bekend als StorSimple on-premises virtuele apparaten).
> * Als u van plan bent te StorSimple Update 2 op uw StorSimple-apparaat installeert, moet u de meest recente versie van StorSimple Snapshot Manager downloaden en installeren **voordat u het StorSimple Update 2 installeert**. De meest recente versie van StorSimple Snapshot Manager is achterwaarts compatibel en werkt met alle uitgebrachte versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple Snapshot Manager gebruikt, moet u bij te werken (u hoeft niet te verwijderen van de vorige versie voordat u de nieuwe versie installeert).


## <a name="storsimple-snapshot-manager-installation"></a>StorSimple Snapshot Manager-installatie
StorSimple Snapshot Manager kan worden geïnstalleerd op computers met de Windows Server 2008 R2 SP1, Windows Server 2012 of Windows Server 2012 R2-besturingssysteem. Op servers met Windows 2008 R2, moet u ook Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren.

Zorg voordat u installeren of de StorSimple Snapshot Manager-module voor de Microsoft Management Console (MMC upgraden), of de Microsoft Azure StorSimple-apparaat en host-server juist zijn geconfigureerd.

## <a name="configure-prerequisites"></a>Vereisten configureren
De volgende stappen bevatten een overzicht van configuratietaken die u uitvoeren moet voordat u het StorSimple Snapshot Manager installeert. Voor volledige Microsoft Azure StorSimple-configuratie en installatie-informatie, met inbegrip van de systeemvereisten en stapsgewijze instructies raadpleegt u [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Voordat u begint, controleert u de [Configuratiecontrolelijst voor implementatie](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) en en [implementatievereisten](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>Voordat u StorSimple Snapshot Manager installeren
1. Uitpakken, te koppelen en verbinding maken met het Microsoft Azure StorSimple-apparaat, zoals beschreven in [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
2. Zorg ervoor dat de hostcomputer wordt uitgevoerd een van de volgende besturingssystemen:
   
   * Windows Server 2008 R2 (op servers met Windows 2008 R2, moet u ook installeren Windows Server 2008 SP1 en Windows Management Framework 3.0)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     Voor een virtueel StorSimple-apparaat moet de host een virtuele Machine van de Microsoft Azure.
3. Zorg ervoor dat u voldoet aan de vereisten voor de configuratie van Microsoft Azure StorSimple. Voor meer informatie gaat u naar [implementatievereisten](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites).
4. Het apparaat aansluit op de host en de eerste configuratie uitvoeren. Voor meer informatie gaat u naar [implementatiestappen](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps).
5. Maken van volumes op het apparaat, toewijzen aan de host en controleer of de host kunt koppelen en ze gebruiken. StorSimple Snapshot Manager ondersteunt de volgende soorten volumes:
   
   * Basic-volumes
   * Eenvoudige volumes
   * Dynamische volumes
   * Gespiegelde dynamische volumes (RAID-1)
   * Gedeelde clustervolumes
     
     Voor informatie over het maken van volumes op de StorSimple-apparaat of een virtueel StorSimple-apparaat, gaat u naar [stap 6: een volume maken](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume)in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Een nieuwe StorSimple Snapshot Manager installeren
Voordat u StorSimple Snapshot Manager installeert, zorg ervoor dat de volumes die u hebt gemaakt op het StorSimple-apparaat of een virtueel StorSimple-apparaat zijn gekoppeld, geïnitialiseerd en geformatteerd zoals beschreven in [vereisten configureren](#configure-prerequisites).

> [!IMPORTANT]
> * Voor een virtueel StorSimple-apparaat moet de host een virtuele Machine van de Microsoft Azure. 
> * De host moet worden uitgevoerd, Windows 2008 R2, Windows Server 2012 of Windows Server 2012 R2. Als Windows Server 2008 R2 wordt uitgevoerd op uw server, moet u ook de Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren.
> * Voordat u het apparaat voor StorSimple Snapshot Manager kunt verbinden, moet u een iSCSI-verbinding tussen de host en het StorSimple-apparaat configureren.

Volg deze stappen om een nieuwe installatie van StorSimple Snapshot Manager te voltooien. Als u een upgrade installeren wilt, gaat u naar [bijwerken of opnieuw installeren van StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

* Stap 1: StorSimple Snapshot Manager installeren 
* Stap 2: Verbinding maken met StorSimple Snapshot Manager op een apparaat 
* Stap 3: Controleer of de verbinding met het apparaat 

### <a name="step-1-install-storsimple-snapshot-manager"></a>Stap 1: StorSimple Snapshot Manager installeren
Gebruik de volgende stappen voor het installeren van StorSimple Snapshot Manager.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager installeren
1. Download de StorSimple Snapshot Manager-software (Ga naar [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) in het Microsoft Download Center) en sla het bestand lokaal op de host.
2. In Verkenner met de rechtermuisknop op de gecomprimeerde map en klik vervolgens op **alle uitpakken**.
3. In de **gecomprimeerde extraheren (Zipped) mappen** venster in de **Selecteer een doel en uitpakken van bestanden** vak, typ of blader naar het pad waar u zou nu naar het bestand moet worden geëxtraheerd.
   
    > [!IMPORTANT]
    > U moet StorSimple Snapshot Manager installeren op station C:.
    
4. Selecteer de **weergeven wanneer de volledige-bestanden hebt uitgepakt** selectievakje en klik vervolgens op **extraheren**.
   
    ![Dialoogvenster bestanden uitpakken](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Wanneer de extractie is voltooid, wordt de doelmap wordt geopend. Dubbelklik op het pictogram van de toepassing setup die wordt weergegeven in de doelmap.
6. Wanneer de **Setup Successful** bericht wordt weergegeven, klikt u op **sluiten**. U ziet het pictogram StorSimple Snapshot Manager op uw bureaublad.
   
    ![pictogram op het bureaublad](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Stap 2: Verbinding maken met StorSimple Snapshot Manager op een apparaat
Gebruik de volgende stappen StorSimple Snapshot Manager verbinding met een StorSimple-apparaat.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Verbinding maken met StorSimple Snapshot Manager een apparaat
1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager-venster wordt weergegeven. Het venster bevat een **bereik** deelvenster een **resultaten** deelvenster en een **acties** deelvenster. 
   
    ![StorSimple Snapshot Manager-gebruikersinterface](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * De **bereik** deelvenster (linker deelvenster) bevat een lijst met knooppunten in een boomstructuur gerangschikt. U kunt sommige knooppunten om te selecteren van een weergave of specifieke gegevens met betrekking tot dat knooppunt uitvouwen. Klik op het pijlpictogram wilt uitvouwen of samenvouwen van een knooppunt. Met de rechtermuisknop op een item in de **bereik** deelvenster voor een overzicht van de beschikbare acties voor dat het item.
   * De **resultaten** deelvenster (middelste deelvenster) bevat gedetailleerde statusinformatie over het knooppunt, weergave of gegevens die u hebt geselecteerd in de **bereik** deelvenster.
   * De **acties** deelvenster vindt u de bewerkingen die u op het knooppunt, weergeven of gegevens die u hebt geselecteerd uitvoeren kunt in de **bereik** deelvenster.
     
     Zie voor een volledige beschrijving van de gebruikersinterface van het StorSimple Snapshot Manager [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).
2. In de **bereik** deelvenster met de rechtermuisknop op de **apparaten** knooppunt en klik vervolgens op **configureren van een apparaat**. De **configureren van een apparaat** dialoogvenster wordt weergegeven.
   
    ![Een apparaat configureren](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. In de **apparaat** keuzelijst, selecteert u het IP-adres van de Microsoft Azure StorSimple-apparaat of het virtuele apparaat. In de **wachtwoord** tekst typt u het wachtwoord voor StorSimple Snapshot Manager die u hebt gemaakt voor het apparaat in de Azure-portal. Klik op **OK**.
4. StorSimple Snapshot Manager zoekt naar het apparaat dat u hebt geïdentificeerd. Als het apparaat beschikbaar is, voegt StorSimple Snapshot Manager een verbinding. U kunt [Controleer of de verbinding met het apparaat](#to-verify-the-connection) om te bevestigen dat de verbinding is toegevoegd.
   
    Als het apparaat voor een of andere reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. Klik op **OK** het foutbericht te sluiten en klik vervolgens op **annuleren** sluiten de **configureren van een apparaat** in het dialoogvenster.
5. Bij het verbinden met een apparaat, importeert StorSimple Snapshot Manager elke groep volume is geconfigureerd voor het apparaat, op voorwaarde dat de groep van volume back-ups is gekoppeld. Volume-groepen die u geen gekoppelde back-ups hebt zijn niet geïmporteerd. Daarnaast worden back-upbeleid die zijn gemaakt voor een groep volume niet geïmporteerd. Overzicht van de geïmporteerde groepen met de rechtermuisknop op de bovenste **Volume groepen** knooppunt in de **bereik** deelvenster en klik op **schakelen geïmporteerde groepen**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Stap 3: Controleer of de verbinding met het apparaat
Gebruik de volgende stappen uit om te controleren of StorSimple Snapshot Manager is verbonden met het StorSimple-apparaat.

#### <a name="to-verify-the-connection"></a>De verbinding controleren
1. In de **bereik** deelvenster, klikt u op de **apparaten** knooppunt.
   
    ![De apparaatstatus StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. Controleer de **resultaten** deelvenster: 
   
   * Als een groene indicator wordt weergegeven op het pictogram van het apparaat en **beschikbaar** wordt weergegeven in de **Status** kolom en vervolgens het apparaat is verbonden. 
   * Als een rode indicator wordt weergegeven op het pictogram van het apparaat en niet beschikbaar in de **Status** kolom en vervolgens het apparaat niet is verbonden. 
   * Als **Refreshing** wordt weergegeven in de **Status** kolom en vervolgens StorSimple Snapshot Manager haalt volume groepen en gekoppelde back-ups voor een verbonden apparaat.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Bijwerken of opnieuw installeren van StorSimple Snapshot Manager
U moet StorSimple Snapshot Manager volledig verwijderen voordat u een upgrade of de software opnieuw te installeren. 

Voordat u opnieuw StorSimple Snapshot Manager installeert, back-up van de bestaande StorSimple Snapshot Manager-database op de hostcomputer. Dit wordt de back-beleid en de configuratie-informatie opgeslagen zodat u deze gegevens gemakkelijk vanuit back-up herstellen kunt.

Volg deze stappen als u een upgrade of StorSimple Snapshot Manager opnieuw te installeren:

* Stap 1: StorSimple Snapshot Manager verwijderen 
* Stap 2: Back-up van de StorSimple Snapshot Manager-database 
* Stap 3: Installeer StorSimple Snapshot Manager en de database herstellen 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Stap 1: StorSimple Snapshot Manager verwijderen
Gebruik de volgende stappen uit om te verwijderen van StorSimple Snapshot Manager.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager verwijderen
1. Open op de hostcomputer het **Configuratiescherm**, klikt u op **programma's**, en klik vervolgens op **programma's en onderdelen**.
2. Klik in het linkerdeelvenster op **een programma verwijderen of wijzigen**.
3. Met de rechtermuisknop op **StorSimple Snapshot Manager**, en klik vervolgens op **verwijderen**.
4. Hiermee start u het StorSimple Snapshot Manager-Setup-programma. Klik op **Setup wijzigen**, en klik vervolgens op **verwijderen**.
   
   > [!NOTE]
   > Als er een MMC-processen op de achtergrond uitgevoerd, zoals StorSimple Snapshot Manager of Schijfbeheer, mislukt de verwijdering wordt en ontvangt u een bericht te sluiten van alle exemplaren van MMC voordat u probeert te verwijderen van het programma. Selecteer **automatisch toepassingen te sluiten en opnieuw openen nadat setup voltooid is**, en klik vervolgens op **OK**.
   > 
   > 
5. Wanneer het verwijderingsproces is voltooid, een **Setup Successful** bericht wordt weergegeven. Klik op **Sluiten**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Stap 2: Back-up van de StorSimple Snapshot Manager-database
Gebruik de volgende stappen uit om te maken en sla een kopie van de StorSimple Snapshot Manager-database.

#### <a name="to-back-up-the-database"></a>Back-up van de database
1. De Microsoft StorSimple Management-Service stoppen:
   
   1. Start Serverbeheer.
   2. Op het Dashboard Serverbeheer op de **extra** selecteert u **Services**.
   3. Op de **Services** pagina **Microsoft StorSimple Management Service**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Stop de service**.
      
        ![Stop de service Manager voor StorSimple-apparaat](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 
   
   > [!NOTE]
   > ProgramData is een verborgen map.
  
3. Het catalogus XML-bestand niet vinden, Kopieer het bestand en de kopie opslaan op een veilige locatie of in de cloud.
   
    ![StorSimple-bestand voor back-upcatalogus](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. De Microsoft StorSimple Management-Service opnieuw starten: 
   
   1. Op het Dashboard Serverbeheer op de **extra** selecteert u **Services**.
   2. Op de **Services** pagina de **StorSimple Management Servic**e.
   3. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Start de service opnieuw**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Stap 3: Installeer StorSimple Snapshot Manager en de database herstellen
Als u wilt installeren StorSimple Snapshot Manager, volg de stappen in [installeren van een nieuwe StorSimple Snapshot Manager](#install-a-new-storsimple-snapshot-manager). Vervolgens gebruikt u de volgende procedure om de StorSimple Snapshot Manager-database te herstellen.

#### <a name="to-restore-the-database"></a>De database herstellen
1. De Microsoft StorSimple Management-Service stoppen:
   
   1. Start Serverbeheer.
   2. Op het Dashboard Serverbeheer op de **extra** selecteert u **Services**.
   3. Op de **Services** pagina **Microsoft StorSimple Management Service**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Stop de service**.
2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog.
   
   > [!NOTE]
   > ProgramData is een verborgen map.
   > 
   > 
3. Verwijder het bestand van de XML-catalogus en vervang deze door de versie die u eerder hebt opgeslagen.
4. De Microsoft StorSimple Management-Service opnieuw starten: 
   
   1. Op het Dashboard Serverbeheer op de **extra** selecteert u **Services**.
   2. Op de **Services** pagina **Microsoft StorSimple Management Service**.
   3. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Start de service opnieuw**.

## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over het StorSimple Snapshot Manager, gaat u naar [wat is er StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md).
* Voor meer informatie over de gebruikersinterface van het StorSimple Snapshot Manager, gaat u naar [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).
* Voor meer informatie over het gebruik van StorSimple Snapshot Manager, gaat u naar [gebruik StorSimple Snapshot Manager voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).

