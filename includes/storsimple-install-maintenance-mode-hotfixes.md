<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Installeren van onderhoud modus hotfixes via Windows PowerShell voor StorSimple
> [!IMPORTANT]
> In de onderhoudsmodus bevindt moet u eerst de hotfix toepassen op een domeincontroller en klik vervolgens op de andere controller.
> 
> 

1. Plaats het apparaat in de onderhoudsmodus. Zie [stap 2: Voer onderhoudsmodus](../articles/storsimple/storsimple-update-device.md#step2) voor instructies voor het invoeren van de onderhoudsmodus.
2. U kunt de hotfix, typt u:
   
     `Start-HcsHotfix` 
3. Als u wordt gevraagd, geeft u het pad naar de gedeelde netwerkmap waarin de hotfixbestanden.
4. U wordt gevraagd om bevestiging. Type **Y** om door te gaan met de hotfixinstallatie.
5. Nadat u de hotfix hebt toegepast op een domeincontroller, moet u zich aanmelden bij de andere controller. De hotfix toepassen, zoals u voor de vorige controller hebt.
6. Nadat de hotfixes worden toegepast, sluit u de onderhoudsmodus. Zie [stap 4: afsluiten onderhoudsmodus](../articles/storsimple/storsimple-update-device.md#step4) voor instructies.

