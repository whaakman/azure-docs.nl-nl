<!--author=SharS last changed: 02/22/2016-->

### <a name="to-configure-and-register-the-device"></a>Het apparaat configureren en registreren
1. Open de Windows PowerShell-interface op de seriële console van het StorSimple-apparaat. Zie [PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) voor instructies. **Volg de procedure exact, omdat u anders geen toegang hebt tot de console.**
2. Druk in de geopende sessie eenmaal op Enter om een opdrachtprompt weer te geven.
3. U wordt gevraagd de taal te kiezen die u voor uw apparaat wilt instellen. Geef de taal op en druk op Enter.
   
    ![Apparaat 1 configureren en registreren met StorSimple](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. Kies in het weergegeven menu van de seriële console optie 1 om u aan te melden met volledige toegang.
   
    ![Apparaat 2 registreren met StorSimple](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. Voer de volgende stappen uit voor het configureren van de minimale vereiste netwerkinstellingen voor uw apparaat.
   
   > [!IMPORTANT]
   > Deze configuratiestappen moeten worden uitgevoerd op de actieve controller van het apparaat. In het menu van de seriële console wordt de controllerstatus in het bannerbericht aangegeven. Als u geen verbinding maken met de actieve controller, verbreken en maak verbinding met de actieve controller.
   > 
   > 
   
   1. Typ uw wachtwoord bij de opdrachtprompt. Het standaardapparaatwachtwoord is **Password1**.
   2. Typ de volgende opdracht:
      
        `Invoke-HcsSetupWizard`
   3. Er wordt een instellingenwizard weergegeven om u te helpen bij het configureren van de netwerkinstellingen voor het apparaat. Geef de volgende gegevens:
      
      * IP-adres voor DATA 0-netwerkinterface
      * Subnetmasker
      * Gateway
      * IP-adres voor de primaire DNS-server
      * IP-adres voor de primaire NTP-server
      
      > [!NOTE]
      > U moet wachten op een paar minuten voor het subnetmasker en DNS-instellingen worden toegepast.
      > 
      > 
   4. Configureer desgewenst uw webproxyserver.
      
      > [!IMPORTANT]
      > Hoewel webproxyconfiguratie optioneel is, worden op de hoogte gebracht als u een webproxy gebruikt, u alleen deze hier configureren kunt. Zie [Configure web proxy for your device](../articles/storsimple/storsimple-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie.
      > 
      > 
6. Druk op Ctrl + C om de wizard setup af te sluiten.
7. Als volgt te werk om de updates te installeren:
   
   1. Gebruik de volgende cmdlet IP-adressen op beide domeincontrollers instellen:
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. Voer bij de opdrachtprompt `Get-HcsUpdateAvailability`. U moet een melding dat er updates beschikbaar zijn.
   3. Voer `Start-HcsUpdate` uit. U kunt deze opdracht uitvoeren op een willekeurig knooppunt. Updates worden toegepast op de eerste domeincontroller, de controller failover en vervolgens de updates worden toegepast op de andere controller.
      
      U kunt de voortgang van de update door te voeren `Get-HcsUpdateStatus`.    
      
      Hieronder ziet u een voorbeeld van uitvoer terwijl de update nog bezig is.
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ````
      
      In de volgende voorbeelduitvoer wordt aangegeven dat de update is voltooid.
      
      ```
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ```
      
      Het kan tot 11 uur duren om alle updates, met inbegrip van de Windows-Updates toepassen.
8. Voer de volgende cmdlet als het apparaat naar de portal voor Microsoft Azure Government (omdat deze naar de openbare klassieke Azure portal standaard verwijst). Dit wordt opnieuw opgestart beide domeincontrollers. Het is raadzaam dat u twee PuTTY sessies tegelijk verbinding maken met beide domeincontrollers zodat u kunt zien als elke domeincontroller opnieuw wordt gestart.
   
    `Set-CloudPlatform -AzureGovt_US`
   
   Hier ziet u een bevestigingsbericht. Accepteer de standaardinstelling (**Y**).
9. Voer de volgende cmdlet als u wilt doorgaan met setup:
   
    `Invoke-HcsSetupWizard`
   
    ![De wizard setup hervatten](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
   Als u setup wilt activeren, de wizard worden de versie van de Update 2.
10. De netwerkinstellingen accepteren. U ziet een validatiebericht nadat u elke instelling hebt geaccepteerd.
11. Uit veiligheidsoverwegingen is het beheerderswachtwoord van het apparaat na de eerste sessie verlopen en moet u het nu wijzigen. Als daarom wordt gevraagd, geeft u een beheerderswachtwoord voor het apparaat op. Een geldig beheerderswachtwoord voor een apparaat moet 8 tot 15 tekens bevatten. Het wachtwoord moet drie van de volgende elementen bevatten: kleine letters, hoofdletters, cijfers en speciale tekens.
    
    <br/>![Apparaat 5 registreren met StorSimple](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. Bij de laatste stap in de instellingenwizard wordt uw apparaat geregistreerd met de StorSimple Manager-service. Hiervoor moet u de serviceregistratiesleutel die u hebt verkregen in [stap 2: de serviceregistratiesleutel ophalen](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key). Nadat u de registratiesleutel hebt opgegeven, moet u mogelijk twee tot drie minuten wachten voordat het apparaat is geregistreerd.
    
    > [!NOTE]
    > U kunt op elk gewenst moment op Ctrl+C drukken om de instellingenwizard af te sluiten. Als u alle netwerkinstellingen (IP-adres voor Data 0, subnetmasker en gateway) hebt ingevoerd, blijven uw gegevens behouden.
    > 
    > 
    
    ![Voortgang van de StorSimple-registratie](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. Nadat het apparaat is geregistreerd, wordt er een gegevensversleutelingssleutel van de service weergegeven. Kopieer deze sleutel en bewaar deze op een veilige plaats. **Deze sleutel is vereist bij de serviceregistratiesleutel om extra apparaten te registreren met de StorSimple Manager-service.** Zie [StorSimple security](../articles/storsimple/storsimple-security.md) (StorSimple-beveiliging) voor meer informatie over deze sleutel.
    
    ![Apparaat 7 registreren met StorSimple](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > Kopieer de tekst uit het venster van de seriële console gewoon door de tekst te selecteren. U kunt de tekst vervolgens op het klembord of in een teksteditor plakken.
    > 
    > Kopieer de gegevensversleutelingssleutel van de service NIET met Ctrl+C. Als u op Ctrl+C drukt, wordt de instellingenwizard afgesloten. Als gevolg hiervan wordt het beheerderswachtwoord van het apparaat niet gewijzigd en wordt het standaardwachtwoord opnieuw ingesteld.
    > 
    > 
14. Sluit de seriële console af.
15. Terug naar de overheid van de Azure Portal en voer de volgende stappen uit:
    
    1. Dubbelklik op de StorSimple Manager-service om de pagina **Quick Start** te openen.
    2. Klik op **Verbonden apparaten weergeven**.
    3. Controleer op de pagina **Apparaten** aan de hand van de status of het apparaat verbinding heeft met de service. Het apparaat moet de status **Online** hebben.
       
        ![Pagina StorSimple-apparaten](./media/storsimple-configure-and-register-device-gov-u2/HCS_DeviceOnline-gov-include.png)
       
        Als de status van het apparaat **Offline** is, moet u een paar minuten wachten totdat het apparaat online is.
       
        Als het apparaat na een paar minuten nog steeds offline is, moet u ervoor zorgen dat uw firewallnetwerk is geconfigureerd zoals wordt beschreven in [Netwerkvereisten voor uw StorSimple-apparaat](../articles/storsimple/storsimple-system-requirements.md).
       
        Controleer of poort 9354 open is voor uitgaande communicatie, omdat de servicebus deze poort gebruikt voor de communicatie van de StorSimple Manager-service naar het apparaat.

