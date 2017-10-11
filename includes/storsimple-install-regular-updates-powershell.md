<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>Reguliere om updates te installeren via Windows PowerShell voor StorSimple
1. Open de seriële console van apparaat en selecteer optie 1, **aanmelden met volledige toegang**. Typ het wachtwoord. Is het standaardwachtwoord *Wachtwoord1*. 
2. Typ het volgende achter de opdrachtprompt:
   
     `Get-HcsUpdateAvailability`
   
    U wordt gewaarschuwd als er updates beschikbaar zijn en of de updates verstoren of niet verstoren zijn.
3. Typ het volgende achter de opdrachtprompt:
   
     `Start-HcsUpdate`
   
    Het updateproces wordt gestart.

> [!IMPORTANT]
> * Met deze opdracht geldt alleen voor regelmatige updates. U deze opdracht uitvoert op slechts één domeincontroller, maar beide domeincontrollers wordt bijgewerkt. 
> * Merkt u wellicht een failover van de domeincontroller tijdens het updateproces; echter, de failover niet van invloed op beschikbaarheid van het systeem of de bewerking.
> 
> 

