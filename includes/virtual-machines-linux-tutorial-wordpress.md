---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 5df1f7ff44a1603dd03d1d803ae9960dc124781e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176042"
---
## <a name="install-wordpress"></a>WordPress installeren

Als u uw stack wilt uitproberen, installeert u een voorbeeld-app. Bijvoorbeeld: met de volgende stappen installeert u het open-sourceplatform [WordPress](https://wordpress.org/) om website en blogs te maken. Andere workloads die u kunt uitproberen, zijn onder andere [Drupal](http://www.drupal.org) en [Moodle](https://moodle.org/). 

Deze WordPress-installatie is alleen bedoeld als Proof of Concept. Raadpleeg de [WordPress-documentatie](https://codex.wordpress.org/Main_Page) voor het installeren van de nieuwste WordPress-versie in een productieomgeving met de aanbevolen beveiligingsinstellingen. 



### <a name="install-the-wordpress-package"></a>Het WordPress-pakket installeren

Voer de volgende opdracht uit:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>WordPress configureren

WordPress configureren voor het gebruik van MySQL en PHP.

Maak in een werkmap een tekstbestand `wordpress.sql` om de MySQL-database te configureren voor WordPress: 

```bash
sudo sensible-editor wordpress.sql
```

Voeg de volgende opdrachten toe, waarbij u *yourPassword* vervangt door een databasewachtwoord van uw keuze (laat andere waarden ongewijzigd). Als u eerder MySQL-beveiligingsbeleid hebt ingesteld om de wachtwoordsterkte te valideren, moet u ervoor zorgen dat het wachtwoord voldoet aan de vereisten voor een sterk wachtwoord. Sla het bestand op.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```

Voer de volgende opdracht uit om de database te maken:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Omdat het bestand `wordpress.sql` databasereferenties bevat, verwijdert u het bestand na gebruik:

```bash
sudo rm wordpress.sql
```

Configureer PHP door de volgende opdracht uit te voeren om een teksteditor van uw keuze te openen en het bestand `/etc/wordpress/config-localhost.php` te maken:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Kopieer de volgende regels in het bestand, waarbij u *yourPassword* vervangt door het WordPress-wachtwoord (laat andere waarden ongewijzigd). Sla het bestand op.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```


Verplaats de WordPress-installatie naar de hoofdmap van het webserverdocument:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

Nu kunt u de WordPress-installatie voltooien en op het platform publiceren. Open een browser en ga naar `http://yourPublicIPAddress/wordpress`. Vervang het openbare IP-adres van uw VM. Het moet er ongeveer uitzien als in deze afbeelding.

![Installatiepagina voor WordPress](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)