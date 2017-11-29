---
title: Het installeren van een Linux-hoofddoelserver voor failover van Azure met on-premises | Microsoft Docs
description: Voordat een virtuele Linux-machine opnieuw te beveiligen, moet u een Linux-hoofddoelserver. Informatie over het installeren van een.
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 11/22/2017
ms.author: rajanaki
ms.openlocfilehash: 7b2416617696e1df30b08f039ab39bfe7b57e093
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="install-a-linux-master-target-server"></a>Een Linux-hoofddoel-server installeren
Nadat u uw virtuele machines failover, u kunt een failback uit op de virtuele machines naar de lokale site. Als u wilt een failback uit, moet u beveiligt u de virtuele machine van Azure naar de lokale site opnieuw. Voor dit proces moet u de doelserver van een lokale voor het ontvangen van het verkeer. 

Als de beveiligde virtuele machine een virtuele Windows-computer is, moet u een Windows-hoofddoel. Voor een virtuele Linux-machine moet u een Linux-hoofddoel. Lees de volgende stappen uit voor informatie over het maken en een Linux-hoofddoel te installeren.

> [!IMPORTANT]
> Vanaf versie van de 9.10.0 hoofddoelserver, de meest recente hoofddoelserver kan alleen worden geïnstalleerd op een 16.04 Ubuntu server. Nieuwe installaties zijn niet toegestaan voor CentOS6.6 servers. U kunt echter doorgaan naar de oude hoofdsleutel doelservers upgraden met behulp van de 9.10.0 versie.

## <a name="overview"></a>Overzicht
In dit artikel bevat instructies voor het installeren van een Linux-hoofddoel.

Opmerkingen of vragen plaatsen aan het einde van dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Vereisten

* Als u de host waarop voor het implementeren van het hoofddoel, bepalen als de failback is het verstandig om een bestaande on-premises virtuele machine of een nieuwe virtuele machine. 
    * Voor een bestaande virtuele machine, moet de host van het hoofddoel toegang hebben tot de gegevensarchieven van de virtuele machine.
    * Als de on-premises virtuele machine niet bestaat, wordt de failback van virtuele machine gemaakt op dezelfde host als het hoofddoel. U kunt een willekeurige host ESXi voor het installeren van het hoofddoel.
* Het hoofddoel moet zich op een netwerk dat met de processerver en de configuratieserver communiceren kan.
* De versie van het hoofddoel moet gelijk zijn aan of ouder is dan de versies van de processerver en de configuratieserver. Bijvoorbeeld, als de versie van de configuratieserver 9.4, kan de versie van het hoofddoel worden 9.4 of 9.3 maar niet 9.5.
* Het hoofddoel mag alleen een virtuele VMware-machine en niet op een fysieke server.

## <a name="create-the-master-target-according-to-the-sizing-guidelines"></a>Het hoofddoel overeenkomstig de richtlijnen van het formaat maken

Het hoofddoel in overeenstemming met de volgende richtlijnen voor schaling maken:
- **RAM-geheugen**: 6 GB of meer
- **OS-schijfgrootte**: 100 GB of meer (om te installeren CentOS6.6)
- **Aanvullende schijfgrootte voor bewaarstation**: 1 TB
- **CPU-kernen**: 4 kernen of meer

De volgende ondersteunde Ubuntu kernels worden ondersteund.


|Kernel-reeks  |Ondersteuning voor maximaal  |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-the-master-target-server"></a>De hoofddoelserver implementeren

### <a name="install-ubuntu-16042-minimal"></a>Ubuntu 16.04.2 installeren minimale

Met het volgende de stappen voor het installeren van de Ubuntu 16.04.2 64-bits besturingssysteem.

**Stap 1:** gaat u naar de [downloadkoppeling](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) en kies de dichtstbijzijnde spiegelen uit welke downloaden Ubuntu 16.04.2 minimaal 64-bits ISO.

Ubuntu 16.04.2 minimaal 64-bits ISO houden in het DVD-station en start het systeem.

**Stap 2:** Selecteer **Engels** als uw voorkeurstaal en selecteer vervolgens **Enter**.

![Een taal selecteren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**Stap 3:** Selecteer **Ubuntu Server installeren**, en selecteer vervolgens **Enter**.

![Selecteer installeren Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**Stap 4:** Selecteer **Engels** als uw voorkeurstaal en selecteer vervolgens **Enter**.

![Engels als uw voorkeurstaal selecteren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**Stap 5:** Selecteer de relevante optie van de **tijdzone** optielijst en selecteer vervolgens **Enter**.

![Selecteer de juiste tijdzone](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**Stap 6:** Selecteer **Nee** (de standaardoptie) en selecteer vervolgens **Enter**.


![Het toetsenbord configureren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**Stap 7:** Selecteer **Engels (V.S.)** als het land van oorsprong voor het toetsenbord en selecteer vervolgens **Enter**.

![VS selecteren als het land van oorsprong](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**Stap 8:** Selecteer **Engels (V.S.)** als de toetsenbordindeling en selecteer vervolgens **Enter**.

![Als de toetsenbordindeling Amerikaans Engels selecteren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**Stap 9:** Geef de hostnaam op voor uw server in de **hostnaam** vak en selecteer vervolgens **doorgaan**.

![Geef de hostnaam op voor uw server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**Stap 10:** voor het maken van een gebruikersaccount, voer de gebruikersnaam en selecteer vervolgens **doorgaan**.

![Een gebruikersaccount maken](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**Stap 11:** Voer het wachtwoord voor het nieuwe gebruikersaccount en selecteer vervolgens **doorgaan**.

![Voer het wachtwoord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**Stap 12:** Bevestig het wachtwoord voor de nieuwe gebruiker en selecteer vervolgens **doorgaan**.

![De wachtwoorden bevestigen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**Stap 13:** Selecteer **Nee** (de standaardoptie) en selecteer vervolgens **Enter**.

![Gebruikers en wachtwoorden instellen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**Step 14:** als de tijdzone die wordt weergegeven correct is, selecteert u **Ja** (de standaardoptie) en selecteer vervolgens **Enter**.

Selecteer om opnieuw te uw tijdzone, **Nee**.

![Configureer de klok](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**Stap 15:** uit de partitionering opties van de methode selecteren **begeleide - volledige schijf gebruiken**, en selecteer vervolgens **Enter**.

![Selecteer de partitionering methode-optie](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**Stap 16:** selecteert u de juiste virtuele schijf van de **Select disk aan partitie** opties en selecteer vervolgens **Enter**.


![Selecteer de schijf](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**Stap 17:** Selecteer **Ja** schrijven van de wijzigingen naar schijf en selecteer vervolgens **Enter**.

![De wijzigingen naar de schijf te schrijven](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**Stap 18:** selecteert u de standaardoptie **doorgaan**, en selecteer vervolgens **Enter**.

![Selecteer de standaardoptie](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**Stap 19:** Selecteer de relevante optie voor het beheren van upgrades op uw systeem en selecteer vervolgens **Enter**.

![Selecteer het beheren van upgrades](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Omdat de Azure Site Recovery hoofddoelserver een specifieke versie van de Ubuntu vereist, moet u ervoor te zorgen dat de kernel upgrades zijn uitgeschakeld voor de virtuele machine. Als ze zijn ingeschakeld, dan eventuele reguliere upgrades voor zorgen dat de hoofddoelserver is niet goed. Zorg ervoor dat u selecteert de **geen automatische updates** optie.


**Stap 20:** standaardopties selecteren. Als u openSSH wilt voor SSH-verbinding maken, selecteer de **OpenSSH server** optie en selecteer vervolgens **doorgaan**.

![Selecteer de software](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**Stap 21:** Selecteer **Ja**, en selecteer vervolgens **Enter**.

![Het opstartlaadprogramma WORMGATEN doelsessie](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**Stap 22:** selecteert u het juiste apparaat voor de installatie van het laadprogramma voor opstarten (bij voorkeur **/dev/sda**), en selecteer vervolgens **Enter**.

![Selecteer een apparaat voor opstarten laadprogramma installeren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**Stap 23:** Selecteer **doorgaan**, en selecteer vervolgens **Enter** om de installatie te voltooien.

![De installatie voltooien](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

Nadat de installatie is voltooid, moet u zich aanmelden bij de virtuele machine met de nieuwe gebruikersgegevens. (Raadpleeg **stap 10** voor meer informatie.)

Voer de stappen die worden beschreven in de volgende schermafbeelding om in te stellen het hoofdwachtwoord van de gebruiker. Meld u vervolgens aan als hoofdgebruiker.

![De hoofdmap gebruikerswachtwoord instellen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-the-machine-for-configuration-as-a-master-target-server"></a>Voorbereiden van de machine voor configuratie als een hoofddoelserver
Bereid de machine voor de configuratie vervolgens als een hoofddoelserver.

Als u de ID voor elke harde SCSI-schijf in een virtuele Linux-machine, schakel de **schijf. EnableUUID = TRUE** parameter.

Voor deze parameter inschakelt, moet u de volgende stappen uitvoeren:

1. De virtuele machine afgesloten.

2. Met de rechtermuisknop op de vermelding voor de virtuele machine in het linkerdeelvenster en selecteer vervolgens **instellingen bewerken**.

3. Selecteer de **opties** tabblad.

4. Selecteer in het linkerdeelvenster **Geavanceerd** > **algemene**, en selecteer vervolgens de **configuratieparameters** knop in het gedeelte rechts van het scherm.

    ![Tabblad Opties](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    De **configuratieparameters** optie is niet beschikbaar wanneer de machine wordt uitgevoerd. Sluit de virtuele machine om op dit tabblad te activeren.

5. Zie of een rij met **schijf. EnableUUID** bestaat al.

    - Als de waarde bestaat en is ingesteld op **False**, wijzig de waarde in **True**. (De waarden zijn niet hoofdlettergevoelig).

    - Als de waarde bestaat en is ingesteld op **True**, selecteer **annuleren**.

    - Als de waarde niet bestaat, selecteert u **rij toevoegen**.

    - Voeg in de naamkolom **schijf. EnableUUID**, en stel de waarde op **TRUE**.

    ![Controleren of schijf. Er bestaat al een EnableUUID](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Kernel-upgrades uitschakelen

Azure Site Recovery-hoofddoelserver vereist een specifieke versie van de Ubuntu, zorg ervoor dat de kernel-upgrades voor de virtuele machine zijn uitgeschakeld.

Als upgrades van de kernel zijn ingeschakeld, dan eventuele upgrades voor reguliere voor zorgen dat de hoofddoelserver is niet goed.

#### <a name="download-and-install-additional-packages"></a>Extra pakketten downloaden en installeren

> [!NOTE]
> Zorg ervoor dat er verbinding met Internet te downloaden en installeren van extra pakketten. Als u geen verbinding met Internet, moet u handmatig deze RPM-pakketten zoeken en te installeren.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-the-installer-for-setup"></a>Ophalen van het installatieprogramma voor installatie

Als uw hoofddoel verbinding met Internet heeft, kunt u de volgende stappen uit voor het downloaden van het installatieprogramma. U kunt anders kopiëren van het installatieprogramma van de processerver en u deze vervolgens installeren.

#### <a name="download-the-master-target-installation-packages"></a>De installatiepakketten hoofddoel downloaden

[Download de meest recente Linux-hoofddoel installatie bits](https://aka.ms/latestlinuxmobsvc).

Als u wilt downloaden met behulp van Linux, typt u:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Zorg ervoor dat u downloaden en uitpakken van het installatieprogramma in de basismap. Als u naar uitpakken **/usr/Local**, zal de installatie mislukken.


#### <a name="access-the-installer-from-the-process-server"></a>Toegang tot het installatieprogramma vanaf de processerver

1. Ga op de processerver naar **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Kopieer het vereiste installer-bestand van de processerver en sla het bestand als **latestlinuxmobsvc.tar.gz** in de basismap.


### <a name="apply-custom-configuration-changes"></a>Aangepaste configuratiewijzigingen toepassen

Aangepaste configuratiewijzigingen wilt toepassen, gebruikt u de volgende stappen uit:


1. Voer de volgende opdracht om het binaire bestand untar.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Schermafbeelding van de opdracht uitvoeren](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Voer de volgende opdracht om toestemming te geven.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Voer de volgende opdracht het script uit te voeren.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Voer het script slechts eenmaal op de server. De server afsluiten. De server opnieuw opstarten nadat u een schijf toevoegen zoals beschreven in de volgende sectie.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Een bewaarperiode schijf toevoegen aan de Linux-hoofddoel virtuele machine

Gebruik de volgende stappen voor het maken van een schijf bewaren:

1. Koppelen van een nieuwe schijf van 1 TB aan de Linux-hoofddoel virtuele machine en de machine start.

2. Gebruik de **multipath -lle** opdracht voor meer informatie over de multipath-ID van de schijf bewaren.

    ```
    multipath -ll
    ```
    ![De multipath-ID van de retentie-schijf](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formatteren van de schijf en maak vervolgens een bestandssysteem op het nieuwe station.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Een bestandssysteem maken op het station](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Nadat u het bestandssysteem gemaakt, moet u de bewaarperiode schijf koppelen.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![De retentie-schijf koppelen](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Maak de **fstab** vermelding koppelen van het bewaarstation telkens wanneer het systeem wordt gestart.
    ```
    vi /etc/fstab
    ```
    Selecteer **invoegen** om te beginnen met het bewerken van het bestand. Maak een nieuwe regel en voeg vervolgens de volgende tekst. De schijf multipath-ID op basis van de gemarkeerde multipath-ID van de vorige opdracht bewerken.

    **/dev/mapper/ <Retention disks multipath id> /mnt/bewaren ext4 rw 0 0**

    Selecteer **Esc**, en typ vervolgens **: wq** (schrijven en sluiten) het editorvenster te sluiten.

### <a name="install-the-master-target"></a>Het hoofddoel installeren

> [!IMPORTANT]
> De versie van de hoofddoelserver moet gelijk zijn aan of ouder is dan de versies van de processerver en de configuratieserver. Als dit probleem niet wordt voldaan, beveiligt is geslaagd, maar mislukt de replicatie.


> [!NOTE]
> Voordat u de hoofddoelserver installeert, Controleer of de **/etc/hosts** -bestand op de virtuele machine bevat items die de lokale hostnaam worden toegewezen aan de IP-adressen die gekoppeld aan alle netwerkadapters zijn.

1. Kopieer de wachtwoordzin van **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** op de configuratieserver. Sla het als **passphrase.txt** in dezelfde lokale map met de volgende opdracht:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Voorbeeld: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Noteer de configuratieserver IP-adres. U moet deze in de volgende stap.

3. Voer de volgende opdracht voor het installeren van de hoofddoelserver en de server geregistreerd met de configuratieserver.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Voorbeeld: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Wacht totdat het script is voltooid. Als het hoofddoel wordt geregistreerd is, het hoofddoel wordt weergegeven op de **Site Recovery-infrastructuur** pagina van de portal.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Het hoofddoel installeren met behulp van interactieve installatie

1. Voer de volgende opdracht voor het installeren van het hoofddoel. Kies voor de rol agent **hoofddoel**.

    ```
    ./install
    ```

2. Kies de standaardlocatie voor installatie en selecteer vervolgens **Enter** om door te gaan.

    ![Een standaardlocatie voor installatie van het hoofddoel kiezen](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Nadat de installatie is voltooid, moet u de configuratieserver registreren met behulp van de opdrachtregel.

1. Noteer de IP-adres van de configuratieserver. U moet deze in de volgende stap.

2. Voer de volgende opdracht voor het installeren van de hoofddoelserver en de server geregistreerd met de configuratieserver.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Voorbeeld: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Wacht totdat het script is voltooid. Als het hoofddoel met succes is geregistreerd, het hoofddoel wordt weergegeven op de **Site Recovery-infrastructuur** pagina van de portal.


### <a name="upgrade-the-master-target"></a>Het hoofddoel upgraden

Het installatieprogramma uitvoert. Automatisch wordt gedetecteerd dat de agent is geïnstalleerd op het hoofddoel. Als u wilt bijwerken, selecteert u **Y**.  Nadat de installatie is voltooid, controleert u de versie van het hoofddoel geïnstalleerd met behulp van de volgende opdracht:

    ```
    cat /usr/local/.vx_version
    ```

Kunt u zien dat de **versie** veld kunt u het versienummer van het hoofddoel.

### <a name="install-vmware-tools-on-the-master-target-server"></a>Installeer VMware tools op de hoofddoelserver

U moet VMware tools installeren op het hoofddoel zodat deze de gegevensarchieven kan detecteren. Als de hulpprogramma's zijn niet geïnstalleerd, wordt het scherm beveiligt niet vermeld in de gegevensarchieven. Na installatie van de VMware-hulpprogramma's moet u opnieuw opstarten.

## <a name="next-steps"></a>Volgende stappen
Nadat de installatie en registratie van het hoofddoel is voltooid, ziet u het hoofddoel worden weergegeven op de **hoofddoel** in sectie **Site Recovery-infrastructuur**, onder de configuratie overzicht van de server.

U kunt nu doorgaan met [beveiligingspoging](site-recovery-how-to-reprotect.md), gevolgd door de failback.

## <a name="common-issues"></a>Algemene problemen

* Zorg ervoor dat u inschakelt Storage vMotion op eventuele management-onderdelen, zoals een hoofddoel. Als het hoofddoel na een geslaagde beveiligt wordt, kan de virtuele machine-schijven (VMDKs) kunnen niet worden losgekoppeld. In dit geval failback is mislukt.

* Het hoofddoel mag geen eventuele momentopnamen op de virtuele machine. Als momentopnamen bevat, mislukt de failback.

* De netwerkinterface tijdens het opstarten is uitgeschakeld als gevolg van een aantal aangepaste configuraties voor NIC op sommige klanten en de hoofddoelserver-agent kan niet worden geïnitialiseerd. Zorg ervoor dat de volgende eigenschappen correct zijn ingesteld. Controleer deze eigenschappen in de Ethernet-kaart van het bestand /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO = dhcp
    * ONBOOT = yes
