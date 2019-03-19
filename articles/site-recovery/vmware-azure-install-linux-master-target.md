---
title: Een Linux-hoofddoelserver voor failback installeren op een on-premises site | Microsoft Docs
description: Meer informatie over het instellen van een Linux-hoofddoelserver voor failback uitvoeren naar een on-premises site tijdens herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery.
author: mayurigupta13
services: site-recovery
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/06/2019
ms.author: mayg
ms.openlocfilehash: 98718709038d7fd753e5eb3d45c130085c5accd9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58099049"
---
# <a name="install-a-linux-master-target-server-for-failback"></a>Installeren van een Linux-hoofddoelserver voor failback
Nadat u uw virtuele machines naar Azure failover, kunt u de virtuele machines om de on-premises site weer failover. Als u wilt uitvoeren van een failback, moet u opnieuw beveiligen van de virtuele machine van Azure naar de on-premises site. Voor dit proces moet u een on-premises hoofddoelserver om het verkeer te ontvangen. 

Als de beveiligde virtuele machine een Windows virtuele machine is, moet u een Windows-hoofddoelserver. Voor een virtuele Linux-machine moet u een Linux-hoofddoelserver. Lees de volgende stappen uit voor informatie over het maken en een Linux-hoofddoel te installeren.

> [!IMPORTANT]
> Vanaf versie van de 9.10.0 hoofddoelserver, de meest recente hoofddoelserver kan alleen worden geïnstalleerd op een Ubuntu 16.04-server. Nieuwe installaties zijn niet toegestaan voor CentOS6.6 servers. U kunt echter doorgaan naar de oude hoofdsleutel doelservers bijwerken met behulp van de 9.10.0 versie.
> Masterdoelserver op LVM wordt niet ondersteund.

## <a name="overview"></a>Overzicht
In dit artikel bevat instructies voor het installeren van een Linux-hoofddoelserver.

Opmerkingen of vragen plaatsen aan het einde van dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Vereisten

* Als u de host waarop de implementatie van het hoofddoel, bepalen als de failback is het verstandig om een bestaande on-premises virtuele machine of een nieuwe virtuele machine. 
    * Voor een bestaande virtuele machine, moet de host van de hoofddoelserver toegang hebben tot de gegevensopslag van de virtuele machine.
    * Als de on-premises virtuele machine (in het geval van herstel naar alternatieve locatie) niet bestaat, wordt de virtuele failback-machine wordt gemaakt op dezelfde host als het hoofddoel. U kunt een ESXi-host voor het installeren van het hoofddoel.
* Het hoofddoel moet zich op een netwerk dat met de processerver en de configuratieserver communiceren kan.
* De versie van het hoofddoel moet gelijk zijn aan of lager dan de versies van de processerver en de configuratieserver. Bijvoorbeeld, als de versie van de configuratieserver 9.4 is, kan de versie van het hoofddoel worden 9.4 of 9.3 maar niet 9.5.
* Het hoofddoel mag alleen een virtuele VMware-machine en niet op een fysieke server.

## <a name="sizing-guidelines-for-creating-master-target-server"></a>Aanbevelingen voor voor het maken van de hoofddoelserver

Maak het hoofddoel in overeenstemming met de volgende richtlijnen voor de grootte:
- **RAM**: 6 GB of meer
- **Grootte besturingssysteemschijf**: 100 GB of meer (om te installeren besturingssysteem)
- **Aanvullende schijfgrootte voor bewaarstation**: 1 TB
- **CPU-kernen**: 4 kernen of meer

De volgende ondersteunde Ubuntu-kernels worden ondersteund.


|Kernel Series  |Ondersteuning voor maximaal  |
|---------|---------|
|4.4      |4.4.0-81-generic         |
|4.8      |4.8.0-56-generic         |
|4.10     |4.10.0-24-generic        |


## <a name="deploy-the-master-target-server"></a>De hoofddoelserver implementeren

### <a name="install-ubuntu-16042-minimal"></a>Install Ubuntu 16.04.2 Minimal

Met het volgende de stappen voor het installeren van de Ubuntu 16.04.2 64-bits besturingssysteem.

1.   Ga naar de [downloadkoppeling](http://old-releases.ubuntu.com/releases/16.04.2/ubuntu-16.04.2-server-amd64.iso), kies de dichtstbijzijnde mirror en downloaden van een Ubuntu 16.04.2 minimaal 64-bit ISO.
Een Ubuntu 16.04.2 minimaal 64-bit ISO behouden in het DVD-station en start het systeem.

1.  Selecteer **Engels** als uw voorkeurstaal en selecteer vervolgens **Enter**.
    
    ![Een taal selecteren](./media/vmware-azure-install-linux-master-target/image1.png)
1. Selecteer **Ubuntu-Server installeren**, en selecteer vervolgens **Enter**.

    ![Selecteer de Ubuntu-Server installeren](./media/vmware-azure-install-linux-master-target/image2.png)

1.  Selecteer **Engels** als uw voorkeurstaal en selecteer vervolgens **Enter**.

    ![Engels als uw voorkeurstaal selecteren](./media/vmware-azure-install-linux-master-target/image3.png)

1. Selecteer de gewenste optie uit de **tijdzone** lijst met opties en selecteer vervolgens **Enter**.

    ![Selecteer de juiste tijdzone](./media/vmware-azure-install-linux-master-target/image4.png)

1. Selecteer **Nee** (de standaardoptie), en selecteer vervolgens **Enter**.

     ![Het toetsenbord configureren](./media/vmware-azure-install-linux-master-target/image5.png)
1. Selecteer **Engels (V.S.)** als het land van oorsprong van het toetsenbord en selecteer vervolgens **Enter**.

1. Selecteer **Engels (V.S.)** als de toetsenbordindeling en selecteer vervolgens **Enter**.

1. Geef de hostnaam op voor uw server in de **hostnaam** vak en selecteer vervolgens **doorgaan**.

1. Voer de gebruikersnaam voor het maken van een gebruikersaccount, en selecteer vervolgens **doorgaan**.

      ![Een gebruikersaccount maken](./media/vmware-azure-install-linux-master-target/image9.png)

1. Voer het wachtwoord voor het nieuwe gebruikersaccount en selecteer vervolgens **doorgaan**.

1.  Bevestig het wachtwoord voor de nieuwe gebruiker en selecteer vervolgens **doorgaan**.

    ![Controleer of de wachtwoorden](./media/vmware-azure-install-linux-master-target/image11.png)

1.  Selecteer in de volgende selectie voor het versleutelen van de basismap **Nee** (de standaardoptie), en selecteer vervolgens **Enter**.

1. Als de tijdzone die wordt weergegeven juist is, selecteert u **Ja** (de standaardoptie), en selecteer vervolgens **Enter**. Als u wilt configureren in uw tijdzone, selecteer **Nee**.

1. Selecteer in de opties voor partitioneren methode, **begeleide - volledige schijf gebruiken**, en selecteer vervolgens **Enter**.

     ![Selecteer de partitionering methode-optie](./media/vmware-azure-install-linux-master-target/image14.png)

1.  Selecteer de juiste schijf uit de **Selecteer schijf naar partitie** opties en selecteer vervolgens **Enter**.

    ![Selecteer de schijf](./media/vmware-azure-install-linux-master-target/image15.png)

1.  Selecteer **Ja** schrijven van de wijzigingen naar schijf en selecteer vervolgens **Enter**.

    ![Selecteer de standaardoptie](./media/vmware-azure-install-linux-master-target/image16-ubuntu.png)

1.  Selecteer de standaardoptie in de selectie van de proxy configureren, selecteert u **doorgaan**, en selecteer vervolgens **Enter**.
     
     ![Selecteer hoe u voor het beheren van upgrades](./media/vmware-azure-install-linux-master-target/image17-ubuntu.png)

1.  Selecteer **geen automatische updates** optie in de selectie voor het beheren van upgrades op uw systeem, en selecteer vervolgens **Enter**.

     ![Selecteer hoe u voor het beheren van upgrades](./media/vmware-azure-install-linux-master-target/image18-ubuntu.png)

    > [!WARNING]
    > Omdat de hoofddoelserver Azure Site Recovery is een zeer specifieke versie van Ubuntu vereist, moet u ervoor te zorgen dat de kernel upgrades zijn uitgeschakeld voor de virtuele machine. Als ze zijn ingeschakeld, klikt u vervolgens eventuele upgrades voor reguliere ertoe leiden dat de hoofddoelserver niet goed. Zorg ervoor dat u selecteert de **geen automatische updates** optie.

1.  Selecteer de standaardopties. Als u openSSH wilt voor SSH verbinding maken, selecteert u de **OpenSSH-server** optie en selecteer vervolgens **doorgaan**.

    ![Selecteer software](./media/vmware-azure-install-linux-master-target/image19-ubuntu.png)

1. Selecteer in de selectie voor het installeren van de GRUB-opstartlaadprogramma **Ja**, en selecteer vervolgens **Enter**.
     
    ![WORMGATEN opstarten installatieprogramma](./media/vmware-azure-install-linux-master-target/image20.png)


1. Selecteer het juiste apparaat voor de installatie van het laadprogramma voor opstarten (bij voorkeur **/dev/sda**), en selecteer vervolgens **Enter**.
     
    ![Juiste apparaat selecteren](./media/vmware-azure-install-linux-master-target/image21.png)

1. Selecteer **doorgaan**, en selecteer vervolgens **Enter** om de installatie te voltooien.

    ![De installatie voltooien](./media/vmware-azure-install-linux-master-target/image22.png)

1. Nadat de installatie is voltooid, moet u zich aanmelden bij de virtuele machine met de referenties van de nieuwe gebruiker. (Raadpleeg **stap 10** voor meer informatie.)

1. Gebruik de stappen die worden beschreven in de volgende schermafbeelding om in te stellen de hoofdmap gebruikerswachtwoord. Meld u vervolgens als hoofdgebruiker.

    ![De hoofdmap van gebruikerswachtwoord instellen](./media/vmware-azure-install-linux-master-target/image23.png)


### <a name="configure-the-machine-as-a-master-target-server"></a>De machine als een hoofddoelserver configureren

Ophalen van de ID voor elke harde SCSI-schijf in een virtuele Linux-machine, de **schijf. EnableUUID = TRUE** parameter moet worden ingeschakeld. Als u wilt deze parameter inschakelt, moet u de volgende stappen uitvoeren:

1. Uw virtuele machine afgesloten.

2. Met de rechtermuisknop op de vermelding voor de virtuele machine in het linkerdeelvenster en selecteer vervolgens **instellingen bewerken**.

3. Selecteer de **opties** tabblad.

4. Selecteer in het linkerdeelvenster **Geavanceerd** > **algemene**, en selecteer vervolgens de **configuratieparameters** knop in het gedeelte rechts van het scherm.

    ![Open de configuratieparameter](./media/vmware-azure-install-linux-master-target/image24-ubuntu.png) 

    De **configuratieparameters** optie is niet beschikbaar wanneer de machine wordt uitgevoerd. Als u wilt dit tabblad activeren, sluit u de virtuele machine.

5. Zie of een rij met **schijf. EnableUUID** bestaat al.

   - Als de waarde bestaat en is ingesteld op **False**, wijzig de waarde in **waar**. (De waarden zijn niet hoofdlettergevoelig).

   - Als de waarde bestaat en is ingesteld op **waar**, selecteer **annuleren**.

   - Als de waarde niet bestaat, selecteert u **rij toevoegen**.

   - Voeg in de naamkolom **schijf. EnableUUID**, en stel de waarde in op **waar**.

     ![Controleren of schijf. Er bestaat al een EnableUUID](./media/vmware-azure-install-linux-master-target/image25.png)

#### <a name="disable-kernel-upgrades"></a>Kernel-upgrades uitschakelen

Azure Site Recovery-hoofddoelserver is vereist voor een specifieke versie van Ubuntu en zorg ervoor dat de kernel-upgrades voor de virtuele machine zijn uitgeschakeld. Als de upgrades kernel zijn ingeschakeld, kan dit ertoe leiden dat de hoofddoelserver niet goed.

#### <a name="download-and-install-additional-packages"></a>Download en installeer extra pakketten

> [!NOTE]
> Zorg ervoor dat u verbinding met Internet te downloaden en installeren van extra pakketten. Als u geen verbinding met Internet, moet u handmatig deze Deb-pakketten zoeken en te installeren.

 `apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx`

### <a name="get-the-installer-for-setup"></a>Het installatieprogramma voor setup ophalen

Als het hoofddoel verbinding met Internet heeft heeft, kunt u de volgende stappen uit om te downloaden van het installatieprogramma. U kunt anders, Kopieer het installatieprogramma van de processerver en deze vervolgens installeren.

#### <a name="download-the-master-target-installation-packages"></a>De installatiepakketten hoofddoel downloaden

[Download de meest recente installatie materialen voor Linux hoofddoel](https://aka.ms/latestlinuxmobsvc).

Als u wilt downloaden met behulp van Linux, typt u:

`wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz`

> [!WARNING]
> Zorg ervoor dat u downloaden en uitpakken van het installatieprogramma in de basismap. Als u naar uitpakken **/usr/Local**, dan de installatie mislukt.


#### <a name="access-the-installer-from-the-process-server"></a>Toegang tot het installatieprogramma van de processerver

1. Op de processerver, gaat u naar **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Het vereiste installer-bestand kopiëren van de processerver en sla het bestand als **latestlinuxmobsvc.tar.gz** in de basismap.


### <a name="apply-custom-configuration-changes"></a>Aangepaste configuratiewijzigingen toepassen

Aangepaste configuratiewijzigingen wilt toepassen, gebruikt u de volgende stappen uit:


1. Voer de volgende opdracht uit om het binaire bestand untar.

    `tar -zxvf latestlinuxmobsvc.tar.gz`

    ![Schermafbeelding van de opdracht om uit te voeren](./media/vmware-azure-install-linux-master-target/image16.png)

2. Voer de volgende opdracht uit om toestemming te geven.

    `chmod 755 ./ApplyCustomChanges.sh`


3. Voer de volgende opdracht om uit te voeren van het script.
    
    `./ApplyCustomChanges.sh`

> [!NOTE]
> Voer het script slechts één keer op de server. Vervolgens de server afsluiten. De server opnieuw opstarten na het toevoegen van een schijf, zoals beschreven in de volgende sectie.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Een bewaarschijf toevoegen aan de virtuele machine voor Linux hoofddoelserver

Gebruik de volgende stappen uit om te maken van een bewaarschijf:

1. Een nieuwe schijf van 1 TB koppelen aan de virtuele machine voor Linux hoofddoelserver en start de machine.

2. Gebruik de **multipath -lle** opdracht voor meer informatie over de MPIO-ID van de bewaarschijf: **multipath -alles**

    ![Multipath-ID](./media/vmware-azure-install-linux-master-target/image27.png)

3. Formatteer de schijf, en maak vervolgens een bestandssysteem op het nieuwe station: **mkfs.ext4 /dev/mapper/< multipath-id van de bewaarschijf >**.
    
    ![Bestandssysteem](./media/vmware-azure-install-linux-master-target/image23-centos.png)

4. Nadat u het bestandssysteem gemaakt, koppelt u de bewaarschijf.

    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```

5. Maak de **fstab** vermelding voor het koppelen van de retentieschijf telkens wanneer het systeem wordt gestart.
    
    `vi /etc/fstab`
    
    Selecteer **invoegen** om te beginnen met het bestand te bewerken. Maak een nieuwe regel en voeg vervolgens de volgende tekst. Bewerk de schijf multipath-ID op basis van de gemarkeerde multipath-ID van de vorige opdracht.

     **/dev/mapper/ <Retention disks multipath id> /mnt/retentie ext4 rw 0 0**

    Selecteer **Esc**, en typ vervolgens **: wq** (schrijven en afsluiten) om de editorvenster te sluiten.

### <a name="install-the-master-target"></a>Het hoofddoel installeren

> [!IMPORTANT]
> De versie van de hoofddoelserver moet gelijk zijn aan of lager dan de versies van de processerver en de configuratieserver. Als niet aan deze voorwaarde wordt voldaan, opnieuw beveiligen is voltooid, maar replicatie is mislukt.


> [!NOTE]
> Voordat u de hoofddoelserver installeert, Controleer of de **/etc/hosts** bestand op de virtuele machine bevat items die de lokale hostnaam toewijzen aan de IP-adressen die gekoppeld aan alle netwerkadapters zijn.

1. Kopieer de wachtwoordzin van **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** op de configuratieserver. Sla het bestand als **passphrase.txt** in dezelfde lokale map met de volgende opdracht:

    `echo <passphrase> >passphrase.txt`

    Voorbeeld: 

       `echo itUx70I47uxDuUVY >passphrase.txt`
    

2. Noteer de IP-adres van de configuratieserver. Voer de volgende opdracht op de hoofddoelserver installeren en registreren van de server met de configuratieserver.

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Voorbeeld: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

Wacht totdat het script is voltooid. Als het hoofddoel correct is geregistreerd, het hoofddoel wordt weergegeven op de **Site Recovery-infrastructuur** pagina van de portal.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Het hoofddoel installeren met behulp van interactieve installatie

1. Voer de volgende opdracht voor het installeren van het hoofddoel. Kies voor de rol van de agent, **hoofddoelserver**.

    ```
    ./install
    ```

2. Kies de standaardlocatie voor installatie, en selecteer vervolgens **Enter** om door te gaan.

    ![Een standaardlocatie voor installatie van de hoofddoelserver kiezen](./media/vmware-azure-install-linux-master-target/image17.png)

Nadat de installatie is voltooid, moet u de configuratieserver registreren via de opdrachtregel.

1. Noteer de IP-adres van de configuratieserver. U hebt deze nodig in de volgende stap.

2. Voer de volgende opdracht op de hoofddoelserver installeren en registreren van de server met de configuratieserver.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Voorbeeld: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

     Wacht totdat het script is voltooid. Als het hoofddoel is geregistreerd, het hoofddoel wordt weergegeven op de **Site Recovery-infrastructuur** pagina van de portal.


### <a name="install-vmware-tools--open-vm-tools-on-the-master-target-server"></a>Installeer VMware tools / open-vm--hulpprogramma's op de hoofddoelserver

U moet VMware-hulpprogramma's of open-vm-hulpprogramma's installeren op de hoofddoelserver zodat deze de gegevensarchieven kunt detecteren. Als de hulpprogramma's niet zijn geïnstalleerd, wordt het scherm opnieuw beveiligen wordt niet weergegeven in de gegevensarchieven. Na de installatie van de VMware-hulpprogramma's moet u opnieuw opstarten.

### <a name="upgrade-the-master-target-server"></a>Upgrade van de hoofddoelserver

Voer het installatieprogramma. Er wordt automatisch gedetecteerd dat de agent is geïnstalleerd op de hoofddoelserver. Als u wilt bijwerken, selecteert u **Y**.  Nadat de installatie is voltooid, controleert u de versie van het hoofddoel geïnstalleerd met behulp van de volgende opdracht uit:

`cat /usr/local/.vx_version`


U ziet dat de **versie** veld geeft het versienummer van het hoofddoel.

## <a name="common-issues"></a>Algemene problemen

* Zorg ervoor dat u opslag-vMotion op alle onderdelen, zoals een hoofddoel niet inschakelen. Als het hoofddoel wordt verplaatst nadat een geslaagde opnieuw beveiligen, kan de virtuele machine-schijven (vmdk's) kunnen niet worden losgekoppeld. In dit geval failback is mislukt.

* Het hoofddoel hoeft niet alle momentopnamen op de virtuele machine. Als er momentopnamen, mislukt de failback.

* De netwerkinterface tijdens het opstarten is uitgeschakeld vanwege bepaalde aangepaste configuraties voor NIC, en de hoofddoelserver-agent kan niet worden geïnitialiseerd. Zorg ervoor dat de volgende eigenschappen correct zijn ingesteld. Controleer deze eigenschappen in de Ethernet-kaart van het bestand /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO=dhcp
    * ONBOOT = Ja


## <a name="next-steps"></a>Volgende stappen
Nadat de installatie en registratie van het hoofddoel is voltooid, ziet u het hoofddoel worden weergegeven op de **hoofddoelserver** in sectie **Site Recovery-infrastructuur**, onder de configuratie overzicht van de server.

U kunt nu doorgaan met [opnieuw beveiligen](vmware-azure-reprotect.md), gevolgd door de failback.

