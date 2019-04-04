---
title: TmaxSoft OpenFrame installeren op Azure Virtual Machines
description: Rehost uw IBM z/OS mainframe-workloads op Azure Virtual Machines (VM's) met behulp van TmaxSoft OpenFrame omgeving.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 6b109f347ee7a917b57acfc56ab4418755295bc5
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896375"
---
# <a name="install-tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame installeren op Azure

Meer informatie over het instellen van een omgeving OpenFrame op Azure geschikt voor ontwikkeling, demo's, tests of productie-workloads. In deze zelfstudie helpt u bij elke stap.

OpenFrame bevat meerdere onderdelen die de mainframe-emulatie-omgeving in Azure maken. Bijvoorbeeld, onlineservices OpenFrame vervangen door de mainframe-middleware, zoals IBM klant informatie besturingselement System (CICS) en OpenFrame Batch, met de component TJES, vervangt de IBM-mainframes taak post subsysteem (JES).

OpenFrame werkt met een relationele database, met inbegrip van Oracle Database, Microsoft SQL Server, IBM Db2 en MySQL. Deze installatie van OpenFrame maakt gebruik van de TmaxSoft Tibero relationele database. Zowel OpenFrame en Tibero worden uitgevoerd op een Linux-besturingssysteem. In deze zelfstudie installeert CentOS 7.3, hoewel u andere ondersteunde Linux-distributies kunt gebruiken. De toepassingsserver OpenFrame en de database Tibero zijn geïnstalleerd op een virtuele machine (VM).

De zelfstudie stapsgewijs uitgelegd hoe u de installatie van de onderdelen van de suite OpenFrame. Sommige moeten apart worden geïnstalleerd.

Belangrijkste OpenFrame onderdelen:

- Vereist van installatiepakketten.
- Tibero-database.
- Open Database Connectivity (ODBC) wordt gebruikt door toepassingen in OpenFrame om te communiceren met de database Tibero.
- Basis-OpenFrame, de middleware die het hele systeem beheert.
- Batch-OpenFrame, de oplossing die van de mainframe-batch-systemen worden vervangen.
- TACF, een servicemodule waarmee de gebruikerstoegang tot systemen en bronnen.
- ProSort, een hulpprogramma sorteren voor batchtransacties.
- OFCOBOL, een compiler die programma's van de mainframe-COBOL geïnterpreteerd.
- OFASM, een compiler die van de mainframe-assembler programma's worden geïnterpreteerd.
- OpenFrame Server Type C (OSC), de oplossing die van de mainframe middleware en IBM CICS vervangt.
- Java Enterprise gebruiker oplossing (JEUS), een webserver van de toepassing die is gecertificeerd voor Java Enterprise Edition 6.
- OFGW, het onderdeel OpenFrame gateway waarmee een listener 3270.
- OFManager, een oplossing die van OpenFrame uitvoeren en beheren van functies in de web-omgeving biedt.

Andere vereist OpenFrame onderdelen:

- OSI, de oplossing die wordt vervangen door de mainframe-middleware en IMS DC.
- TJES, de oplossing die van de mainframe JES omgeving biedt.
- OFTSAM, de oplossing waarmee SAM (V)-bestanden moet worden gebruikt in het systeem openen.
- OFHiDB, de oplossing die wordt vervangen door de mainframe's IMS DB.
- OFPLI, een compiler die de mainframe interpreteert de PL / ik programma's.
- PROTRIEVE, een oplossing die de mainframe-taal CA-Easytrieve wordt uitgevoerd.
- OFMiner, een oplossing die analyseert de activa mainframes en ze vervolgens naar Azure migreert.

## <a name="architecture"></a>Architectuur

De volgende afbeelding geeft een overzicht van de architectuur OpenFrame 7.0-onderdelen geïnstalleerd in deze zelfstudie:

![OpenFrame onderdelen](media/openframe-02.png)

## <a name="azure-system-requirements"></a>Vereisten voor Azure

De volgende tabel bevat de vereisten voor de installatie op Azure.
<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Vereiste</th><th>Beschrijving</th></tr>
</thead>
<tbody>
<tr><td>Ondersteunde Linux-distributies op Azure
</td>
<td>
Linux x86 2.6 (32-bits, 64-bits)<br/>
Red Hat 7.x<br/>
CentOS 7.x<br/>
</td>
</tr>
<tr><td>Hardware
</td>
<td>Kernen: 2 (minimaal)<br/>
Geheugen: 4 GB (minimaal)<br/>
Wisselruimte: 1 GB (minimaal)<br/>
Harde schijf: 100 GB (minimaal)<br/>
</td>
</tr>
<tr><td>Optionele software voor Windows-gebruikers
</td>
<td>PuTTY: In deze handleiding wordt gebruikt om functies van de virtuele machine te configureren<br/>
WinSCP: Een populaire SFTP-client en FTP-client kunt u<br/>
Eclipse voor Windows: Een ontwikkelingsplatform ondersteund door TmaxSoft<br/>
(Microsoft Visual Studio wordt niet ondersteund op dit moment)
</td>
</tr>
</tbody>
</table>

<!-- markdownlint-enable MD033 -->

## <a name="prerequisites"></a>Vereisten

Een paar dagen voor het samenstellen van de vereiste software en het voltooien van de handmatige processen uitgaven van plan bent.

Voordat u aan de slag, het volgende doen:

- Ontvang de installatiemedia OpenFrame van TmaxSoft. Als u een bestaande TmaxSoft-klant bent, neem dan contact op met uw vertegenwoordiger TmaxSoft voor een gelicentieerd exemplaar. Vraag een proefversie van anders [TmaxSoft](http://www.tmaxsoft.com/contact/).

- De documentatie OpenFrame aanvragen door te verzenden van e-mailbericht <support@tmaxsoft.com>.

- Een Azure-abonnement krijgen als u er nog geen hebt. U kunt ook maken een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- Optioneel. Instellen van een site-naar-site VPN-tunnel of een jumpbox die de toegang tot de Azure-VM naar de toegestane gebruikers in uw organisatie beperkt. Deze stap is niet vereist, maar het is een best practice.

## <a name="set-up-a-vm-on-azure-for-openframe-and-tibero"></a>Een virtuele machine op Azure voor OpenFrame en Tibero instellen

U kunt de OpenFrame-omgeving met behulp van verschillende implementaties instellen, maar de volgende procedure laat zien hoe de toepassingsserver OpenFrame en de database Tibero op één virtuele machine te implementeren. In grotere omgevingen en voor aanzienlijk werkbelastingen is een aanbevolen procedure voor het implementeren van de database afzonderlijk op een eigen virtuele machine voor betere prestaties.

**Een virtuele machine maken**

1. Ga naar de Azure portal op <http://portal.azure.com> en meld u aan bij uw account.

2. Klik op **virtuele machines**.

    ![Lijst met resources in Azure portal](media/vm-01.png)

3. Klik op **Add**.

    ![De optie toevoegen in Azure portal](media/vm-02.png)

4. Aan de rechterkant van **besturingssystemen**, klikt u op **meer**.

     ![Meer opties in Azure portal](media/vm-03.png)

5. Klik op **op basis van CentOS 7.3** Volg deze procedure exact, omdat, maar u kunt een ander ondersteund Linux-distributie.

     ![Besturingssysteem-opties in Azure portal](media/vm-04.png)

6. In de **basisbeginselen** instellingen, voer **naam**, **gebruikersnaam**, **verificatietype**, **abonnement** (Betalen per gebruik is de AWS-stijl van betaling), en **resourcegroep** (gebruik een bestaande resourcegroep of maak een groep TmaxSoft).

7. Wanneer u klaar bent (met inbegrip van het openbare/persoonlijke sleutelpaar voor **verificatietype**), klikt u op **indienen**.

> [!NOTE]
> Als u een openbare SSH-sleutel voor **verificatietype**, Zie de stappen in de volgende sectie voor het genereren van het openbare/persoonlijke sleutelpaar en vervolgens hervatten van de stappen die hier.

### <a name="generate-a-publicprivate-key-pair"></a>Een openbaar/persoonlijk sleutelpaar genereren

Als u een Windows-besturingssysteem gebruikt, moet u PuTTYgen naar een openbaar/persoonlijk sleutelpaar genereren.

De openbare sleutel naar eigen inzicht kan worden gedeeld, maar de persoonlijke sleutel moet volledig geheime worden bewaard en mag nooit worden gedeeld met een andere partij. Na het genereren van de sleutels, plakt u de **openbare SSH-sleutel** in de configuratie, in feite uploaden naar de Linux-VM. Deze zijn opgeslagen binnen geautoriseerd\_sleutels binnen de \~/.ssh directory van de basismap van het gebruikersaccount. De Linux-VM kan vervolgens te valideren van de verbinding wanneer u de bijbehorende bieden **persoonlijke SSH-sleutel** in de SSH-client (in ons geval PuTTY).

Wanneer u nieuwe personen toegang tot de virtuele machine: 

- Elke nieuwe afzonderlijke genereert hun eigen openbare/persoonlijke sleutels met behulp van PuTTYgen.
- Personen hun eigen persoonlijke sleutels afzonderlijk opslaan en verzenden van gegevens van de openbare sleutel naar de beheerder van de virtuele machine.
- De beheerder de inhoud van de openbare sleutel die moet worden geplakt de \~/.ssh/authorized\_sleutelsbestand.
- De nieuwe gebruiker verbinding maakt via PuTTY.

**Een openbaar/persoonlijk sleutelpaar genereren**

1.  Download PuTTYgen uit <https://www.putty.org/> en installeer deze met de standaardinstellingen.

2.  Als u wilt openen PuTTYgen, zoekt u de PuTTY-installatiemap in C:\\Program Files\\PuTTY.

    ![PuTTY-interface](media/puttygen-01.png)

3.  Klik op **genereren**.

    ![In het dialoogvenster van puTTY-Sleutelgenerator](media/puttygen-02.png)

4.  Nadat de generatie van opslaan voor de openbare sleutel en de persoonlijke sleutel. Plak de inhoud van de openbare sleutel in de **openbare SSH-sleutel** sectie van de **maken van virtuele machine \> basisbeginselen** deelvenster (weergegeven in stap 6 en 7 in de vorige sectie).

    ![In het dialoogvenster van puTTY-Sleutelgenerator](media/puttygen-03.png)

### <a name="configure-vm-features"></a>Functies van de virtuele machine configureren

1. In Azure-portal in de **Kies een grootte** blade, kiest u de Linux machine hardware-instellingen u wilt. De *minimale* vereisten voor het installeren van zowel Tibero en OpenFrame zijn 2 CPU's en 4 GB RAM-geheugen, zoals wordt weergegeven in de installatie van dit voorbeeld:

    ![Maken van virtuele machine - basisbeginselen](media/create-vm-01.png)

2. Klik op **3 instellingen** en de standaardinstellingen gebruiken om optionele functies te configureren.
3. Controleer uw betalingsgegevens.

    ![Maken van virtuele machine - aankoop](media/create-vm-02.png)

4. Dien uw selecties. Azure begint met het implementeren van de virtuele machine. Dit proces duurt doorgaans enkele minuten.

5. Wanneer de virtuele machine wordt geïmplementeerd, wordt het dashboard ervan weergegeven, met de instellingen die zijn geselecteerd tijdens de configuratie. Noteer de **openbaar IP-adres**.

    ![Tmax in Azure-dashboard](media/create-vm-03.png)

6. Open PuTTY.

7. Voor **hostnaam**, typt u uw gebruikersnaam en het openbare IP-adres dat u hebt gekopieerd. Bijvoorbeeld, **gebruikersnaam\@publicip**.

    ![Dialoogvenster voor puTTY-configuratie](media/putty-01.png)

8. In de **categorie** Klik **verbinding \> SSH \> Auth**. Geef het pad naar uw **privésleutel** bestand.

    ![Dialoogvenster voor puTTY-configuratie](media/putty-02.png)

9. Klik op **Open** het PuTTY-venster te starten. Als dit lukt, kunt u bent verbonden met uw nieuwe CentOS-VM wordt uitgevoerd op Azure.

10. Als u wilt aanmelden als hoofdgebruiker, typt u **sudo bash**.

    ![Aanmelding van de gebruiker in het opdrachtvenster hoofdmap](media/putty-03.png)

## <a name="set-up-the-environment-and-packages"></a>Instellen van de omgeving en pakketten

Nu dat de virtuele machine is gemaakt en u bent aangemeld, moet u een paar stappen van setup uitvoeren en de vereiste pakketten vooraf geïnstalleerd.

1. De naam toewijzen **ofdemo** naar het lokale IP-adres met behulp van vi bewerken van het hosts-bestand (`vi /etc/hosts`). Ervan uitgaande dat ons IP is 192.168.96.148 ofdemo, dit is voordat de wijziging:

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    <IP Address>    <your hostname>
    ```

     Dit is na de wijziging:

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    192.168.96.148   ofdemo
    ```

2. Groepen en gebruikers maken:

    ```vi
    [root@ofdemo ~]# adduser -d /home/oframe7 oframe7 
    [root@ofdemo ~]# passwd oframe7
    ```

3. Het wachtwoord voor gebruiker oframe7 wijzigen:

    ```vi
    New password: 
    Retype new password: 
    passwd: all authentication tokens updated successfully.
    ```

4. Werk de parameters van de kernel in /etc/sysctl.conf:

    ```vi
    [root@ofdemo ~]# vi /etc/sysctl.conf
    kernel.shmall = 7294967296 
    kernel.sem = 10000 32000 10000 10000
    ```

5. Vernieuw de kernel-parameters dynamisch zonder opnieuw opstarten:

    ```vi
    [root@ofdemo ~]# /sbin/sysctl -p
    ```

6. Haal de vereiste pakketten: Zorg ervoor dat de server is verbonden met Internet, de volgende pakketten downloaden en installeer ze dan:

     - dos2unix
     - glibc
     - glibc.i686 glibc.x86\_64
     - libaio
     - ncurses

          > [!NOTE]
          > Na de installatie van het pakket ncurses, maken de volgende symbolische koppelingen:
         ```
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so.2
         ```

     - gcc
     - gcc-c++
     - libaio-devel.x86\_64
     - strace
     - ltrace
     - gdb

7. In het geval van Java-RPM-installatie, het volgende doen:

```
root@ofdemo ~]# rpm -ivh jdk-7u79-linux-x64.rpm
[root@ofdemo ~]# vi .bash_profile

# JAVA ENV
export JAVA_HOME=/usr/java/jdk1.7.0_79/
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=$CLASSPATH:$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar

[root@ofdemo ~]# source /etc/profile
[root@ofdemo ~]# java –version

java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)

[root@ofdemo ~]# echo $JAVA_HOME /usr/java/jdk1.7.0_79/
```

## <a name="install-the-tibero-database"></a>Installatie van de database Tibero

Tibero biedt de verschillende belangrijke functies in de omgeving OpenFrame op Azure:

- Tibero wordt gebruikt als de intern gegevensarchief OpenFrame voor verschillende systeemfuncties.
- VSAM bestanden, met inbegrip van KSDS RRDS en ESDS, intern gebruikmaken van de database Tibero voor gegevensopslag.
- De gegevensopslagplaats TACF wordt opgeslagen in Tibero.
- De catalogusgegevens OpenFrame wordt opgeslagen in Tibero.
- De database Tibero kan worden gebruikt als vervanging voor IBM Db2 voor het opslaan van toepassingsgegevens.

**Voor het installeren van Tibero**

1. Controleer of het Tibero binaire installer-bestand aanwezig is en controleer het versienummer.
2. Kopieer de software Tibero aan het gebruikersaccount Tibero (oframe). Bijvoorbeeld:

    ```
    [oframe7@ofdemo ~]$ tar -xzvf tibero6-bin-6_rel_FS04-linux64-121793-opt-tested.tar.gz 
    [oframe7@ofdemo ~]$ mv license.xml /opt/tmaxdb/tibero6/license/
    ```

3. Open .bash\_profiel in vi (`vi .bash_profile`) en plak het volgende erin:

    ```
    # Tibero6 ENV
    export TB_HOME=/opt/tmaxdb/tibero6 
    export TB_SID=TVSAM export TB_PROF_DIR=$TB_HOME/bin/prof 
    export LD_LIBRARY_PATH=$TB_HOME/lib:$TB_HOME/client/lib:$LD_LIBRARY_PATH 
    export PATH=$TB_HOME/bin:$TB_HOME/client/bin:$PATH
    ```

4. Voor het uitvoeren van de bash-profiel, bij de opdrachtprompt te typen:

    ```
    source .bash_profile
    ```

5. De tip-bestand (een configuratiebestand voor Tibero) en vervolgens opent u het in vi. Bijvoorbeeld:

    ```
    [oframe7@ofdemo ~]$ sh $TB_HOME/config/gen_tip.sh
    [oframe7@ofdemo ~]$ vi $TB_HOME/config/$TB_SID.tip
    ```

6. Wijzigen \$TB\_HOME/client/config/tbdsn.tbr en 127.0.0.1 in plaats daarvan oflocalhost zoals wordt weergegeven:

    ```
    TVSAM=( 
    (INSTANCE=(HOST=127.0.0.1)
        (PT=8629)
    (DB_NAME=TVSAM)
          )
     )
    ```

7. Maken van de database. De volgende uitvoer wordt weergegeven:

    ```
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NOMOUNT mode).
     /--------------------- newmount sql ------------------------/
    create database character set MSWIN949 national character set UTF16;
    /-----------------------------------------------------------/
    Database created.
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    /opt/tmaxdb/tibero6/bin/tbsvr
    ………………………..
    Creating agent table...
    Done.
    For details, check /opt/tmaxdb/tibero6/instance/TVSAM/log/system_init.log.
    ************************************************** 
    * Tibero Database TVSAM is created successfully on Fri Aug 12 19:10:43 UTC 2016.
    *     Tibero home directory ($TB_HOME) =
    *         /opt/tmaxdb/tibero6
    *     Tibero service ID ($TB_SID) = TVSAM
    *     Tibero binary path =
    *         /opt/tmaxdb/tibero6/bin:/opt/tmaxdb/tibero6/client/bin
    *     Initialization parameter file =
    *         /opt/tmaxdb/tibero6/config/TVSAM.tip
    * 
    * Make sure that you always set up environment variables $TB_HOME and
    * $TB_SID properly before you run Tibero.
     ******************************************************************************
    ```

8. Als u wilt recyclen Tibero, eerst afsluiten met behulp van de `tbdown` opdracht. Bijvoorbeeld:

    ```
    [oframe7@ofdemo ~]$$ tbdown 
    Tibero instance terminated (NORMAL mode).
    ```

9. Nu starten met behulp van Tibero `tbboot`. Bijvoorbeeld:

    ```
    [oframe7@ofdemo ~]$ tbboot
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof. Listener port = 8629

    Tibero 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    ```

10. Voor het maken van een tabelruimte, toegang krijgen tot de database met behulp van SYS gebruiker (sys/tmax), klikt u vervolgens de tabelruimte die nodig zijn voor de standaardvolume en TACF maken:

    ```
    [oframe7@ofdemo ~]$ tbsql tibero/tmax
    tbSQL 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Connected to Tibero.
    ```

11. Typ nu het volgende SQL-opdrachten:

    ```
    SQL> create tablespace "DEFVOL" datafile 'DEFVOL.dbf' size 500M autoextend on; create tablespace "TACF00" datafile 'TACF00.dbf' size 500M autoextend on; create tablespace "OFM_REPOSITORY" datafile 'ofm_repository.dbf' size 300M autoextend on;
    SQL> Tablespace 'DEFVOL' created.
    SQL> Tablespace 'TACF00' created.
    SQL> Tablespace ' OFM_REPOSITORY ' created.
    SQL> SQL> Disconnected.
    ```

12. Tibero opstarten en controleer of de Tibero processen zijn uitgevoerd:

    ```
    [oframe7@ofdemo ~]$ tbboot 
    ps -ef | egrep tbsvr
    ```

Uitvoer:

![Tibero uitvoer](media/tibero-01.png)

## <a name="install-odbc"></a>Installeren van ODBC

Toepassingen in OpenFrame communiceren met de Tibero-database met behulp van de ODBC-API die door de open-source unixODBC-project.

Voor het installeren van ODBC:

1. Controleer of het unixODBC 2.3.4.tar.gz installer-bestand aanwezig is, of gebruik de `wget unixODBC-2.3.4.tar.gz` opdracht. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ wget ftp://ftp.unixodbc.org/pub/unixODBC/unixODBC-2.3.4.tar.gz
     ```

2. Pak het binaire bestand. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ tar -zxvf unixODBC-2.3.4.tar.gz
     ```

3. Ga naar de map unixODBC 2.3.4 en de makefile te genereren met behulp van de gegevens van de controle. Bijvoorbeeld:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ ./configure --prefix=/opt/tmaxapp/unixODBC/ --sysconfdir=/opt/tmaxapp/unixODBC/etc
     ```

     Standaard unixODBC is geïnstalleerd in/usr/local, dus `--prefix` geeft een waarde om de locatie te wijzigen. Op deze manier-configuratiebestanden worden standaard geïnstalleerd in/etc, dus `--sysconfdir` geeft de waarde van de gewenste locatie.

4. Makefile te worden uitgevoerd: `[oframe7@ofdemo unixODBC-2.3.4]$ make`

5. Kopieer het uitvoerbare bestand in de programmamap na compileren. Bijvoorbeeld:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ make install
     ```

6. Vi gebruiken om de bash-profiel te bewerken (`vi ~/.bash_profile`) en voeg de volgende:

     ```
     # UNIX ODBC ENV 
     export ODBC_HOME=$HOME/unixODBC 
     export PATH=$ODBC_HOME/bin:$PATH 
     export LD_LIBRARY_PATH=$ODBC_HOME/lib:$LD_LIBRARY_PATH 
     export ODBCINI=$HOME/unixODBC/etc/odbc.ini 
     export ODBCSYSINI=$HOME
     ```

7. De ODBC van toepassing. Bewerk de volgende bestanden dienovereenkomstig. Bijvoorbeeld:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ source ~/.bash_profile

     [oframe7@ofdemo ~]$ cd

     [oframe7@ofdemo ~]$ odbcinst -j unixODBC 2.3.4
     DRIVERS............: /home/oframe7/odbcinst.ini
     SYSTEM DATA SOURCES: /home/oframe7/odbc.ini
     FILE DATA SOURCES..: /home/oframe7/ODBCDataSources
     USER DATA SOURCES..: /home/oframe7/unixODBC/etc/odbc.ini
     SQLULEN Size.......: 8
     SQLLEN Size........: 8
     SQLSETPOSIROW Size.: 8

     [oframe7@ofdemo ~]$ vi odbcinst.ini

     [Tibero]
     Description = Tibero ODBC driver for Tibero6
     Driver = /opt/tmaxdb/tibero6/client/lib/libtbodbc.so
     Setup = 
     FileUsage = 
     CPTimeout = 
     CPReuse = 
     Driver Logging = 7

     [ODBC]
     Trace = NO 
     TraceFile = /home/oframe7/odbc.log 
     ForceTrace = Yes 
     Pooling = No 
     DEBUG = 1

     [oframe7@ofdemo ~]$ vi odbc.ini

     [TVSAM]
     Description = Tibero ODBC driver for Tibero6 
     Driver = Tibero 
     DSN = TVSAM 
     SID = TVSAM 
     User = tibero 
     password = tmax
     ```

8. Maken van een symbolische koppeling en valideren van de databaseverbinding Tibero:

     ```
     [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbc.so $ODBC_HOME/lib/libodbc.so.1 [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbcinst.so 
     $ODBC_HOME/lib/libodbcinst.so.1

     [oframe7@ofdemo lib]$ isql TVSAM tibero tmax
     ```

De volgende uitvoer wordt weergegeven:

![ODBC-uitvoer weergeven die zijn verbonden met SQL](media/odbc-01.png)

## <a name="install-openframe-base"></a>Installatiebasis voor OpenFrame

De basis-toepassingsserver is geïnstalleerd voordat u de afzonderlijke services ervan die OpenFrame wordt gebruikt voor het beheren van het systeem op Azure, met inbegrip van de transactie serverprocessen verwerken.

**OpenFrame Base installeren**

1. Controleer of de Tibero-installatie is voltooid en controleer de volgende OpenFrame\_Base7\_0\_Linux\_x86\_64. bin installer-bestand en het configuratiebestand base.properties aanwezig zijn.

2. De bash-profiel met de volgende Tibero-specifieke informatie bijwerken:

     ```bash
     alias ofhome='cd $OPENFRAME_HOME'
     alias ulog='cd $OPENFRAME_HOME/log/tmax/ulog'
     alias sysjcl='cd $OPENFRAME_HOME/volume_default/SYS1.JCLLIB'
     alias sysload='cd $OPENFRAME_HOME/volume_default/SYS1.LOADLIB'
     alias sysproc='cd $OPENFRAME_HOME/volume_default/SYS1.PROCLIB'
     alias oscsrc='cd $OPENFRAME_HOME/osc/oivp'
     alias osisrc='cd $OPENFRAME_HOME/osi/oivp'
     alias defvol='cd $OPENFRAME_HOME/volume_default'
     ```

3. Voer de bash-profiel:`[oframe7@ofdemo ~]$ . .bash_profile`
4. Zorg ervoor dat de Tibero processen worden uitgevoerd. Bijvoorbeeld:

     ```linux
     [oframe7@ofdemo ~]$ ps -ef|grep tbsvr
     ```

    ![Basis](media/base-01.png)

     > [!IMPORTANT]
     > Zorg ervoor dat u Tibero voordat de installatie start.

5. Genereren van de licentie op [technet.tmaxsoft.com](https://technet.tmaxsoft.com/en/front/main/main.do) en PLAATST het OpenFrame Base, Batch, TACF, OSC-licenties in de juiste map:

     ```
     [oframe7@ofdemo ~]$ cp license.dat /opt/tmaxapp/OpenFrame/core/license/
     [oframe7@ofdemo ~]$ cp lictjes.dat lictacf.dat licosc.dat $OPENFRAME_HOME/license/
     ```

6. De OpenFrame Base base.properties van binaire bestanden en bestanden te downloaden:

     ```
     [oframe7@ofdemo ~]$ vi base.properties
     OPENFRAME_HOME= <appropriate location for installation> ex. /opt/tmaxapp/OpenFrame TP_HOST_NAME=<your IP Hostname> ex. ofdemo 
     TP_HOST_IP=<your IP Address> ex. 192.168.96.148 
     TP_SHMKEY=63481 
     TP_TPORTNO=6623 
     TP_UNBLOCK_PORT=6291 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     MASCAT_NAME=SYS1.MASTER.ICFCAT 
     MASCAT_CREATE=YES 
     DEFAULT_VOLSER=DEFVOL 
     VOLADD_DEFINE=YES TSAM_USERNAME=tibero 
     TSAM_PASSWORD=tmax 
     TSAM_DATABASE=oframe 
     DATASET_SHMKEY=63211 
     DSLOCK_DATA=SYS1.DSLOCK.DATA 
     DSLOCK_LOG=SYS1.DSLOCK.LOG 
     DSLOCK_SEQ=dslock_seq.dat 
     DSLOCK_CREATE=YES 
     OPENFRAME_LICENSE_PATH=/opt/tmaxapp/license/OPENFRAME TMAX_LICENSE_PATH=/opt/tmaxapp/license/TMAX
     ```

7. Het installatieprogramma uit met het bestand base.properties worden uitgevoerd. Bijvoorbeeld:

    ```
    [oframe7@ofdemo ~]$ chmod a+x OpenFrame_Base7_0_Linux_x86_64.bin 
    [oframe7@ofdemo ~]$ ./OpenFrame_Base7_0_Linux_x86_64.bin -f base.properties
    ```

    Als u klaar bent, is de volledige installatie bericht weergegeven.

8. Controleer of de OpenFrame Base directory structuur via de `ls -ltr` opdracht. Bijvoorbeeld:

     ```
     [oframe7@ofdemo OpenFrame]$ ls -ltr
     total 44

     drwxrwxr-x. 4 oframe7 oframe7 61 Nov 30 16:57 UninstallerData 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 bin 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 cpm drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 data 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 include 
     drwxrwxr-x. 2 oframe7 oframe7 8192 Nov 30 16:57 lib 
     drwxrwxr-x. 6 oframe7 oframe7 48 Nov 30 16:57 log 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 profile 
     drwxrwxr-x. 7 oframe7 oframe7 62 Nov 30 16:57 sample 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 schema 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 temp 
     drwxrwxr-x. 3 oframe7 oframe7 16 Nov 30 16:57 shared 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 license 
     drwxrwxr-x. 23 oframe7 oframe7 4096 Nov 30 16:58 core 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 config 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 scripts 
     drwxrwxr-x. 2 oframe7 oframe7 25 Nov 30 16:58 volume_default
     ```

9. Start OpenFrame Base:

     ```
     [oframe7@ofdemo ~]$ cp /usr/lib/libtermcap.so.2 $TMAXDIR/lib
     Startup Tmax Server
     [oframe7@ofdemo ~]$ tmboot
     ```

     ![uitvoer van de opdracht tmboot](media/base-02.png)

10. Controleer of dat de processtatus is klaar met de opdracht tmadmin in si. RDY wordt weergegeven in de **status** kolom voor elk van de processen:

     ![uitvoer van de opdracht tmadmin](media/base-03.png)

11. OpenFrame Base afsluiten:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(vtammgr:43) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLH downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLL downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TLM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-openframe-batch"></a>OpenFrame Batch installeren

OpenFrame Batch bestaat uit verschillende onderdelen die mainframe-batch-omgeving simuleren en wordt gebruikt voor het batch-taken uitvoeren op Azure.

**Voor het installeren van Batch**

1. Zorg ervoor dat de basisinstallatie is voltooid en vervolgens controleren of de OpenFrame\_Batch7\_0\_Fix2\_MVS\_Linux\_x86\_64. bin installer-bestand en het configuratiebestand batch.Properties aanwezig zijn:

2. Typ het volgende achter de opdrachtprompt `vi batch.properties` het batch.properties-bestand met behulp van vi bewerken.

3. De parameters als volgt wijzigen:

     ```
     OPENFRAME_HOME = /opt/tmaxapp/OpenFrame
     DEFAULT_VOLSER=DEFVOL 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     RESOURCE_SHMKEY=66991 
     #JOBQ_DATASET_CREATE=YES 
     #OUTPUTQ_DATASET_CREATE=YES 
     DEFAULT_JCLLIB_CREATE=YES 
     DEFAULT_PROCLIB_CREATE=YES 
     DEFAULT_USERLIB_CREATE=YES 
     TJES_USERNAME=tibero 
     TJES_PASSWORD=tmax 
     TJES_DATABASE=oframe 
     BATCH_TABLE_CREATE=YES
     ```

4. Het installatieprogramma voor de batch achter de opdrachtprompt: uitvoeren:

     ```
     ./OpenFrame_Batch7_0_Fix2_MVS_Linux_x86_64.bin -f batch.properties
     ```

5. Wanneer de installatie voltooid is, start u de geïnstalleerde OpenFrame-pakketten door te typen `tmboot` bij de opdrachtprompt.

    ![tmboot uitvoer](media/tmboot-01.png)

6. Type `tmadmin` bij de opdrachtprompt om te controleren of het proces OpenFrame.

    ![Tmax Admin scherm](media/tmadmin-01.png)

7. Voer de volgende opdrachten uit:

     ```
     $$2 NODE1 (tmadm): quit 
     ADM quit for node (NODE1)
     ```

8. Gebruik de `tmdown` opdracht voor het opstarten en afsluiten Batch:

     ```
     [oframe7@ofdemo ~]$tmdown
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:44) downed: Wed Sep  7 16:01:46 2016
     TMDOWN: SERVER(vtammgr: 43) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:45) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:46) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjinit:55) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:49) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:51) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:52) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:53) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLL downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TLM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLH downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-tacf"></a>TACF installeren

TACF Manager is een service-module voor OpenFrame die gebruikerstoegang tot systemen en -resources via RACF beveiliging regelt.

**Voor het installeren van TACF**

1. Controleer de OpenFrame\_Tacf7\_0\_Fix2\_Linux\_x86\_64. bin installer-bestand en het configuratiebestand tacf.properties aanwezig zijn.
2. Zorg ervoor dat de Batch-installatie is voltooid en vervolgens vi gebruiken om de tacf.properties-bestand te openen (`vi tacf.properties`).
3. Wijzig de TACF parameters:

     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame 
     USE_OS_AUTH=NO 
     TACF_USERNAME=tibero 
     TACF_PASSWORD=tmax 
     TACF_DATABASE=oframe 
     TACF_TABLESPACE=TACF00 
     TACF_TABLE_CREATE=YES
     ```

4. Na het voltooien van TACF installer, zijn van toepassing de omgevingsvariabelen TACF. Typ in de opdrachtprompt:

     ```
     source \~/.bash\_profile
     ```

5. Het installatieprogramma TACF worden uitgevoerd. Typ in de opdrachtprompt:

     ```
     ./OpenFrame_Tacf7_0_Fix2_Linux_x86_64.bin -f tacf.properties
     ```

     De uitvoer ziet er ongeveer als volgt uit:

     ```
     Wed Dec 07 17:36:42 EDT 2016
     Free Memory: 18703 kB 
     Total Memory: 28800 kB

     4 Command Line Args: 
     0:  -f 1:  tacf.properties 
     2:  -m 
     3:  SILENT 
     java.class.path: 
     /tmp/install.dir.41422/InstallerData 
     /tmp/install.dir.41422/InstallerData/installer.zip 
     ZGUtil.CLASS_PATH: 
     /tmp/install.dir.41422/InstallerData 
     tmp/install.dir.41422/InstallerData/installer.zip 
     sun.boot.class.path: 
     /tmp/install.dir.41422/Linux/resource/jre/lib/resources.jar /tmp/install.dir.41422/Linux/resource/jre/lib/rt.jar /tmp/install.dir.41422/Linux/resource/jre/lib/sunrsasign.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jsse.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jce.jar /tmp/install.dir.41422/Linux/resource/jre/lib/charsets.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jfr.jar /tmp/install.dir.41422/Linux/resource/jre/classes
     ```

6. Typ het volgende achter de opdrachtprompt `tmboot` OpenFrame opnieuw starten. De uitvoer ziet er ongeveer als volgt uit:

     ```
     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/7 
     TMBOOT: TMM is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLL is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLH is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: TLM(tlm) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsasvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrlhsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdsedt) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrcmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofruisvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsmlog) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(vtammgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjschd) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjinit) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjhist) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjspbk) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrpmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmtsmgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(tmsvr) is starting: Wed Sep  7 17:48:53 2016
     ```

7. Controleer of de processtatus is klaar met behulp van `tmadmin` in de `si` opdracht. Bijvoorbeeld:

     ```
     [oframe7\@ofdemo \~]\$ tmadmin
     ```

     In de **status** kolom RDY wordt weergegeven:

    ![RDY in de statuskolom](media/tmboot-02.png)

8. Voer de volgende opdrachten uit:

     ```
     $$2 NODE1 (tmadm): quit 
     DM quit for node (NODE1)

     [oframe7@ofdemo ~]$ tacfmgr 
     Input USERNAME  : ROOT 
     Input PASSWORD  : SYS1

     TACFMGR: TACF MANAGER START!!!
     QUIT TACFMGR: TACF MANAGER END!!!

     [oframe7@ofdemo ~]$ tmdow
     ```

9. Sluit de server met behulp van de `tmdown` opdracht. De uitvoer ziet er ongeveer als volgt uit:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(tmsvr:60) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLL downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLH downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TLM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-prosort"></a>ProSort installeren

ProSort is een hulpprogramma dat wordt gebruikt in batchtransacties voor het sorteren van gegevens.

**Voor het installeren van ProSort**

1. Zorg ervoor dat de Batch-installatie is voltooid en controleer vervolgens de **prosort bin prosort\_2sp3 linux64-2123 opt.tar.gz** installer-bestand aanwezig is.

2. Het installatieprogramma uit met het eigenschappenbestand worden uitgevoerd. Typ in de opdrachtprompt:

     ```
     tar -zxvf prosort-bin-prosort\_2sp3-linux64-2123-opt.tar.gz
     ```

3. De prosort map verplaatsen naar de oorspronkelijke locatie. Typ in de opdrachtprompt:

     ```
     mv prosort /opt/tmaxapp/prosort
     ```

4. Maak een submap licentie en kopieer er het licentiebestand. Bijvoorbeeld:

     ```
     cd /opt/tmaxapp/prosort 
     mkdir license 
     cp /opt/tmaxsw/oflicense/prosort/license.xml /opt/tmaxapp/prosort/license
     ```

5. Bash.profile openen in vi (`vi .bash_profile`) en als volgt bijwerken:

     ```bash
     #       PROSORT

     PROSORT_HOME=/opt/tmaxapp/prosort 
     PROSORT_SID=gbg 
     PATH=$PATH:$PROSORT_HOME/bin LD_LIBRARY_PATH=$PROSORT_HOME/lib:$LD_LIBRARY_PATH LIBPATH$PROSORT_HOME/lib:$LIBPATH 
     export PROSORT_HOME PROSORT_SID 
     PATH LD_LIBRARY_PATH LIBPATH 
     PATH=$PATH:$OPENFRAME_HOME/shbin 
     export PATH
     ```

6. Voor het uitvoeren van de bash-profiel, bij de opdrachtprompt, typt u: ` . .bash_profile`

7. Het configuratiebestand maken. Bijvoorbeeld:

     ```
     oframe@oframe7: cd /opt/tmaxapp/prosort/config 
     oframe@oframe7: ./gen_tip.sh 
     Using PROSORT_SID "gbg"
      /home/oframe7/prosort/config/gbg.tip generated
     ```

8. Maakt de symbolische koppeling. Bijvoorbeeld:

     ```
     oframe@oframe7: cd /opt/tmaxapp/OpenFrame/util/ 
     oframe@oframe7home/oframe7/OpenFrame/util :  ln -s DFSORT SORT
     ```

9. De ProSort installatie controleren door het uitvoeren van de `prosort -h` opdracht. Bijvoorbeeld:

     ```
     oframe@oframe7: prosort -h

     Usage: prosort [options] [sort script files]
     options ------
     -h             Display this information 
     -v             Display version information 
     -s             Display state information 
     -j             Display profile information 
     -x             Use SyncSort compatible mode
     ```

## <a name="install-ofcobol"></a>OFCOBOL installeren

OFCOBOL is de compiler OpenFrame die programma's van de mainframe-COBOL geïnterpreteerd. 

**Voor het installeren van OFCOBOL**

1. Zorg ervoor dat de Batch/Online-installatie is voltooid en vervolgens controleren of de OpenFrame\_COBOL3\_0\_40\_Linux\_x86\_64. bin installer-bestand aanwezig is.

2. Voor het uitvoeren van het installatieprogramma voor de OFCOBOL bij de opdrachtprompt, typt u:

     ```
      ./OpenFrame\_COBOL3\_0\_40\_Linux\_x86\_64.bin
     ```

3. Lees de gebruiksrechtovereenkomst en druk op Enter om door te gaan.

4. Accepteer de gebruiksrechtovereenkomst. Wanneer de installatie voltooid is, wordt het volgende weergegeven:

     ```
     Choose Install Folder 
     --------------------
     Where would you like to install?
     Default Install Folder: /home/oframe7/OFCOBOL

     ENTER AN ABSOLUTE PATH, OR PRESS <ENTER> TO ACCEPT THE DEFAULT : /opt/tmaxapp/OFCOBOL

     INSTALL FOLDER IS: /opt/tmaxapp/OFCOBOL 
     IS THIS CORRECT? (Y/N): Y[oframe7@ofdemo ~]$ vi .bash_profile

     ============================================================================ Installing... 
     ------------
     [==================|==================|==================|==================] 
     [------------------|------------------|------------------|------------------]

     =============================================================================== Installation Complete 
     --------------------
     Congratulations. OpenFrame_COBOL has been successfully installed
     PRESS <ENTER> TO EXIT THE INSTALLER
     ```

5. Open de bash-profiel in vi (`vi .bash_profile`) en controleer of die is bijgewerkt met OFCOBOL variabelen.
6. Voer de bash-profiel. Typ in de opdrachtprompt:

     ```
      source ~/.bash_profile
     ```

7. De licentie OFCOBOL kopiëren naar de map is geïnstalleerd. Bijvoorbeeld:
     ```
     mv licofcob.dat $OFCOB_HOME/license
     ```
8. Ga naar het configuratiebestand van OpenFrame tjclrun.conf en opent u het in vi. Bijvoorbeeld:
     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

   Hier volgt de sectie SYSLIB voordat de wijziging:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib
     ```
   Hier volgt de sectie SYSLIB na de wijziging:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib :${ODBC_HOME}/lib 
     :${OFCOB_HOME}/lib
     ```
9. Bekijk de OpenFrame\_COBOL\_InstallLog.log bestand in vi en controleer of dat er geen fouten zijn. Bijvoorbeeld:
     ```
     [oframe7@ofdemo ~]$ vi $OFCOB_HOME/UninstallerData/log/OpenFrame_COBOL_InstallLog.log 
     …….. 
     Summary 
     ------
     Installation: Successful. 
     131 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalError
     ```
10. Gebruik de `ofcob --version ` beheren en controleren van het versienummer om de installatie te controleren. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ ofcob --version 
     OpenFrame COBOL Compiler 3.0.54 
     CommitTag:: 645f3f6bf7fbe1c366a6557c55b96c48454f4bf
     ```

11. Opnieuw opstarten met behulp van OpenFrame de `tmdown/tmboot` opdracht.

## <a name="install-ofasm"></a>OFASM installeren

OFASM is de compiler OpenFrame die van de mainframe-assembler programma's worden geïnterpreteerd.

**Voor het installeren van OFASM**

1. Zorg ervoor dat de Batch/Online-installatie is voltooid en vervolgens controleren of de OpenFrame\_ASM3\_0\_Linux\_x86\_64. bin installer-bestand aanwezig is.

2. Het installatieprogramma worden uitgevoerd. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ ./OpenFrame_ASM3_0_Linux_x86_64.bin
     ```

3. Lees de gebruiksrechtovereenkomst en druk op Enter om door te gaan.
4. Accepteer de gebruiksrechtovereenkomst.
5. Controleer of dat de bash-profiel is bijgewerkt met OFASM variabelen. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ source .bash_profile
     [oframe7@ofdemo ~]$ ofasm --version 
     # TmaxSoft OpenFrameAssembler v3 r328 
     (3ff35168d34f6e2046b96415bbe374160fcb3a34)

     [oframe7@ofdemo OFASM]$ vi .bash_profile

     # OFASM ENV 
     export OFASM_HOME=/opt/tmaxapp/OFASM 
     export OFASM_MACLIB=$OFASM_HOME/maclib/free_macro 
     export PATH="${PATH}:$OFASM_HOME/bin:" 
     export LD_LIBRARY_PATH="./:$OFASM_HOME/lib:$LD_LIBRARY_PATH"
     ```

6. Open het configuratiebestand van OpenFrame tjclrun.conf in vi en bewerk deze als volgt:

     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

     Hier volgt de sectie [SYSLIB] *voordat* de wijziging:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib
     ```

     Hier volgt de sectie [SYSLIB] *nadat* de wijziging:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib:${OFASM_HOME}/lib
     ```

7. Open de OpenFrame\_ASM\_InstallLog.log bestand in vi en controleer of dat er geen fouten zijn. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ vi 
     $OFASM_HOME/UninstallerData/log/OpenFrame_ASM_InstallLog.log 
     …….. 
     Summary 
     ------

     Installation: Successful.

     55 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalErrors
     ```

8. Opnieuw opstarten OpenFrame door uitgifte van een van de volgende opdrachten:

     ```
     tmdown / tmboot
     ```

     — of:

     ```
     oscdown / oscboot
     ```

## <a name="install-osc"></a>OSC installeren

OSC is de OpenFrame-omgeving die vergelijkbaar is met IBM CICS die ondersteuning biedt voor snelle OLTP-transacties en andere functies voor beheer.

**OSC installeren**

1. Zorg ervoor dat de basisinstallatie is voltooid en vervolgens controleren of de OpenFrame\_OSC7\_0\_Fix2\_Linux\_x86\_64. bin installer-bestand en het configuratiebestand osc.properties zijn aanwezig zijn.
2. De volgende parameters in het bestand osc.properties bewerken:
     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame OSC_SYS_OSC_NCS_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_NCS OSC_APP_OSC_TC_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_TC
     ```

3. Voer het installatieprogramma uit met het eigenschappenbestand, zoals wordt weergegeven:

     ```
     [oframe7@ofdemo ~]$ chmod a+x OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin [oframe7@ofdemo ~]$ ./OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin -f osc.properties
     ```

     Wanneer u klaar bent, wordt het bericht 'Installatie voltooid' weergegeven.

4. Controleren of de bash-profiel is bijgewerkt met OSC variabelen.
5. Bekijk de OpenFrame\_OSC7\_0\_Fix2\_InstallLog.log-bestand. Het ziet er ongeveer als volgt uit:

     ```
     Summary 
     ------ 
     Installation: Successful.

     233 Successes
     0 Warnings
     0 NonFatalErrors
     0 FatalError
     ```

6. Vi gebruiken om de configuratie van ofsys.seq bestand te openen. Bijvoorbeeld:

     ```
     vi $OPENFRAME_HOME/config/ofsys.seq
     ```

7. In de \#BASE en \#secties BATCH, wijzigt u de parameters zoals weergegeven.

     ```
     Before changes
     #BASE
     ofrsasvr
     ofrlhsvr
     ofrdmsvr
     ofrdsedt
     ofrcmsvr
     ofruisvr
     ofrsmlog
     vtammgr
     TPFMAGENT

     #BATCH 
     #BATCH#obmtsmgr
     #BATCH#ofrpmsvr
     #BATCH#obmjmsvr
     #BATCH#obmjschd
     #BATCH#obmjinit
     #BATCH#obmjhist
     #BATCH#obmjspbk
     #TACF #TACF#tmsvr

     After changes  #BATCH
     #BASE          obmtsmgr 
     ofrsasvr       ofrpmsvr
     ofrlhsvr       obmjmsvr
     ofrdmsvr       obmjschd
     ofrdsedt       obmjinit
     ofrcmsvr       obmjhist
     ofruisvr       obmjspbk
     ofrsmlog
     vtammgr        #TACF
     TPFMAGENT      tmsvr
    ```

8. Kopieer het licentiebestand. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ cp /home/oframe7/oflicense/ofonline/licosc.dat 

     $OPENFRAME_HOME/license

     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/license 
     oframe@oframe7/OpenFrame/license / ls -l 
     -rwxr-xr-x. 1 oframe mqm 80 Sep 12 01:37 licosc.dat 
     -rwxr-xr-x. 1 oframe mqm 80 Sep  8 09:40 lictacf.dat 
     -rwxrwxr-x. 1 oframe mqm 80 Sep  3 11:54 lictjes.da
     ```

9. Als u wilt starten en afsluiten OSC, initialiseren het CICS regio gedeeld geheugen door te typen `osctdlinit OSCOIVP1` bij de opdrachtprompt.

10. Voer `oscboot` opstarten van OSC. De uitvoer ziet er ongeveer als volgt uit:

     ```
     OSCBOOT : pre-processing       [ OK ]

     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/12 
          TMBOOT: TMM is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLL is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLH is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: TLM(tlm) is starting: Mon Sep 12 01:40:25 2016 
     ```

11. Als u wilt controleren of de processtatus gereed is, gebruikt u de `tmadmin` opdracht in si. Alle processen weergegeven RDY in de **status** kolom.

    ![Processen RDY weergeven](media/tmadmin-02.png)

12. Afsluiten OSC met behulp van de `oscdown` opdracht.

## <a name="install-jeus"></a>JEUS installeren

JEUS (Java Enterprise gebruiker oplossing) biedt de presentatielaag van de OpenFrame-server.

Voordat u JEUS installeert, installeer het pakket Apache Ant biedt de bibliotheken en opdrachtregelprogramma's die nodig zijn voor het installeren van JEUS.

**Apache Ant installeren**

1. Download Ant binaire met behulp van de `wget` opdracht. Bijvoorbeeld:

     ```
     wget http://apache.mirror.cdnetworks.com/ant/binaries/apacheant-1.9.7-bin.tar.gz
     ```

2. Gebruik de `tar` hulpprogramma voor het uitpakken van het binaire bestand en verplaatsen naar een geschikte locatie. Bijvoorbeeld:

     ```
     tar -xvzf apache-ant-1.9.7-bin.tar.gz
     ```

3. Voor efficiëntie en een symbolische koppeling te maken:

     ```
     ln -s apache-ant-1.9.7 ant
     ```

4. Open de bash-profiel in vi (`vi .bash_profile`) en bijwerken met de volgende variabelen:

     ```
     # Ant ENV
     export ANT_HOME=$HOME/ant 
     export PATH=$HOME/ant/bin:$PATH
     ```

5.  De gewijzigde omgevingsvariabele van toepassing. Bijvoorbeeld:

     ```
     [oframe7\@ofdemo \~]\$ source \~/.bash\_profile
     ```

**Voor het installeren van JEUS**

1. Vouw het installatieprogramma uit met de `tar` hulpprogramma. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ tar -zxvf jeus704.tar.gz
     ```

2. Maak een **jeus** map (`mkdir jeus7`) en pak deze uit het binaire bestand.
3. Wijzig in het **setup** map (of gebruik de parameter JEUS voor uw eigen omgeving). Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ cd jeus7/setup/
     ```

4. Voer `ant clean-all` voordat u de build uitvoert. De uitvoer ziet er ongeveer als volgt uit:

     ```
     Buildfile: /home/oframe7jeus7/setup/build.xml

     clean-bin:
     delete-domain:
     [echo] Deleting a domain configuration: domain = jeus_domain
     delete-nodesxml:
     clean-config:
     clean-all:
     BUILD SUCCESSFUL
     Total time: 0 seconds
     ```

5.  Maak een back-up van de domein-config-template.properties-bestand. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ cp domain-config-template.properties domain-configtemplate.properties.bkp
     ```

6. Open het bestand domein-config-template.properties in vi:

     ```
     [oframe7\@ofdemo setup]\$ vi domain-config-template.properties
     ```

7. Wijziging `jeus.password=jeusadmin nodename=Tmaxsoft` naar `jeus.password=tmax1234 nodename=ofdemo`

8. Voer de `ant install` opdracht voor het bouwen van JEUS.
9.  Bijwerken van de .bash\_profielbestand met de JEUS variabelen, zoals wordt weergegeven:

     ```
     # JEUS ENV 
     export JEUS_HOME=/opt/tmaxui/jeus7 PATH="/opt/tmaxui/jeus7/bin:/opt/tmaxui/jeus7/lib/system:/opt/tmaxui/jeus7/webserver/bin:$ {PATH}" 
     export PATH
     ```

10. Voer de bash-profiel. Bijvoorbeeld:

     ```
     [oframe7@ofdemo setup]$ . .bash_profile
     ```

11. *Optioneel*. Maak een alias voor eenvoudig afsluiten en opstarten van JEUS onderdelen:

     ```     
     # JEUS alias

     alias dsboot='startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin'
     alias msboot='startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin' 
     alias msdown=‘jeusadmin -u administrator -p tmax1234 "stop-server server1“’ 
     alias dsdown=‘jeusadmin -domain jeus_domain -u administrator -p tmax1234 "local-shutdown“’
     ```

12. Om te controleren of de installatie, start u de beheerserver domein zoals wordt weergegeven:

     ```
     [oframe7@ofdemo ~]$ startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin
     ```

13. Controleer of door met behulp van de syntaxis van de webaanmelding:

     ```
     http://<IP>:<port>/webadmin/login
     ```

     Bijvoorbeeld, <http://192.168.92.133:9736/webadmin/login.> het aanmeldingsscherm wordt weergegeven:
    
     ![JEUS WebAdmin aanmeldingsscherm](media/jeus-01.png)

     > [!NOTE]
     > Als u problemen ondervindt met poortbeveiliging, poort 9736 openen of de firewall uitschakelen (`systemctl stop firewall`).

14. De hostnaam voor server1, klikt u op **vergrendelen & bewerken**, klikt u vervolgens op **server1**. In het venster van de Server, moet u de hostnaam als volgt wijzigen:

    1.  Wijziging **knooppuntnaam** naar **ofdemo**.
    2.  Klik op **OK** aan de rechterkant van het venster.
    3.  Klik op **wijzigingen toepassen** lagere aan de linkerkant van het venster en beschrijving, voer *hostnaam wijzigen*.

    ![JEUS WebAdmin scherm](media/jeus-02.png)

15. Controleer of de configuratie is voltooid in het bevestigingsvenster.

    ![scherm jeus_domain-Server](media/jeus-03.png)

16. Start de beheerde server-proces 'server1' met de volgende opdracht:

     ```
     [oframe7@ofdemo ~]$ startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin
     ```

## <a name="install-ofgw"></a>OFGW installeren

OFGW Is de OpenFrame-gateway die ondersteuning biedt voor communicatie tussen de 3270 terminal emulator en de OSI-base en beheert de tussen de terminal emulator en OSI-sessies.

**Voor het installeren van OFGW**

1. Zorg ervoor dat JEUS is geïnstalleerd, en vervolgens controleren of de OFGW7\_0\_1\_Generic.bin installer-bestand aanwezig is.
2. Het installatieprogramma worden uitgevoerd. Bijvoorbeeld:

     ```
     [oframe7@ofdemo ~]$ ./OFGW7_0_1_Generic.bin
     ````

3. Gebruik de volgende locaties voor de bijbehorende stappen:
     -   Basismap van de JEUS
     -   De domeinnaam JEUS
     -   De naam van de Server JEUS
     -   Tibero stuurprogramma
     -   Ofdemo Tmax knooppunt-ID

4. De rest van de standaardwaarden accepteren en druk op Enter om het installatieprogramma af te sluiten.

5. Controleren of de URL voor OFGW werkt zoals verwacht:

     ```
     Type URL 
     http://192.168.92.133:8088/webterminal/ and press enter
      < IP >               :8088/webterminal/
     ```

     Het volgende scherm wordt weergegeven:

    ![OpenFrame WebTerminal](media/ofgw-01.png)

## <a name="install-ofmanager"></a>OFManager installeren

OFManager biedt bewerking en beheerfuncties voor OpenFrame in de web-omgeving.

**Voor het installeren van OFManager**

1. Controleer de OFManager7\_Generic.bin installer-bestand aanwezig is.
2. Het installatieprogramma worden uitgevoerd. Bijvoorbeeld:

     ```
     OFManager7_Generic.bin
     ```

3.  Druk op Enter om door te gaan en accepteer de gebruiksrechtovereenkomst.
4.  Kies de installatiemap.
5.  Accepteer de standaardwaarden.
6.  Kies Tibero als de database.
7.  Druk op Enter om het installatieprogramma af te sluiten.
8.  Controleren of de URL voor OFManager werkt zoals verwacht:

     ```
     Type URL http://192.168.92.133:8088/ofmanager and press enter <  IP >  : < PORT >  ofmanager Enter ID:   ROOT 
     Password: SYS1
     ```

Het startscherm wordt weergegeven:

![Aanmeldingsscherm Tmax OpenFrame Manager](media/ofmanager-01.png)

De installatie van de OpenFrame-onderdelen is voltooid.

## <a name="next-steps"></a>Volgende stappen

Als u een mainframe-migratie overweegt, is ons groeiende partnerecosysteem beschikbaar om u te helpen. Raadpleeg voor gedetailleerde richtlijnen over het kiezen van een partneroplossing de [Platform modernisering Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

-   [Aan de slag met Azure](https://docs.microsoft.com/azure/)
-   [Host Integration Server HIS ()-documentatie](https://docs.microsoft.com/host-integration-server/)
-   [Lift-and-Shift-handleiding voor Azure Virtual Datacenter](https://blogs.msdn.microsoft.com/azurecat/2018/03/12/new-whitepaper-azure-virtual-datacenter-lift-and-shift-guide/)
