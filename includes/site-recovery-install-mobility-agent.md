---
title: 
description: 
services: site-recovery
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 4871e5959483058aa957e3373ddc5121b398e1d9
ms.openlocfilehash: 1d6d595bf788fc3fdba0bbc98887614e17c791dc


---



### <a name="install-the-mobility-service"></a>De Mobility-service installeren
De eerste stap bij het inschakelen van beveiliging voor virtuele machines en fysieke servers is het installeren van de Mobility-service. U kunt dit op verschillende manieren doen:

* **Push van processerver**: wanneer u replicatie op een virtuele machine inschakelt, pusht en installeert u het onderdeel van de Mobility-service vanaf de processerver. 
*Opmerking*: de push-installatie vindt niet plaats als op de computers al een actuele versie van het onderdeel wordt uitgevoerd.
* **Enterprise-push**: installeer automatisch het onderdeel met behulp van uw Enterprise-pushproces, zoals WSUS of System Center Configuration Manager of [Azure Automation en Desired State Configuration](site-recovery-automate-mobility-service-install.md). Stel de configuratieserver in voordat u dit doet.
* **Handmatige installatie**: installeer het onderdeel handmatig op elke computer die u wilt repliceren. Stel de configuratieserver in voordat u dit doet.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Automatische push voorbereiden op Windows-computers
Hier volgt een uitleg van hoe u Windows-computers kunt voorbereiden, zodat de Mobility-service automatisch kan worden geïnstalleerd door de processerver.

1. Maak een account dat door de processerver kan worden gebruikt voor toegang tot de computer. Het account moet over beheerdersbevoegdheden (lokaal of domein) beschikken en wordt alleen gebruikt voor de push-installatie.

   > [!NOTE]
   > Als u niet een domeinaccount gebruikt, moet u toegangsbeheer voor externe gebruikers uitschakelen op de lokale computer. U doet dit door in het register onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System de DWORD-vermelding LocalAccountTokenFilterPolicy met een waarde van 1 toe te voegen. De registervermelding toevoegen van een CLI-type **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.
   >
   >
2. Selecteer op de Windows Firewall van de computer die u wilt beveiligen **via Firewall de optie Toestaan op een app of functie**. Schakel **Bestands- en printerdeling** en **Windows Management Instrumentation** in. Voor computers die tot een domein behoren, kunt u de firewallinstellingen met een GPO configureren.

   ![Firewallinstellingen](./media/site-recovery-vmware-to-azure/mobility1.png)
3. Voeg het account toe dat u hebt gemaakt:

   * Open **cspsconfigtool**. Deze is beschikbaar als snelkoppeling op het bureaublad en in de map [INSTALL LOCATION]\home\svsystems\bin folder.
   * Klik in het tabblad **Accounts beheren** op **Account toevoegen**.
   * Voeg het account toe dat u hebt gemaakt. Nadat u het account hebt toegevoegd, dient u de referenties op te geven wanneer u replicatie voor een computer inschakelt.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Automatische push voorbereiden op Linux-servers
1. Zorg ervoor dat de Linux-computer die u wilt beveiligen, wordt ondersteund, zoals wordt beschreven in [Vereisten voor beveiligde machines](#protected-machine-prerequisites). Zorg ervoor dat er een netwerkverbinding is tussen de Linux-computer en de processerver.
2. Maak een account dat door de processerver kan worden gebruikt voor toegang tot de computer. Het account moet een hoofdgebruiker zijn op de Linux-bronserver en wordt alleen gebruikt voor de push-installatie.

   * Open **cspsconfigtool**. Deze is beschikbaar als snelkoppeling op het bureaublad en in de map [INSTALL LOCATION]\home\svsystems\bin folder.
   * Klik in het tabblad **Accounts beheren** op **Account toevoegen**.
   * Voeg het account toe dat u hebt gemaakt. Nadat u het account hebt toegevoegd, dient u de referenties op te geven wanneer u replicatie voor een computer inschakelt.
3. Controleer of het bestand /etc/hosts op de Linux-bronserver vermeldingen bevat die de lokale hostnaam toewijzen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
4. Installeer de meest recente openssh, openssh-server en openssl-pakketten op de computer die u wilt repliceren.
5. Zorg ervoor dat SSH is ingeschakeld en wordt uitgevoerd op poort 22.
6. U kunt als volgt het SFTP-subsysteem en wachtwoordverificatie inschakelen in het bestand sshd_config:

   * Meld u aan als hoofdgebruiker.
   * In het bestand /etc/ssh/sshd_config zoekt u de regel die begint met **PasswordAuthentication**.
   * Verwijder opmerkingen bij de regel en wijzig de waarde van **nee** naar **ja**.
   * Zoek de regel die begint met **Subsystem** en verwijder opmerkingen bij de regel.

     ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>De Mobility-service handmatig installeren
De installatieprogramma's zijn beschikbaar op de configuratieserver in **C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

| Bronbesturingssysteem | Installatiebestand van de Mobility-service |
| --- | --- |
| Windows Server (alleen&64;-bits) |Microsoft-ASR_UA_9.*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (alleen 64-bits) |Microsoft-ASR_UA_9.*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (alleen 64-bits) |Microsoft-ASR_UA_9.*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (alleen 64-bits) |Microsoft-ASR_UA_9.*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-mobility-service-on-a-windows-server"></a>Mobility-service installeren op een Windows-server
1. Download het relevante installatieprogramma en voer het uit.
2. Selecteer in **Voordat u begint** **Mobility-service**.

    ![Mobility-service](./media/site-recovery-vmware-to-azure/mobility3.png)
3. Bij **Configuration Server Details** (Serverconfiguratiedetails) geeft u het IP-adres van de configuratieserver op en de wachtwoordzin die is gegenereerd bij het uitvoeren van geïntegreerde Setup. U kunt de wachtwoordzin ophalen door ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –v** uit te voeren op de configuratieserver.

    ![Mobility-service](./media/site-recovery-vmware-to-azure/mobility6.png)
4. Laat in **Installatielocatie** de standaardinstelling staan en klik op **Volgende** om de installatie te starten.
5. Controleer de installatie in **Voortgang van installatie** en start de computer opnieuw op wanneer dat wordt gevraagd. Na de installatie van de service kan het circa 15 minuten duren voordat de status is bijgewerkt in de portal.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Mobility-service installeren op een Windows-server met de opdrachtprompt
1. Kopieer het installatieprogramma naar een lokale map (bijvoorbeeld C:\Temp) op de server die u wilt beveiligen. Het installatieprogramma vindt u op de configuratieserver onder **[Install Location]\home\svsystems\pushinstallsvc\repository**. Het pakket voor Windows-besturingssystemen heeft een naam die lijkt op Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe
2. Wijzig de naam van dit bestand in MobilitySvcInstaller.exe
3. Voer de volgende opdracht uit om het MSI-installatieprogramma op te halen:

    ``C:\> cd C:\Tempww
    ``C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted``
    ``C:\Temp> cd Extracted``
    ``C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>``

##### <a name="full-command-line-syntax"></a>Volledige opdrachtregelsyntaxis
    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parameters**

* **/Role:** Verplicht. Hiermee geeft u op of de Mobility-service moet worden geïnstalleerd. Invoerwaardenagent|MasterTarget
* **/InstallLocation:** Verplicht. Geeft aan waar de service moet worden geïnstalleerd.
* **/PassphraseFilePath:** Verplicht. De wachtwoordzin voor de configuratieserver.
* **/LogFilePath:** Verplicht. De locatie waar de installatielogboekbestanden moeten worden gemaakt.

#### <a name="uninstall-the-mobility-service-manually"></a>De Mobility-service handmatig verwijderen
De mobility-service kan worden verwijderd met behulp van Programma's en onderdelen in het Configuratiescherm of met behulp van deze opdrachtregelinstructie: MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="install-the-mobility-service-on-a-linux-server"></a>De Mobility-service installeren op een Linux-server
1. Kopieer het juiste tar-archief, op basis van de bovenstaande tabel, naar de Linux-computer die u wilt repliceren.
2. Open een shellprogramma en pak het gezipte tar-archief uit op een lokaal pad door het volgende uit te voeren: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Maak een passphrase.txt-bestand in de lokale directory waarin u de inhoud van het tar-archief hebt uitgepakt. Om dit te doen, kopieert u de wachtwoordzin van C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase op de configuratieserver en slaat u deze op in passphrase.txt door *`echo <passphrase> >passphrase.txt`* uit te voeren in shell.
4. Installeer de Mobility-service door *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`* uit te voeren.
5. Geef het interne IP-adres van de configuratieserver op en zorg ervoor dat poort 443 is geselecteerd. Na de installatie van de service kan het circa 15 minuten duren voordat de status is bijgewerkt in de portal.

**U kunt ook installeren vanaf de volgende opdrachtregel**:

Kopieer de wachtwoordzin van C:\Program Files (x86) \InMage Systems\private\connection op de configuratieserver en sla deze op als 'passphrase.txt' op de configuratieserver. Voer daarna deze opdrachten uit. In ons voorbeeld is het IP-adres van de configuratieserver 104.40.75.37 en moet de HTTPS-poort 443 zijn:


Installeren op een productieserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Installeren op een hoofddoelserver:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt






<!--HONumber=Feb17_HO3-->


