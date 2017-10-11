<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Onderhoud modus om updates te installeren via Windows PowerShell voor StorSimple
1. Als u dit nog niet hebt gedaan, toegang tot de seriële console van apparaat en selecteer optie 1, **aanmelden met volledige toegang**. 
2. Typ het wachtwoord. Is het standaardwachtwoord **Wachtwoord1**.
3. Typ het volgende achter de opdrachtprompt:
   
     `Get-HcsUpdateAvailability` 
4. U wordt gewaarschuwd als er updates beschikbaar zijn en of de updates verstoren of niet verstoren zijn. Om te verstoren updates hebt toegepast, moet u het apparaat in de onderhoudsmodus te plaatsen. Zie [stap 2: Voer onderhoudsmodus](../articles/storsimple/storsimple-update-device.md#step2) voor instructies.
5. Wanneer het apparaat in de onderhoudsmodus is, bij de opdrachtprompt, typt u:`Start-HcsUpdate`
6. U wordt gevraagd om bevestiging. Nadat u de updates hebt bevestigd, wordt ze worden geïnstalleerd op de domeincontroller die u momenteel gebruikmaken van. Nadat de updates zijn geïnstalleerd, wordt de controller wordt opnieuw opgestart. 
7. De status van de updates bewaken. Type:
   
    `Get-HcsUpdateStatus`
   
    Als de `RunInProgress` is `True`, de update wordt nog uitgevoerd. Als `RunInProgress` is `False`, betekent dit dat de update is voltooid.  
8. Wanneer de update is geïnstalleerd op de huidige controller en deze opnieuw is opgestart, verbinding met de andere controller en stappen 1 tot en met 6 uitvoeren.
9. Nadat beide domeincontrollers zijn bijgewerkt, sluit u de onderhoudsmodus. Zie [stap 4: afsluiten onderhoudsmodus](../articles/storsimple/storsimple-update-device.md#step4) voor instructies.

