
1. Als u wilt escaleren bevoegdheden, typt u:
   
        sudo -s
   
    Voer uw wachtwoord in.
2. Voor het installeren van MySQL-Community servereditie, typt u:
   
        zypper install mysql-community-server
   
    Een ogenblik geduld terwijl MySQL downloadt en installeert.
3. Om in te stellen MySQL wordt gestart wanneer het systeem wordt opgestart, typt u:
   
        insserv mysql
4. De MySQL-daemon (mysqld) handmatig starten met deze opdracht:
   
        rcmysql start
   
    Als u wilt de status van de MySQL-daemon controleren, typt u:
   
        rcmysql status
   
    Als u wilt de MySQL-daemon stoppen, typt u:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > Na de installatie is het hoofdwachtwoord MySQL standaard leeg. Het is raadzaam dat u uitvoert **mysql\_beveiligde\_installatie**, een script waarmee veilige MySQL. Het script vraagt u om te wijzigen van het hoofdwachtwoord MySQL, verwijdert u anonieme gebruikersaccounts, externe hoofdmap aanmeldingen uitschakelen, test databases verwijderen en opnieuw laden van de tabel bevoegdheden. We raden u antwoord Ja op al deze opties en het root-wachtwoord wijzigen.
   > 
   > 
5. Typ dit script van MySQL voor installatie van het script uitgevoerd:
   
        mysql_secure_installation
6. Aanmelden bij MySQL:
   
        mysql -u root -p
   
    Voer het hoofdwachtwoord van MySQL (die u hebt gewijzigd in de vorige stap) en er moet worden weergegeven met een prompt waar SQL-instructies om te communiceren met de database kunnen worden verleend.
7. Voor het maken van een nieuwe MySQL-gebruiker, voert u de volgende op de **mysql >** prompt:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Opmerking: de puntkomma (;) aan het einde van de regels zijn essentieel voor het beëindigen van de opdrachten.
8. Een database maken en verlenen de `mysqluser` gebruikersmachtigingen voor het uitgeven van de volgende opdrachten:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Houd er rekening mee dat database gebruikersnamen en wachtwoorden alleen worden gebruikt door scripts verbinden met de database.  Account voor database gebruikersnamen noodzakelijkerwijs niet werkelijke gebruikersaccounts op het systeem.
9. Als u wilt zich aanmelden vanaf een andere computer, typt u:
   
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    waar `ip-address` het IP-adres van de computer van waaruit u verbinding met MySQL.
10. Typ de MySQL-database-beheerprogramma om af te sluiten:
    
        quit

## <a name="add-an-endpoint"></a>Een eindpunt toevoegen
1. Nadat de MySQL is geïnstalleerd, moet u een eindpunt voor MySQL op afstand toegang tot configureren. Meld u aan bij de [klassieke Azure-portal][AzurePortal]. Klik op **virtuele Machines**, klik op de naam van uw nieuwe virtuele machine en klik vervolgens op **eindpunten**.
2. Klik op **toevoegen** aan de onderkant van de pagina.
3. Toevoegen van een eindpunt met de naam 'MySQL' met protocol **TCP**, en **openbare** en **persoonlijke** poorten ingesteld op '3306'.
4. Om extern verbinding maken met de virtuele machine van de computer, typt u:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Met behulp van de virtuele machine die we in deze zelfstudie hebt gemaakt, typ deze opdracht:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
