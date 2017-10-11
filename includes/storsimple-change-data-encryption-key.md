<!--author=SharS last changed: 12/01/15-->

### <a name="step-1-authorize-a-device-to-change-the-service-data-encryption-key-in-the-azure-classic-portal"></a>Stap 1: Machtig een apparaat wijzigen van de gegevensversleutelingssleutel van service in de klassieke Azure portal
Normaal gesproken wordt de apparaatbeheerder vragen of de service-beheerder een apparaat wijzigen gegevensversleutelingssleutels service autoriseren. De servicebeheerder machtigen vervolgens het apparaat om de sleutel te wijzigen.

Deze stap wordt uitgevoerd in de klassieke Azure portal. De servicebeheerder kunt u een apparaat selecteren uit een lijst weergegeven van de apparaten die in aanmerking komen worden geautoriseerd. Het apparaat is vervolgens gemachtigd om de service gegevens versleuteling wijzigingsproces te starten.

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Welke apparaten kunnen worden geautoriseerd service gegevensversleutelingssleutels wijzigen?
Een apparaat moet de volgende criteria voldoen voordat deze kan worden geautoriseerd data encryption key servicewijzigingen initiëren:

* Het apparaat moet online in aanmerking komt voor service data encryption wijziging autorisatie.
* Als de wijziging is niet geïnitieerd, kunt u hetzelfde apparaat na 30 minuten opnieuw autoriseren.
* U kunt een ander apparaat autoriseren, mits de sleutel wijziging is niet geïnitieerd door de eerder gemachtigd apparaat. Nadat het nieuwe apparaat is verleend, kan de wijziging van het oude apparaat niet starten.
* U kunt een apparaat kan niet machtigen, terwijl de overschakeling van de gegevensversleutelingssleutel van de service uitgevoerd wordt.
* Wanneer enkele van de apparaten die zijn geregistreerd bij de service via de versleuteling hebt hersteld, terwijl andere gebruikers niet hebben, kunt u een apparaat autoriseren. In dergelijke gevallen worden de apparaten die in aanmerking wijzigen de waarden die u hebt de gegevensversleutelingssleutel van service voltooid.

> [!NOTE]
> In de klassieke Azure portal, worden virtuele StorSimple-apparaten niet weergegeven in de lijst met apparaten die kunnen worden gemachtigd voor het starten van de sleutel wijzigen.
> 
> 

De volgende stappen uit om te selecteren en autoriseren van een apparaat opnieuw te starten van de service gegevens versleuteling belangrijke wijziging uitvoeren.

#### <a name="to-authorize-a-device-to-change-the-key"></a>Voor het autoriseren van een apparaat om de sleutel te wijzigen
1. Klik op de dashboardpagina service **wijziging service gegevensversleutelingssleutel**.
   
    ![Versleutelingssleutel voor service wijzigen](./media/storsimple-change-data-encryption-key/HCS_ChangeServiceDataEncryptionKey-include.png)
2. In de **wijziging service gegevensversleutelingssleutel** dialoogvenster vak en selecteer een apparaat opnieuw te starten van de service gegevens versleuteling belangrijke wijziging autoriseren. De vervolgkeuzelijst bevat alle in aanmerking komende apparaten die kunnen worden geautoriseerd.
3. Klik op het vinkje ![vinkje](./media/storsimple-change-data-encryption-key/HCS_CheckIcon-include.png).

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Stap 2: Gebruik Windows PowerShell voor StorSimple initiëren service data encryption key wijzigen
Deze stap wordt uitgevoerd in de Windows PowerShell voor StorSimple-interface op het geautoriseerde StorSimple-apparaat.

> [!NOTE]
> Er zijn geen bewerkingen kunnen worden uitgevoerd in de klassieke Azure portal van uw StorSimple Manager-service totdat de overschakeling van de sleutel is voltooid.
> 
> 

Als u de seriële console van het apparaat verbinding maken met de Windows PowerShell-interface gebruikt, moet u de volgende stappen uitvoeren.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Starten van service data encryption key wijzigen
1. Selecteer optie 1 om aan te melden met volledige toegang.
2. Typ het volgende achter de opdrachtprompt:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Nadat de cmdlet is voltooid, ontvangt u een nieuwe versleutelingssleutel van de service-gegevens. Kopieer en sla deze sleutel voor gebruik in stap 3 van dit proces. Deze sleutel wordt gebruikt om bij te werken van alle resterende apparaten die zijn geregistreerd bij de StorSimple Manager-service.
   
   > [!NOTE]
   > Dit proces moet worden gestart binnen vier uur na het autoriseren van een StorSimple-apparaat.
   > 
   > 
   
   Deze nieuwe sleutel is vervolgens naar de service moet worden gepusht naar alle apparaten die zijn geregistreerd bij de service verzonden. Een waarschuwing wordt vervolgens weergegeven op het servicedashboard. De service worden alle bewerkingen op de geregistreerde apparaten uitschakelen en vervolgens de apparaatbeheerder moet bijwerken van de gegevensversleutelingssleutel van service op de andere apparaten. De i/o's (hosts verzenden van gegevens naar de cloud) worden echter niet verstoord.
   
   Als u één apparaat geregistreerd met uw service hebt, de rollover is nu voltooid en kunt u de volgende stap overslaan. Als u meerdere apparaten die zijn geregistreerd met uw service hebt, gaat u verder met stap 3.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Stap 3: Bijwerken van de gegevensversleutelingssleutel van service op andere StorSimple-apparaten
Deze stappen moeten worden uitgevoerd in de Windows PowerShell-interface van uw StorSimple-apparaat als er meerdere apparaten zijn geregistreerd met uw StorSimple Manager-service. De sleutel die u hebt verkregen in stap 2: gebruik Windows PowerShell voor StorSimple starten van de service gegevens versleuteling belangrijke wijziging moet worden gebruikt om bij te werken van alle de resterende StorSimple-apparaat geregistreerd bij de StorSimple Manager-service.

Voer de volgende stappen uit voor het bijwerken van de versleuteling van de service-gegevens op uw apparaat.

#### <a name="to-update-the-service-data-encryption-key"></a>Bijwerken van de gegevensversleutelingssleutel van service
1. Windows PowerShell voor StorSimple gebruiken voor het verbinding maken met de console. Selecteer optie 1 om aan te melden met volledige toegang.
2. Typ het volgende achter de opdrachtprompt:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Geef de versleutelingssleutel van de service-gegevens die u hebt verkregen in [stap 2: gebruik Windows PowerShell voor StorSimple starten van de service gegevens versleuteling belangrijke wijziging](#to-initiate-the-service-data-encryption-key-change).

