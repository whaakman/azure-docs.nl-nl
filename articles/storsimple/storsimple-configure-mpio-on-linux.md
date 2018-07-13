---
title: MPIO op StorSimple Linux-host configureren | Microsoft Docs
description: MPIO op StorSimple die zijn verbonden met een Linux-host met CentOS 6.6 configureren
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: ccd24e1498282cd2b627226df79af22e9647b64d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38681569"
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>U kunt MPIO configureren op een StorSimple-host waarop CentOS wordt uitgevoerd
In dit artikel worden de stappen beschreven die vereist voor het configureren van multipath i/o-(MPIO) op uw Centos 6.6 host-server. De host-server is verbonden met uw Microsoft Azure StorSimple-apparaat voor hoge beschikbaarheid via iSCSI-initiators. Er wordt in detail beschreven de automatische detectie van multipath-apparaten en de specifieke instellingen alleen van StorSimple-volumes.

Deze procedure is van toepassing op alle modellen van apparaten uit de StorSimple 8000-serie.

> [!NOTE]
> Deze procedure kan niet worden gebruikt voor een StorSimple Cloud Appliance. Zie voor meer informatie over het configureren van hostservers voor uw cloudapparaat.


## <a name="about-multipathing"></a>Over meerdere paden
De functie multipath kunt u het configureren van meerdere i/o-paden tussen een host-server en een opslagapparaat. Deze i/o-paden zijn fysieke SAN-verbindingen die afzonderlijke kabels, switches, netwerkinterfaces en domeincontrollers kunnen opnemen. Meerdere paden combineert de i/o-paden, voor het configureren van een nieuw apparaat dat is gekoppeld aan alle van de samengevoegde paden.

Het doel van meerdere paden is tweeledig:

* **Hoge beschikbaarheid**: biedt een alternatief pad uitval van een element van het i/o-pad (bijvoorbeeld een kabel, switch, netwerk-interface of domeincontroller).
* **Taakverdeling**: afhankelijk van de configuratie van uw opslagapparaat, kan deze de prestaties verbeteren door het detecteren van de belasting van de i/o-paden en dynamisch herverdeling de belasting.

### <a name="about-multipathing-components"></a>Over Multipath-onderdelen
Meerdere paden in Linux bestaat uit onderdelen van de kernel en gebruikersruimte onderdelen zoals Zie tabel hieronder.

* **Kernel**: het belangrijkste onderdeel is het *apparaat-mapper* die omgeleid i/o en biedt ondersteuning voor failover voor paden en pad groepen.

* **Gebruikersruimte**: dit zijn *multipath-hulpprogramma's* die multipathed apparaten beheren door de apparaat-mapper-module voor multipath wat te doen. De hulpprogramma's bestaan uit:
   
   * **Multipath**: geeft een lijst van en multipathed apparaten configureert.
   * **Multipathd**: daemon waarvoor het uitvoeren van multipath en bewaakt de paden.
   * **Naam van de Devmap**: biedt een zinvolle apparaatnaam naar udev worden voor devmaps.
   * **Kpartx**: lineaire devmaps wordt toegewezen aan de apparaat-partities kunnen meerdere paden maps partitioneerbare.
   * **Multipath.conf**: configuratiebestand voor multipath-daemon die wordt gebruikt voor het overschrijven van de ingebouwde configuratietabel.

### <a name="about-the-multipathconf-configuration-file"></a>Over het configuratiebestand multipath.conf
Het configuratiebestand `/etc/multipath.conf` maakt veel van de functies van meerdere paden kunnen worden geconfigureerd met een gebruiker. De `multipath` opdracht en de kernel-daemon `multipathd` informatie vindt u in dit bestand gebruiken. Het bestand is geraadpleegd alleen tijdens de configuratie van de MPIO-apparaten. Zorg ervoor dat alle wijzigingen worden aangebracht voordat u de `multipath` opdracht. Als u het bestand later wijzigt, moet u stoppen en starten multipathd opnieuw om de wijzigingen van kracht te laten worden.

De multipath.conf heeft vijf secties:

- **Niveau standaardwaarden van het systeem** *(standaard ingesteld)*: U kunt op standaardwaarden van het systeem onderdrukken.
- **Op de blokkeringslijst apparaten** *(blacklist)*: kunt u de lijst met apparaten die niet moeten worden beheerd door apparaat toewijzen.
- **Uitzonderingen zwarte** *(blacklist_exceptions)*: U kunt specifieke apparaten moet worden behandeld als multipath-apparaten, zelfs als die worden vermeld in de zwarte lijst identificeren.
- **Specifieke Opslaginstellingen controller** *(apparaten)*: U kunt configuratie-instellingen die worden toegepast op apparaten met de leverancier en productinformatie opgeven.
- **Specifieke instellingen voor apparaten** *(multipaths)*: U kunt in deze sectie gebruiken voor het afstemmen van de configuratie-instellingen voor afzonderlijke LUN's.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Meerdere paden op StorSimple die zijn verbonden met Linux-host configureren
Een StorSimple-apparaat dat is verbonden met een Linux-host kan worden geconfigureerd voor hoge beschikbaarheid en taakverdeling. Bijvoorbeeld, als de Linux-host twee interfaces met de SAN verbonden heeft en het apparaat heeft twee interfaces zijn verbonden met de SAN zodanig dat deze interfaces op hetzelfde subnet bevinden worden, worden klikt u vervolgens er 4 paden beschikbaar zijn. Echter, als elke interface gegevens op het apparaat en de host-interface op een ander IP-subnet (en niet routeerbaar), klikt u vervolgens alleen 2 paden zijn beschikbaar. U kunt meerdere paden voor het automatisch detecteren van de beschikbare paden, kiest u een algoritme voor taakverdeling voor deze paden, toepassen van specifieke configuratie-instellingen voor alleen-StorSimple-volumes, en vervolgens inschakelen en controleer of Multipath configureren.

De volgende procedure wordt beschreven hoe u meerdere paden configureren wanneer een StorSimple-apparaat met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces.

## <a name="prerequisites"></a>Vereisten
In deze sectie worden de configuratievereisten voor CentOS-server en het StorSimple-apparaat.

### <a name="on-centos-host"></a>Op de host CentOS
1. Zorg ervoor dat uw CentOS-host 2 netwerkinterfaces zijn ingeschakeld. Type:
   
    `ifconfig`
   
    Het volgende voorbeeld ziet u de uitvoer wanneer twee netwerkinterfaces (`eth0` en `eth1`) aanwezig zijn op de host.
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. Installeer *iSCSI-initiator-utils* op uw server CentOS. Voer de volgende stappen uit voor het installeren van *iSCSI-initiator-utils*.
   
   1. Meld u aan als `root` in uw CentOS-host.
   2. Installeer de *iSCSI-initiator-utils*. Type:
      
       `yum install iscsi-initiator-utils`
   3. Na de *iSCSI-Initiator-utils* nog niet is geïnstalleerd, start de iSCSI-service. Type:
      
       `service iscsid start`
      
       In gevallen `iscsid` mogelijk niet daadwerkelijk wordt gestart en de `--force` optie kan ook nodig zijn
   4. Om ervoor te zorgen dat de iSCSI-initiator tijdens het opstarten is ingeschakeld, gebruikt u de `chkconfig` opdracht uit om de service.
      
       `chkconfig iscsi on`
   5. Als u wilt controleren of deze is correct ingesteld, moet u de opdracht uitvoeren:
      
       `chkconfig --list | grep iscsi`
      
       Hieronder ziet u een voorbeeld van de uitvoer.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       In het bovenstaande voorbeeld ziet u dat de iSCSI-omgeving worden uitgevoerd op opstarten op uitvoeringsniveaus van 2, 3, 4 en 5.
3. Installeer *apparaat-mapper-multipath*. Type:
   
    `yum install device-mapper-multipath`
   
    De installatie wordt gestart. Type **Y** om door te gaan wanneer u om bevestiging wordt gevraagd.

### <a name="on-storsimple-device"></a>StorSimple-apparaat
Uw StorSimple-apparaat moet hebben:

* Minimaal twee interfaces zijn ingeschakeld voor iSCSI. Als u wilt controleren of twee interfaces voor iSCSI-zijn ingeschakeld op uw StorSimple-apparaat, moet u de volgende stappen uitvoeren in de klassieke Azure portal voor uw StorSimple-apparaat:
  
  1. Meld u aan bij de klassieke portal voor uw StorSimple-apparaat.
  2. Selecteer uw StorSimple Manager-service, klikt u op **apparaten** en kies de specifieke StorSimple-apparaat. Klik op **configureren** en controleer of de netwerkinterface-instellingen. Hieronder ziet u een schermopname met twee netwerkinterfaces die op iSCSI-zijn ingeschakeld. Hier DATA 2 en DATA 3, beide 10 GbE interfaces voor iSCSI-zijn ingeschakeld.
     
      ![MPIO StorsSimple DATA 2-configuratie](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![MPIO StorSimple DATA 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      In de **configureren** pagina
     
     1. Zorg ervoor dat beide netwerkinterfaces ingeschakeld voor iSCSI zijn. De **iSCSI ingeschakeld** veld moet worden ingesteld op **Ja**.
     2. Zorg ervoor dat de netwerkinterfaces hebben dezelfde snelheid, zowel moet 1 GbE of 10 GbE.
     3. Houd er rekening mee de IPv4-adressen van de interfaces voor iSCSI-zijn ingeschakeld en opslaan voor later gebruik op de host.
* De iSCSI-interfaces op uw StorSimple-apparaat moet bereikbaar is vanaf de CentOS-server.
      Om dit te controleren, moet u IP-adressen van uw StorSimple-ingeschakeld voor iSCSI-netwerkinterfaces op uw host-server opgeven. De opdrachten die worden gebruikt en de bijbehorende uitvoer met DATA2 (10.126.162.25) en DATA3 (10.126.162.26) wordt hieronder weergegeven:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Hardwareconfiguratie
Het is raadzaam dat u verbinding maken met de twee iSCSI-netwerkinterfaces op afzonderlijke paden voor redundantie. De onderstaande afbeelding ziet u de aanbevolen hardwareconfiguratie voor hoge beschikbaarheid en taakverdeling Multipath voor uw CentOS-server en de StorSimple-apparaat.

![MPIO hardwareconfiguratie voor StorSimple op Linux-host](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Zoals wordt weergegeven in de voorgaande afbeelding:

* Uw StorSimple-apparaat is een actief-passief-configuratie met twee controllers.
* Twee SAN-switches zijn verbonden met uw apparaatcontrollers.
* Twee iSCSI-initiators zijn ingeschakeld op uw StorSimple-apparaat.
* Twee netwerkinterfaces zijn ingeschakeld op uw host CentOS.

De configuratie van de bovenstaande tot 4 afzonderlijke paden tussen uw apparaat en de host als de host en gegevens-interfaces routeerbaar zijn.

> [!IMPORTANT]
> * Het is raadzaam om niet 1 GbE en 10 GbE-netwerkinterfaces voor de opslaginfrastructuur te combineren. Wanneer u twee netwerkinterfaces, moeten zowel de interfaces welk identiek zijn.
> * Op uw StorSimple-apparaat, DATA0, bestand1, DATA4 en DATA5 zijn 1 GbE-netwerkinterfaces DATA2 en DATA3 10 GbE-netwerkinterfaces zijn. |
> 
> 

## <a name="configuration-steps"></a>Configuratiestappen
De configuratiestappen voor meerdere paden hebben betrekking op het configureren van de beschikbare paden voor automatische detectie, op te geven het taakverdelingsalgoritme moet worden gebruikt, Multipath inschakelen en ten slotte de configuratie te controleren. Elk van deze stappen wordt besproken in de volgende secties.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Stap 1: Configureer meerdere paden voor automatische detectie
De multipath-ondersteunde apparaten kunnen automatisch worden gedetecteerd en geconfigureerd.

1. Initialiseren `/etc/multipath.conf` bestand. Type:
   
     `mpathconf --enable`
   
    De bovenstaande opdracht maakt u een `sample/etc/multipath.conf` bestand.
2. Start multipath-service. Type:
   
    `service multipathd start`
   
    Hier ziet u de volgende uitvoer:
   
    `Starting multipathd daemon:`
3. Automatische detectie van multipaths inschakelen. Type:
   
    `mpathconf --find_multipaths y`
   
    Dit wordt, wijzigt u de sectie standaardinstellingen van uw `multipath.conf` zoals hieronder wordt weergegeven:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Stap 2: Configureer meerdere paden voor StorSimple-volumes
Standaard worden alle apparaten zwart zijn vermeld in het bestand multipath.conf en worden overgeslagen. U moet een zwarte lijst uitzonderingen om toe te staan van meerdere paden voor volumes van StorSimple-apparaten maken.

1. Bewerk de `/etc/mulitpath.conf` bestand. Type:
   
    `vi /etc/multipath.conf`
2. Zoek de sectie blacklist_exceptions in het bestand multipath.conf. Uw StorSimple-apparaat moet worden weergegeven als een blacklist uitzondering in deze sectie. U kunt Verwijder opmerkingen bij de desbetreffende regels in dit bestand te wijzigen zoals hieronder (Gebruik alleen het specifieke model van het apparaat dat u gebruikt):
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Stap 3: Meerdere paden van round robin configureren
Dit algoritme load balancing gebruikt alle beschikbare multipaths naar de actieve controller een met gelijke taakverdeling, round robin besturingsaanvraag.

1. Bewerk de `/etc/multipath.conf` bestand. Type:
   
    `vi /etc/multipath.conf`
2. Onder de `defaults` sectie, stelt u de `path_grouping_policy` naar `multibus`. De `path_grouping_policy` Hiermee geeft u het standaardpad beleid toe te passen op niet-opgegeven multipaths groeperen. De standaard-sectie eruit zoals hieronder wordt weergegeven.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> De meest voorkomende waarden van `path_grouping_policy` opnemen:
> 
> * failover = 1 pad per groep van de prioriteit
> * multibus = alle geldige paden in 1 groep
> 
> 

### <a name="step-4-enable-multipathing"></a>Stap 4: Schakel Multipath
1. Start opnieuw op de `multipathd` daemon. Type:
   
    `service multipathd restart`
2. De uitvoer zijn, zoals hieronder weergegeven:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>Stap 5: Controleer of de opslaginfrastructuur
1. Zorg eerst dat iSCSI-verbinding tot stand is gebracht met het StorSimple-apparaat als volgt:
   
   a. Ontdek uw StorSimple-apparaat. Type:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>
    ```
    
    De uitvoer als IP-adres voor DATA0 10.126.162.25 en poort 3260 wordt geopend op het StorSimple-apparaat voor uitgaande iSCSI-verkeer is, zoals hieronder weergegeven:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    Kopieer het IQN van uw StorSimple-apparaat, `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, uit de bovenstaande uitvoer.

   b. Verbinding maken met het apparaat met behulp van IQN van het doel. Het StorSimple-apparaat is het iSCSI-doel. Type:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    Het volgende voorbeeld ziet u uitvoer met een IQN van het doel van `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. De uitvoer geeft aan dat u verbinding op de twee netwerkinterfaces voor iSCSI-zijn ingeschakeld op uw apparaat gemaakt hebt.

    ```
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    Als u slechts één hostinterface en hier twee paden ziet, moet u zowel de interfaces op de host voor iSCSI-inschakelen. U kunt volgen de [gedetailleerde instructies in de documentatie voor virtuele Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).

2. Een volume wordt blootgesteld aan de CentOS-server van de StorSimple-apparaat. Zie voor meer informatie, [stap 6: een volume maken](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) via Azure portal op uw StorSimple-apparaat.

3. Controleer of de beschikbare paden. Type:

      ```
      multipath –l
      ```

      Het volgende voorbeeld ziet de uitvoer voor de twee netwerkinterfaces op een StorSimple-apparaat dat is verbonden met een afzonderlijke host-netwerkinterface met twee beschikbare paden.

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        The following example shows the output for two network interfaces on a StorSimple device connected to two host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After the paths are configured, refer to the specific instructions on your host operating system (Centos 6.6) to mount and format this volume.

## <a name="troubleshoot-multipathing"></a>Meerdere paden oplossen
Deze sectie bevat enkele nuttige tips als u problemen tijdens het configureren van meerdere paden ondervindt.

Q. Ik zie niet de wijzigingen in `multipath.conf` bestand die van kracht.

A. Als u wijzigingen hebt aangebracht de `multipath.conf` -bestand, moet u de Multipath-service opnieuw starten. Typ de volgende opdracht:

    service multipathd restart

Q. Kan ik hebt twee netwerkinterfaces op het StorSimple-apparaat en twee netwerkinterfaces op de host ingeschakeld. Wanneer ik de beschikbare paden, zie ik slechts twee paden. Ik verwacht te zien van vier beschikbare paden.

A. Zorg ervoor dat de twee paden op hetzelfde subnet en routeerbaar. Als de netwerkinterfaces op verschillende VLAN's en niet-routeerbare zijn, ziet u slechts twee paden. Er is één manier om dit te controleren om ervoor te zorgen dat u zowel de hostinterfaces van een netwerkinterface op het StorSimple-apparaat kunt bereiken. U moet [Neem contact op met Microsoft Support](storsimple-8000-contact-microsoft-support.md) omdat deze verificatie kan alleen worden uitgevoerd via een ondersteuningssessie voor.

Q. Wanneer ik beschikbare paden, zie ik geen uitvoer.

A. Normaal gesproken een mogelijk probleem met de daemon Multipath ziet niet alle paden multipathed aangeeft, en het is zeer waarschijnlijk dat er probleem hier ligt in de `multipath.conf` bestand.

Het is ook waard controleren dat u daadwerkelijk sommige schijven zien kan nadat u verbinding met het doel als er geen reactie van de multipath aanbiedingen kan ook betekenen dat u hebt geen eventuele schijven.

* Gebruik de volgende opdracht om te scannen van de SCSI-bus:
  
    `$ rescan-scsi-bus.sh `(onderdeel van sg3_utils pakket)
* Typ de volgende opdrachten:
  
    `$ dmesg | grep sd*`
     
     of
  
    `$ fdisk –l`
  
    Deze wordt details van onlangs toegevoegde schijven geretourneerd.
* Om te bepalen of het een StorSimple-schijf is, gebruikt u de volgende opdrachten:
  
    `cat /sys/block/<DISK>/device/model`
  
    Een tekenreeks, die bepaalt of het een StorSimple-schijf is er wordt geretourneerd.

Een minder waarschijnlijk, maar mogelijk de oorzaak kan ook worden de verouderde iscsid pid. Gebruik de volgende opdracht af te melden bij de iSCSI-sessies:

    iscsiadm -m node --logout -p <Target_IP>

Herhaal deze opdracht voor de verbonden netwerkinterfaces op de iSCSI-doel is van uw StorSimple-apparaat. Nadat u afgemeld bij alle iSCSI-sessies bent, gebruikt u de IQN van het iSCSI-doel te herstellen van de iSCSI-sessie. Typ de volgende opdracht:

    iscsiadm -m node --login -T <TARGET_IQN>


Q. Ik ben niet zeker weet of het apparaat opgenomen in de whitelist is.

A. Als u wilt controleren of uw apparaat in de whitelist opgenomen, gebruik de volgende opdracht voor het oplossen van problemen interactieve:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Ga voor meer informatie naar [gebruiken het oplossen van interactieve opdracht voor meerdere paden](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).

## <a name="list-of-useful-commands"></a>Lijst met nuttige opdrachten
| Type | Opdracht | Beschrijving |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |ISCSI-service starten |
| &nbsp; |`service iscsid stop` |ISCSI-service stoppen |
| &nbsp; |`service iscsid restart` |ISCSI-service opnieuw starten |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |Beschikbare doelen op het opgegeven adres detecteren |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |Meld u aan bij de iSCSI-doel |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |Meld u af bij de iSCSI-doel |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |Naam van de iSCSI-initiator afdrukken |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |Controleer de status van de iSCSI-sessie en het volume dat is gedetecteerd op de host |
| &nbsp; |`iscsi –m session` |Geeft alle iSCSI-sessies tot stand gebracht tussen de host en het StorSimple-apparaat |
|  | | |
| **Meerdere paden** |`service multipathd start` |Multipath-daemon starten |
| &nbsp; |`service multipathd stop` |Multipath-daemon stoppen |
| &nbsp; |`service multipathd restart` |Multipath-daemon opnieuw starten |
| &nbsp; |`chkconfig multipathd on` </br> OF </br> `mpathconf –with_chkconfig y` |Multipath-daemon om te beginnen bij het opstarten inschakelen |
| &nbsp; |`multipathd –k` |Start de interactieve console voor het oplossen van problemen |
| &nbsp; |`multipath –l` |Lijst met meerdere paden verbindingen en -apparaten |
| &nbsp; |`mpathconf --enable` |Maken van een voorbeeldbestand mulitpath.conf in `/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Volgende stappen
Voordat u MPIO op Linux-host configureert, moet u ook om te verwijzen naar de volgende CentoS 6.6 documenten:

* [MPIO op CentOS instellen](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [Handleiding voor Linux-Training](http://linux-training.be/linuxsys.pdf)

