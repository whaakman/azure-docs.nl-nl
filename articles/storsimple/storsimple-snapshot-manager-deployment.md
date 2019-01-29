---
title: StorSimple Snapshot Manager implementeren | Microsoft Docs
description: Informatie over het downloaden en installeren van de StorSimple Snapshot Manager een MMC-module voor het beheer van StorSimple data protection en back-up-functies.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: ee17e4b69d1e6c9de465e4241ee2237361e320b7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168002"
---
# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>De StorSimple Snapshot Manager MMC-module implementeren

## <a name="overview"></a>Overzicht
StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module waarmee de beveiliging van gegevens en back-upbeheer in een omgeving met Microsoft Azure StorSimple vereenvoudigd. Met StorSimple Snapshot Manager kunt u Microsoft Azure StorSimple on-premises beheren en cloud storage alsof het een volledig geïntegreerde opslagsysteem, waardoor de back-up en herstel processen te vereenvoudigen en kosten te verlagen. 

Deze zelfstudie beschrijft configuratievereisten, evenals de procedures voor het installeren, verwijderen en bijwerken van de StorSimple Snapshot Manager.

> [!NOTE]
> * U kunt geen StorSimple Snapshot Manager gebruiken voor het beheren van Microsoft Azure StorSimple-matrices (ook wel bekend als StorSimple on-premises virtuele apparaten).
> * Als u van plan bent voor het StorSimple Update 2 installeren op uw StorSimple-apparaat, moet u de meest recente versie van StorSimple Snapshot Manager download en installeer deze **voordat u het StorSimple Update 2 installeren**. De meest recente versie van StorSimple Snapshot Manager achterwaarts compatibel is en werkt met alle versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple Snapshot Manager gebruikt, moet u bij te werken (u hoeft niet de vorige versie verwijderen voordat u de nieuwe versie installeert).


## <a name="storsimple-snapshot-manager-installation"></a>StorSimple Snapshot Manager-installatie
StorSimple Snapshot Manager kan worden geïnstalleerd op computers waarop de Windows Server 2008 R2 SP1, Windows Server 2012 of Windows Server 2012 R2-besturingssysteem. Op servers met Windows 2008 R2, moet u ook Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren.

Voordat het installeren of upgraden van de StorSimple Snapshot Manager-module voor de Microsoft Management Console (MMC), zorg ervoor dat de Microsoft Azure StorSimple-apparaat en de host-server correct zijn geconfigureerd.

## <a name="configure-prerequisites"></a>Vereisten configureren
De volgende stappen bevatten een overzicht op hoog niveau van configuratietaken die u uitvoeren moet voordat u StorSimple Snapshot Manager installeert. Voor volledige Microsoft Azure StorSimple-configuratie en installatie-informatie, waaronder systeemvereisten en stapsgewijze instructies, Zie [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Voordat u begint, controleert u de [Configuratiecontrolelijst voor implementatie](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) en en [implementatievereisten](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>Voordat u StorSimple Snapshot Manager installeren
1. Uitpakken, koppelen en verbinding maken met het Microsoft Azure StorSimple-apparaat, zoals beschreven in [uw StorSimple 8100-apparaat installeert](storsimple-8100-hardware-installation.md) of [uw StorSimple 8600-apparaat installeert](storsimple-8600-hardware-installation.md).
2. Zorg ervoor dat de hostcomputer wordt uitgevoerd een van de volgende besturingssystemen:
   
   * Windows Server 2008 R2 (op servers met Windows 2008 R2, moet u ook installeren Windows Server 2008 SP1 en Windows Management Framework 3.0)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     Voor een StorSimple-apparaat moet de host een Microsoft Azure-Machine.
3. Zorg ervoor dat u voldoet aan de vereisten voor de configuratie van Microsoft Azure StorSimple. Voor meer informatie gaat u naar [implementatievereisten](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites).
4. Het apparaat verbinding met de host en voer de eerste configuratie. Voor meer informatie gaat u naar [implementatiestappen](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps).
5. Maak volumes op het apparaat, deze toewijzen aan de host en controleer of de host kunt koppelen en ze gebruiken. StorSimple Snapshot Manager ondersteunt de volgende typen volumes:
   
   * Eenvoudige volumes
   * Eenvoudige volumes
   * Dynamische volumes
   * Gespiegelde dynamische volumes (RAID-1)
   * Gedeelde clustervolumes
     
     Voor informatie over het maken van volumes op het StorSimple-apparaat of de virtuele StorSimple-apparaat, gaat u naar [stap 6: Maak een volume](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume)in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Een nieuwe StorSimple Snapshot Manager installeren
Voordat u StorSimple Snapshot Manager installeert, zorg ervoor dat de volumes die u hebt gemaakt op het StorSimple-apparaat of de virtuele StorSimple-apparaat zijn gekoppeld, geïnitialiseerd en wordt beschreven in de opmaak [vereisten configureren](#configure-prerequisites).

> [!IMPORTANT]
> * Voor een StorSimple-apparaat moet de host een Microsoft Azure-Machine. 
> * De host moet worden uitgevoerd, Windows 2008 R2, Windows Server 2012 of Windows Server 2012 R2. Als uw server wordt uitgevoerd Windows Server 2008 R2, moet u ook de Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren.
> * Voordat u het apparaat aan StorSimple Snapshot Manager kunt verbinden, moet u een iSCSI-verbinding van de host om het StorSimple-apparaat te configureren.

Volg deze stappen om een nieuwe installatie van de StorSimple Snapshot Manager te voltooien. Als u een upgrade installeren wilt, gaat u naar [bijwerken of opnieuw installeren van StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

* Stap 1: StorSimple Snapshot Manager installeren 
* Stap 2: Verbinding maken met StorSimple Snapshot Manager op een apparaat 
* Stap 3: Controleer of de verbinding met het apparaat 

### <a name="step-1-install-storsimple-snapshot-manager"></a>Stap 1: StorSimple Snapshot Manager installeren
Gebruik de volgende stappen voor het installeren van StorSimple Snapshot Manager.

#### <a name="to-install-storsimple-snapshot-manager"></a>Voor het installeren van StorSimple Snapshot Manager
1. Download de StorSimple Snapshot Manager-software (Ga naar [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) in het Microsoft Download Center) en slaat dit lokaal op de host.
2. In Windows Verkenner met de rechtermuisknop op de gecomprimeerde map en klik vervolgens op **alle uitpakken**.
3. In de **gecomprimeerde uitpakken (Zipped) mappen** venster in de **een doel selecteren en uitpakken van bestanden** vak, typ of blader naar het pad waar u graag naar het bestand moet worden geëxtraheerd.
   
    > [!IMPORTANT]
    > U moet StorSimple Snapshot Manager installeren op station C:.
    
4. Selecteer de **Show uitgepakte bestanden als u klaar bent** selectievakje en klik vervolgens op **extraheren**.
   
    ![Dialoogvenster bestanden uitpakken](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Wanneer de extractie is voltooid, wordt de doelmap wordt geopend. Dubbelklik op het pictogram van de toepassing setup die wordt weergegeven in de doelmap.
6. Wanneer de **Setup geslaagde** bericht wordt weergegeven, klikt u op **sluiten**. U ziet het pictogram StorSimple Snapshot Manager op uw bureaublad.
   
    ![pictogram op het bureaublad](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Stap 2: Verbinding maken met StorSimple Snapshot Manager op een apparaat
Gebruik de volgende stappen uit StorSimple Snapshot Manager verbinden met een StorSimple-apparaat.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Verbinding maken met StorSimple Snapshot Manager een apparaat
1. Klik op het pictogram StorSimple Snapshot Manager op uw bureaublad. Het StorSimple Snapshot Manager-venster wordt weergegeven. Het venster bevat een **bereik** in het deelvenster een **resultaten** in het deelvenster en een **acties** deelvenster. 
   
    ![Gebruikersinterface van de StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * De **bereik** deelvenster (in het linkerdeelvenster) bevat een lijst met knooppunten in een boomstructuur gerangschikt. U kunt sommige knooppunten om te selecteren van een weergave of een specifieke gegevens die betrekking hebben op dat knooppunt uitvouwen. Klik op het pijlpictogram wilt uitvouwen of samenvouwen van een knooppunt. Met de rechtermuisknop op een item in de **bereik** deelvenster voor een overzicht van beschikbare acties voor het item.
   * De **resultaten** deelvenster (het middelste deelvenster) bevat gedetailleerde statusinformatie over het knooppunt, weergave of gegevens die u hebt geselecteerd in de **bereik** deelvenster.
   * De **acties** deelvenster geeft een lijst van de bewerkingen die u op het knooppunt, weergave of gegevens die u hebt geselecteerd uitvoeren kunt in de **bereik** deelvenster.
     
     Zie voor een volledige beschrijving van de gebruikersinterface van de StorSimple Snapshot Manager [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).
2. In de **bereik** in het deelvenster met de rechtermuisknop op de **apparaten** knooppunt en klik vervolgens op **configureren van een apparaat**. De **configureren van een apparaat** in het dialoogvenster wordt weergegeven.
   
    ![Een apparaat configureren](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. In de **apparaat** in een keuzelijst, selecteert u het IP-adres van de Microsoft Azure StorSimple-apparaat of het virtuele apparaat. In de **wachtwoord** tekst typt u het StorSimple Snapshot Manager-wachtwoord die u hebt gemaakt voor het apparaat in de Azure-portal. Klik op **OK**.
4. StorSimple Snapshot Manager zoekt naar het apparaat dat u hebt geïdentificeerd. Als het apparaat beschikbaar is, voegt met StorSimple Snapshot Manager een verbinding. U kunt [controleren of de verbinding met het apparaat](#to-verify-the-connection) om te bevestigen dat de verbinding is toegevoegd.
   
    Als het apparaat voor een bepaalde reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. Klik op **OK** om te sluiten van het foutbericht en klik vervolgens op **annuleren** sluiten de **configureren van een apparaat** in het dialoogvenster.
5. Bij het verbinden met een apparaat, importeert StorSimple Snapshot Manager elk volumegroep die voor dat apparaat is geconfigureerd dat de volumegroep back-ups is gekoppeld. Volumegroepen waarvoor geen gekoppelde back-ups worden niet geïmporteerd. Daarnaast worden back-upbeleid die zijn gemaakt voor een volumegroep niet geïmporteerd. Als u wilt zien van de geïmporteerde groepen, met de rechtermuisknop op de bovenste **Volumegroepen** knooppunt in de **bereik** deelvenster en klik op **in-/ uitschakelen geïmporteerde groepen**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Stap 3: Controleer of de verbinding met het apparaat
Gebruik de volgende stappen uit om te controleren of StorSimple Snapshot Manager is verbonden met het StorSimple-apparaat.

#### <a name="to-verify-the-connection"></a>De verbinding controleren
1. In de **bereik** deelvenster, klikt u op de **apparaten** knooppunt.
   
    ![Apparaatstatus StorSimple Snapshot Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. Controleer de **resultaten** deelvenster: 
   
   * Als er een groene indicator wordt weergegeven op het pictogram voor het apparaat en **beschikbaar** wordt weergegeven in de **Status** kolom en vervolgens het apparaat is verbonden. 
   * Als een rode indicator wordt weergegeven op het pictogram voor het apparaat en niet beschikbaar in de **Status** kolom en vervolgens het apparaat niet is verbonden. 
   * Als **vernieuwen** wordt weergegeven in de **Status** kolom en vervolgens StorSimple Snapshot Manager is bezig met ophalen van volumegroepen en gekoppelde back-ups voor een verbonden apparaat.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Bijwerken of opnieuw installeren van StorSimple Snapshot Manager
Voordat u de software opnieuw te installeren of bijwerkt, moet u StorSimple Snapshot Manager volledig verwijderen. 

Voordat u opnieuw installeert StorSimple Snapshot Manager, back-up van de bestaande StorSimple Snapshot Manager-database op de hostcomputer. Dit bespaart de back-beleid en de configuratie-informatie, zodat u deze gegevens eenvoudig vanuit back-up herstellen kunt.

Volg deze stappen als u een upgrade of StorSimple Snapshot Manager opnieuw te installeren:

* Stap 1: StorSimple Snapshot Manager verwijderen 
* Stap 2: Back-up van de StorSimple Snapshot Manager-database 
* Stap 3: StorSimple Snapshot Manager opnieuw en herstel de database 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Stap 1: StorSimple Snapshot Manager verwijderen
Gebruik de volgende stappen uit om te verwijderen van StorSimple Snapshot Manager.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager verwijderen
1. Open op de computer, de **Configuratiescherm**, klikt u op **programma's**, en klik vervolgens op **programma's en onderdelen**.
2. Klik in het linkerdeelvenster op **een programma verwijderen of wijzigen**.
3. Met de rechtermuisknop op **StorSimple Snapshot Manager**, en klik vervolgens op **verwijderen**.
4. Hiermee start u het StorSimple Snapshot Manager-Setup-programma. Klik op **Setup wijzigen**, en klik vervolgens op **verwijderen**.
   
   > [!NOTE]
   > Als er een MMC-processen worden uitgevoerd op de achtergrond, zoals StorSimple Snapshot Manager of Schijfbeheer, mislukt de verwijdering wordt en u ontvangt een bericht naar alle exemplaren van MMC te sluiten voordat u probeert te verwijderen van het programma. Selecteer **automatisch toepassingen sluiten en opnieuw openen nadat setup voltooid is opnieuw proberen**, en klik vervolgens op **OK**.
   > 
   > 
5. Wanneer de verwijdering is voltooid, een **Setup geslaagde** bericht wordt weergegeven. Klik op **Sluiten**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Stap 2: Back-up van de StorSimple Snapshot Manager-database
Gebruik de volgende stappen uit om te maken en opslaan van een kopie van de StorSimple Snapshot Manager-database.

#### <a name="to-back-up-the-database"></a>Back-up van de database
1. Stop de Microsoft StorSimple Management-Service:
   
   1. Start Serverbeheer.
   2. Op het Dashboard Serverbeheer op de **extra** in het menu **Services**.
   3. Op de **Services** weergeeft, schakelt **Microsoft StorSimple-beheerservice**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Stop de service**.
      
        ![De StorSimple Device Manager-service stoppen](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 
   
   > [!NOTE]
   > ProgramData is een verborgen map.
  
3. Het XML-bestand van catalogus zoeken, Kopieer het bestand en de kopie opslaan op een veilige locatie of in de cloud.
   
    ![StorSimple-back-catalogusbestand](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. De Microsoft StorSimple Management-Service opnieuw starten: 
   
   1. Op het Dashboard Serverbeheer op de **extra** in het menu **Services**.
   2. Op de **Services** weergeeft, schakelt de **Microsoft StorSimple-beheerservice**.
   3. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Start de service opnieuw**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Stap 3: StorSimple Snapshot Manager opnieuw en herstel de database
Als u opnieuw StorSimple Snapshot Manager, wilt u de stappen in [installeren van een nieuwe StorSimple Snapshot Manager](#install-a-new-storsimple-snapshot-manager). Gebruik vervolgens de volgende procedure om het StorSimple Snapshot Manager-database te herstellen.

#### <a name="to-restore-the-database"></a>De database herstellen
1. Stop de Microsoft StorSimple Management-Service:
   
   1. Start Serverbeheer.
   2. Op het Dashboard Serverbeheer op de **extra** in het menu **Services**.
   3. Op de **Services** weergeeft, schakelt **Microsoft StorSimple-beheerservice**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Stop de service**.
2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog.
   
   > [!NOTE]
   > ProgramData is een verborgen map.
   > 
   > 
3. Verwijderen van de catalogus XML-bestand en vervang deze door de versie die u eerder hebt opgeslagen.
4. De Microsoft StorSimple Management-Service opnieuw starten: 
   
   1. Op het Dashboard Serverbeheer op de **extra** in het menu **Services**.
   2. Op de **Services** weergeeft, schakelt **Microsoft StorSimple-beheerservice**.
   3. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Start de service opnieuw**.

## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over StorSimple Snapshot Manager, gaat u naar [wat is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md).
* Voor meer informatie over de StorSimple Snapshot Manager-gebruikersinterface, gaat u naar [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).
* Voor meer informatie over het gebruik van StorSimple Snapshot Manager, gaat u naar [gebruikt StorSimple Snapshot Manager voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).

