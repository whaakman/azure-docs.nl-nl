---
title: Apparaten beheren met StorSimple Snapshot Manager | Microsoft Docs
description: Hierin wordt beschreven hoe u de StorSimple Snapshot Manager MMC-module om te verbinden en beheren van StorSimple-apparaten.
services: storsimple
documentationcenter: ''
author: SharS
manager: timlt
editor: ''
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 51632b8b68640814fc113a94925b6d6deaca4c5c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64682489"
---
# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>StorSimple Snapshot Manager gebruiken om te verbinden en beheren van StorSimple-apparaten
## <a name="overview"></a>Overzicht
U kunt knooppunten gebruiken in de StorSimple Snapshot Manager **bereik** deelvenster om te controleren of de geïmporteerde gegevens van de StorSimple-apparaat en vernieuwen van verbonden apparaten. Bovendien, als u klikt op de **apparaten** knooppunt, ziet u een lijst van verbonden apparaten en de bijbehorende statusinformatie in de **resultaten** deelvenster.

![Verbonden apparaten](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Afbeelding 1: Verbonden apparaat StorSimple Snapshot Manager** 

Afhankelijk van uw **weergave** selectie heeft de **resultaten** deelvenster ziet u de volgende informatie over elk apparaat. (Voor meer informatie over het configureren van een weergave, gaat u naar [menu Beeld](storsimple-use-snapshot-manager.md#view-menu).

| Resultatenkolom | Description |
|:--- |:--- |
| Name |De naam van het apparaat zoals geconfigureerd in de klassieke Azure portal |
| Model |Het modelnummer van het apparaat |
| Version |De versie van de software is geïnstalleerd op het apparaat |
| Status |Of het apparaat beschikbaar is |
| Laatste synchronisatie |Datum en tijd wanneer het apparaat het laatst is gesynchroniseerd |
| Serienummer |Het serienummer van het apparaat |

Als u met de rechtermuisknop op de **apparaten** knooppunt in de **bereik** deelvenster kunt u kiezen uit de volgende acties:

* Toevoegen of vervangen van een apparaat
* Verbinding maken met een apparaat en controleer of invoer
* Vernieuwen van verbonden apparaten

Als u klikt op de **apparaten** knooppunt en klik vervolgens met de rechtermuisknop op een apparaat een naam de **resultaten** deelvenster kunt u kiezen uit de volgende acties:

* Een apparaat verifiëren
* Apparaatdetails weergeven
* Een apparaat vernieuwen
* De apparaatconfiguratie van een verwijderen
* Het apparaatwachtwoord van een wijzigen

> [!NOTE]
> Al deze acties zijn ook beschikbaar in de **acties** deelvenster.


In deze zelfstudie wordt uitgelegd hoe u StorSimple Snapshot Manager gebruiken om te verbinden en beheren van apparaten en de volgende taken uitvoeren:

* Toevoegen of vervangen van een apparaat
* Verbinding maken met een apparaat en controleer of invoer
* Vernieuwen van verbonden apparaten
* Een apparaat verifiëren
* Apparaatdetails weergeven
* Een afzonderlijk apparaat vernieuwen
* De apparaatconfiguratie van een verwijderen
* Een apparaatwachtwoord verlopen wijzigen
* Vervangen van een mislukte apparaat

> [!NOTE]
> Voor algemene informatie over het gebruik van de StorSimple Snapshot Manager-interface, gaat u naar [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Toevoegen of vervangen van een apparaat
Gebruik de volgende procedure toe te voegen of te vervangen door een StorSimple-apparaat.

#### <a name="to-add-or-replace-a-device"></a>Toe te voegen of te vervangen door een apparaat
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** in het deelvenster met de rechtermuisknop op de **apparaten** knooppunt en klik vervolgens op **configureren van een apparaat**. De **configureren van een apparaat** in het dialoogvenster wordt weergegeven.
   
    ![Een StorSimple-apparaat configureren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. In de **apparaat** vervolgkeuzelijst vak, selecteert u het IP-adres van het apparaat of het virtuele apparaat. 
4. In de **wachtwoord** tekst typt u het StorSimple Snapshot Manager-wachtwoord die u hebt gemaakt voor het apparaat in de klassieke Azure portal. Klik op **OK**. StorSimple Snapshot Manager zoekt naar het apparaat dat u hebt geïdentificeerd. 
   
   * Als het apparaat beschikbaar is, voegt met StorSimple Snapshot Manager een verbinding.
   * Als het apparaat voor een bepaalde reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. Klik op **OK** om te sluiten van het foutbericht en klik vervolgens op **annuleren** sluiten de **configureren van een apparaat** in het dialoogvenster.

## <a name="connect-a-device-and-verify-imports"></a>Verbinding maken met een apparaat en controleer of invoer
Gebruik de volgende procedure voor het verbinding maken met een StorSimple-apparaat en te controleren dat elke bestaande volumegroepen die zijn gekoppeld back-ups worden geïmporteerd.

#### <a name="to-connect-a-device-and-verify-imports"></a>Verbinding maken met een apparaat en controleer of de invoer
1. Als u wilt verbinding maken met een apparaat voor StorSimple Snapshot Manager, volg de instructies in toevoegen of vervangen van een apparaat. Bij het verbinden met een apparaat, reageert de StorSimple Snapshot Manager als volgt:
   
   * Als het apparaat voor een bepaalde reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. 
   
   * Als het apparaat beschikbaar is, voegt met StorSimple Snapshot Manager een verbinding. Wanneer u het apparaat selecteert, wordt deze weergegeven de **resultaten** deelvenster en het statusveld geeft aan dat het apparaat is **beschikbaar**. StorSimple Snapshot Manager importeert volumegroepen geconfigureerd voor het apparaat, mits de volumegroepen aan back-ups zijn gekoppeld. Back-upbeleid zijn niet geïmporteerd. Volumegroepen waarvoor geen gekoppelde back-ups worden niet geïmporteerd.
2. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
3. Met de rechtermuisknop op het bovenste knooppunt in de **bereik** deelvenster en klik vervolgens op **in-/ uitschakelen invoer weergeven**.
   
    ![Selecteer in-/ uitschakelen invoer weergeven](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. De **in-/ uitschakelen invoer weergeven** in het dialoogvenster wordt weergegeven, met de status van de geïmporteerde volumegroepen en back-ups. Klik op **OK**.

Nadat de volumegroepen en de back-ups geïmporteerd zijn, kunt u StorSimple Snapshot Manager gebruiken om te beheren, net zoals u zou volumegroepen en back-ups die u hebt gemaakt en geconfigureerd met StorSimple Snapshot Manager beheren. 

## <a name="refresh-connected-devices"></a>Vernieuwen van verbonden apparaten
Gebruik de volgende procedure om te synchroniseren van de verbonden StorSimple-apparaten met StorSimple Snapshot Manager.

#### <a name="to-refresh-connected-devices"></a>Vernieuwen van verbonden apparaten
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** in het deelvenster met de rechtermuisknop op **apparaten**, en klik vervolgens op **apparaten vernieuwen**. Dit synchroniseert de aangesloten apparaten met StorSimple Snapshot Manager zodat u kunt de volumegroepen en back-ups, met inbegrip van recente aanvullingen weergeven. 
   
    ![Vernieuwen van de StorSimple-apparaten](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

De **apparaten vernieuwen** actie haalt u alle nieuwe volumegroepen en alle gekoppelde back-ups van verbonden apparaten. In tegenstelling tot de **volumes opnieuw scannen** actie beschikbaar voor de **Volumes** knooppunt **apparaten vernieuwen** de reservekopie van het register niet hersteld.

## <a name="authenticate-a-device"></a>Een apparaat verifiëren
Gebruik de volgende procedure om een StorSimple-apparaat met StorSimple Snapshot Manager te verifiëren.

#### <a name="to-authenticate-a-device"></a>Om een apparaat te verifiëren
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, klikt u op **apparaten**.
3. In de **resultaten** in het deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **verifiëren**.
4. De **verifiëren** in het dialoogvenster wordt weergegeven. Typ het wachtwoord van het apparaat en klik vervolgens op **OK**.
   
    ![In het dialoogvenster verifiëren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Apparaatdetails weergeven
Gebruik de volgende procedure om te bekijken van de details van een StorSimple-apparaat en, indien nodig, opnieuw synchroniseren van het apparaat met StorSimple Snapshot Manager.

#### <a name="to-view-and-resynchronize-device-details"></a>Voor het weergeven en opnieuw synchroniseren van Apparaatdetails
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, klikt u op **apparaten**.
3. In de **resultaten** in het deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **Details**.

4. de **Apparaatdetails** in het dialoogvenster wordt weergegeven. In dit vak geeft de naam, model, versie, serienummer, status, iSCSI-doel Qualified Name (IQN), en laatste synchronisatiedatum en tijd.

* Klik op **Resync** om te synchroniseren van het apparaat.
* Klik op **OK** of **annuleren** om het dialoogvenster te sluiten.
  
  ![Apparaatdetails](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Een afzonderlijk apparaat vernieuwen
Gebruik de volgende procedure om opnieuw te synchroniseren van een afzonderlijke StorSimple-apparaat met StorSimple Snapshot Manager.

#### <a name="to-refresh-a-device"></a>Een apparaat vernieuwen
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad. 
2. In de **bereik** deelvenster, klikt u op **apparaten**. 
3. In de **resultaten** in het deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **apparaat vernieuwen**. Dit synchroniseert het apparaat met StorSimple Snapshot Manager.

## <a name="delete-a-device-configuration"></a>De apparaatconfiguratie van een verwijderen
Gebruik de volgende procedure om te verwijderen van een afzonderlijke configuratie van de StorSimple-apparaat uit StorSimple Snapshot Manager.

#### <a name="to-delete-a-device-configuration"></a>De configuratie van een apparaat verwijderen
1. Klik op het pictogram voor het starten van StorSimple Snapshot Manager op het bureaublad.
2. In de **bereik** deelvenster, klikt u op **apparaten**. 
3. In de **resultaten** in het deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **verwijderen**. 
4. Het volgende bericht wordt weergegeven. Klik op **Ja** wilt verwijderen van de configuratie of klik op **Nee** om de verwijdering te annuleren.
   
    ![Apparaatconfiguratie verwijderen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Een apparaatwachtwoord verlopen wijzigen
U moet een wachtwoord voor het verifiëren van een StorSimple-apparaat met StorSimple Snapshot Manager. U kunt dit wachtwoord configureren wanneer u de Windows PowerShell-interface gebruiken voor het instellen van het apparaat. Het wachtwoord kan echter verloopt. Als dit gebeurt, kunt u de klassieke Azure portal gebruiken om het wachtwoord te wijzigen. Vervolgens worden gebruikt, omdat het apparaat is StorSimple Snapshot Manager geconfigureerd voordat het wachtwoord is verlopen, moet u het apparaat in StorSimple Snapshot Manager opnieuw worden geverifieerd.

#### <a name="to-change-the-expired-password"></a>Wijzig het wachtwoord verlopen
1. In de klassieke Azure portal, de StorSimple Manager-service te starten.
2. Klik op **apparaten** > **configureren** voor het apparaat.
3. Schuif omlaag naar het gedeelte van de StorSimple Snapshot Manager. Voer een wachtwoord dat is 14-15 tekens. Zorg ervoor dat het wachtwoord een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens bevat.
4. Voer het wachtwoord ter bevestiging.
5. Klik op **Opslaan** onder aan de pagina.

#### <a name="to-re-authenticate-the-device"></a>Het apparaat opnieuw worden geverifieerd
1. StorSimple Snapshot Manager start.
2. In de **bereik** deelvenster, klikt u op **apparaten**. Een lijst met geconfigureerde apparaten wordt weergegeven in de **resultaten** deelvenster.
3. Selecteer het apparaat, klik met de rechtermuisknop en klik vervolgens op **verifiëren**.
4. In de **verifiëren** venster, voer het nieuwe wachtwoord.
5. Selecteer het apparaat, klik met de rechtermuisknop en selecteer **vernieuwen apparaat**. Dit synchroniseert het apparaat met StorSimple Snapshot Manager.

## <a name="replace-a-failed-device"></a>Vervangen van een mislukte apparaat
Als een StorSimple-apparaat mislukt en is vervangen door een apparaat stand-by (failover), gebruik de volgende stappen verbinding maken met het nieuwe apparaat en de bijbehorende back-ups weergeven.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Verbinding maken met een nieuw apparaat na een failover
1. Configuratie van de iSCSI-verbinding met het nieuwe apparaat. Voor instructies gaat u naar ' stap 7: Koppelen, initialiseren en formatteren van een volume"in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

> [!NOTE]
> Als het nieuwe StorSimple-apparaat hetzelfde IP-adres als de oude heeft heeft, is het mogelijk dat u de oude configuratie verbinding kunnen maken.


1. Stop de Microsoft StorSimple Management-Service:
   
   1. Start Serverbeheer.
   2. Op het Dashboard Serverbeheer op de **extra** in het menu **Services**.
   3. Op de **Services** venster de **Microsoft StorSimple-beheerservice**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Stop de service**.
2. De configuratie-informatie met betrekking tot het oude apparaat verwijderen:
   
   1. Blader in Windows Verkenner naar C:\ProgramData\Microsoft\StorSimple\BACatalog.
   2. Verwijder de bestanden in de map BACatalog.
3. De Microsoft StorSimple Management-Service opnieuw starten:
   
   1. Op het Dashboard Serverbeheer op de **extra** in het menu **Services**.
   2. Op de **Services** venster de **Microsoft StorSimple-beheerservice**.
   3. In het rechterdeelvenster onder **Microsoft StorSimple-beheerservice**, klikt u op **Start de service opnieuw**.
4. StorSimple Snapshot Manager start.
5. De stappen voor het configureren van het nieuwe StorSimple-apparaat, in stap 2: Verbinding maken met een StorSimple-apparaat in [implementeren StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
6. Met de rechtermuisknop op het knooppunt op het hoogste niveau in de **bereik** deelvenster (StorSimple Snapshot Manager in het voorbeeld) en klik vervolgens op **in-/ uitschakelen invoer weergeven**. 
7. Een bericht wordt weergegeven wanneer de geïmporteerde volumegroepen en back-ups zichtbaar in StorSimple Snapshot Manager zijn. Klik op **OK**.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over het [StorSimple Snapshot Manager gebruiken voor het weergeven en beheren van volumes](storsimple-snapshot-manager-manage-volumes.md).

