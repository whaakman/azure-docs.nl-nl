### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Een push-installatie voorbereiden op een Linux-server

1. Zorg ervoor dat er netwerkverbinding tussen de Linux-computer en de processerver is.
2. Maak een account dat op de processerver kan worden gebruikt voor toegang tot de computer. Het account moet een **rootgebruiker** zijn op de Linux-bronserver. Gebruik dit account alleen voor de push-installatie en voor updates.
3. Controleer of het bestand /etc/hosts op de Linux-bronserver vermeldingen bevat die de lokale hostnaam toewijzen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
4. Installeer de meest recente openssh, openssh-server en openssl-pakketten op de computer die u wilt repliceren.
5. Zorg ervoor dat SSH (Secure Shell) is ingeschakeld en wordt uitgevoerd op poort 22.
6. SFTP subsysteem voor verificatie en wachtwoord in het bestand sshd_config inschakelen. Volg deze stappen:

    a. Meld u aan als **rootgebruiker**.

    b. In de **/etc/ssh/sshd_config** bestand, zoek de regel die met begint **PasswordAuthentication**.

    c. Verwijder de opmerkingen in de regel en wijzig de waarde in **Ja**.

    d. Zoek de regel die met begint **subsysteem**, en het commentaar verwijderen van de regel.

      ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

    e. Start de service **sshd** opnieuw.

7. Voeg het account toe dat u hebt gemaakt in CSPSConfigtool. Volg deze stappen:

    a. Meld u aan bij de configuratieserver.

    b. Open **cspsconfigtool.exe**. Het is beschikbaar als een snelkoppeling op het bureaublad en in de map %ProgramData%\home\svsystems\bin.

    c. Op de **Accounts beheren** tabblad **Account toevoegen**.

    d. Voeg het account toe dat u hebt gemaakt.

    d. Voer de referenties in die u gebruikt wanneer u replicatie voor een computer inschakelt.
