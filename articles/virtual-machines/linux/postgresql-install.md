---
title: Instellen van PostgreSQL op een Linux-VM | Microsoft Docs
description: Meer informatie over het installeren en configureren van PostgreSQL op een virtuele Linux-machine in Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: e851f6f5433a832aa30a0d87a917b64d1bc721f2
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888274"
---
# <a name="install-and-configure-postgresql-on-azure"></a>PostgreSQL installeren en configureren op Azure
PostgreSQL is een geavanceerde open-source-database die vergelijkbaar is met Oracle en DB2. Het bevat enterprise-ready-functies zoals volledige ACID naleving, betrouwbare transactionele verwerking en gelijktijdigheidsbeheer voor meerdere versies. Het biedt ook ondersteuning voor standaarden, zoals ANSI SQL en SQL/MED (met inbegrip van externe gegevens wrappers voor Oracle, MySQL, MongoDB en nog veel meer). Het is maximaal worden uitgebreid met ondersteuning voor meer dan 12 procedurele talen GIN en basisvertalingen indexen, ruimtelijke gegevens en ondersteuning meerdere NoSQL-achtige functies voor JSON of toepassingen op basis van sleutel-waarde.

In dit artikel leert u hoe u PostgreSQL installeren en configureren op een Azure-machine waarop Linux wordt uitgevoerd.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>Install PostgreSQL
> [!NOTE]
> U moet al een Azure virtuele machine waarop Linux wordt uitgevoerd om te kunnen voltooien van deze zelfstudie hebben. Als u wilt maken en instellen van een Linux-VM voordat u doorgaat, Zie de [zelfstudie voor Azure Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

In dit geval poort 1999 gebruiken als de PostgreSQL-poort.  

Verbinding maken met de Linux VM die u hebt gemaakt via PuTTY. Als dit de eerste keer dat u een virtuele Azure Linux-machine, Zie [hoe u kunt SSH gebruiken met Linux op Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over hoe u PuTTY gebruiken om verbinding maken met een Linux-VM.

1. Voer de volgende opdracht uit om over te schakelen naar de hoofdmap (beheerder):
   
        # sudo su -
2. Bepaalde distributies hebben afhankelijkheden die moeten worden geïnstalleerd voor de installatie van PostgreSQL. Controleer voor uw distributie in deze lijst en voer de juiste opdracht uit:
   
   * Red Hat basis-Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian basis-Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. PostgreSQL downloaden naar de hoofdmap en pak vervolgens het pakket:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Het bovenstaande is een voorbeeld. U vindt het downloadadres van de meer gedetailleerde in de [Index/pub/bron /](https://ftp.postgresql.org/pub/source/).
4. Voer deze opdrachten uit voor het starten van de build:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Als u wilt bouwt alles die kunnen worden gebouwd, met inbegrip van de documentatie (HTML en man's) en de aanvullende modules (contrib), in plaats daarvan de volgende opdracht uitvoeren:
   
        # gmake install-world
   
    U moet het volgende bevestigingsbericht ontvangt:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL configureren
1. (Optioneel) Maak een symbolische koppeling voor het verkorten van de PostgreSQL-verwijzing om het versienummer niet bevatten:
   
        # ln -s /opt/postgresql-9.3.5 /opt/pgsql
2. Maak een map voor de database:
   
        # mkdir -p /opt/pgsql_data
3. Een niet-hoofdgebruiker profiel maken en beheren van die gebruiker. Ga vervolgens naar deze nieuwe gebruiker (met de naam *postgres* in ons voorbeeld):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Uit veiligheidsoverwegingen PostgreSQL een niet-hoofdgebruiker gebruikt om te initialiseren, starten of afsluiten van de database.
   > 
   > 
4. Bewerk de *bash_profile* bestand met de onderstaande opdrachten. Deze regels worden toegevoegd aan het einde van de *bash_profile* bestand:
   
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
5. Voer de *bash_profile* bestand:
   
        $ source .bash_profile
6. De installatie valideren met behulp van de volgende opdracht uit:
   
        $ which psql
   
    Als de installatie geslaagd is, ziet u het volgende antwoord:
   
        /opt/pgsql/bin/psql
7. U kunt ook de PostgreSQL-versie controleren:
   
        $ psql -V

8. Initialiseren van de database:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    U ontvangt de volgende uitvoer:

![image](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Instellen van PostgreSQL
<!--    [postgres@ test ~]$ exit -->

Voer de volgende opdrachten uit:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Wijzig de twee variabelen in het bestand /etc/init.d/postgresql. Het voorvoegsel is ingesteld op het pad voor de installatie van PostgreSQL: **/opt/pgsql**. PGDATA is ingesteld op het gegevenspad voor de opslag van de PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/postgresql-install/no2.png)

Het bestand zodat het uitvoerbare bestand wijzigt:

    # chmod +x /etc/init.d/postgresql

Start PostgreSQL:

    # /etc/init.d/postgresql start

Controleer of het eindpunt van de PostgreSQL op:

    # netstat -tunlp|grep 1999

In dat geval moet de volgende uitvoer worden weergegeven:

![image](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Verbinding maken met de database voor Postgres
Overschakelen naar de gebruiker postgres opnieuw:

    # su - postgres

Maak een Postgres-database:

    $ createdb events

Verbinding maken met de database van de gebeurtenissen die u zojuist hebt gemaakt:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Maken en verwijderen van een Postgres-tabel
Nu dat u een verbinding met de database hebt gemaakt, kunt u tabellen maken in het.

Bijvoorbeeld, een nieuwe voorbeeld Postgres-tabel maken met behulp van de volgende opdracht uit:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

U hebt nu een tabel met vier kolommen met de volgende kolomnamen en -beperkingen ingesteld:

1. De kolom "naam" heeft is beperkt door de opdracht VARCHAR onder 20 tekens lang.
2. De kolom 'voeding' geeft aan dat de food-item dat elke persoon die ervoor zorgt dat. VARCHAR beperkt deze tekst als 30 tekens bevatten.
3. De kolom "bevestigde" registreert of de persoon die aan het Amerikaans heeft RSVP'd. De acceptabele waarden zijn "Y" en "N".
4. De 'datum' kolom wordt weergegeven wanneer ze zijn geregistreerd voor de gebeurtenis. Postgres vereist dat de datums worden geschreven als jjjj-mm-dd.

U ziet het volgende als de tabel is gemaakt:

![image](./media/postgresql-install/no4.png)

U kunt ook de tabelstructuur controleren met behulp van de volgende opdracht uit:

![image](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Gegevens toevoegen aan een tabel
Eerst, gegevens invoegen in een rij:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

U ziet deze uitvoer:

![image](./media/postgresql-install/no6.png)

U kunt een aantal meer mensen toevoegen aan de tabel ook. Hier zijn enkele opties of kunt u uw eigen maken:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabellen weergeven
Gebruik de volgende opdracht om een tabel weer te geven:

    select * from potluck;

De uitvoer is:

![image](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Gegevens in een tabel verwijderen
Gebruik de volgende opdracht uit om gegevens in een tabel te verwijderen:

    delete from potluck where name=’John’;

Hiermee verwijdert u alle informatie in de rij 'John'. De uitvoer is:

![image](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Gegevens in een tabel bijwerken
Gebruik de volgende opdracht uit om gegevens in een tabel te werken. Voor deze een heeft Sandy bevestigd dat ze bijwonen, zodat we haar RSVP van "N" naar "Y" wijzigen:

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>Meer informatie over PostgreSQL
Nu dat u de installatie van PostgreSQL in een Azure Linux-VM hebt voltooid, kunt u met plezier gebruiken in Azure. Voor meer informatie over PostgreSQL, gaat u naar de [PostgreSQL website](http://www.postgresql.org/).

