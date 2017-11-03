---
title: PostgreSQL op een Linux-VM instellen | Microsoft Docs
description: Meer informatie over het installeren en configureren van PostgreSQL op een virtuele Linux-machine in Azure
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: 0bccdc1cfdbda06b57da8cd662373ef137768672
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-and-configure-postgresql-on-azure"></a>PostgreSQL installeren en configureren op Azure
PostgreSQL is vergelijkbaar met Oracle en DB2 een geavanceerde open source-database. Het bevat enterprise-ready functies zoals volledige ACID naleving, betrouwbare transactionele verwerking en meerdere versie gelijktijdigheidbeheer. Het ondersteunt ook standaarden zoals ANSI SQL en SQL/MED (inclusief externe gegevens wrappers voor Oracle, MySQL, MongoDB en vele andere). Het is maximaal worden uitgebreid met ondersteuning voor meer dan 12 procedurele talen, GIN en basisvertalingen indexen, ondersteuning voor ruimtelijke gegevens en meerdere NoSQL-achtige functies voor JSON of toepassingen op basis van sleutels-waarde.

In dit artikel leert u hoe u kunt installeren en configureren van PostgreSQL op Azure een virtuele machine waarop Linux wordt uitgevoerd.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>PostgreSQL installeren
> [!NOTE]
> U moet al een virtuele machine van Azure waarop Linux wordt uitgevoerd om te kunnen voltooien van deze zelfstudie hebben. Als u wilt maken en instellen van een Linux-VM voordat u doorgaat, Zie de [Azure Linux VM-zelfstudie](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

In dit geval poort 1999 gebruiken als de PostgreSQL-poort.  

Verbinding maken met de Linux VM die u hebt gemaakt via de PuTTY. Als dit de eerste keer dat u een Azure Linux VM, Zie [het gebruik van SSH met Linux op Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over hoe u met PuTTY verbinding maken met een Linux-VM.

1. Voer de volgende opdracht om over te schakelen naar de hoofdmap (admin):
   
        # sudo su -
2. Sommige distributies hebben afhankelijkheden die moeten worden geïnstalleerd voordat u installeert PostgreSQL. Controleren op uw distro in deze lijst en voer de juiste opdracht:
   
   * Red Hat base Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian base Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. PostgreSQL downloaden naar de hoofdmap en pak vervolgens het pakket:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Het bovenstaande is een voorbeeld. U vindt meer gedetailleerde downloadadres in de [Index/pub/bron /](https://ftp.postgresql.org/pub/source/).
4. Voer deze opdrachten voor het starten van de build:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Als u bouwen alles die kunnen worden opgebouwd wilt, met inbegrip van de documentatie (HTML en man's) en de aanvullende modules (contrib) in plaats daarvan de volgende opdracht uitvoeren:
   
        # gmake install-world
   
    U moet het volgende bevestigingsbericht ontvangt:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL configureren
1. (Optioneel) Geen symbolische koppeling om de verwijzing PostgreSQL zodanig dat niet het versienummer korter te maken:
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. Maak een map voor de database:
   
        # mkdir -p /opt/pgsql_data
3. Een niet-hoofdgebruiker profiel maken en beheren van de gebruiker. Ga vervolgens naar deze nieuwe gebruiker (aangeroepen *postgres* in ons voorbeeld):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Uit veiligheidsoverwegingen PostgreSQL gebruikmaakt van een niet-hoofdgebruiker te initialiseren, openen of afsluiten van de database.
   > 
   > 
4. Bewerk de *bash_profile* bestand door te voeren van de onderstaande opdrachten. Deze regels worden toegevoegd aan het einde van de *bash_profile* bestand:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Uitvoeren van de *bash_profile* bestand:
   
        $ source .bash_profile
6. De installatie valideren met behulp van de volgende opdracht:
   
        $ which psql
   
    Als de installatie geslaagd is, ziet u het volgende antwoord:
   
        /opt/pgsql/bin/psql
7. U kunt ook de PostgreSQL-versie controleren:
   
        $ psql -V
8. Initialiseren van de database:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    U ontvangt de volgende uitvoer:

![Afbeelding](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL instellen
<!--    [postgres@ test ~]$ exit -->

Voer de volgende opdrachten uit:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Twee variabelen in het bestand /etc/init.d/postgresql wijzigen. Het voorvoegsel is ingesteld op het installatiepad van PostgreSQL: **/opt/pgsql**. PGDATA is ingesteld op het pad voor de opslag van gegevens van PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Afbeelding](./media/postgresql-install/no2.png)

Het bestand zodat het uitvoerbare bestand wijzigen:

    # chmod +x /etc/init.d/postgresql

PostgreSQL starten:

    # /etc/init.d/postgresql start

Controleer of het eindpunt van PostgreSQL op:

    # netstat -tunlp|grep 1999

U ziet de volgende uitvoer:

![Afbeelding](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Verbinding maken met de database Postgres
Overschakelen naar de gebruiker postgres weer:

    # su - postgres

Een database Postgres maken:

    $ createdb events

Verbinding maken met de database van de gebeurtenissen die u zojuist hebt gemaakt:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Maken en verwijderen van een tabel Postgres
Nu dat u verbinding hebt gemaakt met de database, kunt u tabellen maken in het.

Bijvoorbeeld, een nieuwe tabel van de voorbeeld-Postgres maken met behulp van de volgende opdracht:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

U hebt nu een tabel met vier kolommen met de volgende kolomnamen en -beperkingen ingesteld:

1. De kolom 'name', is door de opdracht VARCHAR onder 20 tekens lang zijn beperkt.
2. De kolom 'voeding' geeft aan dat het item voeding ervoor te dat elke persoon zorgen. VARCHAR beperkt deze tekst als maximaal 30 tekens.
3. De kolom 'bevestigde' registreert of de persoon uitnodiging heeft geaccepteerd het Amerikaans. De acceptabele waarden zijn "Y" en "N".
4. De 'datum' kolom ziet u wanneer ze zich aangemeld voor de gebeurtenis. Postgres vereist dat datums worden geschreven als jjjj-mm-dd.

Zie de volgende als de tabel is gemaakt:

![Afbeelding](./media/postgresql-install/no4.png)

U kunt ook de tabelstructuur controleren met behulp van de volgende opdracht:

![Afbeelding](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Gegevens toevoegen aan een tabel
Voeg eerst de informatie in een rij:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

U ziet deze uitvoer:

![Afbeelding](./media/postgresql-install/no6.png)

U kunt een aantal meer gebruikers toevoegen aan de tabel ook. Hier volgen enkele opties of u kunt uw eigen maken:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabellen weergeven
Gebruik de volgende opdracht om een tabel weer te geven:

    select * from potluck;

De uitvoer is:

![Afbeelding](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Gegevens in een tabel verwijderen
De volgende opdracht gebruiken om gegevens in een tabel te verwijderen:

    delete from potluck where name=’John’;

Hiermee verwijdert u de informatie in de rij 'John'. De uitvoer is:

![Afbeelding](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Gegevens bijwerken in een tabel
Gebruik de volgende opdracht om gegevens in een tabel te werken. Voor deze een heeft Sandy bevestigd dat ze is aanwezig, zodat we haar RSVP van "N" om 'Y' te wijzigen:

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>Meer informatie over PostgreSQL
Nu dat u de installatie van PostgreSQL in een Azure Linux VM hebt voltooid, kunt u profiteren van gebruiken in Azure. Voor meer informatie over PostgreSQL, gaat u naar de [PostgreSQL website](http://www.postgresql.org/).

