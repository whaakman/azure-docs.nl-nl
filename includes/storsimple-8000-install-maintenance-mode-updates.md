### <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Onderhoud modus updates via Windows PowerShell voor StorSimple installeren

Wanneer u onderhoud modus updates op StorSimple-apparaat toepassen, worden alle i/o-aanvragen zijn onderbroken. Services zoals niet-vluchtige RAM-geheugen (NVRAM) of de clusteringservice zijn gestopt. Beide domeincontrollers opnieuw worden opgestart wanneer u in of uit deze modus. Wanneer u deze modus afsluit, worden alle services hervatten en zijn in orde. (Dit kan enkele minuten duren.)

> [!IMPORTANT]
> * Controleer voordat u de onderhoudsmodus invoert, of beide apparaatcontrollers goed in de Azure portal. Als de domeincontroller is niet in orde, [contact opnemen met Microsoft ondersteuning](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) voor de volgende stappen.
> * Wanneer u in de onderhoudsmodus bevindt bent, moet u eerst één domeincontroller en vervolgens de andere controller bijwerken.

1. PuTTY gebruiken om verbinding maken met de seriële console. Volg de gedetailleerde instructies in [PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). Druk op de opdrachtregel op **Enter**. Selecteer optie 1 om aan te melden op het apparaat met volledige toegang.

2. Als u wilt de controller in onderhoudsmodus te plaatsen, typt u:
    
    `Enter-HcsMaintenanceMode`

    Beide domeincontrollers opnieuw opstarten in de onderhoudsmodus.

3. Uw onderhoud modus updates installeren. Type:

    `Start-HcsUpdate`

    U wordt gevraagd om bevestiging. Nadat u de updates hebt gecontroleerd, moeten ze worden geïnstalleerd op de domeincontroller die u momenteel gebruikmaken van. Nadat de updates zijn geïnstalleerd, wordt de domeincontroller opnieuw wordt opgestart.

4. De status van de updates bewaken. Meld u aan bij de peer-controller omdat de huidige controller worden bijgewerkt en kan geen andere opdrachten verwerken. Type:

    `Get-HcsUpdateStatus`

    Als de `RunInProgress` is `True`, de update wordt nog uitgevoerd. Als `RunInProgress` is `False`, betekent dit dat de update is voltooid.

5. Nadat de schijf firmware-updates zijn toegepast en de bijgewerkte domeincontroller opnieuw is opgestart, controleert u of de firmwareversie van de schijf. Typ op de bijgewerkte domeincontroller:

    `Get-HcsFirmwareVersion`
   
    De verwachte schijf-firmware-versies zijn:`XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`

6. De onderhoudsmodus af te sluiten. Typ de volgende opdracht voor elke domeincontroller apparaat:

    `Exit-HcsMaintenanceMode`

    De controllers starten opnieuw op wanneer u de onderhoudsmodus afsluit.

7. Ga terug naar de Azure-portal. De portal kan niet worden weergegeven dat u de updates van de modus onderhoud voor 24 uur geïnstalleerd.