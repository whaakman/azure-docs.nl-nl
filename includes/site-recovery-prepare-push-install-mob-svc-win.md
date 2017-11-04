### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Voorbereiden voor een pushinstallatie op een Windows-computer

1. Zorg ervoor dat er netwerkverbinding tussen de Windows-computer en de processerver is.
2. Maak een account dat op de processerver kan worden gebruikt voor toegang tot de computer. Het account moet administrator-rechten (lokaal of domeinbeheerder) hebben. (Gebruik dit account alleen voor de push-installatie en agentupdates.)

   > [!NOTE]
   > Als u niet een domeinaccount, moet u externe toegang van de gebruiker besturingselement op de lokale computer uitschakelen. Een nieuwe DWORD uitschakelen van de externe gebruiker toegangsbeheer, onder de registersleutel HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen: **LocalAccountTokenFilterPolicy**. De waarde instelt op **1**. Om dit te doen bij een opdrachtprompt, voer de volgende opdracht:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. Selecteer in Windows Firewall op de computer die u wilt beveiligen, **toestaan van een app of functie via Firewall**. Schakel **bestands- en printerdeling** en **Windows Management Instrumentation (WMI)**. Voor computers die deel uitmaken van een domein, kunt u de firewall-instellingen configureren met behulp van een groepsbeleidsobject (GPO).

   ![Firewallinstellingen](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. Voeg het account toe dat u hebt gemaakt in CSPSConfigtool.
    1.  Meld u aan bij de configuratieserver.
    2.  Open **cspsconfigtool.exe**. (Als snelkoppeling beschikbaar op het bureaublad en in de map %ProgramData%\home\svsystems\bin.)
    3.  Op de **Accounts beheren** tabblad **Account toevoegen**.
    4.  Voeg het account toe dat u hebt gemaakt.
    5.  Voer de referenties in die u gebruikt wanneer u replicatie voor een computer inschakelt.
