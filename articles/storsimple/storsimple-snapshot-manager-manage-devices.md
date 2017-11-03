---
title: Beheer van apparaten met StorSimple Snapshot Manager | Microsoft Docs
description: Beschrijft hoe u het StorSimple Snapshot Manager MMC-module voor het verbinding maken en beheren van StorSimple-apparaten.
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f5e3186a4271e0be781f367fa75ada195c58c960
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>StorSimple Snapshot Manager gebruiken voor het verbinding maken en beheren van StorSimple-apparaten
## <a name="overview"></a>Overzicht
U kunt knooppunten in het StorSimple Snapshot Manager **bereik** deelvenster om te controleren of de geïmporteerde gegevens van de StorSimple-apparaat en vernieuw aangesloten opslagapparaten. Bovendien, als u klikt op de **apparaten** knooppunt, ziet u een lijst van verbonden apparaten en de bijbehorende statusinformatie in de **resultaten** deelvenster.

![Verbonden apparaten](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Afbeelding 1: StorSimple Snapshot Manager aangesloten apparaat** 

Afhankelijk van uw **weergave** selecties, de **resultaten** deelvenster ziet u de volgende informatie over elk apparaat. (Voor meer informatie over het configureren van een weergave, gaat u naar [menu Beeld](storsimple-use-snapshot-manager.md#view-menu).

| Kolom | Beschrijving |
|:--- |:--- |
| Naam |De naam van het apparaat zoals geconfigureerd in de klassieke Azure portal |
| Model |Het modelnummer van het apparaat |
| Versie |De versie van de software is geïnstalleerd op het apparaat |
| Status |Hiermee wordt aangegeven of het apparaat is beschikbaar |
| Laatste synchronisatie |Datum en tijd waarop het apparaat het laatst is gesynchroniseerd |
| Serienummer |Het serienummer van het apparaat |

Als u met de rechtermuisknop op de **apparaten** knooppunt in de **bereik** deelvenster die u kunt kiezen uit de volgende acties:

* Toevoegen of vervangen van een apparaat
* Een apparaat aansluit en controleer of invoer
* Verbonden apparaten vernieuwen

Als u op de **apparaten** knooppunt en klik vervolgens met de rechtermuisknop op een apparaat een naam de **resultaten** deelvenster die u kunt kiezen uit de volgende acties:

* Een apparaat verifiëren
* Details van de apparaten weergeven
* Een apparaat vernieuwen
* De apparaatconfiguratie van een verwijderen
* Wachtwoord van een apparaat wijzigen

> [!NOTE]
> Al deze acties zijn ook beschikbaar in de **acties** deelvenster.


Deze zelfstudie wordt uitgelegd hoe u van StorSimple Snapshot Manager verbinding maken en beheren van apparaten en de volgende taken uitvoeren:

* Toevoegen of vervangen van een apparaat
* Een apparaat aansluit en controleer of invoer
* Verbonden apparaten vernieuwen
* Een apparaat verifiëren
* Details van de apparaten weergeven
* Een afzonderlijk apparaat vernieuwen
* De apparaatconfiguratie van een verwijderen
* Een apparaatwachtwoord verlopen wijzigen
* Vervangen van een mislukte apparaat

> [!NOTE]
> Voor algemene informatie over het gebruik van de StorSimple Snapshot Manager-interface, gaat u naar [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Toevoegen of vervangen van een apparaat
Gebruik de volgende procedure toevoegen of vervangen van een StorSimple-apparaat.

#### <a name="to-add-or-replace-a-device"></a>Toevoegen of vervangen van een apparaat
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster met de rechtermuisknop op de **apparaten** knooppunt en klik vervolgens op **configureren van een apparaat**. De **configureren van een apparaat** dialoogvenster wordt weergegeven.
   
    ![Een StorSimple-apparaat configureren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. In de **apparaat** vervolgkeuzelijst selecteert u het IP-adres van het apparaat of een virtueel apparaat. 
4. In de **wachtwoord** tekst typt u het wachtwoord voor StorSimple Snapshot Manager die u hebt gemaakt voor het apparaat in de klassieke Azure portal. Klik op **OK**. StorSimple Snapshot Manager zoekt naar het apparaat dat u hebt geïdentificeerd. 
   
   * Als het apparaat beschikbaar is, voegt StorSimple Snapshot Manager een verbinding.
   * Als het apparaat voor een of andere reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. Klik op **OK** het foutbericht te sluiten en klik vervolgens op **annuleren** sluiten de **configureren van een apparaat** in het dialoogvenster.

## <a name="connect-a-device-and-verify-imports"></a>Een apparaat aansluit en controleer of invoer
Gebruik de volgende procedure om verbinding te een StorSimple-apparaat en controleer of alle bestaande volume groepen die zijn gekoppeld aan back-ups zijn geïmporteerd.

#### <a name="to-connect-a-device-and-verify-imports"></a>Voor een apparaat aansluit en controleren van invoer
1. Verbinding maken met een apparaat voor StorSimple Snapshot Manager, volg de instructies in toevoegen of vervangen van een apparaat. Bij het verbinden met een apparaat, reageert StorSimple Snapshot Manager als volgt:
   
   * Als het apparaat voor een of andere reden niet beschikbaar is, retourneert StorSimple Snapshot Manager een foutbericht weergegeven. 
   
   * Als het apparaat beschikbaar is, voegt StorSimple Snapshot Manager een verbinding. Wanneer u het apparaat selecteert, wordt deze weergegeven de **resultaten** deelvenster en het statusveld geeft aan dat het apparaat **beschikbaar**. StorSimple Snapshot Manager importeert volume groepen geconfigureerd voor het apparaat, mits de groepen volume hebt gekoppeld aan back-ups. Back-upbeleid zijn niet geïmporteerd. Volume-groepen die u geen gekoppelde back-ups hebt zijn niet geïmporteerd.
2. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
3. Met de rechtermuisknop op het bovenste knooppunt in de **bereik** deelvenster en klik vervolgens op **wisselknop invoer weergave**.
   
    ![Selecteer wisselknop invoer weergeven](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. De **wisselknop invoer weergave** dialoogvenster wordt weergegeven, met de status van de geïmporteerde volume groepen en de back-ups. Klik op **OK**.

Nadat de volume-groepen en de back-ups geïmporteerd zijn, kunt u StorSimple Snapshot Manager te beheren, net zoals u zou volume groepen en back-ups die u hebt gemaakt en geconfigureerd met StorSimple Snapshot Manager beheren. 

## <a name="refresh-connected-devices"></a>Verbonden apparaten vernieuwen
Gebruik de volgende procedure om te synchroniseren van de verbonden StorSimple-apparaten met StorSimple Snapshot Manager.

#### <a name="to-refresh-connected-devices"></a>Verbonden apparaten vernieuwen
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster met de rechtermuisknop op **apparaten**, en klik vervolgens op **apparaten vernieuwen**. Dit synchroniseert de verbonden apparaten met StorSimple Snapshot Manager zodat u de volume-groepen en de back-ups, met inbegrip van alle recente toevoegingen kunt weergeven. 
   
    ![Vernieuwen van het StorSimple-apparaten](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

De **apparaten vernieuwen** actie haalt nieuwe groepen volume en eventuele bijbehorende back-ups van verbonden apparaten. In tegenstelling tot de **volumes opnieuw scannen** actie beschikbaar is voor de **Volumes** knooppunt **apparaten vernieuwen** herstelt niet de reservekopie van het register.

## <a name="authenticate-a-device"></a>Een apparaat verifiëren
Gebruik de volgende procedure om een StorSimple-apparaat met StorSimple Snapshot Manager te verifiëren.

#### <a name="to-authenticate-a-device"></a>Een apparaat te verifiëren
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, klikt u op **apparaten**.
3. In de **resultaten** deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **verifiëren**.
4. De **verifiëren** dialoogvenster wordt weergegeven. Typ het apparaatwachtwoord en klik vervolgens op **OK**.
   
    ![In het dialoogvenster verifiëren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Details van de apparaten weergeven
Gebruik de volgende procedure om te zien van de details van een StorSimple-apparaat en, indien nodig, opnieuw synchroniseren van het apparaat met StorSimple Snapshot Manager.

#### <a name="to-view-and-resynchronize-device-details"></a>Details weergeven en opnieuw synchroniseren van apparaat
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, klikt u op **apparaten**.
3. In de **resultaten** deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **Details**.

4. Klik de **Apparaatdetails** dialoogvenster wordt weergegeven. In dit vak weergegeven naam, model, versie, serienummer, status, doel iSCSI Qualified Name (IQN) en laatste synchronisatiedatum en tijd.

* Klik op **Resync** de synchronisatie.
* Klik op **OK** of **annuleren** om het dialoogvenster te sluiten.
  
  ![Apparaatdetails](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Een afzonderlijk apparaat vernieuwen
Gebruik de volgende procedure om te synchroniseren van een afzonderlijk StorSimple-apparaat met StorSimple Snapshot Manager.

#### <a name="to-refresh-a-device"></a>Een apparaat vernieuwen
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager. 
2. In de **bereik** deelvenster, klikt u op **apparaten**. 
3. In de **resultaten** deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **apparaat vernieuwen**. Dit synchroniseert met StorSimple Snapshot Manager het apparaat.

## <a name="delete-a-device-configuration"></a>De apparaatconfiguratie van een verwijderen
Gebruik de volgende procedure om te verwijderen van een afzonderlijke configuratie van de StorSimple-apparaat uit StorSimple Snapshot Manager.

#### <a name="to-delete-a-device-configuration"></a>De configuratie van een apparaat verwijderen
1. Klik op het bureaubladpictogram voor het starten van StorSimple Snapshot Manager.
2. In de **bereik** deelvenster, klikt u op **apparaten**. 
3. In de **resultaten** deelvenster met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **verwijderen**. 
4. Het volgende bericht wordt weergegeven. Klik op **Ja** wilt verwijderen van de configuratie of klik op **Nee** om de verwijdering te annuleren.
   
    ![Configureren van een apparaat verwijderen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Een apparaatwachtwoord verlopen wijzigen
U moet een wachtwoord voor het verifiëren van een StorSimple-apparaat met StorSimple Snapshot Manager invoeren. U kunt dit wachtwoord configureren wanneer u de Windows PowerShell-interface gebruiken voor het instellen van het apparaat. Het wachtwoord kan echter verloopt. Als dit gebeurt, kunt u de klassieke Azure portal gebruiken om het wachtwoord te wijzigen. Vervolgens, omdat het apparaat is in StorSimple Snapshot Manager geconfigureerd voordat het wachtwoord is verlopen, moet u het apparaat in StorSimple Snapshot Manager opnieuw worden geverifieerd.

#### <a name="to-change-the-expired-password"></a>Het verlopen wachtwoord te wijzigen
1. In de klassieke Azure portal, de StorSimple Manager-service te starten.
2. Klik op **apparaten** > **configureren** voor het apparaat.
3. Schuif omlaag naar de sectie StorSimple Snapshot Manager. Voer een wachtwoord 14-15 tekens. Zorg ervoor dat het wachtwoord een combinatie van hoofdletters, kleine letters, numerieke en speciale tekens bevat.
4. Voer het wachtwoord ter bevestiging.
5. Klik op **Opslaan** onder aan de pagina.

#### <a name="to-re-authenticate-the-device"></a>Het apparaat opnieuw worden geverifieerd
1. StorSimple Snapshot Manager start.
2. In de **bereik** deelvenster, klikt u op **apparaten**. Een lijst met geconfigureerde apparaten wordt weergegeven in de **resultaten** deelvenster.
3. Selecteer het apparaat, klik met de rechtermuisknop en klik vervolgens op **verifiëren**.
4. In de **verifiëren** venster, voer het nieuwe wachtwoord.
5. Selecteer het apparaat, klik met de rechtermuisknop en selecteer **vernieuwen apparaat**. Dit synchroniseert met StorSimple Snapshot Manager het apparaat.

## <a name="replace-a-failed-device"></a>Vervangen van een mislukte apparaat
Als een StorSimple-apparaat mislukt en wordt vervangen door een apparaat stand-by (failover), gebruik de volgende stappen verbinding maken met het nieuwe apparaat en de bijbehorende back-ups weergeven.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Verbinding maken met een nieuw apparaat na een failover
1. De iSCSI-verbinding naar het nieuwe apparaat opnieuw configureren. Voor instructies, gaat u naar ' stap 7: koppelen, initialiseren en een volume-indeling ' in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md).

> [!NOTE]
> Als het nieuwe StorSimple-apparaat hetzelfde IP-adres als de oude heeft heeft, is het mogelijk dat u de configuratie van de oude verbinding kunnen maken.


1. De Microsoft StorSimple Management-Service stoppen:
   
   1. Start Serverbeheer.
   2. Op het Dashboard Serverbeheer op de **extra** selecteert u **Services**.
   3. Op de **Services** Selecteer de **Microsoft StorSimple Management Service**.
   4. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Stop de service**.
2. De configuratie-informatie die betrekking hebben op het oude apparaat verwijderen:
   
   1. Blader in Windows Verkenner naar C:\ProgramData\Microsoft\StorSimple\BACatalog.
   2. Verwijder de bestanden in de map BACatalog.
3. De Microsoft StorSimple Management-Service opnieuw starten:
   
   1. Op het Dashboard Serverbeheer op de **extra** selecteert u **Services**.
   2. Op de **Services** Selecteer de **Microsoft StorSimple Management Service**.
   3. In het rechterdeelvenster onder **Microsoft StorSimple Management Service**, klikt u op **Start de service opnieuw**.
4. StorSimple Snapshot Manager start.
5. Voor het configureren van het nieuwe StorSimple-apparaat, voer de stappen in stap 2: verbinding maken met een virtueel StorSimple-apparaat in [implementeren StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
6. Met de rechtermuisknop op het knooppunt op het hoogste niveau in de **bereik** deelvenster (StorSimple Snapshot Manager in het voorbeeld) en klik vervolgens op **wisselknop invoer weergave**. 
7. Een bericht wordt weergegeven wanneer de geïmporteerde volume groepen en de back-ups kunnen weergegeven in StorSimple Snapshot Manager worden. Klik op **OK**.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Meer informatie over hoe [StorSimple Snapshot Manager weergeven en beheren van volumes gebruiken](storsimple-snapshot-manager-manage-volumes.md).

