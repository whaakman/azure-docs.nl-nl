<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Installeren van reguliere hotfixes via Windows PowerShell voor StorSimple
1. Verbinding maken met de seriële console van het apparaat. Zie voor meer informatie [stap 1: verbinding maken met de seriële console](../articles/storsimple/storsimple-update-device.md#step1).
2. Selecteer in het menu van de seriële console optie 1, **aanmelden met volledige toegang**. Typ het wachtwoord. Is het standaardwachtwoord **Wachtwoord1**.
3. Typ het volgende achter de opdrachtprompt:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Met deze opdracht geldt alleen voor reguliere hotfixes. U deze opdracht uitvoert op slechts één domeincontroller, maar beide domeincontrollers wordt bijgewerkt.
    > Merkt u wellicht een failover van de domeincontroller tijdens het updateproces; echter, de failover niet van invloed op beschikbaarheid van het systeem of de bewerking.

4. Als u wordt gevraagd, geeft u het pad naar de gedeelde netwerkmap waarin de hotfixbestanden.
5. U wordt gevraagd om bevestiging. Type **Y** om door te gaan met de hotfixinstallatie.

