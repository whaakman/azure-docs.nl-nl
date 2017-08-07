### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Een push-installatie voorbereiden op een Linux-server

1. Zorg dat er een netwerkverbinding is tussen de Linux-computer en de processerver.
2. Maak een account dat op de processerver kan worden gebruikt voor toegang tot de computer. Het account moet een **rootgebruiker** zijn op de Linux-bronserver. (Gebruik dit account alleen voor de push-installatie en voor updates.)
3. Controleer of het bestand /etc/hosts op de Linux-bronserver vermeldingen bevat die de lokale hostnaam toewijzen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
4. Installeer de meest recente openssh, openssh-server en openssl-pakketten op de computer die u wilt repliceren.
5. Zorg ervoor dat SSH (Secure Shell) is ingeschakeld en wordt uitgevoerd op poort 22.
6. Schakel het SFTP-subsysteem en wachtwoordverificatie in het bestand sshd_config in:
  1.  Meld u aan als **rootgebruiker**.
  2.  In het bestand /etc/ssh/sshd_config zoekt u de regel die begint met **PasswordAuthentication**.
  3.  Verwijder opmerkingen bij de regel en wijzig de waarde in **ja**.
  4.  Zoek de regel die begint met **Subsystem** en verwijder opmerkingen bij de regel.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. Start de service **sshd** opnieuw.

7. Voeg het account toe dat u hebt gemaakt in CSPSConfigtool.
    1.  Meld u aan bij de configuratieserver.
    2.  Open **cspsconfigtool.exe**. (Als snelkoppeling beschikbaar op het bureaublad en in de map %ProgramData%\home\svsystems\bin.)
    3.  Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    4.  Voeg het account toe dat u hebt gemaakt. 
    5.  Voer de referenties in die u gebruikt wanneer u replicatie voor een computer inschakelt.
