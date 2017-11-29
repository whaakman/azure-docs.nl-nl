## <a name="install-wordpress"></a>WordPress installeren

Als u uw stack proberen wilt, installeert u een voorbeeld-app. Als u bijvoorbeeld de volgende stappen uit de open source installeren [WordPress](https://wordpress.org/) platform om websites en blogs te maken. Andere werkbelastingen om te proberen behoren [Drupal](http://www.drupal.org) en [Moodle](https://moodle.org/). 

Dit WordPress is alleen voor het testen van het concept. Zie voor informatie over het installeren van de meest recente WordPress in productie met aanbevolen beveiligingsinstellingen voor de [WordPress documentatie](https://codex.wordpress.org/Main_Page). 



### <a name="install-the-wordpress-package"></a>De WordPress-pakket installeren

Voer de volgende opdracht uit:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>WordPress configureren

Configureer WordPress voor het gebruik van MySQL en PHP.

Maak een tekstbestand in een werkmap `wordpress.sql` de MySQL-database voor WordPress configureren: 

```bash
sudo sensible-editor wordpress.sql
```

Voeg de volgende opdrachten, vervangen door een wachtwoord van uw keuze voor *yourPassword* (andere waarden ongewijzigd laten). Als u eerder hebt ingesteld dat een MySQL-beveiligingsbeleid Wachtwoordsterkte valideren, zorg er dan voor dat het wachtwoord voldoet aan de vereisten voor Wachtwoordsterkte. Sla het bestand op.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```

Voer de volgende opdracht om de database te maken:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Omdat het bestand `wordpress.sql` bevat databasereferenties, dit na gebruik verwijderen:

```bash
sudo rm wordpress.sql
```

Voer de volgende opdracht te open een teksteditor van uw keuze en maak het bestand voor het configureren van PHP `/etc/wordpress/config-localhost.php`:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Kopieer de volgende regels op het bestand, vervangen door een wachtwoord voor de WordPress-database voor *yourPassword* (andere waarden ongewijzigd laten). Sla het bestand op.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```


De WordPress-installatie verplaatsen naar de hoofdmap van het web server-document:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

U kunt nu de WordPress-installatie is voltooid en publiceren van het platform. Open een browser en Ga naar `http://yourPublicIPAddress/wordpress`. Vervang het openbare IP-adres van uw virtuele machine. Het moet eruitzien als aan deze installatiekopie.

![Pagina met WordPress-installatie](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)